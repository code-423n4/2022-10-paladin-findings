
## N-01 EVENT IS MISSING INDEXED FIELDS

Each `event` should use three `indexed` fields if there are three or more fields

_There are 11 instances of this issue_

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol

```
File: contracts/WardenPledge.sol

85-92: event NewPledge(
        address creator,
        address receiver,
        address rewardToken,
        uint256 targetVotes,
        uint256 rewardPerVote,
        uint256 endTimestamp
    );

94: event ExtendPledgeDuration(uint256 indexed pledgeId, uint256 oldEndTimestamp, uint256 newEndTimestamp);

96: event IncreasePledgeTargetVotes(uint256 indexed pledgeId, uint256 oldTargetVotes, uint256 newTargetVotes);

98: event IncreasePledgeRewardPerVote(uint256 indexed pledgeId, uint256 oldRewardPerVote, uint256 newRewardPerVote);

102: event RetrievedPledgeRewards(uint256 indexed pledgeId, address receiver, uint256 amount);

105: event Pledged(uint256 indexed pledgeId, address indexed user, uint256 amount, uint256 endTimestamp);

108: event NewRewardToken(address indexed token, uint256 minRewardPerSecond);

110: event UpdateRewardToken(address indexed token, uint256 minRewardPerSecond);

115: event ChestUpdated(address oldChest, address newChest);

117: event PlatformFeeUpdated(uint256 oldfee, uint256 newFee);

119: event MinTargetUpdated(uint256 oldMinTarget, uint256 newMinTargetVotes);
```

---------------
## N-02 USE A MORE RECENT VERSION OF SOLIDITY

It's a best practice to use the latest compiler version.

The specified minimum compiler version is quite old. Older compilers might be susceptible to some bugs. We recommend changing the solidity version pragma to the latest version to enforce the use of an up to date compiler.

List of known compiler bugs and their severity can be found here: [https://etherscan.io/solcbuginfo](https://etherscan.io/solcbuginfo)

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol

```
File: contracts/WardenPledge.sol

2: pragma solidity 0.8.10;
```
-------------------
