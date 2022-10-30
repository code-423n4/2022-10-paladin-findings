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

