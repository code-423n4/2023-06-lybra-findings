|     | Issues                                                                                                                  | Instances |
| :-- | :---------------------------------------------------------------------------------------------------------------------- | --------: |
| 01  | Return values of `approve()` not checked.                                                                               |         2 |
| 02  | `SafeMath` is generally not needed starting with `Solidity 0.8`, since the compiler now has built in overflow checking. |         1 |
| 03  | Don't use `catch`-block empty.                                                                                          |         8 |

## 01 - Return values of `approve()` not checked.

_Not all `IERC20` implementations `revert()` when there's a failure in `approve()`. The function signature has a `boolean` return value and they indicate errors that way instead. By not checking the return value, operations that should have marked as failed, may potentially go through without actually approving anything_

There are 2 instances for this issue:

-   [contracts/lybra/configuration/LybraConfigurator.sol#L302](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L302)

```ts
EUSD.approve(address(curvePool), balance);
```

-   [contracts/lybra/pools/LybraWstETHVault.sol#L35](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWstETHVault.sol#L35)

```ts
lido.approve(address(collateralAsset), msg.value);
```

Recommended Mitigation Steps:

-   It is recommend to use safeApprove().
-   Consider using safeIncreaseAllowance instead of approve function. (Approve race condition)

## 02 - `SafeMath` is generally not needed starting with `Solidity 0.8`, since the compiler now has built in overflow checking.

_All 3(ver cuantos?) in-scope contracts use the SafMath library for simple addition, subtraction, multiplication and division._

_This causes an unnecessary overhead since beginning from Solidity version 0.8.0 arithemtic operations revert by default on overflow / underflow._

_By looking into the implementation of the SafeMath library you can also see that it is just a wrapper around the basic arithmatic operations and does not add any checks (at least for the functions used in the in-scope contracts)._

[Docs-Changes of the Semantics](https://docs.soliditylang.org/en/v0.8.9/080-breaking-changes.html#silent-changes-of-the-semantics)

There an instance:

-   [contracts/lybra/token/EUSD.sol](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol)

Recommended Mitigation Steps:

-   Remove `SafeMath` library.

## 03 - Don't use `catch`-block empty.

_Usually empty try-catch is a bad idea because you are silently swallowing an error condition and then continuing execution._

_Exceptions should only be thrown if there is truly an exception - something happening beyond the norm. An empty catch block basically says "something bad is happening, but I just don't care"._

_If you don't want to handle the exception, let it propagate upwards until it reaches some code that can handle it._

There are 8 instances:

-   [contracts/lybra/miner/EUSDMiningIncentives.sol#L216](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L216)

```ts
try configurator.distributeRewards() {} catch {}
```

-   [contracts/lybra/pools/LybraStETHVault.sol#L73](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L73)

```ts
try configurator.distributeRewards() {} catch {}
```

-   [contracts/lybra/pools/LybraStETHVault.sol#L87](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L87)

```ts
try configurator.distributeRewards() {} catch {}
```

-   [contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L261](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L261)

```ts
try configurator.refreshMintReward(_provider) {} catch {}
```

-   [contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L280](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L280)

```ts
try configurator.refreshMintReward(_onBehalfOf) {} catch {}
```

-   [contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L177](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L177)

```ts
try configurator.refreshMintReward(_provider) {} catch {}
```

-   [contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L193](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L193)

```ts
try configurator.refreshMintReward(_onBehalfOf) {} catch {}
```

-   [contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L205](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L205)

```ts
try configurator.distributeRewards() {} catch {}
```
