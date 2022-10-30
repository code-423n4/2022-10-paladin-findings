## Summary

### Low Risk Issues List
| Number |Issues Details|Context|
|:----:|:-------|:--:|
|[L-01]| There is no 0 check for very critical addresses in the constructor| 3 |
|[L-02]| Very critical `owner` privileges can cause complete destruction of the project in a possible privateKey exploit | 1 |

Total 2 issues

### Non-Critical Issues List
| Number |Issues Details|Context|
|:--:|:-------|:--:|
| [NC-01 ]| `Function writing` that does not comply with the `Solidity Style Guide` |1 |
| [NC-02] |Need Fuzzing test|1|
| [NC-03] |Use a more recent version of Solidity | 1|
| [NC-04] |Solidity compiler optimizations can be problematic | 1 |
| [NC-05] |NatSpec is missing | 1 |
| [NC-06] |For functions, follow Solidity standard naming conventions| 1 |
| [NC-07] |Missing Event for Critical Parameters Change | 2|
| [NC-08] |Add NatSpec Mapping comment|4 |
| [NC-09] |Same Contract imported twice | 1 |

Total 9 issues


### Suggestions
| Number | Suggestion Details |
|:--:|:-------|
| [S-01] |Add to _blacklist function_ |
| [S-02] | Generate perfect code headers every time |

Total 2 suggestions


### [L-01] There is no 0 check for very critical addresses in the constructor.

With the constructor in the `WardenPledge.sol` file, the initial addresses of `chestAddress` , `minTargetVotes` , `delegationBoost` and `votingEscrow` are set with an argument of type addresses, but there are no check that prevents this addresses from starting with 0.
There is a risk that they  are accidentally initialized to 0

Passing the addresses variables blank causes it to be set as 0 address.

The addresses initialized with 0 by mistake or forgetting cant be changed.

The biggest risk is; Until this value is found to be incorrect, the project can be used, users can trade, use the platform


```solidity

contracts/WardenPledge.sol:
  130      */
  131:     constructor(
  132:         address _votingEscrow,
  133:         address _delegationBoost,
  134:         address _chestAddress,
  135:         uint256 _minTargetVotes
  136:     ) {
  137:         votingEscrow = IVotingEscrow(_votingEscrow);
  138:         delegationBoost = IBoostV2(_delegationBoost);
  139: 
  140:         chestAddress = _chestAddress;
  141: 
  142:         minTargetVotes = _minTargetVotes;
  143:     } 

```


### Tools Used
Manual Code Review

### Recommended Mitigation Steps

Add an if block to the constructor ;

```js

error ZeroAddressError();

        if _votingEscrow = address(0) && _delegationBoost = address(0) && _chestAddress = address(0))
            ZeroAddressError();
}
```

### [L-02]  Very critical `owner` privileges can cause complete destruction of the project in a possible privateKey exploit

The contract’s `owner` is most impartant power role in this project. the owner is able to perform certain privileged activities.

However, `owner` privileges are numerous and there is no timelock structure in the process of using these privileges.

The `owner` is assumed to be an EOA, since the documents do not provide information on whether the `owner`  will be a multisign structure.

In parallel with the private key thefts of the project owners, which have increased recently, this vulnerability has been stated as medium.

Similar vulnerability;
Private keys stolen:

Hackers have stolen cryptocurrency worth around €552 million from a blockchain project linked to the popular online game Axie Infinity, in one of the largest cryptocurrency heists on record. Security issue : PrivateKey of the project officer was stolen:
https://www.euronews.com/next/2022/03/30/blockchain-network-ronin-hit-by-552-million-crypto-heist


### Proof of Concept

`owner` powers;
```solidity
contracts/WardenPledge.sol:
  541:     function addMultipleRewardToken(address[] calldata tokens) external onlyOwner {
  560:     function addRewardToken(address token, uint256 minRewardPerSecond) external onlyOwner {
  570:     function updateRewardToken(address token, uint256 minRewardPerSecond) external onlyOwner {
  585:     function removeRewardToken(address token) external onlyOwner {
  599:     function updateChest(address chest) external onlyOwner {
  612:     function updateMinTargetVotes(uint256 newMinTargetVotes) external onlyOwner {
  625:     function updatePlatformFee(uint256 newFee) external onlyOwner {
  636:     function pause() external onlyOwner {
  643:     function unpause() external onlyOwner {
  653:     function recoverERC20(address token) external onlyOwner returns(bool) {

```

