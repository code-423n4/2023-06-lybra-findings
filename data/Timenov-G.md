# Lybra Finance report by Timenov

## Summary
G-01 Use `constant` for variables that will not be changed in the future.

### [G-01] Use `constant` for variables that will not be changed in the future.
*There are 2 instances of this issue.*

```solidity
File: contracts/lybra/token/esLBR.sol

20: uint256 maxSupply = 100_000_000 * 1e18;
```

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/esLBR.sol#L20C5-L20C44

```solidity
File: contracts/lybra/token/LBR.sol

15: uint256 maxSupply = 100_000_000 * 1e18;
```

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/LBR.sol#L15