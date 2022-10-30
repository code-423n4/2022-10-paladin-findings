# [01] x += y costs more gas than x = x + y for state variables

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L340

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L401

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L445

Using the addition operator instead of plus-equals saves 113 [gas](https://gist.github.com/IllIllI000/cbbfb267425b898e5be734d4008d4fe8).

# [02] Cache state variable reads

Caching of a state variable replace each Gwarmaccess (100 gas) with a cheaper stack read.

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L312-L313