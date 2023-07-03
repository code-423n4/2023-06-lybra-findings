#### Q1
The comment states that:
`amount Must be higher than 0. Individual mint amount shouldn't surpass 10% when the circulation reaches 10_000_000`
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L124C10-L124C126
But it is not implemented same as what the comment suggests.
This amount is limited to `mintVaultMaxSupply` set in the `LybraConfigurator`:
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L260
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/configuration/LybraConfigurator.sol#L119
##### Recommendation
Whether remove this comment or implement such functionality in `mint(...)`:
```
     if (
            (borrowed[msg.sender] * 100) / poolTotalEUSDCirculation > 10 &&
            poolTotalEUSDCirculation  > 10_000_000 * 1e18
        ) revert("Mint Amount cannot be more than 10% of total circulation");
```

#### Q2
Wrong error message in require-statement. `EUSD` should be changed to `PeUSD`.
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L131




