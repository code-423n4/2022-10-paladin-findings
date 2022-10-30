## Use of `Block.timestamp`
Block timestamps have historically been used for a variety of applications, such as entropy for random numbers (see the Entropy Illusion for further details), locking funds for periods of time, and various state-changing conditional statements that are time-dependent. Miners have the ability to adjust timestamps slightly, which can prove to be dangerous if block timestamps are used incorrectly in smart contracts.
```solidity
WardenPledge.sol:229:        if(pledgeParams.endTimestamp <= block.timestamp) revert Errors.ExpiredPledge();
WardenPledge.sol:237:        uint256 boostDuration = endTimestamp - block.timestamp;
WardenPledge.sol:319:        vars.duration = endTimestamp - block.timestamp;
WardenPledge.sol:380:        if(pledgeParams.endTimestamp <= block.timestamp) revert Errors.ExpiredPledge();
WardenPledge.sol:426:        if(pledgeParams.endTimestamp <= block.timestamp) revert Errors.ExpiredPledge();
WardenPledge.sol:430:        uint256 remainingDuration = pledgeParams.endTimestamp - block.timestamp;
WardenPledge.sol:463:        if(pledgeParams.endTimestamp > block.timestamp) revert Errors.PledgeNotExpired();
WardenPledge.sol:496:        if(pledgeParams.endTimestamp <= block.timestamp) revert Errors.ExpiredPledge();
```
## Use Weeks time unit as `1 weeks` instned of  7 * 8640
```solidity
WardenPledge.sol:24:    uint256 public constant WEEK = 7 * 86400;
```
## Use `allowlist/denylist` rather than `blacklist/whitelist`
```solidity
WardenPledge.sol:66:    // Also used to whitelist the tokens for rewards
WardenPledge.sol:312:        if(minAmountRewardToken[rewardToken] == 0) revert Errors.TokenNotWhitelisted();
WardenPledge.sol:521:    * @dev Adds a given reward token to the whitelist
WardenPledge.sol:536:    * @notice Adds a given reward token to the whitelist
WardenPledge.sol:537:    * @dev Adds a given reward token to the whitelist
WardenPledge.sol:555:    * @notice Adds a given reward token to the whitelist
WardenPledge.sol:556:    * @dev Adds a given reward token to the whitelist
WardenPledge.sol:581:    * @notice Removes a reward token from the whitelist
WardenPledge.sol:582:    * @dev Removes a reward token from the whitelist
```


