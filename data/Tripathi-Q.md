### Low Risk Issues

| |Issue|Instances|
|-|:-|:-:|
| [L&#x2011;01] | `_mintEUSD ` emits incorrect parameter | 1 | 

### [L&#x2011;01] `_mintEUSD ` emits incorrect parameter
`_mintEUSD ` is a internal function in pool/base contracts which emit an event after sucessfull minting of eUSD. 
Currently first parameter is `msg.sender` which should be replaced by `_provider`. The fact that, everywhere `msg.sender` is same as `_provider` in current implementation is not creating any problem .But this will emit incorrect event if `msg.sender` and `_provider` are different in any case.

```solidity
File: contracts/lybra/pools/base/LybraEUSDVaultBase.sol

268:                emit Mint(msg.sender, _onBehalfOf, _mintAmount, block.timestamp);

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L268C5-L268C74



