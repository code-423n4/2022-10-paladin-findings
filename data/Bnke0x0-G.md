

### [G01] State variables only set in the constructor should be declared `immutable`

#### Impact
Avoids a Gsset (20000 gas)
#### Findings:
```
2022-10-paladin/contracts/WardenPledge.sol::47 => Pledge[] public pledges;
2022-10-paladin/contracts/WardenPledge.sol::60 => IVotingEscrow public votingEscrow;
2022-10-paladin/contracts/WardenPledge.sol::62 => IBoostV2 public delegationBoost;
2022-10-paladin/contracts/WardenPledge.sol::73 => address public chestAddress;
2022-10-paladin/contracts/WardenPledge.sol::76 => uint256 public minTargetVotes;
```



### [G02] State variables can be packed into fewer storage slots

#### Impact
If variables occupying the same slot are both written the same 
function or by the constructor avoids a separate Gsset (20000 gas). 
Reads of the variables are also cheaper
#### Findings:
```
2022-10-paladin/contracts/WardenPledge.sol::73 => address public chestAddress;
```



### [G03] `<array>.length` should not be looked up in every loop of a `for` loop

#### Impact
Even memory arrays incur the overhead of bit tests and bit shifts to 
calculate the array length. Storage array length checks incur an extra 
Gwarmaccess (100 gas) PER-LOOP.
#### Findings:
```
2022-10-paladin/contracts/WardenPledge.sol::547 => for(uint256 i = 0; i < length;){
```


### [G04] Using `> 0` costs more gas than `!= 0` when used on a `uint` in a `require()` statement


#### Findings:
```
2022-10-paladin/contracts/WardenPledge.sol::471 => if(remainingAmount > 0) {
2022-10-paladin/contracts/WardenPledge.sol::504 => if(remainingAmount > 0) {
```



### [G05] It costs more gas to initialize variables to zero than to let the default of zero be applied


#### Findings:
```
2022-10-paladin/contracts/WardenPledge.sol::473 => pledgeAvailableRewardAmounts[pledgeId] = 0;
2022-10-paladin/contracts/WardenPledge.sol::506 => pledgeAvailableRewardAmounts[pledgeId] = 0;
2022-10-paladin/contracts/WardenPledge.sol::547 => for(uint256 i = 0; i < length;){
2022-10-paladin/contracts/WardenPledge.sol::589 => minAmountRewardToken[token] = 0;
```


### [G06] Functions guaranteed to revert when called by normal users can be marked `payable`

#### Impact
If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided.
#### Findings:
```
2022-10-paladin/contracts/WardenPledge.sol::541 => function addMultipleRewardToken(address[] calldata tokens, uint256[] calldata minRewardsPerSecond) external onlyOwner {
2022-10-paladin/contracts/WardenPledge.sol::560 => function addRewardToken(address token, uint256 minRewardPerSecond) external onlyOwner {
2022-10-paladin/contracts/WardenPledge.sol::570 => function updateRewardToken(address token, uint256 minRewardPerSecond) external onlyOwner {
2022-10-paladin/contracts/WardenPledge.sol::585 => function removeRewardToken(address token) external onlyOwner {
2022-10-paladin/contracts/WardenPledge.sol::599 => function updateChest(address chest) external onlyOwner {
2022-10-paladin/contracts/WardenPledge.sol::612 => function updateMinTargetVotes(uint256 newMinTargetVotes) external onlyOwner {
2022-10-paladin/contracts/WardenPledge.sol::625 => function updatePlatformFee(uint256 newFee) external onlyOwner {
2022-10-paladin/contracts/WardenPledge.sol::636 => function pause() external onlyOwner {
2022-10-paladin/contracts/WardenPledge.sol::643 => function unpause() external onlyOwner {
2022-10-paladin/contracts/WardenPledge.sol::653 => function recoverERC20(address token) external onlyOwner returns(bool) {
```



### [G07] Use a more recent version of solidity

#### Impact
Use a solidity version of at least 0.8.13 to have external calls skip
 contract existence checks if the external call has a return value
#### Findings:
```
2022-10-paladin/contracts/WardenPledge.sol::2 => pragma solidity 0.8.10;
```



### [G08] Bitshift for divide by 2

#### Impact
When multiplying or dividing by a power of two, it is cheaper to bitshift than to use standard math operations.
#### Findings:
```
2022-10-paladin/contracts/WardenPledge.sol::263 => uint256 totalDelegatedAmount = ((bias * boostDuration) + bias) / 2;
```



