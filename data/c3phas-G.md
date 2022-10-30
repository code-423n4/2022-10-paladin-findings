## FINDINGS
NB: Some functions have been truncated where neccessary to just show affected parts of the code.

Throught the report some places might be denoted with audit tags to show the actual place affected.

### Using immutable on variables that are only set in the constructor and never after 
Use immutable if you want to assign a permanent value at construction. Use constants if you already know the permanent value. Both get directly embedded in bytecode, saving SLOAD.
Variables only set in the constructor and never edited afterwards should be marked as immutable, as it would avoid the expensive storage-writing operation in the constructor (around 20 000 gas per variable) and replace the expensive storage-reading operations (around 2100 gas per reading) to a less expensive value reading (3 gas)

https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L60
```solidity
File: /contracts/WardenPledge.sol
60:    IVotingEscrow public votingEscrow;
```
https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L62
```solidity
File: /contracts/WardenPledge.sol
62:    IBoostV2 public delegationBoost;
```

### Use constants for variables whose value is known beforehand and is never changed

https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L79
```solidity
File: /contracts/WardenPledge.sol
79:    uint256 public minDelegationTime = 1 weeks;
```

```diff
diff --git a/contracts/WardenPledge.sol b/contracts/WardenPledge.sol
index beb990d..642a848 100644
--- a/contracts/WardenPledge.sol
+++ b/contracts/WardenPledge.sol
@@ -76,7 +76,7 @@ contract WardenPledge is Ownable, Pausable, ReentrancyGuard {
     uint256 public minTargetVotes;

     /** @notice Minimum delegation time, taken from veBoost contract */
-    uint256 public minDelegationTime = 1 weeks;
+    uint256 public constant minDelegationTime = 1 weeks;

```

### Cache storage values in memory to minimize SLOADs
The code can be optimizhttps://github.com/code-423n4/2022-08-fiatdao-findings/blob/main/data/GalloDaSballo-G.mded by minimizing the number of SLOADs.

SLOADs are expensive (100 gas after the 1st one) compared to MLOADs/MSTOREs (3 gas each). Storage values read multiple times should instead be cached in memory the first time (costing 1 SLOAD) and then read from this cache to avoid multiple SLOADs.

NB: *Some functions have been truncated where neccessary to just show affected parts of the code

https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L222-L274
### WardenPledge.sol.\_pledge(): delegationBoost should be cached(Saves  4 SLOADs ~394 gas)
```solidity
File: /contracts/WardenPledge.sol
222:    function _pledge(uint256 pledgeId, address user, uint256 amount, uint256 endTimestamp) internal {

240:        delegationBoost.checkpoint_user(user); //@audit: 1st SLOAD
241:        if(delegationBoost.allowance(user, address(this)) < amount) revert Errors.InsufficientAllowance(); //@audit: 2nd SLOAD
242:        if(delegationBoost.delegable_balance(user) < amount) revert Errors.CannotDelegate(); //@audit: 3rd SLOAD

245:        if(delegationBoost.adjusted_balance_of(pledgeParams.receiver) + amount > pledgeParams.targetVotes) revert Errors.TargetVotesOverflow(); //@audit: 4th SLOAD

248:        delegationBoost.boost( //@audit: 5th SLOAD
            pledgeParams.receiver,
            amount,
            endTimestamp,
            user
        );

```

