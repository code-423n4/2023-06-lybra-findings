Architecture recommendations:

1. The user has to automatically calculate his amount to be above the necessary collateralization ratio. This can be seen in depositAssetToMint() and depositEtherToMint() where user provides mintAmount and then the functions call internal _mintEUSD() and _checkHealth() in it correspondingly. _checkHealth() checks whether the collateralization ratio of newly minted amount is above the lower limit. It's not very convenient for the users to calculate this amount by themselves and it's more likely for them to make a slight mistake after which the function will be reverted due to _checkHealth() revert. So one of the suggestions will be change the way deposit functions work and make the mechanism for automatic calculation of minted amount according to the collateral provided so it's above the collateralization ratio.

Code snipppet:

depositAssetToMint():
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L83

_mintEUSD():

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L72-87

_checkHealth():

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L291-293

2. There is no any economic incentive for a user to deposit his collateral without minting tokens. This means that there is no point to make the mintAmount parameter that's provided by the user. If mintAmount == 0, it means that the collateral provided will just be "stuck" in the contract and not used anyhow until it can be withdrawn after 3 days period. But the purpose of the protocol is to attract collateral and mint interest-bearing stablecoin. So there is no any sense in providing mintAmount == 0 and also in the check whether mintAmount > 0 or not. The suggestion here is just to allow user to provide only assetAmount and then the protocol itself will calculate mintAmount for this particular assetAmount.

Code snippet:

mintAmount parameter:

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L72

mintAmount check:

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L82-84

3. It's better for the protocol to have the ability to make flexible changes to the badCollateralRatio value (one of my findings). At the moment it's in between 130 and 150% and cannot be higher than 150% as determined by the require statement:

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L127

This means that the user will be heavily subjected to liquidations due to slight market fluctuations. It's better to have some lower limit but it's better not to have higher limit so the user can overcollateralize the minting.

Centralization risks:

1. The DAO can just deactivate any existing vault any time by calling setMintVault(). 

Code snippet:

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L109-111

2. The DAO can manipulate the total supply by changing the maxSupply - this can affect the price of the stablecoin and potentially create some deviations. If the DAO is a malicious actor, it can make supply infinite.

Code snippet: https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L120

3. The DAO can also stop the repayment functionality (this is related to my finding) while the users are eligble for liquidations. This should not be the case for the protocol and it's recommended not to have this function and if there is one, make the mechanism so that it's used only in the case of critical emergency.

Code snippet: https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L158-160

### Time spent:
10 hours