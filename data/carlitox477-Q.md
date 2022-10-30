# constructor does not check address != zero for input
It allows to set ```votingEscrow```, ```delegationBoost``` and ```chestAddress``` to address zero.

Impact:
* Setting ```votingEscrow``` to address zero will leave [pledgePercent](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L209) and [createPledge](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L325) unusable.
* Setting ```delegationBoost``` to address zero will leave [_pledge](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L240-L248) unusable. This will provoke functions [pledge](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L196) and [pledgePercent](https://github.com/code-423n4/2022-10-paladin/blob/d6d0c0e57ad80f15e9691086c9c7270d4ccfe0e6/contracts/WardenPledge.sol#L211) to become unusable too.

# WardenPledge#pledgesIndex is so simple that can be declared as external
This function just returns ```pledges.length```. This is simple to do inside the contract, calling this function inside the contract is more expensive than just calling ```pledges.length```.

Mitigation steps: replace every occurency of ```pledgesIndex()``` for ```pledges.length```; and change ```pledgesIndex()```visibility to external

# updateRewardToken can emit invalid event due to lack of check
If ```minAmountRewardToken[token] == minRewardPerSecond``` then the ```minAmountRewardToken[token]``` would not really be updated, leading to a wrong ```UpdateRewardToken``` event emission.

Mitigation steps: Check that ```minAmountRewardToken[token] != minRewardPerSecond```

# updateChest can emit invalid event due to lack of check
If ```chestAddress == chest``` then the ```chestAddress``` would not really be updated, leading to a wrong ```ChestUpdated``` event emission

Mitigation steps: Check that ```chestAddress != chest```

# updateMinTargetVotes can emit invalid event due to lack of check
If ```minTargetVotes == newMinTargetVotes``` then the ```minTargetVotes``` would not really be updated, leading to a wrong ```MinTargetUpdated``` event emission

Mitigation steps: Check that ```minTargetVotes != newMinTargetVotes```

# updatePlatformFee can emit invalid event due to lack of check
If ```protocalFeeRatio == newFee``` then the ```protocalFeeRatio``` would not really be updated, leading to a wrong ```PlatformFeeUpdated``` event emission

Mitigation steps: Check that ```protocalFeeRatio != newFee```