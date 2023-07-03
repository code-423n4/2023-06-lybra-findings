The `stakingToken.transferFrom` and `stakingToken.transfer` calls in the `stake` and `withdraw` functions, and the `rewardsToken.mint` call in the `getReward` function are not checked for their return value. Although it's not mandatory in ERC20 standard to return a boolean for these functions, some tokens do.


- here is the code of contract : https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/stakerewardV2pool.sol#L1C1-L150C2

- to fix this,  added to the stake, withdraw, and getReward functions:

```solidity
if (stakingToken.transferFrom(msg.sender, address(this), _amount) == false) {
    revert("Transfer failed");
}

if (stakingToken.transfer(msg.sender, _amount) == false) {
    revert("Transfer failed");
}

if (rewardsToken.mint(msg.sender, _amount) == false) {
    revert("Mint failed");
}
```
