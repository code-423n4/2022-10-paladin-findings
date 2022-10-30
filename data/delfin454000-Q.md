## QA Report - low risk

### Missing check for address(0x0) when assigning value to address state variable
___
Check for address(0x0) is missing for `chestAddress`:

[WardenPledge.sol: L140](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L140)
```solidity
        chestAddress = _chestAddress;
```
___
___

## QA Report - non-critical

### Typos
___
[WardenPledge.sol: L232](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L232)
```solidity
        // so it's override by the Pledge's endTimestamp
```
Change `override` to `overridden`
___
[WardenPledge.sol: L236](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L236)
```solidity
        // Calculated the effective Pledge duration
```
Change `Calculated` to `Calculate`
___
[WardenPledge.sol: L261](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L261)
```solidity
        // based on the Boost bias & the Boost duration, to take in account that the delegated amount decreases
```
Change `in` to `into`
___
[WardenPledge.sol: L292](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L292)
```solidity
    * @param targetVotes Maximum taget of votes to have (own balacne + delegation) for the receiver
```
Change `taget` to `target` and `balacne` to `balance`
___
[WardenPledge.sol: L295](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L295)
```solidity
    * @param maxTotalRewardAmount Maximum total reward amount allowed ot be pulled by this contract
```
Change `ot` to `to`

The same typo also occurs in the following lines: 

[WardenPledge.sol: L296](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L296)

[WardenPledge.sol: L365](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L365)

[WardenPledge.sol: L366](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L366)

[WardenPledge.sol: L411](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L411)

[WardenPledge.sol: L412](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L412)
___
[WardenPledge.sol: L339](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L339)
```solidity
        // Add the total reards as available for the Pledge & write Pledge parameters in storage
```
Change `reards` to `rewards`
___
[WardenPledge.sol: L453](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L453)
```solidity
    * @param pledgeId ID fo the Pledge
```
Change `fo` to `for`

The same typo also occurs in the following line: 

[WardenPledge.sol: L485](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L485)
___
[WardenPledge.sol: L523](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L523)
```solidity
    * @param minRewardPerSecond Minmum amount of reward per vote per second for the token
```
Change `Minmum` to `Minimum`

The same typo also occurs in the following lines: 

[WardenPledge.sol: L539](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L539)

[WardenPledge.sol: L558](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L558)

[WardenPledge.sol: L568](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L568)
___
[WardenPledge.sol: L621](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L621)
```solidity
    * @notice Updates the Platfrom fees BPS ratio
```
Change `Platfrom` to `Platform`

The same typo also occurs in the next line: 

[WardenPledge.sol: L622](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L622)
___
[WardenPledge.sol: L650](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L650)
```solidity
    * @param token Address tof the EC2O token
```
Change `tof` to `of`
___
___


### Incomplete `@notice`
Explanation for event trigger is missing

[WardenPledge.sol: L84](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L84)
```solidity
    /** @notice Event emitted when xx */
```
Similarly for the following lines:

[WardenPledge.sol: L93](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L93)

[WardenPledge.sol: L95](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L95)

[WardenPledge.sol: L97](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L97)

[WardenPledge.sol: L99](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L99)

[WardenPledge.sol: L101](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L101)

[WardenPledge.sol: L104](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L104)

[WardenPledge.sol: L107](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L107)

[WardenPledge.sol: L109](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L109)

[WardenPledge.sol: L111](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L111)

[WardenPledge.sol: L114](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L114)

[WardenPledge.sol: L118](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L118)
___
___

### Missing NatSpec
___
[WardenPledge.sol: L125-136](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L125-L136)
```solidity
    /**
    * @dev Creates the contract, set the given base parameters
    * @param _votingEscrow address of the voting token to delegate
    * @param _delegationBoost address of the contract handling delegation
    * @param _minTargetVotes min amount of veToken to target in a Pledge
    */
    constructor(
        address _votingEscrow,
        address _delegationBoost,
        address _chestAddress,
        uint256 _minTargetVotes
    ) {
```
Missing: `@param _chestAddress`
___
[WardenPledge.sol: L663-668](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L663-L668)
```solidity
    // Utils 

    function safe64(uint256 n) internal pure returns (uint64) {
        if(n > type(uint64).max) revert Errors.NumberExceed64Bits();
        return uint64(n);
    }
```
Missing `@notice`, `@param n` and `@return`
___
___

