1:
createPledge() totalRewardAmount maybe be zero,when votesDifference is zero , suggest add check

```
    function createPledge(
        address receiver,
        address rewardToken,
        uint256 targetVotes,
        uint256 rewardPerVote, // reward/veToken/second
        uint256 endTimestamp,
        uint256 maxTotalRewardAmount,
        uint256 maxFeeAmount
    ) external whenNotPaused nonReentrant returns(uint256){
    ...
        vars.votesDifference = targetVotes - votingEscrow.balanceOf(receiver);
+       require(vars.votesDifference>0);

```


2: _pledge() suggest add checkpoint_user(receiver) before check <=targetVotes

```
    function _pledge(uint256 pledgeId, address user, uint256 amount, uint256 endTimestamp) internal {
....

+       delegationBoost.checkpoint_user(pledgeParams.receiver);
        if(delegationBoost.adjusted_balance_of(pledgeParams.receiver) + amount > pledgeParams.targetVotes) revert Errors.TargetVotesOverflow();


```