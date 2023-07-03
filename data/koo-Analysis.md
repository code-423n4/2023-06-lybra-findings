I think the EUSD and PEUSD are too centralization.
```solidity
    function transferFrom(address from, address to, uint256 amount) public returns (bool) {
        address spender = _msgSender();
        if (!configurator.mintVault(spender)) {
            _spendAllowance(from, spender, amount);
        }
        _transfer(from, to, amount);
        return true;
    }
```
Admin has the power to transfer the EUSD and PEUSD owned by anybody, which will violation of decentralization blockchain. And also, once a private key of the address which have mintVault permision and isRedemptionProvider permission leak and is controled bt a attacker. Attacker can stole everbody's EUSD and PEUSD. Further more, see the following code:
```solidity
    function rigidRedemption(address provider, uint256 peusdAmount) external virtual {
        require(configurator.isRedemptionProvider(provider), "provider is not a RedemptionProvider");
        require(borrowed[provider] >= peusdAmount, "peusdAmount cannot surpass providers debt");
        uint256 assetPrice = getAssetPrice();
        uint256 providerCollateralRatio = (depositedAsset[provider] * assetPrice * 100) / borrowed[provider];
        require(providerCollateralRatio >= 100 * 1e18, "provider's collateral ratio should more than 100%");
        _repay(msg.sender, provider, peusdAmount);
        uint256 collateralAmount = (((peusdAmount * 1e18) / assetPrice) * (10000 - configurator.redemptionFee())) / 10000;
        depositedAsset[provider] -= collateralAmount;
        collateralAsset.transfer(msg.sender, collateralAmount);
        emit RigidRedemption(msg.sender, provider, peusdAmount, collateralAmount, block.timestamp);
    }
```
Using stolen money, attacker then can use the rigidRedemption call to get all collateralAsset in the pool. It will cause serious damage to the user's money.

### Time spent:
20 hours