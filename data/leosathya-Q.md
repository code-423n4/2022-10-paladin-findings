### [L-01] ABSENCE OF ZERO ADDRESS CHECK FOR ADDRESSES IN CONSTRUCTOR 
There are 2 instances of this issue:
> ** File :  **  => https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L137-L140
> ** File :  **  => https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L

#### Recommended Mitigation
There should be require() that validate a perticular address is zero address or not


### [L-02] ABSENCE OF CHECK FOR minTargetVotes IN CONSTRUCTOR 

There are 1 instances of this issue:
> ** File :  **  => https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L142

#### Recommended Mitigation
There should be upperbound or lowerbound check for minTargetVotes inside constructor


### [L-03] DIVIDE BEFORE MULTIPLICATION CAUSE PRECISION LOSS

There are 1 instances of this issue:
> ** File :  **  => https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L182

#### Recommended Mitigation
Multiple should occur before Divide like,
(timestamp / WEEK) * WEEK; 
To
(timestamp * WEEK) / WEEK; 


### [N-01] REPEATED CODE NOTICED IN SOME FUNCTIONS

This following code repeatedly used in multiple functions like in extendPledge(), increasePledgeRewardPerVote(), retrievePledgeRewards(), closePledge()

if(pledgeId >= pledgesIndex()) revert Errors.InvalidPledgeID();
        address creator = pledgeOwner[pledgeId];
        if(msg.sender != creator) revert Errors.NotPledgeCreator();
        if(receiver == address(0)) revert Errors.ZeroAddress();

> ** File :  **  => https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L374-L378
> ** File :  **  => https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L420-L424
> ** File :  **  => https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L456-L462
> ** File :  **  => https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L488-L493

So i recommend there should be a private fuction that handle this verification of pledge part and return "Pledge storage pledgeParams" on success, and that private function call inside other external functions. 