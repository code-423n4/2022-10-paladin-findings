### L-01 `WardenPledge` inherits Ownable instead of Owner

- [https://github.com/code-423n4/2022-10-paladin/blob/66f9fcc813e220906a13e779b3198891e30459cc/contracts/WardenPledge.sol#L18](https://github.com/code-423n4/2022-10-paladin/blob/66f9fcc813e220906a13e779b3198891e30459cc/contracts/WardenPledge.sol#L18)

The contract imports `Owner.sol` but inherits `Ownable` - should both import and inherit `Owner`

### L-2 Use OpenZeppelin’s ****`Ownable2Step`** instead of inheriting from your own implementation for a 2 step ownership transfer

- [https://github.com/code-423n4/2022-10-paladin/blob/66f9fcc813e220906a13e779b3198891e30459cc/contracts/WardenPledge.sol#L18](https://github.com/code-423n4/2022-10-paladin/blob/66f9fcc813e220906a13e779b3198891e30459cc/contracts/WardenPledge.sol#L18)

The codebase makes heavy use of OpenZeppelin’s contracts but it has implemented the two-step ownership transfer by itself. OpenZeppelin recently added it with `Ownable2Step.sol` - use it instead

### L-3 Not emitting `ClosePledge` event in `retrievePledgeRewards`

- [https://github.com/code-423n4/2022-10-paladin/blob/66f9fcc813e220906a13e779b3198891e30459cc/contracts/WardenPledge.sol#L469](https://github.com/code-423n4/2022-10-paladin/blob/66f9fcc813e220906a13e779b3198891e30459cc/contracts/WardenPledge.sol#L469)

`retrievePledgeRewards` will close a Pledge if it hasn’t been closed yet, but it does not emit the `ClosePledge` event - add it

### L-4 Misleading docs in `retrievePledgeRewards`

- [https://github.com/code-423n4/2022-10-paladin/blob/66f9fcc813e220906a13e779b3198891e30459cc/contracts/WardenPledge.sol#L451](https://github.com/code-423n4/2022-10-paladin/blob/66f9fcc813e220906a13e779b3198891e30459cc/contracts/WardenPledge.sol#L451)

 Docs for `retrievePledgeRewards` say the method retrieves reward from a closed pledge but actually it retrieves them from an expired pledge ONLY, and if it is not closed it closes it. Update docs with the actual action of the method

### L-5 `pledgePercent` does not actually use the `percent` argument as a percentage value but as bps value (it divides by 10 000)

- [https://github.com/code-423n4/2022-10-paladin/blob/66f9fcc813e220906a13e779b3198891e30459cc/contracts/WardenPledge.sol#L206](https://github.com/code-423n4/2022-10-paladin/blob/66f9fcc813e220906a13e779b3198891e30459cc/contracts/WardenPledge.sol#L206)

The `pledgePercent()`method gives the impression with its NatSpec and naming that the `percent` parameter is a percentage value - from 1 to 100. But it is actually used as a bps value because it is divided by 10000. This can result in unexpected behaviour from a UX standpoint, update naming and docs

### NC-01 Remove `/** @notice Event emitted when xx */` comment

- [https://github.com/code-423n4/2022-10-paladin/blob/66f9fcc813e220906a13e779b3198891e30459cc/contracts/WardenPledge.sol#L84](https://github.com/code-423n4/2022-10-paladin/blob/66f9fcc813e220906a13e779b3198891e30459cc/contracts/WardenPledge.sol#L84)

The comment is copy-pasted 13 times but it does not bring any value to the reader/auditor/dev - remove it

### NC-02 Missing param from NatSpec in constructor

- [https://github.com/code-423n4/2022-10-paladin/blob/66f9fcc813e220906a13e779b3198891e30459cc/contracts/WardenPledge.sol#L134](https://github.com/code-423n4/2022-10-paladin/blob/66f9fcc813e220906a13e779b3198891e30459cc/contracts/WardenPledge.sol#L134)

The `_chestAddress` parameter is missing in the NatSpec of the constructor - add it

### NC-03 Change `public` to `private` for storage fields with explicit getter function

- [https://github.com/code-423n4/2022-10-paladin/blob/66f9fcc813e220906a13e779b3198891e30459cc/contracts/WardenPledge.sol#L47](https://github.com/code-423n4/2022-10-paladin/blob/66f9fcc813e220906a13e779b3198891e30459cc/contracts/WardenPledge.sol#L47)
- [https://github.com/code-423n4/2022-10-paladin/blob/66f9fcc813e220906a13e779b3198891e30459cc/contracts/WardenPledge.sol#L52](https://github.com/code-423n4/2022-10-paladin/blob/66f9fcc813e220906a13e779b3198891e30459cc/contracts/WardenPledge.sol#L52)

Both the `pledges` and `ownerPledges` storage fields are `public` which automatically generates a getter for them, but the code also specifies explicit getters with different names for them. Change their visibility to `private` so there is no automatically generated getters for them

### NC-04 Validate function arguments in the start of the method, before any business logic

- [https://github.com/code-423n4/2022-10-paladin/blob/66f9fcc813e220906a13e779b3198891e30459cc/contracts/WardenPledge.sol#L237](https://github.com/code-423n4/2022-10-paladin/blob/66f9fcc813e220906a13e779b3198891e30459cc/contracts/WardenPledge.sol#L237)

In `_pledge()` the code does `uint256 boostDuration **=** endTimestamp **-** **block**.timestamp;` calculation before actually completely valudate the `amount` argument. Move that `boostDuration` math further below, after all validations are done

### NC-05 Lots of typos in the code

Words with typos in `WardenPledge.sol`:

- `protocalFeeRatio` → `protocolFeeRatio`
- `Creates the contract, set the given base parameters` → `Creates the contract, sets the given base parameters`
- `taget` → `target`
- `balacne` → `balance`
- `ot` → `to`
- `feeamount` → `fee amount`
- `reards` → `rewards`
- `minmum` → `minimum`
- `Platfrom` → `Platform`
- `Address tof the EC2O token` → `Address of the ERC20 token`