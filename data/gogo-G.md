# 2022-10-PALADIN

## Gas Optimizations Report

### State variables should be cached in stack variables rather than re-reading them.

The instances below point to the second+ access of a state variable within a function. Caching of a state variable replace each Gwarmaccess (100 gas) with a much cheaper stack read. Other less obvious fixes/optimizations include having local memory caches of state variable structs, or having local caches of state variable contracts/addresses.

_There are **1** instances of this issue:_

```solidity
File: contracts/WardenPledge.sol

      /// @audit Cache `delegationBoost`. Used 5 times in `_pledge`
240:  delegationBoost.checkpoint_user(user);
241:  if(delegationBoost.allowance(user, address(this)) < amount) revert Errors.InsufficientAllowance();
242:  if(delegationBoost.delegable_balance(user) < amount) revert Errors.CannotDelegate();
245:  if(delegationBoost.adjusted_balance_of(pledgeParams.receiver) + amount > pledgeParams.targetVotes) revert Errors.TargetVotesOverflow();
248:  delegationBoost.boost(
```

https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol

### Functions that are access-restricted from most users may be marked as `payable`

Marking a function as `payable` reduces gas cost since the compiler does not have to check whether a payment was provided or not. This change will save around 21 gas per function call.

_There are **10** instances of this issue:_

```solidity
File: contracts/WardenPledge.sol

541:  function addMultipleRewardToken(address[] calldata tokens, uint256[] calldata minRewardsPerSecond) external onlyOwner {

560:  function addRewardToken(address token, uint256 minRewardPerSecond) external onlyOwner {

570:  function updateRewardToken(address token, uint256 minRewardPerSecond) external onlyOwner {

585:  function removeRewardToken(address token) external onlyOwner {

599:  function updateChest(address chest) external onlyOwner {

612:  function updateMinTargetVotes(uint256 newMinTargetVotes) external onlyOwner {

625:  function updatePlatformFee(uint256 newFee) external onlyOwner {

636:  function pause() external onlyOwner {

643:  function unpause() external onlyOwner {

653:  function recoverERC20(address token) external onlyOwner returns(bool) {
```

https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol

### `++i`/`i++` should be `unchecked{++I}`/`unchecked{I++}` in `for`-loops

When an increment or any arithmetic operation is not possible to overflow it should be placed in `unchecked{}` block. \This is because of the default compiler overflow and underflow safety checks since Solidity version 0.8.0. \In for-loops it saves around 30-40 gas **per loop**

_There are **1** instances of this issue:_

```solidity
File: contracts/WardenPledge.sol

550:      unchecked{ ++i; }
```

https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol

### `<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables

_There are **4** instances of this issue:_

```solidity
File: contracts/WardenPledge.sol

268:  pledgeAvailableRewardAmounts[pledgeId] -= rewardAmount;

340:  pledgeAvailableRewardAmounts[vars.newPledgeID] += vars.totalRewardAmount;

401:  pledgeAvailableRewardAmounts[pledgeId] += totalRewardAmount;

445:  pledgeAvailableRewardAmounts[pledgeId] += totalRewardAmount;
```

https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol

### Usage of `uint`s/`int`s smaller than 32 bytes (256 bits) incurs overhead

'When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.' \ https://docs.soliditylang.org/en/v0.8.15/internals/layout_in_storage.html \ Use a larger size then downcast where needed

_There are **1** instances of this issue:_

```solidity
File: contracts/WardenPledge.sol

41:    uint64 endTimestamp;
```

https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol

### Replace `x <= y` with `x < y + 1`, and `x >= y` with `x > y - 1`

In the EVM, there is no opcode for `>=` or `<=`. When using greater than or equal, two operations are performed: `>` and `=`. Using strict comparison operators hence saves gas

_There are **10** instances of this issue:_

```solidity
File: contracts/WardenPledge.sol

223:  if(pledgeId >= pledgesIndex()) revert Errors.InvalidPledgeID();

229:  if(pledgeParams.endTimestamp <= block.timestamp) revert Errors.ExpiredPledge();

374:  if(pledgeId >= pledgesIndex()) revert Errors.InvalidPledgeID();

380:  if(pledgeParams.endTimestamp <= block.timestamp) revert Errors.ExpiredPledge();

420:  if(pledgeId >= pledgesIndex()) revert Errors.InvalidPledgeID();

426:  if(pledgeParams.endTimestamp <= block.timestamp) revert Errors.ExpiredPledge();

429:  if(newRewardPerVote <= oldRewardPerVote) revert Errors.RewardsPerVotesTooLow();

457:  if(pledgeId >= pledgesIndex()) revert Errors.InvalidPledgeID();

489:  if(pledgeId >= pledgesIndex()) revert Errors.InvalidPledgeID();

496:  if(pledgeParams.endTimestamp <= block.timestamp) revert Errors.ExpiredPledge();
```

https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol

### Use `immutable` & `constant` for state variables that do not change their value

_There are **2** instances of this issue:_

```solidity
File: contracts/WardenPledge.sol

60:   IVotingEscrow public votingEscrow;

62:   IBoostV2 public delegationBoost;
```

https://github.com/code-423n4/2022-10-paladin/tree/main/contracts/WardenPledge.sol
