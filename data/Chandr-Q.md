L[1]: The depositEtherToMint function does not follow a standard pattern for total balance calculation

code: https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWstETHVault.sol#L29-L46

```
    function depositEtherToMint(uint256 mintAmount) external payable override {
        require(msg.value >= 1 ether, "DNL");

        uint256 sharesAmount = lido.submit{value: msg.value}(address(configurator));
        require(sharesAmount > 0, "ZERO_DEPOSIT");

        lido.approve(address(collateralAsset), msg.value);

        uint256 wstETHAmount = IWstETH(address(collateralAsset)).wrap(msg.value);

        depositedAsset[msg.sender] += wstETHAmount;

        if (mintAmount > 0) {
            _mintPeUSD(msg.sender, msg.sender, mintAmount, getAssetPrice());
        }

        emit DepositEther(msg.sender, address(collateralAsset), msg.value,wstETHAmount, block.timestamp);
    }
```

mitigation:
Modify the depositEtherToMint function to calculate the total balance before and after the deposit