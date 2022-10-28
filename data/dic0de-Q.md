Not all expired pledges are marked closed. The contract provides two mechanism of closing a pledge via the `retrievePledgeRewards ()` and the `closePledge ()` functions. In the `retrievePledgeRewards ()` function, the pledge has to be expired and if it was not closed, it will be closed via this line of code  ` if(!pledgeParams.closed) pledgeParams.closed = true;` as seen here: https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L456-L479.

In the `closePledge ()` function, it is required that the pledge is active and not expired for it to be closed.

As such, the contract envisions two scenarios when a pledge is closed, when it is still active and not expired hence the pledge creator can close it and when creator is retrieving pledge rewards. 

However, the contract does not anticipate a scenario where there are no pledge rewards in the contract and the pledge is expired. In this specific case, the pledges will remain open.  