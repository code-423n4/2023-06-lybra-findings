### Use calldata instead of memory for arrays

**Summary**
You can indeed use `calldata` instead of `memory` for the `_pools` array argument, because the function is marked as `external`.
Using `calldata` instead of `memory` can reduce the gas cost of this function, particularly for larger arrays.

**There is 1 instance of this issue:**
```solidity
File: contracts/lybra/miner/EUSDMiningIncentives.sol

    function setPools(address[] memory _pools) external onlyOwner {
        for (uint i = 0; i < _pools.length; i++) {
            require(configurator.mintVault(_pools[i]), "NOT_VAULT");
        }
        pools = _pools;
    }

```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L93-L98

**Potential Gas Savings**
When array is passed as an argument of a function, the EVM charges gas for each element of the array.
Here's the approximate cost:
- 200 gas per word for memory
- 3 gas per word for calldata