1. In LybraConfigurator.sol, there is no need in newRatio <= vaultSafeCollateralRatio[pool] + 1e19 check as it has always to be below 150%. This require statement:

require(newRatio >= 130 * 1e18 && newRatio <= 150 * 1e18 && newRatio <= vaultSafeCollateralRatio[pool] + 1e19, "LNA"); checks whether the newRatio >= 130% and <= 150% and the last check is to make sure that it's lower than
current safeCollateralRatio of the pool + 10%. safeCollateralRatio is always above 160% as defined by the specification so there is no need to check the last condition at all as the second condition would be enough.

Code snippet: https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L127

2. badCollateralRatio variable in LybraEUSDVaultBase.sol can be changed to constant as it's predetermined and not initialized in the constructor (also this can be considered as gas optimization).

Code snippet:

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L21

3. In LybraConfigurator.sol, it's said that EUSD, peUSD tokens can be only initialized once but they're not initialized in the constructor and not marked as immutable. It's better to initialize something only for one time in the constructor and mark as immutable. Technically, they are initialized once but syntactically they can be initialized more times as they are not immutable.

Code snippet:

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L54
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L55

