## Title QA
EUSD shares can be transferred to EUSD contract itself

## Links to affected code:
Provide GitHub links, including line numbers, to all instances of this bug throughout the repo.

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/EUSD.sol#L334-L341
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/EUSD.sol#L391-L400

## Proof of Concept
Provide direct links to all referenced code in GitHub. Add screenshots, logs, or any other relevant proof that illustrates the concept.

There is a lack of any check in `transferShares` and `_transferShares` for the recipient to not be `address(this)` meaning the `EUSD` contract itself.

`transferShares` and `_transferShares` functions in `EUSD.sol`
```
    function transferShares(address _recipient, uint256 _sharesAmount) public returns (uint256) {
        address owner = _msgSender();
        _transferShares(owner, _recipient, _sharesAmount);
        emit TransferShares(owner, _recipient, _sharesAmount);
        uint256 tokensAmount = getMintedEUSDByShares(_sharesAmount);
        emit Transfer(owner, _recipient, tokensAmount);
        return tokensAmount;
    }
    //...
    function _transferShares(address _sender, address _recipient, uint256 _sharesAmount) internal {
        require(_sender != address(0), "TRANSFER_FROM_THE_ZERO_ADDRESS");
        require(_recipient != address(0), "TRANSFER_TO_THE_ZERO_ADDRESS");

        uint256 currentSenderShares = shares[_sender];
        require(_sharesAmount <= currentSenderShares, "TRANSFER_AMOUNT_EXCEEDS_BALANCE");

        shares[_sender] = currentSenderShares.sub(_sharesAmount);
        shares[_recipient] = shares[_recipient].add(_sharesAmount);
    }
```
[EUSD.sol#L334-L341](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/EUSD.sol#L334-L341)
[EUSD.sol#L391-L400](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/EUSD.sol#L391-L400)

## Tools Used
Manual Review

## Recommended Mitigation Steps
Add `require(_recipient != address(this), "TRANSFER_TO_EUSD_CONTRACT");` to the function `_transferShares` in `EUSD.sol`
