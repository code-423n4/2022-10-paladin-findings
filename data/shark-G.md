## > costs less gas than >= (same for <, <=)

The reason `>` and `<` costs less gas is because in the EVM, there is no opcode for `>=` or `<=`

Here is an example:

File: `WardenPledge.sol` [Line 223](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L223)

```
if(pledgeId >= pledgesIndex()) revert Errors.InvalidPledgeID();
```

The above could be replaced to the following:

```
if(pledgeId > pledgesIndex() - 1) revert Errors.InvalidPledgeID();
```

Here are some more examples of this issue:
File: `WardenPledge.sol` [Line 229](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L229)
File: `WardenPledge.sol` [Line 374](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L374)
File: `WardenPledge.sol` [Line 420](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L420)
File: `WardenPledge.sol` [Line 457](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L457)

## `x += y` costs more gas than `x = x + y`

Here is an example:
File: `WardenPledge.sol` [Line 340](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L340)

```
pledgeAvailableRewardAmounts[vars.newPledgeID] += vars.totalRewardAmount;
```

The above should be changed to:

```
pledgeAvailableRewardAmounts[vars.newPledgeID] = pledgeAvailableRewardAmounts[vars.newPledgeID] + vars.totalRewardAmount;
```

Here are the rest of the instances:
File: `WardenPledge.sol` [Line 268](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L268)
File: `WardenPledge.sol` [Line 401](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L401)
File: `WardenPledge.sol` [Line 445](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L445)

## Function Order Affects Gas Consumption

The order of the functions will have an impact on gas consumption. The reason that this is the case is because in smart contracts, there's a difference in the order of the functions. Each position will use up an extra 22 gas. The order is dependent on the Method ID.

For more info: https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92

## Unused event

The event `IncreasePledgeTargetVotes()` is not emitting anything in `WardenPledge.sol`. Consider either removing it from the contract or making use of it.

File: `WardenPledge.sol` [Line 96](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L96)
