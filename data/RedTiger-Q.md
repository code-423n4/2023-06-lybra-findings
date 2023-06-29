# Low Risk and Non-Critical Issues
## L01 -  No part of the bounty is returned to the mining pool
According to Lybra [blog post](https://medium.com/@Lybra_Finance/lybra-finance-v1-v2-and-the-future-e458d541cc0b)    a percentage of the rewards bounty sold at a discount should be returned to the mining pool
" If the qualifier drops below the minimum 5% threshold, the user will become ineligible for subsequent esLBR emissions. Simultaneously, a bounty equal to the amount of emissions that the user has earned while ineligible will be placed. This bounty can be purchased by any user at a 50% discount in LBR.

3. The LBR received will then be distributed as follows: 10% will be burned, and the remaining 90% will be returned to the mining pool."

However, we can see in the code that 100% of the bounty is burned.
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/EUSDMiningIncentives.sol#L218
   ```solidity
 IesLBR(LBR).burn(msg.sender, biddingFee);
```

## L02-  Error in the comments of reStake() in ProtocolRewardsPool.sol
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/ProtocolRewardsPool.sol#L139C22-L140C8
 * @dev Convert unredeemed and converting ESLBR tokens back to LBR.
Should be 
 * @dev Convert unredeemed and converting LBR tokens back to ESLBR

## L03 Wrong address in the comments for WBETH
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/LybraWbETHVault.sol#L16
The address of the WBETH contract listed in the comments for WBETH is 0xae78736Cd615f374D3085123A210448E74Fc6393. 
But this address is the goerli RETH address. This comment should be change to the WBETH goerli address. Or mainet WBETH address.
 

## L04 Allowance check could be improved
lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L131
The allowance check in the liquidiation function checks only that the allowance is greater than zero. A more complete check should be that EUSD.allowance(provider, address(this)) > assetAmount.

     require(EUSD.allowance(provider, address(this)) > 0, "provider should authorize to provide liquidation EUSD");
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L160
https://github.com/code-423n4/2023-06-


## L05 stETH mentionned 3 times in LybraPeUSDVaultBase.sol
LybraPeUSDVaultBase.sol is about non-rebasing LST. So it should not mention stETH.
But 3 times in the comments of this file stETH is mentionned.
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L80

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L123C11-L123C11

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L149

## L06 esLBR total supply is not clear

The maxSupply of esLBR is defined as 100 000 000 tokens here
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/esLBR.sol#L20
  ```solidity
uint256 maxSupply = 100_000_000 * 1e18;
```

But the comments at the beginning of the file mention a limit of 55 millions tokens in the esLBRMinter, and it is the only way to get esLBR

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/esLBR.sol#L6C91-L6C91
  ```solidity 
//The maximum amount that can be minted through the esLBRMinter contract is 55 million.
```


## L07 Incosistency between Price of Oracle used
The app uses the liquity oracle to get the price of ETHER, but also sometimes uses the chainlink ETH/USD feed directly. These are not the same as the Liquity Oracle uses chainlink but also TELLOR as a backup and contains additional checks.
Liquity Oracle
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L242
Chainlink Oracle : 
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/EUSDMiningIncentives.sol#L63

## L08 - executeFlashloan doest not check if there is enough PEUSD in the contract to lend
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/PeUSDMainnetStableVision.sol#L129
executeFlashloan  does not check if there is enough PEUSD in the contract to lend.


## N01- No need for LBR to be an OFT token
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/LBR.sol#L13
  ```solidity 
contract LBR is BaseOFTV2, ERC20 {
```
It is not mentionned in any documentations that LBR should be an OFT token. So LBR should not be BaseOFTV2.


## N02 - Stuck ETHER due to Unecessary Payable functions  
In PeUSDMainnetStableVision.sol , convertToPeUSDAndCrossChain and executeFlashloan are payable, but they don't manipulate ETHER. They should not be payable. As a result Ether can be stuck in the contract since there is no way to distribue this ETHER is sent by error. Eiher remove Payable when not needed or use " address(this).balance" to get the amount of Ether in the contract before depositing to LST to avoid Ether being stuck.
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/PeUSDMainnetStableVision.sol#L102
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/PeUSDMainnetStableVision.sol#L129
