# [G-01] A modifier used only once and not being inherited should be inlined to save gas
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L83-L86](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L83-L86)
````solidity
File: contracts/lybra/token/EUSD.sol

83           modifier MintPaused() {
84                require(!configurator.vaultMintPaused(msg.sender), "MPP");
85                _;
86:          }
````
The above modifier is only being called on [Line 411](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L411).        

                  
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L46-L53](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L46-L53)
````solidity
File: contracts/lybra/token/PeUSDMainnetStableVision.sol

46           modifier MintPaused() {
47                   require(!configurator.vaultMintPaused(msg.sender), "MPP");
48                   _;
49:          }
50           modifier BurnPaused() {
51                   require(!configurator.vaultBurnPaused(msg.sender), "BPP");
52                   _;
53:          }
````
The MintPaused modifier is only being called on [Line 63](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L63) and the BurnPaused modifier on [Line 69](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L69).
# [G-02] Immutables should be in uppercase
````solidity
File: contracts/lybra/miner/EUSDMiningIncentives.sol
28:    Iconfigurator public immutable configurator;

30:    IEUSD public immutable EUSD;
````
[EUSDMiningIncentives.sol#L28](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L28)
````solidity
File: contracts/lybra/miner/ProtocolRewardsPool.sol
25:    Iconfigurator public immutable configurator;
    
39:    uint256 immutable exitCycle = 90 days;
````
[ProtocolRewardsPool.sol#L25](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L25)
````solidity
File: contracts/lybra/miner/stakerewardV2pool.sol
18    IERC20 public immutable stakingToken;
19:   IesLBR public immutable rewardsToken;
````
[stakerewardV2pool.sol#L18-L19](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L18-L19)
````solidity
File: contracts/lybra/pools/LybraWstETHVault.sol
22:    Ilido immutable lido;
````
[LybraWstETHVault.sol#L22](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWstETHVault.sol#L22)
````solidity
File: contracts/lybra/pools/base/LybraEUSDVaultBase.sol
18:    IEUSD public immutable EUSD;
19:    IERC20 public immutable collateralAsset;
20:    Iconfigurator public immutable configurator;
21:    uint256 public immutable badCollateralRatio = 150 * 1e18;
22:    IPriceFeed immutable etherOracle;

30:    uint8 immutable vaultType = 0;
````
[LybraEUSDVaultBase.sol#L18-L22](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L18-L22)
````solidity
File: contracts/lybra/pools/base/LybraPeUSDVaultBase.sol
14:    IPeUSD public immutable PeUSD;
15:    IERC20 public immutable collateralAsset;
16:    Iconfigurator public immutable configurator;

18:    uint8 immutable vaultType = 1;
19:    IPriceFeed immutable etherOracle;
````
[LybraPeUSDVaultBase.sol#L14-L16](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L14-L16)
````solidity
File: contracts/lybra/token/esLBR.sol
18:    Iconfigurator public immutable configurator;
````
[esLBR.sol#L18](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/esLBR.sol#L18)
````solidity
File: contracts/lybra/token/EUSD.sol
39:    Iconfigurator public immutable configurator;
````
[EUSD.sol#L39](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L39)
````solidity
File: contracts/lybra/token/LBR.sol
14:    Iconfigurator public immutable configurator;

16:    uint internal immutable ld2sdRatio;
````
[LBR.sol#L14](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol#L14)
````solidity
File: contracts/lybra/token/PeUSD.sol
9:    uint internal immutable ld2sdRatio;
````
[PeUSD.sol#L9](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSD.sol#L9)
````solidity
File: contracts/lybra/token/PeUSDMainnetStableVision.sol
30    IEUSD public immutable EUSD;
31    Iconfigurator public immutable configurator;
32:   uint internal immutable ld2sdRatio;
````
[PeUSDMainnetStableVision.sol#L30-L32](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L30-L32)