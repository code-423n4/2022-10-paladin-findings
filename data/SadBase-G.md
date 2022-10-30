 ## Variable can be Constant as there is no change. It can be Private as Well

- Line 79

```solidity
uint256 public minDelegationTime = 1 weeks;
```

Since there is no change to `minDelegationTime`, it can be set as constant. With it being constant, it can be `private` as well to save gas.

## Conditions can be swapped to reduce Gas

2nd check would not be checked if the first one succeeded, reducing gas cost.

### Line 383 - extendPledge() function

```
if(newEndTimestamp != _getRoundedTimestamp(newEndTimestamp) || newEndTimestamp < oldEndTimestamp) revert Errors.InvalidEndTimestamp();
```