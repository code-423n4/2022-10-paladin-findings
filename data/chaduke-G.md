1. https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L312
Cache minAmountRewardToken[rewardToken] in createPledge() as it has been accessed multiple times.

2. pledgesIndex() is not necessary, use pledges.length is sufficient and saves gas.  

3. getUserPledges() is not necessary, use ownerPledges[user] instead to save gas. There is already a default getter for public 
state variable. 

4. getAllPledges() is not necessary, use pledges is sufficient. There is already a default getter for public 
state variable. 



