## [L-01] CALLING `extendPledge` FUNCTION WITH `newEndTimestamp` INPUT EQUALING `pledgeParams.endTimestamp` DOES NOT MAKE SENSE
Calling the following `extendPledge` function executes `if(newEndTimestamp != _getRoundedTimestamp(newEndTimestamp) || newEndTimestamp < oldEndTimestamp) revert Errors.InvalidEndTimestamp()`. Yet, although not extending the pledge duration at all, calling this function with the `newEndTimestamp` input equaling `pledgeParams.endTimestamp` does not revert. Moreover, this implementation is inconsistent with the `increasePledgeRewardPerVote` function's implementation, which executes `if(newRewardPerVote <= oldRewardPerVote) revert Errors.RewardsPerVotesTooLow()` where `oldRewardPerVote` is `pledgeParams.rewardPerVote`. Hence, for consistency and avoiding confusion, please consider updating the `extendPledge` function to execute `if(newEndTimestamp != _getRoundedTimestamp(newEndTimestamp) || newEndTimestamp <= oldEndTimestamp) revert Errors.InvalidEndTimestamp()`.

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L368-L404
```solidity
    function extendPledge(
        uint256 pledgeId,
        uint256 newEndTimestamp,
        uint256 maxTotalRewardAmount,
        uint256 maxFeeAmount
    ) external whenNotPaused nonReentrant {
        ...
        if(newEndTimestamp == 0) revert Errors.NullEndTimestamp();
        uint256 oldEndTimestamp = pledgeParams.endTimestamp;
        if(newEndTimestamp != _getRoundedTimestamp(newEndTimestamp) || newEndTimestamp < oldEndTimestamp) revert Errors.InvalidEndTimestamp();

        uint256 addedDuration = newEndTimestamp - oldEndTimestamp;
        ...
    }
```

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L414-L448
```solidity
    function increasePledgeRewardPerVote(
        uint256 pledgeId,
        uint256 newRewardPerVote,
        uint256 maxTotalRewardAmount,
        uint256 maxFeeAmount
    ) external whenNotPaused nonReentrant {
        ...

        uint256 oldRewardPerVote = pledgeParams.rewardPerVote;
        if(newRewardPerVote <= oldRewardPerVote) revert Errors.RewardsPerVotesTooLow();
        ...
    }
```

## [L-02] MISSING `address(0)` CHECKS FOR CRITICAL ADDRESS INPUTS
To prevent unintended behaviors, critical addresses inputs should be checked against `address(0)`. Please consider checking `_votingEscrow`, `_delegationBoost`, and `_chestAddress` in the following `constructor`.

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L131-L143
```solidity
    constructor(
        address _votingEscrow,
        address _delegationBoost,
        address _chestAddress,
        uint256 _minTargetVotes
    ) {
        votingEscrow = IVotingEscrow(_votingEscrow);
        delegationBoost = IBoostV2(_delegationBoost);

        chestAddress = _chestAddress;

        minTargetVotes = _minTargetVotes;
    }
```

## [L-03] INACCURATE COMMENT
Inaccurate comment can be misleading. The following `retrievePledgeRewards` function is for retrieving the non-distributed rewards from an expired pledge, not just a closed pledge. Calling this function for a pledge that is closed but not expired reverts. For better readability and maintainability, please consider updating this function's comment.

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L450-L480
```solidity
    /**
    * @notice Retrieves all non distributed rewards from a closed Pledge
    * @dev Retrieves all non distributed rewards from a closed Pledge & send them to the given receiver
    * @param pledgeId ID fo the Pledge
    * @param receiver Address to receive the remaining rewards
    */
    function retrievePledgeRewards(uint256 pledgeId, address receiver) external whenNotPaused nonReentrant {
        if(pledgeId >= pledgesIndex()) revert Errors.InvalidPledgeID();
        address creator = pledgeOwner[pledgeId];
        if(msg.sender != creator) revert Errors.NotPledgeCreator();
        if(receiver == address(0)) revert Errors.ZeroAddress();

        Pledge storage pledgeParams = pledges[pledgeId];
        if(pledgeParams.endTimestamp > block.timestamp) revert Errors.PledgeNotExpired();

        // Get the current remaining amount of rewards not distributed for the Pledge
        uint256 remainingAmount = pledgeAvailableRewardAmounts[pledgeId];

        // Set the Pledge as Closed
        if(!pledgeParams.closed) pledgeParams.closed = true;

        if(remainingAmount > 0) {
            // Transfer the non used rewards and reset storage
            pledgeAvailableRewardAmounts[pledgeId] = 0;

            IERC20(pledgeParams.rewardToken).safeTransfer(receiver, remainingAmount);

            emit RetrievedPledgeRewards(pledgeId, receiver, remainingAmount);

        }
    }
```

## [N-01] CONSTANT CAN BE USED INSTEAD OF MAGIC NUMBER
To improve readability and maintainability, a constant can be used instead of the magic number. Please consider replacing `500` in the following code with a constant.

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L625-L631
```solidity
    function updatePlatformFee(uint256 newFee) external onlyOwner {
        if(newFee > 500) revert Errors.InvalidValue();
        uint256 oldfee = protocalFeeRatio;
        protocalFeeRatio = newFee;

        emit PlatformFeeUpdated(oldfee, newFee);
    }
```

## [N-02] MISSING `indexed` EVENT FIELDS
Querying events can be optimized with indices. Please consider adding `indexed` to the relevant fields of the following events.

```solidity
contracts\WardenPledge.sol
  85-92:
    event NewPledge(
        address creator,
        address receiver,
        address rewardToken,
        uint256 targetVotes,
        uint256 rewardPerVote,
        uint256 endTimestamp
    );
  115: event ChestUpdated(address oldChest, address newChest);
  117: event PlatformFeeUpdated(uint256 oldfee, uint256 newFee);
  119: event MinTargetUpdated(uint256 oldMinTarget, uint256 newMinTargetVotes);
```

## [N-03] MISSING NATSPEC COMMENT
NatSpec comment provides rich code documentation. NatSpec comment is missing for the following function. Please consider adding it.

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L665-L668
```solidity
    function safe64(uint256 n) internal pure returns (uint64) {
        if(n > type(uint64).max) revert Errors.NumberExceed64Bits();
        return uint64(n);
    }
```