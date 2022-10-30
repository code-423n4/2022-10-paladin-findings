1.
Impact:  expensive gas, because in the line https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L542, the tokens.length is save to a new variable to be used in the for loop, instead of call tokens.length directly in the for loop.

contract A {
    error EmptyArray();
  
   function addMultipleRewardToken(address[] calldata tokens, uint256[] calldata minRewardsPerSecond) external  {
   uint length=token.length;

        if(length == 0) revert EmptyArray();

        for(uint256 i = 0; i < length;){

            unchecked{ ++i; }
        }
    }

}//172003

contract B {
    error EmptyArray();
  
   function addMultipleRewardToken(address[] calldata tokens, uint256[] calldata minRewardsPerSecond) external  {

        if(tokens.length == 0) revert EmptyArray();

        for(uint256 i = 0; i < tokens.length;){

            unchecked{ ++i; }
        }
    }

}//171367

Tools Used
remix

2.2.Functions marked as  payable  are 24 gas cheaper than their counterpart (in non-payable functions, Solidity adds an extra check to ensure msg.value is zero). When users can't mistakenly send ETH to a function (as an example, when there's an onlyOwner modifier or alike), it is safe to mark it as payable.

    function addMultipleRewardToken(address[] calldata tokens, uint256[] calldata minRewardsPerSecond) external onlyOwner {
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L541

    function addRewardToken(address token, uint256 minRewardPerSecond) external onlyOwner {
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L560

    function removeRewardToken(address token) external onlyOwner {
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L585

    function updateChest(address chest) external onlyOwner {
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L599

    function updateMinTargetVotes(uint256 newMinTargetVotes) external onlyOwner {
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L612

    function updatePlatformFee(uint256 newFee) external onlyOwner {
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L625

    function recoverERC20(address token) external onlyOwner returns(bool) {
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L653

Recommended Mitigation Steps
Mark as payable the external functions that have a trusted modifier which prevents users from mistakenly sending Ether.
