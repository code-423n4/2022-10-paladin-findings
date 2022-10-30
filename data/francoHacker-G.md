[G-01] 'X = X + Y' IS CHEAPER THAN 'X += Y';

Usually does not work with struct and mappings.

   ' pledgeAvailableRewardAmounts[vars.newPledgeID] += vars.totalRewardAmount;'

fix:

    ' pledgeAvailableRewardAmounts[vars.newPledgeID] = pledgeAvailableRewardAmounts[vars.newPledgeID] +  vars.totalRewardAmount;'



instances:

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L340-L401-L445-L268



[G-02] USE NAMED RETURNS FOR LOCAL VARIABLES WHERE IT IS POSSIBLE

   'function getAllPledges() external view returns(Pledge[] memory){
        return pledges;
    }'

fix:

  'function getAllPledges() external view returns(Pledge[] memory pledges){
        return pledges;
    }'

instances:

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L172

