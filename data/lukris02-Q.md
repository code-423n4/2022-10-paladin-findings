# QA Report for Paladin - Warden Pledges contest

## Overview
During the audit, 1 low and 5 non-critical issues were found.

№ | Title | Risk Rating  | Instance Count
--- | --- | --- | ---
L-1 | [Missing check for zero address](#l-1-missing-check-for-zero-address) | Low | 3
NC-1 | [Misleading function name](#nc-1-misleading-function-name) | Non-Critical | 1
NC-2 | [Order of Functions](#nc-2-order-of-functions) | Non-Critical | 2
NC-3 | [Spaces between the control structures](#nc-3-spaces-between-the-control-structures) | Non-Critical | 65
NC-4 | [Maximum line length exceeded](#nc-4-maximum-line-length-exceeded) | Non-Critical | 3
NC-5 | [Typos](#nc-5-typos) | Non-Critical | 14

## Low Risk Findings (1)
### L-1. Missing check for zero address
##### Description
If address(0x0) is set it may cause the contract to revert or work wrong.

##### Instances
- https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L137
- https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L138
- https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L140

##### Recommendation
Add checks.

## Non-Critical Risk Findings (5)
### NC-1. Misleading function name
##### Instances
- [L148-L155](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L148-L155):
```
    /**
    * @notice Amount of Pledges listed in this contract
    * @dev Amount of Pledges listed in this contract
    * @return uint256: Amount of Pledges listed in this contract
    */
    function pledgesIndex() public view returns(uint256){
        return pledges.length;
    }
```

##### Recommendation
Consider changing the name to ```pledgesLength()``` or ```pledgesAmount()```.

#
### NC-2. Order of Functions
##### Description
According to [Style Guide](https://docs.soliditylang.org/en/v0.8.16/style-guide.html#order-of-functions), ordering helps readers identify which functions they can call and to find the constructor and fallback definitions easier.  
Functions should be grouped according to their visibility and ordered:
1) constructor
2) receive function (if exists)
3) fallback function (if exists)
4) external
5) public
6) internal
7) private

##### Instances
Internal function should be after external:
- https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L525

Public function should be after external:
- https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L153

##### Recommendation
Reorder functions where possible.

#
### NC-3. Spaces between the control structures
##### Description
According to [Style Guide](https://docs.soliditylang.org/en/v0.8.16/style-guide.html#control-structures), there should be a single space between the control structures ```if```, ```while```, and ```for``` and the parenthetic block representing the conditional.

##### Instances
- 64 instances with ```if(```
- [1 instance](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L547) with ```for(```

##### Recommendation
Change:
```
if(...) 
```
to:
```
if (...) 
```

#
### NC-4. Maximum line length exceeded
##### Description
Some lines of code are too long.

##### Instances
- (135 symbols) https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L234
- (135) https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L245
- (134) https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L383

##### Recommendation
According to [Style Guide](https://docs.soliditylang.org/en/v0.8.16/style-guide.html#maximum-line-length), maximum suggested line length is 120 characters.  
Make the lines shorter.

#
### NC-5. Typos
##### Instances
- [``` * @param targetVotes Maximum taget of votes to have (own balacne + delegation) for the receiver```](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L292) => ```target```
- [``` * @param targetVotes Maximum taget of votes to have (own balacne + delegation) for the receiver```](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L292) => ```balance```
- [```* @param maxTotalRewardAmount Maximum total reward amount allowed ot be pulled by this contract```](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L295) => ```to```
- [```* @param maxFeeAmount Maximum feeamount allowed ot be pulled by this contract```](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L296) => ```fee amount```
- [```* @param maxFeeAmount Maximum feeamount allowed ot be pulled by this contract```](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L296) => ```to```
- [```// Add the total reards as available for the Pledge & write Pledge parameters in storage```](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L339) => ```rewards```
- [```* @param maxTotalRewardAmount Maximum added total reward amount allowed ot be pulled by this contract```](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L365) => ```to```
- [```* @param maxFeeAmount Maximum fee amount allowed ot be pulled by this contract```](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L366) => ```to```
- [```* @param pledgeId ID fo the Pledge```](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L453) => ``` for```
- [``` * @param pledgeId ID fo the Pledge to close```](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L485) => ``` for```
- [```* @param minRewardPerSecond Minmum amount of reward per vote per second for the token```](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L523) => ```Minimum```
- [```* @param minRewardsPerSecond Minmum amount of reward per vote per second for each token in the list```](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L539) => ```Minimum```
- [```* @param minRewardPerSecond Minmum amount of reward per vote per second for the token```](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L558) => ```Minimum```
- [```* @param minRewardPerSecond Minmum amount of reward per vote per second for the token```](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L568) => ```Minimum```