Overall, the codebase was quite well-written, with clear intentions for gas optimizations found in e.g. unchecked for loops. The C4udit tool also points out a lot of impactful gas optimizations. This report points out some other possible gas savings for the contract.

|  | Title | Instances | Gas saved |
|---|---|:---:|---|
| [G-01] | Several variables can be made `constant` and `immutable` | 3 | 17090 deployment, 2000 per operations |
| [G-02] | `unchecked` applicable for arithmetic operations that cannot overflow/underflow | 2 | approx. 150 per operations |
| [G-03] | `protocalFeeRatio` can be better packed | 1 | -18698 deployment, 2000 per operations |

## [G-01] Several variables can be made `constant` and `immutable`

The following value can be made `constant`: 

```solidity=
uint256 public minDelegationTime = 1 weeks;
```

- https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L79

The following two values can be made `immutable`:

```solidity=
IVotingEscrow public votingEscrow;
IBoostV2 public delegationBoost;
```

- https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L60
- https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L62

Applying all the aforementioned reduces deployment gas from `2695825` down to `2678735` (for `17090` gas in total). 

Furthermore, since constants and immutables are stored in the contract bytecode rather than storage, this also saves around `2000` storage-reading gas for every operations that require these values.

## [G-02] `unchecked` applicable for arithmetic operations that cannot overflow/underflow

Two instances are found:

- https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L182
    - Obviously cannot overflow/underflow. Can change to:
```solidity
unchecked {return (timestamp / WEEK) * WEEK;}
```
- https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L268
    - The condition check one line before ensures cannot underflow. Thus can be changed to:
```solidity
unchecked{pledgeAvailableRewardAmounts[pledgeId] -= rewardAmount;}
```

Saves approx. 150 operational gas on re-running tests with these modifications applied, also saves minor deployment gas.

## [G-03] `protocalFeeRatio` can be better packed

(For the sake of reporting, we ignore the typo)

https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L71

The storage variable `protocalFeeRatio` can be changed to type `uint96`, since the fee ratio cannot exceed 500. Then the storage slot looks like follow:

```solidity=
// this is one slot
uint96 public protocalFeeRatio = 250;
address public chestAddress;
```

`updatePlatformFee()` can be easily modified accordingly.

Re-running tests with this optimization increases deployment gas by `18698` (up to `2714523`). However, all pledging operations (except for `closePledge()`) saves approx. `2000` gas, for having reduced one storage slot to look at. Hence this optimization is impactful in the long run.
