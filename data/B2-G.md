##  Multiple  mappings of same `key` can be combined into a single mapping to a struct, where appropriate.

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot

```
    mapping(uint256 => address) public pledgeOwner;
    mapping(uint256 => uint256) public pledgeAvailableRewardAmounts;
```
>File: contracts/WardenPledge.sol 
(https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L50 &
(https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L56)
```
    mapping(address => uint256[]) public ownerPledges;
    mapping(address => uint256) public minAmountRewardToken;
```
>File: contracts/WardenPledge.sol 
 (https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L52) &
 (https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L67)

## Use `unchecked` to save gas

```
        pledgeAvailableRewardAmounts[pledgeId] -= rewardAmount;

```
>File: contracts/WardenPledge.sol [(line 268)](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L268) 

## `storage` pointer to a structure is cheaper than copying each value of the structure into `memory,` same for `array` and `mapping`

 ```
        CreatePledgeVars memory vars;

```
>File: contracts/WardenPledge.sol [(line 318)](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L318) 

## <x> += <y> costs more gas than <x> = <x> + <y> for state variables

 ```
        pledgeAvailableRewardAmounts[pledgeId] += totalRewardAmount;

```
>File: contracts/WardenPledge.sol [(line 445)](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L445) 

 ```
        pledgeAvailableRewardAmounts[pledgeId] += totalRewardAmount;

```
>File: contracts/WardenPledge.sol [(line 401)](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L401) 
