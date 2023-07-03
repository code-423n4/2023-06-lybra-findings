### [G-01] – Prefer `CONSTANT` over `IMMUTABLE` for variables not set in the constructor.

For variables that are not initialized within the constructor and expected to remain constant, it's more beneficial to use the `CONSTANT` keyword instead of `IMMUTABLE`. This is especially true for variables that are less than 32 bytes. Unlike `IMMUTABLE` variables, which reserve a full 32 bytes in the contract bytecode regardless of the actual size required, `CONSTANT` variables are never padded. This means that a `CONSTANT`, irrespective of its size, won't take up unnecessary space, resulting in more efficient bytecode and potential gas savings. For instance, an immutable variable defined as uint8 will still be allocated 32 bytes.

https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L18

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L21


### [G-02] "Indexing" variables inside an Event (up to 3 indexes) saves GAS.

Using the `indexed` keyword for value types such as uint, bool, and address saves gas costs. However, this is only the case for value types, whereas indexing bytes and strings are more expensive than their unindexed version.

https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L26-L35

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/configuration/LybraConfigurator.sol#L63-L70

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L34-L44


### [G-03] Duplicated `require()`/`revert()` checks should be refactored to a modifier to save gas.

Within the `LybraPeUSDVaultBase.sol` and the `LybraEUSDVaultBase.sol` contracts :

`require(onBehalfOf != address(0), "TZA")` appears 3 times.
`require(amount > 0, "ZA")` appears 3 times.


### [G-04] Function should revert early to save gas

Within the `liquidation()` function, as defined in the `LybraPeUSDVaultBase.sol` contract, the following require statement is currently implemented: 

`require(PeUSD.allowance(provider, address(this)) > 0, "provider should authorize to provide liquidation EUSD");`

This condition verifies that the provider has given the contract allowance to utilize their PeUSD tokens for liquidation. However, an issue arises if the liquidator attempts to use an amount that exceeds the provider's allowance. This discrepancy will cause the function to revert when the transfer operation is executed later in the function.

Consider modifying the require statement to : 

`require(PeUSD.allowance(provider, address(this)) >= peusdAmount, "provider should authorize to provide liquidation EUSD");`

The same correction can be made in the following function: 

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L160


### [G-05] Avoid redundant check to save gas

The `setBadCollateralRatio()` in the `LybraConfigurator.sol` contract includes the following require: 

`require(newRatio >= 130 * 1e18 && newRatio <= 150 * 1e18 && newRatio <= vaultSafeCollateralRatio[pool] + 1e19, "LNA");`

The last `&&` condition of the require can be removed since `vaultSafeCollateralRatio[pool]` is by definition always greater than 160%.
