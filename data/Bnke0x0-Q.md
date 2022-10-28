

### [L01] Missing checks for `address(0x0)` when assigning values to `address` state variables


#### Findings:
```
2022-10-paladin/contracts/WardenPledge.sol::140 => chestAddress = _chestAddress;
2022-10-paladin/contracts/WardenPledge.sol::142 => minTargetVotes = _minTargetVotes;
```


#### Non-Critical Issues



### [N01] Adding a return statement when the function defines a named return variable, is redundant


#### Findings:
```

2022-10-paladin/contracts/WardenPledge.sol::173 => return pledges;
```



### [N02] constants should be defined rather than using magic numbers


#### Findings:
```
2022-10-paladin/contracts/WardenPledge.sol::24 => uint256 public constant WEEK = 7 * 86400;
```


### [N03] Use a more recent version of solidity

#### Impact
Use a solidity version of at least 0.8.13 to get the ability to use using for with a list of free functions
#### Findings:
```
2022-10-paladin/contracts/WardenPledge.sol::2 => pragma solidity 0.8.10;
```



### [N04] Event is missing `indexed` fields

#### Impact
Each event should use three indexed fields if there are three or more fields
#### Findings:
```
2022-10-paladin/contracts/WardenPledge.sol::94 => event ExtendPledgeDuration(uint256 indexed pledgeId, uint256 oldEndTimestamp, uint256 newEndTimestamp);
2022-10-paladin/contracts/WardenPledge.sol::96 => event IncreasePledgeTargetVotes(uint256 indexed pledgeId, uint256 oldTargetVotes, uint256 newTargetVotes);
2022-10-paladin/contracts/WardenPledge.sol::98 => event IncreasePledgeRewardPerVote(uint256 indexed pledgeId, uint256 oldRewardPerVote, uint256 newRewardPerVote);
2022-10-paladin/contracts/WardenPledge.sol::102 => event RetrievedPledgeRewards(uint256 indexed pledgeId, address receiver, uint256 amount);
2022-10-paladin/contracts/WardenPledge.sol::105 => event Pledged(uint256 indexed pledgeId, address indexed user, uint256 amount, uint256 endTimestamp);
2022-10-paladin/contracts/WardenPledge.sol::108 => event NewRewardToken(address indexed token, uint256 minRewardPerSecond);
2022-10-paladin/contracts/WardenPledge.sol::110 => event UpdateRewardToken(address indexed token, uint256 minRewardPerSecond);
2022-10-paladin/contracts/WardenPledge.sol::112 => event RemoveRewardToken(address indexed token);
2022-10-paladin/contracts/WardenPledge.sol::115 => event ChestUpdated(address oldChest, address newChest);
2022-10-paladin/contracts/WardenPledge.sol::117 => event PlatformFeeUpdated(uint256 oldfee, uint256 newFee);
2022-10-paladin/contracts/WardenPledge.sol::119 => event MinTargetUpdated(uint256 oldMinTarget, uint256 newMinTargetVotes);
```



### [N05] Use of sensitive/NC-inclusive terms


#### Findings:
```
2022-10-paladin/contracts/WardenPledge.sol::43 => bool closed;
```

### [N06] Unused file

#### Findings:
```
2022-10-paladin/contracts/WardenPledge.sol::1 => // SPDX-License-Identifier: MIT
```



### [N07] `public` functions not called by the contract should be declared `external` instead

#### Impact
Contracts are allowed to override their parents’ functions and change the visibility from public to external .
#### Findings:
```
2022-10-paladin/contracts/WardenPledge.sol::153 => function pledgesIndex() public view returns(uint256){
2022-10-paladin/contracts/WardenPledge.sol::163 => function getUserPledges(address user) external view returns(uint256[] memory){
2022-10-paladin/contracts/WardenPledge.sol::172 => function getAllPledges() external view returns(Pledge[] memory){
2022-10-paladin/contracts/WardenPledge.sol::181 => function _getRoundedTimestamp(uint256 timestamp) internal pure returns(uint256) {
2022-10-paladin/contracts/WardenPledge.sol::195 => function pledge(uint256 pledgeId, uint256 amount, uint256 endTimestamp) external whenNotPaused nonReentrant {
2022-10-paladin/contracts/WardenPledge.sol::206 => function pledgePercent(uint256 pledgeId, uint256 percent, uint256 endTimestamp) external whenNotPaused nonReentrant {
2022-10-paladin/contracts/WardenPledge.sol::222 => function _pledge(uint256 pledgeId, address user, uint256 amount, uint256 endTimestamp) internal {
2022-10-paladin/contracts/WardenPledge.sol::299 => function createPledge(
2022-10-paladin/contracts/WardenPledge.sol::368 => function extendPledge(
2022-10-paladin/contracts/WardenPledge.sol::414 => function increasePledgeRewardPerVote(
2022-10-paladin/contracts/WardenPledge.sol::456 => function retrievePledgeRewards(uint256 pledgeId, address receiver) external whenNotPaused nonReentrant {
2022-10-paladin/contracts/WardenPledge.sol::488 => function closePledge(uint256 pledgeId, address receiver) external whenNotPaused nonReentrant {
2022-10-paladin/contracts/WardenPledge.sol::525 => function _addRewardToken(address token, uint256 minRewardPerSecond) internal {
2022-10-paladin/contracts/WardenPledge.sol::541 => function addMultipleRewardToken(address[] calldata tokens, uint256[] calldata minRewardsPerSecond) external onlyOwner {
2022-10-paladin/contracts/WardenPledge.sol::560 => function addRewardToken(address token, uint256 minRewardPerSecond) external onlyOwner {
2022-10-paladin/contracts/WardenPledge.sol::570 => function updateRewardToken(address token, uint256 minRewardPerSecond) external onlyOwner {
2022-10-paladin/contracts/WardenPledge.sol::585 => function removeRewardToken(address token) external onlyOwner {
2022-10-paladin/contracts/WardenPledge.sol::599 => function updateChest(address chest) external onlyOwner {
2022-10-paladin/contracts/WardenPledge.sol::612 => function updateMinTargetVotes(uint256 newMinTargetVotes) external onlyOwner {
2022-10-paladin/contracts/WardenPledge.sol::625 => function updatePlatformFee(uint256 newFee) external onlyOwner {
2022-10-paladin/contracts/WardenPledge.sol::636 => function pause() external onlyOwner {
2022-10-paladin/contracts/WardenPledge.sol::643 => function unpause() external onlyOwner {
2022-10-paladin/contracts/WardenPledge.sol::653 => function recoverERC20(address token) external onlyOwner returns(bool) {
2022-10-paladin/contracts/WardenPledge.sol::665 => function safe64(uint256 n) internal pure returns (uint64) {
```



### [N08] Numeric values having to do with time should use time units for readability

#### Impact
There are units for seconds, minutes, hours, days, and weeks
#### Findings:
```
2022-10-paladin/contracts/WardenPledge.sol::34 => // Price per vote per second, set by the owner
2022-10-paladin/contracts/WardenPledge.sol::79 => uint256 public minDelegationTime = 1 weeks;
2022-10-paladin/contracts/WardenPledge.sol::262 => // each second of the Boost duration
2022-10-paladin/contracts/WardenPledge.sol::303 => uint256 rewardPerVote, // reward/veToken/second
```

