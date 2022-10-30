## G-01 x += y COSTS MORE GAS THAN x = x + y FOR STATE VARIABLES (SAME APPLIES FOR x -= y , x = x - y)

_There are 3 instances of this issue_

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol

```
File: contracts/WardenPledge.sol

340: pledgeAvailableRewardAmounts[vars.newPledgeID] += vars.totalRewardAmount;
401: pledgeAvailableRewardAmounts[pledgeId] += totalRewardAmount;
445: pledgeAvailableRewardAmounts[pledgeId] += totalRewardAmount;
```

-----------

## G-02 USAGE OF UINTS OR INTS SMALLER THAN 32 BYTES (26 BITS) INCURS OVERHEAD

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

[https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html](https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html) Use a larger size then downcast where needed

_There are 2 instances of this issue_

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol

```
File: contracts/WardenPledge.sol

41: uint64 endTimestamp;
665: function safe64(uint256 n) internal pure returns (uint64) {
```

----------

## G-03 USING BOOLS FOR STORAGE INCURS OVERHEAD

Booleans are more expensive than uint256 or any type that takes up a full word because each write operation emits an extra SLOAD to first read the slot's contents, replace the bits taken up by the boolean, and then write back. This is the compiler's defense against contract upgrades and pointer aliasing, and it cannot be disabled.

_There is 1 instance of this issue_

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol

```
File: contracts/WardenPledge.sol

43: bool closed;
```

-------
