1. Using private rather than public for constants, saves gas (8 instances)

code snippet:-
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L22
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L23
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L24

recommendation :-
change  to private instead of public constant  

reference:-
https://code4rena.com/reports/2022-08-olympus/#g-03--using-private-rather-than-public-for-constants-saves-gas-8-instances
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
2.
x = x + y is cheaper than x += y; (6 instances)


code snippet:-
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L340
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L401
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L445

recommendation:-
use x = x+y instead of x+=y

reference:-
https://code4rena.com/reports/2022-08-olympus/#g-11-x--x--y-is-cheaper-than-x--y-6-instances
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
3.
storage pointer to a structure is cheaper than copying each value of the structure into memory, same for array and mapping (7 instances)

code snippet:-
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L227

reference :-
https://code4rena.com/reports/2022-08-olympus/#g-02--storage-pointer-to-a-structure-is-cheaper-than-copying-each-value-of-the-structure-into-memory-same-for-array-and-mapping-7-instances

recommendation:-
Pledge memory pledgeParams = pledges[pledgeId]; change to Pledge storage pledgeParams = pledges[pledgeId];
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


