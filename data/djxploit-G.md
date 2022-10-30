## 1) Use unchecked keyword wherever appropriate to save gas by avoiding unnecessary underflow/overflow check

[https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L268](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L268)

## 2) Storage pointer to a structure is cheaper than copying each value of the structure into memory 

It may not be obvious, but every time you copy a storage struct/array/mapping to a memory variable, you are literally copying each member by reading it from storage, which is expensive. And when you use the storage keyword, you are just storing a pointer to the storage, which is much cheaper.

[https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L227](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L227)
[https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L318](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L318)

## 3) `x = x + y` is cheaper than `x+=y`

[https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L340](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L340)
[https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L401](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L401)
[https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L445](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L445)

## 4) Usage of uints/ints smaller than 32 bytes (256 bits) incurs overhead

[https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L41](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L41)

## 5) Multiple mappings with same key can be combined to a struct

[https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L50](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L50) with [https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L56](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L56)

[https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L52](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L52) with [https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L67](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L67)