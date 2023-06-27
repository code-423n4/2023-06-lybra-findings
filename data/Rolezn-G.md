## GAS Summary<a name="GAS Summary">

### Gas Optimizations
| |Issue|Contexts|Estimated Gas Saved|
|-|:-|:-|:-:|
| [GAS&#x2011;1](#GAS&#x2011;1) | Consider activating `via-ir` for deploying | 1 | - |
| [GAS&#x2011;2](#GAS&#x2011;2) | Use assembly to emit events | 53 | 2014 |
| [GAS&#x2011;3](#GAS&#x2011;3) | Use `assembly` to write address storage values | 4 | - |
| [GAS&#x2011;4](#GAS&#x2011;4) | Structs can be packed into fewer storage slots by editing time variables | 1 | 2000 |
| [GAS&#x2011;5](#GAS&#x2011;5) | State variables can be packed into fewer storage slots | 3 | 6000 |
| [GAS&#x2011;6](#GAS&#x2011;6) | Use solidity version 0.8.20 to gain some gas boost | 21 | 1848 |

Total: 83 contexts over 6 issues

## Gas Optimizations

### <a href="#GAS Summary">[GAS&#x2011;1]</a><a name="GAS&#x2011;1"> Consider activating `via-ir` for deploying

The IR-based code generator was introduced with an aim to not only allow code generation to be more transparent and auditable but also to enable more powerful optimization passes that span across functions.

You can enable it on the command line using `--via-ir` or with the option `{"viaIR": true}`.

This will take longer to compile, but you can just simple test it before deploying and if you got a better benchmark then you can add --via-ir to your deploy command

More on: https://docs.soliditylang.org/en/v0.8.17/ir-breaking-changes.html




### <a href="#GAS Summary">[GAS&#x2011;2]</a><a name="GAS&#x2011;2"> Use assembly to emit events

We can use assembly to emit events efficiently by utilizing `scratch space` and the `free memory pointer`. This will allow us to potentially avoid memory expansion costs.
Note: In order to do this optimization safely, we will need to cache and restore the free memory pointer.

For example, for a generic `emit` event for `eventSentAmountExample`:
```solidity
// uint256 id, uint256 value, uint256 amount
emit eventSentAmountExample(id, value, amount);
```

We can use the following assembly emit events:

```solidity
        assembly {
            let memptr := mload(0x40)
            mstore(0x00, calldataload(0x44))
            mstore(0x20, calldataload(0xa4))
            mstore(0x40, amount)
            log1(
                0x00,
                0x60,
                // keccak256("eventSentAmountExample(uint256,uint256,uint256)")
                0xa622cf392588fbf2cd020ff96b2f4ebd9c76d7a4bc7f3e6b2f18012312e76bc3
            )
            mstore(0x40, memptr)
        }
```

#### <ins>Proof Of Concept</ins>

<details>

```solidity
129: emit SafeCollateralRatioChanged(pool, newRatio);

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/configuration/LybraConfigurator.sol#L129

```solidity
139: emit ProtocolRewardsPoolChanged(addr, block.timestamp);

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/configuration/LybraConfigurator.sol#L139

```solidity
149: emit EUSDMiningIncentivesChanged(addr, block.timestamp);

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/configuration/LybraConfigurator.sol#L149

```solidity
189: emit RedemptionFeeChanged(newFee);

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/configuration/LybraConfigurator.sol#L189

```solidity
205: emit SafeCollateralRatioChanged(pool, newRatio);

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/configuration/LybraConfigurator.sol#L205

```solidity
216: emit BorrowApyChanged(pool, newApy);

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/configuration/LybraConfigurator.sol#L216

```solidity
227: emit KeeperRatioChanged(pool, newRatio);

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/configuration/LybraConfigurator.sol#L227

```solidity
238: emit tokenMinerChanges(_contracts[i], _bools[i]);

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/configuration/LybraConfigurator.sol#L238

```solidity
255: emit FlashloanFeeUpdated(fee);

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/configuration/LybraConfigurator.sol#L255

```solidity
271: emit RedemptionProvider(msg.sender, _bool);

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/configuration/LybraConfigurator.sol#L271

```solidity
198: emit ClaimReward(msg.sender, reward, block.timestamp);

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L198

```solidity
222: emit ClaimedOtherEarnings(msg.sender, user, reward, biddingFee, useEUSD, block.timestamp);

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L222

```solidity
241: emit NotifyRewardChanged(amount, block.timestamp);

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L241

```solidity
76: emit StakeLBR(msg.sender, amount, block.timestamp);

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L76

```solidity
97: emit UnstakeLBR(msg.sender, amount, block.timestamp);

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L97

```solidity
106: emit WithdrawLBR(user, amount, block.timestamp);

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L106

```solidity
146: emit Restake(msg.sender, amount, block.timestamp);

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L146

```solidity
203: emit ClaimReward(msg.sender, EUSD.getMintedEUSDByShares(eUSDShare), address(peUSD), reward - eUSDShare, block.timestamp);
211: emit ClaimReward(msg.sender, EUSD.getMintedEUSDByShares(eUSDShare), address(token), reward - eUSDShare, block.timestamp);
214: emit ClaimReward(msg.sender, EUSD.getMintedEUSDByShares(eUSDShare), address(0), 0, block.timestamp);

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L203

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L211

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L214



```solidity
89: emit StakeToken(msg.sender, _amount, block.timestamp);

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/stakerewardV2pool.sol#L89

```solidity
98: emit WithdrawToken(msg.sender, _amount, block.timestamp);

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/stakerewardV2pool.sol#L98

```solidity
116: emit ClaimReward(msg.sender, reward, block.timestamp);

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/stakerewardV2pool.sol#L116

```solidity
144: emit NotifyRewardChanged(_amount, block.timestamp);

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/stakerewardV2pool.sol#L144

```solidity
38: emit DepositEther(msg.sender, address(collateralAsset), msg.value,balance - preBalance, block.timestamp);

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/LybraRETHVault.sol#L38

```solidity
51: emit DepositEther(msg.sender, address(collateralAsset), msg.value, msg.value, block.timestamp);

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/LybraStETHVault.sol#L51

```solidity
83: emit FeeDistribution(address(configurator), income, block.timestamp);
89: emit FeeDistribution(address(configurator), payAmount, block.timestamp);
94: emit LSDValueCaptured(realAmount, payAmount, getDutchAuctionDiscountPrice(), block.timestamp);

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/LybraStETHVault.sol#L83

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/LybraStETHVault.sol#L89

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/LybraStETHVault.sol#L94



```solidity
31: emit DepositEther(msg.sender, address(collateralAsset), msg.value,balance - preBalance, block.timestamp);

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/LybraWbETHVault.sol#L31

```solidity
45: emit DepositEther(msg.sender, address(collateralAsset), msg.value,wstETHAmount, block.timestamp);

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/LybraWstETHVault.sol#L45

```solidity
85: emit DepositAsset(msg.sender, address(collateralAsset), assetAmount, block.timestamp);

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L85

```solidity
111: emit WithdrawAsset(msg.sender, address(collateralAsset), onBehalfOf, withdrawal, block.timestamp);

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L111

```solidity
175: emit LiquidationRecord(provider, msg.sender, onBehalfOf, eusdAmount, reducedAsset, reward2keeper, false, block.timestamp);

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L175

```solidity
210: emit LiquidationRecord(provider, msg.sender, onBehalfOf, eusdAmount, assetAmount, reward2keeper, true, block.timestamp);

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L210

```solidity
243: emit RigidRedemption(msg.sender, provider, eusdAmount, collateralAmount, block.timestamp);

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L243

```solidity
268: emit Mint(msg.sender, _onBehalfOf, _mintAmount, block.timestamp);

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L268

```solidity
285: emit Burn(_provider, _onBehalfOf, amount, block.timestamp);

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L285

```solidity
69: emit DepositAsset(msg.sender, address(collateralAsset), assetAmount, block.timestamp);

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L69

```solidity
145: emit LiquidationRecord(provider, msg.sender, onBehalfOf, peusdAmount, reducedAsset, reward2keeper, false, block.timestamp);

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L145

```solidity
167: emit RigidRedemption(msg.sender, provider, peusdAmount, collateralAmount, block.timestamp);

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L167

```solidity
184: emit Mint(_provider, _onBehalfOf, _mintAmount, block.timestamp);

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L184

```solidity
209: emit Burn(_provider, _onBehalfOf, amount, block.timestamp);

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L209

```solidity
219: emit WithdrawAsset(_provider, address(collateralAsset), _onBehalfOf, _amount, block.timestamp);

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L219

```solidity
337: emit TransferShares(owner, _recipient, _sharesAmount);
339: emit Transfer(owner, _recipient, tokensAmount);

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/token/EUSD.sol#L337

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/token/EUSD.sol#L339



```solidity
351: emit Transfer(_sender, _recipient, _amount);
352: emit TransferShares(_sender, _recipient, _sharesToTransfer);

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/token/EUSD.sol#L351-L352

```solidity
371: emit Approval(_owner, _spender, _amount);

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/token/EUSD.sol#L371

```solidity
427: emit Transfer(address(0), _recipient, _mintAmount);

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/token/EUSD.sol#L427

```solidity
446: emit Transfer(_account, address(0), _burnAmount);

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/token/EUSD.sol#L446

```solidity
477: emit SharesBurnt(_account, preRebaseTokenAmount, postRebaseTokenAmount, _sharesAmount);

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/token/EUSD.sol#L477

```solidity
138: emit Flashloaned(receiver, eusdAmount, burnShare);

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L138



</details>





### <a href="#GAS Summary">[GAS&#x2011;3]</a><a name="GAS&#x2011;3"> Use `assembly` to write address storage values

#### <ins>Proof Of Concept</ins>

<details>

```solidity
421: _totalShares = newTotalShares;
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/token/EUSD.sol#L421

```solidity
471: _totalShares = newTotalShares;
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/token/EUSD.sol#L471



</details>







### <a href="#GAS Summary">[GAS&#x2011;4]</a><a name="GAS&#x2011;4"> Structs can be packed into fewer storage slots by editing time variables

The following structs contain time variables that are unlikely to ever reach max `uint256` then it is possible to set them as `uint32`, saving gas slots. Max value of `uint32` is equal to Year 2016, which is more than enough.

#### <ins>Proof Of Concept</ins>

<details>

```solidity
18: struct LockStatus {
        uint256 unlockTime;
        uint256 duration;
        uint256 miningBoost;
    }

```

Can save 1 storage slot by changing to:

```solidity
18: struct LockStatus {
        uint128 unlockTime;
        uint128 duration;
        uint256 miningBoost;
    }

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/esLBRBoost.sol#L18



</details>





### <a href="#GAS Summary">[GAS&#x2011;5]</a><a name="GAS&#x2011;5"> State variables can be packed into fewer storage slots

If variables occupying the same slot are both written the same function or by the constructor, avoids a separate Gsset (20000 gas). Reads of the variables can also be cheaper

#### <ins>Proof Of Concept</ins>

<details>

```solidity
37: contract Configurator {
    ...
    uint256 public flashloanFee = 500;
    // Limiting the maximum percentage of eUSD that can be cross-chain transferred to L2 in relation to the total supply.
    uint256 maxStableRatio = 5_000;
    ...
```


Can save 1 storage slot by changing to:

```solidity
37: contract Configurator {
    ...
    uint128 public flashloanFee = 500;
    // Limiting the maximum percentage of eUSD that can be cross-chain transferred to L2 in relation to the total supply.
    uint128 maxStableRatio = 5_000;
    ...
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/configuration/LybraConfigurator.sol#L37

```solidity
27: contract EUSDMiningIncentives is Ownable {
    ...
    // Duration of rewards to be paid out (in seconds)
    uint256 public duration = 2_592_000;
    // Timestamp of when the rewards finish
    uint256 public finishAt;
    // Minimum of last updated time and reward finish time
    uint256 public updatedAt;
```

Can save 2 storage slots by changing to:

```solidity
27: contract EUSDMiningIncentives is Ownable {
    ...
    // Duration of rewards to be paid out (in seconds)
    uint64 public duration = 2_592_000;
    // Timestamp of when the rewards finish
    uint64 public finishAt;
    // Minimum of last updated time and reward finish time
    uint64 public updatedAt;
```


https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L27



</details>



### <a href="#GAS Summary">[GAS&#x2011;6]</a><a name="GAS&#x2011;6"> Use solidity version 0.8.20 to gain some gas boost

Upgrade to the latest solidity version 0.8.20 to get additional gas savings.
See latest release for reference: https://blog.soliditylang.org/2023/05/10/solidity-0.8.20-release-announcement/

#### <ins>Proof Of Concept</ins>


<details>

```solidity
pragma solidity ^0.8.17;
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/configuration/LybraConfigurator.sol#L15

```solidity
pragma solidity ^0.8.17;
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/governance/AdminTimelock.sol#L3

```solidity
pragma solidity ^0.8.17;
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/governance/GovernanceTimelock.sol#L3

```solidity
pragma solidity ^0.8.17;
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/governance/LybraGovernance.sol#L3

```solidity
pragma solidity ^0.8.17;
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/esLBRBoost.sol#L3

```solidity
pragma solidity ^0.8.17;
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L3

```solidity
pragma solidity ^0.8.17;
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L3

```solidity
pragma solidity ^0.8;
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/stakerewardV2pool.sol#L2

```solidity
pragma solidity ^0.8.17;
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/LybraRETHVault.sol#L3

```solidity
pragma solidity ^0.8.17;
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/LybraStETHVault.sol#L3

```solidity
pragma solidity ^0.8.17;
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/LybraWbETHVault.sol#L3

```solidity
pragma solidity ^0.8.17;
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/LybraWstETHVault.sol#L3

```solidity
pragma solidity ^0.8.17;
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L3

```solidity
pragma solidity ^0.8.17;
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L3

```solidity
pragma solidity ^0.8.17;
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/Proxy/LybraProxy.sol#L3

```solidity
pragma solidity ^0.8.17;
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/Proxy/LybraProxyAdmin.sol#L3

```solidity
pragma solidity ^0.8.17;
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/token/esLBR.sol#L3

```solidity
pragma solidity ^0.8.17;
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/token/EUSD.sol#L3

```solidity
pragma solidity ^0.8.17;
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/token/LBR.sol#L3

```solidity
pragma solidity ^0.8.17;
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/token/PeUSD.sol#L3

```solidity
pragma solidity ^0.8.17;
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L14



</details>

