## [G-1] Refactor to avoid emitting already available data in the transaction - block.timestamp.

Refactor code to remove `block.timestamp` from the emitted events because it is already available in the transaction. 
This would save loading data into memory (potentially avoiding memory expansion costs) and Glogdata (8 gas) * bytes emitted.

Files: Almost all events emitted in the codebase contain block.timestamp which is not necessary.
- https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/LybraRETHVault.sol#L38
- https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/configuration/LybraConfigurator.sol#L139
- https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/configuration/LybraConfigurator.sol#L149
- https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/ProtocolRewardsPool.sol#L106
- https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/ProtocolRewardsPool.sol#L97
- https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/ProtocolRewardsPool.sol#L146
- https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/ProtocolRewardsPool.sol#L214
- https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/LybraRETHVault.sol#L38
- https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/LybraStETHVault.sol#L89
- https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/LybraWbETHVault.sol#L31
- https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/LybraWstETHVault.sol#L45
- https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L85
- https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L111
- https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L175
- https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L210
- https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L219

Code: 
```
emit DepositEther(msg.sender, address(collateralAsset), msg.value, balance - preBalance, block.timestamp); 
```

## [G-2] Require statements used for validation of input parameters should be at the top.

The second `require` statement on line 249 of the `notifyRewardAmount` function of ProtocolRewardsPool.sol file should be at the top so that transaction is reverted early when validations are not met.

File: https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/ProtocolRewardsPool.sol#L230


```
function notifyRewardAmount(uint256 amount, uint256 tokenType) external {
        require(msg.sender == address(configurator));
        if (totalStaked() == 0) return;
        require(amount > 0, "amount = 0"); //@audit require statement should be at the top
        if (tokenType == 0) {
            uint256 share = IEUSD(configurator.getEUSDAddress()).getSharesByMintedEUSD(amount);
            rewardPerTokenStored = rewardPerTokenStored + (share * 1e18) / totalStaked();
        } else if (tokenType == 1) {
            ERC20 token = ERC20(configurator.stableToken());
            rewardPerTokenStored = rewardPerTokenStored + (amount * 1e36 / token.decimals()) / totalStaked();
        } else {
            rewardPerTokenStored = rewardPerTokenStored + (amount * 1e18) / totalStaked();
        }
    }
```

## [G-3] The result of function call should be cached rather than recalling the function

The `totalStaked()` function is called 3 times in the `notifyRewardAmount` function of ProtocolRewardsPool.sol file. The `totalStaked()` should be called once and cached then the cached result should be used in the 3 places.

File: https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/ProtocolRewardsPool.sol#L229

```
function notifyRewardAmount(uint256 amount, uint256 tokenType) external {
        require(msg.sender == address(configurator));
        if (totalStaked() == 0) return;//@audit totalStaked() called 3 times in this function.
        require(amount > 0, "amount = 0"); 
        if (tokenType == 0) {
            uint256 share = IEUSD(configurator.getEUSDAddress()).getSharesByMintedEUSD(amount);
            rewardPerTokenStored = rewardPerTokenStored + (share * 1e18) / totalStaked();
        } else if (tokenType == 1) {
            ERC20 token = ERC20(configurator.stableToken());
            rewardPerTokenStored = rewardPerTokenStored + (amount * 1e36 / token.decimals()) / totalStaked();
        } else {
            rewardPerTokenStored = rewardPerTokenStored + (amount * 1e18) / totalStaked();
        }
    }
```

## [G-4] Duplicated require/revert checks should be refactored to a modifier or function 

Duplicated require/revert checks should be refactored to a modifier or function to save deployment gas.

File: 
- https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/EUSDMiningIncentives.sol#L106
- https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/EUSDMiningIncentives.sol#L111


```
function setExtraRatio(uint256 ratio) external onlyOwner {
        require(ratio <= 1e20, "BCE"); //@audit duplicated require
        extraRatio = ratio;
    }

    function setPeUSDExtraRatio(uint256 ratio) external onlyOwner {
        require(ratio <= 1e20, "BCE"); //@audit duplicated require
        peUSDExtraRatio = ratio;
    }
```

## [G-5] Use calldata instead of memory for function arguments that do not get mutated

Use calldata instead of memory for the `_pools` parameter in the `setPools` function of the EUSDMiningIncentives.sol file.

File: https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/EUSDMiningIncentives.sol#L93

```
                 //@audit use calldata for the _pools parameter
function setPools(address[] memory _pools) external onlyOwner {
        
        for (uint256 i = 0; i < _pools.length; i++) {
            require(configurator.mintVault(_pools[i]), "NOT_VAULT");
        }
        pools = _pools;
    }

```

