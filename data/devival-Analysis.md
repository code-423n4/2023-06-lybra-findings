Personal comment:
Spent 43 hours auditing Lybra Finance. Spent a bit too much time navigating docs and finding the meaning behind each function. However, managed to cover almost 98% of code excluding some parts of EUSDMiningIncentives.sol and ProtocolRewardsPool.sol due to the lack of time.

For the Lybra team, I highly suggest providing more comments and more clear documention for future audits. 
Did not get deeper into OFT and cross-chain as not quite familiar with them at this moment.

Analysis:

The codebase extensively utilises existing Openzeppelin librariers, some functionality from Synthetix Staking Rewards



Architecture feedback
The overall auditing scope lacks BETH Pool contract according to the provided "Lybra V2 Workflow" graph.

Centralization risks
An owner is a single point of failure, as it was mentioned in a "bot race" report and a huge centralization risk.
Dependency on an external uncommon oracle Liquity poses an oracle risk. Dependency on ETH price may lead to the depeg of eUSD. Let us assume the ETH price drops by ~35% from when 1 eUSD was worth $1.5 worth of stETH. This would lead to under-collateralization and depeg of eUSD.


Systemic risks
Voting delay and voting period have to be changed, as currently both of them are set to last only 1-3 blocks.

Other recommendations
Missing configurator.initEUSD(this.EUSDMock.address) as initEUSD in configurator does not exist. As I understood, it's not same as initToken.

### Time spent:
43 hours