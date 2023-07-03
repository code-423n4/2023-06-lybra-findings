## Summary

### Gas Optimizations
| |Issue|Instances| |
|-|:-|:-:|:-:|
| [G&#x2011;01] | Save gas by preventing zero amount in mint() and burn() | 2 |
| [G&#x2011;02] | Catch function parameters as local variables to save gas | 1 |
| [G&#x2011;03] | Use nested-if and avoid "&&" to save gas | 2 |
| [G&#x2011;04] | Catch proposalId in LybraGovernance.sol within function | 3 |

### [G&#x2011;01]  Save gas by preventing zero amount in mint() and burn()
In esLBR.sol, L-30 to L-43.
There are 2 instances of this issue:
[Link to code](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/esLBR.sol#L30-L43)

### Recommended Mitigation steps
```Solidity
File: contracts/lybra/token/esLBR.sol

    function mint(address user, uint256 amount) external returns (bool) {
+       require(amount != 0, "invalid amount);
        require(configurator.tokenMiner(msg.sender), "not authorized");
        require(totalSupply() + amount <= maxSupply, "exceeding the maximum supply quantity.");
        try IProtocolRewardsPool(configurator.getProtocolRewardsPool()).refreshReward(user) {} catch {}
        _mint(user, amount);
        return true;
    }

    function burn(address user, uint256 amount) external returns (bool) {
+       require(amount != 0, "invalid amount);
        require(configurator.tokenMiner(msg.sender), "not authorized");
        try IProtocolRewardsPool(configurator.getProtocolRewardsPool()).refreshReward(user) {} catch {}
        _burn(user, amount);
        return true;
    }
```

### [G&#x2011;02]  Catch function parameters as local variables to save gas
Caching of a state variable replaces each Gwarmaccess (100 gas) with a much cheaper stack read. 

In esLBRBoost.sol, L-57 to L-69
There is 1 instance of this issue:
[Link to code](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/esLBRBoost.sol#L57-L69)

### Recommended Mitigation steps
```Solidity
File: contracts/lybra/miner/esLBRBoost.sol

    function getUserBoost(address user, uint256 userUpdatedAt, uint256 finishAt) external view returns (uint256) {
+       address _user = user;
+       uint256 _userUpdatedAt = userUpdatedAt;
+       uint256 _finishAt = finishAt;
-        uint256 boostEndTime = userLockStatus[user].unlockTime;
+        uint256 boostEndTime = userLockStatus[_user].unlockTime;
-        uint256 maxBoost = userLockStatus[user].miningBoost;
+        uint256 maxBoost = userLockStatus[_user].miningBoost;

-        if (userUpdatedAt >= boostEndTime || userUpdatedAt >= finishAt) {
+        if (_userUpdatedAt >= boostEndTime || _userUpdatedAt >= _finishAt) {
            return 0;
        }
-        if (finishAt <= boostEndTime || block.timestamp <= boostEndTime) {
+        if (_finishAt <= boostEndTime || block.timestamp <= boostEndTime) {
            return maxBoost;
        } else {
-            uint256 time = block.timestamp > finishAt ? finishAt : block.timestamp;
+            uint256 time = block.timestamp > _finishAt ? _finishAt : block.timestamp;
-            return ((boostEndTime - userUpdatedAt) * maxBoost) / (time - userUpdatedAt);
+            return ((boostEndTime - _userUpdatedAt) * maxBoost) / (time - _userUpdatedAt);
        }
    }
```

### [G&#x2011;03]  Use nested-if else and avoid "&&" to save gas
In LBR.sol at L-64,
There is 1 instance of this issue:
[Link to code](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/LBR.sol#L64)

### [G&#x2011;04]  Catch proposalId in LybraGovernance.sol within function
Catch the proposalId as a local variable in _quorumReached(), _voteSucceeded(), _countVote() to save Gwarmaccess (100 gas).
In LybraGovernance.sol at L-59 to L-76.
There is 2 instance of this issue:
[Link to code](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/governance/LybraGovernance.sol#L59-L76)

In PeUSDMainnetStableVision.sol at L-199
[Link to code](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/PeUSDMainnetStableVision.sol#L199)
