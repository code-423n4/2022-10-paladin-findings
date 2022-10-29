## none-critical issue foundings
### N01: EVENT IS MISSING INDEXED FIELDS
#### prof
WardenPledge.sol, 92, b'    event NewPledge(\n        address creator,\n        address receiver,\n        address rewardToken,\n        uint256 targetVotes,\n        uint256 rewardPerVote,\n        uint256 endTimestamp\n    );'
WardenPledge.sol, 115, b'    event ChestUpdated(address oldChest, address newChest);'
WardenPledge.sol, 117, b'    event PlatformFeeUpdated(uint256 oldfee, uint256 newFee);'
WardenPledge.sol, 119, b'    event MinTargetUpdated(uint256 oldMinTarget, uint256 newMinTargetVotes);'

### L01: INPUT ARRAY LENGTHS MAY DIFFER
#### problem
If the caller makes a copy-paste error, the lengths may be mismatchd and an operation believed to have been completed may not in fact have been completed
function withdrawMultipleERC721(address[] memory _tokens, uint256[] memory _tokenId, address _to) external override {
#### prof
WardenPledge.sol, 552, b'    function addMultipleRewardToken(address[] calldata tokens, uint256[] calldata minRewardsPerSecond) external onlyOwner '

### L02: address variable should check if it is zero MISSING CHECKS FOR ADDRESS(0X0) WHEN ASSIGNING VALUES TO ADDRESS STATE VARIABLES
#### prof
WardenPledge.sol, 137, b'        votingEscrow = IVotingEscrow(_votingEscrow);'
WardenPledge.sol, 138, b'        delegationBoost = IBoostV2(_delegationBoost);'
WardenPledge.sol, 140, b'        chestAddress = _chestAddress;'
WardenPledge.sol, 602, b'        chestAddress = chest;'