# Report
## Gas Optimizations ## 

### [G-01]: Place subtractions where the operands can't underflow in unchecked {} block 
**Context:**

1. https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L268
2. https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L385
3. https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L430
4. https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L431

**Description:**

Some gas can be saved by using an unchecked {} block if an underflow isn't possible because of a previous require() or if-statement.


### [G-02]: Catch a value inside a mapping/array in local memory
**Context:**

``` 
        if(minAmountRewardToken[rewardToken] == 0) revert Errors.TokenNotWhitelisted();
        if(rewardPerVote < minAmountRewardToken[rewardToken]) revert Errors.RewardPerVoteTooLow();
```
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L312-L313

**Recommendation:**

Change:
```
        if(minAmountRewardToken[rewardToken] == 0) revert Errors.TokenNotWhitelisted();
        if(rewardPerVote < minAmountRewardToken[rewardToken]) revert Errors.RewardPerVoteTooLow();
```

to:
```
        uint256 _minAmountRewardToken = minAmountRewardToken[rewardToken]
        if(_minAmountRewardToken == 0) revert Errors.TokenNotWhitelisted();
        if(rewardPerVote < _minAmountRewardToken) revert Errors.RewardPerVoteTooLow();
```

### [G-03]: No need to catch state variable in local memory if it only use once 
**Context:**

1. https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L458
2. https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L490

**Recommendation:**

Read the state variable from storage.
