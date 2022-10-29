## Remove Shorthand Addition/Subtraction Assignment
Instead of using the shorthand of addition/subtraction assignment operators (```+=```, ```-=```) 
it costs less to remove the shorthand (```  x += y  ``` same as ```      x = x+y  ```)  saves ~22 gas
There are 3 instances of this issue:


[Line 445](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L445)

[Line 340](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L340)

[Line 401](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L401)

as an example [Line 445](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L445)
```
Line 445:    pledgeAvailableRewardAmounts[pledgeId] += totalRewardAmount;
```

could be refactored to

```
Line 445:    pledgeAvailableRewardAmounts[pledgeId] = pledgeAvailableRewardAmounts[pledgeId] + totalRewardAmount;
```




## Function Order Affects Gas Consumption
public/external function names and public member variable names can be optimized to save gas. See [this](https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92) link for an example of how it works. Below are the interfaces/abstract contracts that can be optimized so that the most frequently-called functions use the least amount of gas possible during method lookup. Method IDs that have two leading zero bytes can save 128 gas each during deployment, and renaming functions to have lower method IDs will save 22 gas per call, per sorted position shifted



## Functions With Access Control Cheaper if Payable
A function with access control marked as payable will be cheaper for legitimate callers: the compiler removes checks for ```msg.value```, saving approximately ```20``` gas per function call.

There are 10 instances of this issue::

File: `WardenPledge.sol` 
[Line 541](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L541)

File: `WardenPledge.sol` 
[Line 560](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L560)

File: `WardenPledge.sol`
[Line 570](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L570)

File: `WardenPledge.sol`
[Line 585](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L585)

File: `WardenPledge.sol`
[Line 599](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L599)

File: `WardenPledge.sol`
[Line 612](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L612)

File: `WardenPledge.sol`
[Line 625](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L625)

File: `WardenPledge.sol`
[Line 636](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L636)

File: `WardenPledge.sol`
[Line 643](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L643)

File: `WardenPledge.sol`
[Line 653](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L653)


## Uncheck arithmetics operations that can’t underflow/overflow
Solidity version 0.8+ comes with an implicit overflow and underflow checks on unsigned integers. When an overflow or an underflow isn’t possible, some gas can be saved by using an unchecked block.
For example:
```
Line 445:    pledgeAvailableRewardAmounts[pledgeId] += totalRewardAmount;
```
[Line 445](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L445)
could be refactored to

```
Line 445:    unchecked { pledgeAvailableRewardAmounts[pledgeId] += totalRewardAmount; }
```

[Line 340](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L340)
[Line 401](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L401)



##  Using `Storage` Instead of `Memory` for Structs/Arrays Saves Gas
When fetching data from a `storage` location, assigning the data to a `memory` variable causes all fields of the struct/array to be read from `storage`, which incurs a Gcoldsload (2100 gas) for each field of the struct/array. If the fields are read from the new `memory` variable, they incur an additional `MLOAD` rather than a cheap stack read. Instead of declaring the variable with the `memory` keyword, declaring the variable with the `storage` keyword and caching any fields that need to be re-read in stack variables, will be much cheaper, only incurring the Gcoldsload for the fields actually read. The only time it makes sense to read the whole struct/array into a `memory` variable, is if the full struct/array is being returned by the function, is being passed to a function that requires `memory`, or if the array/struct is being read from another `memory` array/struct.

There are 2 instances of this issue:

[Line 227](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L227)

[Line 318](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L318)