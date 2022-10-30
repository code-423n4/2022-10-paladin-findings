## Use `delete` to reset variables

The `delete` keyword better communicates the intention of what you are trying to do.

For example:

File: `WardenPledge.sol` [Line 473](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L473)

```
pledgeAvailableRewardAmounts[pledgeId] = 0;
```

The above could use the `delete` keyword like so:

```
delete pledgeAvailableRewardAmounts[pledgeId] // resets to initial value, for uint256 that would be 0
```

Here are some more instances of this issue:

File: `WardenPledge.sol` [Line 506](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L506)
File: `WardenPledge.sol` [Line 589](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L589)

## Typos

File: `WardenPledge.sol`: Lines Affected: [71](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L71), [292](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L292), [295-296](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L295-L296), [339](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L339), [523](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L523), [539](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L539), [558](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L558), [568](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L568), [621-622](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L621-L622), [650](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L650)

```
      // Correction: Change "protocal" to "protocol"
71:   uint256 public protocalFeeRatio = 250;

      // Correction: Change "taget" to "target" and "balacne" to "balance"
292:  * @param targetVotes Maximum taget of votes to have (own balacne + delegation) for the receiver

      // Correction: Change "ot" to "to"
295:  * @param maxTotalRewardAmount Maximum total reward amount allowed ot be pulled by this contract

      // Correction: Change "feeamount" to "fee amount" or "feeAmount" and "ot" to "to"
296:  * @param maxFeeAmount Maximum feeamount allowed ot be pulled by this contract

      // Correction: Change "reards" to "rewards"
339:  // Add the total reards as available for the Pledge & write Pledge parameters in storage

      // Correction: Change "Minmum" to Minimum"
523:  * @param minRewardPerSecond Minmum amount of reward per vote per second for the token
539:  * @param minRewardsPerSecond Minmum amount of reward per vote per second for each token in the list
558:  * @param minRewardPerSecond Minmum amount of reward per vote per second for the token
568:  * @param minRewardPerSecond Minmum amount of reward per vote per second for the token

      // Correction: Change "Platfrom" to "Platform"
621:  * @notice Updates the Platfrom fees BPS ratio
622:  * @dev Updates the Platfrom fees BPS ratio

      // Correction: Change "tof" to "to" and "EC20" to "ERC20"
650:  * @param token Address tof the EC2O token
```

## Sanity checks on deployment

Zero address/value checks should be implemented at the constructor to avoid errors that can result in non-functional calls associated with them.

File: `WardenPledge.sol` [Line 131-143](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L131-L143)

Consider adding these zero address/value checks at the start of the constructor:

```
if (_votingEscrow == address(0)|| _delegationBoost == address(0) || _chestAddress == address(0)) revert Errors.ZeroAddress();
if(_minTargetVotes == 0) revert Errors.InvalidValue();
```
