## Caching in local variable

Solidity by default checks underflow/overflow when doing any calculation, this uses some gas and calculating same values multiple times is not good approach. In [ProtocolRewardsPool](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/ProtocolRewardsPool.sol#L190) inside `getReward()` function `reward - eUSDShare` is used multiple times. It should be cached in local variable and use the variable insted of calculate it multiple times.

## Using `constant` variables

In [esLBR](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/esLBR.sol#L20) and [LBR](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/LBR.sol#L15) contracts `maxSupply` variable should be changed to `constant` variable since its value is not changing in the code, this can reduce gas costs.

## Using `immutable` variables

These variables can be changed to `immutable` since their value its not changing after initialization

1. [Line 50](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/configuration/LybraConfigurator.sol#L50) in LybraConfigurator contract

2. [Line 55](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/EUSDMiningIncentives.sol#L55) in EUSDMiningIncentives contract

3. [Line 11](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/governance/LybraGovernance.sol#L11C19-L11C24) and [Line 12](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/governance/LybraGovernance.sol#L12) in LybraGovernance contract

## Using ternary operator insted of if statement

Using ternary operator can reduce deployment size and deployment costs

1. [Line 301](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/EUSD.sol#L301) and [Line 312](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/EUSD.sol#L312) in EUSD contract can be changed to:
```solidity
return totalMintedEUSD == 0 ? 0 : _EUSDAmount.mul(_totalShares).div(totalMintedEUSD);
```
```solidity
return _totalShares == 0 ? 0 : _sharesAmount.mul(_totalSupply).div(_totalShares);
```

2. [Line 178](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/OFT/OFTCoreV2.sol#L178) in OFTCoreV2 contract can be changed to:
```solidity
useCustomAdapterParams ? _checkGasLimit(_dstChainId, _pkType, _adapterParams, _extraGas) : require(_adapterParams.length == 0, "OFTCore: _adapterParams must be empty.");
```

## Change `memory` to `calldata`

In these external functions you can use `calldata` for function arguments insted of memory to reduce gas costs

1. [Line 93](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/EUSDMiningIncentives.sol#L93) in EUSDMiningIncentives contract

2. [Line 33](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/esLBRBoost.sol#L33) in esLBRBoost contract

## Reduce for loop gas costs

In EUSDMiningIncentives contract at [Line 138](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/EUSDMiningIncentives.sol#L138) insted of reading the array data from storage you can cache it in local variable and reading it using the declared variable.


Also for all of the the `for loops` inside the protocol if you are sure the value you iterate from its not going to overflow you can increment the `i` inside `unchecked` scope, here is an example:
```solidity
for (uint256 i = 0; i < array.length; ) {
    // do something
    unchecked {
	  i++;
    }
}
```