# LOW FINDINGS

##

## [L-] Divide by zero should be avoided 

### Impact
Division by zero should be avoided in Solidity and in any programming language. Division by zero is an undefined operation, and it can lead to unexpected and unpredictable results. In Solidity, division by zero will cause the contract to revert. This means that the transaction will be canceled, and the caller will not receive any funds or tokens

### POC

```solidity
FILE: 2023-06-lybra/contracts/lybra/miner/esLBRBoost.sol

67: return ((boostEndTime - userUpdatedAt) * maxBoost) / (time - userUpdatedAt);

```
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/esLBRBoost.sol#L67C89-L67C89

### Recommended Mitigation
- Use the SafeMath library
- Check the divisor before dividing

##

## [L-] Lack of ``check-effects-interactions (CEI) pattern`` is vulnerable to ``reentrancy attacks``

### Impact
In the function ``depositEtherToMint``, the state change of minting ``PEUSD`` is performed after the external call to the IWBETH contract. This means that an attacker could call the IWBETH contract and then call the ``_mintPeUSD`` function before the state change of minting PEUSD has been completed. This could allow the attacker to mint more PEUSD than they are entitled to, which could create a security risk

### POC

```solidity
FILE: Breadcrumbs2023-06-lybra/contracts/lybra/pools/LybraWbETHVault.sol

23: IWBETH(address(collateralAsset)).deposit{value: msg.value}(address(configurator));
24:        uint256 balance = collateralAsset.balanceOf(address(this));
25:        depositedAsset[msg.sender] += balance - preBalance;
26:
27:        if (mintAmount > 0) {
28:            _mintPeUSD(msg.sender, msg.sender, mintAmount, getAssetPrice());
29:        }

```
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/LybraWbETHVault.sol#L23-L29

```solidity
FILE: 2023-06-lybra/contracts/lybra/pools/LybraRETHVault.sol

 rkPool.deposit{value: msg.value}();
        uint256 balance = collateralAsset.balanceOf(address(this));
        depositedAsset[msg.sender] += balance - preBalance;

        if (mintAmount > 0) {
            _mintPeUSD(msg.sender, msg.sender, mintAmount, getAssetPrice());
        }

```
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/LybraRETHVault.sol#L30-L36


https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L58-L70

### Recommended Mitigation

Use check-effects-interactions (CEI) pattern 

##

## [L-] Shadowed variable should be avoided 

### Impact

Name masking can be a source of errors, so it is important to be aware of the potential for name masking when writing Solidity contracts. By giving variables unique names and avoiding name masking, you can help to prevent errors in your contracts.

If a variable is declared with the same name as a function, the variable will shadow the function. This means that the variable will be used instead of the function when the name is referenced.

### POC

```solidity
FILE: 2023-06-lybra/contracts/lybra/token/PeUSD.sol

12: uint8 decimals = decimals();

```
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/PeUSD.sol#L12

```solidity
FILE: 2023-06-lybra/contracts/lybra/token/LBR.sol

20: uint8 decimals = decimals();

```
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/LBR.sol#L20

### Recommended Mitigation
Avoid using the same name for a variable and a function


