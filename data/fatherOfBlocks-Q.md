- L96 - An IncreasePledgeTargetVotes event is created, but it is never used, so it should be removed.

- L256 - A division is made with the variable boostDuration, this parameter is obtained from an operation between an input and the block.timestamp, therefore it should be validated before making the division that the variable is not 0. This is necessary so that if it is reverted it should show a matching message and not just a revert.
