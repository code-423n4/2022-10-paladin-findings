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