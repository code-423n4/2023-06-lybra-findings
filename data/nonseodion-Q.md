### Oracle should return price of stETH and not ETH
Oracles should be used to return the price of a token and not the price of another token similar. A bad example is using an oracle to get the price of a token as the price of the derivative. This shouldn't be done because the price of the derivative will not always align with the price of the original token. The primitives that support the derivative may have issues which do not affect the original token which can lead to price discrepancies.

In `LybraStETHVault` it is inferred that the price of ETH is returned from the oracle instead of stEth. Lido the entity supporting stEth can have issues like smart contract hacks which might affect the price of stEth but not Eth. 

https://github.com/code-423n4/2023-06-lybra/blob/dc901a3560b71ed2376feb6418b3d81e3d067fb9/contracts/lybra/pools/LybraStETHVault.sol#L107C1-L109C6

### Unchecked input parameter
The Natspec comment specifies that `amount` argument must be greater than zero in the `burn` function of `LybraEUSDVaultBase` but it isn't checked in the code. 

https://github.com/code-423n4/2023-06-lybra/blob/dc901a3560b71ed2376feb6418b3d81e3d067fb9/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L132-L144

### An address's positions health can't be checked publicly
A user's position's health should be easily accessible by other contracts and the outside world. Currently, the _checkHealth function is internal in `LybraEUSDVaultBase` so it's not easily accessible in `LybraStETHVault` which inherits from it. Other entities would have to perform multiple calls to get the health of an address's position.

https://github.com/code-423n4/2023-06-lybra/blob/dc901a3560b71ed2376feb6418b3d81e3d067fb9/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L291C1-L293
