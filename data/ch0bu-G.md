## 1. Add `unchecked {}` for substractions where the operands cannot underflow because of a previous `require()` or `if` statement

`require(a <= b); x = b - a` => `require(a <= b); unchecked { x = b - a }`

```
237	uint256 boostDuration = endTimestamp - block.timestamp;
385	uint256 addedDuration = newEndTimestamp - oldEndTimestamp;
430	uint256 remainingDuration = pledgeParams.endTimestamp - block.timestamp;
431	uint256 rewardPerVoteDiff = newRewardPerVote - oldRewardPerVote;
```
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol


## 2. Usage of `uint/int` smaller than 32 bytes (256 bits) incurs overhead

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html Each operation involving a `uint8` costs an extra 22-28 gas (depending on whether the other operand is also a variable of type `uint8`) as compared to ones involving `uint256`, due to the compiler having to clear the higher bits of the memory word before operating on the `uint8`, as well as the associated stack operations of doing so. Use a larger size then downcast where needed.

```
41	uint64 endTimestamp;
```
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol


## 3. Not using the named return variables when a function returns, wastes deployment gas

```
154	return pledges.length;
164	return ownerPledges[user];
182	return (timestamp / WEEK) * WEEK;
357	return vars.newPledgeID;
```

## 4. Constructor parameters should be avoided when possible

Constructor parameters are expensive. The contract deployment will be cheaper in gas if they are hard coded instead of using constructor parameters.

```
140	chestAddress = _chestAddress;
142	minTargetVotes = _minTargetVotes
```
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol
