## 1.Use `calldata` instead of `memory` for function parameters
### Impact
In some cases, having function arguments in `calldata` instead of `memory` is more optimal.

Consider the following instance :
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L163
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L172
```
163.         function getUserPledges(address user) external view returns(uint256[] memory){
164.                return ownerPledges[user];
165.             }

172.         function getAllPledges() external view returns(Pledge[] memory){
173.              return pledges;
174.             }
```
In the above instance, the dynamic array arr has the storage location memory. When the function gets called externally, the array values are kept in call data and copied to memory during ABI decoding (using the opcode calldata load and mstore). Consider the following snippet instead:
```
163.         function getUserPledges(address user) external view returns(uint256[] calldata){
164.                return ownerPledges[user];
165.             }

172.         function getAllPledges() external view returns(Pledge[] calldata){
173.              return pledges;
174.             }

```
### Recommended Mitigation Steps
Change `memory` definition with `calldata`.