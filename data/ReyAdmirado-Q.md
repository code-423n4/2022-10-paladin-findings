# QA

| | issue |
| ----------- | ----------- |
| 1 | [typo](#1-typo) |
| 2 | [event is missing indexed fields](#2-event-is-missing-indexed-fields) |
| 3 | [Code Structure Deviates From Best-Practice](#3-code-structure-deviates-from-best-practice) |
| 4 | [Use Underscores for Number Literals](#4-use-underscores-for-number-literals) |
| 5 | [Constant redefined elsewhere](#5-constant-redefined-elsewhere) |
| 6 | [consider changing this state var to constant to reduce confusion ](#6-consider-changing-this-state-var-to-constant-to-reduce-confusion) |
| 7 | [Missing event emitting](#7-missing-event-emitting) |
| 8 | [Outdated compiler version](#8-outdated-compiler-version) |

## 1. typo

protocalFeeRatio --> protocolFeeRatio (this is a mistake in a statevar and it has 6 instances)
- [WardenPledge.sol#L41](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L41)

taget --> target
- [WardenPledge.sol#L292](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L292)

balacne --> balance
- [WardenPledge.sol#L292](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L292)

reards --> rewards
- [WardenPledge.sol#L339](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L339)

Minmum --> Minimum (4 instances) 
- [WardenPledge.sol#L523](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L523)

Platfrom --> Platform
- [WardenPledge.sol#L621](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L621)


## 2. event is missing indexed fields
Index event fields make the field more quickly accessible to off-chain tools that parse events. However, note that each index field costs extra gas during emission, so it’s not necessarily best to index the maximum allowed per event (three fields). Each event should use three indexed fields if there are three or more fields, and gas usage is not particularly of concern for the events in question. If there are fewer than three fields, all of the fields should be indexed.

- [WardenPledge.sol#L94](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L94)
- [WardenPledge.sol#L96](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L96)
- [WardenPledge.sol#L98](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L98)

- [WardenPledge.sol#L102](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L102)
- [WardenPledge.sol#L105](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L105)


## 3. Code Structure Deviates From Best-Practice
The best-practice layout for a contract should follow the following order: state variables, events, modifiers, constructor and functions. Function ordering helps readers identify which functions they can call and find constructor and fallback functions easier. Functions should be grouped according to their visibility and ordered as: constructor, receive function (if exists), fallback function (if exists), external, public, internal, private.

wrong place for struct
- [WardenPledge.sol#L279](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L279)

external should be after each other first then public then internal then private
- [WardenPledge.sol#L153](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L153)
- [WardenPledge.sol#L181](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L181)
- [WardenPledge.sol#L222](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L222)
- [WardenPledge.sol#L525](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L525)


## 4. Use Underscores for Number Literals
There are multiple occasions where certain numbers have been hardcoded, either in variables or in the code itself. Large numbers can become hard to read.

- [WardenPledge.sol#L23](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L23)
- [WardenPledge.sol#L24](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L24)


## 5. Constant redefined elsewhere
Consider defining in only one contract so that values cannot become out of sync when only one location is updated. A cheap way to store constants in a single location is to create an internal constant in a library. If the variable is a local cache of another contract’s value, consider making the cache variable internal or private, which will require external users to query the contract with the source of truth, so that callers don’t get out of sync.
https://medium.com/coinmonks/gas-cost-of-solidity-library-functions-dbe0cedd4678

- [WardenPledge.sol#L22](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L22)
- [WardenPledge.sol#L23](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L23)
- [WardenPledge.sol#L24](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L24)


## 6. consider changing this state var to constant to reduce confusion 

`minDelegationTime` state var does not change and its always 1 week, change it to constant to improve readability of the contract

- [WardenPledge.sol#L79](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L79)

## 7. Missing event emitting

Event is never emitted, consider adding emit in the position intended

`IncreasePledgeTargetVotes`
- [WardenPledge.sol#L96](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L96)


## 8. Outdated compiler version
Using old versions of Solidity prevents benefits of bug fixes and newer security checks. Using the latest versions might make contracts susceptible to undiscovered compiler bugs