### [Gas-01] Multiply by 2 Should be performed using `bit shifting`
```solidity
require(assetAmount * 2 <= depositedAsset[onBehalfOf],
```
*Instances(2)*
```
File: LybraEUSDVaultBase.sol
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L159
```
```
File: LybraPeUSDVaultBase.sol
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L130
```

### [Gas-02] `_checkHealth()` could placed some step above, so that on function call fail on that step, will seve some gas of caller.

```solidity
    function _mintPeUSD(address _provider, address _onBehalfOf, uint256 _mintAmount, uint256 _assetPrice) internal virtual {
        require(poolTotalPeUSDCirculation + _mintAmount <= configurator.mintVaultMaxSupply(address(this)), "ESL");  
        _updateFee(_provider);

        try configurator.refreshMintReward(_provider) {} catch {}

        borrowed[_provider] += _mintAmount;
+       _checkHealth(_provider, _assetPrice);
        PeUSD.mint(_onBehalfOf, _mintAmount);
        poolTotalPeUSDCirculation += _mintAmount;
-       _checkHealth(_provider, _assetPrice); 
        emit Mint(_provider, _onBehalfOf, _mintAmount, block.timestamp);
    }
```
*Instances(1)*
```
File: LybraPeUSDVaultBase.sol
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L183
```

### [Gas-03] External Contract Calls Could Be Preformed With Low Level Calls, To Exclude Contract Exitance Check And Save Gas.

```solidity
rkPool.deposit{value: msg.value}(); 
```
*Instances(4)*
```
File: LybraRETHVault.sol
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraRETHVault.sol#L30
```
```
File: LybraStETHVault.sol
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L40
```
```
File: LybraWbETHVault.sol
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWbETHVault.sol#L23
```
```
File: LybraWstETHVault.sol
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWstETHVault.sol#L32
```

### [Gas-04] Use `assembly` To Write To State Variables, Which Will Save Gas
```solidity
    function setRkPool(address addr) external {
        require(configurator.hasRole(keccak256("TIMELOCK"), msg.sender));
        rkPool = IRkPool(addr); // @audit assembly could used to write to state variable
    }
```
*Instances(2)*
```
File: LybraRETHVault.sol
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraRETHVault.sol#L43
```
```
File: LybraStETHVault.sol
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L27
```

