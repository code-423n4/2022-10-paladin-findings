## Right shift or Left shift instead of dividing or multiplying by powers of two
### Lines
- WardenPledge.sol:263

## Consider marking constants as private
Marking constant variables in storage as constant saves gas. Unless a constant variable should be easily accessible by another protocol or offchain logic, consider marking it as private.
### Lines
- WardenPledge.sol:22
- WardenPledge.sol:24
- WardenPledge.sol:23

## Mark functions as payable (with discretion)
You can mark public or external functions as payable to save gas. Functions that are not payable have additional logic to check if there was a value sent with a call, however, making a function payable eliminates this check. This optimization should be carefully considered due to potentially unwanted behavior when a function does not need to accept ether.
### Lines
- WardenPledge.sol:636
- WardenPledge.sol:488
- WardenPledge.sol:653
- WardenPledge.sol:456
- WardenPledge.sol:163
- WardenPledge.sol:195
- WardenPledge.sol:599
- WardenPledge.sol:612
- WardenPledge.sol:414
- WardenPledge.sol:368
- WardenPledge.sol:560
- WardenPledge.sol:172
- WardenPledge.sol:643
- WardenPledge.sol:299
- WardenPledge.sol:153
- WardenPledge.sol:206
- WardenPledge.sol:541
- WardenPledge.sol:570
- WardenPledge.sol:625
- WardenPledge.sol:585

## Use assembly to check for address(0)
### Lines
- WardenPledge.sol:600
- WardenPledge.sol:571
- WardenPledge.sol:310
- WardenPledge.sol:492
- WardenPledge.sol:460
- WardenPledge.sol:310
- WardenPledge.sol:586
- WardenPledge.sol:527

## Mark storage variables as `constant` if they never change.
### Lines
- WardenPledge.sol:79


## Use assembly for math (add, sub, mul, div)

Use assembly for math instead of Solidity. You can check for overflow/underflow in assembly to ensure safety. If using Solidity versions < 0.8.0 and you are using Safemath, you can gain significant gas savings by using assembly to calculate values and checking for overflow/underflow.
### Lines
- WardenPledge.sol:432
- WardenPledge.sol:237
- WardenPledge.sol:263
- WardenPledge.sol:327
- WardenPledge.sol:256
- WardenPledge.sol:263
- WardenPledge.sol:327
- WardenPledge.sol:245
- WardenPledge.sol:433
- WardenPledge.sol:388
- WardenPledge.sol:209
- WardenPledge.sol:328
- WardenPledge.sol:24
- WardenPledge.sol:182
- WardenPledge.sol:319
- WardenPledge.sol:430
- WardenPledge.sol:431
- WardenPledge.sol:327
- WardenPledge.sol:432
- WardenPledge.sol:387
- WardenPledge.sol:433
- WardenPledge.sol:265
- WardenPledge.sol:432
- WardenPledge.sol:385
- WardenPledge.sol:257
- WardenPledge.sol:325
- WardenPledge.sol:209
- WardenPledge.sol:263
- WardenPledge.sol:182
- WardenPledge.sol:387
- WardenPledge.sol:328
- WardenPledge.sol:265
- WardenPledge.sol:387
- WardenPledge.sol:388




