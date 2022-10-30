# missing input validation in constructor

[https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L131-L143](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L131-L143)

In `constructor`, input validation is missing. `votingEscrow` and `delegationBoost`are unchangeable after deployment, so should be validated in `constructor`.  `chestAddress` and `minTargetVotes` changeable in update function. Since update function has input validation, variables should be validated in `constructor`, too.