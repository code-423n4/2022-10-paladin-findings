1. The function modifier can be modified to public
The transferOwnership and acceptOwnership function modifiers in the Owner contract can be modified to public to save gas consumption.

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/utils/Owner.sol#L29
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/utils/Owner.sol#L19


2. Missing zero address judgment
The constructor of the WardenPledge contract lacks zero address judgments for _votingEscrow, _delegationBoost, and _chestAddress.
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L131-L143


3. Wrong interface definition
The approve function in the test/BoostV2.vy file has a return value, while the approve function interface defined in the interfaces/IBoostV2.sol file has no return value.
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/interfaces/IBoostV2.sol#L25

4. minDelegationTime judgment error
In the extendPledge of the WardenPledge contract, minDelegationTime refers to the minimum duration when the pledge is created. When extending the duration of the pledge, I think it should only satisfy: newEndTimestamp - block.timestamp >= minDelegationTime.
It is recommended to confirm whether it is consistent with the designed business logic.
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L386
