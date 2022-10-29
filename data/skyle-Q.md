Different versions of Solidity are used:
        - Version used: ['0.8.10', '^0.8.0', '^0.8.1', '^0.8.4']
        - 0.8.10 (contracts/WardenPledge.sol#2)
        - ^0.8.4 (contracts/interfaces/IBoostV2.sol#2)
        - ^0.8.4 (contracts/interfaces/IVotingEscrow.sol#2)
        - ^0.8.0 (contracts/oz/interfaces/IERC20.sol#4)
        - ^0.8.0 (contracts/oz/libraries/SafeERC20.sol#3)
        - ^0.8.1 (contracts/oz/utils/Address.sol#4)
        - ^0.8.0 (contracts/oz/utils/Context.sol#4)
        - ^0.8.0 (contracts/oz/utils/Ownable.sol#4)
        - ^0.8.0 (contracts/oz/utils/Pausable.sol#4)
        - ^0.8.0 (contracts/oz/utils/ReentrancyGuard.sol#4)
        - ^0.8.4 (contracts/utils/Errors.sol#2)
        - 0.8.10 (contracts/utils/Owner.sol#2)
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#different-pragma-directives-are-used

Different versions of Solidity are used:
        - Version used: ['0.8.10', '^0.8.0', '^0.8.1', '^0.8.4']
        - 0.8.10 (contracts/WardenPledge.sol#2)
        - ^0.8.4 (contracts/interfaces/IBoostV2.sol#2)
        - ^0.8.4 (contracts/interfaces/IVotingEscrow.sol#2)
        - ^0.8.0 (contracts/oz/interfaces/IERC20.sol#4)
        - ^0.8.0 (contracts/oz/libraries/SafeERC20.sol#3)
        - ^0.8.1 (contracts/oz/utils/Address.sol#4)
        - ^0.8.0 (contracts/oz/utils/Context.sol#4)
        - ^0.8.0 (contracts/oz/utils/Ownable.sol#4)
        - ^0.8.0 (contracts/oz/utils/Pausable.sol#4)
        - ^0.8.0 (contracts/oz/utils/ReentrancyGuard.sol#4)
        - ^0.8.4 (contracts/utils/Errors.sol#2)
        - 0.8.10 (contracts/utils/Owner.sol#2)
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#different-pragma-directives-are-used


WardenPledge.minDelegationTime (contracts/WardenPledge.sol#79) should be constant
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#state-variables-that-could-be-declared-constant
