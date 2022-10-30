## [NAZ-L1] Missing Time locks
**Severity**: Low 
**Context**: [`WardenPledge.sol#L541`](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L541), [`WardenPledge.sol#L560`](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L560), [`WardenPledge.sol#L570`](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L570), [`WardenPledge.sol#L585`](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L585), [`WardenPledge.sol#L599`](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L599), [`WardenPledge.sol#L612`](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L612), [`WardenPledge.sol#L625`](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L625)

**Description**:
When critical parameters of systems need to be changed, it is required to broadcast the change via event emission and recommended to enforce the changes after a time-delay. This is to allow system users to be aware of such critical changes and give them an opportunity to exit or adjust their engagement with the system accordingly. None of the onlyOwner functions that change critical protocol addresses/parameters have a timelock for a time-delayed change to alert: (1) users and give them a chance to engage/exit protocol if they are not agreeable to the changes (2) team in case of compromised owner(s) and give them a chance to perform incident response.

**Recommendation**:
Users may be surprised when critical parameters are changed. Furthermore, it can erode users' trust since they can’t be sure the protocol rules won’t be changed later on. Compromised owner keys may be used to change protocol addresses/parameters to benefit attackers. Without a time-delay, authorized owners have no time for any planned incident response.


## [NAZ-L2] Missing Zero-address Validation
**Severity**: Low
**Context**: [`WardenPledge.sol#L131`](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L131)

**Description**:
Lack of zero-address validation on address parameters may lead to transaction reverts, waste gas, require resubmission of transactions and may even force contract redeployments in certain cases within the protocol.

**Recommendation**:
Consider adding explicit zero-address validation on input parameters of address type.


## [NAZ-N1] Function && Variable Naming Convention
**Severity** Informational
**Context**: [`WardenPledge.sol#L665`](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L665)

**Description**:
The linked variables do not conform to the standard naming convention of Solidity whereby functions and variable names(local and state) utilize the `mixedCase` format unless variables are declared as `constant` in which case they utilize the `UPPER_CASE_WITH_UNDERSCORES` format. Private variables and functions should lead with an `_underscore`.

**Recommendation**:
Consider naming conventions utilized by the linked statements are adjusted to reflect the correct type of declaration according to the [Solidity style guide](https://docs.soliditylang.org/en/latest/style-guide.html). 


## [NAZ-N2] Line Length
**Severity**: Informational
**Context**: [`WardenPledge.sol#L206`](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L206), [`WardenPledge.sol#L234`](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L234), [`WardenPledge.sol#L245`](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#l245), [`WardenPledge.sol#L383`](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L383), [`WardenPledge.sol#L541`](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L541) 

**Description**:
Max line length must be no more than 120 but many lines are extended past this length.

**Recommendation**:
Consider cutting down the line length below 120.


## [NAZ-N3] Code Structure Deviates From Best-Practice
**Severity**: Informational
**Context**: [`WardenPledge.sol#L28`](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L28)

**Description**:
The best-practice layout for a contract should follow the following order: state variables, events, modifiers, constructor and functions. Function ordering helps readers identify which functions they can call and find constructor and fallback functions easier.  Functions should be grouped according to their visibility and ordered as: constructor, receive function (if exists), fallback function (if exists), external, public, internal, private. Functions should then further be ordered with view functions coming after the non-view labeled ones.

**Recommendation**:
Consider adopting recommended best-practice for code structure and layout.


## [NAZ-N4] Unindexed Event Parameters
**Severity** Informational
**Context**: [`WardenPledge.sol#L85`](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L85), [`WardenPledge.sol#L115-L119`](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L115-L119)

**Description**:
Parameters of certain events are expected to be indexed so that they’re included in the block’s bloom filter for faster access. Failure to do so might confuse off-chain tooling looking for such indexed events.

**Recommendation**:
Consider adding the indexed keyword to event parameters that should include it.


## [NAZ-N4] Use Underscores for Number Literals
**Severity**: Informational
**Context**: [`WardenPledge.sol#L23-L24`](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L23-L24)

**Description**:
There are multiple occasions where certain numbers have been hardcoded, either in variables or in the code itself. Large numbers can become hard to read.

**Recommendation**:
Consider using underscores for number literals to improve its readability.


## [NAZ-N5] Spelling Errors
**Severity**: Informational
**Context**: [`WardenPledge.sol#L71 (protocalFeeRatio => protocolFeeRatio)`](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L71), [`WardenPledge.sol#L292 (taget => target)`](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L292), [`WardenPledge.sol#L292 (balacne => balance)`](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L292), [`WardenPledge.sol#L328 (protocalFeeRatio => protocolFeeRatio)`](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L328), [`WardenPledge.sol#L339 (reards => rewards)`](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L339), [`WardenPledge.sol#L388 (protocalFeeRatio => protocolFeeRatio)`](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L388), [`WardenPledge.sol#L433 (protocalFeeRatio => protocolFeeRatio)`](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L433), [`WardenPledge.sol#L523 (Minmum => Minimum)`](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L523), [`WardenPledge.sol#L539 (Minmum => Minimum)`](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L539), [`WardenPledge.sol#L558 (Minmum => Minimum)`](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L558), [`WardenPledge.sol#L568 (Minmum => Minimum)`](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L568), [`WardenPledge.sol#L621-L622 (Platfrom => Platform)`](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L621-L622), [`WardenPledge.sol#L627 (protocalFeeRatio => protocolFeeRatio)`](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L627)

**Description**:
Spelling errors in comments can cause confusion to both users and developers.

**Recommendation**:
Consider checking all misspellings to ensure they are corrected.


## [NAZ-N6] Older Version Pragma
**Severity**: Informational
**Context**: [`WardenPledge.sol`](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol)

**Description**:
Using very old versions of Solidity prevents benefits of bug fixes and newer security checks. Using the latest versions might make contracts susceptible to undiscovered compiler bugs. 

**Recommendation**:
Consider using the most recent version.