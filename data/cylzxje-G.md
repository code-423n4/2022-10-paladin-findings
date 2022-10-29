### Gas Optimizations:
##### [G-01] Use `storage` instead of `memory` for pledgeParams
##### [G-02] Unnecessary computation `slope`  
##### [G-03] Use `payable` for `onlyOwner` function
##### [G-04] Use `!=` instead of `<`

---
### Details:
#### [G-01] Use `storage` instead of `memory`
Since this variable accessing storage it's more gas efficient to declare it as `storage`
- https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L227

Recommend changing from memory to `storage`   
```sol
Pledge storage pledgeParams = pledges[pledgeId];
```


#### [G-02] Unnecessary computation `slope`
`slop` isn't used anywhere outside function _pledge() 

```sol
L256-257:
uint256 slope = amount / boostDuration;
uint256 bias = slope * boostDuration;
```  

Recommend changing to `uint256 bias = amount / boostDuration * boostDuration;`



#### [G-03] Use `payable` for `onlyOwner` function
Since these functions are only for developers `onlyOwner`, the chances of accidentally sending ETH are very low 

- https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L541
- https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L560
- https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L570
- https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L585
- https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L599
- https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L612
- https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L625
- https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L636
- https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L643
- https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L653

Recommend marking these functions `payable` to save some gas



#### [G-04] Use `!=` instead of `<`
Optimize for loop (not in automated findings)
- https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L547

Recommend changing to:
```sol
for(uint256 i; i != length;)
```
