1. https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L312
Cache minAmountRewardToken[rewardToken] in createPledge() as it has been accessed multiple times.

2. 