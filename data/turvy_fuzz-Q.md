## Incorrect required allowance check
It checks allowance > 0 rather than allowance >= eusdAmount as done correctly here:
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L197
```solidity
require(EUSD.allowance(provider, address(this)) >= eusdAmount, "provider should authorize to provide liquidation EUSD");
```
but incorrectly here:
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L160
```solidity
require(EUSD.allowance(provider, address(this)) > 0, "provider should authorize to provide liquidation EUSD"); // @audit-info 
```
## Dead code, if condition will always pass since amount is already calculated to be always >= totalFee
This block of code will never get executed
```solidity
 else {
            feeStored[_onBehalfOf] = totalFee - amount;
            PeUSD.transferFrom(_provider, address(configurator), amount);
        }
```
rather, only the if block will ever get executed because amount will never be less than totalFee due to the fact that it was already checked before assigning, here:
```solidity
uint256 amount = borrowed[_onBehalfOf] + totalFee >= _amount ? _amount : borrowed[_onBehalfOf] + totalFee;
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L196
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L201

## set at declaration should be constants not immutable variables
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L39
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L18
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L21
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L30

## Incorrect error message,  should be `overallCollateralRatio should below badCollateralRatio` and not strictly 150%
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L189
error message `overallCollateralRatio should below 150%` should be `overallCollateralRatio should below badCollateralRatio` and not strictly 150%

## just return userLockStatus[user].unlockTime. Unnecessary stack variable declaration
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol#L49

## These state variables should be set to constant
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol#L15
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/esLBR.sol#L20