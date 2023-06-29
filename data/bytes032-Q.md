## QA REPORT

|      | Issue                                                                                     |
| ---- |:----------------------------------------------------------------------------------------- |
| [01] | Flash loans can be exploited by whales                                                    |
| [02] | The owner can set the flash loan fee to 100%                                              |
| [03] | Not following LayerZero's integration checklist                                           |
| [04] | EVM vs NON-EVM                                                                            |
| [05] | When setting tokenMiners, there could be duplicates that would lead to overwriting values |
| [06] | The user can select non-existing lock settings                                            |
| [07] | If vault mint fee is not set, fees will be lost                                           |
| [08] | Optional proposal threshold is not optional                                               |
| [09] | Allowance might not be enough                                                             |
| [10] | Keeper reward can be 0                                                                                          |


## [01] Flash loans can be exploited by whales   

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.19;
import "forge-std/Test.sol";
import "forge-std/console.sol";
import "@openzeppelin/contracts/utils/math/SafeMath.sol";

contract Test3 is Test {
  using SafeMath for uint256;
  uint256 private _totalShares = 13346578470111834461045143;
  uint256 private _totalSupply = 109628939112783448801625680;


  function getSharesByMintedEUSD(uint256 _EUSDAmount) public view returns (uint256) {
        uint256 totalMintedEUSD = _totalSupply;
        if (totalMintedEUSD == 0) {
            return 0;
        } else {
            return _EUSDAmount.mul(_totalShares).div(totalMintedEUSD);
        }
    }

  
    function getMintedEUSDByShares(uint256 _sharesAmount) public view returns (uint256) {
        if (_totalShares == 0) {
            return 0;
        } else {
            return _sharesAmount.mul(_totalSupply).div(_totalShares);
        }
    }
    
    function mint(uint256 _mintAmount) external  returns (uint256 newTotalShares) {

        uint256 sharesAmount = getSharesByMintedEUSD(_mintAmount);
        if (sharesAmount == 0) {
            //EUSD totalSupply is 0: assume that shares correspond to EUSD 1-to-1
            sharesAmount = _mintAmount;
        }

        newTotalShares = _totalShares.add(sharesAmount);
        _totalShares = newTotalShares;

        _totalSupply += _mintAmount;
  }

   function burn(uint256 _burnAmount) public  returns (uint256 newTotalShares) {
        uint256 sharesAmount = getSharesByMintedEUSD(_burnAmount);
        newTotalShares = _onlyBurnShares(sharesAmount);
        _totalSupply -= _burnAmount;
    }

     function _onlyBurnShares(uint256 _sharesAmount) private returns (uint256 newTotalShares) {
        newTotalShares = _totalShares.sub(_sharesAmount);
        _totalShares = newTotalShares;
    }
  function testPEUSDFlashLoan() external {
    uint256 amount = 500_000_000e18;
    uint256 shareAmount = getSharesByMintedEUSD(amount); 
    burn(shareAmount);
    assertEq(getMintedEUSDByShares(shareAmount) < amount, true);
  }
}


```

## [02] The owner can set the flash loan fee to 100%

https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/configuration/LybraConfigurator.sol#L253-L257

```solidity
   function setFlashloanFee(uint256 fee) external checkRole(TIMELOCK) {
        if (fee > 10_000) revert('EL');
        emit FlashloanFeeUpdated(fee);
        flashloanFee = fee;
    }
```

https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/token/PeUSDMainnetStableVision.sol#L163-L167

```solidity

    function getFee(uint256 share) public view returns (uint256) {
        return (share * configurator.flashloanFee()) / 10_000;
    }

```

## [03] Not following LayerZero's integration checklist

In [LZ's](https://layerzero.gitbook.io/docs/evm-guides/layerzero-integration-checklist) docs, the first check is:

> Use the latest version of [`solidity-examples`](https://www.npmjs.com/package/@layerzerolabs/solidity-examples) package. Do not copy contracts from LayerZero repositories directly to your project.

However, in Lybra all of the files are copied instead

![](https://i.imgur.com/TogUcxk.png)

## [04] EVM vs NON-EVM

Currently, Lybra uses OFTv2, which is for both EVM and non-EVM chains. However, [LZ's](https://layerzero.gitbook.io/docs/evm-guides/layerzero-integration-checklist) docs state that:

> For bridging only between EVM chains use `OFT` and for bridging between EVM and non EVM chains (e.g., Aptos) use `OFTV2`.

![](https://i.imgur.com/O4cNmWZ.png)


## [05] When setting tokenMiners, there could be duplicates that would lead to overwriting values

For example, if the same address is set two times (1x true, 1x false)

https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/configuration/LybraConfigurator.sol#L235-L241

```solidity
    function setTokenMiner(address[] calldata _contracts, bool[] calldata _bools) external checkRole(TIMELOCK) {
        for (uint256 i = 0; i < _contracts.length; i++) {
            tokenMiner[_contracts[i]] = _bools[i];
            emit tokenMinerChanges(_contracts[i], _bools[i]);
        }
    }

