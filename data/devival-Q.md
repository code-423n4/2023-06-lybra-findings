## 1 Title - LOW
Wrong WBETH contract address in `LybraWbETHVault.sol`

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

## 2 Title - LOW
Tokens deployed through `PeUSD.sol` contract cannot be used due to the lack of `mint` and `burn` functionalities.

## Links to affected code

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/PeUSD.sol#L8

## Impact

All contracts calling `mint` and `burn` functions in `PeUSD` will fail the transaction. The purpose of `PeUSD.sol` remains unclear, as there is also `PeUSDMainnetStableVision.sol` which contains all the functionality.

## Proof of Concept
Provide direct links to all referenced code in GitHub. Add screenshots, logs, or any other relevant proof that illustrates the concept.

`PeUSD.sol` file description: 

"PeUSD is a stable, interest-free ERC20-like token minted through eUSD in the Lybra protocol. It is pegged to 1eUSD and does not undergo rebasing. The token operates by allowing users to deposit eUSD and mint an equivalent amount of PeUSD. When users redeem PeUSD, they can retrieve the corresponding proportion of eUSD. As a result, users can utilize PeUSD without sacrificing the yield on their eUSD holdings.

In addition to minting PeUSD by using eUSD as collateral, PeUSD can also be minted by depositing assets (such as WstETH) into non-rebase asset vaults.PeUSD leverages the LayerZero's OFT protocol to enable native cross-chain functionality, allowing seamless transfers and interactions across different blockchain networks. 

By integrating with OFT, PeUSD is not constrained by liquidity pools and can freely move between chains. This interoperability enhances the versatility and utility of PeUSD, empowering users with the ability to utilize PeUSD's stable value and features across multiple blockchain ecosystems."

According to the description, `...PeUSD can also be minted by depositing assets (such as WstETH) into non-rebase asset vaults...`. This is not possible in the `PeUSD.sol` contract. However, it is acknowledged `PeUSDMainnetStableVision.sol` contains all the required functions.
It is also possible that the team intends to use `PeUSD.sol` to deploy it on L2, however, this information does not seem to be available.

## Tools Used
Manual Review

## Recommended Mitigation Steps
Implement `mint` and `burn` functions in `PeUSD.sol` or provide more explicit description of the intented purpose of the contract.

