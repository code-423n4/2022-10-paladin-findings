## QA
### Missing checks for address(0x0) when assigning values to address state variables

https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L137-L140
```solidity
File: /contracts/WardenPledge.sol
137:        votingEscrow = IVotingEscrow(_votingEscrow);
138:        delegationBoost = IBoostV2(_delegationBoost);
140:        chestAddress = _chestAddress;
```

### Constants should be defined rather than using magic numbers
There are several occurrences of literal values with unexplained meaning .Literal values in the codebase without an explained meaning make the code harder to read, understand and maintain, thus hindering the experience of developers, auditors and external contributors alike.

Developers should define a constant variable for every magic value used , giving it a clear and self-explanatory name. 

https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L626
```solidity
File: /contracts/WardenPledge.sol

//@audit: 500
626:        if(newFee > 500) revert Errors.InvalidValue();
```

### Typos - see audit tags

https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L292
```solidity
File: /contracts/WardenPledge.sol

//@audit: **balacne** instead of **balance*
292:    * @param targetVotes Maximum taget of votes to have (own balacne + delegation) for the receiver
```

https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L365
```solidity
File: /contracts/WardenPledge.sol

//@audit: **ot** instead of **to**
295:    * @param maxTotalRewardAmount Maximum total reward amount allowed ot be pulled by this contract
296:    * @param maxFeeAmount Maximum feeamount allowed ot be pulled by this contract
365:    * @param maxTotalRewardAmount Maximum added total reward amount allowed ot be pulled by this contract
366:    * @param maxFeeAmount Maximum fee amount allowed ot be pulled by this contract
411:    * @param maxTotalRewardAmount Maximum added total reward amount allowed ot be pulled by this contract
412:    * @param maxFeeAmount Maximum fee amount allowed ot be pulled by this contract
```

https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L523
```solidity
File: /contracts/WardenPledge.sol

//@audit: **Minmum** instead of **Minimum**
523:    * @param minRewardPerSecond Minmum amount of reward per vote per second for the token
```
https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L539
```solidity
File: /contracts/WardenPledge.sol

//@audit: **Minmum** instead of **Minimum**
539:    * @param minRewardsPerSecond Minmum amount of reward per vote per second for each token in the list
```
https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L558
```solidity
File: /contracts/WardenPledge.sol

//@audit: **Minmum** instead of **Minimum**
558:    * @param minRewardPerSecond Minmum amount of reward per vote per second for the token
```

https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L568
```solidity
File: /contracts/WardenPledge.sol
//@audit: **Minmum** instead of **Minimum**
568:    * @param minRewardPerSecond Minmum amount of reward per vote per second for the token
```
https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L621-L622
```solidity
File: /contracts/WardenPledge.sol

//@audit: Platfrom instead of Platform
621:    * @notice Updates the Platfrom fees BPS ratio
622:     * @dev Updates the Platfrom fees BPS ratio
```

https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L650
```solidity
File: /contracts/WardenPledge.sol
//@audit: **tof** instead of **of**
650:    * @param token Address tof the EC2O token
```

### Natspec is incomplete
https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L125-L136
```solidity
File: /contracts/WardenPledge.sol

//@audit: Missing @param _chestAddress

125:    /**
126:    * @dev Creates the contract, set the given base parameters
127:    * @param _votingEscrow address of the voting token to delegate
128:    * @param _delegationBoost address of the contract handling delegation
129:    * @param _minTargetVotes min amount of veToken to target in a Pledge
130:    */
131:    constructor(
132:        address _votingEscrow,
133:        address _delegationBoost,
134:        address _chestAddress,
135:        uint256 _minTargetVotes
136:    ) {
```

### Event is missing `indexed` fields

Index event fields make the field more quickly accessible to off-chain tools that parse events. However, note that each index field costs extra gas during emission, so it's not necessarily best to index the maximum allowed per event (three fields). Each `event` should use three `indexed` fields if there are three or more fields, and gas usage is not particularly of concern for the events in question. If there are fewer than three fields, all of the fields should be indexed.

https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L85-L92
```solidity
File: /contracts/WardenPledge.sol
85:    event NewPledge(
86:        address creator,
87:        address receiver,
88:        address rewardToken,
89:        uint256 targetVotes,
90:        uint256 rewardPerVote,
91:        uint256 endTimestamp
92:    );


115:    event ChestUpdated(address oldChest, address newChest);
116:    /** @notice Event emitted when xx */
117:    event PlatformFeeUpdated(uint256 oldfee, uint256 newFee);
118:    /** @notice Event emitted when xx */
119:    event MinTargetUpdated(uint256 oldMinTarget, uint256 newMinTargetVotes);
```

### The nonReentrant modifier should occur before all other modifiers
This is a best-practice to protect against reentrancy in other modifiers

https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L195
```solidity
File: /contracts/WardenPledge.sol
195:    function pledge(uint256 pledgeId, uint256 amount, uint256 endTimestamp) external whenNotPaused nonReentrant {

206:    function pledgePercent(uint256 pledgeId, uint256 percent, uint256 endTimestamp) external whenNotPaused nonReentrant {

299:    function createPledge(
300:        address receiver,
301:        address rewardToken,
302:        uint256 targetVotes,
303:        uint256 rewardPerVote, // reward/veToken/second
304:        uint256 endTimestamp,
305:        uint256 maxTotalRewardAmount,
306:        uint256 maxFeeAmount
307:    ) external whenNotPaused nonReentrant returns(uint256){


368:    function extendPledge(
369:        uint256 pledgeId,
370:        uint256 newEndTimestamp,
371:        uint256 maxTotalRewardAmount,
372:        uint256 maxFeeAmount
373:    ) external whenNotPaused nonReentrant {


414:    function increasePledgeRewardPerVote(
415:        uint256 pledgeId,
416:        uint256 newRewardPerVote,
417:        uint256 maxTotalRewardAmount,
418:        uint256 maxFeeAmount
419:    ) external whenNotPaused nonReentrant {


456:    function retrievePledgeRewards(uint256 pledgeId, address receiver) external whenNotPaused nonReentrant {


488:    function closePledge(uint256 pledgeId, address receiver) external whenNotPaused nonReentrant {

```

### Code Structure Deviates From Best-Practice
### Order of Functions
Some constructs deviate from this recommended best-practice: External/public functions are mixed with internal/private ones
Ordering helps readers identify which functions they can call and to find the constructor and fallback definitions easier.

Functions should be grouped according to their visibility and ordered:-  
-    constructor
-   receive function (if exists)  
-   fallback function (if exists) 
-   external
-   public
-   internal 
-   private

Our code does not seem to follow this recommendations . We have public functions declared before external functions 

https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L153
```solidity
File: /contracts/WardenPledge.sol
153:    function pledgesIndex() public view returns(uint256){

163:    function getUserPledges(address user) external view returns(uint256[] memory){
```

Internal functions are also mixed with external functions rather than be ordered accordingly

https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L222

**Recommendation:**
Consider adopting recommended best-practice for code structure and layout.
[See Documentation](https://docs.soliditylang.org/en/latest/style-guide.html#order-of-functions "Permalink to this heading")
