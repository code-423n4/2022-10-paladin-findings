
- [Gas](#gas)
    - [**1. Avoid compound assignment operator in state variables**](#1-avoid-compound-assignment-operator-in-state-variables)
        - [Total gas saved: **13 * 4 = 52**](#total-gas-saved-13--4--52)
    - [**2. Avoid unused returns**](#2-avoid-unused-returns)
    - [**3. delete optimization**](#3-delete-optimization)
        - [Total gas saved: **5 * 3 = 15**](#total-gas-saved-5--3--15)
    - [**4. Use constant for static value**](#4-use-constant-for-static-value)
    - [**5. Optimize _pledge with unchecked**](#5-optimize-_pledge-with-unchecked)
    - [**6. Optimize createPledge**](#6-optimize-createpledge)
    - [**7. Optimize extendPledge with unchecked**](#7-optimize-extendpledge-with-unchecked)
    - [**8. Use msg.sender instead of the cached creator**](#8-use-msgsender-instead-of-the-cached-creator)
    - [**9. Optimize increasePledgeRewardPerVote with unchecked**](#9-optimize-increasepledgerewardpervote-with-unchecked)
    - [**10. Optimize updateRewardToken**](#10-optimize-updaterewardtoken)
    - [**11. Avoid creating a new variable to emit an event**](#11-avoid-creating-a-new-variable-to-emit-an-event)

# Gas

## **1. Avoid compound assignment operator in state variables**

Using compound assignment operators for state variables (like `State += X` or `State -= X` ...) it's more expensive than using operator assignment (like `State = State + X` or `State = State - X` ...).

**Proof of concept (*without optimizations*):**

```javascript
pragma solidity 0.8.15;

contract TesterA {
uint private _a;
function testShort() public {
_a += 1;
}
}

contract TesterB {
uint private _a;
function testLong() public {
_a = _a + 1;
}
}
```

Gas saving executing: **13 per entry**

```
TesterA.testShort: 43507
TesterB.testLong:  43494
```

**Affected source code:**

- [WardenPledge.sol:268](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L268)
- [WardenPledge.sol:340](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L340)
- [WardenPledge.sol:401](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L401)
- [WardenPledge.sol:445](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L445)

### Total gas saved: **13 * 4 = 52**

## **2. Avoid unused returns**

Having a method that always returns the same value is not correct in terms of consumption, if you want to modify a value, and the method will perform a `revert` in case of failure, it is not necessary to return a `true`, since it will never be `false`. It is less expensive not to return anything, and it also eliminates the need to double-check the returned value by the caller.

**Affected source code:**

- [WardenPledge.sol:653-661](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L653-L661)

## **3. `delete` optimization**

Use `delete` instead of set to default value (`false` or `0`).

5 gas could be saved per entry in the following affected lines:

**Affected source code:**

- [WardenPledge.sol:473](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L473)
- [WardenPledge.sol:506](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L506)
- [WardenPledge.sol:589](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L589)

### Total gas saved: **5 * 3 = 15**

## **4. Use `constant` for static value**

The following variables are state variables, their value doesn't change but they weren't defined as `constants`, changing the definition to `constant` will save a lot of gas, because it won't use storage.

**Affected source code:**

- [WardenPledge.sol:79](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L79)

## **5. Optimize `_pledge` with `unchecked`**

By applying the following changes it is possible to save gas:

```diff
        ...
        // Re-calculate the new Boost bias & slope (using Boostv2 logic)
        uint256 slope = amount / boostDuration;
+       uint256 bias;
+       unchecked { // It was previously checked
-       uint256 bias = slope * boostDuration;
+           bias = slope * boostDuration;
+       }

        // Rewards are set in the Pledge as reward/veToken/sec
        // To find the total amount of veToken delegated through the whole Boost duration
        // based on the Boost bias & the Boost duration, to take in account that the delegated amount decreases
        // each second of the Boost duration
        uint256 totalDelegatedAmount = ((bias * boostDuration) + bias) / 2;
        // Then we can calculate the total amount of rewards for this Boost
        uint256 rewardAmount = (totalDelegatedAmount * pledgeParams.rewardPerVote) / UNIT;

        if(rewardAmount > pledgeAvailableRewardAmounts[pledgeId]) revert Errors.RewardsBalanceTooLow();
+       unchecked { // It was previously checked
        pledgeAvailableRewardAmounts[pledgeId] -= rewardAmount;
+       }

        // Send the rewards to the user
        IERC20(pledgeParams.rewardToken).safeTransfer(user, rewardAmount);

        emit Pledged(pledgeId, user, amount, endTimestamp);
    }
```

**Affected source code:**

- [WardenPledge.sol:268](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L268)

## **6. Optimize `createPledge`**

By applying the following changes it is possible to save gas by avoiding access to storage.

```diff
    function createPledge(
        address receiver,
        address rewardToken,
        uint256 targetVotes,
        uint256 rewardPerVote, // reward/veToken/second
        uint256 endTimestamp,
        uint256 maxTotalRewardAmount,
        uint256 maxFeeAmount
    ) external whenNotPaused nonReentrant returns(uint256){
        ...

        if(receiver == address(0) || rewardToken == address(0)) revert Errors.ZeroAddress();
        if(targetVotes < minTargetVotes) revert Errors.TargetVoteUnderMin();
-       if(minAmountRewardToken[rewardToken] == 0) revert Errors.TokenNotWhitelisted();
-       if(rewardPerVote < minAmountRewardToken[rewardToken]) revert Errors.RewardPerVoteTooLow();
+       if(rewardPerVote == 0 || rewardPerVote <= minAmountRewardToken[rewardToken]) revert Errors.RewardPerVoteTooLow();
        ...
    }
```

**Affected source code:**

- [WardenPledge.sol:312-313](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L312-L313)

## **7. Optimize `extendPledge` with `unchecked`**

By applying the following changes it is possible to save gas:

```diff
    function extendPledge(
        uint256 pledgeId,
        uint256 newEndTimestamp,
        uint256 maxTotalRewardAmount,
        uint256 maxFeeAmount
    ) external whenNotPaused nonReentrant {
        ...
        uint256 oldEndTimestamp = pledgeParams.endTimestamp;
        if(newEndTimestamp != _getRoundedTimestamp(newEndTimestamp) || newEndTimestamp < oldEndTimestamp) revert Errors.InvalidEndTimestamp();

+       uint256 addedDuration;
+       unchecked { // It was previously checked
-       uint256 addedDuration = newEndTimestamp - oldEndTimestamp;
-           addedDuration = newEndTimestamp - oldEndTimestamp;
+       }
        ...
    }
```

**Affected source code:**

- [WardenPledge.sol:385](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L385)

## **8. Use `msg.sender` instead of the cached `creator`**

Using `msg.sender` will always be cheaper than using a local variable.

The reason is the following:

- The `CALLER` operation which is reading the msg.sender global variable costs 2.
- While `MLOAD/MSTORE` operations which are memory operations (storage where local variables are stored) costs 3.

**Affected source code:**

- [WardenPledge.sol:333](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L333)
- [WardenPledge.sol:335](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L335)
- [WardenPledge.sol:352](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L352)
- [WardenPledge.sol:353](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L353)
- [WardenPledge.sol:355](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L355)
- [WardenPledge.sol:394](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L394)
- [WardenPledge.sol:396](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L396)
- [WardenPledge.sol:438](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L438)
- [WardenPledge.sol:440](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L440)

## **9. Optimize `increasePledgeRewardPerVote` with `unchecked`**

By applying the following changes it is possible to save gas:

```diff
    function increasePledgeRewardPerVote(
        uint256 pledgeId,
        uint256 newRewardPerVote,
        uint256 maxTotalRewardAmount,
        uint256 maxFeeAmount
    ) external whenNotPaused nonReentrant {
        ...
        uint256 oldRewardPerVote = pledgeParams.rewardPerVote;
        if(newRewardPerVote <= oldRewardPerVote) revert Errors.RewardsPerVotesTooLow();
        uint256 remainingDuration = pledgeParams.endTimestamp - block.timestamp;
+       uint256 rewardPerVoteDiff;
+       unchecked { // It was previously checked
-       uint256 rewardPerVoteDiff = newRewardPerVote - oldRewardPerVote;
+           rewardPerVoteDiff = newRewardPerVote - oldRewardPerVote;
+       }
        ...
    }
```

**Affected source code:**

- [WardenPledge.sol:431](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L431)

## **10. Optimize `updateRewardToken`**

It is possible to optimize the input verification order in the following way:

```diff
    function updateRewardToken(address token, uint256 minRewardPerSecond) external onlyOwner {
        if(token == address(0)) revert Errors.ZeroAddress();
+       if(minRewardPerSecond == 0) revert Errors.InvalidValue();
        if(minAmountRewardToken[token] == 0) revert Errors.NotAllowedToken();
-       if(minRewardPerSecond == 0) revert Errors.InvalidValue();

        minAmountRewardToken[token] = minRewardPerSecond;

        emit UpdateRewardToken(token, minRewardPerSecond);
    }
```

**Affected source code:**

- [WardenPledge.sol:573](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L573)

## **11. Avoid creating a new variable to emit an event**

It's possible to emit the event before the change and save one variable creation.

```diff
    function updateChest(address chest) external onlyOwner {
        if(chest == address(0)) revert Errors.ZeroAddress();
-       address oldChest = chestAddress;
+       emit ChestUpdated(chestAddress, chest);
        chestAddress = chest;
-       emit ChestUpdated(oldChest, chest);
    }

    function updateMinTargetVotes(uint256 newMinTargetVotes) external onlyOwner {
        if(newMinTargetVotes == 0) revert Errors.InvalidValue();
-       uint256 oldMinTarget = minTargetVotes;
+       emit MinTargetUpdated(minTargetVotes, newMinTargetVotes);
        minTargetVotes = newMinTargetVotes;
-       emit MinTargetUpdated(oldMinTarget, newMinTargetVotes);
    }

    function updatePlatformFee(uint256 newFee) external onlyOwner {
        if(newFee > 500) revert Errors.InvalidValue();
-       uint256 oldfee = protocalFeeRatio;
+       emit PlatformFeeUpdated(protocalFeeRatio, newFee);
        protocalFeeRatio = newFee;
-       emit PlatformFeeUpdated(oldfee, newFee);
    }
```

**Affected source code:**

- [WardenPledge.sol:604](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L604)
- [WardenPledge.sol:617](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L617)
- [WardenPledge.sol:630](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L630)
