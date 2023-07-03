## cache storage variable peUSDExtraRatio before looping to avoid SLOAD(100) opcode in every loop
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L142

## To save repeated SLOAD(100) gas cost, cache result of time2fullRedemption[user] and lastWithdrawTime[user]
3 instance of this:
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L92
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L156
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L162

# save result of:
### store result of borrowed[_onBehalfOf] + totalFee to avoid repeating same operation and SLOAD(100)
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L196

### save the result of configurator.vaultKeeperRatio(address(this)) * 1e18 to avoid repeating same operation and SLOAD(100)
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L204
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L205

### save result of reward - eUSDShare rather than repeating same operation
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L201

### save result of payAmount - income rather than repeating same operation
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L75
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L78

### The result of function *getDutchAuctionDiscountPrice()* should be cached rather than re-calling the function
```solidity
uint256 payAmount = (((realAmount * getAssetPrice()) / 1e18) * getDutchAuctionDiscountPrice()) / 10000;
```
```solidity
emit LSDValueCaptured(realAmount, payAmount, getDutchAuctionDiscountPrice(), block.timestamp);
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L66
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L94

## rewardPerToken() is executed twice in the same single transaction
In the modifier *updateReward()* , rewardPerToken() is called twice
first here:
```solidity
rewardPerTokenStored = rewardPerToken();
```
and still called in the *earned()* function
```solidity
modifier updateReward(address _account) {
        rewardPerTokenStored = rewardPerToken();
        updatedAt = lastTimeRewardApplicable();

        if (_account != address(0)) {
            rewards[_account] = earned(_account);
            userRewardPerTokenPaid[_account] = rewardPerTokenStored;
            userUpdatedAt[_account] = block.timestamp;
        }
        _;
    }
```
```solidity
function earned(address _account) public view returns (uint256) {
        return ((balanceOf[_account] * getBoost(_account) * (rewardPerToken() - userRewardPerTokenPaid[_account])) / 1e38) + rewards[_account];
    }
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L56

## cache storage variable totalsupply
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L75

## These state variables should be set to constant
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol#L15
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/esLBR.sol#L20

## Dead code, if condition will always pass since amount is already calculated to be always >= totalFee
This block of code will never get executed
```solidity
 else {
            feeStored[_onBehalfOf] = totalFee - amount;
            PeUSD.transferFrom(_provider, address(configurator), amount);
        }
```
rather, only the if block will ever get executed because amount will never be less than totalFee due to the fact that it was already checked before assigning, here:
```solidity
uint256 amount = borrowed[_onBehalfOf] + totalFee >= _amount ? _amount : borrowed[_onBehalfOf] + totalFee;
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L196
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L201

## cache the value of depositedAsset[onBehalfOf] rather than re executing SLOAD repeatedly
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L156
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L159

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L190
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L192