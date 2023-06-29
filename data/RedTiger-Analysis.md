## Analysis of the codebase (What's unique? What's using existing patterns?)
What I found unique is that the previously deployed code contains no major bugs, although it was audited only once and the audit did not find much. However, the newly deployed contains many.

## Architecture feedback
The protocol should implement a Dutch auction mechanism for grabEsLBR and purchaseOtherEarnings. Currently, too much money is left on the table with the current fixed discount.

It is also strange that there is no Superliquidation for non-rebasing tokens. This slows down the liquidation mechanism by only allowing half of someone's collateral to be liquidated. The same mechanism as rebasing vaults should be used.

## Centralization risks
The setLidoRebaseTime function can only be called by the ADMIN. A malicious admin could constantly change the time to avoid any discount on the price of stETH in getDutchAuctionDiscountPrice. By doing this, they would prevent the yield from being distributed to users.

## Systemic risks
The protocol does not use the Oracle Feeds of the different LSTs. It always assumes that 1 LST is equal to the quantity of ETH inside. This is a wrong assumption. The LST could be hacked or could pause withdrawals temporarily, which could impact its price (depeg).

In the case of a depeg of stETH of more than 50%, stETH would not be sold to users since the maximum discount provided by getDutchAuctionDiscountPrice is 50%.

## Other recommendations
Provide a full suite of tests before the mitigation reviews.

## How much time did you spend?
25 hours

### Time spent:
24 hours