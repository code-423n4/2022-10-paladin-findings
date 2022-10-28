### Q-01 - Check for 0 addresses not done in constructor

https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L131-L143

The 0 addresses check are done in the respective `update()` functions but not done in constructor. 
In fact, it is important to ensure that the `chestAddress` is not accidentally set to the 0 address as fees are transferred to it and funds can be lost forever.
There is also no way to update the `votingEscrow` and `delegationBoost` address.


### Q-02 - Huge trust assumption in `recoverErc20()`

https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L653-L661

Reward tokens are transferred into this contract when a pledge is created. The comment notes " Recovers ERC2O tokens sent by mistake to the contract", but this function can also be used by the owner to draw and drain all tokens that are deposited by pledge creators.

The cons of this function far outweighs the benefit, hence I recommend removing this function.

