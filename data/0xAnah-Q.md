# **Non-Critical Issues**


## [N-01]  A modifier used only once
A modifier used only once and not being inherited should be inline (3 instances)
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L83
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L46
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L50


## [N-02] Use double if statements instead of &&
Using multiple if improves code readability and makes it easier to debug. (5 instances)
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L204
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSD.sol#L46
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol#L64
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L199
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L204


## [N-03] The if statement can be refactored to the ternary operator
The normal if / else statement can be written in a shorthand way using the ternary operator. It increases readability and reduces the number of lines of code. (4 instances)
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L133
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L301
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L312
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L199
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L230


## [N-04] Interface defined and not used
The following interfaces were defined and they were not used in their respective files consider removing or implementing the said interfaces.

- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L9
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWbETHVault.sol#L9


## [N-05] Storage Variable was assigned values at declaration and in constructor
In the [LybraRETHVault](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraRETHVault.sol#L18) file on line 18 the rkPool variable was declared and initialized but it was also assigned a value in the constructor. Since the variable would be assigned a value in the constructor there is no need to assign it a value at declaration. I would recommend that you should either make a choice between assignment at declaration or assignment in the constructor.
