Low findings

1.in _pledge()
on line 237: no adequite checks on endTimestamp-block.timestamp
consider adding a if() revert check

2. in createPledge()
line 319, endTimestamp needs to be checked, that is bigger than block.timestamp
consider adding a if() revert check

Gas & QA: function pledgePercent
if statement on line 207, it's recommended to use >=, than > in solidity


