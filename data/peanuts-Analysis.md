## Table of Contents

1. Summary of the Protocol
2. Audit Approach
3. Codebase Analysis
4. Architecture Comments
5. Disclaimer

### 1. Summary of the Protocol

The protocol takes Liquid Staking Derivatives one step further and makes it collateralizable. This means that users can use their LSD (stETH, rETH) as collateral to borrow stablecoins. Before that, people were thinking of how to make more out of their LSD, and the most common way was to use it to provide liquidity, as seen in pools like stETH-ETH and rETH/WETH. Now, holders of LSD can choose to deposit their LSD inside Lybra and borrow stablecoins, which is an innovative way to make use of idle LSDs. 

Lybra divides two types of LSD: rebasing and non-rebasing. 

- Rebasing tokens means the token amount will increase with time. For example, 1 rebasing token will become 1.1 rebasing token in 1 year, and this means that the rebasing APR is 10%. An example of Rebasing liquid staking derivatives is staked Ether (stETH) from Lido. Lido rebases every 12 hours, meaning that the amount of stETH held will go up. 

- Non-rebasing tokens means that the token value, and not amount, will increase with time. For example, let's say 1 non-rebasing token is currently worth 1:1 with ETH. In one year, the ratio will become 1.1:1, which means that the value of non-rebasing token has increased. An example of non-rebasing token is rETH from Rocket Pool.

In this protocol, non-rebasing tokens will be able to mint PeUSD stablecoin and rebasing tokens will be able to mint EUSD stablecoins. The cool thing about EUSD stablecoin is that this stablecoin will earn interest just by holding it, because of the rebasing mechanism of its collateral. For example, a user deposits his stETH as collateral to mint 1000 EUSD. Remember that stETH is a rebasing token, so the amount of stETH increases over time. The protocol takes the rebasing amount, converts it into EUSD and pays it back to the user as interest. When the user withdraws his stETH, the amount will stay the same, but his value of EUSD would have increased.

The collaterization ratio of the protocol is set at 160%, meaning that for every $1000 of LSD deposited, a user can mint 625 stablecoins. The bad collaterization ratio is set between 130-150%, meaning that if the ratio reaches  
bad collaterization rate, then the user is liquitable.

There is no need to worry about how to get LSD tokens because the protocol helps users convert their native ETH into respective LSD tokens through a simple function. This means that users can simply choose which type of LSD he wants and choose the vault to deposit in and the protocol will take care of the conversion.

There is also the governance token, LBR, which can be staked for esLBR and depending on the time staked, the user can earn profits.

### 2. Approach taken in evaluating the codebase

- The first step was to read through all the medium articles to get a better sense of the protocol's intention.
- After that, I examined all the vaults to see whether Lybra has integrated and interacted with different types of LSD protocols properly. 
- Next, I looked for logical errors and potential attack vectors in the Vault Bases and check each collateral calculation thoroughly.
- Afterwards, I browsed through the configurator contracts to see the constrains set in place and check for lapses. 
- When I get a good sense of the whole protocol, I created hypothetical end-to-end scenarios, from depositing to minting to redeeming and finally withdrawing. I checked for any accounting errors, attack paths and potential denial of service.

### 3. Codebase Analysis

The codebase, especially the vaults, is pretty understandable after a good examination of the contracts. The functions written are comprehensible, and the NATSPEC is helpful. It would be better if there is a short explanation for every function, but generally the meaning of the function is easily devisable from the function name. 

There are some function that requires a general understanding of the whole codebase because of external calls to different contract, such as `refreshMintReward`. These external calls could be explained briefly in the comments. 

Some functions are quite unique, like `excessIncomeDistribution` together with `getDutchAuctionDiscountPrice`. It would be great if there is visual documentation with hypothetical examples of what is actually happening.  

### 4. Architecture Comments

There is a lot of reliance on external projects but its understandable because of the nature of the Lybra protocol. It was great to see that users can directly deposit their staked derivative instead of only having one route into the protocol. 

I'm not sure if relying on Liquity oracle is the best and most cost-efficient way to get the ether price. Perhaps building an oracle contract is better because there will be more control. 

Seems like only 4 types of staked derivatives is included in the protocol. Probably will have more soon, so it's important to make sure that every single integration is properly maintained before shipping out to production.

The borrow/repay process is straightforward which is great. The liquidation process is also easy to understand. There is something new in the liquidation process, which is that keepers are only allowed to liquidate maximum half of the liquitable amount, and 10% will be the reward for liquidation. This is good incentive for liquidators to liquidate bad positions, and also for users to make sure that their position is well maintained. Also, the disparity between badCollaterizationRatio and CollaterizationRatio is well thought out, making sure that a user that mints their maximum amount of collaterization will not be immediately liquidated.

Interacting with other protocols will definitely incur systemic risks. Lybra should be well prepared of the potential failures / exploits from different protocol. For example, there could be a pause/unpause mechanism for all the different vaults in case something bad happens. If rocket pool is compromised and the value of rETH drops significantly, Lybra could immediately pause all rETH deposits and withdraw all rETH funds inside. However, building fail-safe mechanisms will definitely become an issue of centralization. Lybra should strongly consider weighing the advantage of decentralization over security before implementing any control.

Overall, I believe that this protocol has good foundation and will definitely succeed considering the first-mover advantage in the space.

### 5. Disclaimer

Total hours spent: 22 hours. Most of the time spent is focused only on Liquid Staking Derivatives and Vaults. Briefly surveyed the protocol reward contracts.

### Time spent:
22 hours