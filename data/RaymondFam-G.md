## Non-strict inequalities are cheaper than strict ones
In the EVM, there is no opcode for non-strict inequalities (>=, <=) and two operations are performed (> + = or < + =). There are a total of five instances associated with >= entailed:

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L223
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L374
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L420
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L457
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L489

As an example, consider replacing >= with the strict counterpart > in line 489:

```
        if(pledgeId > pledgesIndex() - 1) revert Errors.InvalidPledgeID();
```
There are also a total of five instances associated with <= entailed:

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L229
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L380
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L426
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L429
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L496

As an example, consider replacing <= with the strict counterpart < in line 496:

```
        if(pledgeParams.endTimestamp < block.timestamp + 1) revert Errors.ExpiredPledge();
```
## += and -= Costs More Gas
`+=` generally costs 22 more gas than writing out the assigned equation explicitly. The amount of gas wasted can be quite sizable when repeatedly operated in a loop (which is fortunately not found in the contract in scope). 

There are also a total of three instances associated with += entailed:

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L445
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L340
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L401

As an example, the following line of code in line 401 could be rewritten as:

```
        pledgeAvailableRewardAmounts[pledgeId] = pledgeAvailableRewardAmounts[pledgeId] + totalRewardAmount;
```
There is one instance associated with -= entailed which could be rewritten as:

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L268

```
        pledgeAvailableRewardAmounts[pledgeId] = pledgeAvailableRewardAmounts[pledgeId] - rewardAmount;
```
## Function Order Affects Gas Consumption
The order of function will also have an impact on gas consumption. Because in smart contracts, there is a difference in the order of the functions. Each position will have an extra 22 gas. The order is dependent on method ID. So, if you rename the frequently accessed function to more early method ID, you can save gas cost. Please visit the following site for further information:

https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92

## Activate the Optimizer
Before deploying your contract, activate the optimizer when compiling using “solc --optimize --bin sourceFile.sol”. By default, the optimizer will optimize the contract assuming it is called 200 times across its lifetime. If you want the initial contract deployment to be cheaper and the later function executions to be more expensive, set it to “ --optimize-runs=1”. Conversely, if you expect many transactions and do not care for higher deployment cost and output size, set “--optimize-runs” to a high number.

```
module.exports = {
  solidity: {
    version: "0.8.11",
    settings: {
      optimizer: {
        enabled: true,
        runs: 1000,
      },
    },
  },
};
```
Please visit the following site for further information:

https://docs.soliditylang.org/en/v0.5.4/using-the-compiler.html#using-the-commandline-compiler

Here's one example of instance on opcode comparison that delineates the gas saving mechanism:

for !=0 before optimization
PUSH1 0x00
DUP2
EQ
ISZERO
PUSH1 [cont offset]
JUMPI

after optimization
DUP1
PUSH1 [revert offset]
JUMPI

Disclaimer: There have been several bugs with security implications related to optimizations. For this reason, Solidity compiler optimizations are disabled by default, and it is unclear how many contracts in the wild actually use them. Therefore, it is unclear how well they are being tested and exercised. High-severity security issues due to optimization bugs have occurred in the past . A high-severity bug in the emscripten -generated solc-js compiler used by Truffle and Remix persisted until late 2018. The fix for this bug was not reported in the Solidity CHANGELOG. Another high-severity optimization bug resulting in incorrect bit shift results was patched in Solidity 0.5.6. Please measure the gas savings from optimizations, and carefully weigh them against the possibility of an optimization-related bug. Also, monitor the development and adoption of Solidity compiler optimizations to assess their maturity.

## Use Storage Instead of Memory for Structs/Arrays
A storage pointer is cheaper since copying a state struct in memory would incur as many SLOADs and MSTOREs as there are slots. In another words, this causes all fields of the struct/array to be read from storage, incurring a Gcoldsload (2100 gas) for each field of the struct/array, and then further incurring an additional MLOAD rather than a cheap stack read. As such, declaring the variable with the storage keyword and caching any fields that need to be re-read in stack variables will be much cheaper, involving only Gcoldsload for all associated field reads. Read the whole struct/array into a memory variable only when it is being returned by the function, passed into a function that requires memory, or if the array/struct is being read from another memory array/struct. 

There are a total of two instances entailed:

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L227
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L318

## Unchecked SafeMath Saves Gas
"Checked" math, which is default in ^0.8.0 is not free. The compiler will add some overflow checks, somehow similar to those implemented by `SafeMath`. While it is reasonable to expect these checks to be less expensive than the current `SafeMath`, one should keep in mind that these checks will increase the cost of "basic math operation" that were not previously covered. This particularly concerns variable increments in for loops. When no arithmetic overflow/underflow is going to happen, `unchecked { ++i ;}` to use the previous wrapping behavior further saves gas.

Here are some of the instances entailed:

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L445
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L340
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L401

As an example, line 445 could be rewritten as:

```
    unchecked { pledgeAvailableRewardAmounts[pledgeId] += totalRewardAmount; }
```
## Use of Named Returns for Local Variables Saves Gas
You can have further advantages in term of gas cost by simply using named return values as temporary local variable. There are a total of seven instances entailed:

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L153
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L163
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L172
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L181
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L307
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L653
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L665

As an example, lines 307 and 337 of `createPledge()` can be refactored via the chain assignment method:

```
Line 307    ) external whenNotPaused nonReentrant returns(uint256 newPledgeId){

Line 337    newPledgeId = vars.newPledgeID = pledgesIndex();

```
Note: Line 357 may therefore be removed.

## Payable Access Control Functions Costs Less Gas
Consider marking functions with access control as `payable`. This will save 20 gas on each call by their respective permissible callers for not needing to have the compiler check for `msg.value`. 

There are a total of ten instances entailed:

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L541
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L560
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L570
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L585
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L599
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L612
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L625
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L636
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L643
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L653
