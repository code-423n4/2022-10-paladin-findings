## [L-01] lack of check on "endTimeStamp" in "_pledge" function.

Within the "_pledge" function. There is no check that the "endTimeStamp" variable provided by the user is not lower than block.timestamp (i.e. that the provided date is not in the past). If this is the case, the function will revert on line 237 due to the overflow protection of the EVM since the solidity version used is 0.8.X.

## [L-02] owner can renounce Ownership

The Openzeppelin’s Ownable contract implements renounceOwnership. This can represent a certain risk if the ownership is renounced for any other reason than by design. Renouncing ownership will leave the contract without an owner, thereby removing any functionality that is only available to the owner.

WardenPledge.sol#L18

## [L-03] Consider two phases Ownership transfer

Admin calls Ownable.transferOwnership function to transfers the ownership to the new address directly. As such, there is a risk that the ownership is transferred to an invalid address, thus causing the contract to be without a owner.

WardenPledge.sol#L18


## [N-01] Large multiples of ten should use scientific notation

Use (e.g. 1e6) rather than decimal literals (e.g. 1000000), for better code readability

WardenPledge.sol#L23


## [N-02] use native notation (i.e. 1 week) instead of large numbers (i.e. 7 * 86400)

WardenPledge.sol#L24 


## [N-03] Use of block.timestamp

Block timestamps have historically been used for a variety of applications, such as entropy for random numbers (see the Entropy Illusion for further details), locking funds for periods of time, and various state-changing conditional statements that are time-dependent. Miners have the ability to adjust timestamps slightly, which can prove to be dangerous if block timestamps are used incorrectly in smart contracts.

WardenPledge.sol#L229
WardenPledge.sol#L237
WardenPledge.sol#L319
WardenPledge.sol#L380
WardenPledge.sol#L426
WardenPledge.sol#L430
WardenPledge.sol#L463
WardenPledge.sol#L496


## [N-04] Consider declaring constants instead of magic numbers

WardenPledge.sol#L626


## [N-05] Consider using a more recent version of solidity.

Different bug corrections have been released after version 0.8.10. For instance, version 0.8.13 Correctly encode literals used in abi.encodeCall in place of fixed bytes arguments. Version 0.8.15 avoids writing dirty bytes to storage when copying bytes arrays.