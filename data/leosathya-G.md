
### [G-01] > 0 IS LESS EFFICIENT THAN != 0 FOR UNSIGNED INTEGERS 

!= 0 costs less gas compared to > 0 for unsigned integers in require statements with the optimizer enabled (6 gas)

Proof: While it may seem that > 0 is cheaper than !=, this is only true without the optimizer enabled and outside a require statement. If you enable the optimizer at 10k AND you’re in a require statement, this will save gas. You can see this tweet for more proofs: https://twitter.com/gzeon/status/1485428085885640706

There are 2 instances of this issue:
> ** File :  **  => https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L471
> ** File :  **  => https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L504

I suggest changing > 0 with != 0 here:



### [G-02] STATE VARIABLE THAT SHOULD BE CONSTANT

There are 1 instances of this issue:
> ** File :  **  => https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L79


### [G-03] DIVISION BY TWO SHOULD USE BIT SHIFTING
<x> / 2 is the same as <x> >> 1. The DIV opcode costs 5 gas, whereas SHR only costs 3 gas

There are 1 instances of this issue:
> ** File :  **  => https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L263



### [G-04] FUNCTIONS GUARANTEED TO REVERT WHEN CALLED BY NORMAL USERS CAN BE MARKED PAYABLE

If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are CALLVALUE(2),DUP1(3),ISZERO(3),PUSH2(3),JUMPI(10),PUSH1(3),DUP1(3),REVERT(0),JUMPDEST(1),POP(2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost

There are 9 instances of this issue:
> ** File :  **  => https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L560
> ** File :  **  => https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L570
> ** File :  **  => https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L585
> ** File :  **  => https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L599
> ** File :  **  => https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L612
> ** File :  **  => https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L625
> ** File :  **  => https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L636
> ** File :  **  => https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L643
> ** File :  **  => https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L653


### [G-05] NO NEED TO EXPLICITLY INITIALIZE VARIABLES WITH DEFAULT VALUES

If a variable is not set/initialized, it is assumed to have the default value (0 for uint, false for bool, address(0) for address…). Explicitly initializing it with its default value is an anti-pattern and wastes gas.

As an example: for (uint256 i = 0; i < numIterations; ++i) { should be replaced with for (uint256 i; i < numIterations; ++i) {

There are 1 instances of this issue:
> ** File :  **  => https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L547





### [G-06] UNCHECKING ARITHMETICS OPERATIONS THAT CAN’T UNDERFLOW/OVERFLOW

Solidity version 0.8+ comes with implicit overflow and underflow checks on unsigned integers. When an overflow or an underflow isn’t possible (as an example, when a comparison is made before the arithmetic operation), some gas can be saved by using an unchecked block: https://docs.soliditylang.org/en/v0.8.10/control-structures.html#checked-or-unchecked-arithmetic




### [G-07] USING BOOLS FOR STORAGE INCURS OVERHEAD

Booleans are more expensive than uint256 or any type that takes up a full word because each write operation emits an extra SLOAD to first read the slot's contents, replace the bits taken up by the boolean, and then write back. This is the compiler's defense against contract upgrades and pointer aliasing, and it cannot be disabled.

There are 1 instances of this issue:
> ** File :  **  => https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L43
> ** File :  **  => https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L469
> ** File :  **  => https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L499

Use uint256(1) and uint256(2) for true/false to avoid a Gwarmaccess (100 gas) for the extra SLOAD, and to avoid Gsset (20000 gas) when changing from ‘false’ to ‘true’, after having been ‘true’ in the past



### [G-08] “CONSTANTS” EXPRESSIONS ARE EXPRESSIONS, NOT CONSTANTS

Due to how constant variables are implemented (replacements at compile-time), an expression assigned to a constant variable is recomputed each time that the variable is used, which wastes some gas.

If the variable was immutable instead: the calculation would only be done once at deploy time (in the constructor), and then the result would be saved and read directly at runtime rather than being recalculated.

Consequences: each usage of a “constant” costs ~100gas more on each access (it is still a little better than storing the result in storage, but not much..). since these are not real constants, they can’t be referenced from a real constant environment (e.g. from assembly, or from another library )

There are 1 instances of this issue:
> ** File :  **  => https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L24

Change these expressions from constant to immutable and implement the calculation in the constructor or hardcode these values in the constants and add a comment to say how the value was calculated.


### [G-09] REPEATED CODE NOTICED IN SOME FUNCTIONS

This following code repeatedly used in multiple functions like in extendPledge(), increasePledgeRewardPerVote(), retrievePledgeRewards(), closePledge()

if(pledgeId >= pledgesIndex()) revert Errors.InvalidPledgeID();
        address creator = pledgeOwner[pledgeId];
        if(msg.sender != creator) revert Errors.NotPledgeCreator();
        if(receiver == address(0)) revert Errors.ZeroAddress();

> ** File :  **  => https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L374-L378
> ** File :  **  => https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L420-L424
> ** File :  **  => https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L456-L462
> ** File :  **  => https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L488-L493

So i recommend there should be a private fuction that handle this verification of pledge part and return "Pledge storage pledgeParams" on success, and that private function call inside other external functions. 



### [G-10] <x> += <y> COSTS MORE GAS THAN <x> = <x> + <y> 

There are 4 instances of this issue:
> ** File :  **  => https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L268
> ** File :  **  => https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L340
> ** File :  **  => https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L401
> ** File :  **  => https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L445


### [G-11] >= COSTS LESS GAS THAN >

The compiler uses opcodes GT and ISZERO for solidity code that uses >, but only requires LT for >=, which saves 3 gas

There are 13 instances of this issue:
> ** File :  **  => https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L234
> ** File :  **  => https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L241
> ** File :  **  => https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L242
> ** File :  **  => https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L245
> ** File :  **  => https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L267
> ** File :  **  => https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L311
> ** File :  **  => https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L313
> ** File :  **  => https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L320
> ** File :  **  => https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L329
> ** File :  **  => https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L330
> ** File :  **  => https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L386
> ** File :  **  => https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L389
> ** File :  **  => https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L390

So its recommended to use >= whenever possible instead of >


