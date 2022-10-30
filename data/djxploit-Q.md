
## 1) Usage of `block.timestamp` is risky, as it can be manipulated by miners, so avoid it.

[https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L237](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L237)
[https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L319](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L319)
[https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L380](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L380)
[https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L426](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L426)

## 2) Missing 0-address check

[https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L137-L140](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L137-L140)


## 3) `Check , effect and interact pattern` is not followed below. State is updated after making an external call. 

[https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L394_L401](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L394_L401)

## 4) Event is missing `indexed` fields

[https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L94_L98](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L94_L98)
[https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L102_L105](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L102_L105)


## 5) `TYPOS` should be resolved, to avoid confusion and enhance readability

(https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L292)[https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L295](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L295)[https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L296](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L296)[https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L411](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L411)[https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L412](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L412)[https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L453](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L453)[https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L485](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L485)
