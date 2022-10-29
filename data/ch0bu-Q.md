# NON-CRITICAL

## 1. Use a more recent version of Solidity

 
- Use a solidity version of at least 0.8.12 to get `string.concat()` instead of `abi.encodePacked(<str>,<str>)`
- Use a solidity version of at least 0.8.13 to get the ability to use `using for` with a list of free functions

```
2	pragma solidity 0.8.10;
```
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol


## 2. The `nonReentrant modifier` should occur before all other modifiers

This is a best-practice to protect against reentrancy in other modifiers

```
195	function pledge(uint256 pledgeId, uint256 amount, uint256 endTimestamp) external whenNotPaused nonReentrant {
206	function pledgePercent(uint256 pledgeId, uint256 percent, uint256 endTimestamp) external whenNotPaused nonReentrant {
307	) external whenNotPaused nonReentrant returns(uint256){
373	) external whenNotPaused nonReentrant {
419	) external whenNotPaused nonReentrant {
456	function retrievePledgeRewards(uint256 pledgeId, address receiver) external whenNotPaused nonReentrant {
488	function closePledge(uint256 pledgeId, address receiver) external whenNotPaused nonReentrant {
```
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol


## 3. NatSpec is incomplete

Check here for NatSpec rules:
https://docs.soliditylang.org/en/develop/natspec-format.html


```
missing @param _chestAddress
131	constructor(
132		address _votingEscrow,
133		address _delegationBoost,
134		address _chestAddress,
135		uint256 _minTargetVotes
136	) {
```

## 4. Event is missing `indexed` fields

Index event fields make the field more quickly accessible to off-chain tools that parse events. Each `event` should use three `indexed` fields if there are three or more fields, and gas usage is not particularly of concern for the events in question. If there are fewer than three fields, all of the fields should be indexed.

```
94	event ExtendPledgeDuration(uint256 indexed pledgeId, uint256 oldEndTimestamp, uint256 newEndTimestamp);
96	event IncreasePledgeTargetVotes(uint256 indexed pledgeId, uint256 oldTargetVotes, uint256 newTargetVotes);
98	event IncreasePledgeRewardPerVote(uint256 indexed pledgeId, uint256 oldRewardPerVote, uint256 newRewardPerVote);
102	event RetrievedPledgeRewards(uint256 indexed pledgeId, address receiver, uint256 amount);
105	event Pledged(uint256 indexed pledgeId, address indexed user, uint256 amount, uint256 endTimestamp);
108	event NewRewardToken(address indexed token, uint256 minRewardPerSecond);
110	event UpdateRewardToken(address indexed token, uint256 minRewardPerSecond);
115	event ChestUpdated(address oldChest, address newChest);
117	event PlatformFeeUpdated(uint256 oldfee, uint256 newFee);
119	event MinTargetUpdated(uint256 oldMinTarget, uint256 newMinTargetVotes);
```
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol


## 5. Numeric values having to do with time should use time uints for readability

There are units for seconds, minutes, hours, days, and weeks
- https://docs.soliditylang.org/en/latest/units-and-global-variables.html#time-units

```
24	uint256 public constant WEEK = 7 * 86400;
```
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol
