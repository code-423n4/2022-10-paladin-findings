# Tight Variable Packing
In Solidity, we can pack multiple contract and struct variables into the same 32 bytes slot in storage. With this pattern, we can greatly optimize gas consumption when storing or loading statically-sized variables.

## Pledge Struct
Under this Struct defined from line 28 to line 44 : https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L28:L44 , we can pack the last 3 variables into a full 32 bytes slot: `rewardToken`, `endTimestamp` and `closed`.

Here are the bytes used per these types:
- `address` = 20 bytes
- `uint64` = 8 bytes
- `bool` = 1 

So, these 3 variables are using a total of 29 bytes (20 + 8 + 1). However, we can easily make them use a full 32 bytes storage slot by changing the `uint64` to an `uint88` (11 bytes) => 20 + 11 + 1 = 32.

The new Struct would look like this:

    struct Pledge{
        // Target amount of veCRV (balance scaled by Boost v2, fetched as adjusted_balance)
        uint256 targetVotes;
        // Difference of votes between the target and the receiver balance at the start of the Pledge
        // (used for later extend/increase of some parameters the Pledge)
        uint256 votesDifference;
        // Price per vote per second, set by the owner
        uint256 rewardPerVote;
        // Address to receive the Boosts
        address receiver;
        // Address of the token given as rewards to Boosters
        address rewardToken;
        // Timestamp of end of the Pledge
        uint88 endTimestamp;
        // Set to true if the Pledge is canceled, or when closed after the endTimestamp
        bool closed;
    }

Of course, we must change the content and the name of the `safe64` function to match the new `uint88` pattern : https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L665:L668

    function safe88(uint256 n) internal pure returns (uint88) {
        if(n > type(uint88).max) revert Errors.NumberExceed88Bits();
        return uint88(n);
    }

With that being applied, we also must change the lines where this function is called as we renamed it, this happens at those lines:
- https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L348
- https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L399

Finally, the revert error name must be changed in this file: https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/utils/Errors.sol#L46

    error NumberExceed88Bits();