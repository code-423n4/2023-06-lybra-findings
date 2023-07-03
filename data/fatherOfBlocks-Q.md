**contracts/lybra/pools/LybraWstETHVault.sol**
- L5/7 - imports of contracts that are not used are made, apart from generating an extra amount of gas in the deploy, it also generates little readability in the code when viewing the code in the blockchain explorer.

- L35 - It is not checked that the approve returns true, this is important since otherwise it would revert when executing a transferFrom().

- There is no mechanism to withdraw ether, this is something that could be a problem, since if you want to withdraw ether in the future it will not be possible.


**contracts/lybra/pools/LybraRETHVault.sol**
- L5/7 - imports of contracts that are not used are made, apart from generating an extra amount of gas in the deploy, it also generates little readability in the code when viewing the code in the blockchain explorer.


**contracts/lybra/pools/LybraStETHVault.sol**
- L5 - IEUSD is imported, but never used, it should be removed.


**contracts/lybra/miner/stakerewardV2pool.sol**
- L134/137 - You should check that duration is != 0 before making divisions on this value, since the setRewardsDuration() function allows you to set any value.


**contracts/lybra/configuration/LybraConfigurator.sol 📤   **
- L236/237 - Two arrays are traversed, but the loop that is used only traverses one array, assuming they have the same length, this should be validated so that it is not congruence.
