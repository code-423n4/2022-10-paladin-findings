Just passing along two small findings in this report.

### Declare variables immutable

The `votingEscrow` and `delegationBoost` state variables are set once at construction time and cannot change, so you can declare them `immutable`:

[`WardenPledge#L59`](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L59-L63)

```solidity
    /** @notice Address of the votingToken to delegate */
    IVotingEscrow public votingEscrow;
    /** @notice Address of the Delegation Boost contract */
    IBoostV2 public delegationBoost;
```

Suggestion:

```solidity
    /** @notice Address of the votingToken to delegate */
    IVotingEscrow public immutable votingEscrow;
    /** @notice Address of the Delegation Boost contract */
    IBoostV2 public immutable delegationBoost;
```

### Use unchecked subtraction

Since the `if` statement on line 267 ensures that `rewardAmount` is greater than `pledgeAvailableRewardAmounts[pledgeId]`,
you may safely perform an unchecked subtraction:

[`_pledge`](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L267-L268)

```solidity
       if(rewardAmount > pledgeAvailableRewardAmounts[pledgeId]) revert Errors.RewardsBalanceTooLow();
       pledgeAvailableRewardAmounts[pledgeId] -= rewardAmount;
```

Suggested:

```solidity
       if(rewardAmount > pledgeAvailableRewardAmounts[pledgeId]) revert Errors.RewardsBalanceTooLow();
       unchecked { pledgeAvailableRewardAmounts[pledgeId] -= rewardAmount; }
```
