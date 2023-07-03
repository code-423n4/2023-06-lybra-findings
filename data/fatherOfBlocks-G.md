**contracts/lybra/pools/LybraWstETHVault.sol**
- L30/35/37/45 - msg.value is consulted multiple times, but gas could be saved by creating a variable in memory.


**contracts/lybra/pools/LybraRETHVault.sol**
- L13 - An interface is created, but the deposit() function is never used.


**contracts/lybra/token/PeUSD.sol**
- L14 - There are operations that were previously checked, therefore a lower cost of gas could be generated, making the operation unchecked.


**contracts/lybra/token/LBR.sol**
- L22 - imports of contracts that are not used are made, apart from generating an extra amount of gas in the deploy, it also generates little readability in the code when viewing the code in the blockchain explorer.


**contracts/lybra/pools/LybraStETHVault.sol**
- L68/88 - imports of contracts that are not used are made, apart from generating an extra amount of gas in the deploy, it also generates little readability in the code when viewing the code in the blockchain explorer.


**contracts/lybra/governance/LybraGovernance.sol**
- L82 - Instead of validating the operation receipt.hasVoted == false, !receipt.hasVoted should be validated, this would generate less gas costs.

- L109 - Commented code, which makes it difficult to understand the code.


**contracts/lybra/token/PeUSDMainnetStableVision.sol 💰**
- L60 - imports of contracts that are not used are made, apart from generating an extra amount of gas in the deploy, it also generates little readability in the code when viewing the code in the blockchain explorer.


**contracts/lybra/miner/ProtocolRewardsPool.sol 📤**
- L39 - The exitCycle variable is immutable, but it already has a default value and a value is not set in the constructor, therefore the variable in storage should be constant.
