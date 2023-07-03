## Analysis Report for Lybra
### Overview 
- Lybra Finance is a groundbreaking DeFi protocol focused on bringing stability to the volatile cryptocurrency market through its innovative stablecoin, eUSD. Built on LSD/LST's, the protocol initially utilizes Lido Finance-issued stETH as its primary components and plans to support additional LST's in the upcoming V2. eUSD is an omnichain LSD/LST-based stablecoin solution. Lybra has been capitalizing on the fresh avenues ushered in by LSD/LST's to offer the world's first interest-bearing stablecoin. In doing so, it is creating exactly the kind of profit-generating utility that LSD/LST's need to start fulfilling their vast potential.
### Understanding the Ecosystem: 
- With the rollout of V2, Lybra will be introducing peUSD into its ecosystem. Consider peUSD as the DeFi-optimized version of eUSD. It's designed to be bridged to any supported L2's, without any constraints on liquidity. The Lybra Protocol introduces a novel design for eUSD interest rebases. When the balance of stETH increases through LSD or other reasons, the excess income is sold for eUSD. This additional stETH is exchanged for eUSD based on the current price, and the eUSD shares of the previous holder are destroyed. As a result, the balances of other eUSD holders increase due to the decrease in total shares. This design ensures that the interest rebases are conducted in a fair and efficient manner, allowing for the distribution of additional income to all eUSD holders.
### Codebase Quality Analysis:
- The Lybra Finance codebase is extensive and well-structured, with a total of 21 contracts in scope and 1093 SLoC. The codebase primarily uses inheritance and includes 3 external imports. The contracts conform to the ERC20 standard and make use of a timelock function. The protocol is multi-chain but does not use a side-chain or rollups. It does not have an AMM and is not a fork of a popular project. It does use an oracle, specifically Chainlink.
### Architecture Recommendations: 
- The architecture of Lybra Finance is complex and involves several interacting components. It is recommended to ensure that all components are thoroughly tested and audited to prevent unexpected behavior or vulnerabilities.
### Centralization Risks: 
- The Lybra Finance protocol does not appear to have significant centralization risks. It uses a decentralized governance model and the protocol is multi-chain, which helps to distribute control and reduce centralization.
### Mechanism Review: 
- The mechanism of Lybra Finance involves the use of LSD/LST's to offer an interest-bearing stablecoin, eUSD. The protocol also introduces a novel design for eUSD interest rebases, which ensures a fair and efficient distribution of additional income to all eUSD holders.
### Systemic Risks:
- The systemic risks associated with Lybra Finance are primarily related to the complexity of the system and the potential for unexpected behavior or vulnerabilities. The use of an oracle also introduces potential risks related to data accuracy and reliability.
### Areas of Concern
- There are no tests or gas reports available for the Lybra Finance protocol, which is a significant area of concern. It is crucial to have comprehensive tests and gas reports to ensure the security and efficiency of the protocol especially during the development process. 
### Codebase Analysis
- The Lybra Finance codebase is extensive and well-structured, but the lack of tests and gas reports is a significant concern. The codebase primarily uses inheritance and includes 3 external imports. The contracts conform to the ERC20 standard and make use of a timelock function.

### Recommendations
It is recommended to develop comprehensive tests and gas reports for the Lybra Finance protocol, specifically during the development process, pre external audit. This will help to ensure the security and efficiency of the protocol.
### Contract Details
The Lybra Finance protocol includes a total of 21 contracts in scope. Some of the key contracts include:
- LybraProxyAdmin.sol: This contract is the admin of the Lybra proxy contracts.
- LybraProxy.sol: This contract inherits TransparentUpgradeableProxy, used to upgrade LybraConfigurator.
- AdminTimelock.sol: Timelock for Lybra Admin.
- GovernanceTimelock.sol: Timelock for Lybra DAO.
- LybraWbETHVault.sol: This contract inherits from the LybraPeUSDVaultBase contract and supports collateralizing WbETH to borrow PeUSD. When users deposit ETH, the contract will deposit the ETH into the WbETH contract and convert it to WbETH.
- esLBR.sol: The eUSD contract is an interest-bearing ERC20-like token designed for the Lybra protocol. It represents the holder's share in the total amount of Ether controlled by the protocol. The contract stores the sum of all shares to calculate each account's token balance, which is based on the account's shares and the total supply of eUSD.
- LybraWstETHVault.sol: This contract inherits from the LybraPeUSDVaultBase contract and supports collateralizing WstETH to borrow PeUSD. When users deposit ETH, the contract will deposit the ETH into the Lido contract and convert it to WstETH.
- LybraRETHVault.sol: This contract inherits from the LybraPeUSDVaultBase contract and supports collateralizing Rocket Pool ETH(rETH) to borrow PeUSD. When users deposit ETH, the contract will deposit the ETH into the RocketDepositPool contract and convert it to rETH.
- PeUSD.sol: PeUSD is a stable, interest-free ERC20-like token minted through eUSD in the Lybra protocol. It is pegged to 1eUSD and does not undergo rebasing. The token operates by allowing users to deposit eUSD and mint an equivalent amount of PeUSD. When users redeem PeUSD, they can retrieve the corresponding proportion of eUSD. As a result, users can utilize PeUSD without sacrificing the yield on their eUSD holdings.
### Conclusion
- Lybra Finance is a complex and innovative DeFi protocol that introduces a novel design for eUSD interest rebases and offers the world's first interest-bearing stablecoin. The protocol is well-structured and uses a decentralized governance model. However, the lack of tests and gas reports is a significant concern and it is recommended to develop these to ensure the security and efficiency of the protocol.

### Time spent:
20 hours