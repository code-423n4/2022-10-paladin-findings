## Time Units
According to:

https://docs.soliditylang.org/en/v0.8.14/units-and-global-variables.html

suffixes like `seconds`, `minutes`, `hours`, `days` and `weeks` after literal numbers can be used to specify units of time where seconds are the base unit and units are considered naively in the following way:

1 == 1 seconds
1 minutes == 60 seconds
1 hours == 60 minutes
1 days == 24 hours
1 weeks == 7 days

To avoid human error while making the assignment more verbose, the following constant declarations may respectively be rewritten as:

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L24

```
    uint256 public constant WEEK = 1 weeks;
```
## `block.timestamp` Unreliable
The use of `block.timestamp` as part of the time checks can be slightly altered by miners/validators to favor them in contracts that have logic strongly dependent on them.

Consider taking into account this issue and warning the users that such a scenario could happen. If the alteration of timestamps cannot affect the protocol in any way, consider documenting the reasoning and writing tests enforcing that these guarantees will be preserved even if the code changes in the future.

There are a total of eight instances entailed:

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L229
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L237
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L319
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L380
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L426
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L430
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L463
https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol#L496