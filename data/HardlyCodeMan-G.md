## Gas

### Using bools for storage incurs overhead
Use a uint for a binary representation boolean rather than an actual boolean to save storage through multiple iterations.

[WardenPledge.sol](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L28-L44)
```solidity
43:     bool closed;
```

### Utilise mapping over array wher no iteration is needed
Use mapping instead of array (5000 gas saved per value). Mappings are usually less expensive than arrays, but you can't iterate over them.

[WardenPledge.sol](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L47)
```solidity
47:     Pledge[] public pledges;
```

