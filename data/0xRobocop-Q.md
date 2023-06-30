
## L-01 Normal liquidations at the EUSD and PeUSD vaults do not take into account providers preferences over their funds.

The `liquidation` function in the EUSD and PeUSD vault contracts only validates that the provider has granted an allowance to the vault greater than zero, which may impact the liquidation provider's preferences since they cannot specify how much eUSD they are willing to spend to participate as liquidations providers. This behavior is also inconsistent with super liquidations, which validate that the provider has approved at least the amount of eUSD that will be repaid with their funds.