```

## [06] The user can select non-existing lock settings

https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/miner/esLBRBoost.sol#L25-L30

```solidity
   constructor() {
        esLBRLockSettings.push(esLBRLockSetting(30 days, 20 * 1e18));
        esLBRLockSettings.push(esLBRLockSetting(90 days, 30 * 1e18));
        esLBRLockSettings.push(esLBRLockSetting(180 days, 50 * 1e18));
        esLBRLockSettings.push(esLBRLockSetting(365 days, 100 * 1e18));
    }
```

https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/miner/esLBRBoost.sol#L38-L45

```solidity
    function setLockStatus(uint256 id) external {
>        esLBRLockSetting memory _setting = esLBRLockSettings[id];
        LockStatus memory userStatus = userLockStatus[msg.sender];
        if (userStatus.unlockTime > block.timestamp) {
            require(userStatus.duration <= _setting.duration, "Your lock-in period has not ended, and the term can only be extended, not reduced.");
        }
        userLockStatus[msg.sender] = LockStatus(block.timestamp + _setting.duration, _setting.duration, _setting.miningBoost);
    }
```

```diff
    function setLockStatus(uint256 id) external {
        esLBRLockSetting memory _setting = esLBRLockSettings[id];
+        require(_setting.duration > 0)
        LockStatus memory userStatus = userLockStatus[msg.sender];
        if (userStatus.unlockTime > block.timestamp) {
            require(userStatus.duration <= _setting.duration, "Your lock-in period has not ended, and the term can only be extended, not reduced.");
        }
        userLockStatus[msg.sender] = LockStatus(block.timestamp + _setting.duration, _setting.duration, _setting.miningBoost);
    }
```

## [07] If vault mint fee is not set, fees will be lost

The default value of the [variable is 0](https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/configuration/LybraConfigurator.sol#L213-L217). It has to be explicitly set to > 0.

https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L300-L302

```solidity
 function _newFee() internal view returns (uint256) {
        return (poolTotalEUSDCirculation * configurator.vaultMintFeeApy(address(this)) * (block.timestamp - lastReportTime)) / (86400 * 365) / 10000;
    }
```

## [08] Optional proposal threshold is not optional

[Lybra's docs state that](https://docs.lybra.finance/lybra-v2-technical-beta/governance/overview)

> We can optionally set a proposal threshold as well. This restricts proposal creation to accounts who have enough voting power.

However, when taking a look at the function, you can observe there's no way the proposal threshold can be set as optional. It will always be applied

https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/governance/LybraGovernance.sol#L167-L173

```solidity
    /**
     * @dev Part of the Governor Bravo's interface: _"The number of votes required in order for a voter to become a proposer"_.
     */
    function proposalThreshold() public pure override returns (uint256) {
        return 1e23;
    }

```

## [09] Allowance might not be enough

When attempting to liquidate, the PeUSDVault checks if the provider's allowance > 0.

https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L125-L132

```solidity
    function liquidation(address provider, address onBehalfOf, uint256 assetAmount) external virtual {
        uint256 assetPrice = getAssetPrice();
        uint256 onBehalfOfCollateralRatio = (depositedAsset[onBehalfOf] * assetPrice * 100) / getBorrowedOf(onBehalfOf);
        require(onBehalfOfCollateralRatio < configurator.getBadCollateralRatio(address(this)), "Borrowers collateral ratio should below badCollateralRatio");

        require(assetAmount * 2 <= depositedAsset[onBehalfOf], "a max of 50% collateral can be liquidated");
>        require(PeUSD.allowance(provider, address(this)) > 0, "provider should authorize to provide liquidation EUSD");
        uint256 peusdAmount = (assetAmount * assetPrice) / 1e18;
```

However, the allowance being > 0 doesn't neccessarily mean that its going to be enough for liquidation.

If allowance > 0, but < peusdAmount, the transaction will revert and lead to loss of gas for the callee.

Instead, the check should be `require(EUSD.allowance(provider, address(this)) >= peusdAmount, "provider should authorize to provide liquidation PEuSD");`

## [10] Keeper reward can be 0

When keepers initiate a liquidation, they are due a reward depending of the currently set configuratio.

https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L167-L171

```solidity
   uint256 reward2keeper;
        if (provider == msg.sender) {
            collateralAsset.transfer(msg.sender, reducedAsset);
        } else {
            reward2keeper = (reducedAsset * configurator.vaultKeeperRatio(address(this))) / 110;
```

However, currently the configuration allows for the ratio to be 0.

https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/configuration/LybraConfigurator.sol#L224-L228

```solidity
  function setKeeperRatio(address pool,uint256 newRatio) external checkRole(TIMELOCK) {
        require(newRatio <= 5, "Max Keeper reward is 5%");
        vaultKeeperRatio[pool] = newRatio;
        emit KeeperRatioChanged(pool, newRatio);
    }
```

In this case, the keepers would still be able execute the transaction, but get no rewards.

It's recommended to check if the `msg.sender != provider` and revert if the ratio is 0.