```diff
diff --git a/contracts/WardenPledge.sol b/contracts/WardenPledge.sol
index beb990d..5b3d1bd 100644
--- a/contracts/WardenPledge.sol
+++ b/contracts/WardenPledge.sol
@@ -237,15 +237,16 @@ contract WardenPledge is Ownable, Pausable, ReentrancyGuard {
         uint256 boostDuration = endTimestamp - block.timestamp;

         // Check that the user has enough boost delegation available & set the correct allowance to this contract
-        delegationBoost.checkpoint_user(user);
-        if(delegationBoost.allowance(user, address(this)) < amount) revert Errors.InsufficientAllowance();
-        if(delegationBoost.delegable_balance(user) < amount) revert Errors.CannotDelegate();
+         IBoostV2 _delegationBoost = delegationBoost;
+        _delegationBoost.checkpoint_user(user);
+        if(_delegationBoost.allowance(user, address(this)) < amount) revert Errors.InsufficientAllowance();
+        if(_delegationBoost.delegable_balance(user) < amount) revert Errors.CannotDelegate();

         // Check that this will not go over the Pledge target of votes
-        if(delegationBoost.adjusted_balance_of(pledgeParams.receiver) + amount > pledgeParams.targetVotes) revert Errors.TargetVotesOverflow();
+        if(_delegationBoost.adjusted_balance_of(pledgeParams.receiver) + amount > pledgeParams.targetVotes) revert Errors.TargetVotesOverflow();

         // Creates the DelegationBoost
-        delegationBoost.boost(
+        _delegationBoost.boost(
             pledgeParams.receiver,
             amount,
             endTimestamp,
```

https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L222-L274
### WardenPledge.sol.\_pledge(): pledgeAvailableRewardAmounts[pledgeId] should be cached(saves 1 SLOAD ~97 gas)

```solidity
File: /contracts/WardenPledge.sol
    function _pledge(uint256 pledgeId, address user, uint256 amount, uint256 endTimestamp) internal {


267:        if(rewardAmount > pledgeAvailableRewardAmounts[pledgeId]) revert Errors.RewardsBalanceTooLow(); //@audit: 1st access
268:        pledgeAvailableRewardAmounts[pledgeId] -= rewardAmount; //@audit: 2nd access
```

```diff
diff --git a/contracts/WardenPledge.sol b/contracts/WardenPledge.sol
index beb990d..2bb2cd2 100644
--- a/contracts/WardenPledge.sol
+++ b/contracts/WardenPledge.sol
@@ -263,9 +263,10 @@ contract WardenPledge is Ownable, Pausable, ReentrancyGuard {
         uint256 totalDelegatedAmount = ((bias * boostDuration) + bias) / 2;
         // Then we can calculate the total amount of rewards for this Boost
         uint256 rewardAmount = (totalDelegatedAmount * pledgeParams.rewardPerVote) / UNIT;
+        uint _pledgeAvailableRewardAmounts = pledgeAvailableRewardAmounts[pledgeId];

-        if(rewardAmount > pledgeAvailableRewardAmounts[pledgeId]) revert Errors.RewardsBalanceTooLow();
-        pledgeAvailableRewardAmounts[pledgeId] -= rewardAmount;
+        if(rewardAmount > _pledgeAvailableRewardAmounts) revert Errors.RewardsBalanceTooLow();
+        pledgeAvailableRewardAmounts[pledgeId] = _pledgeAvailableRewardAmounts - rewardAmount;

         // Send the rewards to the user
         IERC20(pledgeParams.rewardToken).safeTransfer(user, rewardAmount);
```


https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L299-L358
### WardenPledge.sol.createPledge(): minAmountRewardToken[rewardToken] should be cached(saves 1 SLOAD ~97 gas) - happy path
```solidity
File: /contracts/WardenPledge.sol
299:    function createPledge(

312:        if(minAmountRewardToken[rewardToken] == 0) revert Errors.TokenNotWhitelisted(); //@audit: 1st access
313:        if(rewardPerVote < minAmountRewardToken[rewardToken]) revert Errors.RewardPerVoteTooLow(); //@audit: 2nd access
```

