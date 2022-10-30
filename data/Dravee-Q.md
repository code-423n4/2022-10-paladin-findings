# 1. Low Risk Issues

## 1.1. `chestAddress` can be set to 0 in the constructor and DOS `createPledge()`

There's no sanity-check to prevent a mistake such as setting `chestAddress = 0` in the [constructor](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L140). Notice that the check does exist in [updateChest()](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L600). If `chestAddress == 0`, [the following line in createPledge() would revert for some tokens such as USDC](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L335), which would prevent pledges from being created until the `owner` reacts to the mistake.

## 1.2. `minTargetVotes` can be set to 0 in the constructor and DOS `createPledge()`

Similar to above, `minTargetVotes = 0` is possible in the [constructor](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L142), while the check does exist in [updateMinTargetVotes()](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L613). If `minTargetVotes == 0`, [the following require statement will always fail in createPledge()](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L311), which would prevent pledges from being created until the `owner` reacts to the mistake.

# 2. Non-Critical Issue

## 2.1. `pledgesIndex()` should be renamed as `nextPledgeID()`

According to the documentation, [`pledgesIndex()`](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L153-L155) is the `Amount of Pledges listed in this contract`, but the name is hacky (actually, `pledgesIndex()` returning `pledges.length` is returning a non-existing (yet) index, used to check the upper bound of indexes or to get the `nextPledgeId`). While its use is perfect in the code, it'd be better named as `nextPledgeID()` or `pledgesLength`
