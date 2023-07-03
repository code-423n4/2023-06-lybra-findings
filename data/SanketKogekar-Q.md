
1. There are multiple cases when transfer can fail silently as returned value is unchecked, one of the example would be in `LybraConfiguration.distributeRewards()`

```
peUSD.transfer(address(lybraProtocolRewardsPool), peUSDBalance);
```
It is recommended to prefer using `safeTransfer` / `safeApprove` functions from OZ's library.

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/configuration/LybraConfigurator.sol#L292-L293

2. The `LybraConfiguration.setMaxStableRatio()` requires a check:

```
require(_ratio > 0, "Ratio should be greater than 0");
```

In case it is set to 0 by accident, the function `LybraConfigurator.getEUSDMaxLocked` will break, which in case would break other functions that use it.

(Likelihood is low becuase it utilizes timelock)

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/configuration/LybraConfigurator.sol#L246-L249

3. The `LybraConfiguration.setTokenMiner` does not check if both arrays from args are of equal length.

Make sure `_contracts.length == _bools.length`

(Likelihood is low becuase it utilizes timelock)

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/configuration/LybraConfigurator.sol#L235-L240

4. The `LybraConfiguration.setKeeperRatio` has an incorrect condition:

```
require(newRatio <= 5, "Max Keeper reward is 5%");
```

based on the comment

```
* @param newRatio The new reward ratio to set, limited to a maximum of 5%.
```

I think the developer meant to code just as how percentage is calculated like other functions:
```
require(newRatio <= 500, "Max Keeper reward is 5%");
```

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/configuration/LybraConfigurator.sol#L224-L228

5. There are several instances in code where tokens are not approved before initiating `transferFrom` in functions.

6. As per the sponsors comments, the plan is to deploy the protocol on other chains as well, but the address for fetching ether price seems to be hardcoded to WETH contract from Ethereum Mainnet:

```
uint256 etherInLp = (IEUSD(0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2).balanceOf(ethlbrLpToken) * uint(etherPrice)) / 1e8;
```

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/EUSDMiningIncentives.sol#L153-L154

7. The comment on `LybraConfigurator.sol` regarding timelock period of 14 days is never implemented.

```
 * * DAO: A time-locked contract initiated by esLBR voting,
 * with a minimum effective period of 14 days. After the vote is passed,
 *  only the developer can execute the action.
```

8. The `ProtocolRewardsPool.setGrabCost()` requires a check:

```
require(_ratio > 0, "Ratio should be greater than 0");
```

In case it is set to 0 by accident, the function `ProtocolRewardsPool.grabEsLBR` will break, which in case would break other functions that use it.

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/ProtocolRewardsPool.sol#L131-L136