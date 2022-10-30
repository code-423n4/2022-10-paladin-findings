1)X = X + Y OR X = X - Y CAN BE USED INSTEAD OF X += Y OR X -= Y

x = x + y or x = x - y costs less gas than x += y or x -= y. For example, v += 27 can be changed to v = v + 27 in the following code.

File: 2022-10-paladin\contracts\WardenPledge.sol
  340,56:         pledgeAvailableRewardAmounts[vars.newPledgeID] += vars.totalRewardAmount;
  401,48:         pledgeAvailableRewardAmounts[pledgeId] += totalRewardAmount;
  445,48:         pledgeAvailableRewardAmounts[pledgeId] += totalRewardAmount;
  268,48:         pledgeAvailableRewardAmounts[pledgeId] -= rewardAmount;
  
2)USE A MORE RECENT VERSION OF SOLIDITY

Use a solidity version of at least 0.8.2 to get compiler automatic inlining

Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads

Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than revert()/require() strings

Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value
    


File: 2022-10-paladin\contracts\interfaces\IBoostV2.sol:
  1  // SPDX-License-Identifier: MIT
  2: pragma solidity ^0.8.4;
  3  

File: 2022-10-paladin\contracts\interfaces\IVotingEscrow.sol:
  1  // SPDX-License-Identifier: MIT
  2: pragma solidity ^0.8.4;
  3  

File: 2022-10-paladin\contracts\oz\interfaces\IERC20.sol:
  3  
  4: pragma solidity ^0.8.0;
  5  

File: 2022-10-paladin\contracts\oz\libraries\SafeERC20.sol:
  2  
  3: pragma solidity ^0.8.0;
  4  

File: 2022-10-paladin\contracts\oz\utils\Address.sol:
  3  
  4: pragma solidity ^0.8.1;
  5  

File: 2022-10-paladin\contracts\oz\utils\Context.sol:
  3  
  4: pragma solidity ^0.8.0;
  5  

File: 2022-10-paladin\contracts\oz\utils\Ownable.sol:
  3  
  4: pragma solidity ^0.8.0;
  5  

File: 2022-10-paladin\contracts\oz\utils\Pausable.sol:
  3  
  4: pragma solidity ^0.8.0;
  5  

File: 2022-10-paladin\contracts\oz\utils\ReentrancyGuard.sol:
  3  
  4: pragma solidity ^0.8.0;
  5  

File: 2022-10-paladin\contracts\utils\Errors.sol:
  1  // SPDX-License-Identifier: MIT
  2: pragma solidity ^0.8.4;
  3  

3)Division by two should use bit shifting

<x> / 2 is the same as <x> >> 1. While the compiler uses the SHR opcode to accomplish both,
 the version that uses division incurs an overhead of 20 gas due to JUMPs to and from a compiler
 utility function that introduces checks which can be avoided by using unchecked {} around the
 division by two


File: 2022-10-paladin\contracts\WardenPledge.sol
  263,72:         uint256 totalDelegatedAmount = ((bias * boostDuration) + bias) / 2;
  
4)Multiple address mappings can be combined into a single mapping of an address to a struct

Multiple address mappings can be combined into a single mapping of an address to a struct, where appropriate saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot.

 
File: 2022-10-paladin\contracts\WardenPledge.sol
  50,5:     mapping(uint256 => address) public pledgeOwner;
  52,5:     mapping(address => uint256[]) public ownerPledges;
  56,5:     mapping(uint256 => uint256) public pledgeAvailableRewardAmounts;
  67,5:     mapping(address => uint256) public minAmountRewardToken; 

5)Use assembly to check for address(0)
Impact

Saves 6 gas per instance if using assembly to check for zero address  

File: 2022-10-paladin\contracts\WardenPledge.sol
  310,24:         if(receiver == address(0) || rewardToken == address(0)) revert Errors.ZeroAddress();
  310,53:         if(receiver == address(0) || rewardToken == address(0)) revert Errors.ZeroAddress();
  460,24:         if(receiver == address(0)) revert Errors.ZeroAddress();
  492,24:         if(receiver == address(0)) revert Errors.ZeroAddress();
  527,21:         if(token == address(0)) revert Errors.ZeroAddress();
  571,21:         if(token == address(0)) revert Errors.ZeroAddress();
  586,21:         if(token == address(0)) revert Errors.ZeroAddress();
  600,21:         if(chest == address(0)) revert Errors.ZeroAddress();

File: 2022-10-paladin\contracts\oz\utils\Ownable.sol

  70,29:         require(newOwner != address(0), "Ownable: new owner is the zero address");  
  
File: 2022-10-paladin\contracts\utils\Owner.sol
  20,24:         if(newOwner == address(0)) revert OwnerZeroAddress();
  
6)Use calldata instead of memory for function parameters type :

If a reference type function parameter is read-only, it is cheaper in gas to use calldata instead of memory. Calldata is a non-modifiable, non-persistent area where function arguments are stored, and behaves mostly like memory.

There are 5 instances of this issue 
File: 2022-10-paladin\contracts\oz\libraries\SafeERC20.sol
  87,54:     function _callOptionalReturn(IERC20 token, bytes memory data) private {

File: 2022-10-paladin\contracts\oz\utils\Address.sol
  85,49:     function functionCall(address target, bytes memory data) internal returns (bytes memory) {
  85,86:     function functionCall(address target, bytes memory data) internal returns (bytes memory) {
  
  147,55:     function functionStaticCall(address target, bytes memory data) internal view returns (bytes memory) {
  147,97:     function functionStaticCall(address target, bytes memory data) internal view returns (bytes memory) {
  
  174,57:     function functionDelegateCall(address target, bytes memory data) internal returns (bytes memory) {
  174,94:     function functionDelegateCall(address target, bytes memory data) internal returns (bytes memory) {
