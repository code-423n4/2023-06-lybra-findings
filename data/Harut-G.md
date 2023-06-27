File: contracts/OFT/OFTCoreV2.sol

In OFTCoreV2.sol and in all derivatives of this contract change visibility of _ld2sdRatio() function from internal to external for calling outside of the contract.

File: contracts/lybra/token/LBR.sol
File: contracts/lybra/token/PeUSD.sol
File: contracts/lybra/token/PeUSDMainnetStableVision.sol

In these contracts there is no need for circulatingSupply(). Instead we can directly call to totalSupply().