- [Low](#low)
    - [**1. Outdated compiler**](#1-outdated-compiler)
    - [**2. Ownable and Pausable**](#2-ownable-and-pausable)
    - [**3. Lack of checks supportsInterface**](#3-lack-of-checks-supportsinterface)
    - [**4. Lack of checks address0**](#4-lack-of-checks-address0)
    - [**5. Lack of checks integer ranges**](#5-lack-of-checks-integer-ranges)
    - [**6. Owners can steal the rewards**](#6-owners-can-steal-the-rewards)
- [Non critical](#non-critical)
    - [**7. Use solidity literals instead of math**](#7-use-solidity-literals-instead-of-math)
    - [**8. Avoid integer underflow**](#8-avoid-integer-underflow)
    - [**9. Improve coding style**](#9-improve-coding-style)

# Low

## **1. Outdated compiler**

The pragma version used is:

```
pragma solidity 0.8.10;
```

The minimum required version must be [0.8.17](https://github.com/ethereum/solidity/releases/tag/v0.8.17); otherwise, contracts will be affected by the following **important bug fixes**:

[0.8.13](https://blog.soliditylang.org/2022/03/16/solidity-0.8.13-release-announcement/):
- Code Generator: Correctly encode literals used in `abi.encodeCall` in place of fixed bytes arguments.

[0.8.14](https://blog.soliditylang.org/2022/05/18/solidity-0.8.14-release-announcement/):

- ABI Encoder: When ABI-encoding values from calldata that contain nested arrays, correctly validate the nested array length against `calldatasize()` in all cases.
- Override Checker: Allow changing data location for parameters only when overriding external functions.

[0.8.15](https://blog.soliditylang.org/2022/06/15/solidity-0.8.15-release-announcement/)

- Code Generation: Avoid writing dirty bytes to storage when copying `bytes` arrays.
- Yul Optimizer: Keep all memory side-effects of inline assembly blocks.

[0.8.16](https://blog.soliditylang.org/2022/08/08/solidity-0.8.16-release-announcement/)

- Code Generation: Fix data corruption that affected ABI-encoding of calldata values represented by tuples: structs at any nesting level; argument lists of external functions, events and errors; return value lists of external functions. The 32 leading bytes of the first dynamically-encoded value in the tuple would get zeroed when the last component contained a statically-encoded array.

[0.8.17](https://blog.soliditylang.org/2022/09/08/solidity-0.8.17-release-announcement/)

- Yul Optimizer: Prevent the incorrect removal of storage writes before calls to Yul functions that conditionally terminate the external EVM call.

Apart from these, there are several minor bug fixes and improvements.

## **2. `Ownable` and `Pausable`**

The contract `WardenPledge` is `Ownable` and `Pausable`, so the owner could resign while the contract is paused, causing a Denial of Service. Owner resignation while the contract is paused should be avoided.

**Affected source code:**

- [WardenPledge.sol:18](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L18)

## **3. Lack of checks `supportsInterface`**

The `EIP-165` standard helps detect that a smart contract implements the expected logic, prevents human error when configuring smart contract bindings, so it is recommended to check that the received argument is a contract and supports the expected interface.

**Reference:**

- https://eips.ethereum.org/EIPS/eip-165

**Affected source code:**

- [WardenPledge.sol:137-138](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L137-L138)

## **4. Lack of checks `address(0)`**

The following methods have a lack of checks if the received argument is an address, it's good practice in order to reduce human error to check that the address specified in the constructor or initialize is different than `address(0)`.

**Affected source code:**

- [WardenPledge.sol:137-138](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L137-L138)
- [WardenPledge.sol:140](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L140)

## **5. Lack of checks integer ranges**

The following methods lack checks on the following integer arguments, you can see the recommendations above.

**Affected source code:**

`minTargetVotes` is allowed to be `0` during the constructor, but when this will be changed using [updateMinTargetVotes](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L613) will not be allowed.

- [WardenPledge.sol:142](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L142)

## **6. Owners can steal the rewards**

The `WardenPledge` contract has a `recoverERC20` method through which the owner can take an arbitrary erc20 token, this method contains a protection to prevent it from being a reward token, however, if the owner calls `removeRewardToken` first, owner will have no problem in being able to subtract the balance.

**Affected source code:**

- [WardenPledge.sol:654](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L654)

---

# Non critical

## **7. Use solidity literals instead of math**

Suffixes like `seconds`, `minutes`, `hours`, `days` and `weeks` after literal numbers can be used to specify units of time where seconds are the base unit and units are considered naively in the following way:

- 1 == 1 seconds
- 1 minutes == 60 seconds
- 1 hours == 60 minutes
- 1 days == 24 hours
- 1 weeks == 7 days

**Reference:**

- https://docs.soliditylang.org/en/v0.8.14/units-and-global-variables.html#time-units

**Affected source code:**

- [WardenPledge.sol:24](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L24)

## **8. Avoid integer underflow**

In the method `_pledge`, when `endTimestamp` is less than `block.timestamp` it will fault with a non custom error.

**Affected source code:**

- [WardenPledge.sol:237](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L237)

## **9. Improve coding style**

Move all structures to a different file or to the top of the contract to improve code readability.

**Affected source code:**

- [WardenPledge.sol:279-285](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L279-L285)