### Recommendation;

1- A timelock contract should be added to use ``owner` ` privileges. In this way, users can be warned in case of a possible security weakness.

2- `owner` can be a Multisign wallet and this part is specified in the documentation


### [N-01] `Function writing` that does not comply with the `Solidity Style Guide`


**Description:**
Order of Functions; ordering helps readers identify which functions they can call and to find the constructor and fallback definitions easier. But there are contracts in the project that do not comply with this.

https://docs.soliditylang.org/en/v0.8.17/style-guide.html

Functions should be grouped according to their visibility and ordered:

 constructor
receive function (if exists)
fallback function (if exists)
external
public
internal
private
within a grouping, place the view and pure functions last

### [N-02] Need Fuzzing test

Project uncheckeds list:

```js

contracts/WardenPledge.sol:
 550:             unchecked{ ++i; }
  551          }
```
The functions used are critical. For this reason, there must be fuzzing tests in the tests of the project. Not seen in tests.

**Recommendation:**
Use should fuzzing test like Echidna.

As Alberto Cuesta Canada said:
_Fuzzing is not easy, the tools are rough, and the math is hard, but it is worth it. Fuzzing gives me a level of confidence in my smart contracts that I didn’t have before. Relying just on unit testing anymore and poking around in a testnet seems reckless now._

https://medium.com/coinmonks/smart-contract-fuzzing-d9b88e0b0a05

### [N-03] Use a more recent version of Solidity


**Description:**
For security, it is best practice to use the latest Solidity version.
For the security fix list in the versions;
https://github.com/ethereum/solidity/blob/develop/Changelog.md


**Recommendation:**
Old version of Solidity is used `(0.8.10)`, newer version can be used `(0.8.17)` 

### [N-04] Solidity compiler optimizations can be problematic


```js
hardhat.config.ts:
  17  
  18: const config: HardhatUserConfig = {
  19:   defaultNetwork: "hardhat",
  20:   solidity: {
  21:     compilers: [
  22:       {
  23:         version: "0.8.10",
  24:         settings: {
  25:           optimizer: {
  26:             enabled: true,
  27:             runs: 200,
  28:           },
  29:         }
  30:       }

```

**Description:**
Protocol has enabled optional compiler optimizations in Solidity.
There have been several optimization bugs with security implications. Moreover, optimizations are actively being developed. Solidity compiler optimizations are disabled by default, and it is unclear how many contracts in the wild actually use them. 

Therefore, it is unclear how well they are being tested and exercised.
High-severity security issues due to optimization bugs have occurred in the past. A high-severity bug in the emscripten-generated solc-js compiler used by Truffle and Remix persisted until late 2018. The fix for this bug was not reported in the Solidity CHANGELOG. 

Another high-severity optimization bug resulting in incorrect bit shift results was patched in Solidity 0.5.6. More recently, another bug due to the incorrect caching of keccak256 was reported.
A compiler audit of Solidity from November 2018 concluded that the optional optimizations may not be safe.
It is likely that there are latent bugs related to optimization and that new bugs will be introduced due to future optimizations.

Exploit Scenario
A latent or future bug in Solidity compiler optimizations—or in the Emscripten transpilation to solc-js—causes a security vulnerability in the contracts.



**Recommendation:**
Short term, measure the gas savings from optimizations and carefully weigh them against the possibility of an optimization-related bug.
Long term, monitor the development and adoption of Solidity compiler optimizations to assess their maturity.

### [N-05] NatSpec is missing 

NatSpec is missing for the `_chestAddress`


```solidity
contracts/WardenPledge.sol:
  122  
  123:     // Constructor
  124: 
  125:     /**
  126:     * @dev Creates the contract, set the given base parameters
  127:     * @param _votingEscrow address of the voting token to delegate
  128:     * @param _delegationBoost address of the contract handling delegation
  129:     * @param _minTargetVotes min amount of veToken to target in a Pledge
  130:     */
  131:     constructor(
  132:         address _votingEscrow,
  133:         address _delegationBoost,
  134:         address _chestAddress,
  135:         uint256 _minTargetVotes

```

