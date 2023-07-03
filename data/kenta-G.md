## 1 use constant instead of immutable.

The state variable vaultType will be not set in the constructor, so this can be constant to save gas costs.

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L18
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L30

## 2 use constant instead of immutable.

The state variable badCollateralRatio will be not set in the constructor, so this can be constant to save gas costs.

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L21C30-L21C48
