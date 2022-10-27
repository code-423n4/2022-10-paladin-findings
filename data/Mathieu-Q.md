# No threshold for the minimum target of votes
In the `WardenPledge.sol` contract, there is no threshold for the `minTargetVotes` contract variable which could be manipulated by the contract's owner.

I would recommend to add a threshold in the update function of the variable: `updateMinTargetVotes`
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L612:L618

# Possible execution error on pledge creation
During the creation of a pledge (`createPledge` function), there is no check that `endTimestamp >= block.timestamp` as we can see in the others functions. In the case where a creator accidentally put an `endTimestamp` value lower than `block.timestamp`, it will throw an execution error and the transaction will be reverted because the second value is subtracted from the first (line 319) and an `uint` cannot be a negative value. We can just avoid this kind of accidental error by adding a check below line 316 - https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L316 :

    if (endTimestamp < block.timestamp) revert Errors.InvalidEndTimestamp();