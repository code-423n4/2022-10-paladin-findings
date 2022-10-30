1.

Grammer issues in comments in:

1.1

Contract: WardenPledge.sol

1.1.1

	line 32

	consider rephrasing "extend" to "extension" for better readability
	
Modified Comment:

	// (used for later extension/increase of some parameters the Pledge)

1.1.2

	line 232

	consider rephrasing "override" to "overridden" for better readability

Modified Comment:

	// so it's overridden by the Pledge's endTimestamp

1.1.3

	line 292

	"target" is misspelled as "target"

	"balance" is misspelled as "balacne"

Modified Comment:

	* @param targetVotes Maximum target of votes to have (own balance + delegation) for the receiver

1.1.4

	lines 295, 296, 365, 366, 411, and 412

	"to" is misspelled as "ot"

Modified Comments:

	* @param maxTotalRewardAmount Maximum total reward amount allowed to be pulled by this contract

	* @param maxFeeAmount Maximum feeamount allowed to be pulled by this contract

	* @param maxTotalRewardAmount Maximum added total reward amount allowed to be pulled by this contract

	* @param maxFeeAmount Maximum fee amount allowed to be pulled by this contract
	
	* @param maxTotalRewardAmount Maximum added total reward amount allowed to be pulled by this contract

	* @param maxFeeAmount Maximum fee amount allowed to be pulled by this contract

1.1.5

	lines 451, 454, 483, and 484

	"non distributed" should be hyphenated into "non-distributed"

Modified Comments:

	* @notice Retrieves all non-distributed rewards from a closed Pledge

	* @dev Retrieves all non-distributed rewards from a closed Pledge & send them to the given receiver

	* @notice Closes a Pledge and retrieves all non-distributed rewards from a Pledge

	* @dev Closes a Pledge and retrieves all non-distributed rewards from a Pledge & send them to the given receiver

1.1.6
	
	lines 453, and 485

	"for" is misspelled as "fo"

Modified Comments:

	* @param pledgeId ID for the Pledge

	* @param pledgeId ID for the Pledge to close

1.1.7

	lines 472, and 505

	"non used" should be hyphenated to "non-used"

Modified Comments:

	// Transfer the non-used rewards and reset storage

	// Transfer the non-used rewards and reset storage

1.1.8

	lines 523, 539, 558, and 568

	"Minimum" is misspelled as "Minmum"

Modified Comments:

	* @param minRewardPerSecond Minimum amount of reward per vote per second for the token

	* @param minRewardsPerSecond Minimum amount of reward per vote per second for each token in the list

	* @param minRewardPerSecond Minimum amount of reward per vote per second for the token

	* @param minRewardPerSecond Minimum amount of reward per vote per second for the token

1.1.9

	lines 621, and 622

	"Platform" is misspelled as "Platfrom"

Modified Comments:

	* @notice Updates the Platform fees BPS ratio

	* @dev Updates the Platform fees BPS ratio

1.1.10

	line 650

	"to" is misspelled as "tof"
	
	"ERC20" is misspelled as "EC20"

Modified Comment:

	* @param token Address to the ERC2O token