###  [G09] Use simple comparison in trinary logic

#### Impact
The comparison operators `>=` and `<=` use more gas than `>`, 
`<`, or `==`. Replacing the  `>=` and `≤` operators with a comparison 
operator that has an opcode in the EVM saves gas.
#### Findings:
```
2022-10-paladin/contracts/WardenPledge.sol::223 => if(pledgeId >= pledgesIndex()) revert Errors.InvalidPledgeID();
2022-10-paladin/contracts/WardenPledge.sol::229 => if(pledgeParams.endTimestamp <= block.timestamp) revert Errors.ExpiredPledge();
2022-10-paladin/contracts/WardenPledge.sol::374 => if(pledgeId >= pledgesIndex()) revert Errors.InvalidPledgeID();
2022-10-paladin/contracts/WardenPledge.sol::380 => if(pledgeParams.endTimestamp <= block.timestamp) revert Errors.ExpiredPledge();
2022-10-paladin/contracts/WardenPledge.sol::420 => if(pledgeId >= pledgesIndex()) revert Errors.InvalidPledgeID();
2022-10-paladin/contracts/WardenPledge.sol::426 => if(pledgeParams.endTimestamp <= block.timestamp) revert Errors.ExpiredPledge();
2022-10-paladin/contracts/WardenPledge.sol::429 => if(newRewardPerVote <= oldRewardPerVote) revert Errors.RewardsPerVotesTooLow();
2022-10-paladin/contracts/WardenPledge.sol::457 => if(pledgeId >= pledgesIndex()) revert Errors.InvalidPledgeID();
2022-10-paladin/contracts/WardenPledge.sol::489 => if(pledgeId >= pledgesIndex()) revert Errors.InvalidPledgeID();
2022-10-paladin/contracts/WardenPledge.sol::496 => if(pledgeParams.endTimestamp <= block.timestamp) revert Errors.ExpiredPledge();
```
### Recommended Mitigation Steps

Replace the comparison operator and reverse the logic to save gas using the suggestions above.





### [G10] Amounts should be checked for 0 before calling a transfer

#### Impact
Checking non-zero transfer values can avoid an expensive external call and save gas.

While this is done in some places, it’s not consistently done in the solution.
#### Findings:
```
2022-10-paladin/contracts/WardenPledge.sol::271 => IERC20(pledgeParams.rewardToken).safeTransfer(user, rewardAmount);
2022-10-paladin/contracts/WardenPledge.sol::475 => IERC20(pledgeParams.rewardToken).safeTransfer(receiver, remainingAmount);
2022-10-paladin/contracts/WardenPledge.sol::508 => IERC20(pledgeParams.rewardToken).safeTransfer(receiver, remainingAmount);
2022-10-paladin/contracts/WardenPledge.sol::658 => IERC20(token).safeTransfer(owner(), amount);
```



### [G11] Using `private` rather than `public` for constants, saves gas

#### Impact
If needed, the value can be read from the verified contract source 
code. Savings are due to the compiler not having to create non-payable getter functions for deployment call data, and not adding another entry to the method ID table
#### Findings:
```
2022-10-paladin/contracts/WardenPledge.sol::22 => uint256 public constant UNIT = 1e18;
2022-10-paladin/contracts/WardenPledge.sol::23 => uint256 public constant MAX_PCT = 10000;
2022-10-paladin/contracts/WardenPledge.sol::24 => uint256 public constant WEEK = 7 * 86400;
```


### [G12] Remove or replace unused state variables

#### Impact
Saves a storage slot. If the variable is assigned a non-zero value, 
saves Gsset (20000 gas). If it’s assigned a zero value, saves Gsreset 
(2900 gas). If the variable remains unassigned, there is no gas savings 
unless the variable is public, in which case the 
compiler-generated non-payable getter deployment cost is saved. If the 
state variable is overriding an interface’s public function, mark the 
variable as constant or immutable so that it does not use a storage slot
#### Findings:
```
2022-10-paladin/contracts/WardenPledge.sol::52 => mapping(address => uint256[]) public ownerPledges;
```



### [G13] Multiplication/division by two should use bit shifting

#### Impact
<x> * 2 is equivalent to <x> << 1 and <x> / 2 is the same as <x> >> 1. The MUL and DIV opcodes cost 5 gas, whereas SHL and SHR only cost 3 gas
#### Findings:
```
2022-10-paladin/contracts/WardenPledge.sol::263 => uint256 totalDelegatedAmount = ((bias * boostDuration) + bias) / 2;
```


