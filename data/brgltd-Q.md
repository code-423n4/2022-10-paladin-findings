# [01] Some tokens will revert for transfers of zero amount and this can affect `createPledge()` functionality

## Impact

Some tokens will [revert](https://github.com/d-xo/weird-erc20#revert-on-zero-value-transfers) on transfers for zero amount. If a token with this behavior is used `WardenPledge`, and `protocolFeeRatio` gets setted to zero, calls to `createPledge()` will revert.

## Proof of Concept

There's no validation preventing `protocolFeeRatio` to be set to zero.

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L625-L631

If `protocolFeeRatio` gets set to zero, `vars.feeAmount` will be zero.

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L328

The transfer on L335 will revert.

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L335

## Recommended Mitgation Steps

Prevent `protocolFeeRatio` from receiving the value zero or add a conditional for `IERC20(rewardToken).safeTransferFrom(creator, chestAddress, vars.feeAmount);` to only be called if `vars.feeAmount` is not zero.

# [02] Missing zero address checks for constructor

If a variable gets configured with address zero, failure to immediately reset the value can result in unexpected behavior for the project.

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L131-L143

# [03] Use the latest version of solidity

`WardenPledge` is using v0.8.10. Consider using the latest stable version of solidity to ensure the compiler contains the latest security fixes.

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L2

# [04] Lack of checks-effects-interactions

The functions `createPledge()`, `extendPledge()` and `increasePledgeRewardPerVote()` are updating state variables after external transfer calls. Consider update the state variables before the external calls to follow the checks-effects-interactions pattern.

# [05] State variable can be a constant

The state variable `minDelegationTime` can be declared as a constant.

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L79

# [06] Order of functions

The solidity [documentation](https://docs.soliditylang.org/en/v0.8.17/style-guide.html#order-of-functions) recommends the following order for functions:

constructor
receive function (if exists)
fallback function (if exists)
external
public
internal
private

Consider adopting the following strategy on the WardenPledge contract.

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L181

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L195

# [07] Add a limit for the maximum number of characters per line

The solidity [documentation](https://docs.soliditylang.org/en/v0.8.17/style-guide.html#maximum-line-length) recommends a maximum of 120 characters.

Consider adding a limit of 120 characters or less to prevent large lines.

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L234
