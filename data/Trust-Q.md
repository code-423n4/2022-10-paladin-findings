# Issues

## 1. Missing event in retrievePledgeRewards

retrievePledgeRewards can only be called on expired rewards. The issue is that if there are no remaining rewards, no event is emitted even though an important state has changed - the pledge is closed. Fix by emitting a ClosePledge similarly to `closePledge` function.

```
// Set the Pledge as Closed
if(!pledgeParams.closed) pledgeParams.closed = true;
if(remainingAmount > 0) {
    // Transfer the non used rewards and reset storage
    pledgeAvailableRewardAmounts[pledgeId] = 0;
    IERC20(pledgeParams.rewardToken).safeTransfer(receiver, remainingAmount);
    emit RetrievedPledgeRewards(pledgeId, receiver, remainingAmount);
}
```

## 2. pledgePercent converts % to amount using veCRV balance, but \_pledge checks amount is above "delegable_balance"

pledgePercent contains this code:
```
uint256 amount = (votingEscrow.balanceOf(msg.sender) * percent) / MAX_PCT;
_pledge(pledgeId, msg.sender, amount, endTimestamp);
```

While  \_pledge has this code:
```
if(delegationBoost.delegable_balance(user) < amount) revert Errors.CannotDelegate();
```

In the exported function the % is amount of actual veCRV balance. But \_pledge checks the amount is over delegable_balance. This is veBalance + received delegations - sent delegations. 

This creates confusion for the user:
1. If their delegable_balance is lower than veCRV balance, the pledge will not succeed.
2. If user intends the % to be from delegable balance, and delegable balance is lower than than veCRV balance, the amount calculated will be higher than user's expectation.
