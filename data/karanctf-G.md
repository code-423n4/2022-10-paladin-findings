## [G-1] Donot use default values Explicit initialization with zero is not required for variable declaration of numTokens because `uints are 0` by default.removeing this will reduce contract size and save a bit of gas.

```solidity
WardenPledge.sol:547:        for(uint256 i = 0; i < length;){
```

## [G-2] Cache `.length` in loops
Reading array length at each iteration of the loop takes 6 gas (3 for mload and 3 to place memory_offset) in the stack.
Caching the array length in the stack saves around 3 gas per iteration.

```solidity
WardenPledge.sol:154:        return pledges.length;
WardenPledge.sol:542:        uint256 length = tokens.length;
WardenPledge.sol:545:        if(length != minRewardsPerSecond.length) revert Errors.InequalArraySizes();
```
## [G-03] Use `!= 0` instead of `> 0`
```solidity
WardenPledge.sol:471:        if(remainingAmount > 0) {
WardenPledge.sol:504:        if(remainingAmount > 0) {
```

## [G-4] `<x> += <y>` costs more gas than `<x> = <x> + <y>` 
```solidity
WardenPledge.sol:268:        pledgeAvailableRewardAmounts[pledgeId] -= rewardAmount;
WardenPledge.sol:340:        pledgeAvailableRewardAmounts[vars.newPledgeID] += vars.totalRewardAmount;
WardenPledge.sol:401:        pledgeAvailableRewardAmounts[pledgeId] += totalRewardAmount;
WardenPledge.sol:445:        pledgeAvailableRewardAmounts[pledgeId] += totalRewardAmount;
```

## [G-5] Use calldata instead of memory when used only for reading

```solidity
WardenPledge.sol:227:        Pledge memory pledgeParams = pledges[pledgeId];
WardenPledge.sol:318:        CreatePledgeVars memory vars;
```

## [G-6] `public` functions not called by the contract should be declared `external` instead   
```solidity
WardenPledge.sol:153:    function pledgesIndex() public view returns(uint256){
```


## [G-7] Use `variable == false|0` -> `!variable` or `variable ==  true` -> `variable`
```solidity
WardenPledge.sol:224:        if(amount == 0) revert Errors.NullValue();
WardenPledge.sol:233:        if(endTimestamp == 0) endTimestamp = pledgeParams.endTimestamp;
WardenPledge.sol:312:        if(minAmountRewardToken[rewardToken] == 0) revert Errors.TokenNotWhitelisted();
WardenPledge.sol:315:        if(endTimestamp == 0) revert Errors.NullEndTimestamp();
WardenPledge.sol:381:        if(newEndTimestamp == 0) revert Errors.NullEndTimestamp();
WardenPledge.sol:528:        if(minRewardPerSecond == 0) revert Errors.NullValue();
WardenPledge.sol:544:        if(length == 0) revert Errors.EmptyArray();
WardenPledge.sol:572:        if(minAmountRewardToken[token] == 0) revert Errors.NotAllowedToken();
WardenPledge.sol:573:        if(minRewardPerSecond == 0) revert Errors.InvalidValue();
WardenPledge.sol:587:        if(minAmountRewardToken[token] == 0) revert Errors.NotAllowedToken();
WardenPledge.sol:613:        if(newMinTargetVotes == 0) revert Errors.InvalidValue();
WardenPledge.sol:657:        if(amount == 0) revert Errors.NullValue();
```

## [G‑08]` >=` COSTS LESS GAS THAN `>`
The compiler uses opcodes GT and ISZERO for solidity code that uses >, but only requires LT for >=, which saves 3 gas https://gist.github.com/IllIllI000/3dc79d25acccfa16dee4e83ffdc6ffde 
```solidity
WardenPledge.sol:207:        if(percent > MAX_PCT) revert Errors.PercentOverMax();
WardenPledge.sol:234:        if(endTimestamp > pledgeParams.endTimestamp || endTimestamp != _getRoundedTimestamp(endTimestamp)) revert Errors.InvalidEndTimestamp();
WardenPledge.sol:245:        if(delegationBoost.adjusted_balance_of(pledgeParams.receiver) + amount > pledgeParams.targetVotes) revert Errors.TargetVotesOverflow();
WardenPledge.sol:267:        if(rewardAmount > pledgeAvailableRewardAmounts[pledgeId]) revert Errors.RewardsBalanceTooLow();
WardenPledge.sol:329:        if(vars.totalRewardAmount > maxTotalRewardAmount) revert Errors.IncorrectMaxTotalRewardAmount();
WardenPledge.sol:330:        if(vars.feeAmount > maxFeeAmount) revert Errors.IncorrectMaxFeeAmount();
WardenPledge.sol:389:        if(totalRewardAmount > maxTotalRewardAmount) revert Errors.IncorrectMaxTotalRewardAmount();
WardenPledge.sol:390:        if(feeAmount > maxFeeAmount) revert Errors.IncorrectMaxFeeAmount();
WardenPledge.sol:434:        if(totalRewardAmount > maxTotalRewardAmount) revert Errors.IncorrectMaxTotalRewardAmount();
WardenPledge.sol:435:        if(feeAmount > maxFeeAmount) revert Errors.IncorrectMaxFeeAmount();
WardenPledge.sol:463:        if(pledgeParams.endTimestamp > block.timestamp) revert Errors.PledgeNotExpired();
WardenPledge.sol:471:        if(remainingAmount > 0) {
WardenPledge.sol:504:        if(remainingAmount > 0) {
WardenPledge.sol:626:        if(newFee > 500) revert Errors.InvalidValue();
WardenPledge.sol:666:        if(n > type(uint64).max) revert Errors.NumberExceed64Bits();
```

## [G-9] Use `private` for `constants`
```solidity
WardenPledge.sol:22:    uint256 public constant UNIT = 1e18;
WardenPledge.sol:23:    uint256 public constant MAX_PCT = 10000;
WardenPledge.sol:24:    uint256 public constant WEEK = 7 * 86400;
```

## [G-10] Multiple address mappings can be combined into a single mapping of an `address` to a `struct`, where appropriate
Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to not having to recalculate the key's keccak256 hash (Gkeccak256 - 30 gas) and that calculation's associated stack operations.
```solidity
WardenPledge.sol:52:    mapping(address => uint256[]) public ownerPledges;
WardenPledge.sol:67:    mapping(address => uint256) public minAmountRewardToken;
```
