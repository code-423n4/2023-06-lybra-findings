# Redundant initialization for rkPool

### 
rkPool is initialized at the contract level and then again in the constructor. This redundancy could potentially lead to confusion or errors. If the intention is to always use the _rkPool address passed to the constructor, the initial assignmet at the contract level is unnecessary and could be removed.


### 
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraRETHVault.sol#L17C1-L25

