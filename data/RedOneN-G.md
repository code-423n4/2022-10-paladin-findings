## [G-01] Usage of UINT/INT smaller than 256bits incurs overhead.

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

WardenPledge.sol#L41



## [G-02] Non-strict inequalities are cheaper than strict ones.

Strict inequalities (> or <) are more expensive than non-strict ones (>= or <=). This is due to some supplementary checks (ISZERO, 3 gas). As a result, it is possible potentialy save 3 gas for various lines in the contract (at least where it has almost no impact on the logic) :

WardenPledge.sol#L234
WardenPledge.sol#L245
WardenPledge.sol#L386
WardenPledge.sol#L463



## [G-03] It costs more gas to initialize variables to zero than to let the default of zero be applied.

if a variable is not set/initialized, it is assumed to have the default value (0 for uint, false for bool, address(0) for address…). Explicitly initializing it with its default value is an anti-pattern and wastes gas.

***File : WardenPledge.sol***

WardenPledge.sol#L547
WardenPledge.sol#L613



## [G-04] Usage of UINT/INT smaller than 256bits incurs overhead.

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

***File : WardenPledge.sol***

WardenPledge.sol#L41



## [G-05] Functions guaranteed to revert when called by normal users (eg. onlyOwner) can be marked payable.

If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are CALLVALUE(2),DUP1(3),ISZERO(3),PUSH2(3),JUMPI(10),PUSH1(3),DUP1(3),REVERT(0),JUMPDEST(1),POP(2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost.

WardenPledge.sol#L541
WardenPledge.sol#L560
WardenPledge.sol#L570
WardenPledge.sol#L585
WardenPledge.sol#L599
WardenPledge.sol#L612
WardenPledge.sol#L625
WardenPledge.sol#L636
WardenPledge.sol#L653


## [G-06] multiple address/ID mappings can be combined into a single mapping of an address/ID to a struct, where appropriate

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to not having to recalculate the key’s keccak256 hash (Gkeccak256 - 30 gas) and that calculation’s associated stack operations.

The pledgeOwner and the pledgeAvailableRewardAmounts mappings can be combined to a single struct.

WardenPledge.sol#L50
WardenPledge.sol#L56


## [G-07] Use a more recent version of solidity 

Solidity versions higher than 0.8.10 have some benefits from a gas optimisation point of view. Version 0.8.12 will remove the mstore and sstore operations if the slot already contains the same value. version 0.8.15 improved Inlining heuristics in the optimizer and resulted in lower bytecode size and consequently lower deployment costs. Version 0.8.16 has a more efficient code for checked addition and subtraction.


## [G-08] CACHING STORAGE VALUES IN MEMORY

Within the same function, calling multiple time the same "state variable" is inefficient from a gas point of view. SLOADs cost 100gas compared to MLOADs/MSTOREs ("gas). As a result, it is more efficient to first store with an sload the state variable into memory and then call this new variable. 

minAmountRewardToken (2SLOADS)
WardenPledge.sol#L312
WardenPledge.sol#L313


## [G-09] For state variable "x = x + y"  is less expensive than "X += Y"
 
There are 4 instances of it : 

WardenPledge.sol#L268
WardenPledge.sol#L340
WardenPledge.sol#L401
WardenPledge.sol#L445


## [G-10] Consider declaring variables that are not modified in the contract as "CONSTANT".

The variable minDelegationTime is called within the contract but is never updated.

WardenPledge.sol#L79

## [G-11] DUPLICATED REQUIRE()/REVERT() CHECKS SHOULD BE REFACTORED TO A MODIFIER OR FUNCTION

Doing so will improve deployment cost and code readability. 

The following set of conditions performed in three different functions (closePledge, retrievePledgeRewards, increasePledgeRewardPerVote) :
WardenPledge.sol#L420:#L426 (excluding the close "check which is only performed in two of the three function).


if(token == address(0)) revert Errors.ZeroAddress() is called 3 times : 
WardenPledge.sol#L527
WardenPledge.sol#L571
WardenPledge.sol#L586


## [G-12] Consider ranking require/revert within the same function by their gas costs 

Revert()/require() that only perform checks on memory variables should come first. Then one could order the revert() based on the number of called state variables.

WardenPledge.sol#L224 should come before WardenPledge.sol#L223

WardenPledge.sol#L315:L316 should come before WardenPledge.sol#L311

WardenPledge.sol#L527:L528 should come before WardenPledge.sol#L526


## [G-13] arithmetic on variables that cannot overflow/underflow can be "UNCHECKED()"

WardenPledge.sol#L385


## [G-14] Events should "indexed" three variables when possible to save gas.

WardenPledge.sol#L94
WardenPledge.sol#L96
WardenPledge.sol#L98
WardenPledge.sol#L102
WardenPledge.sol#L105
WardenPledge.sol#L108
WardenPledge.sol#L110
WardenPledge.sol#L115
WardenPledge.sol#L116
WardenPledge.sol#L119




