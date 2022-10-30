# Gas optimizations
## Redundant variables
### `creator` in `createPledge()` can be replaced with `msg.sender`
[WardenPledge.sol L308](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L308)
Redundant variable `creator` can be removed - use `msg.sender` instead. This slightly reduces contract size and interaction cost on `createPledge()`
Using the project tests we see the following gas improvement after removal:

|  Contract  |  Method   |  Min        |  Max        |  Avg        |  # calls      |  eur (avg)  |
|---|---|---|---|---|---|---|
|  WardenPledge  |  createPledge                 |     284406  |     342104  |     327297  |           85  |          -  |
|  WardenPledge                                  |          -  |          -  |    2695837  |          9 %  |          -  |
|  **After:**                                  |  |  |  |  |  |
|  WardenPledge  |  createPledge                 |     284390  |     342088  |     327281  |           85  |          -  |
|  WardenPledge                                  |          -  |          -  |    2694757  |          9 %  |          -  |
|  **Diff:**                                  |  |  |  |  |  |
|  WardenPledge  |  createPledge                 |     **-16** |     **-16** |     **-16** |             |          -  |
|  WardenPledge                                  |          -  |          -  |    **-1080** |           |          -  |

### `creator` in `retrievePledgeRewards()` can be removed
[WardenPledge.sol L458](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L458)
Redundant variable `creator` can be removed. This slightly reduces contract size and interaction cost on `retrievePledgeRewards()`. Use `pledgeOwner[pledgeId]` in the if statement instead.
Using the project tests we see the following improvement after removal:

|  Contract  |  Method   |  Min        |  Max        |  Avg        |  # calls      |  eur (avg)  |
|---|---|---|---|---|---|---|
|  WardenPledge  |  retrievePledgeRewards        |      35659  |      74513  |      53254  |           12  |          -  |
|  WardenPledge                                  |          -  |          -  |    2695837  |          9 %  |          -  |
|  **After:**                                  |  |  |  |  |  |
|  WardenPledge  |  retrievePledgeRewards        |      35654  |      74508  |      53249  |           12  |          -  |
|  WardenPledge                                  |          -  |          -  |    2693257  |          9 %  |          -  |
|  **Diff:**                                  |  |  |  |  |  |
|  WardenPledge  |  retrievePledgeRewards        |      **-5** |      **-5** |      **-5** |           |          -  |
|  WardenPledge                                  |          -  |          -  |    **-2580** |            |          -  |

### `creator` in `closePledge()` can be replaced with `msg.sender`
[WardenPledge.sol L490](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L490)
Redundant variable `creator` can be removed - use `msg.sender` instead. This slightly reduces contract size and interaction cost on `closePledge()`
Using the project tests we see the following gas improvement after removal:

|  Contract  |  Method   |  Min        |  Max        |  Avg        |  # calls      |  eur (avg)  |
|---|---|---|---|---|---|---|
|  WardenPledge  |  closePledge                  |      52205  |      75604  |      59111  |           15  |          -  |
|  WardenPledge                                  |          -  |          -  |    2695837  |          9 %  |          -  |
|  **After:**                                  |  |  |  |  |  |
|  WardenPledge  |  closePledge                  |      52200  |      75599  |      59106  |           15  |          -  |
|  WardenPledge                                  |          -  |          -  |    2695429  |          9 %  |          -  |
|  **Diff:**                                  |  |  |  |  |  |
|  WardenPledge  |  closePledge                  |      **-5** |      **-5** |      **-5** |           |          -  |
|  WardenPledge                                  |          -  |          -  |    **-408** |            |          -  |

### Redundant variables `oldMinTarget` and `oldfee` in update methods
[WardenPledge.sol L614](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L614)
[WardenPledge.sol L627](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L627)
By emitting before assignment you can remove the redundant variable. There is a slight increase in deployment cost however.
Using the project tests we see the following gas improvement after removal of both variables:

|  Contract  |  Method   |  Min        |  Max        |  Avg        |  # calls      |  eur (avg)  |
|---|---|---|---|---|---|---|
|  WardenPledge  |  updateMinTargetVotes         |          -  |          -  |      30121  |            2  |          -  |
|  WardenPledge  |  updatePlatformFee            |          -  |          -  |      30037  |            2  |          -  |
|  WardenPledge                                  |          -  |          -  |    2695837  |          9 %  |          -  |
|  **After:**                                  |  |  |  |  |  |
|  WardenPledge  |  updateMinTargetVotes         |          -  |          -  |      30107  |            2  |          -  |
|  WardenPledge  |  updatePlatformFee            |          -  |          -  |      30012  |            2  |          -  |
|  WardenPledge                                  |          -  |          -  |    2696929 |          9 %  |          -  |
|  **Diff:**                                  |  |  |  |  |  |
|  WardenPledge  |  updateMinTargetVotes         |          -  |          -  |      **-14** |              |          -  |
|  WardenPledge  |  updatePlatformFee            |          -  |          -  |      **-25** |              |          -  |
|  WardenPledge                                  |          -  |          -  |    **+1092** |            |          -  |


## Redundant operations
### Redundant add operation in `createPledge()`
[WardenPledge.sol L340](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L340)
Redundant addition costs more gas than assignment. `pledgeAvailableRewardAmounts` of the newly created pledge will always start with 0, so the add sign doesn't make much sense and increases gos cast on each pledge creation.
Using the project tests we see the following improvement by removing the addition:

|  Contract  |  Method   |  Min        |  Max        |  Avg        |  # calls      |  eur (avg)  |
|---|---|---|---|---|---|---|
|  WardenPledge  |  createPledge                 |     284406  |     342104  |     327297  |           85  |          -  |
|  WardenPledge                                  |          -  |          -  |    2695837  |          9 %  |          -  |
|  **After:**                                  |  |  |  |  |  |
|  WardenPledge  |  createPledge                 |     284312  |     342010  |     327203  |           85  |          -  |
|  WardenPledge                                  |          -  |          -  |    2689362  |          9 %  |          -  |
|  **Diff:**                                  |  |  |  |  |  |
|  WardenPledge  |  createPledge                 |     **-94** |     **-94** |     **-94** |             |          -  |
|  WardenPledge                                  |          -  |          -  |    **-6475** |            |          -  |

### Redundant return statement in `recoverERC20()`
[WardenPledge.sol L660](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L660)
The return statement in `recoverERC20()` always returns true. Seems redundant, the function and transfer of funds will revert on failure, or pass otherwise. Removing return statement saves some gas.
Using the project tests here's the gas improvement after removing the return statement:

|  Contract  |  Method   |  Min        |  Max        |  Avg        |  # calls      |  eur (avg)  |
|---|---|---|---|---|---|---|
|  WardenPledge  |  recoverERC20                 |          -  |          -  |      39654  |            1  |          -  |
|  WardenPledge                                  |          -  |          -  |    2695837  |          9 %  |          -  |
|  **After:**                                  |  |  |  |  |  |
|  WardenPledge  |  recoverERC20                 |          -  |          -  |      39579  |            1  |          -  |
|  WardenPledge                                  |          -  |          -  |    2693905  |          9 %  |          -  |
|  **Diff:**                                  |  |  |  |  |  |
|  WardenPledge  |  recoverERC20                 |          -  |          -  |      -75 |              |          -  |
|  WardenPledge                                  |          -  |          -  |    -1932 |            |          -  |
