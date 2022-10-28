## [L001] - Unsafe ERC20 Operation(s)

```solidity

  475:
    WardenPledge.sol::475 => IERC20(pledgeParams.rewardToken).safeTransfer(receiver, remainingAmount);
  658:
    => IERC20(token).safeTransfer(owner(), amount);
  508:
    => IERC20(pledgeParams.rewardToken).safeTransfer(receiver, remainingAmount);
  475:
    => IERC20(pledgeParams.rewardToken).safeTransfer(receiver, remainingAmount);
  271:
    => IERC20(pledgeParams.rewardToken).safeTransfer(user, rewardAmount);
    ```
