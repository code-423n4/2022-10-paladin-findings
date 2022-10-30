# [N-01] `Pledged` event is emitted with a wrong amount of delegated votes
## Targets
- https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L273
## Proof of Concept
The actual amount of delegated votes is `bias`, not `amount` ([WardenPledge.sol#L255-L263](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L255-L263)):
```solidity
uint256 slope = amount / boostDuration;
uint256 bias = slope * boostDuration;

// Rewards are set in the Pledge as reward/veToken/sec
// To find the total amount of veToken delegated through the whole Boost duration
// based on the Boost bias & the Boost duration, to take in account that the delegated amount decreases
// each second of the Boost duration
uint256 totalDelegatedAmount = ((bias * boostDuration) + bias) / 2;
```
## Tools Used
Manual review
## Recommended Mitigation Steps
Consider using `bias` instead of `amount`.