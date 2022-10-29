Dangerous strict equalities (incorrect-equality)
https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L224


Recommendation: Don't use strict equality to determine if an account has enough Ether or tokens  `if(amount >= 0)`