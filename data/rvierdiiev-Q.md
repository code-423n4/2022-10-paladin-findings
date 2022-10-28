### 1. Use solidity variable instead of calculations
No need to calculate how much is 1 week in seconds. You can use solidity `1 weeks` variable instead. `uint256 public constant WEEK = 1 weeks;` 
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L24
### 2. Use constant instead of magic number
Use constant instead of magic number `500` inside `updatePlatformFee` function.
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L626
### 3. `WardenPledge` can be initialized with 0 `chest` address which leads to unpredictable behaviour of protocol.
In constructor of `WardenPledge` there is [no check]( https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L140) that chest address is not 0. However [it is]( https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L600) in setter.
`chest` address is used in different places of code to transfer protocol fees to it using `IERC20(rewardToken).safeTransferFrom(creator, chestAddress, vars.feeAmount);`.
If `chest` value was initialized as 0 using constructor then the behaviour of protocol will depend on reward token.
Different reward tokens will give different results. Some will revert, because of sending to 0 address, while someone will still work.
As the result, in one option protocol will be working with reward token, but will burn protocol fees and in another option, protocol will revert when sending fees.

Make sure that `chest != address(0)` in constructor.
