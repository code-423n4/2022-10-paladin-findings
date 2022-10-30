# QA Report
## NC Found [2]

### [NC-01] Underscore Notation Improves Readability

Solidity allows for the use of _ between every 3 digits of large numbers.

#### Findings [2]:

*/contracts/WardenPledge.sol*
Line(s): [23](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L23), [24](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L24)
```solidity
23:	uint256 public constant MAX_PCT = 10000;
24:	uint256 public constant WEEK = 7 * 86400;
```

### [NC-02] Misspelled Variable 

Misspelling variables makes code harder to follow.

#### Findings [1]:

*/contracts/WardenPledge.sol*
Line(s): [71](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L71)

```solidity
71:	uint256 public protocalFeeRatio = 250; //bps
```

**should be named**

```solidity
71:	uint256 public protocolFeeRatio = 250; //bps
```