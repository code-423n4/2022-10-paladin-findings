## [G002]

_Instances (4)_:

```solidity
  154:
     WardenPledge.sol::154 => return pledges.length;

    **Possible Solution:**
    function pledgesIndex() public view returns(uint256){
        uint256 index = pledges.length;
        return index;
    }


  542:
     WardenPledge.sol::542 => uint256 length = tokens.length;
  544:
    WardenPledge.sol::544 => if(length == 0) revert Errors.EmptyArray();
  545:
     WardenPledge.sol::545 => if(length != minRewardsPerSecond.length) revert Errors.InequalArraySizes();
```