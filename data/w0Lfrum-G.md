# Gas Optimization Report

This report describes opportunity for gas optimization in the order of appearance in the code.

## G01: Bit Manipulation for Division by 2 

**Line Reference :**

[WardenPledge.sol#L263](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L263)

**Details and Solution**

- Using right shift for division by 2 saves gas.

- Replace `((bias * boostDuration) + bias) / 2;` with `((bias * boostDuration) + bias) >> 1;`

## G02: Using `private` Instead of `public` for Constants

**Line Reference :**

[WardenPledge.sol#22-24](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L22-L24)

**Details and Solution**

When the constants are declared as `private`, the compiler doesn't have to create separate non-payable getter functions. This results in saving gas.

## G03: Duplicated `revert()` Checks Should Be Refactored to a Modifier or Function

**Line Reference :**

    if(pledgeParams.endTimestamp <= block.timestamp) revert Errors.ExpiredPledge();  // 4 instances

    if(pledgeParams.closed) revert Errors.PledgeClosed();  // 3 instances

    if(minAmountRewardToken[token] == 0) revert Errors.NotAllowedToken();  // 2 instances

    if(totalRewardAmount > maxTotalRewardAmount) revert Errors.IncorrectMaxTotalRewardAmount();  // 3 instances

    if(pledgeId >= pledgesIndex()) revert Errors.InvalidPledgeID();  // 5 instances

    if(msg.sender != creator) revert Errors.NotPledgeCreator();  / 4 instances

    