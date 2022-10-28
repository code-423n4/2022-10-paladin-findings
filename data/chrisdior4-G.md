# x += y COSTS MORE GAS THAN  x = x + y FOR STATE VARIABLES

### Using the addition operator instead of plus-equals saves 113 gas

#### File: WardenPledge.sol

1.pledgeAvailableRewardAmounts[vars.newPledgeID] += vars.totalRewardAmount;
1.https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L340

#### Consider changing this and the other alike instances this way:
1.pledgeAvailableRewardAmounts[vars.newPledgeID] = pledgeAvailableRewardAmounts[vars.newPledgeID]  + vars.totalRewardAmount;

=======================

2.pledgeAvailableRewardAmounts[pledgeId] += totalRewardAmount;
2.https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L401

3.pledgeAvailableRewardAmounts[pledgeId] += totalRewardAmount;
3.https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L445

4.pledgeAvailableRewardAmounts[pledgeId] -= rewardAmount;
4.https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L268

=================================

# Division by two should use bit shifting

### X * 2 is equivalent to X << 1 and X / 2 is the same as X >> 1. The MUL and DIV opcodes cost 5 gas, whereas SHL and SHR only cost 3 gas.

1.uint256 totalDelegatedAmount = ((bias * boostDuration) + bias) / 2;
1.https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L263

================================


