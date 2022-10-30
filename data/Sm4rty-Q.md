
## NC-01 Missing Natspec at several Places
There are several instances of incomplete or missing natspec at manay places over the contract
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L125-L136
```
    /**
    * @dev Creates the contract, set the given base parameters
    * @param _votingEscrow address of the voting token to delegate
    * @param _delegationBoost address of the contract handling delegation
    * @param _minTargetVotes min amount of veToken to target in a Pledge // @audit missing natspec _chestAddress 
    */
    constructor(
        address _votingEscrow,
        address _delegationBoost,
        address _chestAddress,
        uint256 _minTargetVotes
```
For all events natspec are incomplete:
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L84-L120
```
    // Events

    /** @notice Event emitted when xx */
    event NewPledge(
        address creator,
        address receiver,
        address rewardToken,
        uint256 targetVotes,
        uint256 rewardPerVote,
        uint256 endTimestamp
    );
    /** @notice Event emitted when xx */
    event ExtendPledgeDuration(uint256 indexed pledgeId, uint256 oldEndTimestamp, uint256 newEndTimestamp);
    /** @notice Event emitted when xx */
    event IncreasePledgeTargetVotes(uint256 indexed pledgeId, uint256 oldTargetVotes, uint256 newTargetVotes);
    /** @notice Event emitted when xx */
    event IncreasePledgeRewardPerVote(uint256 indexed pledgeId, uint256 oldRewardPerVote, uint256 newRewardPerVote);
    /** @notice Event emitted when xx */
    event ClosePledge(uint256 indexed pledgeId);
    /** @notice Event emitted when xx */
    event RetrievedPledgeRewards(uint256 indexed pledgeId, address receiver, uint256 amount);

    /** @notice Event emitted when xx */
    event Pledged(uint256 indexed pledgeId, address indexed user, uint256 amount, uint256 endTimestamp);

    /** @notice Event emitted when xx */
    event NewRewardToken(address indexed token, uint256 minRewardPerSecond);
    /** @notice Event emitted when xx */
    event UpdateRewardToken(address indexed token, uint256 minRewardPerSecond);
    /** @notice Event emitted when xx */
    event RemoveRewardToken(address indexed token);

    /** @notice Event emitted when xx */
    event ChestUpdated(address oldChest, address newChest);
    /** @notice Event emitted when xx */
    event PlatformFeeUpdated(uint256 oldfee, uint256 newFee);
    /** @notice Event emitted when xx */
    event MinTargetUpdated(uint256 oldMinTarget, uint256 newMinTargetVotes);
```

