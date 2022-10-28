- L267/268/383/385/426/429/430/431 - The subtraction operation can be unchecked since it is first validated in the if.

- L79 - As minDelegationTime is already defined with "1 week" and there is no way to change its value, the least expensive thing is to make it constant.

- L471/504 - It is less expensive to validate that uint != 0 than to validate uint > 0
	
- L547 - When we initialize a variable and we want to set its default value, it is not necessary to set it, since it has that value by default.
