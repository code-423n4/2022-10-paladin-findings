## [NAZ-G1] Using `msg.sender` Instead of Setting It To Memory
**Context**: [`WardenPledge.sol#L299`](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L299)

**Description**:
Using `msg.sender` through out the function `createPledge()` instead of setting it to `creator` variable in memory is cheaper in gas.

**Recommendation**: 
Consider removing `address creator = msg.sender;` in memory and just use `msg.sender` in it's place throughout `createPledge()`.


## [NAZ-G2] Rearanging Setters To Save gas
**Context**: [`WardenPledge.sol#L599`](), [`WardenPledge.sol#L612`](), [`WardenPledge.sol#L625`]()

**Description**:
Moving the event before setting the new value can save gas by not having to `sload` the state varialbe into memory EX:
```js
contract UnOptimized {
    uint256 public value;

    event ValueUpdated(uint256 oldMinTarget, uint256 newMinTargetVotes);

    function updateValue(uint256 newValue) external {
        uint256 oldValue = value;
        value = newValue;

        emit ValueUpdated(oldValue, newValue);
    }
}


contract Optimized {
    uint256 public value;

    event ValueUpdated(uint256 oldValue, uint256 newValue);

    function updateValue(uint256 newValue) external {
        emit ValueUpdated(value, newValue);
        value = newValue;
    }
}
```

**Recommendation**: 
Consider Doing a similar code structure to save gas for setters.


## [NAZ-G3] Use Assembly To Set Storage Variables 
**Context**: [`WardenPledge.sol#L140-L142`](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L140-L142), [`WardenPledge.sol#L602`](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L602), [`WardenPledge.sol#L615`](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L615), [`WardenPledge.sol#L628`](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L628) 

**Description**:
Using assembly to set storage variable is cheaper in gas than setting it normally. This also doesn't degrade readability for the code is still self-explanatory EX:
```js
contract SetExpensive {
    uint256 public set;

    // ~22_520 gas
    function setIt(uint256 _set) public {
        set = _set;
    }
}

contract SetOptimized {
    uint256 public set;
    // ~22_512 gas
    function setIt(uint256 _set) public {
        assembly {
            sstore(set.slot, _set)
        }
    }
}
// ~8 gas cheaper
```

**Recommendation**: 
Consider using assembly to set storage variables.
