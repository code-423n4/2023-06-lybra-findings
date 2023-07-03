# [L-01] Do not use hard-coded address
Instances: [EUSDMiningIncentives](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L153), [LybraRETHVault](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraRETHVault.sol#L18)

# [L-02] Do not set duration to zero
If new duration is eqaul to 0 than will occur zero division error in `notifyRewardAmount` function [stakerewardV2pool](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L123)

# [L-03] Use modifier when is possible
Instance: [LybraRETHVault](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraRETHVault.sol#L42)

# [L-04] Not handled result from function
The return value from function `EUSD.transferShares` is not handled properly. Instance: [120](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L120), [131](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L131)

# [L-05] Zero convert to PeUSD is possible
If `eusdAmount` is equal to zero than zero converting to PeUSD is possible in `convertToPeUSD` function. 
Instance: [PeUSDMainnetStableVision](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L79)

# [L-06] Wrong event argument
Wrong event argument is used in emitting of `Flashloaned` event. It should be `shareAmount` instead of `eusdAmount`
Instance: [PeUSDMainnetStableVision](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L138)

# [L-06] Fee upper limit is missed
Fee upper limit is missed when new value for fee is setted. Fee for flashloan can be updated to `10_000` and to get all shares of user. 
Instance: [PeUSDMainnetStableVision](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#157)

# [L-07] Wrong commented address
Anothor address is used for WBETH. Instance: [LybraWbETHVault](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWbETHVault.sol#L16)

# [L-08] Missed check for zero amount
Regarding comment above function `burn` the given amount from user should be bigger than zero. [LybraEUSDVaultBase](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L137)

# [L-09] Possible array length mismatch
It is possible length mismatch in `setTokenMiner` function for arrays `_contracts` and `_bools`. [LybraConfigurator](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L235)