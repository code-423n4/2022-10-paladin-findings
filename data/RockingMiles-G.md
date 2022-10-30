## Unnecessary index init


In for loops you initialize the index to start from 0, but it already initialized to 0 in default and this assignment cost gas. 
It is more clear and gas efficient to declare without assigning 0 and will have the same meaning:

### Code instance:

        WardenPledge.sol, 547



## Short the following require messages

The following require messages are of length more than 32 and we think are short enough to short
them into exactly 32 characters such that it will be placed in one slot of memory and the require 
function will cost less gas. 
The list: 

### Code instances:

        Solidity file: Ownable.sol, In line 70, Require message length to shorten: 38, The message: Ownable: new owner is the zero address
        Solidity file: Address.sol, In line 189, Require message length to shorten: 38, The message: Address: delegate call to non-contract
        Solidity file: Address.sol, In line 134, Require message length to shorten: 38, The message: Address: insufficient balance for call
        Solidity file: Address.sol, In line 162, Require message length to shorten: 36, The message: Address: static call to non-contract



## Use != 0 instead of > 0


Using != 0 is slightly cheaper than > 0. (see https://github.com/code-423n4/2021-12-maple-findings/issues/75 for similar issue)


### Code instances:

        Address.sol, 61: change 'balance > 0' to 'balance != 0'
        WardenPledge.sol, 245: change 'balance > 0' to 'balance != 0'
        Address.sol, 134: change 'balance > 0' to 'balance != 0'



## Consider inline the following functions to save gas


    You can inline the following functions instead of writing a specific function to save gas.
    (see https://github.com/code-423n4/2021-11-nested-findings/issues/167 for a similar issue.)

    
### Code instances

        Context.sol, _msgData, { return msg.data; }
        Context.sol, _msgSender, { return msg.sender; }
        WardenPledge.sol, _getRoundedTimestamp, { return (timestamp / WEEK) * WEEK; }
        Address.sol, functionCall, { return functionCallWithValue(target, data, 0, errorMessage); }



## Inline one time use functions


The following functions are used exactly once. Therefore you can inline them and save gas and improve code clearness.
    

### Code instances:

        Ownable.sol, _checkOwner
        SafeERC20.sol, safeTransfer
        Pausable.sol, _requireNotPaused
        Address.sol, functionCall
        Pausable.sol, _requirePaused



## Do not cache msg.sender


We recommend not to cache msg.sender since calling it is 2 gas while reading a variable is more.


### Code instance:

        https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol#L309

