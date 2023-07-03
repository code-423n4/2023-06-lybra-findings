1)   /**
     * @notice Initializes the eUSD and peUSD address. This function can only be executed once.
     */
    function initToken(address _eusd, address _peusd) external onlyRole(DAO) {
        if (address(EUSD) == address(0)) EUSD = IEUSD(_eusd);
        if (address(peUSD) == address(0)) peUSD = IEUSD(_peusd);
    }

initToken in LybraConfigurator.sol should be part of the constructor since it is only called once 



2) There should be three modifiers in LybraConfigurator.sol instead of two since there are three different ADMINS for better readability.



