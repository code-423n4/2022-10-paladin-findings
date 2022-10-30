### [G-01] Functions guaranteed to revert when called by normal users can be declared as payable.


#### Impact
If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are CALLVALUE(2),DUP1(3),ISZERO(3),PUSH2(3),JUMPI(10),PUSH1(3),DUP1(3),REVERT(0),JUMPDEST(1),POP(2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost.


#### Findings:
```
contracts/WardenPledge.sol:L541    function addMultipleRewardToken(address[] calldata tokens, uint256[] calldata minRewardsPerSecond) external onlyOwner {

contracts/WardenPledge.sol:L560    function addRewardToken(address token, uint256 minRewardPerSecond) external onlyOwner {

contracts/WardenPledge.sol:L570    function updateRewardToken(address token, uint256 minRewardPerSecond) external onlyOwner {

contracts/WardenPledge.sol:L585    function removeRewardToken(address token) external onlyOwner {

contracts/WardenPledge.sol:L599    function updateChest(address chest) external onlyOwner {

contracts/WardenPledge.sol:L612    function updateMinTargetVotes(uint256 newMinTargetVotes) external onlyOwner {

contracts/WardenPledge.sol:L625    function updatePlatformFee(uint256 newFee) external onlyOwner {

contracts/WardenPledge.sol:L636    function pause() external onlyOwner {

contracts/WardenPledge.sol:L643    function unpause() external onlyOwner {

contracts/WardenPledge.sol:L653    function recoverERC20(address token) external onlyOwner returns(bool) {

```

### [G-02] Explicit initialization with zero not required


#### Impact
Explicit initialization with zero is not required for variable declaration because uints are 0 by default. Removing this will reduce contract size and save a bit of gas.


#### Findings:
```
contracts/WardenPledge.sol:L547        for(uint256 i = 0; i < length;){

```
