# Findings Summary

| ID     | Title                                                                              | Severity      |
| ------ | ---------------------------------------------------------------------------------- | ------------- |
| [L-01] | Missing zero address & pausable checks in `EUSD.sol`                               | Low      |
| [NC-01] | Use a safe pragma statement                                                       | Non-critical   |
| [NC-02] |  Import  OZ `SafeCast` library                                                    | Non-critical      |

# Findings

# [L-01] Missing zero address & pausable checks in `EUSD.sol` 

Natspec comment mentioned checks that are not actually done, as you can see from the example below, there are no checks implemented to validate that recipient is not zero address or that the contract is not paused

 ```solidity
     * Requirements:
     *
     * - `_recipient` cannot be the zero address.
     * - the caller must have at least `_sharesAmount` shares.
     * - the contract must not be paused.
     *
     * @dev The `_sharesAmount` argument is the amount of shares, not tokens.
     */
    function transferShares(address _recipient, uint256 _sharesAmount) public returns (uint256) {
        address owner = _msgSender();
        _transferShares(owner, _recipient, _sharesAmount);
        emit TransferShares(owner, _recipient, _sharesAmount);
        uint256 tokensAmount = getMintedEUSDByShares(_sharesAmount);
        emit Transfer(owner, _recipient, tokensAmount);
        return tokensAmount;
    }
```

# [NC-01] Use a safe pragma statement

Always use stable pragma statement to lock the compiler version and to have deterministic compilation to bytecode. Finally consider upgrading the version to a newer one to use bugfixes and optimizations in the compiler. This is recommended to be changed in all contracts in the scope.

# [NC-02] Import  OZ `SafeCast` library 

Function `clock` returns a `SafeCast.toUint48(block.number)` but the `SafeCast` library from OZ is not actually implemented in the contract, this may create delusion sense for security. Edit the function parameters or import the library.

```solidity
    function clock() public override view returns (uint48){
        return SafeCast.toUint48(block.number);
```


