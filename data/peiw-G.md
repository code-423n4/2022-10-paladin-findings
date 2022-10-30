- Using `PRIVATE` rather than `PUBLIC` for `CONSTANTS`, saves gas.
	-  If needed, the value can be read from the verified contract source code. Savings are due to the compiler not having to create non-payable getter functions for deployment calldata, and not adding another entry to the method ID table.
	- Instance:
		- https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L22-L24

- It costs more gas to initialize NON-CONSTANT/NON-IMMUTABLE variables to zero than to let the default of zero be applied.
	- Instance:
		- https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L547