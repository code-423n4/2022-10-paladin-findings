## 1. Explicitly assingning default values to variables is a waste of gas
### Use `uint256 i;` instead of `uint256 i = 0;`

_contracts/WardenPledge.sol:_ [L547](https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L547)

## 2. When comparing variables of type `uint`, use `require(x != 0)` instead of `require(x > 0)`

_contracts/WardenPledge.sol:_ [L471](https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L471)
[L504](https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L504)

## 3. Cache storage variables in function call stack to save gas

_contracts/WardenPledge.sol:_ [L240-L248](https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L240-L248)

## 4. Use `constant` and `immutable` for constants

_contracts/WardenPledge.sol:_ [L60](https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L60)
[L62](https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L62)

## 5. `public` storage `constant`(and `immutable`) variable should be `private`
### saves tons of gas on deployment

_contracts/WardenPledge.sol:_ [L22](https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L22)
[L23](https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L23)
[L24](https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L24)

## 6. Use `x < y + 1` in stead of `x <= y`

_contracts/WardenPledge.sol:_ [L223](https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L223)
[L229](https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L229)
[L374](https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L374)
[L380](https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L380)
[L426](https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L426)
[L429](https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L429)
[L489](https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L489)

## 7. use x = x + y instead of x+= y

_contracts/WardenPledge.sol:_ [L268](https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L268)
[L340](https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L340)
[L401](https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L401)

## 8. Place `i++` in an `unchecked` blocks in for-loops

_contracts/WardenPledge.sol:_ [L550](https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L550)

