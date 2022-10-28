### Don't Initialize Variables with Default Value

#### Impact
Issue Information: Uninitialized variables are assigned with the types default value.

Explicitly initializing a variable with it's default value costs unnecessary gas.

Example
🤦 Bad:

uint256 x = 0;
bool y = false;
🚀 Good:

uint256 x;
bool y;

#### Findings:
```
2022-10-paladin/contracts/WardenPledge.sol::547 => for(uint256 i = 0; i < length;){
```
#### Tools used
Manual



---------------------------------------------------------------------------------------------------------------------------------------------------------------------





### Cache Array Length Outside of Loop

#### Impact
Issue Information: Caching the array length outside a loop saves reading it on each iteration, as long as the array's length is not changed during the loop.

Example
🤦 Bad:

for (uint256 i = 0; i < array.length; i++) {
    // invariant: array's length is not changed
}
🚀 Good:

uint256 len = array.length
for (uint256 i = 0; i < len; i++) {
    // invariant: array's length is not changed
}

#### Findings:
```
2022-10-paladin/contracts/WardenPledge.sol::154 => return pledges.length;
2022-10-paladin/contracts/WardenPledge.sol::542 => uint256 length = tokens.length;
2022-10-paladin/contracts/WardenPledge.sol::544 => if(length == 0) revert Errors.EmptyArray();
2022-10-paladin/contracts/WardenPledge.sol::545 => if(length != minRewardsPerSecond.length) revert Errors.InequalArraySizes();
2022-10-paladin/contracts/WardenPledge.sol::547 => for(uint256 i = 0; i < length;){
```
#### Tools used
Manual



---------------------------------------------------------------------------------------------------------------------------------------------------------------------





### Use != 0 instead of > 0 for Unsigned Integer Comparison

#### Impact
Issue Information: When dealing with unsigned integer types, comparisons with != 0 are cheaper than with > 0.

Example
🤦 Bad:

// `a` being of type unsigned integer
require(a > 0, "!a > 0");
🚀 Good:

// `a` being of type unsigned integer
require(a != 0, "!a > 0");

#### Findings:
```
2022-10-paladin/contracts/WardenPledge.sol::471 => if(remainingAmount > 0) {
2022-10-paladin/contracts/WardenPledge.sol::504 => if(remainingAmount > 0) {
```
#### Tools used
Manual






---------------------------------------------------------------------------------------------------------------------------------------------------------------------







### Use Shift Right/Left instead of Division/Multiplication if possible

#### Impact
Issue Information: A division/multiplication by any number x being a power of 2 can be calculated by shifting log2(x) to the right/left.

While the DIV opcode uses 5 gas, the SHR opcode only uses 3 gas. Furthermore, Solidity's division operation also includes a division-by-0 prevention which is bypassed using shifting.

Example
🤦 Bad:

uint256 b = a / 2;
uint256 c = a / 4;
uint256 d = a * 8;
🚀 Good:

uint256 b = a >> 1;
uint256 c = a >> 2;
uint256 d = a << 3;

#### Findings:
```
2022-10-paladin/contracts/WardenPledge.sol::24 => uint256 public constant WEEK = 7 * 86400;
2022-10-paladin/contracts/WardenPledge.sol::263 => uint256 totalDelegatedAmount = ((bias * boostDuration) + bias) / 2;
```
#### Tools used
Manual


