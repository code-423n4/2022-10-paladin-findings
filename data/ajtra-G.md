# Index
1. Using storage instead of memory for structs/arrays
2. I = I + (-) X is cheaper in gas cost than I += (-=) X
3. Emit the event before modify the storage variable
4. Operatos >= cost less gas than operator >
5. State variables that never change should be declared immutable or constant

# Details
## 1. Using storage instead of memory for structs/arrays
### Description
When retrieving data from a memory location, assigning the data to a memory variable causes all fields of the struct/array to be read from memory, 
resulting in a Gcoldsload (2100 gas) for each field of the struct/array. 
When reading fields from new memory variables, they cause an extra MLOAD instead of a cheap stack read. 
Rather than declaring a variable with the memory keyword, it is much cheaper to declare a variable with the storage keyword and cache all fields 
that need to be read again in a stack variable, because the fields actually read will only result in a Gcoldsload. 
The only case where the entire struct/array is read into a memory variable is when the entire struct/array is returned by a function, 
passed to a function that needs memory, or when the array/struct is read from another store array/struc

### Lines in the code
[WardenPledge.sol#L227](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L227)
	
## 2. I = I + (-) X is cheaper in gas cost than I += (-=) X
### Description
In the following example (optimizer = 10000) it's possible to demostrate that I = I + X cost less gas than I += X in state variable.

```solidity
contract Test_Optimization {
    uint256 a = 1;
    function Add () external returns (uint256) {
        a = a + 1;
        return a;
    }
}

contract Test_Without_Optimization {
    uint256 a = 1;
    function Add () external returns (uint256) {
        a += 1;
        return a;
    }
}
```
* Test_Optimization cost is 26324 gas
* Test_Without_Optimization cost is 26440 gas

With this optimization it's possible to **save 116 gas**

### Lines in the code
[WardenPledge.sol#L268](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L268)
[WardenPledge.sol#L340](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L340)
[WardenPledge.sol#L401](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L401)
[WardenPledge.sol#L445](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L445)

## 3. Emit the event before modify the storage variable
### Description
If emit the event before assign de value to the local variable we can save to use the local variable
and remove it.

```diff
    function updateChest(address chest) external onlyOwner {
        if(chest == address(0)) revert Errors.ZeroAddress();
-       address oldChest = chestAddress;
+       emit ChestUpdated(chestAddress, chest);
        chestAddress = chest;

-       emit ChestUpdated(oldChest, chest);
    }
```
[WardenPledge.sol#L599-L605](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L599-L605)

```diff
    function updateMinTargetVotes(uint256 newMinTargetVotes) external onlyOwner {
        if(newMinTargetVotes == 0) revert Errors.InvalidValue();
-       uint256 oldMinTarget = minTargetVotes;
+       emit MinTargetUpdated(minTargetVotes, newMinTargetVotes);
        minTargetVotes = newMinTargetVotes;

-       emit MinTargetUpdated(oldMinTarget, newMinTargetVotes);
    }
```
[WardenPledge.sol#L612-L618](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L612-L618)

```diff
    function updatePlatformFee(uint256 newFee) external onlyOwner {
        if(newFee > 500) revert Errors.InvalidValue();
-       uint256 oldfee = protocalFeeRatio;
+       emit PlatformFeeUpdated(protocalFeeRatio, newFee);
        protocalFeeRatio = newFee;

-       emit PlatformFeeUpdated(oldfee, newFee);
    }
```
[WardenPledge.sol#L625-L631](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L625-L631)

## 4. Operatos >= cost less gas than operator >
### Description
When use >= the evm use only LT and when use > the evm use GT and ISZERO what allow to save 3 gas for each instance.

### Lines in the code
[WardenPledge.sol#L207](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L207)
[WardenPledge.sol#L234](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L234)
[WardenPledge.sol#L245](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L245)
[WardenPledge.sol#L267](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L267)
[WardenPledge.sol#L329](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L329)
[WardenPledge.sol#L330](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L330)
[WardenPledge.sol#L389](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L389)
[WardenPledge.sol#L390](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L390)
[WardenPledge.sol#L434](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L434)
[WardenPledge.sol#L435](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L435)
[WardenPledge.sol#L463](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L463)
[WardenPledge.sol#L666](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L666)

## 5. State variables that never change should be declared immutable or constant
### Description
Avoids a Gsset (20000 gas) in the constructor, and replaces the first access in each transaction (Gcoldsload - 2100 gas) 
and each access thereafter (Gwarmacces - 100 gas) with a PUSH32 (3 gas).

### Lines in the code
[WardenPledge.sol#L60](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L60)
[WardenPledge.sol#L62](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L62)
