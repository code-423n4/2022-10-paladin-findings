## Typos

Stick to the proper spelling or else otherwise, it will make it decrease readability

There are 9 instances of this issue consider making the following changes:

**Change** "minmum" to "minimum" [Line 523](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L523), [Line 539](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L539), [Line 558](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L558), [Line 568](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L568)

**Change** "protocalFeeRatio" to "protocolFeeRatio" [Line 71](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L71)

**Change** "reards" to "rewards" [Line 339](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L339)

**Change** "InequalArraySizes" to "UnequalArraySizes" [Line 545](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L545)

**Change** "taget" to "target" and "balacne" to "balance" [Line 292](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L292)

**Change** "Platfrom" to "Platform" [Line 621-622](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L621-L622)

## Use CamelCase

Consider sticking to using the CamelCase naming convention as it is highly recommended to follow these guidelines to help improve the readability of the source code
for example:

```
Line 117:    event PlatformFeeUpdated(uint256 oldfee, uint256 newFee);
```

[Line 117](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L117)

Should be refactored to

```
Line 117:    event PlatformFeeUpdated(uint256 oldFee, uint256 newFee);
```