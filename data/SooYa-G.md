## Add unchecked {} for subtractions where the operands cannot underflow because of a previous `require()` or `if`-statement
since checking for underflow is already done in previous `require()` or `if`-statement, unchecked the subtractions saves gas for the operation.
```require(a <= b); x = b - a``` => ```require(a <= b); unchecked { x = b - a }``` 
There are some instances for this issue:
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L319
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L385
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L431


## `<X> = <X> + <Y>` is cheaper than `<X> += <Y>`
For operation like addition or subtraction in state variable, `<X> = <X> + <Y>` is more efficient than `<X> += <Y>`. There are some instances for this issue :
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L268
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L401
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L445


## Duplicated check for user should be refactored to a modifier or function
By refactored the `require()`/`revert()` to a modifier or function saves deployment gas. 
There are 4 checks for `creator` in this contract :

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L376
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L422
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L459
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L491


I suggest to refactored the code below to a modifier or function to saves deployment gas.
```if(msg.sender != creator) revert Errors.NotPledgeCreator();```


## Redundant `revert()` check in `addMultipleRewardToken()` function
In the `addMultipleRewardToken()` function, `minRewardsPerSecond.length` should not be zero. So if length is 0, the second `revert()` is enough, no need to check using first `revert()`, it saves gas. So its better to remove the first `revert()` :
```if (length == 0) revert error.EmptyArray();```
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L544-L545