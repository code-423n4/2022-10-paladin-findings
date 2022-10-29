## Summary<a name="Summary">

### Low Risk Issues
| |Issue|Contexts|
|-|:-|:-:|
| [LOW&#x2011;1](#LOW&#x2011;1) | Missing parameter validation in `constructor` | 4 |

Total: 4 contexts over 1 issues

### Non-critical Issues
| |Issue|Contexts|
|-|:-|:-:|
| [NC&#x2011;1](#NC&#x2011;1) | Use a more recent version of Solidity | 1 |
| [NC&#x2011;2](#NC&#x2011;2) | Event Is Missing Indexed Fields | 4 |
| [NC&#x2011;3](#NC&#x2011;3) | Implementation contract may not be initialized | 1 |
| [NC&#x2011;4](#NC&#x2011;4) | Unused event may be unused code or indicative of missed emit/logic | 1 |
| [NC&#x2011;5](#NC&#x2011;5) | Non-usage of specific imports | 8 |
| [NC&#x2011;6](#NC&#x2011;6) | Lines are too long | 2 |

Total: 17 contexts over 6 issues

## Low Risk Issues


### <a href="#Summary">[LOW&#x2011;1]</a><a name="LOW&#x2011;1"> Missing parameter validation in `constructor`

Some parameters of constructors are not checked for invalid values.

#### <ins>Proof Of Concept</ins>

```
132: constructor(
133:    address _votingEscrow,
134:    address _delegationBoost,
135:    address _chestAddress,
136:    uint256 _minTargetVotes
137: ) {
```

https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L132-L137




#### <ins>Recommended Mitigation Steps</ins>

Validate the parameters.



## Non Critical Issues

### <a href="#Summary">[NC&#x2011;1]</a><a name="NC&#x2011;1"> Use a more recent version of Solidity

Use a solidity version of at least 0.8.12 to get string.concat() instead of abi.encodePacked(<str>,<str>)
Use a solidity version of at least 0.8.13 to get the ability to use using for with a list of free functions

#### <ins>Proof Of Concept</ins>


```
pragma solidity 0.8.10;
```

https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L2



#### <ins>Recommended Mitigation Steps</ins>

Consider updating to a more recent solidity version.



### <a href="#Summary">[NC&#x2011;2]</a><a name="NC&#x2011;2"> Event Is Missing Indexed Fields

Index event fields make the field more quickly accessible to off-chain tools that parse events. However, note that each index field costs extra gas during emission, so it's not necessarily best to index the maximum allowed per event (three fields). 

Each event should use three indexed fields if there are three or more fields, and gas usage is not particularly of concern for the events in question. If there are fewer than three fields, all of the fields should be indexed.

#### <ins>Proof Of Concept</ins>


```
event NewPledge(
        address creator,
        address receiver,
        address rewardToken,
        uint256 targetVotes,
        uint256 rewardPerVote,
        uint256 endTimestamp
    );
```

https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L85

```
event ChestUpdated(address oldChest, address newChest);
```

https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L115

```
event PlatformFeeUpdated(uint256 oldfee, uint256 newFee);
```

https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L117

```
event MinTargetUpdated(uint256 oldMinTarget, uint256 newMinTargetVotes);
```

https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L119






### <a href="#Summary">[NC&#x2011;3]</a><a name="NC&#x2011;3"> Implementation contract may not be initialized

OpenZeppelin recommends that the initializer modifier be applied to constructors. 
Per OZs Post implementation contract should be initialized to avoid potential griefs or exploits.
https://forum.openzeppelin.com/t/uupsupgradeable-vulnerability-post-mortem/15680/5

#### <ins>Proof Of Concept</ins>


```
132: constructor(
133:    address _votingEscrow,
134:    address _delegationBoost,
135:    address _chestAddress,
136:    uint256 _minTargetVotes
137: ) {
```

https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L132-L137





### <a href="#Summary">[NC&#x2011;4]</a><a name="NC&#x2011;4"> Unused event may be unused code or indicative of missed emit/logic

Events that are declared but not used may be indicative of unused declarations where it makes sense to remove them for better readability/maintainability/auditability, or worse indicative of a missing emit which is bad for monitoring or missing logic that would have emitted that event.

#### <ins>Proof Of Concept</ins>


```
96: event IncreasePledgeTargetVotes
```

https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L96



#### <ins>Recommended Mitigation Steps</ins>
Add emit or remove event declaration.



### <a href="#Summary">[NC&#x2011;5]</a><a name="NC&#x2011;5"> Non-usage of specific imports

The current form of relative path import is not recommended for use because it can unpredictably pollute the namespace.
Instead, the Solidity docs recommend specifying imported symbols explicitly.
https://docs.soliditylang.org/en/v0.8.15/layout-of-source-files.html#importing-other-source-files

#### <ins>Proof Of Concept</ins>


```
4: import "./oz/interfaces/IERC20.sol";
5: import "./oz/libraries/SafeERC20.sol";
6: import "./utils/Owner.sol";
7: import "./oz/utils/Pausable.sol";
8: import "./oz/utils/ReentrancyGuard.sol";
9: import "./interfaces/IVotingEscrow.sol";
10: import "./interfaces/IBoostV2.sol";
11: import "./utils/Errors.sol";

https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L4-L11




#### <ins>Recommended Mitigation Steps</ins>

Use specific imports syntax per solidity docs recommendation.



### <a href="#Summary">[NC&#x2011;6]</a><a name="NC&#x2011;6"> Lines are too long

Usually lines in source code are limited to 80 characters. Today's screens are much larger so it's reasonable to stretch this in some cases. Since the files will most likely reside in GitHub, and GitHub starts using a scroll bar in all cases when the length is over 164 characters, the lines below should be split when they reach that length
Reference: https://docs.soliditylang.org/en/v0.8.10/style-guide.html#maximum-line-length

#### <ins>Proof Of Concept</ins>

```
239: // Check that the user has enough boost delegation available & set the correct allowance to this contract
```

https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L239

```
261: // based on the Boost bias & the Boost duration, to take in account that the delegated amount decreases
```

https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L261





