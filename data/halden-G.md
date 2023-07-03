# [G-01] Use unchecked {} block when is possible
It is previous checked: [esLBRBoost](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol#L67)
LybraEUSDVaultBase: [103](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L103), [282](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L282)

# [G-02] Do not use block.timestamp in emitting of event
Instance LybraEUSDVaultBase: [111](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L111), [263](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#263), [285](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L285)