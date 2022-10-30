# [L] Amount to delegate is not checked thoroughly

in `_pledge()` internal function, the `amount` value is only check if it's not 0, but not being checked to at least to make the slope > 0 (in L:256), in other word if the amount is less than `boostDuration` value, it will make the slope 0, when this slope is 0, the `bias`, `totalDelegatedAmount`, and `rewardAmount` will also 0, thus waste of gas for transferring 0 token reward.
```
File: WardenPledge.sol
234:         if(endTimestamp > pledgeParams.endTimestamp || endTimestamp != _getRoundedTimestamp(endTimestamp)) revert Errors.InvalidEndTimestamp();

237:         uint256 boostDuration = endTimestamp - block.timestamp;

256:         uint256 slope = amount / boostDuration;
257:         uint256 bias = slope * boostDuration;

263:         uint256 totalDelegatedAmount = ((bias * boostDuration) + bias) / 2;
264:         // Then we can calculate the total amount of rewards for this Boost
265:         uint256 rewardAmount = (totalDelegatedAmount * pledgeParams.rewardPerVote) / UNIT;
266: 
267:         if(rewardAmount > pledgeAvailableRewardAmounts[pledgeId]) revert Errors.RewardsBalanceTooLow();
268:         pledgeAvailableRewardAmounts[pledgeId] -= rewardAmount;
269: 
270:         // Send the rewards to the user
271:         IERC20(pledgeParams.rewardToken).safeTransfer(user, rewardAmount);

```

# [NC] Typo

Protocol vs Protocal
```
File: WardenPledge.sol
70:     /** @notice ratio of fees to pay the protocol (in BPS) */
71:     uint256 public protocalFeeRatio = 250; //bps

328:         vars.feeAmount = (vars.totalRewardAmount * protocalFeeRatio) / MAX_PCT ;

388:         uint256 feeAmount = (totalRewardAmount * protocalFeeRatio) / MAX_PCT ;

433:         uint256 feeAmount = (totalRewardAmount * protocalFeeRatio) / MAX_PCT ;

627:         uint256 oldfee = protocalFeeRatio;
628:         protocalFeeRatio = newFee;
```

# [NC] Unfinished Comment for events

Events descriptions is not finished it will make the contract less prepared
```
/** @notice Event emitted when xx */
```

# [NC] NUMERIC VALUES HAVING TO DO WITH TIME SHOULD USE TIME UNITS FOR READABILITY

There are [units](https://docs.soliditylang.org/en/latest/units-and-global-variables.html#time-units) for seconds, minutes, hours, days, and weeks

```
File: WardenPledge.sol
24:     uint256 public constant WEEK = 7 * 86400;
```

# [NC] LINE WIDTH

Keep line width to max 120 characters for better readability.

