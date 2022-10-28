# Gas Optimizations

## [G-001] Functions guaranteed to revert when called by normal users can be marked payable

### Description:

If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided.

### Findings:

Total:10

[WardenPledge.sol#L541](https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L541)

```
541:    function addMultipleRewardToken(address[] calldata tokens, uint256[] calldata minRewardsPerSecond) external onlyOwner {
```

[WardenPledge.sol#L560](https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L560)

```
560:    function addRewardToken(address token, uint256 minRewardPerSecond) external onlyOwner {
```

[WardenPledge.sol#L570](https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L570)

```
570:    function updateRewardToken(address token, uint256 minRewardPerSecond) external onlyOwner {
```

[WardenPledge.sol#L585](https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L585)

```
585:    function removeRewardToken(address token) external onlyOwner {
```

[WardenPledge.sol#L599](https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L599)

```
599:    function updateChest(address chest) external onlyOwner {
```

[WardenPledge.sol#L612](https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L612)

```
612:    function updateMinTargetVotes(uint256 newMinTargetVotes) external onlyOwner {
```

[WardenPledge.sol#L625](https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L625)

```
625:    function updatePlatformFee(uint256 newFee) external onlyOwner {
```

[WardenPledge.sol#L636](https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L636)

```
636:    function pause() external onlyOwner {
```

[WardenPledge.sol#L643](https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L643)

```
643:    function unpause() external onlyOwner {
```

[WardenPledge.sol#L653](https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L653)

```
653:    function recoverERC20(address token) external onlyOwner returns(bool) {
```

## [G-002] internal function not called by the contract should be removed to save deployment gas

### Description:

If the functions are required by an interface, the contract should inherit from that interface and use the override keyword.

### Findings:

Total:3

[WardenPledge.sol#L181](https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L181)

```
181:    function _getRoundedTimestamp(uint256 timestamp) internal pure returns(uint256) {
```

[WardenPledge.sol#L222](https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L222)

```
222:    function _pledge(uint256 pledgeId, address user, uint256 amount, uint256 endTimestamp) internal {
```

[WardenPledge.sol#L525](https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L525)

```
525:    function _addRewardToken(address token, uint256 minRewardPerSecond) internal {
```