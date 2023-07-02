#### Q1
The interface used in `LybraRETHVault.sol` for getting the exchange rate does not match the target contract RETH.
In line 10, the interface is `getExchangeRatio`:
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/LybraRETHVault.sol#L10

while the contract `RocketTokenRETH.sol` implements `getExchangeRate`.
https://github.com/rocket-pool/rocketpool/blob/6a9dbfd85772900bb192aabeb0c9b8d9f6e019d1/contracts/contract/token/RocketTokenRETH.sol#L66
which is deployed in the following address:
https://etherscan.io/address/0xae78736Cd615f374D3085123A210448E74Fc6393#readContract#F6

This can cause the protocol not to be able to mint PeUSD when depositing ETH into Rocket Pool, because it will revert. The flow is:
LybraRETHVault::depositEtherToMint >> LybraRETHVault::getAssetPrice >> IRETH(address(collateralAsset)).getExchangeRatio() >> Revert!!!
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/LybraRETHVault.sol#L27
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/LybraRETHVault.sol#L35
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/LybraRETHVault.sol#L47C33-L47C83

The same issue is also available in `LybraWbETHVault.sol`.
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/LybraWbETHVault.sol#L10
https://etherscan.io/token/0xa2E3356610840701BDf5611a53974510Ae27E2e1#readProxyContract#F13


#### Q2
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




