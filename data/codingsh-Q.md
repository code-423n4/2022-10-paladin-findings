## [L001] - Unsafe ERC20 Operation(s)

```solidity

  475:
    WardenPledge.sol::475 => IERC20(pledgeParams.rewardToken).safeTransfer(receiver, remainingAmount);
  658:
    => IERC20(token).safeTransfer(owner(), amount);
  508:
    => IERC20(pledgeParams.rewardToken).safeTransfer(receiver, remainingAmount);
  475:
    => IERC20(pledgeParams.rewardToken).safeTransfer(receiver, remainingAmount);
  271:
    => IERC20(pledgeParams.rewardToken).safeTransfer(user, rewardAmount);
```

## state variables that could be declared constan

```solidity
79:
    => WardenPledge.minDelegationTime (contracts/WardenPledge.sol#79) should be constant
```

Reference: <https://github.com/crytic/slither/wiki/Detector-Documentation#state-variables-that-could-be-declared-constant>

## missing zero address validation

```solidity
WardenPledge.constructor(address,address,address,uint256)._chestAddress (contracts/WardenPledge.sol#134) lacks a zero-check on :
  - chestAddress = _chestAddress (contracts/WardenPledge.sol#140)
```

Reference: <https://github.com/crytic/slither/wiki/Detector-Documentation#missing-zero-address-validation>

### Reentracy

```solidity
273:
    => Reentrancy in WardenPledge._pledge(uint256,address,uint256,uint256) (contracts/WardenPledge.sol#223-275):
```

### External calls

```solidity
241:
    => - delegationBoost.checkpoint_user(user) (contracts/WardenPledge.sol#241)
249:
    => - delegationBoost.boost(pledgeParams.receiver,amount,endTimestamp,user) (contracts/WardenPledge.sol#249-254)
```

### State variables written after the call(s):

```solidity
269:
    => - pledgeAvailableRewardAmounts[pledgeId] -= rewardAmount (contracts/WardenPledge.sol#269)
300:
    => Reentrancy in WardenPledge.createPledge(address,address,uint256,uint256,uint256,uint256,uint256) (contracts/WardenPledge.sol#300-359):
```

### External calls:

```solidity
334:
    =>  - IERC20(rewardToken).safeTransferFrom(creator,address(this),vars.totalRewardAmount) (contracts/WardenPledge.sol#334)
336:
    =>
 - IERC20(rewardToken).safeTransferFrom(creator,chestAddress,vars.feeAmount) (contracts/WardenPledge.sol#336)
341:
    =>
 State variables written after the call(s):
 - ownerPledges[creator].push(vars.newPledgeID) (contracts/WardenPledge.sol#354)
 - pledgeAvailableRewardAmounts[vars.newPledgeID] += vars.totalRewardAmount (contracts/WardenPledge.sol#341)
 - pledgeOwner[vars.newPledgeID] = creator (contracts/WardenPledge.sol#353)
 - pledges.push(Pledge(targetVotes,vars.votesDifference,rewardPerVote,receiver,rewardToken,safe64(endTimestamp),false)) (contracts/WardenPledge.sol#343-351)
```

```solidity
Reentrancy in WardenPledge.extendPledge(uint256,uint256,uint256,uint256) (contracts/WardenPledge.sol#369-405):
 External calls:
 - IERC20(pledgeParams.rewardToken).safeTransferFrom(creator,address(this),totalRewardAmount) (contracts/WardenPledge.sol#395)
 - IERC20(pledgeParams.rewardToken).safeTransferFrom(creator,chestAddress,feeAmount) (contracts/WardenPledge.sol#397)
 State variables written after the call(s):
 - pledgeAvailableRewardAmounts[pledgeId] += totalRewardAmount (contracts/WardenPledge.sol#402)
```

```solidity
Reentrancy in WardenPledge.increasePledgeRewardPerVote(uint256,uint256,uint256,uint256) (contracts/WardenPledge.sol#415-449):
 External calls:
 - IERC20(pledgeParams.rewardToken).safeTransferFrom(creator,address(this),totalRewardAmount) (contracts/WardenPledge.sol#439)
 - IERC20(pledgeParams.rewardToken).safeTransferFrom(creator,chestAddress,feeAmount) (contracts/WardenPledge.sol#441)
 State variables written after the call(s):
 - pledgeAvailableRewardAmounts[pledgeId] += totalRewardAmount (contracts/WardenPledge.sol#446)
```

Reference: <https://github.com/crytic/slither/wiki/Detector-Documentation#reentrancy-vulnerabilities-2>

```solidity
Reentrancy in WardenPledge._pledge(uint256,address,uint256,uint256) (contracts/WardenPledge.sol#223-275):
 External calls:
 - delegationBoost.checkpoint_user(user) (contracts/WardenPledge.sol#241)
 - delegationBoost.boost(pledgeParams.receiver,amount,endTimestamp,user) (contracts/WardenPledge.sol#249-254)
 - IERC20(pledgeParams.rewardToken).safeTransfer(user,rewardAmount) (contracts/WardenPledge.sol#272)
 Event emitted after the call(s):
 - Pledged(pledgeId,user,amount,endTimestamp) (contracts/WardenPledge.sol#274)
```

```solidity
Reentrancy in WardenPledge.closePledge(uint256,address) (contracts/WardenPledge.sol#489-516):
 External calls:
 - IERC20(pledgeParams.rewardToken).safeTransfer(receiver,remainingAmount) (contracts/WardenPledge.sol#509)
 Event emitted after the call(s):
 - ClosePledge(pledgeId) (contracts/WardenPledge.sol#515)
 - RetrievedPledgeRewards(pledgeId,receiver,remainingAmount) (contracts/WardenPledge.sol#511)
```

```solidity
Reentrancy in WardenPledge.createPledge(address,address,uint256,uint256,uint256,uint256,uint256) (contracts/WardenPledge.sol#300-359):
 External calls:
 - IERC20(rewardToken).safeTransferFrom(creator,address(this),vars.totalRewardAmount) (contracts/WardenPledge.sol#334)
 - IERC20(rewardToken).safeTransferFrom(creator,chestAddress,vars.feeAmount) (contracts/WardenPledge.sol#336)
 Event emitted after the call(s):
 - NewPledge(creator,receiver,rewardToken,targetVotes,rewardPerVote,endTimestamp) (contracts/WardenPledge.sol#356)
```

```solidity
Reentrancy in WardenPledge.extendPledge(uint256,uint256,uint256,uint256) (contracts/WardenPledge.sol#369-405):
 External calls:
 - IERC20(pledgeParams.rewardToken).safeTransferFrom(creator,address(this),totalRewardAmount) (contracts/WardenPledge.sol#395)
 - IERC20(pledgeParams.rewardToken).safeTransferFrom(creator,chestAddress,feeAmount) (contracts/WardenPledge.sol#397)
 Event emitted after the call(s):
 - ExtendPledgeDuration(pledgeId,oldEndTimestamp,newEndTimestamp) (contracts/WardenPledge.sol#404)
 ```

 ```solidity
Reentrancy in WardenPledge.increasePledgeRewardPerVote(uint256,uint256,uint256,uint256) (contracts/WardenPledge.sol#415-449):
 External calls:
 - IERC20(pledgeParams.rewardToken).safeTransferFrom(creator,address(this),totalRewardAmount) (contracts/WardenPledge.sol#439)
 - IERC20(pledgeParams.rewardToken).safeTransferFrom(creator,chestAddress,feeAmount) (contracts/WardenPledge.sol#441)
 Event emitted after the call(s):
 - IncreasePledgeRewardPerVote(pledgeId,oldRewardPerVote,newRewardPerVote) (contracts/WardenPledge.sol#448)
```

