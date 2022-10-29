#### Low risk: 
##### [L-01] Missing `endTimestamp > block.timestamp` check leads to reverted transaction
##### [L-02] More inclusive check on `newEndTimestamp < oldEndTimestamp`
##### [L-03] Missing `remainingDuration > 0` check

#### Non-critical:
##### [N-01] Natspec @notice and @dev duplicate a lot
##### [N-02] Natspec Typo

---
### Low risk:
#### [L-01]  Missing `endTimestamp > block.timestamp` check leads to reverted transaction  
```sol
L237: uint256 boostDuration = endTimestamp - block.timestamp;   // @audit: revert if endTimestamp <= block.timestamp
L256: uint256 slope = amount / boostDuration;
```


Recommend adding check `endTimestamp > block.timestamp`
```sol
L234:
if(endTimestamp <= block.timestamp || endTimestamp > pledgeParams.endTimestamp || endTimestamp != _getRoundedTimestamp(endTimestamp)) revert Errors.InvalidEndTimestamp();
```


#### [L-02] More inclusive check on `newEndTimestamp < oldEndTimestamp`
newEndTimestamp = oldEndTimestamp is not valid

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L383

Recommend more inclusive check:
```sol
newEndTimestamp <= oldEndTimestamp 
```


#### [L-03] Missing `remainingDuration > 0` check
Missing this check leads to `totalRewardAmount` = 0 and get reverted later

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L430
```sol
L430: uint256 remainingDuration = pledgeParams.endTimestamp - block.timestamp;
L432: uint256 totalRewardAmount = (rewardPerVoteDiff * pledgeParams.votesDifference * remainingDuration) / UNIT;
```

Recommend adding the following check:
```sol
if(remainingDuration == 0) revert ... ;
```


###  Non-critical:
#### [N-01]  Natspec @notice and @dev duplicate a lot
It's quite annoying reading a same line twice many times
- https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L149-L150
- https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L158-L159
- https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L168-L169
- https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L189-L190
- https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L200-L201
- https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L288-L289
- https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L536-L537
- https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L555-L556
- https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L565-L566
- https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L581-L582
- https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L595-L596
- https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L608-L609
- https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L621-L622
- https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L648-L649

Recommend deleting all duplicates


#### [N-02]  Typo 
- https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L292
- https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L295-L296
- https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L339
- https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L365-L366
- https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L411-L412

Recommend correcting Natpec `balacne` -> balance, `feeamount` -> feeAmount, `ot` -> to, `reards` -> rewards