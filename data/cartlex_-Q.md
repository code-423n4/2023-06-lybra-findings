Lost of gas when calling `InitToken` function in `LybraConfigurator.sol` contract.

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/configuration/LybraConfigurator.sol#L95-L101

```
    /**
     * @notice Initializes the eUSD and peUSD address. This function can only be executed once.
     */
    function initToken(address _eusd, address _peusd) external onlyRole(DAO) {
        if (address(EUSD) == address(0)) EUSD = IEUSD(_eusd);
        if (address(peUSD) == address(0)) peUSD = IEUSD(_peusd);
    }
```

In commentary block of this function says that it can be called once, but it possible to call it again due to `else` statment doesn't check.

To restrict calling this function again it is needed to use `!=` statement and custom error.
```
    /**
     * @notice Initializes the eUSD and peUSD address. This function can only be executed once.
     */
    function initToken(address _eusd, address _peusd) external onlyRole(DAO) {
        if (address(EUSD) != address(0)) {
            revert AddressAlreadyInit();
        }
        if (address(peUSD) != address(0)) {
            revert AddressAlreadyInit();
        }
        peUSD = IEUSD(_peusd);
        EUSD = IEUSD(_eusd);
    }
```