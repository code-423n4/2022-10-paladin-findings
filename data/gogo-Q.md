# 2022-10-PALADIN

## Low Risk And Non-Critical Issues

### Event is missing `indexed` fields

_There are **5** instances of this issue:_

```solidity
File: contracts/WardenPledge.sol

94:   event ExtendPledgeDuration(uint256 indexed pledgeId, uint256 oldEndTimestamp, uint256 newEndTimestamp);

96:   event IncreasePledgeTargetVotes(uint256 indexed pledgeId, uint256 oldTargetVotes, uint256 newTargetVotes);

98:   event IncreasePledgeRewardPerVote(uint256 indexed pledgeId, uint256 oldRewardPerVote, uint256 newRewardPerVote);

102:  event RetrievedPledgeRewards(uint256 indexed pledgeId, address receiver, uint256 amount);

105:  event Pledged(uint256 indexed pledgeId, address indexed user, uint256 amount, uint256 endTimestamp);
```

https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol
