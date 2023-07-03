## Emitting event before it happens
```
function setFlashloanFee(uint256 fee) external checkRole(TIMELOCK) {
        if (fee > 10_000) revert('EL');
        emit FlashloanFeeUpdated(fee);
        flashloanFee = fee;
    }
```
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/configuration/LybraConfigurator.sol#L253-L257

## Not implemented function logic
```
     /**
     * @notice Update user's claimable reward data and record the timestamp.
     */
    function refreshReward(address _account) external updateReward(_account) {}
```
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/EUSDMiningIncentives.sol#L171-L174

## Isn't check if _contracts and _bools arrays are equal
It is assumed that the two arrays will be equal and this is not checked. But if the _contracts array has more values than _bools, the pools that have no corresponding values from _bools will be set as default to false.

```
    function setTokenMiner(address[] calldata _contracts, bool[] calldata _bools) external checkRole(TIMELOCK) {
        for (uint256 i = 0; i < _contracts.length; i++) {
            tokenMiner[_contracts[i]] = _bools[i];
            emit tokenMinerChanges(_contracts[i], _bools[i]);
        }
    }
```

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/configuration/LybraConfigurator.sol#L235-L240

## Missing require check
It is written in the comments that 'amount' must me higher than 0 but that isn't checked. 
Add require(amount > 0, "ZERO_MINT");
```
     * Requirements:
     * - `onBehalfOf` cannot be the zero address.
     * - `amount` Must be higher than 0.
     * @dev Calling the internal`_repay`function.
     */
    function burn(address onBehalfOf, uint256 amount) external {
        require(onBehalfOf != address(0), "BURN_TO_THE_ZERO_ADDRESS");
        _repay(msg.sender, onBehalfOf, amount);
    }
```
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L135-L143
