## Use assembly to check for address(0)  
Saves 6 gas per instance if using assembly to check for address(0)  
e.g.  
  
```  
assembly {  
 if iszero(_addr) {  
  mstore(0x00, "zero address")  
  revert(0x00, 0x20)  
 }  
}  
```  
  
Instances include:
[`contracts/WardenPledge.sol:310`](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L310)
[`contracts/WardenPledge.sol:460`](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L460)
[`contracts/WardenPledge.sol:492`](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L492)
[`contracts/WardenPledge.sol:527`](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L527)
[`contracts/WardenPledge.sol:571`](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L571)
[`contracts/WardenPledge.sol:586`](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L586)
[`contracts/WardenPledge.sol:600`](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L600)

## Write direct outcome, instead of performing mathematical operations   
Instances include:  
[`contracts/WardenPledge.sol:24`](https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L24)

instead of using this:
```
	uint256 public constant WEEK = 7 * 86400;
```
use this:
```
	uint256 public constant WEEK = 604800;  // 7 * 86400
```

## DUPLICATED REQUIRE()/REVERT() CHECKS SHOULD BE REFACTORED TO A MODIFIER OR FUNCTION
Error instances:
NotAllowedToken()
NotPledgeCreator()
ExpiredPledge()
PledgeClosed()
InvalidPledgeID()