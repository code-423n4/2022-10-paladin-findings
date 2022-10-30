### [QA-01] Use pagination for ***getAllPledges()*** function

This function returns array of **all** ``Pledge`` structures which is a fairly big structure.
I think this is a good case for using pagination. Something like this:

```
function getAllPledges(uint256 cursor, uint256 howMany) 
    external view 
    returns(Pledge[] memory values, uint256 newCursor) {
            
        uint256 length = howMany;
        if (length > pledges.length - cursor) {
            length = pledges.length - cursor;
        }

        values = new Pledge[](length);
        for (uint256 i; i < length; ) {
            values[i] = pledges[cursor + i];

            unchecked{ ++i; }
        }

        return (values, cursor + length);
}
```

### [QA-02] Can remove ``WEEK`` constant state variable
Solidity provides some native units for dealing with time. The WEEK constant its only used in one place when calculating the rounded down timestamp.

Instead of using this :

```
    uint256 public constant WEEK = 7 * 86400;

    function _getRoundedTimestamp(uint256 timestamp) view public returns(uint256) {
        return (timestamp / WEEK) * WEEK;
    }
```

You can use something like this:

```
function _getRoundedTimestamp(uint256 timestamp) view public returns(uint256) {
        return (timestamp / 1 weeks) * 1 weeks;
    }
```

### [LOW-01] ***retrievePledgeRewards*** is not doing what the comment says is doing

The comment for this functions says that the function will : ``Retrieves all non distributed rewards from a closed Pledge``
But the function 
1. is not verifying a Pledge is closed and then retrieving the rewards
2. but is closing a Pledge and then retrieving rewards (this action can be repeated)

The only difference with ``closePledge`` function is that the ``closePledge`` will check if a Pledge is closed and revert in the case it is closed. 

I don't think you need both functions because a user cannot pledge a closed Pledge so there is no point to having both. And also in closing the pledge with ``closePledge`` you are also retrieving the rewards as in (as the name would suggest) ``retrievePledgeRewards``.


