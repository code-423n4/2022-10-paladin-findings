# Gas Report
## Optimizations found [3]

### [G-01] Variable Can be Made Constant to Save Gas 

Variables that are the same value no matter the context can be made `constant` to save gas.

`minDelegationTime` can be made `constant`. If it is not called by other functions (it is `constant` now so it won't need to be) it can also be made `private` to save more gas.

#### Findings [1]:

*/contracts/WardenPledge.sol*
Line(s): [79](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L79)
```solidity
79:	uint256 public minDelegationTime = 1 weeks;
```

### [G-02] Variable Can be Made Immutable to Save Gas 

If a variable is only initialized in the constructor of a function it can be made `immutable` to save gas.

**Example:**

```solidity
60:	IVotingEscrow public votingEscrow;
```

**to**

```solidity
60:	IVotingEscrow public immutable votingEscrow;
```

#### Findings [2]:

*/contracts/WardenPledge.sol*
Line(s): [60](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L60), [62](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L62)
```solidity
60:	IVotingEscrow public votingEscrow;
62:	IBoostV2 public delegationBoost;
```

### [G-03] Using `unchecked` when Arithmetic Cnnot Oerflow Saves gas

There are mutliple occurances where arithmetic is performed that cannot overflow / underflow based on a previous require. Adding an `unchecked` brace around these occurances will save gas (Solidity will not force an underflow / overflow).

**NOTE:** Findings show the check before each line that allows the second line to be `unchecked`.  Only the line following the check should be unchecked (NOT the check itself).

```solidity
unchecked {
	//Code
}
```

#### Findings [3]:

*/contracts/WardenPledge.sol*
Line(s): [267-268](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L267-L268), [383-385](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L383-L385), [429](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L429)/[431](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L431)

```solidity
267:	ewardAmount > pledgeAvailableRewardAmounts[pledgeId]) revert Errors.RewardsBalanceTooLow();
268:	eAvailableRewardAmounts[pledgeId] -= rewardAmount;
```

```solidity
383:	if(newEndTimestamp != _getRoundedTimestamp(newEndTimestamp) || newEndTimestamp < oldEndTimestamp) revert Errors.InvalidEndTimestamp();
385:	uint256 addedDuration = newEndTimestamp - oldEndTimestamp;
```

```solidity
429:	if(newRewardPerVote <= oldRewardPerVote) revert Errors.RewardsPerVotesTooLow();
431:	uint256 rewardPerVoteDiff = newRewardPerVote - oldRewardPerVote;
```