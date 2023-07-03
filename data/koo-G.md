From the implementation of EUSD.transferFrom.
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/EUSD.sol#L226
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
We can see it will always return true or revert the transaction. Thus We don't need to check the return value of EUSD.transferFrom. 

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/EUSDMiningIncentives.sol#L215

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/LybraStETHVault.sol#L70

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/LybraStETHVault.sol#L85

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/PeUSDMainnetStableVision.sol#L82

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/PeUSDMainnetStableVision.sol#L133
