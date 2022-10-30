## Time Units
According to:

https://docs.soliditylang.org/en/v0.8.14/units-and-global-variables.html

suffixes like `seconds`, `minutes`, `hours`, `days` and `weeks` after literal numbers can be used to specify units of time where seconds are the base unit and units are considered naively in the following way:

1 == 1 seconds
1 minutes == 60 seconds
1 hours == 60 minutes
1 days == 24 hours
1 weeks == 7 days

To avoid human error while making the assignment more verbose, the following constant declarations may respectively be rewritten as:

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L24

```
    uint256 public constant WEEK = 1 weeks;
```
## `block.timestamp` Unreliable
The use of `block.timestamp` as part of the time checks can be slightly altered by miners/validators to favor them in contracts that have logic strongly dependent on them.

Consider taking into account this issue and warning the users that such a scenario could happen. If the alteration of timestamps cannot affect the protocol in any way, consider documenting the reasoning and writing tests enforcing that these guarantees will be preserved even if the code changes in the future.

There are a total of eight instances entailed:

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L229
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L237
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L319
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L380
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L426
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L430
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L463
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L496

## Sanity Checks Upon Deployment
Zero address and zero value checks should be implemented at the constructor to avoid human error(s) that could result in non-functional calls associated with them. The constructor should be refactored to:

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L131-L143

```
    constructor(
        address _votingEscrow,
        address _delegationBoost,
        address _chestAddress,
        uint256 _minTargetVotes
    ) {
        if (_votingEscrow == address(0)|| _delegationBoost == address(0) || _chestAddress == address(0)) revert Errors.ZeroAddress();
        if(_minTargetVotes == 0) revert Errors.InvalidValue();
        
        votingEscrow = IVotingEscrow(_votingEscrow);
        delegationBoost = IBoostV2(_delegationBoost);

        chestAddress = _chestAddress;

        minTargetVotes = _minTargetVotes;
    }
```
Doing this will be consistent with the sanity checks implemented in the setter functions,  `updateChest()` and `updateMinTargetVotes()`.

Better yet, incorporate complementary codehash checks too just to make sure the address inputs are the matching ones if these have not been deemed an overkill from the developer team's perspective.

## Add a Timelock to Critical Parameter Change
It is a good practice to give time for users to react and adjust to critical changes with a mandatory time window between them. The first step merely broadcasts to users that a particular change is coming, and the second step commits that change after a suitable waiting period. This allows users that do not accept the change to withdraw within the grace period. A timelock provides more guarantees and reduces the level of trust required, thus decreasing risk for users. It also indicates that the project is legitimate (less risk of a malicious Owner making any malicious or ulterior intention). Specifically, privileged roles could use front running to make malicious changes just ahead of incoming transactions, or purely accidental negative effects could occur due to the unfortunate timing of changes. 

Here are the instances entailed:

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L541-L645

## Contract Owner Has Too Many Privileges
The owner of the contracts has too many privileges relative to standard users. The consequence is disastrous if the contract owner's private key has been compromised. And, in the event the key was lost or unrecoverable, no implementation upgrades and system parameter updates will ever be possible.

For a project this grand, it increases the likelihood that the owner will be targeted by an attacker, especially given the insufficient protection on sensitive owner private keys. The concentration of privileges creates a single point of failure; and, here are some of the incidents that could possibly transpire:

Transfer ownership and mess up with all the setter functions, hijacking the entire protocol.

Consider:
1) splitting privileges (e.g. via the multisig option) to ensure that no one address has excessive ownership of the system,
2) clearly documenting the functions and implementations the owner can change,
3) documenting the risks associated with privileged users and single points of failure, and
4) ensuring that users are aware of all the risks associated with the system.

## Typo Errors
```
            @ balacne
Line 292    * @param targetVotes Maximum taget of votes to have (own balacne + delegation) for the receiver
            @ ot
Line 365    * @param maxTotalRewardAmount Maximum added total reward amount allowed ot be pulled by this contract
            @ ot
Line 366    * @param maxFeeAmount Maximum fee amount allowed ot be pulled by this contract
```
## Function Calls in Loop Could Lead to Denial of Service
Function calls made in unbounded loop are error-prone with potential resource exhaustion as it can trap the contract due to gas limitations or failed transactions. Here is the one instance entailed:

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L547

Consider bounding the loop where possible to avoid unnecessary gas wastage and denial of service. 

## Non-Changing State Variable Should be Declared as Constant
`minDelegationTime` should be declared constant and capitalized considering it is not meant to be update in the contract codebase.

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L79

It has only be used in the following two conditional statements:

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L320
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L386

## Use `delete` to Clear Variables
`delete a` assigns the initial value for the type to `a`. i.e. for integers it is equivalent to `a = 0`, but it can also be used on arrays, where it assigns a dynamic array of length zero or a static array of the same length with all elements reset. For structs, it assigns a struct with all members reset. Similarly, it can also be used to set an address to zero address. It has no effect on whole mappings though (as the keys of mappings may be arbitrary and are generally unknown). However, individual keys and what they map to can be deleted: If `a` is a mapping, then `delete a[x]` will delete the value stored at x.

The delete key better conveys the intention and is also more idiomatic. Consider replacing assignments of zero with delete statements. Here are some the three instances entailed:

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L473
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L506
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L589

## Un-indexed Parameters in Events
Consider indexing parameters for events, serving as logs filter when looking for specifically wanted data. Up to three parameters in an event function can receive the attribute `indexed` which will cause the respective arguments to be treated as log topics instead of data. There are the instances entailed:

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L85-L92
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L115-L119