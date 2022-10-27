# No threshold for the minimum target of votes
In the `WardenPledge.sol` contract, there is no threshold for the `minTargetVotes` contract variable which could be manipulated by the contract's owner.

I would recommend to add a threshold in the update function of the variable: `updateMinTargetVotes`
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L612:L618