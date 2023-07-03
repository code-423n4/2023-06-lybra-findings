### [L-01] – Consider checking that transfer to `address(this)` is disabled

Functions allowing users to specify the receiving address should check whether the referenced address is not `address(this)`. This would prevent tokens to remain lock inside the contract.  An example of the potential for loss by leaving this open is the EOS token smart contract where more than 90,000 tokens are stuck at the contract address.

https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L82

https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L96

https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L110

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L126

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L140



### [L-02] `liquidation()` function is subject to front-running.

Lybra's` liquidation()` function, as defined in the `LybraPeUSDVaultBase.sol` contract, is susceptible to front-running. In this context, Users can watch the mempool for liquidation transactions targeting their positions. Once identified, they could launch transactions with higher gas prices, ensuring their execution before the impending liquidation. They can effectively prevent the liquidation by either:
1.	Calling the `depositAssetToMint()` function with an assetAmount sufficient to increase their collateral ratio to 150%.
2.	Calling the `burn()` function to decrease their borrowed amount, thus increasing their collateral ratio to 150%.

This vulnerability poses a potential risk for Lybra liquidators, as users could evade a liquidation fee of +-10% (11/10-1) on their deposited collateral amount. It also disrupts the expected system behavior, where positions below the safe collateral threshold are meant to be liquidated.

Lines of code:
https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L125-L146

### [L-03] Lybra Smart Contracts Malfunction with Collateral Assets of Decimal Precision Lower Than 18

The logic of Lybra's smart contracts can malfunction when interacting with collateral assets that use a decimal precision lower than 18. This is due to the hard-coded assumption of an 18-decimal standard in various computations, which is not universally applicable to all ERC20 tokens. If a token with lower decimal precision is used as collateral, it will lead to imprecise calculations, causing significant discrepancies in value assessments and interactions.

https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L132

https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L164

### [L-04] Change in `vaultBadCollateralRatio[]` could trigger a set of liquidations.

The `setBadCollateralRatio()` function empowers the DAO to adjust the `vaultBadCollateralRatio` for any given vault. Nevertheless, abrupt increase in the collateral ratio may trigger a liquidation cascade affecting multiple users. It's advisable to avoid making these changes in a direct, immediate manner. To mitigate such risks, consider implementing a time-delay mechanism that postpones the application of any alterations to the collateral ratio.

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/configuration/LybraConfigurator.sol#L126-L130


### [L-05] – Function Implementation Doesn't Enforce Condition Stated in Comments

Within the `mint()` function of the `LybraPeUSDVaultBase.sol` contract, comments indicate that "individual mint amounts shouldn't surpass 10% when circulation reaches 10_000_000". However, no code within the function enforces this stated condition.

https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L94



### [QA-01] - Bundling related data in a struct within a single mapping improves code structure and clarity

In the `LybraPeUSDVaultBase.sol` and  `LybraEUSDVaultBase.sol` contracts, several mappings indexed by the same key (user address) are used to keep track of user accounting information such as the balance of the deposited asset, borrowed amount, balance of fees, and the last date of fee update. Consolidating these mappings into a single struct within one mapping will significantly improve code structure and clarity, and could also potentially save gas.

https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L21-L24

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/configuration/LybraConfigurator.sol#L38-L47


### [QA-02] – Consider explicitly labeling the visibility of functions and state variables

It is considered as a good practice to always explicitly label the visibility of functions and variables. Labeling the visibility explicitly will make it easier to catch incorrect assumptions about who can call the function or access the variable.

https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L18

https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L22-L24

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/configuration/LybraConfigurator.sol#L46

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/configuration/LybraConfigurator.sol#L58

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L29

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L32


### [QA-03] – Favor invoking existing functions over duplicating logic within new ones.

For better code maintainability and structure, it's advisable to utilize already implemented functions instead of recreating the same logic. For instance, in the `LybraPeUSDVaultBase.sol` contract's `depositAssetToMint()` function, the expression `collateralAsset.balanceOf(address(this))` is used twice. This duplication could be streamlined by employing the `totalDepositedAsset()` function in its place, thereby reducing redundancy and increasing the readability of the code.

https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L44-L46

https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L60

https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L62


### [QA-04] – No check on the value returned by the oracle when calling the `getAssetPrice()` function.

It's considered best practice to verify the value returned by an oracle, regardless of its source. Although Lybra employs the Liquidity `priceFeed` contract, which includes dual oracle sourcing and various sanity checks before returning an ETH price, it is still advisable to perform a cross-check on the returned value for enhanced security.

https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L269

### [QA-05] – Consider using `CONSTANT` variables instead of hardcoded values.

For improved readability and maintainability of the code, consider replacing hardcoded values with `CONSTANT` variables. This approach enhances comprehension of the code's logic and facilitates any future modifications.

https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L135

https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L141

https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L238

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L115

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L164
