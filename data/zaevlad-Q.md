`exitCycle` variable is marked as `immutable` but has an initial value. It's better to set it up as `constant` if you do not plan to set it up in a constructor later. 

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L39

```
   ... 
    uint256 immutable exitCycle = 90 days;
   ...
```