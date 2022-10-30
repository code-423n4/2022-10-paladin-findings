## [G-01] Using msg.sender will always be cheaper than using a local variable.
The CALLER operation which is reading the msg.sender global variable costs 2. While MLOAD/MSTORE operations which are memory operations (storage where local variables are stored) costs 3.

[308](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L308)

## [G-02] Optimizations with assembly
### [G-02.1] Use assembly for math (add, sub, mul, div)
[263](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L263),
[265](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L265),
[327-328](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L327-L328),
[387-388](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L387-L388),[432-433](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L432-L433)

### [G-02.2] Use assembly to check for address(0)
[310](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L310), [460](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L460), [492](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L492), [527](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L527),  [571](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L571), [586](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L586), [600](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L600)

## [G-03] Use Add unchecked {} where the operands can not underflow/overflow because of a previous check
[431](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L431)

## [G-04] Cache storage values in memory to minimize SLOADs
`pledgeParams.endTimestamp` [380](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L380), [382](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L382), [426](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L426), [430](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L430)

`minAmountRewardToken[rewardToken]` [312-313](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L312-L313)

## [G-05] X += Y or X -= Y costs more gas than  X = X + Y or X = X - Y  for state variables
[268](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L268), [340](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L340), [401](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L401), [445](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L445)

## [G-05] Use modifier to decrease deployment cost
`pledgeId >= pledgesIndex()` 
[223](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L223),
[374](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L374),
[420](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L420),
[457](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L457),
[489](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L489)

to -> modifier validPledgeID
```
address creator = pledgeOwner[pledgeId];
if(msg.sender != creator) revert Errors.NotPledgeCreator(); 
```
to -> modifier onlyPledgeCreator
[458](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L458),
[490](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L490)
