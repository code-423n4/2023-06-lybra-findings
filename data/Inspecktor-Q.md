1. There is no check for address(0) in the LybraConfigurator.sol contract constructor. Need to check. In the future, there is no possibility to change the address in the contract.
Link: https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L81-L82
Corrections: add
if (_dao != 0 && _curvePool != 0) {...}
Or add the ability to change in the future.

2. LybraConfigurator.setProtocolRewardsPool(), LybraConfigurator.setEUSDMiningIncentives() need to check the input addr address against address(0).

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L137-L140
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L147-L150

3. In LybraConfigurator.setTokenMiner(), before setting, you need to check for equality of the lengths of the input arrays

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L235-L240

if(_contracts.length == _bools.length) {...} else {revert(...)}

4. Setting values in ProtocolRewardsPool.setTokenAddress() is best done separately, not together

If the administrator only needs to change one address out of three, then there is no need to enter three addresses. There is a high probability of error.

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L52-L56

5. To save gas, the variable uint8 immutable vaultType = 0 in LybraEUSDVaultBase.sol must be changed to uint256

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L30

6. In LybraEUSDVaultBase.sol, you need to check that the EUSD address has already been initialized.

Then there is no way to change the EUSD address.

if(configurator.getEUSDAddress() != 0) {
     EUSD = IEUSD(configurator.getEUSDAddress());
}

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L49

7. It is necessary to provide for the possibility of changing the etherOracle address in LybraEUSDVaultBase.sol.

If something happens to the oracle, and there is no possibility for the administrator to change the address, then the contract will not work in the future.

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L50

8. Wrong comment

* - `assetAmount` Must be higher than 0.
Must be
* - `assetAmount` Must be higher or equal than 1.

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L69

9. To improve accuracy, it is better to use one division, not two

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L301