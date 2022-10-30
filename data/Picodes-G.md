### [GAS-1] `Pledge` variables could be batched

In `Pledge`, `targetVotes` and `rewardPerVote` could safely be stored in `uint128` to batch them and save a `SLOAD` in `_pledge`, `extendPledge` and `increasePledgeRewardPerVote`. Considering 18 decimals token there is no risk of overflow.

### [GAS-2] It can be cheaper to avoid loading `pledgeParams` in `memory`

In [`_pledge`](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L227), `pledgeParams` is loaded in memory but `votesDifference` is not used, hence is loaded for nothing and a `SLOAD` could be saved.