```diff
diff --git a/contracts/WardenPledge.sol b/contracts/WardenPledge.sol
index beb990d..247e5f8 100644
--- a/contracts/WardenPledge.sol
+++ b/contracts/WardenPledge.sol
@@ -309,8 +309,9 @@ contract WardenPledge is Ownable, Pausable, ReentrancyGuard {

         if(receiver == address(0) || rewardToken == address(0)) revert Errors.ZeroAddress();
         if(targetVotes < minTargetVotes) revert Errors.TargetVoteUnderMin();
-        if(minAmountRewardToken[rewardToken] == 0) revert Errors.TokenNotWhitelisted();
-        if(rewardPerVote < minAmountRewardToken[rewardToken]) revert Errors.RewardPerVoteTooLow();
+        uint256 _minAmountRewardToken = minAmountRewardToken[rewardToken]
+        if(_minAmountRewardToken == 0) revert Errors.TokenNotWhitelisted();
+        if(rewardPerVote < _minAmountRewardToken) revert Errors.RewardPerVoteTooLow();

         if(endTimestamp == 0) revert Errors.NullEndTimestamp();
         if(endTimestamp != _getRoundedTimestamp(endTimestamp)) revert Errors.InvalidEndTimestamp();
```

###  require() or revert() statements that check input arguments should be at the top of the function

Checks that involve constants should come before checks that involve state variables, function calls, and calculations. By doing these checks first, the function is able to revert before wasting a Gcoldsload (2100 gas) in a function that may ultimately revert in the unhappy case.

https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L222-L274
```solidity
File: /contracts/WardenPledge.sol
222:    function _pledge(uint256 pledgeId, address user, uint256 amount, uint256 endTimestamp) internal {
223:        if(pledgeId >= pledgesIndex()) revert Errors.InvalidPledgeID();
224:        if(amount == 0) revert Errors.NullValue();
```

```diff
diff --git a/contracts/WardenPledge.sol b/contracts/WardenPledge.sol
index beb990d..dfd3ff4 100644
--- a/contracts/WardenPledge.sol
+++ b/contracts/WardenPledge.sol
@@ -220,8 +220,9 @@ contract WardenPledge is Ownable, Pausable, ReentrancyGuard {
     * @param endTimestamp End of delegation
     */
     function _pledge(uint256 pledgeId, address user, uint256 amount, uint256 endTimestamp) internal {
-        if(pledgeId >= pledgesIndex()) revert Errors.InvalidPledgeID();
         if(amount == 0) revert Errors.NullValue();
+        if(pledgeId >= pledgesIndex()) revert Errors.InvalidPledgeID();

         // Load Pledge parameters & check the Pledge is still active
         Pledge memory pledgeParams = pledges[pledgeId];
```

https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L456-L480
```solidity
File: /contracts/WardenPledge.sol
456:    function retrievePledgeRewards(uint256 pledgeId, address receiver) external whenNotPaused nonReentrant {
457:        if(pledgeId >= pledgesIndex()) revert Errors.InvalidPledgeID();
458:        address creator = pledgeOwner[pledgeId];
459:        if(msg.sender != creator) revert Errors.NotPledgeCreator();
460:        if(receiver == address(0)) revert Errors.ZeroAddress();
```

```diff
diff --git a/contracts/WardenPledge.sol b/contracts/WardenPledge.sol
index beb990d..9c82ad9 100644
--- a/contracts/WardenPledge.sol
+++ b/contracts/WardenPledge.sol
@@ -454,10 +454,11 @@ contract WardenPledge is Ownable, Pausable, ReentrancyGuard {
     * @param receiver Address to receive the remaining rewards
     */
     function retrievePledgeRewards(uint256 pledgeId, address receiver) external whenNotPaused nonReentrant {
+        if(receiver == address(0)) revert Errors.ZeroAddress();
         if(pledgeId >= pledgesIndex()) revert Errors.InvalidPledgeID();
         address creator = pledgeOwner[pledgeId];
         if(msg.sender != creator) revert Errors.NotPledgeCreator();
-        if(receiver == address(0)) revert Errors.ZeroAddress();
+

         Pledge storage pledgeParams = pledges[pledgeId];
         if(pledgeParams.endTimestamp > block.timestamp) revert Errors.PledgeNotExpired();
```
https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L488-L515
```solidity
File: /contracts/WardenPledge.sol
488:    function closePledge(uint256 pledgeId, address receiver) external whenNotPaused nonReentrant {
489:        if(pledgeId >= pledgesIndex()) revert Errors.InvalidPledgeID();
490:        address creator = pledgeOwner[pledgeId];
491:        if(msg.sender != creator) revert Errors.NotPledgeCreator();
492:        if(receiver == address(0)) revert Errors.ZeroAddress();
```

