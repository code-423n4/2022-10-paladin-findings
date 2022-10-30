### N-01 EVENT IS MISSING INDEXED FIELDS

Each `event` should use three `indexed` fields if there are three or more fieldsv 

```
85: event NewPledge
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

### N-02 USE A MORE RECENT VERSION OF SOLIDITY

When deploying contracts, you should use the latest released version of Solidity. Apart from exceptional cases, only the latest version receives [security fixes](https://github.com/ethereum/solidity/security/policy#supported-versions). Furthermore, breaking changes as well as new features are introduced regularly.
Latest Version is 0.8.17

[https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol)  

```
2: pragma solidity 0.8.10;
```

