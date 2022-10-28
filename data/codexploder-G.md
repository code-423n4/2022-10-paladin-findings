## Checks are missing

Contract:
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L207

Function:
pledgePercent

Issue:
In pledgePercent function, percent can be defined as 0. This will fail later in _pledge function at (wasting gas)

```
if(amount == 0) revert Errors.NullValue();
```

Recommendation:
Add a new check percent>0

___

## Incorrect operator used

Contract:
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L340

Function:
createPledge

Issue:
The pledgeAvailableRewardAmounts is updated like below. This is first time initialization of pledgeAvailableRewardAmounts so += is not required

```
pledgeAvailableRewardAmounts[vars.newPledgeID] += vars.totalRewardAmount;
```

Recommendation:
Change the initialization as below:

```
pledgeAvailableRewardAmounts[vars.newPledgeID] = vars.totalRewardAmount;
```