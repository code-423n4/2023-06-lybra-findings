Passing 0 as `finishAt` [here](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/esLBRBoost.sol#L57-L69) will incur in a division by 0 exception if `userUpdatedAt` is 0 too or an integer underflow in the other situation (due to being `block.timestamp > 0` always true and becoming `time` 0). Put two requires to ensure they are not 0.

Setting rebase time arbitrarily high or low without any checks, just faith in the ADMIN. Check [here](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/LybraStETHVault.sol#L25)

No check for `addr` not being `address(0)` [here](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/configuration/LybraConfigurator.sol#L138). Add a `require` at the start of the function (it happens again [here](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/configuration/LybraConfigurator.sol#L148), [here](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/configuration/LybraConfigurator.sol#L262) and [here](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/EUSDMiningIncentives.sol#L84-L91))

Porcentages should be `1e18` not `1e19` which leads to the money needing to be 10 times more to adhere to the normal porcentage. See [here](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/configuration/LybraConfigurator.sol#L202)



