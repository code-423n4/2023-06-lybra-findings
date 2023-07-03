
## QA

### 1 a wrong address for the WbETH.

The following address is not for the WbETH but for the rETH.

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWbETHVault.sol#L16

### 2 a wrong description.

The following description is not fit for the revert condition in the depositAssetToMint function. https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L55
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L69

Use
-  `assetAmount` Must be equal to or greater than 1e18.
instead of
- `assetAmount` Must be higher than 0.

 ### 3 Use > instead of >=.

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L62

The above line checks the balance of the collateral asset after transferring it to this contract. The input assetAmount must be equal to or greater than 1 Ether, so using '>=' as a condition for this validation is incorrect.   

Use > instead of  >=.

### 4 Use the right interface for peUSD.

IEUSD is used for the peUDC in the following line of the LybraConfigurator.sol. Use the IPeUSD interface instead of IEUSD.

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L55
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L100
