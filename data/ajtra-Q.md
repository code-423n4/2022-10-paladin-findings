# Summary
## Low
1. L01 - Missing checks for address(0x0) when assigning values to address state variables

## Non Critical
2. NC01 - Event is missing indexed fields
3. NC02 - Outdated compiler version
4. NC03 - No reentrant modifier should be in the first place

## L01 - Missing checks for address(0x0) when assigning values to address state variables

### Mitigation
Add check for address(0x0)

### Lines in the code
[WardenPledge.sol#L137-L140](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L137-L140)
	
# Non Critical
## NC01 - Event is missing indexed fields
### Description
Index event fields make the field more quickly accessible to off-chain tools that parse events. 
However, note that each index field costs extra gas during emission, so it's not necessarily best to index the maximum allowed per event (threefields). 
Each event should use three indexed fields if there are three or more fields, and gas usage is not particularly of concern for the events in question. 
If there are fewer than three fields, all of the fields should be indexed.

### Lines in the code
[WardenPledge.sol#L85-L119](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L85-L119)


## NC02 - Outdated compiler version
### Description
The project is using the solidity version 0.8.10. It's a best practice to use the latest release version. 
You can consult it in the following [link](https://github.com/ethereum/solidity/releases)

### Mitigation 
Update the solidity version to 0.8.17

### Lines in the code
[WardenPledge.sol#L2](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L2)
	
## NC03 - No reentrant modifier should be in the first place
[WardenPledge.sol#L206](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L206)
[WardenPledge.sol#L307](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L307)
[WardenPledge.sol#L373](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L373)
[WardenPledge.sol#L419](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L419)
[WardenPledge.sol#L456](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L456)
[WardenPledge.sol#L488](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L488)

