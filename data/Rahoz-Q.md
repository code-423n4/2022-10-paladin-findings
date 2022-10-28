## N. FUNCTION ORDER
Functions should be ordered following the Solidity conventions.
Link: https://docs.soliditylang.org/en/v0.8.15/style-guide.html#order-of-functions
### Proof of Concept
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol
### Recommended Mitigation Steps

## L. Check update with old value
Function `updatePlatformFee`, `updateMinTargetVotes` and `updateChest` should prevent owner update newFee same with old value
### Proof of Concept
https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L625-L631
https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L612-L618
https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L599-L605
### Recommended Mitigation Steps
Add check `newFee` should different with `protocalFeeRatio`
Add check `newMinTargetVotes` should different with `minTargetVotes`
Add check `chest` should different with `chestAddress`

## L. Missing Input Sanity Check
The function `constructor` does not check for zero address for `_votingEscrow`, `_delegationBoost` and `_chestAddress`
### Proof of Concept
https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L131-L143
### Recommended Mitigation Steps
Add zero check