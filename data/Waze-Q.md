## [L-01] Missing zero address for constructor
## Code Snipped
https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L131
## Recommendation
Checking addresses against zero-address during initialization in constructor is a security best-practice. However, such checks are missing in multiple constructors.  Allowing zero-addresses will lead to contract reverts and force redeployments if there are no setters for such address variables. So we recommend to Add zero-address checks in the constructors.

## [L-02] Missing check zero address
## code snipped
https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L222
## recommendation
to avoid zero address in parameter input. we suggest to add zero check address to the function.

## [N-01] Missing indexed field
## code snipped
https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L85
https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L115
## recommendation
event is missing indexed fields in important parameter. we recommend indexed at important field for increase creadibility.


