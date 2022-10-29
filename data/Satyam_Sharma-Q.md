1.Didn't implement any emit FOR event IncreasePledgeTargetVotes:

Not implementing any emit functionality after declaring an event cause unrequired gas wastage-

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L96

2.Use && instead of using ||

The operators “||” and “&&” apply the common short-circuiting rules. This means that in the expression “f(x) || g(y)”, if “f(x)” evaluates to true, “g(y)” will not be evaluated even if it may have side-effect.

And in below repo both condition should be true i.e; receiver and rewardToken should not be equal to zero!!

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L310

3.For the safe side implement a check that requires rewardPerVote should be greater than/not equal to zero.

In WardenPledge.createPledge uint256 rewardPerVote = 0(as default value) which will effect both

        vars.totalRewardAmount = (rewardPerVote * vars.votesDifference * vars.duration) / UNIT;
        vars.feeAmount = (vars.totalRewardAmount * protocalFeeRatio) / MAX_PCT ;

 For the safe side implement a check require(rewardPerVote != 0,"");


 4.condition get implemented even after it is stated to get revert if it matches!!

 In function removeRewardToken condition that is stated in if condition is same as to be implemented whenever the if condition donot get executed or when if statement tends to false..

 if(minAmountRewardToken[token] == 0) revert Errors.NotAllowedToken();
        
        minAmountRewardToken[token] = 0;

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L585

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L422
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L459
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L491