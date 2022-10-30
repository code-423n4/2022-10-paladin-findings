
## Missing fee parameter validation


Some fee parameters of functions are not checked for invalid values. Validate the parameters:
        
        
### Code instances:

        WardenPledge.increasePledgeRewardPerVote (maxFeeAmount)
        WardenPledge.extendPledge (maxFeeAmount)
        WardenPledge.createPledge (maxFeeAmount)
        WardenPledge.updatePlatformFee (newFee)



## safeApprove of openZeppelin is deprecated


You use safeApprove of openZeppelin although it's deprecated.
(see https://github.com/OpenZeppelin/openzeppelin-contracts/blob/566a774222707e424896c0c390a84dc3c13bdcb2/contracts/token/ERC20/utils/SafeERC20.sol#L38)
You should change it to increase/decrease Allowance as OpenZeppilin says.
        
### Code instances:

        Deprecated safeApprove in SafeERC20.sol line 64: _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, newAllowance));
        Deprecated safeApprove in SafeERC20.sol line 76: _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, newAllowance));
        Deprecated safeApprove in SafeERC20.sol line 55: _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, value));



## Not verified input


external / public functions parameters should be validated to make sure the address is not 0.
Otherwise if not given the right input it can mistakenly lead to loss of user funds.    

### Code instance:

        WardenPledge.sol.recoverERC20 token



## Solidity compiler versions mismatch


The project is compiled with different versions of solidity, which is not recommended because it can lead to undefined behaviors.
        
### Code instance:

        



## Not verified owner


        owner param should be validated to make sure the owner address is not address(0).
        Otherwise if not given the right input all only owner accessible functions will be unaccessible.
        
        
### Code instance:

        Ownable.sol.transferOwnership newOwner



## Two Steps Verification before Transferring Ownership

The following contracts have a function that allows them an admin to change it to a different address. If the admin accidentally uses an invalid address for which they do not have the private key, then the system gets locked.
It is important to have two steps admin change where the first is announcing a pending new admin and the new address should then claim its ownership. 
A similar issue was reported in a previous contest and was assigned a severity of medium: [code-423n4/2021-06-realitycards-findings#105](https://github.com/code-423n4/2021-06-realitycards-findings/issues/105) 

### Code instance:

        Ownable.sol



## Missing non reentrancy modifier

The following functions are missing reentrancy modifier although some other pulbic/external functions does use reentrancy modifer.
Even though I did not find a way to exploit it, it seems like those functions should have the nonReentrant modifier as the other functions have it as well..

### Code instance:

        WardenPledge.sol, recoverERC20 is missing a reentrancy modifier



## In the following public update functions no value is returned

In the following functions no value is returned, due to which by default value of return will be 0. 
We assumed that after the update you return the latest new value. 
(similar issue here: https://github.com/code-423n4/2021-10-badgerdao-findings/issues/85). 

### Code instances:

        WardenPledge.sol, updateChest
        WardenPledge.sol, updateMinTargetVotes
        WardenPledge.sol, updatePlatformFee
        WardenPledge.sol, updateRewardToken



## Check transfer receiver is not 0 to avoid burned money


Transferring tokens to the zero address is usually prohibited to accidentally avoid "burning" tokens by sending them to an unrecoverable zero address.


### Code instances:

        https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L658
        https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L472
        https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L271
        https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L438
        https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L333
        https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L394
        https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L505



## Missing commenting


        The following functions are missing commenting as describe below:
        
### Code instance:

        Pausable.sol, paused (public), @return is missing



## Unsafe Cast

use openzeppilin's safeCast in:
 
### Code instance:

        https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L665 : unsafe cast uint64(n)



## Div by 0


Division by 0 can lead to accidentally revert,
(An example of a similar issue - https://github.com/code-423n4/2021-10-defiprotocol-findings/issues/84)

### Code instance:

        https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L256 boostDuration might be 0



## Tokens with fee on transfer are not supported


There are ERC20 tokens that charge fee for every transfer() / transferFrom().

Vault.sol#addValue() assumes that the received amount is the same as the transfer amount, 
and uses it to calculate attributions, balance amounts, etc. 
But, the actual transferred amount can be lower for those tokens.
Therefore it's recommended to use the balance change before and after the transfer instead of the amount.
This way you also support the tokens with transfer fee - that are popular.


### Code instances:

        https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L438
        https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L333
        https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L394