```solidity
Reentrancy in WardenPledge.retrievePledgeRewards(uint256,address) (contracts/WardenPledge.sol#457-481):
 External calls:
 - IERC20(pledgeParams.rewardToken).safeTransfer(receiver,remainingAmount) (contracts/WardenPledge.sol#476)
 Event emitted after the call(s):
 - RetrievedPledgeRewards(pledgeId,receiver,remainingAmount) (contracts/WardenPledge.sol#478)
````

Reference: <https://github.com/crytic/slither/wiki/Detector-Documentation#reentrancy-vulnerabilities-3>

```solidity
WardenPledge._pledge(uint256,address,uint256,uint256) (contracts/WardenPledge.sol#223-275) uses timestamp for comparisons
 Dangerous comparisons:
 - pledgeId >= pledgesIndex() (contracts/WardenPledge.sol#224)
 - pledgeParams.endTimestamp <= block.timestamp (contracts/WardenPledge.sol#230)
 - endTimestamp == 0 (contracts/WardenPledge.sol#234)
 - endTimestamp > pledgeParams.endTimestamp || endTimestamp != _getRoundedTimestamp(endTimestamp) (contracts/WardenPledge.sol#235)
 - delegationBoost.adjusted_balance_of(pledgeParams.receiver) + amount > pledgeParams.targetVotes (contracts/WardenPledge.sol#246)
 - rewardAmount > pledgeAvailableRewardAmounts[pledgeId] (contracts/WardenPledge.sol#268)
```

```solidity
WardenPledge.createPledge(address,address,uint256,uint256,uint256,uint256,uint256) (contracts/
WardenPledge.sol#300-359) uses timestamp for comparisons
 Dangerous comparisons:
 - endTimestamp != _getRoundedTimestamp(endTimestamp) (contracts/WardenPledge.sol#317)
 - vars.duration < minDelegationTime (contracts/WardenPledge.sol#321)
 - vars.totalRewardAmount > maxTotalRewardAmount (contracts/WardenPledge.sol#330)
 - vars.feeAmount > maxFeeAmount (contracts/WardenPledge.sol#331)
```

```solidity
WardenPledge.extendPledge(uint256,uint256,uint256,uint256) (contracts/WardenPledge.sol#369-405) uses timestamp for comparisons
 Dangerous comparisons:
 - pledgeId >= pledgesIndex() (contracts/WardenPledge.sol#375)
 - pledgeParams.endTimestamp <= block.timestamp (contracts/WardenPledge.sol#381)
 - newEndTimestamp != _getRoundedTimestamp(newEndTimestamp) || newEndTimestamp < oldEndTimestamp (contracts/WardenPledge.sol#384)
WardenPledge.increasePledgeRewardPerVote(uint256,uint256,uint256,uint256) (contracts/
```

```solidity
WardenPledge.sol#415-449) uses timestamp for comparisons
 Dangerous comparisons:
 - pledgeId >= pledgesIndex() (contracts/WardenPledge.sol#421)
 - pledgeParams.endTimestamp <= block.timestamp (contracts/WardenPledge.sol#427)
 - totalRewardAmount > maxTotalRewardAmount (contracts/WardenPledge.sol#435)
 - feeAmount > maxFeeAmount (contracts/WardenPledge.sol#436)
```

```solidity
WardenPledge.retrievePledgeRewards(uint256,address) (contracts/WardenPledge.sol#457-481) uses timestamp for comparisons
 Dangerous comparisons:
 - pledgeId >= pledgesIndex() (contracts/WardenPledge.sol#458)
 - pledgeParams.endTimestamp > block.timestamp (contracts/WardenPledge.sol#464)
```

```solidity
WardenPledge.closePledge(uint256,address) (contracts/WardenPledge.sol#489-516) uses timestamp for comparisons
 Dangerous comparisons:
 - pledgeId >= pledgesIndex() (contracts/WardenPledge.sol#490)
 - pledgeParams.endTimestamp <= block.timestamp (contracts/WardenPledge.sol#497)
```

Reference: <https://github.com/crytic/slither/wiki/Detector-Documentation#block-timestamp>