### Duplicate information in `@notice` and `@dev` statements

___
[WardenPledge.sol: L148-155](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L148-L155)
```solidity
    /**
    * @notice Amount of Pledges listed in this contract
    * @dev Amount of Pledges listed in this contract
    * @return uint256: Amount of Pledges listed in this contract
    */
    function pledgesIndex() public view returns(uint256){
        return pledges.length;
    }
```
Recommendation: Remove `@dev` since it contains the same information as `@notice`. 

Similarly for the other `@dev` and `@notice` pairs with identical information:

[WardenPledge.sol: L158-159](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L158-L159)

[WardenPledge.sol: L168-169](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L168-L169)

[WardenPledge.sol: L189-190](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L189-L190)

[WardenPledge.sol: L200-201](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L200-L201)

[WardenPledge.sol: L288-289](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L288-L289)

[WardenPledge.sol: L536-537](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L536-L537)

[WardenPledge.sol: L565-566](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L565-L566)

[WardenPledge.sol: L581-582](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L581-L582)

[WardenPledge.sol: L595-596](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L595-L596)

[WardenPledge.sol: L608-609](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L608-L609)

[WardenPledge.sol: L621-622](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L621-L622)

[WardenPledge.sol: L648-649](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L648-L649)
___

In some cases, the `@dev` statement contains additional information (i.e., appended to that in @notice):

[WardenPledge.sol: L360-367](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L360-L367)
```solidity
    /**
    * @notice Extends the Pledge duration
    * @dev Extends the Pledge duration & add rewards for that new duration
    * @param pledgeId ID of the Pledge
    * @param newEndTimestamp New end of the Pledge
    * @param maxTotalRewardAmount Maximum added total reward amount allowed ot be pulled by this contract
    * @param maxFeeAmount Maximum fee amount allowed ot be pulled by this contract
    */
```
Recommendation: Move the extra information in the `@dev` statement to the `@notice`, then remove the `@dev`, as follows:
```solidity
    /**
    * @notice Extends the Pledge duration & add rewards for that new duration
    * @param pledgeId ID of the Pledge
    * @param newEndTimestamp New end of the Pledge
    * @param maxTotalRewardAmount Maximum added total reward amount allowed to be pulled by this contract
    * @param maxFeeAmount Maximum fee amount allowed to be pulled by this contract
    */
```
Similarly for the other `@dev` and `@notice` pairs with analogous configuration:

[WardenPledge.sol: L407-408](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L407-L408)

[WardenPledge.sol: L451-452](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L451-L452)

[WardenPledge.sol: L483-484](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L483-L484)
___
___

### Event is missing `indexed` fields
Each `event` should use three `indexed` fields if there are three or more fields. Below are `events` with missing indexed fields.
___
[WardenPledge.sol: L85-92](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L85-L92)
```solidity
    event NewPledge(
        address creator,
        address receiver,
        address rewardToken,
        uint256 targetVotes,
        uint256 rewardPerVote,
        uint256 endTimestamp
    );
```
Similarly for the following events:

[WardenPledge.sol: L94](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L94)

[WardenPledge.sol: L96](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L96)

[WardenPledge.sol: L98](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L98)

[WardenPledge.sol: L102](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L102)

[WardenPledge.sol: L105](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L105)
___
___

### Update sensitive terms in both comments and code
Terms incorporating "black," "white," "slave" or "master" are potentially problematic. Substituting more neutral terminology is becoming [common practice](https://www.zdnet.com/article/mysql-drops-master-slave-and-blacklist-whitelist-terminology/).
___
[WardenPledge.sol: L66](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L66)
```solidity
    // Also used to whitelist the tokens for rewards
```
Suggestion: Change `whitelist` to `allowlist` 

Similarly for other instances of `whitelist` and its variants
___
___
