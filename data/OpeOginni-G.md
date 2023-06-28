## 1. IF’s/require() statements that check input arguments should be at the top of the function

***Checks that involve constants should come before checks that involve state variables, function calls, and calculations. By doing these checks first, the function is able to revert before wasting a Gas in a function that may ultimately revert in the unhappy case.***

``` 
File: contracts/lybra/pools/base/LybraEUSDVaultBase.sol

192:        require(assetAmount <= depositedAsset[onBehalfOf], "total of collateral can be liquidated at most");
```

This require statement only checks the values from the parameters of the Function `superLiquidation(address provider, address onBehalfOf, uint256 assetAmount)`, but is put below some other function calls that might waste gas if this Require statement ends up failing the whole function. Therefore it the require statement at line 192 should be put at the top of the function BEFORE any internal function calls.