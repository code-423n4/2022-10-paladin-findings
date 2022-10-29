-> MISSING INDEXED FIELDS IN EVENT


Fields, that are indexed, are more quickly accessible to off-chain tools that parse events. Every indexed field cost extra gas.
If there are three or more fields in an event you should use at least 3 or more indexed fields. When there are less than three fields all should be indexed.

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#:~:text=event%20NewPledge(,)%3B
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#:~:text=event%20ExtendPledgeDuration(uint256%20indexed%20pledgeId%2C%20uint256%20oldEndTimestamp%2C%20uint256%20newEndTimestamp)%3B
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#:~:text=event%20IncreasePledgeTargetVotes(uint256%20indexed%20pledgeId%2C%20uint256%20oldTargetVotes%2C%20uint256%20newTargetVotes)%3B
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#:~:text=event%20IncreasePledgeRewardPerVote(uint256%20indexed%20pledgeId%2C%20uint256%20oldRewardPerVote%2C%20uint256%20newRewardPerVote)%3B
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#:~:text=event%20RetrievedPledgeRewards(uint256%20indexed%20pledgeId%2C%20address%20receiver%2C%20uint256%20amount)%3B
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#:~:text=event%20Pledged(uint256%20indexed%20pledgeId%2C%20address%20indexed%20user%2C%20uint256%20amount%2C%20uint256%20endTimestamp)%3B
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#:~:text=event%20NewRewardToken(address%20indexed%20token%2C%20uint256%20minRewardPerSecond)%3B
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#:~:text=event%20UpdateRewardToken(address%20indexed%20token%2C%20uint256%20minRewardPerSecond)%3B
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#:~:text=event%20ChestUpdated(address%20oldChest%2C%20address%20newChest)%3B
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#:~:text=event%20PlatformFeeUpdated(uint256%20oldfee%2C%20uint256%20newFee)%3B
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#:~:text=event%20MinTargetUpdated(uint256%20oldMinTarget%2C%20uint256%20newMinTargetVotes)%3B
