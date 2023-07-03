| Issues Summarize|
|-----------------|

| Risk    | Issues Details                                                                                                |
|---------|---------------------------------------------------------------------------------------------------------------|
| [QA-01] | Owner may delete all the pools unintentionally                                                                |
| [QA-02] | Loss of precision due to division before multiplication                                                       |

### [QA-01] Owner may delete all the pools unintentionally

#### Impact

The [`EUSDMiningIncentives.setPools()`](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L93-L98) function has a bad design where all pools can be unintentionally deleted. This can render the contract unusable if the owner adds a single pool.

```solidity
    function setPools(address[] memory _pools) external onlyOwner {
        for (uint i = 0; i < _pools.length; i++) {
            require(configurator.mintVault(_pools[i]), "NOT_VAULT");
        }
        pools = _pools;
    }
```

The current implementation overwrites the existing pool list with the new input, causing an unintentional deletion of all pools except the ones provided as arguments.

#### Tools Used

Manual Review

#### Recommended Mitigation Steps

We rcommend updating the function as follow:

```solidity
    function setPools(address[] memory _pools) external onlyOwner {
        for (uint i = 0; i < _pools.length; i++) {
            require(configurator.mintVault(_pools[i]), "NOT_VAULT");
            pools.push(_pools[i]);
        }
    }
```

### [QA-02] Loss of precision due to division before multiplication

#### Impact

`collateralAmount` incurs a precision loss due to division before multiplication. As a result, users will receive less collateral than they are entitled to.

#### Proof of Concept

```solidity
        uint256 collateralAmount = (((peusdAmount * 1e18) / assetPrice) * (10000 - configurator.redemptionFee())) / 10000;
```

- [LybraPeUSDVaultBase.sol#L164](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L164)

```solidity
        uint256 collateralAmount = (((eusdAmount * 1e18) / assetPrice) * (10000 - configurator.redemptionFee())) / 10000;
```

- [LybraEUSDVaultBase.sol#L239C8-L239C8](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L239C8-L239C8)

#### Tools Used

Manual Review

#### Recommended Mitigation Steps

We recommend multiplying before dividing.
