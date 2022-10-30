## Missing check for `votesDifference`

There is no check for `votesDifference` in https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L325. If `receiver` balance of veCRV equals `targetVotes` this would create a `Pledge` with zero `totalRewardAmount`, making this pledge useless as no rational user will pledge without receiving rewards.

```solidity
    vars.votesDifference = targetVotes - votingEscrow.balanceOf(receiver);

    vars.totalRewardAmount = (rewardPerVote * vars.votesDifference * vars.duration) / UNIT;
```

Please consider checking `vars.votesDifference > 0` and reverting with the appropriate error.  


## `totalDelegatedAmount` and `rewardAmount` calculated incorrectly

The following expression for calculating `totalDelegatedAmounts` has an extra `bias` term (https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L259-L265), opposed to what is defined in boostV2 contract (https://github.com/curvefi/curve-veBoost/blobdb3dec43b6e4fac0fca1f01509f9133563f43ebb/contracts/BoostV2.vy#L191-L206). Therefore the `totalDelegatedAmounts` overstates the actual amount pledged per sec, consequently also overstating `rewardAmount`.

```solidity
    uint256 totalDelegatedAmount = ((bias * boostDuration) + bias) / 2;
```

Please consider removing the extra bias term, as shown below.
```solidity
    uint256 totalDelegatedAmount = (bias * boostDuration) / 2;
```