## [LOW-01] Upon executing the liquidation process, LybraPeUSDVaultBase::liquidation() should guarantee that the user is no longer exposed to the risk of further liquidation.

This protocol's main purpose of liquidation is to raise the borrower's collateral ratio above the bad collateral ratio and reach at least the safe collateral ratio. However, currently, there is no check after liquidation to confirm if the borrower's account has achieved this goal.

Recommendation:
To ensure the intended liquidation objective is fully met, it is recommended to implement a verification step using the _checkHealth() function from the base vault contract. This will provide a straightforward measure to confirm that the borrower's collateral ratio is indeed above the bad collateral ratio. By incorporating this simple yet important verification process, the liquidation mechanism can be strengthened, reducing any potential vulnerabilities and ensuring the desired outcome.

## [LOW-02] initializer modifier missing in LybraConfigurator::initToken()

    function initToken(address _eusd, address _peusd) external onlyRole(DAO) {
        if (address(EUSD) == address(0)) EUSD = IEUSD(_eusd);
        if (address(peUSD) == address(0)) peUSD = IEUSD(_peusd);
    }


The comments accompanying the `initToken()` function indicate that it should only be called once. However, there is no modifier in place to enforce this restriction. Although the impact of this omission is relatively minor, as only the DAO can call the function to reset the token address, it is important to ensure clarity and consistency in the codebase.

Recommendation: To align with the intention expressed in the comments, consider either removing the comment if the function is meant to be called multiple times or adding a modifier to restrict the number of calls to only one. By implementing this change, the code will accurately reflect the expected behavior and avoid any potential confusion for future developers working with the contract.

## [LOW-03] Precision Error causes the reward due to the keeper to be 1% less than they deserve

In the event of a liquidation that is initiated by someone other than the provider, the individual who utilized the provider's account to trigger the liquidation process is eligible to receive a reward2keeper. This reward is capped at a maximum of 5%.

        function setKeeperRatio(address pool,uint256 newRatio) external checkRole(TIMELOCK) {
            require(newRatio <= 5, "Max Keeper reward is 5%");
            vaultKeeperRatio[pool] = newRatio;
            emit KeeperRatioChanged(pool, newRatio);
        }
    
The calculation for the reward2keeper in the code divides the keeper ratio by 110 instead of 100. As a result, the intended 5% reward2keeper is approximately 4.5%. However, due to a precision error caused by the decimal, the keeper may end up losing a maximum of 1% in rewards.

        reward2keeper = (reducedAsset * configurator.vaultKeeperRatio(address(this))) / 110;
        collateralAsset.transfer(provider, reducedAsset - reward2keeper);
        collateralAsset.transfer(msg.sender, reward2keeper);

## [LOW-04] safe collateral ratio should be changed simultaneously as the bad collateral ratio changes

Since the safe collateral ratio is a derivative of the bad collateral ratio, once the bad collateral ratio changes, the safe collateral ratio should also be set to at least 10% more than the safe collateral ratio. Failure to do this translates to the usage of a stale and incongruent safe collateral ratio when the bad collateral ratio changes.

Recommendation:

    function setBadCollateralRatio(address pool, uint256 newRatio, uint256 safeRatio) external onlyRole(DAO) {
        require(newRatio >= 130 * 1e18 && newRatio <= 150 * 1e18 && newRatio <= vaultSafeCollateralRatio[pool] + 1e19, "LNA");
        vaultBadCollateralRatio[pool] = newRatio;
        setSafeCollateralRatio(safeRatio); //change visibility from external to public
        emit SafeCollateralRatioChanged(pool, newRatio);
    }

## [LOW-05] LybraConfigurator::getSafeCollateralRatio() should have returned 10% less of a bad collateral ratio when it hasn't been set yet.

Just like in LybraConfigurator::getBadCollateralRatio(), instead of outrightly returning 160%, calculate the safe collateral ratio as a derivative of the bad collateral ratio.

Recommendation:

    function getSafeCollateralRatio(
        address pool
    ) external view returns (uint256) {
        if (vaultSafeCollateralRatio[pool] == 0) return vaultBadCollateralRatio[pool] + 1e19;
        return vaultSafeCollateralRatio[pool];
    }