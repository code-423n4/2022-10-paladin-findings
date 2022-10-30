# Gas Optimizations Report for Paladin - Warden Pledges contest

## Overview
During the audit, 3 gas issues were found.  
Total savings ~200.

№ | Title | Instance Count
--- | --- | --- 
G-1 | [Use unchecked blocks for subtractions where underflow is impossible](#g-1-use-unchecked-blocks-for-subtractions-where-underflow-is-impossible) | 4
G-2 | [Use local variable cache instead of accessing mapping or array multiple times](#g-2-use-local-variable-cache-instead-of-accessing-mapping-or-array-multiple-times) | 1
G-3 | [Unnecessary write to the memory variable](#g-3-unnecessary-write-to-the-memory-variable) | 2

## Gas Optimizations Findings (3)
### G-1. Use unchecked blocks for subtractions where underflow is impossible
##### Description
In Solidity 0.8+, there’s a default overflow and underflow check on unsigned integers. When an overflow or underflow isn’t possible (after require or if-statement), some gas can be saved by using [unchecked blocks](https://docs.soliditylang.org/en/v0.8.17/control-structures.html#checked-or-unchecked-arithmetic).

##### Instances 
- https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L268
- https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L385
- https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L430
- https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L431

##### Saved
This saves ~35.  
So, ~35*4 = 140

#
### G-2. Use local variable cache instead of accessing mapping or array multiple times
##### Description
It saves gas due to not having to perform the same key’s keccak256 hash and/or offset recalculation.

##### Instances 
- (minAmountRewardToken[rewardToken], 2nd access) https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L313

##### Saved
This saves ~40.  
So, ~40*1 = 40

#
### G-3. Unnecessary write to the memory variable
##### Description
When array or mapping accessed only once, there is no need to use local variable.

##### Instances
- https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L458
- https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L490

##### Saved
This saves ~3.  
So, ~3*2 = 6