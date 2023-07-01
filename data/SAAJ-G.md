 
# Gas Optimizations Report

This report focuses on Lybra contest, in context of various improvements that can be made in terms of gas cost.

Some of the opportunities identified for improving gas efficiency throughout the codebase of Lybra protocol are categorised into 06 main areas; with further multiple instances in each of the category.


# [G-01] Cache array length outside of loop (02 Instances)

If not cached, the solidity compiler will always read the length of the array during each iteration. That is, if it is a storage array, this is an extra sload operation (100 additional extra gas for each iteration except for the first) and if it is a memory array, this is an extra mload operation (3 additional gas for each iteration except for the first).

Link to the Code:
1.	https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L236
2.	https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L94


# [G-02] It's cheaper to declare the variable outside the loop (01 Instances)
Declaring a variable inside a loop result in variable being redeclared during each loop iteration which consume higher gas.
The variable gets reallocated when declared outside loop making it more gas efficient.

Link to the Code:
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L140



# [G-03] Wastage of deployed gas when return is not present for returns function (02 Instances)
Wastage of gas during deployment; when return is absent for named variable when function returns.

Link to the Code:
1.	https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol#L49
2.	https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L115

# [G-04] Use hardcode address instead address(this) (23 Instances)

Instead of using address(this), it is more gas-efficient to pre-calculate and use the hardcoded address.

Link to the code:
1.	https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/GovernanceTimelock.sol#L20
2.	https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWbETHVault.sol#L22
3.	https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWbETHVault.sol#L24
4.	https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraRETHVault.sol#L29
5.	https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraRETHVault.sol#L31
6.	https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSD.sol#L25
7.	https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSD.sol#L46
8.	https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol#L43
9.	https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol#L64
10.	https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L63
11.	https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L85
12.	https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L80
13.	https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L81
14.	https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L82
15.	https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L133
16.	https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L199
17.	https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L195
18.	https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L200
19.	https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L290
20.	https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L295
21.	https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L75
22.	https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L204
23.	https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L301

# [G-05] Use assembly to check for address(0) (16 Instances)
	
Link to the Code:
1.	https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L60
2.	https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L64
3.	https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L367
4.	https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L368
5.	https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L392
6.	https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L393
7.	https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L412
8.	https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L441
9.	https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L460
10.	https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L99
11.	https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L100
12.	https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L76
13.	https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L99
14.	https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L141
15.	https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L83
16.	https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L111



# [G-06] Use calldata instead of memory for function parameters (05 Instances)

Using calldata in external function does not require data to be stored, which reduced the process time as compared to memory. This in return saves gas during calling the data.

Link to the Code:
1.	https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol#L33
2.	https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol#L76
3.	https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol#L98
4.	https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol#L106
5.	https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L93

