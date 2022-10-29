# Gas Optimization Report

This report describes opportunity for gas optimization in the order of appearance in the code.

## Bit Manipulation for Division by 2 

**Line Reference :**

[WardenPledge.sol#L263](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L263)

**Details and Solution**

- Using right shift for division by 2 saves gas.

- Replace `((bias * boostDuration) + bias) / 2;` with `((bias * boostDuration) + bias) >> 1;`