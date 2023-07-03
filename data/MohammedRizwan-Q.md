## Summary

### Low Risk Issues
|Number|Issue|Instances| |
|-|:-|:-:|:-:|
| [L&#x2011;01] | Violation of checks, Effects, Interaction Pattern in stakerewardV2pool.stake() | 1 |
| [L&#x2011;02] | Prevent reward ratio from rounding to 0 in stakerewardV2pool.notifyRewardAmount() | 1 |
| [L&#x2011;03] | Avoid self transfer shares in _transferShares() | 1 |
| [L&#x2011;04] | Array length not checked in LybraConfigurator.setTokenMiner() | 1 |

### Low Risk Issues
### [L&#x2011;01]  Violation of checks, Effects, Interaction Pattern(CEI) in stakerewardV2pool.stake()
Always CEI pattern to prevent reentrancy attacks.
In stakerewardV2pool.sol, at L-83 to L-90
There is 1 instance of this issue:
[Link to code](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/stakerewardV2pool.sol#L83-L90)

### Recommended Mitigation steps

```Solidity

    function stake(uint256 _amount) external updateReward(msg.sender) {
        require(_amount > 0, "amount = 0");                                              // checks
-       bool success = stakingToken.transferFrom(msg.sender, address(this), _amount);
-       require(success, "TF");
        balanceOf[msg.sender] += _amount;                                                // Effects
        totalSupply += _amount;
+       bool success = stakingToken.transferFrom(msg.sender, address(this), _amount);     // Interactions
+       require(success, "TF");
        emit StakeToken(msg.sender, _amount, block.timestamp);
    }
```

### [L&#x2011;02]  Prevent reward ration from rounding to 0 in stakerewardV2pool.notifyRewardAmount()
Add zero value check to prevent this issue.
In stakerewardV2pool.sol at L-132.
There is 1 instance of this issue:
[Link to code](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/stakerewardV2pool.sol#L132-L145)

### Recommended Mitigation steps

```Solidity
File: contracts/lybra/miner/stakerewardV2pool.sol

    function notifyRewardAmount(uint256 _amount) external onlyOwner updateReward(address(0)) {
+       require(_amount != 0, "invalid amount");
        if (block.timestamp >= finishAt) {
            rewardRatio = _amount / duration;
        } else {
            uint256 remainingRewards = (finishAt - block.timestamp) * rewardRatio;
            rewardRatio = (_amount + remainingRewards) / duration;
        }

        require(rewardRatio > 0, "reward ratio = 0");

        finishAt = block.timestamp + duration;
        updatedAt = block.timestamp;
        emit NotifyRewardChanged(_amount, block.timestamp);
    }
```

### [L&#x2011;03]  Avoid self transfer shares in _transferShares()
In EUSD.sol at L-391, Ensure msg.sender should not be equal to recipient address.
There is 1 instance of this issue:
[Link to code](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/EUSD.sol#L391-L400)

### Recommended Mitigation steps

```Solidity
File: 
    function _transferShares(address _sender, address _recipient, uint256 _sharesAmount) internal {
+       require(_sender != _recipient, "self transfer not allowed");
        require(_sender != address(0), "TRANSFER_FROM_THE_ZERO_ADDRESS");
        require(_recipient != address(0), "TRANSFER_TO_THE_ZERO_ADDRESS");

        uint256 currentSenderShares = shares[_sender];
        require(_sharesAmount <= currentSenderShares, "TRANSFER_AMOUNT_EXCEEDS_BALANCE");

        shares[_sender] = currentSenderShares.sub(_sharesAmount);
        shares[_recipient] = shares[_recipient].add(_sharesAmount);
    }
```

### [L&#x2011;04] Array length not checked in LybraConfigurator.setTokenMiner()
In LybraConfigurator.sol at 235.
There is 1 instance of this issue:
[Link to code](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/configuration/LybraConfigurator.sol#L235-L240)

### Recommended Mitigation steps

```Solidity

    function setTokenMiner(address[] calldata _contracts, bool[] calldata _bools) external checkRole(TIMELOCK) {
+       require(_contracts.length > 0 ,"array must be greater than 0");
+       require(_bools.length > 0 ,"array must be greater than 0");
+       require(_contracts.length == _bools.length > 0 ,"array length must match");
        for (uint256 i = 0; i < _contracts.length; i++) {
            tokenMiner[_contracts[i]] = _bools[i];
            emit tokenMinerChanges(_contracts[i], _bools[i]);
        }
    }
```