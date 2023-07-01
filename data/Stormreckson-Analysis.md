
Project Name: Lybra Finance 

Brief Overview: Lybra Finance is a groundbreaking decentralized protocol designed to bring stability to the
volatile world of cryptocurrency. Built on LSD (Liquid Staking Derivatives), the protocol initially
leverages Lido Finance-issued ETH proof-of-stake and stETH as its primary components, with plans to
support additional LSD assets in the future. The protocol's primary objective is to provide the
cryptocurrency industry with a safer, more decentralized stablecoin, eUSD, which offers stable interest
to its token holders.

Deposited ETH is automatically converted to stETH through the Lybra Protocol. (stETH is an ERC20 token representing ETH staked with Lido) One stETH can always be exchanged for one ETH, and due to the LSD (Liquidity Staking Derivatives) process, stETH will continue to grow over time Increased income from stETH is converted to eUSD based on the USD value of ETH at that time A certain percentage of the increased income is shared proportionally among LBR holders The remaining income is distributed to eUSD holders, with a base APY of approximately 8%.

Contracts in scope/review:
•contracts/lybra/Proxy/LybraProxyAdmin.sol
•contracts/lybra/Proxy/LybraProxy.sol
•contracts/lybra/governance/AdminTimelock.sol
•contracts/lybra/governance/GovernanceTimelock.sol
•contracts/lybra/pools/LybraWbETHVault.sol
•contracts/lybra/token/esLBR.sol
•contracts/lybra/pools/LybraWstETHVault.sol
•contracts/lybra/pools/LybraRETHVault.sol
•contracts/lybra/token/PeUSD.sol
•contracts/lybra/miner/esLBRBoost.sol
•contracts/lybra/token/LBR.sol
•contracts/lybra/pools/LybraStETHVault.sol
•contracts/lybra/governance/LybraGovernance.sol
•contracts/lybra/miner/stakerewardV2pool.sol
•contracts/lybra/governance/LybraGovernance.sol
•contracts/lybra/token/PeUSDMainnetStableVision.sol
•contracts/lybra/miner/ProtocolRewardsPool.sol
•contracts/lybra/token/EUSD.sol
•contracts/lybra/configuration/LybraConfigurator.sol
•contracts/lybra/miner/EUSDMiningIncentives.sol

Abstracts:
•contracts/lybra/pools/base/LybraEUSDVaultBase.sol
•contracts/lybra/pools/base/LybraPeUSDVaultBase.sol

Total number of contracts -21
Total number of lines of code -1762

Methodology used during the audit:
I strictly used the manual code review method,as a new warden I believe getting familiar with the smart contract syntax by auditing manually is good for the development of the mind towards auditing, regardless here are some areas (and not limited to) Of focus:
•Configuration
•Data Processing Issues
•Numeric Errors
•Security Features
•Time and State
•Error Conditions,Return Values,Status Codes
•Resource Management
•Behavioral Issues
•Business Logic
•Initialization and Cleanup
•Arguments and Parameters
•Expression Issues
•Coding Practices

Findings, during the audit a total of 24 issues where found 

High: 1
Medium: 10
Low/QA: 13

Report details can be found in the report section under warden name `Stormreckson`, I made an effort to include some instances not found by the bot to ensure good code coverage.

While auditing the protocol I got to understand what LSD (liquid Derivative Staking) mean and in general Lybra Finance was an eye opener for me.

I hope my findings are actual findings and I wish the Lybra Finance team good luck.




### Time spent:
150 hours