## [G-01] Don't Initialize Variables with Default Value

Uninitialized variables are assigned with the types default value. Explicitly initializing a variable with it's default value costs unnecesary gas.

```
WardenPledge.sol::547 => for(uint256 i = 0; i < length;){
```

## [G-02] Use Shift Right/Left instead of Division/Multiplication if possible

A division/multiplication by any number x being a power of 2 can be calculated by shifting log2(x) to the right/left.

While the DIV opcode uses 5 gas, the SHR opcode only uses 3 gas. Furthermore, Solidity's division operation also includes a division-by-0 prevention which is bypassed using shifting.

```
WardenPledge.sol::24 => uint256 public constant WEEK = 7 * 86400;
WardenPledge.sol::263 => uint256 totalDelegatedAmount = ((bias * boostDuration) + bias) / 2;
```

## [G-03] Functions guaranteed to revert when called by normal users can be marked payable

If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are CALLVALUE(2),DUP1(3),ISZERO(3),PUSH2(3),JUMPI(10),PUSH1(3),DUP1(3),REVERT(0),JUMPDEST(1),POP(2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost

```
WardenPledge.sol::541 => function addMultipleRewardToken(address[] calldata tokens, uint256[] calldata minRewardsPerSecond) external onlyOwner {
WardenPledge.sol::560 => function addRewardToken(address token, uint256 minRewardPerSecond) external onlyOwner {
WardenPledge.sol::570 => function updateRewardToken(address token, uint256 minRewardPerSecond) external onlyOwner {
WardenPledge.sol::585 => function removeRewardToken(address token) external onlyOwner {
WardenPledge.sol::599 => function updateChest(address chest) external onlyOwner {
WardenPledge.sol::612 => function updateMinTargetVotes(uint256 newMinTargetVotes) external onlyOwner {
WardenPledge.sol::625 => function updatePlatformFee(uint256 newFee) external onlyOwner {
WardenPledge.sol::636 => function pause() external onlyOwner {
WardenPledge.sol::643 => function unpause() external onlyOwner {
WardenPledge.sol::653 => function recoverERC20(address token) external onlyOwner returns(bool) {
```

## [G-04] Usage of uints/ints smaller than 32 bytes (256 bits) incurs overhead

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

```
WardenPledge.sol::41 => uint64 endTimestamp;
```

## [G-05] <x> += <y> costs more gas than <x> = <x> + <y> for state variables

use <x> = <x> + <y> or <x> = <x> - <y> instead to save gas

```
WardenPledge.sol::268 => pledgeAvailableRewardAmounts[pledgeId] -= rewardAmount;
WardenPledge.sol::340 => pledgeAvailableRewardAmounts[vars.newPledgeID] += vars.totalRewardAmount;
WardenPledge.sol::401 => pledgeAvailableRewardAmounts[pledgeId] += totalRewardAmount;
WardenPledge.sol::445 => pledgeAvailableRewardAmounts[pledgeId] += totalRewardAmount;
```

## [G-06] Do not calculate constants

Due to how constant variables are implemented (replacements at compile-time), an expression assigned to a constant variable is recomputed each time that the variable is used, which wastes some gas.
Consequences: each usage of a constant costs more gas on each access. Since these are not real constants, they can't be referenced from a real constant environment (e.g. from assembly, or from another library)

```
WardenPledge.sol::24 => uint256 public constant WEEK = 7 * 86400;
```

## [G-07] Multiple address mappings can be combined into a single mapping of an address to a struct, where appropriate

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to not having to recalculate the key's keccak256 hash (Gkeccak256 - 30 gas) and that calculation's associated stack operations.

```
WardenPledge.sol::52 => mapping(address => uint256[]) public ownerPledges;
WardenPledge.sol::67 => mapping(address => uint256) public minAmountRewardToken;
```

## [G-08] Using storage instead of memory for structs/arrays saves gas

When fetching data from a storage location, assigning the data to a memory variable causes all fields of the struct/array to be read from storage, which incurs a Gcoldsload (2100 gas) for each field of the struct/array. If the fields are read from the new memory variable, they incur an additional MLOAD rather than a cheap stack read.

Instead of declearing the variable with the memory keyword, declaring the variable with the storage keyword and caching any fields that need to be re-read in stack variables, will be much cheaper, only incuring the Gcoldsload for the fields actually read. The only time it makes sense to read the whole struct/array into a memory variable, is if the full struct/array is being returned by the function, is being passed to a function that requires memory, or if the array/struct is being read from another memory array/struct

```
WardenPledge.sol::227 => Pledge memory pledgeParams = pledges[pledgeId];
```

## [G-09] State variables only set in the constructor should be declared immutable

Avoids a Gsset (20000 gas) in the constructor, and replaces each Gwarmacces (100 gas) with a PUSH32 (3 gas).

```
WardenPledge.sol::71 => uint256 public protocalFeeRatio = 250; //bps
WardenPledge.sol::79 => uint256 public minDelegationTime = 1 weeks;
WardenPledge.sol::137 => votingEscrow = IVotingEscrow(_votingEscrow);
WardenPledge.sol::138 => delegationBoost = IBoostV2(_delegationBoost);
```