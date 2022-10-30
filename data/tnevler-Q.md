# Report
## Low Risk ##

### [L-01]: Missing checks for address(0x0) 
**Context:** 

1. https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L137
2. https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L138
3. https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L140

**Recommendation:**

Add non-zero address checks when set address state variables.

## Non-Critical Issues ##

### [N-01]: Wrong order of functions
**Context:** 

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol

**Description:**

According [official solidity documentation](https://docs.soliditylang.org/en/v0.8.17/style-guide.html#order-of-functions) functions should be grouped according to their visibility and ordered:

+ constructor

+ receive function (if exists)

+ fallback function (if exists)

+ external

+ public

+ internal

+ private

**Recommendation:**

Put the functions in the correct order according to the documentation.

### [N-02]: Typos
**Context:** 

1. ``` // so it's override by the Pledge's endTimestamp ``` [L232](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L232) (change ***override*** to ***overridden***)
2. ``` // based on the Boost bias & the Boost duration, to take in account that the delegated amount decreases ``` [L261](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L261) (change ***to take in account*** to ***to take into account***)
3. ``` * @param targetVotes Maximum taget of votes to have (own balacne + delegation) for the receiver ``` [L292](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L292) (change ***taget*** to ***target***)
4. ``` * @param targetVotes Maximum taget of votes to have (own balacne + delegation) for the receiver ``` [L292](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L292) (change ***balacne*** to ***balance***)
5. ``` * @param maxTotalRewardAmount Maximum total reward amount allowed ot be pulled by this contract ``` [L295](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L295) (change ***ot*** to ***to***)
6. ``` * @param maxFeeAmount Maximum feeamount allowed ot be pulled by this contract ``` [L296](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L296) (change ***feeamount*** to ***fee amount***)
7. ``` * @param maxFeeAmount Maximum feeamount allowed ot be pulled by this contract ``` [L296](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L296) (change ***ot*** to ***to***)
8. ``` // Add the total reards as available for the Pledge & write Pledge parameters in storage ``` [L339](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L339) (change ***reards*** to ***rewards***)
9. ``` * @param maxTotalRewardAmount Maximum added total reward amount allowed ot be pulled by this contract ``` [L365](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L365) (change ***ot*** to ***to***)
10. ``` * @param maxFeeAmount Maximum fee amount allowed ot be pulled by this contract ``` [L366](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L366) (change ***ot*** to ***to***)
11. ``` * @param pledgeId ID fo the Pledge ``` [L453](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L453) (change ***fo*** to ***for***)
12. ``` * @param pledgeId ID fo the Pledge to close ``` [L485](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L485) (change ***fo*** to ***for***)
13. ``` * @param minRewardPerSecond Minmum amount of reward per vote per second for the token ``` [L523](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L523) (change ***Minmum*** to ***Minimum***)
14. ``` * @param minRewardsPerSecond Minmum amount of reward per vote per second for each token in the list ``` [L539](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L539) (change ***Minmum*** to ***Minimum***)
15. ``` * @param minRewardPerSecond Minmum amount of reward per vote per second for the token ``` [L558](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L558) (change ***Minmum*** to ***Minimum***)
16. ``` * @param minRewardPerSecond Minmum amount of reward per vote per second for the token ``` [L568](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L568) (change ***Minmum*** to ***Minimum***)