# minDelegationTime can be declared as a private constant
It is not modifed anywhere

# _pledge can save gas if first we check amount
The amount check do just one comparison and no function call, so ```amount == 0``` is cheaper than ```pledgeId >= pledgesIndex()```. Replace [these line](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L223-L224) for:
```solidity
if(amount == 0) revert Errors.NullValue();
if(pledgeId >= pledgesIndex()) revert Errors.InvalidPledgeID();
```

# _pledge function call pledgesIndex() can be omitted
Calling ```pledgesIndex()``` is more expensive than just calling ```pledges.length```. Replace [this line](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L223) for
```solidity
if(pledgeId >= pledges.length) revert Errors.InvalidPledgeID();
```

# _pledge can cache delegationBoost
This state variable is used multiple times, it can be cached in order to save gas.

```solidity
_pledgeAvailableRewardAmounts = pledgeAvailableRewardAmounts[pledgeId]
if(rewardAmount > _pledgeAvailableRewardAmounts) revert Errors.RewardsBalanceTooLow();
pledgeAvailableRewardAmounts[pledgeId] = _pledgeAvailableRewardAmounts - rewardAmount;
```

# createPledge call pledgesIndex() can be omitted
This state variable is used multiple times, it can be cached in order to save gas.

```solidity
// vars.newPledgeID = pledgesIndex(); //before
vars.newPledgeID = plendge.length();
```

# createPledge double access to state variable minAmountRewardToken\[rewardToken\] can be omitted
This state variable is accessed two times, it can be cached in order to save gas.

```solidity
if(receiver == address(0) || rewardToken == address(0)) revert Errors.ZeroAddress();
if(targetVotes < minTargetVotes) revert Errors.TargetVoteUnderMin();

uint256 _minAmountRewardToken = minAmountRewardToken[rewardToken];
if(_minAmountRewardToken == 0) revert Errors.TokenNotWhitelisted();
if(rewardPerVote < _minAmountRewardToken) revert Errors.RewardPerVoteTooLow();
```

# extendPledge call pledgesIndex() can be omitted
Calling ```pledgesIndex()``` is more expensive than just calling ```pledges.length```. Replace [this line](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L374) for
```solidity
if(pledgeId >= pledges.length) revert Errors.InvalidPledgeID();
```

# extendPledge: pledgeParams.rewardToken is accessed twice
Replace [these lines](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L393-L396) for:
```solidity
address _rewardToken = pledgeParams.rewardToken
// Pull all the rewards in this contract
IERC20(_rewardToken).safeTransferFrom(creator, address(this), totalRewardAmount);
// And transfer the fees from the Pledge creator to the Chest contract
IERC20(_rewardToken).safeTransferFrom(creator, chestAddress, feeAmount);
```

# increasePledgeRewardPerVote call pledgesIndex() can be omitted
Calling ```pledgesIndex()``` is more expensive than just calling ```pledges.length```. Replace [this line](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L420) for
```solidity
if(pledgeId >= pledges.length) revert Errors.InvalidPledgeID();
```

# increasePledgeRewardPerVote: pledgeParams.rewardToken is accessed twice
Replace [these lines](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L437-L440) for:
```solidity
address _rewardToken = pledgeParams.rewardToken
// Pull all the rewards in this contract
IERC20(_rewardToken).safeTransferFrom(creator, address(this), totalRewardAmount);
// And transfer the fees from the Pledge creator to the Chest contract
IERC20(_rewardToken).safeTransferFrom(creator, chestAddress, feeAmount);
```

# retrievePledgeRewards call pledgesIndex() can be omitted
Calling ```pledgesIndex()``` is more expensive than just calling ```pledges.length```. Replace [this line](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L457) for
```solidity
if(pledgeId >= pledges.length) revert Errors.InvalidPledgeID();
```

# closePledge call pledgesIndex() can be omitted
Calling ```pledgesIndex()``` is more expensive than just calling ```pledges.length```. Replace [this line](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L489) for
```solidity
if(pledgeId >= pledges.length) revert Errors.InvalidPledgeID();
```

# increasePledgeRewardPerVote: can use unchecked block for newRewardPerVote - oldRewardPerVote
Given that ```newRewardPerVote <= oldRewardPerVote``` was [previously check](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L429), it is impossible for ```newRewardPerVote - oldRewardPerVote``` to underflow. Replace [this line](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L431) for
```solidity
unchecked{
    uint256 rewardPerVoteDiff = newRewardPerVote - oldRewardPerVote;
}

```

# increasePledgeRewardPerVote: can use unchecked block for pledgeParams.endTimestamp - block.timestamp
Given that ```pledgeParams.endTimestamp <= block.timestamp``` was [previously check](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L426), it is impossible for ```pledgeParams.endTimestamp - block.timestamp``` to underflow. Replace [this line](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L430) for
```solidity
unchecked{
    uint256 remainingDuration = pledgeParams.endTimestamp - block.timestamp;
}
```

# increasePledgeRewardPerVote: can use unchecked block for pledgeAvailableRewardAmounts\[pledgeId\] += totalRewardAmount;
Given that this amount was [previously transfered](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L438), if this transfer was successful we can assure that it won't be an overflow, replace [this line](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L445) for:
```solidity
unchecked{
    pledgeAvailableRewardAmounts[pledgeId] += totalRewardAmount;
}
```

# extendPledge: can use unchecked block for newEndTimestamp - oldEndTimestamp calculation
Possible underflow was [previously checked](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L383). Replace [this line](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L385) for
```solidity
unchecked{
    uint256 addedDuration = newEndTimestamp - oldEndTimestamp;
}
```

# extendPledge: can use unchecked block for pledgeAvailableRewardAmounts\[pledgeId\] += totalRewardAmount;
Given that this amount was [previously transfered](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L394), if this transfer was successful we can assure that it won't be an overflow, replace [this line](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L401) for:
```solidity
unchecked{
    pledgeAvailableRewardAmounts[pledgeId] += totalRewardAmount;
}
```

# createPledge: can use unchecked block for pledgeAvailableRewardAmounts\[vars.newPledgeID\] += vars.totalRewardAmount;
Given that this amount was [previously transfered](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L333), if this transfer was succesful we can assure that it won't be an overflow, replace [this line](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L340) for:
```solidity
unchecked{
    pledgeAvailableRewardAmounts[vars.newPledgeID] += vars.totalRewardAmount;
}
```

# _pledge: can use unchecked block for pledgeAvailableRewardAmounts\[pledgeId\] -= rewardAmount;
Given that this amount was [previously checked](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L267), we can assure that it won't be an underflow, replace [this line](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L268) for:
```solidity
unchecked{
    pledgeAvailableRewardAmounts[pledgeId] -= rewardAmount;
}
```