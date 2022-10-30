Overall, the codebase was quite well-written, with clear optimizations spotted in e.g. unchecked for loops. The C4udit tool also points out a lot of impactful gas optimizations. This report points out some other possible gas savings for the contract.

## [G-01] Several variables can be made `constant` and `immutable`

The following value can be made `constant`: 

```solidity=
uint256 public minDelegationTime = 1 weeks;
```

- https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L79

The following two values can be made `immutable`:

```solidity=
IVotingEscrow public votingEscrow;
IBoostV2 public delegationBoost;
```

- https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L60
- https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L62

Applying all the aforementioned reduces deployment gas from `2695825` down to `2678735` (for `17090` gas in total). 

Furthermore, since constants and immutables are stored in the contract bytecode rather than storage, this also saves around `2000` storage-reading gas for every operations that require these values.

## [G-02] `unchecked` applicable for arithmetic operations that cannot overflow/underflow

Two instances are found:

- https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L182
    - Can be changed to 
```solidity
unchecked {return (timestamp / WEEK) * WEEK;}
```
- https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L268
    - Can be changed to
```solidity
unchecked{pledgeAvailableRewardAmounts[pledgeId] -= rewardAmount;}
```

Saves approx. 150 gas on re-running tests with these modifications appleid.
