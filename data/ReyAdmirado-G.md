# Gas


| | issue |
| ----------- | ----------- |
| 1 | [State variables only set in the constructor should be declared immutable](#1-state-variables-only-set-in-the-constructor-should-be-declared-immutable) |
| 2 | [state variable should be declared as constant which saves lots of gas](#2-state-variable-should-be-declared-as-constant-which-saves-lots-of-gas) |
| 3 | [state variables should be cached in stack variables rather than re-reading them from storage](#3-state-variables-should-be-cached-in-stack-variables-rather-than-re-reading-them-from-storage) |
| 4 | [Stack variable used as a cheaper cache for a state variable is only used once](#4-stack-variable-used-as-a-cheaper-cache-for-a-state-variable-is-only-used-once) |
| 5 | [Add `unchecked {}` for subtractions where the operands cannot underflow because of a previous `if` statement](#5-add-unchecked--for-subtractions-where-the-operands-cannot-underflow-because-of-a-previous-if-statement) |
| 6 | [`<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables](#6-x--y-costs-more-gas-than-x--x--y-for-state-variables) |
| 7 | [Avoid a function use by swapping two sides of `or operation` in if statement](#7-avoid-a-function-use-by-swapping-two-sides-of--in-if-statement) |
| 8 | [use a more recent version of solidity](#8-use-a-more-recent-version-of-solidity) |
| 9 | [usage of uint/int smaller than 32 bytes (256 bits) incurs overhead](#9-usage-of-uintint-smaller-than-32-bytes-256-bits-incurs-overhead) |


## 1. State variables only set in the constructor should be declared immutable.

Avoids a Gsset (20000 gas) in the constructor, and replaces each Gwarmaccess (100 gas) with a PUSH32 (3 gas).

`votingEscrow`
- [WardenPledge.sol#L60](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L60)

`delegationBoost`
- [WardenPledge.sol#L62](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L62)


## 2. state variable should be declared as constant which saves lots of gas

- [WardenPledge.sol#L79](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L79)


## 3. state variables should be cached in stack variables rather than re-reading them from storage

The instances below point to the second+ access of a state variable within a function. Caching of a state variable replace each Gwarmaccess (100 gas) with a much cheaper stack read. Other less obvious fixes/optimizations include having local memory caches of state variable structs, or having local caches of state variable contracts/addresses. 

mostly the if statements gonna pass, so its better to cache `minAmountRewardToken[rewardToken]` at this line
- [WardenPledge.sol#L312](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L312)

consider caching `pledgeParams.endTimestamp` earlier before this line because the if statement mostly gonna pass
- [WardenPledge.sol#L380](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L380)

`pledgeParams.rewardToken`
- [WardenPledge.sol#L394](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L394)

same here
- [WardenPledge.sol#L438](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L438)


## 4. Stack variable used as a cheaper cache for a state variable is only used once

If the variable is only accessed once, it’s cheaper to use the state variable directly that one time, and save the 3 gas the extra stack assignment would spend

`amount`
- [WardenPledge.sol#L209](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L209)

`slope`
- [WardenPledge.sol#L256](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L256)

`totalDelegatedAmount`
- [WardenPledge.sol#L263](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L263)

`remainingDuration`
- [WardenPledge.sol#L430](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L430)

`rewardPerVoteDiff`
- [WardenPledge.sol#L431](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L431)

`creator`
- [WardenPledge.sol#L458](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L458)
- [WardenPledge.sol#L490](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L490)


## 5. Add `unchecked {}` for subtractions where the operands cannot underflow because of a previous `if` statement

if(a <= b); x = b - a => if(a <= b); unchecked { x = b - a }
this will stop the check for overflow and underflow so it will save gas

endTimestamp is bigger than block.timestamp because its checked in line 229. (endTimestamp got pledgeParams.endTimestamp value in line 233):
- [WardenPledge.sol#L237](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L237)

checked in line 383
- [WardenPledge.sol#L385](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L385)

checked in line 426
- [WardenPledge.sol#L430](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L430)

checked in line 429
- [WardenPledge.sol#L431](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L431)


## 6. `<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables
Using the addition operator instead of plus-equals saves gas

- [WardenPledge.sol#L268](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L268)

- [WardenPledge.sol#L340](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L340)
- [WardenPledge.sol#L401](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L401)
- [WardenPledge.sol#L445](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L445)


## 7. Avoid a function use by swapping two sides of `||` in if statement

There is a chance that the first part will be true so the second part doesn’t need to be checked, so it is better to use the part that we have instead of the part that needs to be called.

- [WardenPledge.sol#L383](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L383)


## 8. use a more recent version of solidity

Use a solidity version of at least 0.8.13 to get the ability to use `using for` with a list of free functions


## 9. usage of uint/int smaller than 32 bytes (256 bits) incurs overhead

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.
Each operation involving a uint8 costs an extra 22-28 gas (depending on whether the other operand is also a variable of type uint8) as compared to ones involving uint256, due to the compiler having to clear the higher bits of the memory word before operating on the uint8, as well as the associated stack operations of doing so. Use a larger size then downcast where needed
https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html Use a larger size then downcast where needed

- [WardenPledge.sol#L41](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L41)
