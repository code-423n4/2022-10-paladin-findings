## Missing zero-address checks in ` WardenPledge.createPledge() `  and ` WardenPledge.extendPledge() `





` https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L131-L143` 

```
    constructor(
        address _votingEscrow,
        address _delegationBoost,
        address _chestAddress,
        uint256 _minTargetVotes
    ) {
        votingEscrow = IVotingEscrow(_votingEscrow);
        delegationBoost = IBoostV2(_delegationBoost);

        chestAddress = _chestAddress;

        minTargetVotes = _minTargetVotes;
    }

```



### Recommendation:
 Short term, consider adding zero-address checks to the WardenPledge createPledge and extendPledge  function to prevent operator errors. Long term, review state variables which referencing contracts to ensure that the code that sets the state variables performs zero-address checks where necessary