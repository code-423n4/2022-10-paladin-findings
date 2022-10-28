# Event is missing `indexed` fields

### Index event fields make the field more quickly accessible to off-chain tools that parse events. However, note that each index field costs extra gas during emission, so it's not necessarily best to index the maximum allowed per event (three fields). Each `event` should use three `indexed` fields if there are three or more fields, and gas usage is not particularly of concern for the events in question. If there are fewer than three fields, all of the fields should be indexed.

### File: WardenPledge.sol

### Couple of examples: 

1.event ChestUpdated(address oldChest, address newChest);
1.https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L115

2.event PlatformFeeUpdated(uint256 oldfee, uint256 newFee);
2.https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L117

3.event MinTargetUpdated(uint256 oldMinTarget, uint256 newMinTargetVotes);
3.https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L119

================================

# Use solidity's native units for dealing with time

### Solidity provides some native units for dealing with time. We can use the following units: seconds, minutes, hours, days, weeks and years. These units will convert to a uint of the number of seconds in that specific length of time.

1.uint256 public constant WEEK = 7 * 86400;
1.https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L24

### Change it to either:
1.uint256 public constant WEEK = 7 days  OR  = 1 weeks;

================================

# THE NONREENTRANT MODIFIER SHOULD OCCUR BEFORE ALL OTHER MODIFIERS

1.function pledgePercent(uint256 pledgeId, uint256 percent, uint256 endTimestamp) external whenNotPaused nonReentrant {
1.https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L206

2.function pledge(uint256 pledgeId, uint256 amount, uint256 endTimestamp) external whenNotPaused nonReentrant {
2.https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L195

3.function retrievePledgeRewards(uint256 pledgeId, address receiver) external whenNotPaused nonReentrant {
3.https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L456

====================================

# Typo

### File: WardenPledge.sol

1.uint256 public protocalFeeRatio = 250;
1.https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L71

1.protocOl **


2.event PlatformFeeUpdated(uint256 oldfee, uint256 newFee);
2.https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L117

2.oldFee **


3.* @param targetVotes Maximum taget of votes to have (own balacne + delegation) for the receiver
3.https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L292

3.taRget **, balaNce **

4.* @param maxFeeAmount Maximum feeamount allowed ot be pulled by this contract
4.https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L296

4.fee amount **


5.// Add the total reards as available for the Pledge & write Pledge parameters in storage
5.https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L339

5.reWards **


6.* @param minRewardPerSecond Minmum amount of reward per vote per second for the token
6.https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L523

6.minImum **


7.* @param minRewardsPerSecond Minmum amount of reward per vote per second for each token in the list
7.https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L539

7.minImum **

8.* @param minRewardPerSecond Minmum amount of reward per vote per second for the token
8.https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L558
8.https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L568

8.minImum **


9.* @notice Updates the Platfrom fees BPS ratio
    * @dev Updates the Platfrom fees BPS ratio
9.https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L621

9.Platform **