```diff
diff --git a/contracts/WardenPledge.sol b/contracts/WardenPledge.sol
index beb990d..c06f2ee 100644
--- a/contracts/WardenPledge.sol
+++ b/contracts/WardenPledge.sol
@@ -486,10 +486,11 @@ contract WardenPledge is Ownable, Pausable, ReentrancyGuard {
     * @param receiver Address to receive the remaining rewards
     */
     function closePledge(uint256 pledgeId, address receiver) external whenNotPaused nonReentrant {
+        if(receiver == address(0)) revert Errors.ZeroAddress();
         if(pledgeId >= pledgesIndex()) revert Errors.InvalidPledgeID();
         address creator = pledgeOwner[pledgeId];
         if(msg.sender != creator) revert Errors.NotPledgeCreator();
-        if(receiver == address(0)) revert Errors.ZeroAddress();
```
https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L525-L533
```solidity
File: /contracts/WardenPledge.sol
525:    function _addRewardToken(address token, uint256 minRewardPerSecond) internal {
526:        if(minAmountRewardToken[token] != 0) revert Errors.AlreadyAllowedToken();
527:        if(token == address(0)) revert Errors.ZeroAddress();
528:        if(minRewardPerSecond == 0) revert Errors.NullValue();
```

```diff
diff --git a/contracts/WardenPledge.sol b/contracts/WardenPledge.sol
index beb990d..71d0087 100644
--- a/contracts/WardenPledge.sol
+++ b/contracts/WardenPledge.sol
@@ -523,10 +523,10 @@ contract WardenPledge is Ownable, Pausable, ReentrancyGuard {
     * @param minRewardPerSecond Minmum amount of reward per vote per second for the token
     */
     function _addRewardToken(address token, uint256 minRewardPerSecond) internal {
-        if(minAmountRewardToken[token] != 0) revert Errors.AlreadyAllowedToken();
         if(token == address(0)) revert Errors.ZeroAddress();
         if(minRewardPerSecond == 0) revert Errors.NullValue();

+        if(minAmountRewardToken[token] != 0) revert Errors.AlreadyAllowedToken();
+
         minAmountRewardToken[token] = minRewardPerSecond;

         emit NewRewardToken(token, minRewardPerSecond);
```

### Using storage instead of memory for structs/arrays saves gas

When fetching data from a storage location, assigning the data to a memory variable causes all fields of the struct/array to be read from storage, which incurs a Gcoldsload (2100 gas) for each field of the struct/array. If the fields are read from the new memory variable, they incur an additional MLOAD rather than a cheap stack read. Instead of declearing the variable with the memory keyword, declaring the variable with the storage keyword and caching any fields that need to be re-read in stack variables, will be much cheaper, only incuring the Gcoldsload for the fields actually read. The only time it makes sense to read the whole struct/array into a memory variable, is if the full struct/array is being returned by the function, is being passed to a function that requires memory, or if the array/struct is being read from another memory array/struct

https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L227
```solidity
File: /contracts/WardenPledge.sol
227:        Pledge memory pledgeParams = pledges[pledgeId];
```

### Using unchecked blocks to save gas
Solidity version 0.8+ comes with implicit overflow and underflow checks on unsigned integers. When an overflow or an underflow isn’t possible (as an example, when a comparison is made before the arithmetic operation), some gas can be saved by using an unchecked block
[see resource](https://github.com/ethereum/solidity/issues/10695)

https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L268
```solidity
File: /contracts/WardenPledge.sol
268:        pledgeAvailableRewardAmounts[pledgeId] -= rewardAmount;
```

The operation `pledgeAvailableRewardAmounts[pledgeId] -= rewardAmount;` cannot underflow due to the check on [Line 267](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L267) that ensures that `pledgeAvailableRewardAmounts[pledgeId]` is greater than `rewardAmount` before perfoming the arithmetic operation
