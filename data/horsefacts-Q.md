# QA

## Low

### Pledges do not support fee on transfer tokens

The total amount required to fund a pledge is calculated at pledge creation time, then transferred to the `WardenPledge` contract:

[`createPledge`](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L327-L333)

```solidity
        vars.totalRewardAmount = (rewardPerVote * vars.votesDifference * vars.duration) / UNIT;
        vars.feeAmount = (vars.totalRewardAmount * protocalFeeRatio) / MAX_PCT ;
        if(vars.totalRewardAmount > maxTotalRewardAmount) revert Errors.IncorrectMaxTotalRewardAmount();
        if(vars.feeAmount > maxFeeAmount) revert Errors.IncorrectMaxFeeAmount();

        // Pull all the rewards in this contract
        IERC20(rewardToken).safeTransferFrom(creator, address(this), vars.totalRewardAmount);
```

The total reward amount is then stored, along with the reward rate inside the `Pledge` struct:

[`createPledge`](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L339-L350)

```solidity
        // Add the total reards as available for the Pledge & write Pledge parameters in storage
        pledgeAvailableRewardAmounts[vars.newPledgeID] += vars.totalRewardAmount;

        pledges.push(Pledge(
            targetVotes,
            vars.votesDifference,
            rewardPerVote,
            rewardToken,
            safe64(endTimestamp),
            false,
            receiver
        ));
```

However, if a pledge is created with a fee-on-transfer token, an amount less than the full calculated reward amount will be transferred to the `WardenPledge` contract when the pledge is created, but the full amount will be stored for reward calculations. The contract may show that there are sufficient rewards remaining for additional pledges, but revert when a pledger actually attempts to make the pledge due to an insufficient balance.

I consider this low risk, since all rewards tokens must be approved before use, and it is unlikely that rewards would be paid in a fee-on-transfer token, but ensure you qualify all rewards tokens before approval.

## QA

### Use date units

The constant `WEEK` can be defined using time unit keywords, either `7 days` or `1 weeks`. (These keywords are used elsewhere in the contract).

[`WardenPledge.sol`](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L24):

```solidity
    uint256 public constant WEEK = 7 * 86400;
```

```solidity
    uint256 public constant WEEK = 1 weeks;
```

### Incomplete comment

There is an incomplete comment on line 84 of `WardenPledge`:

[`WardenPledge#L84`](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L84)

```solidity
    /** @notice Event emitted when xx */
```

### Index `NewPledge` parameters

Consider indexing the address parameters of `NewPledge`, to allow filtering events by `creator`/`receiver`/`rewardToken` address:

[`WardenPledge#L85`](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L85-L92):

```solidity
    event NewPledge(
        address creator,
        address receiver,
        address rewardToken,
        uint256 targetVotes,
        uint256 rewardPerVote,
        uint256 endTimestamp
    );
```

### Misspelling

"Protocol fee ratio" is misspelled as "protocal fee ratio":

[`WardenPledge#L627`](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L627-L629)

```solidity
       uint256 oldfee = protocalFeeRatio;
        protocalFeeRatio = newFee;

```