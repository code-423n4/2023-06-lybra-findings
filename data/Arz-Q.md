## Issue 1 - getExchangeRatio() function does not exist

Contract: LybraRETHVault.sol
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/LybraRETHVault.sol#L47

In LybraRETHVault.sol getExchangeRatio() function is used when calculating the asset price.

However the Rocket pool contract(0xae78736Cd615f374D3085123A210448E74Fc6393) doesnt have a function that is called getExchangeRatio(). It only has a function that is called getExchangeRate(). If getExchangeRatio() would get called then the call would revert.

## Recommended Mitigation Steps
Use getExchangeRate()
```solidity
function getAssetPrice() public override returns (uint256) {
     return (_etherPrice() * IRETH(address(collateralAsset)).getExchangeRate()) / 1e18;
}

```


## Issue 2 - A user is allowed to burn 0 EUSD in the EUSD Vault
Contract: LybraEUSDVaultBase.sol
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L140

The burn() function in LybraEUSDVaultBase.sol doesnt check if the amount that the user is burning is 0. 
Above the function is written a requirement: `amount` Must be higher than 0 however this isnt checked in the fuction

## Recommended Mitigation Steps
Check if the amount that the user is burning is 0

```solidity

function burn(address onBehalfOf, uint256 amount) external {
        require(amount > 0, "ZERO_BURN");
        require(onBehalfOf != address(0), "BURN_TO_THE_ZERO_ADDRESS");
        _repay(msg.sender, onBehalfOf, amount);
    }
```