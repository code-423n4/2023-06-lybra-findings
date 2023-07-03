|      | Issues                                  | Instances |
| :--- | :-------------------------------------- | --------: |
| G-01 | Remove `SafeMath` library.              |         1 |
| G-02 | Use assembly to check for `address(0)`. |         6 |

## G-01 - Remove `SafeMath` library.

_Gas saved: 2.1k for each tx that uses `SafeMath` + 100 units per call._

_It seems like safe math doesn’t serve any purpose here and it can simply be replaced with normal math operands. The usage of library is pretty expensive since it makes a delegate call to an external address._

There an instance:

[contracts/lybra/token/EUSD.sol#5](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L5)

Recommended Mitigation Steps:

-   Remove `SafeMath` library.

## G-02 - Use assembly to check for `address(0)`.

_Saves 6 gas per instance._

Example:

```ts
function ownerNotZero(address _addr) public pure {
    require(_addr != address(0), "zero address)");
}

// Gas: 258
```

```ts
function assemblyOwnerNotZero(address _addr) public pure {
    assembly {
        if iszero(_addr) {
            mstore(0x00, "zero address")
            revert(0x00, 0x20)
        }
    }
}

// Gas: 252
```

There are 14 instances and [34 future instances](https://gist.github.com/liveactionllama/27513952718ec3cbcf9de0fda7fef49c#l01-missing-checks-for-address0x0-when-assigning-values-to-address-state-variables):

-   [contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L83](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L83)

```ts
require(onBehalfOf != address(0), 'TZA');
```

-   [contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L97](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L97)

```ts
require(onBehalfOf != address(0), 'TZA');
```

-   [contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L111](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L111)

```ts
require(onBehalfOf != address(0), 'TZA');
```

-   [contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L99](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L99)

```ts
require(onBehalfOf != address(0), 'TZA');
```

-   [contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L127](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L127)

```ts
require(onBehalfOf != address(0), 'MINT_TO_THE_ZERO_ADDRESS');
```

-   [contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L141](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L141)

```ts
require(onBehalfOf != address(0), 'BURN_TO_THE_ZERO_ADDRESS');
```

-   [contracts/lybra/token/EUSD.sol#L367-L368](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L367-368)

```ts
require(_owner != address(0), 'APPROVE_FROM_ZERO_ADDRESS');
require(_spender != address(0), 'APPROVE_TO_ZERO_ADDRESS');
```

-   [contracts/lybra/token/EUSD.sol#L392-L393](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L392-L393)

```ts
require(_sender != address(0), 'TRANSFER_FROM_THE_ZERO_ADDRESS');
require(_recipient != address(0), 'TRANSFER_TO_THE_ZERO_ADDRESS');
```

-   [contracts/lybra/token/EUSD.sol#L412](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L412)

```ts
require(_recipient != address(0), 'MINT_TO_THE_ZERO_ADDRESS');
```

-   [contracts/lybra/token/EUSD.sol#L441](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L441)

```ts
require(_account != address(0), 'BURN_FROM_THE_ZERO_ADDRESS');
```

-   [contracts/lybra/token/EUSD.sol#L460](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L460)

```ts
require(_account != address(0), 'BURN_FROM_THE_ZERO_ADDRESS');
```

-   [contracts/lybra/token/PeUSDMainnetStableVision.sol#L64](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L64)

```ts
require(to != address(0), 'TZA');
```

Recommended Mitigation Steps:

-   Use assembly for this check.
