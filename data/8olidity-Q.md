## `depositEtherToMint()` code redundancy
Since the `depositEtherToMint()` function stores eth, the value of `collateralAsset.balanceOf(address(this))` will not change, but the code judges the balance of collateralAsset before and after the deposit operation, which is redundant code

```solidity
function depositEtherToMint(uint256 mintAmount) external payable override {
    require(msg.value >= 1 ether, "DNL");
    uint256 preBalance = collateralAsset.balanceOf(address(this));
    rkPool.deposit{value: msg.value}();
    uint256 balance = collateralAsset.balanceOf(address(this));
    depositedAsset[msg.sender] += balance - preBalance;//@audit 

    if (mintAmount > 0) {
        _mintPeUSD(msg.sender, msg.sender, mintAmount, getAssetPrice());
    }

    emit DepositEther(msg.sender, address(collateralAsset), msg.value,balance - preBalance, block.timestamp);
}
```
### Recommended

```diff
function depositEtherToMint(uint256 mintAmount) external payable override {
    require(msg.value >= 1 ether, "DNL");
-    uint256 preBalance = collateralAsset.balanceOf(address(this));
    rkPool.deposit{value: msg.value}();
-   uint256 balance = collateralAsset.balanceOf(address(this));
-    depositedAsset[msg.sender] += balance - preBalance;//@audit 

    if (mintAmount > 0) {
        _mintPeUSD(msg.sender, msg.sender, mintAmount, getAssetPrice());
    }

    emit DepositEther(msg.sender, address(collateralAsset), msg.value,balance - preBalance, block.timestamp);
}
```

## The function does not exist in the WBETH interface
The LybraWBETHVault contract comment wrote that the WBETH address is 0xae78736Cd615f374D3085123A210448E74Fc6393 , but in the  https://etherscan.io/address/0xae78736cd615f374d3085123a210448e74fc6393#readContract , the contract does not have the `deposit` and `exchangeRatio` functions written in the interface.

```solidity
interface IWBETH {
    function exchangeRatio() external view returns (uint256);

    function deposit(address referral) external payable;
}

contract LybraWBETHVault is LybraPeUSDVaultBase {
    //WBETH = 0xae78736Cd615f374D3085123A210448E74Fc6393
```

Annotation error or interface error