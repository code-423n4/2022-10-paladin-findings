## CONSTRUCTOR SANITY CHECKS
Zero address, zero value, and empty string checks implemented at the constructor could avoid human errors leading to non-functional calls associated with the mistakes. This is especially so when the incidents entail immutable state variables that could end up having had to redeploy the contract.

[Lines 131 - 143](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L131-L143)

## TIMELOCK FOR CRITICAL PARAMETER CHANGE
It is a good practice giving time to users to react and adjust to critical changes with a mandatory time window between the changes. The first step is simply broadcasting to users with a specific change that is coming whilst the second step commits that change after an appropriate period of waiting. This would allow time for users opposing to the change to withdraw within the set time frame. A timelock provides more guarantees and reduces the level of trust required, thus decreasing risk for users. It also indicates that the project is legitimate (less risk of the owner making a malicious act). Specifically, privileged roles could use front running to make malicious changes just ahead of incoming transactions, or purely accidental negative effects could occur due to the unfortunate timing of changes.

[Lines 541 - 631](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L541-L631)

## TYPO ERRORS
`override` should be changed to `overridden`
[Line 232](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L232)

`balacne` should be changed to `balance`
[Line 292](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L292)

`ot` should be changed to `to`
[Line 366]((https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L366)

## USE OF BLOCK.TIMESTAMP
Some contract code uses the block.timestamp as part of the calculations and time checks. Nevertheless, timestamps can be slightly altered by miners/validators to favor them in contracts that have logics that depend strongly on them.

Consider taking into account this issue and warning the users that such a scenario could happen. If the alteration of timestamps cannot affect the protocol in any way, consider documenting the reasoning and writing tests enforcing that these guarantees will be preserved even if the code changes in the future. Here are eight instances found.

[Line 229](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L229)
[Line 237](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L237)
[Line 319](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L319)
[Line 380](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L380)
[Line 426](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L426)
[Line 430](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L430)
[Line 463](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L463)
[Line 496](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L496)

## USE OF TIME UNITS
As documented in the link below:

https://docs.soliditylang.org/en/v0.8.14/units-and-global-variables.html

The following instance of constant should be assigned in an error free manner utilizing as much of the time units as possible:

[Line 24](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L24)

```
    uint256 public constant WEEK = 1 weeks;
```
## EVENT PARAMETERS SHOULD BE INDEXED
Up to three event parameters should be indexed. This will help filter off the logs in listening for specifically wanted data. Using `indexed` has the benefit of making the arguments log topics instead of data. There are two instances found.

[Lines 85 - 92](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L85-L92)
[Lines 115 - 119](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L115-L119)

## DOS ON UNBOUNDED LOOP
Unbounded loop could lead to OOG (Out of Gas) denying the users' of needed services. Here is one instance found.

[Line 547](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L547)

## SINGLE POINT OF FAILURE
Contract owner possesses numerous privileges that could prove disastrous if the private key was compromised. If the key was lost/unrecoverable, all needed system parameter updates would be halted.

An established owner is generally prone to target attack, especially given the insufficient protection on sensitive owner private keys. The concentration of privileges creates a single point of failure, leading to transfer of ownership and malicious reset of setter function parameters.

The following measures are recommended.
1) Split privileges using multisig option making sure that no one address has excessive ownership of the system.
2) Clearly document the functions and implementations the owner can change.
3) Document the risks associated with privileged users and single points of failure.
4) Make sure that users are aware of all the risks associated with the system.

## SETTING TO DEFAULT VALUES
As documented in the link:

https://docs.soliditylang.org/en/v0.8.17/types.html?highlight=delete#delete

It is recommended using `delete` whenever there is a need to set state variable to its default value, which has a good side effect of saving gas. There are three instances found.

[Line 473](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L473)
[Line 506](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L506)
[Line 589](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L589)

## CONSTANT VISIBILITY FOR UNCHANGING STATE VARIABLE
The public state variable, `minDelegationTime`, should be declared as a constant. 

[Line 79](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L79)

Apparently, it has only been used in the following two conditional instances with no state changes intended.

[Line 320](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L320)
[Line 386](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L386)