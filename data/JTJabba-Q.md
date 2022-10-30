## Low Risk

## [L-01] Pledges with unwhitelisted tokens can be extended/increased
https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L368-L404

https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L414-L448

The owner of a WardenPledge contract is able to whitelist and unwhitelist reward tokens. New pledges are not able to be created with unwhitelisted reward tokens, but existing pledges using reward tokens that have been unwhitelisted can be extended, or their rewards per vote can be increased causing the pledge to be farther incentivized.

The same check used in `createPledge` to revert if the pledge's reward token is not whitelisted should be added to `extendPledge` and `increasePledgeRewardPerVote`.

[/contracts/WardenPledge.sol#L312](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L312):
```solidity
312        if(minAmountRewardToken[rewardToken] == 0) revert Errors.TokenNotWhitelisted();
```
## Non-Critical

## [N-01] Missing functionality to extend a pledge's `targetVotes`
https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L96

An event for a pledge's target votes being increased is present, but is not referenced and no functionality for increasing a pledge's target votes is present.

[/contracts/WardenPledge.sol#L96](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L96):
```solidity
96    event IncreasePledgeTargetVotes(uint256 indexed pledgeId, uint256 oldTargetVotes, uint256 newTargetVotes);
```

Functionality should be added for increasing a pledge's target votes that makes use of the `IncreasePledgeTargetVotes` event, or the event should be removed.

## [N-02] Events have unfinished documentation
https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L84-119

All events defined have placeholders for documentation.

[/contracts/WardenPledge.sol#L84-L96](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L84-L96):
```solidity
84    /** @notice Event emitted when xx */
85    event NewPledge(
86        address creator,
87        address receiver,
88        address rewardToken,
89        uint256 targetVotes,
90        uint256 rewardPerVote,
91        uint256 endTimestamp
92    );
93    /** @notice Event emitted when xx */
94    event ExtendPledgeDuration(uint256 indexed pledgeId, uint256 oldEndTimestamp, uint256 newEndTimestamp);
95    /** @notice Event emitted when xx */
96    event IncreasePledgeTargetVotes(uint256 indexed pledgeId, uint256 oldTargetVotes, uint256 newTargetVotes);

// event definitions continue...
```

## [N-03] Typos

[/contracts/WardenPledge.sol](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol):
```solidity
// @audit taget
292    * @param targetVotes Maximum taget of votes to have (own balacne + delegation) for the receiver

// @audit ot
295    * @param maxTotalRewardAmount Maximum total reward amount allowed ot be pulled by this contract
296    * @param maxFeeAmount Maximum feeamount allowed ot be pulled by this contract
365    * @param maxTotalRewardAmount Maximum added total reward amount allowed ot be pulled by this contract
366    * @param maxFeeAmount Maximum fee amount allowed ot be pulled by this contract
411    * @param maxTotalRewardAmount Maximum added total reward amount allowed ot be pulled by this contract
412    * @param maxFeeAmount Maximum fee amount allowed ot be pulled by this contract

// @audit reards
339        // Add the total reards as available for the Pledge & write Pledge parameters in storage

// @audit fo
453    * @param pledgeId ID fo the Pledge
485* @param pledgeId ID fo the Pledge to close

// @audit Minmum
523    * @param minRewardPerSecond Minmum amount of reward per vote per second for the token
539    * @param minRewardsPerSecond Minmum amount of reward per vote per second for each token in the list
558    * @param minRewardPerSecond Minmum amount of reward per vote per second for the token
568    * @param minRewardPerSecond Minmum amount of reward per vote per second for the token

// @audit Platfrom
621    * @notice Updates the Platfrom fees BPS ratio
622    * @dev Updates the Platfrom fees BPS ratio

// @audit tof
650    * @param token Address tof the EC2O token
```