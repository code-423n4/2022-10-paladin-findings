## [L-01] Use of Block.timestamp

Block timestamps have historically been used for a variety of applications, such as entropy for random numbers (see the Entropy Illusion for further details), locking funds for periods of time, and various state-changing conditional statements that are time-dependent. Miners have the ability to adjust timestamps slightly, which can prove to be dangerous if block timestamps are used incorrectly in smart contracts.

```
WardenPledge.sol::229 => if(pledgeParams.endTimestamp <= block.timestamp) revert Errors.ExpiredPledge();
WardenPledge.sol::237 => uint256 boostDuration = endTimestamp - block.timestamp;
WardenPledge.sol::319 => vars.duration = endTimestamp - block.timestamp;
WardenPledge.sol::380 => if(pledgeParams.endTimestamp <= block.timestamp) revert Errors.ExpiredPledge();
WardenPledge.sol::426 => if(pledgeParams.endTimestamp <= block.timestamp) revert Errors.ExpiredPledge();
WardenPledge.sol::430 => uint256 remainingDuration = pledgeParams.endTimestamp - block.timestamp;
WardenPledge.sol::463 => if(pledgeParams.endTimestamp > block.timestamp) revert Errors.PledgeNotExpired();
WardenPledge.sol::496 => if(pledgeParams.endTimestamp <= block.timestamp) revert Errors.ExpiredPledge();
```

## [L-02] Missing Zero address checks

Zero-address checks are a best practice for input validation of critical address parameters. While the codebase applies this to most cases, there are many places where this is missing in constructors and setters.
Impact: Accidental use of zero-addresses may result in exceptions, burn fees/tokens, or force redeployment of contracts.

```
WardenPledge.sol::140 => chestAddress = _chestAddress;
```

## [N-01] Use a more recent version of solidity

Use a solidity version of at least 0.8.4 to get bytes.concat() instead of abi.encodePacked(<bytes>,<bytes>)
Use a solidity version of at least 0.8.12 to get string.concat() instead of abi.encodePacked(<str>,<str>)
Use a solidity version of at least 0.8.13 to get the ability to use using for with a list of free functions

```
WardenPledge.sol::2 => pragma solidity 0.8.10;
```

## [N-02] Large multiples of ten should use scientific notation

Use (e.g. 1e6) rather than decimal literals (e.g. 1000000), for better code readability

```
WardenPledge.sol::23 => uint256 public constant MAX_PCT = 10000;
```

## [N-03] Event is missing indexed fields

Each event should use three indexed fields if there are three or more fields

```
WardenPledge.sol::115 => event ChestUpdated(address oldChest, address newChest);
WardenPledge.sol::117 => event PlatformFeeUpdated(uint256 oldfee, uint256 newFee);
WardenPledge.sol::119 => event MinTargetUpdated(uint256 oldMinTarget, uint256 newMinTargetVotes);
```

## [N-04] Constants should be defined rather than using magic numbers

It is bad practice to use numbers directly in code without explanation

```
WardenPledge.sol::23 => uint256 public constant MAX_PCT = 10000;
WardenPledge.sol::24 => uint256 public constant WEEK = 7 * 86400;
WardenPledge.sol::71 => uint256 public protocalFeeRatio = 250; //bps
WardenPledge.sol::626 => if(newFee > 500) revert Errors.InvalidValue();
```
