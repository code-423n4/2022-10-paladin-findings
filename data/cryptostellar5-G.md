### G-01 USING BOOLS FOR STORAGE INCURS OVERHEAD


```
    // Booleans are more expensive than uint256 or any type that takes up a full    // word because each write operation emits an extra SLOAD to first read the    // slot's contents, replace the bits taken up by the boolean, and then write    // back. This is the compiler's defense against contract upgrades and    // pointer aliasing, and it cannot be disabled.
```

[https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27) Use `uint256(1)` and `uint256(2)` for true/false to avoid a Gwarmaccess (**[100 gas](https://gist.github.com/IllIllI000/1b70014db712f8572a72378321250058)**) for the extra SLOAD, and to avoid Gsset (**20000 gas**) when changing from ‘false’ to ‘true’, after having been ‘true’ in the past

[https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol)  

```
43: bool closed;
```


### G-02 X += Y COSTS MORE GAS THAN X = X + Y FOR STATE VARIABLES

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol

```
340: pledgeAvailableRewardAmounts[vars.newPledgeID] += vars.totalRewardAmount;
401: pledgeAvailableRewardAmounts[pledgeId] += totalRewardAmount;
445: pledgeAvailableRewardAmounts[pledgeId] += totalRewardAmount;
```


### G-03 USAGE OF UINTS OR INTS SMALLER THAN 32 BYTES (26 BITS) INCURS OVERHEAD


> When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

[https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html](https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html) Use a larger size then downcast where needed

[https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol)  

```
41: uint64 endTimestamp;
665: function safe64(uint256 n) internal pure returns (uint64) {
```
