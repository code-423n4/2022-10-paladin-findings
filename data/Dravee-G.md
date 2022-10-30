## 1. State variables only set in the constructor should be declared `immutable`

Variables only set in the constructor and never edited afterwards should be marked as immutable, as it would avoid the expensive storage-writing operation in the constructor (around **20 000 gas** per variable) and replace the expensive storage-reading operations (around **2100 gas** per reading) to a less expensive value reading (**3 gas**)

```solidity
contracts/WardenPledge.sol:
   60:     IVotingEscrow public votingEscrow;//@audit-issue GAS should be immutable
   62:     IBoostV2 public delegationBoost;//@audit-issue GAS should be immutable
   79:     uint256 public minDelegationTime = 1 weeks; //@audit-issue GAS should be immutable
```

## 2. Use `=` instead of `+=` when assigning a value for the 1st time

On the following, `mapping(uint256 => uint256) public pledgeAvailableRewardAmounts` is being set for the first time with the new key `vars.newPledgeID`:

```diff
File: WardenPledge.sol
337:         vars.newPledgeID = pledgesIndex();
338: 
339:         // Add the total reards as available for the Pledge & write Pledge parameters in storage
- 340:         pledgeAvailableRewardAmounts[vars.newPledgeID] += vars.totalRewardAmount; //@audit-issue GAS: it's a new var on a new ID
+ 340:         pledgeAvailableRewardAmounts[vars.newPledgeID] = vars.totalRewardAmount;
```

Therefore, `+=` should be replaced with `=` to avoid reading the storage variable `pledgeAvailableRewardAmounts[vars.newPledgeID]` which is always equal to `0`.
