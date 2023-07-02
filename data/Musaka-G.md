# Table of findings
|          |                                               |
|----------|-----------------------------------------------|
| **G-01** | Make a modifier to check for collateral ratio |

# Findings

**[G-01]** Make a modifier to check for collateral ratio 
Collateral ratio is checked in a couple of function is bolt [`LybraPeUSDVaultBase`](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol) and [`LybraEUSDVaultBase`](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol), to lower the total gas, you can make it as a modifier
**Example:**
```jsx
modifier checkColl(address who, uint coll) {
   uint256 providerCollateralRatio = (depositedAsset[who] * assetPrice * 100) / borrowed[who];
   require(providerCollateralRatio >= coll, "Inseficient collateral ration");
  }
```
You can use it in [`rigidRedemption`](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L157-L168) as:
```jsx
 function rigidRedemption(address provider, uint256 peusdAmount) external virtual checkColl(provider,100e18){}
```