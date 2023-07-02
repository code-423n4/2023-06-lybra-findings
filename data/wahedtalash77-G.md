# Gas Optimization

## [G-1] If statements that use && can be refactored into nested if statements
204: `if (msg.sender != provider && onBehalfOfCollateralRatio >= 1e20 + configurator.vaultKeeperRatio(address(this)) * 1e18) {`
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L204

## [G-2] Rearranging order of state variable declarations to pack them will save storage slots and gas
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L21-L31

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L31-L57

## [G-3] Events are not indexed
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L38C7-L44

## [G-04]  Avoiding Initialization of Loop Index If It Is 0 and Avoid unnecessary read of array length in for loops can save gas.
94: `  for (uint i = 0; i < _pools.length; i++) {`
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L94

138: ` for (uint i = 0; i < pools.length; i++) {`
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L138

