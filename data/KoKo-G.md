# GAS ISSUES FOR [2022-10-PALADIN](https://github.com/code-423n4/2022-10-paladin)

## [G-02] Make `constant` and `immutable` variables `private` instead of `public`

Description: Removes the getter function for this variable which saves gas. At the same time the variable get still be read from outside the contract

./contracts/WardenPledge.sol

```
L22:   uint256 public constant UNIT = 1e18;

L23:   uint256 public constant MAX_PCT = 10000;

L24:   uint256 public constant WEEK = 7 * 86400;
```

## [G-03] If a function is not open to any user, it can be marked as `payable` to save gas

Description: Under the hood the compiler checks if a function is payable. The check is skipped if it is marked as such by the developer

./contracts/WardenPledge.sol

```
L560:  function addRewardToken(address token, uint256 minRewardPerSecond) external onlyOwner {

L570:  function updateRewardToken(address token, uint256 minRewardPerSecond) external onlyOwner {

L585:  function removeRewardToken(address token) external onlyOwner {

L599:  function updateChest(address chest) external onlyOwner {

L612:  function updateMinTargetVotes(uint256 newMinTargetVotes) external onlyOwner {

L625:  function updatePlatformFee(uint256 newFee) external onlyOwner {

L636:  function pause() external onlyOwner {
```

## [G-04] `!= 0` comparison is cheaper than `> 0`

./contracts/WardenPledge.sol

```
L471:  if(remainingAmount > 0) {

L504:  if(remainingAmount > 0) {
```

## [G-05] A=A+B costs less gas than A+=B if A and B are located in storage

./contracts/WardenPledge.sol

```
L268:  pledgeAvailableRewardAmounts[pledgeId] -= rewardAmount;

L340:  pledgeAvailableRewardAmounts[vars.newPledgeID] += vars.totalRewardAmount;

L401:  pledgeAvailableRewardAmounts[pledgeId] += totalRewardAmount;
```

## [G-06] Mark contract constants with the `constant` and `immutable` keywords to save gas

./contracts/WardenPledge.sol

```
L60:   IVotingEscrow public votingEscrow;

L62:   IBoostV2 public delegationBoost;
```

## [G-07] Cache variables that are read multiple times in a single fuction or loop

./contracts/WardenPledge.sol

```
L240:  delegationBoost.checkpoint_user(user);
L241:  if(delegationBoost.allowance(user, address(this)) < amount) revert Errors.InsufficientAllowance();
L242:  if(delegationBoost.delegable_balance(user) < amount) revert Errors.CannotDelegate();
L245:  if(delegationBoost.adjusted_balance_of(pledgeParams.receiver) + amount > pledgeParams.targetVotes) revert Errors.TargetVotesOverflow();
L248:  delegationBoost.boost(
```