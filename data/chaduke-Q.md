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


