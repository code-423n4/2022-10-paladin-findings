A few improvements can be made to enhance the readability and trustworthiness of the contract.

1. pledgeAvailableRewardAmounts, can be defined in the Pledge struct, in this way, no separate mapping is necessary and  it is easy to look up for the AvailableRewardAmount for each Pledge and the data structure is more organized. The ith pledge's AvaiablerRewardAmount is:

```
   pledges[i].AvailableRewardAmount

```


2. pledgeOwner can be defined in the Pledge struct, in this way, no mapping is needed and and  it is easy to look up for the owner for each Pledge and the data structure is more organized. The ith pledge's owner is: 
```
    pledges[i].pledgeOwner
```

3. both functions, retrievePledgeRewards() and closePledge() have similar functionality: close the Pledge and retrieve all remaining reward ammount. Suggestion: delete one of them or refactor so that they both call another common internal function

4. https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L599
The transfer of ChestAddress needs to be done in two steps to avoid input error: 1) submit a new pending chest; 2) the new pending chest accept the proposal and accept it become the new chest address. 

5. In the function removeRewardToken(), one needs to consider if there are some pledges that use this token as the reward token and if yes, then might need to wait for these pledges to close before removing the token from the contract. 