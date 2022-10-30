# QA ISSUES FOR [2022-10-PALADIN](https://github.com/code-423n4/2022-10-paladin)

## [N-01] Events should have 3 `indexed` fields where possible

./contracts/WardenPledge.sol

```
L94:   event ExtendPledgeDuration(uint256 indexed pledgeId, uint256 oldEndTimestamp, uint256 newEndTimestamp);

L96:   event IncreasePledgeTargetVotes(uint256 indexed pledgeId, uint256 oldTargetVotes, uint256 newTargetVotes);

L98:   event IncreasePledgeRewardPerVote(uint256 indexed pledgeId, uint256 oldRewardPerVote, uint256 newRewardPerVote);

L102:  event RetrievedPledgeRewards(uint256 indexed pledgeId, address receiver, uint256 amount);
```
