## Missing checks for `address(0x0)` when assigning values to `address` state variables

Missing checks for zero-addresses may lead to infunctional protocol, if the variable addresses are updated incorrectly.

```
        votingEscrow = IVotingEscrow(_votingEscrow);
        delegationBoost = IBoostV2(_delegationBoost);

        chestAddress = _chestAddress;
```
>File: contracts/WardenPledge.sol [(line 137-140)](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L137-L140)

## `TYPOS`

```
    ///@audit taget
    * @param targetVotes Maximum taget of votes to have (own balacne + delegation) for the receiver

```
>File: contracts/WardenPledge.sol [(line 292)](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L292)
```
    ///@audit taget
    * @param targetVotes Maximum taget of votes to have (own balacne + delegation) for the receiver

```
>File: contracts/WardenPledge.sol [(line 292)](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L292)
```
    ///@audit ot
    * @param maxTotalRewardAmount Maximum total reward amount allowed ot be pulled by this contract

```
>File: contracts/WardenPledge.sol [(line 295)](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L295)
```
    ///@audit ot
    * @param maxFeeAmount Maximum feeamount allowed ot be pulled by this contract

```
>File: contracts/WardenPledge.sol [(line 296)](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L296)
```
    ///@audit ot
    * @param maxTotalRewardAmount Maximum added total reward amount allowed ot be pulled by this contract

```
>File: contracts/WardenPledge.sol [(line 411)](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L411)
```
    ///@audit ot
    * @param maxTotalRewardAmount Maximum added total reward amount allowed ot be pulled by this contract

```
>File: contracts/WardenPledge.sol [(line 365)](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L365)
```
    ///@audit ot
    * @param maxFeeAmount Maximum fee amount allowed ot be pulled by this contract

```
>File: contracts/WardenPledge.sol [(line 366)](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L366)
```
    ///@audit ot
    * @param maxFeeAmount Maximum fee amount allowed ot be pulled by this contract

```
>File: contracts/WardenPledge.sol [(line 412)](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L412)
```
    ///@audit fo
    * @param pledgeId ID fo the Pledge

```
>File: contracts/WardenPledge.sol [(line 453)](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L453)
```
    ///@audit fo
    * @param pledgeId ID fo the Pledge to close

```
>File: contracts/WardenPledge.sol [(line 485)](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L485)
```
    ///@audit Minmum
    * @param minRewardPerSecond Minmum amount of reward per vote per second for the token

```
>File: contracts/WardenPledge.sol [(line 523)](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L523)
```
    ///@audit Minmum
    * @param minRewardsPerSecond Minmum amount of reward per vote per second for each token in the list

```
>File: contracts/WardenPledge.sol [(line 539)](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L539)
```
    ///@audit Minmum
    * @param minRewardPerSecond Minmum amount of reward per vote per second for the token

```
>File: contracts/WardenPledge.sol [(line 558)](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L558)
```
    ///@audit Minmum
    * @param minRewardPerSecond Minmum amount of reward per vote per second for the token

```
>File: contracts/WardenPledge.sol [(line 568)](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L568)

## Use of `block.timestamp`

Block timestamps have historically been used for a variety of applications, such as entropy for random numbers, locking funds for periods of time, and various state-changing conditional statements that are time-dependent. Miners have the ability to adjust timestamps slightly, which can prove to be dangerous if block timestamps are used incorrectly in smart contracts.

```
        if(pledgeParams.endTimestamp <= block.timestamp) revert Errors.ExpiredPledge();
```
>File: contracts/WardenPledge.sol [(line 229)](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L229)
```
        uint256 boostDuration = endTimestamp - block.timestamp;
```
>File: contracts/WardenPledge.sol [(line 237)](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L237)
```
        vars.duration = endTimestamp - block.timestamp;
```
>File: contracts/WardenPledge.sol [(line 319)](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L319)
```
        if(pledgeParams.endTimestamp <= block.timestamp) revert Errors.ExpiredPledge();
```
>File: contracts/WardenPledge.sol [(line 380)](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L380)
```
        if(pledgeParams.endTimestamp <= block.timestamp) revert Errors.ExpiredPledge();
```
>File: contracts/WardenPledge.sol [(line 426)](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L426)
```
        uint256 remainingDuration = pledgeParams.endTimestamp - block.timestamp;
```
>File: contracts/WardenPledge.sol [(line 430)](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L430)
```
        if(pledgeParams.endTimestamp > block.timestamp) revert Errors.PledgeNotExpired();
```
>File: contracts/WardenPledge.sol [(line 463)](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L463)
```
        if(pledgeParams.endTimestamp <= block.timestamp) revert Errors.ExpiredPledge();
```
>File: contracts/WardenPledge.sol [(line 496)](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L496)

## Event is missing `indexed` fields

Each event should use three indexed fields if there are three or more fields.

```
    event NewPledge(
        address creator,
        address receiver,
        address rewardToken,
        uint256 targetVotes,
        uint256 rewardPerVote,
        uint256 endTimestamp
```
>File: contracts/WardenPledge.sol [(line 85-91)](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L85-L91)
```
    event ExtendPledgeDuration(uint256 indexed pledgeId, uint256 oldEndTimestamp, uint256 newEndTimestamp);
    /** @notice Event emitted when xx */
    event IncreasePledgeTargetVotes(uint256 indexed pledgeId, uint256 oldTargetVotes, uint256 newTargetVotes);
    /** @notice Event emitted when xx */
    event IncreasePledgeRewardPerVote(uint256 indexed pledgeId, uint256 oldRewardPerVote, uint256 newRewardPerVote);
```
>File: contracts/WardenPledge.sol [(line 94-98)](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L94_L98)
```
    event RetrievedPledgeRewards(uint256 indexed pledgeId, address receiver, uint256 amount);

    /** @notice Event emitted when xx */
    event Pledged(uint256 indexed pledgeId, address indexed user, uint256 amount, uint256 endTimestamp);
```
>File: contracts/WardenPledge.sol [(line 102-105)](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L102_L105)

## Upgradeable contract is missing a `__gap[50]` storage variable to allow for new storage variables in later versions

While some contracts may not currently be sub-classed, adding the variable now protects against forgetting to add it in the future.

```
    contract WardenPledge is Ownable, Pausable, ReentrancyGuard {
```
>File: contracts/WardenPledge.sol [(line 18)](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L18)

## `Check , effect and interact pattern` should be followed while making an external call
```
        IERC20(pledgeParams.rewardToken).safeTransferFrom(creator, address(this), totalRewardAmount);
        // And transfer the fees from the Pledge creator to the Chest contract
        IERC20(pledgeParams.rewardToken).safeTransferFrom(creator, chestAddress, feeAmount);

        // Update the Pledge parameters in storage
        pledgeParams.endTimestamp = safe64(newEndTimestamp);

        pledgeAvailableRewardAmounts[pledgeId] += totalRewardAmount;
```
>File: contracts/WardenPledge.sol [(line 394-401)](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L394_L401)

