### G01: COMPARISONS WITH ZERO FOR UNSIGNED INTEGERS
#### problem
0 is less gas efficient than !0 if you enable the optimizer at 10k AND you’re in a require statement. Detailed explanation with the opcodes https://twitter.com/gzeon/status/1485428085885640706
#### prof
WardenPledge.sol, 471, b'        if(remainingAmount > 0) {'
WardenPledge.sol, 504, b'        if(remainingAmount > 0) {'

### G02: X += Y COSTS MORE GAS THAN X = X + Y FOR STATE VARIABLES
#### prof
WardenPledge.sol, 268, b'        pledgeAvailableRewardAmounts[pledgeId] -= rewardAmount;'
WardenPledge.sol, 340, b'        pledgeAvailableRewardAmounts[vars.newPledgeID] += vars.totalRewardAmount;'
WardenPledge.sol, 401, b'        pledgeAvailableRewardAmounts[pledgeId] += totalRewardAmount;'
WardenPledge.sol, 445, b'        pledgeAvailableRewardAmounts[pledgeId] += totalRewardAmount;'

### G03: resign the default value to the variables.
#### problem
 resign the default value to the variables will cost more gas.
#### prof
WardenPledge.sol, 547, b'        for(uint256 i = 0; i < length;){'

### G04: FUNCTIONS GUARANTEED TO REVERT WHEN CALLED BY NORMAL USERS CAN BE MARKED PAYABLE
#### problem
If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are CALLVALUE(2),DUP1(3),ISZERO(3),PUSH2(3),JUMPI(10),PUSH1(3),DUP1(3),REVERT(0),JUMPDEST(1),POP(2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost
#### prof
WardenPledge.sol, 165, b'    function getUserPledges(address user) external view returns(uint256[] memory)'
WardenPledge.sol, 358, b'    function createPledge(\n        address receiver,\n        address rewardToken,\n        uint256 targetVotes,\n        uint256 rewardPerVote, // reward/veToken/second\n        uint256 endTimestamp,\n        uint256 maxTotalRewardAmount,\n        uint256 maxFeeAmount\n    ) external whenNotPaused nonReentrant returns(uint256)'
WardenPledge.sol, 404, b'    function extendPledge(\n        uint256 pledgeId,\n        uint256 newEndTimestamp,\n        uint256 maxTotalRewardAmount,\n        uint256 maxFeeAmount\n    ) external whenNotPaused nonReentrant '
WardenPledge.sol, 448, b'    function increasePledgeRewardPerVote(\n        uint256 pledgeId,\n        uint256 newRewardPerVote,\n        uint256 maxTotalRewardAmount,\n        uint256 maxFeeAmount\n    ) external whenNotPaused nonReentrant '
WardenPledge.sol, 480, b'    function retrievePledgeRewards(uint256 pledgeId, address receiver) external whenNotPaused nonReentrant '
WardenPledge.sol, 515, b'    function closePledge(uint256 pledgeId, address receiver) external whenNotPaused nonReentrant '
WardenPledge.sol, 552, b'    function addMultipleRewardToken(address[] calldata tokens, uint256[] calldata minRewardsPerSecond) external onlyOwner '
WardenPledge.sol, 552, b'    function addMultipleRewardToken(address[] calldata tokens, uint256[] calldata minRewardsPerSecond) external onlyOwner '
WardenPledge.sol, 562, b'    function addRewardToken(address token, uint256 minRewardPerSecond) external onlyOwner '
WardenPledge.sol, 562, b'    function addRewardToken(address token, uint256 minRewardPerSecond) external onlyOwner '
WardenPledge.sol, 578, b'    function updateRewardToken(address token, uint256 minRewardPerSecond) external onlyOwner '
WardenPledge.sol, 578, b'    function updateRewardToken(address token, uint256 minRewardPerSecond) external onlyOwner '
WardenPledge.sol, 592, b'    function removeRewardToken(address token) external onlyOwner '
WardenPledge.sol, 592, b'    function removeRewardToken(address token) external onlyOwner '
WardenPledge.sol, 605, b'    function updateChest(address chest) external onlyOwner '
WardenPledge.sol, 605, b'    function updateChest(address chest) external onlyOwner '
WardenPledge.sol, 618, b'    function updateMinTargetVotes(uint256 newMinTargetVotes) external onlyOwner '
WardenPledge.sol, 618, b'    function updateMinTargetVotes(uint256 newMinTargetVotes) external onlyOwner '
WardenPledge.sol, 631, b'    function updatePlatformFee(uint256 newFee) external onlyOwner '
WardenPledge.sol, 631, b'    function updatePlatformFee(uint256 newFee) external onlyOwner '
WardenPledge.sol, 638, b'    function pause() external onlyOwner '
WardenPledge.sol, 638, b'    function pause() external onlyOwner '
WardenPledge.sol, 645, b'    function unpause() external onlyOwner '
WardenPledge.sol, 645, b'    function unpause() external onlyOwner '
WardenPledge.sol, 661, b'    function recoverERC20(address token) external onlyOwner returns(bool) '
WardenPledge.sol, 661, b'    function recoverERC20(address token) external onlyOwner returns(bool) '

### G05: USING PRIVATE RATHER THAN PUBLIC FOR CONSTANTS, SAVES GAS
#### problem:
We can save getter function of public constants.
#### prof:
WardenPledge.sol, 22, b'    uint256 public constant UNIT = 1e18;'
WardenPledge.sol, 23, b'    uint256 public constant MAX_PCT = 10000;'
WardenPledge.sol, 24, b'    uint256 public constant WEEK = 7 * 86400;'

### G06: USE A MORE RECENT VERSION OF SOLIDITY
WardenPledge.sol, 2, b'pragma solidity 0.8.10;'

### G07: USAGE OF UINTS/INTS SMALLER THAN 32 BYTES (256 BITS) INCURS OVERHEAD
#### problem
When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.
#### prof
WardenPledge.sol, 224, b'        if(amount == 0) revert Errors.NullValue();'
WardenPledge.sol, 229, b'        if(pledgeParams.endTimestamp <= block.timestamp) revert Errors.ExpiredPledge();'
WardenPledge.sol, 233, b'        if(endTimestamp == 0) endTimestamp = pledgeParams.endTimestamp;'
WardenPledge.sol, 234, b'        if(endTimestamp > pledgeParams.endTimestamp || endTimestamp != _getRoundedTimestamp(endTimestamp)) revert Errors.InvalidEndTimestamp();'
WardenPledge.sol, 263, b'        uint256 totalDelegatedAmount = ((bias * boostDuration) + bias) / 2;'
WardenPledge.sol, 310, b'        if(receiver == address(0) || rewardToken == address(0)) revert Errors.ZeroAddress();'
WardenPledge.sol, 310, b'        if(receiver == address(0) || rewardToken == address(0)) revert Errors.ZeroAddress();'
WardenPledge.sol, 312, b'        if(minAmountRewardToken[rewardToken] == 0) revert Errors.TokenNotWhitelisted();'
WardenPledge.sol, 315, b'        if(endTimestamp == 0) revert Errors.NullEndTimestamp();'
...
WardenPledge.sol, 666, b'        if(n > type(uint64).max) revert Errors.NumberExceed64Bits();'
WardenPledge.sol, 666, b'        if(n > type(uint64).max) revert Errors.NumberExceed64Bits();'
WardenPledge.sol, 666, b'        if(n > type(uint64).max) revert Errors.NumberExceed64Bits();'
WardenPledge.sol, 667, b'        return uint64(n);'
WardenPledge.sol, 667, b'        return uint64(n);'