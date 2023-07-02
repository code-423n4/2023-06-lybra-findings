# The LybraConfigurator.setBadCollateralRatio function emits incorrect event. It should emit BadCollaterialRatioChanged instead of SafeCollateralRatioChanged.
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/configuration/LybraConfigurator.sol#L126C1-L130
```
    function setBadCollateralRatio(address pool, uint256 newRatio) external onlyRole(DAO) {
        require(newRatio >= 130 * 1e18 && newRatio <= 150 * 1e18 && newRatio <= vaultSafeCollateralRatio[pool] + 1e19, "LNA");
        vaultBadCollateralRatio[pool] = newRatio;
        emit SafeCollateralRatioChanged(pool, newRatio);
    }
```

# The EUSDMiningIncentives.notifyRewardAmount function call can be used to significantly reduce rewardRatio.
1. Let's assume duration=1_000_000
 Admin calls EUSDMiningIncentives.notifyRewardAmount(1_000_000)
rewardRatio = 1_000_000 / 1_000_000 = 1
2. After half of the duration admin calls EUSDMiningIncentives.notifyRewardAmount(2)
remainingRewards = 500_000
rewardRatio = ((1_000_000 - 500_000) + 2) / 1_000_000 = 0.502