### [N-06]  For functions, follow Solidity standard naming conventions
The above codes don't follow Solidity's standard naming convention,
internal and private functions : the mixedCase format starting with an underscore (_mixedCase starting with an underscore)

```solidity
 function safe64(uint256 n) internal pure returns (uint64) {
        if(n > type(uint64).max) revert Errors.NumberExceed64Bits();
        return uint64(n);
    }

```

### [N-07]  Missing Event for Critical Parameters Change

Events help non-contract tools to track changes, and events prevent users from being surprised by changes


```solidity

contracts/WardenPledge.sol:
  635       */
  636:     function pause() external onlyOwner {
  637:         _pause();
  638:     }
  639: 
  640:     /**
  641:      * @notice Unpauses the contract
  642:      */
  643:     function unpause() external onlyOwner {
  644:         _unpause();
  645:     }

```
**Recommendation:**
Add Event-Emit

### [N-08]  Add NatSpec Mapping comment

**Description:**
Add NatSpec comments describing mapping keys and values


**Context:**

```solidity

4 results - 1 file

contracts/WardenPledge.sol:
  50:     mapping(uint256 => address) public pledgeOwner;
  52:     mapping(address => uint256[]) public ownerPledges;
  56:     mapping(uint256 => uint256) public pledgeAvailableRewardAmounts;
  67:     mapping(address => uint256) public minAmountRewardToken;
```
**Recommendation:**

```solidity

 /// @dev    address(user) -> address(ERC0 Contract Address) -> uint256(allowance amount from user)
    mapping(address => mapping(address => uint256)) public allowance;

```

### [N-09]   Same Contract imported twice

The SafeERC20 contract already inherits IERC20, it is also unnecessary to import IERC20.

```solidity

contracts/WardenPledge.sol:
  1  // SPDX-License-Identifier: MIT
  2: pragma solidity 0.8.10;
  3: 
  4: import "./oz/interfaces/IERC20.sol";
  5: import "./oz/libraries/SafeERC20.sol";


```
### [S-01] Add to _blacklist function_

**Description:**
Cryptocurrency mixing service, Tornado Cash, has been blacklisted in the OFAC.
A lot of blockchain companies, token projects, NFT Projects have ```blacklisted``` all Ethereum addresses owned by Tornado Cash listed in the US Treasury Department's sanction against the protocol.
https://home.treasury.owner/policy-issues/financial-sanctions/recent-actions/20220808

Some of these Projects;
 USDC 
 Aave 
 Uniswap
 Balancer
 Infura
 Alchemy 
 Opensea
 dYdX 

For this reason, every project in the Ethereum network must have a blacklist function, this is a good method to avoid legal problems in the future, apart from the current need.

Transactions from the project by an account funded by Tonadocash or banned by OFAC can lead to legal problems.Especially American citizens may want to get addresses to the blacklist legally, this is not an obligation

If you think that such legal prohibitions should be made directly by validators, this may not be possible:
https://www.paradigm.xyz/2022/09/base-layer-neutrality

```The ban on Tornado Cash makes little sense, because in the end, no one can prevent people from using other mixer smart contracts, or forking the existing ones. It neither hinders cybercrime, nor privacy.```

Here is the most beautiful and close to the project example; Manifold

Manifold Contract
https://etherscan.io/address/0xe4e4003afe3765aca8149a82fc064c0b125b9e5a#code

```js
     modifier nonBlacklistRequired(address extension) {
         require(!_blacklistedExtensions.contains(extension), "Extension blacklisted");
         _;
     }
```
Recommended Mitigation Steps add to Blacklist function and modifier.
### [S-02] Generate perfect code headers every time

**Description:**
I recommend using header for Solidity code layout and readability

https://github.com/transmissions11/headers

```js
/*//////////////////////////////////////////////////////////////
                           TESTING 123
//////////////////////////////////////////////////////////////*/
```