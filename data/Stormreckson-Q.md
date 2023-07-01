1-The `EUSDMiningIncentives.sol` contract functions lack Natspec, add natspec documentation to each function, describing their purpose and parameters. This will help users in understanding what each function purpose is.

2- in `LybraEUSDVaultBase.sol` 
the nastpec states
        //* Emits a `WithdrawEther` event.
        (..)
        function withdraw(address 
        onBehalfOf, uint256 amount) 
        external 
        virtual {
        (...)
        emit WithdrawAsset(msg.sender, 
        address(collateralAsset), 
        onBehalfOf, withdrawal, 
        block.timestamp);

But the code emits a `withdrawAsset` event
This can be confusing for users, making the natspec  compatible with the function statement is good practice.

4-https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L140-L143

Lack of input validation amount can be set to zero in the `burn` function, contradicting the contract's Natspec 

     * - `onBehalfOf` cannot be the 
     zero address.
     * - `amount` Must be higher 
     than 0.
     * @dev Calling the 
      internal`_repay`function.
     */
     function burn(address 
     onBehalfOf, uint256 amount) 
     external {
        require(onBehalfOf != 
     address(0), 
     "BURN_TO_THE_ZERO_ADDRESS");
        _repay(msg.sender, 
      onBehalfOf, amount);
      }



5-https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/token/EUSD.sol#L333-L341

Potential transfer of zero or negative shares without proper validation



This may lead to 
   - Undesired transfers of zero or negative shares can occur.
   - Intended transfers may fail due to the incorrect validation.
   - Account balances and share ownership may become inconsistent.

To mitigate this issue, it is recommended to add a check within the "transferShares" function to verify that sharesAmount is greater than zero before proceeding with the transfer. This ensures that only valid share amounts are transferred, preventing unintended behavior and maintaining the integrity of the application.


6- In `contract PeUSDMainnetStableVision` there are a few missing address zero checks in the following functions

https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/token/PeUSDMainnetStableVision.sol#L79-L80

https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/token/PeUSDMainnetStableVision.sol#L97-L98

https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/token/PeUSDMainnetStableVision.sol#L129-L130

https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/token/PeUSDMainnetStableVision.sol#L142

Adding this check ensures that the user` address provided is valid and not a zero address before executing any further logic within the function.

7-https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/miner/EUSDMiningIncentives.sol#L95-L96

Mapping Access Issue

the mapping access issue in the code is that it leads to incorrect function invocation. The function `configurator.mintVault(_pools[i])` is mistakenly called as if it were a function, while it is actually a mapping. This results in a runtime error and prevents the code from functioning as intended.
During the execution, the code attempts to invoke `configurator.mintVault(_pools[i])` as a function, but due to the mapping access issue, it fails to execute correctly. As a result, it throws a runtime error, and the transaction reverts.
it may cause inconvenience and waste user gas

To fix the issue, we need to modify the code to correctly access the mapping. Replace the incorrect function invocation with `configurator.mintVault[_pools[i]]` in the setPools function. This will ensure that the mapping is accessed properly, allowing the code to function as intended.

8-https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L72-L87

Token validation if the token is stETH in the `depositAssetToMint` function.

In the `depositAssetToMint` function, there should be a check if the token is stEth, it would be appropriate to add a check to verify if the transferred token is indeed stEth.


        require(collateralAsset == 
        
        
       address(stEthTokenContract), 
        "Invalid token"); ensures 
        that the 
        token being transferred is 
        exactly 
        stEth.

9- https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/miner/EUSDMiningIncentives.sol#L137-L138

in the `StakedOf` function the line         uint256 amount; is not initialized before being used in the loop, which could lead to unexpected results. 

To ensure accurate calculations, it is advisable to initialize `amount` to zero before entering the loop, like this:

      uint256 `amount` = 0;

By initializing `amount` to zero, you can guarantee that its value is consistent throughout the execution of the loop.

10- Missing events for critical parameters changes, add events to track changes 

https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/configuration/LybraConfigurator.sol#L109-L113)

https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/configuration/LybraConfigurator.sol#L119-L123

https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/configuration/LybraConfigurator.sol#L158-L162

https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/configuration/LybraConfigurator.sol#L167-L171

https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/configuration/LybraConfigurator.sol#L177-L181

https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/configuration/LybraConfigurator.sol#L246-L251

https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/configuration/LybraConfigurator.sol#L261-L264

11- Function emits wrong event 
The function `setBadCollateralRatio` Emits `emit SafeCollateralRatioChanged(pool, newRatio);`
 Instead of `emit BadCollateralRatioChanged(pool, newRatio);`

(https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/configuration/LybraConfigurator.sol#L126-L131)

12- Stake and unstake function not checking input validation, amount can be zero 

https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/miner/ProtocolRewardsPool.sol#L73-L78)

https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/miner/ProtocolRewardsPool.sol#L87-L98

13- https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/ProtocolRewardsPool.sol#L33-L39

Uint type is not declared explicitly as uint256

it is recommended to explicitly specify the size of integers, even if it is uint256. It improves readability and avoids potential confusion or compatibility issues when interacting with other contracts.