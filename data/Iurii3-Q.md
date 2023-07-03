1. During initiation of tokens in LybraConfiguration.sol wrong interface is used

[LybraConfigurator.solL100](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/configuration/LybraConfigurator.sol#L100C1-L100C65)

Should use IPeUSD instead of IEUSD. However, this mistake won't escalate into bigger error as IEUSD interface has all necessary functions

Correct line of code: ``` if (address(peUSD) == address(0)) peUSD = IPeUSD(_peusd); ```

2. setTokenMiner function may revert due to array length discrepancies 

[LybraConfigurator.solL235-240](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/configuration/LybraConfigurator.sol#L235C1-L240C6)

setTokenMiner function has two arguments - array of _contracts and array of _bools. In case their length is not equal function will revert while using extensive amount of gas in a for loop.
Adding ```require(_contracts.length == _bools.length, "Length of contracts not equal to bools"); ``` will save gas in this situation

3. Check in setLockStatus might be done wrongly 

[esLBRBoost.solL41](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/esLBRBoost.sol#L42)

Considering Lybra Fincance allow users to extend the time of lock-up, this require statement is not correctly implemented.

For example
* user's current LockStatus is (unlockTime = +7 days from now; duration = 90 days, miningBoost = 30 * 1e18) 
* user want to change in to (unlockTime = +30 days from now; duration = 30 days, miningBoost = 20 * 1e18)
* his lock-up time will increase from 7 to 30 days, but it is currently not allowed as overall duration will be lowered.

Team should consider rewriting requirement to
``` require(userStatus.unlockTime <= block.timestamp + _setting.duration, "Your lock-in period has not ended, and the term can only be extended, not reduced."); ```

4. Vaults would be almost impossible to use with assets with decimals < 18.

[LybraEUSDVaultBase.solL73](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L73-L73)
[LybraPeUSDVaultBase.solL59](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L59-L59)

Currently Lybra Vaults may be used with 18-decimal LSD assets - stETH, wstETH, rETH, wbETH. 

As it stated in Lybra docs "_Collateral is any asset that a borrower must provide to take out a loan, acting as security for the debt._" 
Should there appear LSD asset with lower decimals, or if team decides to add 6-decimal USDC/USDT as a possible collateral this requirement ``` require(assetAmount >= 1 ether, "") ``` would be passed only with much bigger amount then anticipated. For 6 decimal assets user would need to deposit 1_000_000_000_000 token to pass this requirement

Team might consider adding mapping with assets decimals (address => uint) or move this requirement from base abstract contract to higher level contract for particular asset

5. Check before transfer would save gas cost in case of revert
[LybraEUSDVaultBase.solL109](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L109-L109)

6. Wrong notation. Should use collateral asset instead of stETH
[LybraPeUSDVaultBase.solL80](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L80-L80)
[LybraPeUSDVaultBase.solL123](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L123-L123)
[LybraPeUSDVaultBase.solL149](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L149-L149)

7. Unnecessary assignment 
[LybraRETHVault.solL18](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/LybraRETHVault.sol#L18-L18)

rkPool value is added during the deployment in constructor, no need to add it directly in the contract.

8. Wrong address in notation
[LybraWbETHVault.solL16](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/LybraWbETHVault.sol#L16-L16)
0xae78736Cd615f374D3085123A210448E74Fc6393 is rETH address, correct wbETH address is 0xa2E3356610840701BDf5611a53974510Ae27E2e1

[esLBR.solL6](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/esLBR.sol#L6-L6)
[esLBR.solL20](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/esLBR.sol#L20-L20)

```uint256 maxSupply = 100_000_000 * 1e18;``` in line 20 while 55 mln in notation

[EUSD.solL149](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/EUSD.sol#L149)
[EUSD.solL196](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/EUSD.sol#L196)
[EUSD.solL222](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/EUSD.sol#L222)
[EUSD.solL246](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/EUSD.sol#L246)
[EUSD.solL266](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/EUSD.sol#L266)
[EUSD.solL330](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/EUSD.sol#L330)
[EUSD.solL364](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/EUSD.sol#L364)
[EUSD.solL389](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/EUSD.sol#L389)

functions has _"the contract must not be paused"_ in notation but actually do not implement any check for pause

9. Use SafeMath for consistency
[EUSD.sol#L425](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/EUSD.sol#L425)
[EUSD.sol#L444](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/EUSD.sol#L444)

EUSD.sol imports SafeMath library and uses it for most arithmetical operations. 
But in two occasions += and -= used. Consider changing to SafeMath .add/.sub for consistency 



