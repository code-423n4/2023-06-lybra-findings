## Links to affected code:
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/LybraWbETHVault.sol#L16

## Impact

According to the developer’s comment in `LybraWbETHVault.sol`, the WBETH contract address is intended to be `0xae78736Cd615f374D3085123A210448E74Fc6393`, which is rETH Token address.
In case rETH Token address is passed in the `LybraWbETHVault.sol` constructor the contract will not be functional.

## Proof of Concept

IWBETH interface has a deposit function 
`function deposit(address referral) external payable;`
While there is no deposit function in rETH.

## Tools Used
Manual Review

## Recommended Mitigation Steps
Use the correct WBETH Token address `0xa2E3356610840701BDf5611a53974510Ae27E2e1`
