# Gas Report

- [G-01: `_pledge()` shouldn't verify user's balance](#g-01-_pledge-shouldnt-verify-users-balance)
- [G-02: trigger underflow instead of explicitly checking for it](#g-02-trigger-underflow-instead-of-explicitly-checking-for-it)
- [G-03: `retrievePledgeRewards()` and `closePledge()` can be merged to reduce deployment gas](#g-03-retrievepledgerewards-and-closepledge-can-be-merged-to-reduce-deployment-gas)

## G-01: `_pledge()` shouldn't verify user's balance

In the `_pledge()` function you check whether the user has enough boost delegation & set the correct allowance. The same checks are already done in `BoostV2.boost()`. You're wasting gas by repeating the same checks in your contract. If the user doesn't fulfill the necessary requirements, the call will simply fail.

- https://github.com/curvefi/curve-veBoost/blob/db3dec43b6e4fac0fca1f01509f9133563f43ebb/contracts/BoostV2.vy#L189
- https://github.com/curvefi/curve-veBoost/blob/db3dec43b6e4fac0fca1f01509f9133563f43ebb/contracts/BoostV2.vy#L223
- https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L241-L242

## G-02: trigger underflow instead of explicitly checking for it

By adding an if-clause to prevent underflows you're increasing the gas costs of the happy path. Just let the value underflow and trigger a revert that way. Most wallets prevent txs that revert from being executed. Thus, you're wasting gas for no reason.

- https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L267

## G-03: `retrievePledgeRewards()` and `closePledge()` can be merged to reduce deployment gas

Both functions implement the same thing. One is callable before the pledge ended and the other one after. The effect of both is the same (retrieval of funds). You can merge them into a single one to reduce deployment gas.

- https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L456-L515
