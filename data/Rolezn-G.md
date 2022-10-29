## Summary<a name="Summary">

### Gas Optimizations
| |Issue|Contexts|
|-|:-|:-:|
| [GAS&#x2011;1](#GAS&#x2011;1) | Multiple Address Mappings Can Be Combined Into A Single Mapping Of An Address To A Struct, Where Appropriate | 2 |
| [GAS&#x2011;2](#GAS&#x2011;2) | Use assembly to check for `address(0)` | 2 |
| [GAS&#x2011;3](#GAS&#x2011;3) | Usage of `uints`/`ints` smaller than 32 bytes (256 bits) incurs overhead | 1 |
| [GAS&#x2011;4](#GAS&#x2011;4) | Optimize names to save gas | 1 |
| [GAS&#x2011;5](#GAS&#x2011;5) | Use `uint256(1)`/`uint256(2)` instead for `true` and `false` boolean states | 3 |

Total: 9 contexts over 5 issues

## Gas Optimizations

### <a href="#Summary">[GAS&#x2011;1]</a><a name="GAS&#x2011;1"> Multiple Address Mappings Can Be Combined Into A Single Mapping Of An Address To A Struct, Where Appropriate

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot.

#### <ins>Proof Of Concept</ins>


```
52: mapping(address => uint256[]) public ownerPledges;
```

https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L52

```
67: mapping(address => uint256) public minAmountRewardToken;
```

https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L67


### <a href="#Summary">[GAS&#x2011;2]</a><a name="GAS&#x2011;2"> Use assembly to check for `address(0)`

Save 6 gas per instance if using assembly to check for address(0)

e.g.
```
assembly {
	if iszero(_addr) {
		mstore(0x00, "AddressZero")
		revert(0x00, 0x20)
	}
}
```

#### <ins>Proof Of Concept</ins>


```
function removeRewardToken(address token) external onlyOwner {
```

https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L585

```
function updateChest(address chest) external onlyOwner {
```

https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L599




### <a href="#Summary">[GAS&#x2011;3]</a><a name="GAS&#x2011;3"> Usage of `uints`/`ints` smaller than 32 bytes (256 bits) incurs overhead

When using elements that are smaller than 32 bytes, your contract's gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html
Each operation involving a `uint8` costs an extra 22-28 gas (depending on whether the other operand is also a variable of type `uint8`) as compared to ones involving `uint256`, due to the compiler having to clear the higher bits of the memory word before operating on the `uint8`, as well as the associated stack operations of doing so. Use a larger size then downcast where needed

#### <ins>Proof Of Concept</ins>


```
41: uint64 endTimestamp;
```

https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L41






### <a href="#Summary">[GAS&#x2011;4]</a><a name="GAS&#x2011;4"> Optimize names to save gas

`public`/`external` function names and `public` member variable names can be optimized to save gas. See [this](https://github.com/enzosv/solidity-optimize-name) link for an example of how it works. Method IDs that have two leading zero bytes can save **128 gas** each during deployment, and renaming functions to have lower method IDs will save **22 gas** per call, [per sorted position shifted](https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92)

#### <ins>Proof Of Concept</ins>

```
File: \2022-10-paladin\contracts\WardenPledge.sol
```

https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol






### <a href="#Summary">[GAS&#x2011;5]</a><a name="GAS&#x2011;5"> Use `uint256(1)`/`uint256(2)` instead for `true` and `false` boolean states

If you don't use boolean for storage you will avoid Gwarmaccess 100 gas. For example, state changes of boolean from `true` to `false` can cost up to ~20000 gas rather than `uint256(2)` to `uint256(1)` that would cost significantly less.

#### <ins>Proof Of Concept</ins>


```
469: if(!pledgeParams.closed) pledgeParams.closed = true;
```

https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L469

```
499: pledgeParams.closed = true;
```

https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L499






