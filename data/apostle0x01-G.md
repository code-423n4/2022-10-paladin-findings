## [G-01] Multiplication/division by two should use bit shifting

<x> * 2 is equivalent to <x> << 1 and <x> / 2 is the same as <x> >> 1. The MUL and DIV opcodes cost 5 gas, whereas SHL and SHR only cost 3 gas

There is 1 instances in 1 file:
## POC 

```solidity
File: contracts/WardenPledge.sol

263:        uint256 totalDelegatedAmount = ((bias * boostDuration) + bias) / 2;
```

https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L263



## [G-02] Using private rather than public for constants, saves gas

The values can still be inspected on the source code if necessary.

## POC

```solidity
File: contracts/WardenPledge.sol
22:    uint256 public constant UNIT = 1e18;
23:    uint256 public constant MAX_PCT = 10000;
24:    uint256 public constant WEEK = 7 * 86400;
```
