## QA Summary<a name="QA Summary">

### Low Risk Issues
| |Issue|Contexts|
|-|:-|:-:|
| [LOW&#x2011;1](#LOW&#x2011;1) | IERC20 `approve()` Is Deprecated | 2 |
| [LOW&#x2011;2](#LOW&#x2011;2) | Consider the case where `totalsupply` is 0 | 1 |
| [LOW&#x2011;3](#LOW&#x2011;3) | Do not allow fees to be set to `100%` | 2 |
| [LOW&#x2011;4](#LOW&#x2011;4) | External calls in an un-bounded `for-`loop may result in a DOS | 2 |
| [LOW&#x2011;5](#LOW&#x2011;5) | Hardcoded String that is repeatedly used can be replaced with a constant | 4 |
| [LOW&#x2011;6](#LOW&#x2011;6) | Lack of checks-effects-interactions | 2 |
| [LOW&#x2011;7](#LOW&#x2011;7) | Minting tokens to the zero address should be avoided | 1 |
| [LOW&#x2011;8](#LOW&#x2011;8) | The Contract Should `approve(0)` First | 2 |
| [LOW&#x2011;9](#LOW&#x2011;9) | Contracts are not using their OZ Upgradeable counterparts | 23 |
| [LOW&#x2011;10](#LOW&#x2011;10) | There should be an upper limit for flash loan settings | 2 |
| [LOW&#x2011;11](#LOW&#x2011;11) | Remove unused code | 8 |
| [LOW&#x2011;12](#LOW&#x2011;12) | Unbounded loop | 1 |
| [LOW&#x2011;13](#LOW&#x2011;13) | Upgrade OpenZeppelin Contract Dependency | 1 |
| [LOW&#x2011;14](#LOW&#x2011;14) | Use `safeTransferOwnership` instead of `transferOwnership` function | 4 |
| [LOW&#x2011;15](#LOW&#x2011;15) | `_creditTo` call should avoid setting to 0 `amount` and to zero `address` | 1 |

Total: 56 contexts over 15 issues

### Non-critical Issues
| |Issue|Contexts|
|-|:-|:-:|
| [NC&#x2011;1](#NC&#x2011;1) | No need for `== true` or `== false` checks | 1 |
| [NC&#x2011;2](#NC&#x2011;2) | Critical Changes Should Use Two-step Procedure | 33 |
| [NC&#x2011;3](#NC&#x2011;3) | `block.timestamp` is already used when emitting events, no need to input timestamp | 36 |
| [NC&#x2011;4](#NC&#x2011;4) | Immutables can be named using the same convention | 18 |
| [NC&#x2011;5](#NC&#x2011;5) | Initial value check is missing in Set Functions | 31 |
| [NC&#x2011;6](#NC&#x2011;6) | Omissions in Events | 1 |
| [NC&#x2011;7](#NC&#x2011;7) | Strings should use double quotes rather than single quotes | 1 |
| [NC&#x2011;8](#NC&#x2011;8) | Using underscore at the end of variable name | 1 |
| [NC&#x2011;9](#NC&#x2011;9) | Large multiples of ten should use scientific notation | 11 |
| [NC&#x2011;10](#NC&#x2011;10) | Use named function calls | 1 |
| [NC&#x2011;11](#NC&#x2011;11) | `maxSupply` should be set as a `constant` | 2 |

Total: 136 contexts over 11 issues

## Low Risk Issues

### <a href="#QA Summary">[LOW&#x2011;1]</a><a name="LOW&#x2011;1"> IERC20 `approve()` Is Deprecated

`approve()` is subject to a known front-running attack. It is deprecated in favor of `safeIncreaseAllowance()` and `safeDecreaseAllowance()`. If only setting the initial allowance to the value that means infinite, `safeIncreaseAllowance()` can be used instead.

https://docs.openzeppelin.com/contracts/3.x/api/token/erc20#IERC20-approve-address-uint256-

#### <ins>Proof Of Concept</ins>

<details>

```solidity
302: EUSD.approve(address(curvePool), balance);
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/configuration/LybraConfigurator.sol#L302

```solidity
35: lido.approve(address(collateralAsset), msg.value);
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/LybraWstETHVault.sol#L35



</details>

#### <ins>Recommended Mitigation Steps</ins>

Consider using `safeIncreaseAllowance()` / `safeDecreaseAllowance()` instead.




### <a href="#QA Summary">[LOW&#x2011;2]</a><a name="LOW&#x2011;2"> Consider the case where totalsupply is 0

Consider the case where `totalsupply` is 0. When `totalsupply` is 0, it should return 0 directly, because there will be an error of dividing by 0.

#### <ins>Impact</ins>

This would cause the affected functions to revert and as a result can lead to potential loss.

#### <ins>Proof Of Concept</ins>

<details>

```solidity
79: return rewardPerTokenStored + (rewardRatio * (lastTimeRewardApplicable() - updatedAt) * 1e18) / totalSupply;

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/stakerewardV2pool.sol#L79



</details>


#### <ins>Recommended Mitigation Steps</ins>
Add check for zero value and return 0.

```solidity
if ( totalSupply() == 0) return 0;
```



### <a href="#QA Summary">[LOW&#x2011;3]</a><a name="LOW&#x2011;3"> Do not allow fees to be set to `100%`

It is recommended from a risk perspective to disallow setting 100% fees at all. A reasonable fee maximum should be checked for instead.

#### <ins>Proof Of Concept</ins>


<details>

```solidity

function setRedemptionFee(uint256 newFee) external checkRole(TIMELOCK) {
        require(newFee <= 500, "Max Redemption Fee is 5%");
        redemptionFee = newFee;
        emit RedemptionFeeChanged(newFee);
    }

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/configuration/LybraConfigurator.sol#L186

```solidity

function setFlashloanFee(uint256 fee) external checkRole(TIMELOCK) {
        if (fee > 10_000) revert('EL');
        emit FlashloanFeeUpdated(fee);
        flashloanFee = fee;
    }

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/configuration/LybraConfigurator.sol#L253



</details>





### <a href="#QA Summary">[LOW&#x2011;4]</a><a name="LOW&#x2011;4"> External calls in an un-bounded `for-`loop may result in a DOS

Consider limiting the number of iterations in `for-`loops that make external calls

#### <ins>Proof Of Concept</ins>

<details>

```solidity
236: for (uint256 i = 0; i < _contracts.length; i++) {

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/configuration/LybraConfigurator.sol#L236

```solidity
94: for (uint i = 0; i < _pools.length; i++) {

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L94



</details>



### <a href="#QA Summary">[LOW&#x2011;5]</a><a name="LOW&#x2011;5"> Hardcoded String that is repeatedly used can be replaced with a constant

Some strings are repeatedly used in the contracts. For better maintainability, please consider replacing it with a `constant`.
 
#### <ins>Proof Of Concept</ins>

<details>

```solidity
100: return "eUSD";
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/token/EUSD.sol#L100

```solidity
108: return "eUSD";
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/token/EUSD.sol#L108



</details>






### <a href="#QA Summary">[LOW&#x2011;6]</a><a name="LOW&#x2011;6"> Lack of checks-effects-interactions

It's recommended to execute external calls after state changes, to prevent reentrancy bugs.

Consider moving the external calls after the state changes on the following instances.

#### <ins>Proof Of Concept</ins>

<details>

```solidity
85: function stake(uint256 _amount) external updateReward(msg.sender) {
        require(_amount > 0, "amount = 0");
        bool success = stakingToken.transferFrom(msg.sender, address(this), _amount);
        require(success, "TF");
        balanceOf[msg.sender] += _amount;
        totalSupply += _amount;
        emit StakeToken(msg.sender, _amount, block.timestamp);
    }

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/stakerewardV2pool.sol#L85

```solidity
75: function depositAssetToMint(uint256 assetAmount, uint256 mintAmount) external virtual {
        require(assetAmount >= 1 ether, "Deposit should not be less than 1 stETH.");

        bool success = collateralAsset.transferFrom(msg.sender, address(this), assetAmount);
        require(success, "TF");

        totalDepositedAsset += assetAmount;
        depositedAsset[msg.sender] += assetAmount;
        depositedTime[msg.sender] = block.timestamp;

        if (mintAmount > 0) {
            _mintEUSD(msg.sender, msg.sender, mintAmount, getAssetPrice());
        }
        emit DepositAsset(msg.sender, address(collateralAsset), assetAmount, block.timestamp);
    }

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L75

</details>






### <a href="#QA Summary">[LOW&#x2011;6]</a><a name="LOW&#x2011;6"> Minting tokens to the zero address should be avoided

The core function `mint` is used by users to mint an option position by providing token1 as collateral and borrowing the max amount of liquidity. `Address(0)` check is missing in both this function and the internal function `_mint`, which is triggered to mint the tokens to the to address. Consider applying a check in the function to ensure tokens aren't minted to the zero address.

#### <ins>Proof Of Concept</ins>


<details>


```solidity

function mint(address user, uint256 amount) external returns (bool) {
        require(configurator.tokenMiner(msg.sender), "not authorized");
        require(totalSupply() + amount <= maxSupply, "exceeding the maximum supply quantity.");
        try IProtocolRewardsPool(configurator.getProtocolRewardsPool()).refreshReward(user) {} catch {}
        _mint(user, amount);
        return true;
    }

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/token/esLBR.sol#L30



</details>





### <a href="#QA Summary">[LOW&#x2011;8]</a><a name="LOW&#x2011;8"> The Contract Should `approve(0)` First

Some tokens (like USDT L199) do not work when changing the allowance from an existing non-zero allowance value. They must first be approved by zero and then the actual allowance must be approved.

#### <ins>Proof Of Concept</ins>


<details>

```solidity
302: EUSD.approve(address(curvePool), balance);
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/configuration/LybraConfigurator.sol#L302

```solidity
35: lido.approve(address(collateralAsset), msg.value);
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/LybraWstETHVault.sol#L35



</details>

#### <ins>Recommended Mitigation Steps</ins>

Approve with a zero amount first before setting the actual amount.




### <a href="#QA Summary">[LOW&#x2011;9]</a><a name="LOW&#x2011;9"> Contracts are not using their OZ Upgradeable counterparts

The non-upgradeable standard version of OpenZeppelin’s library are inherited / used by the contracts.
It would be safer to use the upgradeable versions of the library contracts to avoid unexpected behaviour.

#### <ins>Proof of Concept</ins>

<details>

```solidity
5: import "@openzeppelin/contracts/governance/TimelockController.sol";

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/governance/AdminTimelock.sol#L5

```solidity
5: import "@openzeppelin/contracts/governance/TimelockController.sol";

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/governance/GovernanceTimelock.sol#L5

```solidity
5: import "@openzeppelin/contracts/governance/extensions/GovernorTimelockControl.sol";

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/governance/LybraGovernance.sol#L5

```solidity
5: import "@openzeppelin/contracts/access/Ownable.sol";

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/esLBRBoost.sol#L5

```solidity
12: import "@openzeppelin/contracts/access/Ownable.sol";

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L12

```solidity
12: import "@openzeppelin/contracts/access/Ownable.sol";
13: import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L12-L13

```solidity
4: import "@openzeppelin/contracts/access/Ownable.sol";
6: import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/stakerewardV2pool.sol#L4-L6

```solidity
7: import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/LybraRETHVault.sol#L7

```solidity
7: import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/LybraWbETHVault.sol#L7

```solidity
7: import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/LybraWstETHVault.sol#L7

```solidity
7: import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L7

```solidity
7: import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L7

```solidity
5: import "@openzeppelin/contracts/proxy/transparent/TransparentUpgradeableProxy.sol";

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/Proxy/LybraProxy.sol#L5

```solidity
4: import "@openzeppelin/contracts/proxy/transparent/ProxyAdmin.sol";

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/Proxy/LybraProxyAdmin.sol#L4

```solidity
10: import "@openzeppelin/contracts/token/ERC20/extensions/ERC20Votes.sol";

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/token/esLBR.sol#L10

```solidity
5: import "@openzeppelin/contracts/utils/math/SafeMath.sol";
6: import "@openzeppelin/contracts/token/ERC20/IERC20.sol";
7: import "@openzeppelin/contracts/utils/Context.sol";

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/token/EUSD.sol#L5-L7

```solidity
9: import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/token/LBR.sol#L9

```solidity
5: import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/token/PeUSD.sol#L5

```solidity
16: import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L16



</details>

#### <ins>Recommended Mitigation Steps</ins>

Where applicable, use the contracts from `@openzeppelin/contracts-upgradeable` instead of `@openzeppelin/contracts`.
See https://github.com/OpenZeppelin/openzeppelin-contracts-upgradeable/tree/master/contracts for list of available upgradeable contracts



### <a href="#QA Summary">[LOW&#x2011;10]</a><a name="LOW&#x2011;10"> There should be an upper limit for flash loan settings

If the admin sets the flash loan too high, the flash loan functionality might be useless as users won't use it.

#### <ins>Proof Of Concept</ins>

<details>

```solidity
56: uint256 public flashloanFee = 500;
256: flashloanFee = fee;

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/configuration/LybraConfigurator.sol#L56

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/configuration/LybraConfigurator.sol#L256





</details>




### <a href="#QA Summary">[LOW&#x2011;11]</a><a name="LOW&#x2011;11"> Remove unused code
This code is not used in the main project contract files, remove it or add event-emit
Code that is not in use, suggests that they should not be present and could potentially contain insecure functionalities.

#### <ins>Proof Of Concept</ins>


<details>

```solidity
function _quorumReached
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/governance/LybraGovernance.sol#L59

```solidity
function _voteSucceeded
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/governance/LybraGovernance.sol#L66

```solidity
function _countVote
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/governance/LybraGovernance.sol#L76

```solidity
function _getVotes
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/governance/LybraGovernance.sol#L98

```solidity
function _debitFrom
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L184

```solidity
function _creditTo
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L191

```solidity
function _transferFrom
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L196

```solidity
function _ld2sdRatio
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L205



</details>




### <a href="#QA Summary">[LOW&#x2011;12]</a><a name="LOW&#x2011;12"> Unbounded loop

New items are pushed into the following arrays but there is no option to `pop` them out. Currently, the array can grow indefinitely. E.g. there's no maximum limit and there's no functionality to remove array values.
If the array grows too large, calling relevant functions might run out of gas and revert. Calling these functions could result in a DOS condition.

#### <ins>Proof Of Concept</ins>

<details>

```solidity
34: esLBRLockSettings.push(setting);

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/esLBRBoost.sol#L34



</details>

#### <ins>Recommended Mitigation Steps</ins>
Add a functionality to delete array values or add a maximum size limit for arrays.






### <a href="#QA Summary">[LOW&#x2011;13]</a><a name="LOW&#x2011;13"> Upgrade OpenZeppelin Contract Dependency

An outdated OZ version is used (which has known vulnerabilities, see: https://github.com/OpenZeppelin/openzeppelin-contracts/security/advisories).
Release: https://github.com/OpenZeppelin/openzeppelin-contracts/releases/tag/v4.9.0

#### <ins>Proof Of Concept</ins>

Require dependencies to be at least version of 4.9.0 to include critical vulnerability patches.

<details>

```solidity
    "@openzeppelin/contracts": "^4.9.1"
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/../package.json#L13



</details>

#### <ins>Recommended Mitigation Steps</ins>

Update OpenZeppelin Contracts Usage in package.json and require at least version of 4.9.0 via `>=4.9.0`






### <a href="#QA Summary">[LOW&#x2011;14]</a><a name="LOW&#x2011;14"> Use `safeTransferOwnership` instead of `transferOwnership` function

Use `safeTransferOwnership` which is safer. Use it as it is more secure due to 2-stage ownership transfer. 
The `Ownable.sol` only uses `transferOwnership` rather than `safeTransferOwnership`

#### <ins>Proof Of Concept</ins>


<details>

```solidity
5: import "@openzeppelin/contracts/access/Ownable.sol";
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/esLBRBoost.sol#L5

```solidity
12: import "@openzeppelin/contracts/access/Ownable.sol";
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L12

```solidity
12: import "@openzeppelin/contracts/access/Ownable.sol";
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L12

```solidity
4: import "@openzeppelin/contracts/access/Ownable.sol";
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/stakerewardV2pool.sol#L4



</details>

#### <ins>Recommended Mitigation Steps</ins>

Use <a href="https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/access/Ownable2Step.sol">Ownable2Step.sol</a>

```solidity
    function acceptOwnership() external {
        address sender = _msgSender();
        require(pendingOwner() == sender, "Ownable2Step: caller is not the new owner");
        _transferOwnership(sender);
    }
}
```


### <a href="#QA Summary">[LOW&#x2011;15]</a><a name="LOW&#x2011;15"> `_creditTo` call should avoid setting to 0 `_amount` and to zero `address` 

The `_creditTo` function can be called with 0 `_amount` and with zero `address` to `_toAddress` which should be avoided.

#### <ins>Proof Of Concept</ins>
```solidity
function _creditTo(uint16, address _toAddress, uint _amount) internal virtual override returns (uint) {
```
#### <ins>Recommended Mitigation Steps</ins>

Add relevant require statements against zero value calls



## Non Critical Issues




### <a href="#QA Summary">[NC&#x2011;1]</a><a name="NC&#x2011;1"> No need for `== true` or `== false` checks

There is no need to verify that `== true` or `== false` when the variable checked upon is a boolean as well.

#### <ins>Proof Of Concept</ins>


<details>

```solidity
82: require(receipt.hasVoted == false, "GovernorBravo::castVoteInternal: voter already voted");

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/governance/LybraGovernance.sol#L82



</details>


#### <ins>Recommended Mitigation Steps</ins>

Instead simply check for `variable` or `!variable`





### <a href="#QA Summary">[NC&#x2011;2]</a><a name="NC&#x2011;2"> Critical Changes Should Use Two-step Procedure

The critical procedures should be two step process.

See similar findings in previous Code4rena contests for reference:
https://code4rena.com/reports/2022-06-illuminate/#2-critical-changes-should-use-two-step-procedure

#### <ins>Proof Of Concept</ins>

<details>

```solidity
109: function setMintVault(address pool, bool isActive) external onlyRole(DAO) {
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/configuration/LybraConfigurator.sol#L109

```solidity
119: function setMintVaultMaxSupply(address pool, uint256 maxSupply) external onlyRole(DAO) {
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/configuration/LybraConfigurator.sol#L119

```solidity
126: function setBadCollateralRatio(address pool, uint256 newRatio) external onlyRole(DAO) {
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/configuration/LybraConfigurator.sol#L126

```solidity
137: function setProtocolRewardsPool(address addr) external checkRole(TIMELOCK) {
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/configuration/LybraConfigurator.sol#L137

```solidity
147: function setEUSDMiningIncentives(address addr) external checkRole(TIMELOCK) {
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/configuration/LybraConfigurator.sol#L147

```solidity
158: function setvaultBurnPaused(address pool, bool isActive) external checkRole(TIMELOCK) {
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/configuration/LybraConfigurator.sol#L158

```solidity
167: function setPremiumTradingEnabled(bool isActive) external checkRole(TIMELOCK) {
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/configuration/LybraConfigurator.sol#L167

```solidity
177: function setvaultMintPaused(address pool, bool isActive) external checkRole(ADMIN) {
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/configuration/LybraConfigurator.sol#L177

```solidity
186: function setRedemptionFee(uint256 newFee) external checkRole(TIMELOCK) {
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/configuration/LybraConfigurator.sol#L186

```solidity
198: function setSafeCollateralRatio(address pool, uint256 newRatio) external checkRole(TIMELOCK) {
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/configuration/LybraConfigurator.sol#L198

```solidity
213: function setBorrowApy(address pool, uint256 newApy) external checkRole(TIMELOCK) {
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/configuration/LybraConfigurator.sol#L213

```solidity
224: function setKeeperRatio(address pool,uint256 newRatio) external checkRole(TIMELOCK) {
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/configuration/LybraConfigurator.sol#L224

```solidity
235: function setTokenMiner(address[] calldata _contracts, bool[] calldata _bools) external checkRole(TIMELOCK) {
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/configuration/LybraConfigurator.sol#L235

```solidity
246: function setMaxStableRatio(uint256 _ratio) external checkRole(TIMELOCK) {
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/configuration/LybraConfigurator.sol#L246

```solidity
253: function setFlashloanFee(uint256 fee) external checkRole(TIMELOCK) {
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/configuration/LybraConfigurator.sol#L253

```solidity
261: function setProtocolRewardsToken(address _token) external checkRole(TIMELOCK) {
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/configuration/LybraConfigurator.sol#L261

```solidity
38: function setLockStatus(uint256 id) external {
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/esLBRBoost.sol#L38

```solidity
84: function setToken(address _lbr, address _eslbr) external onlyOwner {
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L84

```solidity
89: function setLBROracle(address _lbrOracle) external onlyOwner {
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L89

```solidity
93: function setPools(address[] memory _pools) external onlyOwner {
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L93

```solidity
100: function setBiddingCost(uint256 _biddingRatio) external onlyOwner {
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L100

```solidity
105: function setExtraRatio(uint256 ratio) external onlyOwner {
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L105

```solidity
110: function setPeUSDExtraRatio(uint256 ratio) external onlyOwner {
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L110

```solidity
115: function setBoost(address _boost) external onlyOwner {
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L115

```solidity
119: function setRewardsDuration(uint256 _duration) external onlyOwner {
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L119

```solidity
124: function setEthlbrStakeInfo(address _pool, address _lp) external onlyOwner {
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L124

```solidity
128: function setEUSDBuyoutAllowed(bool _bool) external onlyOwner {
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L128

```solidity
52: function setTokenAddress(address _eslbr, address _lbr, address _boost) external onlyOwner {
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L52

```solidity
58: function setGrabCost(uint256 _ratio) external onlyOwner {
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L58

```solidity
121: function setRewardsDuration(uint256 _duration) external onlyOwner {
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/stakerewardV2pool.sol#L121

```solidity
127: function setBoost(address _boost) external onlyOwner {
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/stakerewardV2pool.sol#L127

```solidity
41: function setRkPool(address addr) external {
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/LybraRETHVault.sol#L41

```solidity
25: function setLidoRebaseTime(uint256 _time) external {
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/LybraStETHVault.sol#L25



</details>

#### <ins>Recommended Mitigation Steps</ins>

Lack of two-step procedure for critical operations leaves them error-prone. Consider adding two step procedure on the critical functions.





### <a href="#QA Summary">[NC&#x2011;4]</a><a name="NC&#x2011;4"> Immutables can be named using the same convention

Immutables should be in uppercase per naming convention.
As shown below, some immutables are named using capital letters and underscores while some are not. For a better code quality, please consider naming these immutables using the same naming convention.

#### <ins>Proof Of Concept</ins>


<details>

```solidity
28: Iconfigurator public immutable configurator;
30: IEUSD public immutable EUSD;

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L28

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L30



```solidity
18: IEUSD public immutable EUSD;
19: IERC20 public immutable collateralAsset;
20: Iconfigurator public immutable configurator;
21: uint256 public immutable badCollateralRatio = 150 * 1e18;
22: IPriceFeed immutable etherOracle;
30: uint8 immutable vaultType = 0;

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L18

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L19

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L20

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L21

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L22

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L30



```solidity
14: IPeUSD public immutable PeUSD;
15: IERC20 public immutable collateralAsset;
16: Iconfigurator public immutable configurator;
18: uint8 immutable vaultType = 1;
19: IPriceFeed immutable etherOracle;

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L14

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L15

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L16

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L18

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L19



```solidity
14: Iconfigurator public immutable configurator;
16: uint internal immutable ld2sdRatio;

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/token/LBR.sol#L14

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/token/LBR.sol#L16



```solidity
30: IEUSD public immutable EUSD;
31: Iconfigurator public immutable configurator;
32: uint internal immutable ld2sdRatio;

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L30-L32



</details>








### <a href="#QA Summary">[NC&#x2011;5]</a><a name="NC&#x2011;5"> Initial value check is missing in Set Functions

A check regarding whether the current value and the new value are the same should be added

#### <ins>Proof Of Concept</ins>

<details>

```solidity
109: function setMintVault(address pool, bool isActive) external onlyRole(DAO) {
        mintVault[pool] = isActive;
    }
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/configuration/LybraConfigurator.sol#L109

```solidity
119: function setMintVaultMaxSupply(address pool, uint256 maxSupply) external onlyRole(DAO) {
        mintVaultMaxSupply[pool] = maxSupply;
    }
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/configuration/LybraConfigurator.sol#L119

```solidity
126: function setBadCollateralRatio(address pool, uint256 newRatio) external onlyRole(DAO) {
        require(newRatio >= 130 * 1e18 && newRatio <= 150 * 1e18 && newRatio <= vaultSafeCollateralRatio[pool] + 1e19, "LNA");
        vaultBadCollateralRatio[pool] = newRatio;
        emit SafeCollateralRatioChanged(pool, newRatio);
    }
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/configuration/LybraConfigurator.sol#L126

```solidity
137: function setProtocolRewardsPool(address addr) external checkRole(TIMELOCK) {
        lybraProtocolRewardsPool = IProtocolRewardsPool(addr);
        emit ProtocolRewardsPoolChanged(addr, block.timestamp);
    }
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/configuration/LybraConfigurator.sol#L137

```solidity
147: function setEUSDMiningIncentives(address addr) external checkRole(TIMELOCK) {
        eUSDMiningIncentives = IeUSDMiningIncentives(addr);
        emit EUSDMiningIncentivesChanged(addr, block.timestamp);
    }
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/configuration/LybraConfigurator.sol#L147

```solidity
158: function setvaultBurnPaused(address pool, bool isActive) external checkRole(TIMELOCK) {
        vaultBurnPaused[pool] = isActive;
    }
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/configuration/LybraConfigurator.sol#L158

```solidity
167: function setPremiumTradingEnabled(bool isActive) external checkRole(TIMELOCK) {
        premiumTradingEnabled = isActive;
    }
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/configuration/LybraConfigurator.sol#L167

```solidity
177: function setvaultMintPaused(address pool, bool isActive) external checkRole(ADMIN) {
        vaultMintPaused[pool] = isActive;
    }
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/configuration/LybraConfigurator.sol#L177

```solidity
186: function setRedemptionFee(uint256 newFee) external checkRole(TIMELOCK) {
        require(newFee <= 500, "Max Redemption Fee is 5%");
        redemptionFee = newFee;
        emit RedemptionFeeChanged(newFee);
    }
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/configuration/LybraConfigurator.sol#L186

```solidity
213: function setBorrowApy(address pool, uint256 newApy) external checkRole(TIMELOCK) {
        require(newApy <= 200, "Borrow APY cannot exceed 2%");
        vaultMintFeeApy[pool] = newApy;
        emit BorrowApyChanged(pool, newApy);
    }
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/configuration/LybraConfigurator.sol#L213

```solidity
224: function setKeeperRatio(address pool,uint256 newRatio) external checkRole(TIMELOCK) {
        require(newRatio <= 5, "Max Keeper reward is 5%");
        vaultKeeperRatio[pool] = newRatio;
        emit KeeperRatioChanged(pool, newRatio);
    }
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/configuration/LybraConfigurator.sol#L224

```solidity
235: function setTokenMiner(address[] calldata _contracts, bool[] calldata _bools) external checkRole(TIMELOCK) {
        for (uint256 i = 0; i < _contracts.length; i++) {
            tokenMiner[_contracts[i]] = _bools[i];
            emit tokenMinerChanges(_contracts[i], _bools[i]);
        }
    }
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/configuration/LybraConfigurator.sol#L235

```solidity
246: function setMaxStableRatio(uint256 _ratio) external checkRole(TIMELOCK) {
        require(_ratio <= 10_000, "The maximum value is 10000");
        maxStableRatio = _ratio;
    }
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/configuration/LybraConfigurator.sol#L246

```solidity
253: function setFlashloanFee(uint256 fee) external checkRole(TIMELOCK) {
        if (fee > 10_000) revert('EL');
        emit FlashloanFeeUpdated(fee);
        flashloanFee = fee;
    }
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/configuration/LybraConfigurator.sol#L253

```solidity
261: function setProtocolRewardsToken(address _token) external checkRole(TIMELOCK) {
        stableToken = _token;
    }
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/configuration/LybraConfigurator.sol#L261

```solidity
84: function setToken(address _lbr, address _eslbr) external onlyOwner {
        LBR = _lbr;
        esLBR = _eslbr;
    }
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L84

```solidity
89: function setLBROracle(address _lbrOracle) external onlyOwner {
        lbrPriceFeed = AggregatorV3Interface(_lbrOracle);
    }
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L89

```solidity
93: function setPools(address[] memory _pools) external onlyOwner {
        for (uint i = 0; i < _pools.length; i++) {
            require(configurator.mintVault(_pools[i]), "NOT_VAULT");
        }
        pools = _pools;
    }
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L93

```solidity
100: function setBiddingCost(uint256 _biddingRatio) external onlyOwner {
        require(_biddingRatio <= 8000, "BCE");
        biddingFeeRatio = _biddingRatio;
    }
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L100

```solidity
105: function setExtraRatio(uint256 ratio) external onlyOwner {
        require(ratio <= 1e20, "BCE");
        extraRatio = ratio;
    }
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L105

```solidity
110: function setPeUSDExtraRatio(uint256 ratio) external onlyOwner {
        require(ratio <= 1e20, "BCE");
        peUSDExtraRatio = ratio;
    }
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L110

```solidity
115: function setBoost(address _boost) external onlyOwner {
        esLBRBoost = IesLBRBoost(_boost);
    }
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L115

```solidity
119: function setRewardsDuration(uint256 _duration) external onlyOwner {
        require(finishAt < block.timestamp, "reward duration not finished");
        duration = _duration;
    }
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L119

```solidity
124: function setEthlbrStakeInfo(address _pool, address _lp) external onlyOwner {
        ethlbrStakePool = _pool;
        ethlbrLpToken = _lp;
    }
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L124

```solidity
128: function setEUSDBuyoutAllowed(bool _bool) external onlyOwner {
        isEUSDBuyoutAllowed = _bool;
    }
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L128

```solidity
52: function setTokenAddress(address _eslbr, address _lbr, address _boost) external onlyOwner {
        esLBR = IesLBR(_eslbr);
        LBR = IesLBR(_lbr);
        esLBRBoost = IesLBRBoost(_boost);
    }
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L52

```solidity
58: function setGrabCost(uint256 _ratio) external onlyOwner {
        require(_ratio <= 8000, "BCE");
        grabFeeRatio = _ratio;
    }
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L58

```solidity
121: function setRewardsDuration(uint256 _duration) external onlyOwner {
        require(finishAt < block.timestamp, "reward duration not finished");
        duration = _duration;
    }
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/stakerewardV2pool.sol#L121

```solidity
127: function setBoost(address _boost) external onlyOwner {
        esLBRBoost = IesLBRBoost(_boost);
    }
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/stakerewardV2pool.sol#L127

```solidity
41: function setRkPool(address addr) external {
        require(configurator.hasRole(keccak256("TIMELOCK"), msg.sender));
        rkPool = IRkPool(addr);
    }
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/LybraRETHVault.sol#L41

```solidity
25: function setLidoRebaseTime(uint256 _time) external {
        require(configurator.hasRole(keccak256("ADMIN"), msg.sender), "not authorized");
        lidoRebaseTime = _time;
    }
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/LybraStETHVault.sol#L25



</details>





### <a href="#QA Summary">[NC&#x2011;6]</a><a name="NC&#x2011;6"> Omissions in Events

Throughout the codebase, events are generally emitted when sensitive changes are made to the contracts. However, some events are missing important parameters
The following events should also add the previous original value in addition to the new value.

#### <ins>Proof Of Concept</ins>


<details>

```solidity
74: event FlashloanFeeUpdated(uint256 fee);
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/configuration/LybraConfigurator.sol#L74



</details>


#### <ins>Recommended Mitigation Steps</ins>
The events should include the new value and old value where possible.





### <a href="#QA Summary">[NC&#x2011;7]</a><a name="NC&#x2011;7"> Strings should use double quotes rather than single quotes

See the Solidity Style <a href="https://docs.soliditylang.org/en/v0.8.20/style-guide.html#other-recommendations">Guide</a>

#### <ins>Proof Of Concept</ins>

<details>

```solidity
254: if (fee > 10_000) revert('EL');

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/configuration/LybraConfigurator.sol#L254



</details>



### <a href="#QA Summary">[NC&#x2011;8]</a><a name="NC&#x2011;8"> Using underscore at the end of variable name

The use of underscore at the end of the variable name is uncommon and also suggests that the variable name was not completely changed. Consider refactoring `variableName_` to `variableName`.

#### <ins>Proof Of Concept</ins>


<details>

```solidity
39: timelock_;

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/governance/LybraGovernance.sol#L39



</details>




### <a href="#QA Summary">[NC&#x2011;9]</a><a name="NC&#x2011;9"> Large multiples of ten should use scientific notation

Use (e.g. 1e6) rather than decimal literals (e.g. 100000), for better code readability.

#### <ins>Proof Of Concept</ins>


<details>

```solidity
247: require(_ratio <= 10_000, "The maximum value is 10000");
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/configuration/LybraConfigurator.sol#L247

```solidity
189: return (stakedLBRLpValue(user) * 10000) / stakedOf(user) < 500;
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L189

```solidity
210: uint256 biddingFee = (reward * biddingFeeRatio) / 10000;
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L210

```solidity
134: LBR.burn(msg.sender, (amount * grabFeeRatio) / 10000);
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L134

```solidity
66: uint256 payAmount = (((realAmount * getAssetPrice()) / 1e18) * getDutchAuctionDiscountPrice()) / 10000;
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/LybraStETHVault.sol#L66

```solidity
103: if (time < 30 minutes) return 10000;
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/LybraStETHVault.sol#L103

```solidity
104: return 10000 - (time / 30 minutes - 1) * 100;
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/LybraStETHVault.sol#L104

```solidity
239: uint256 collateralAmount = (((eusdAmount * 1e18) / assetPrice) * (10000 - configurator.redemptionFee())) / 10000;
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L239

```solidity
301: return (poolTotalEUSDCirculation * configurator.vaultMintFeeApy(address(this)) * (block.timestamp - lastReportTime)) / (86400 * 365) / 10000;
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L301

```solidity
164: uint256 collateralAmount = (((peusdAmount * 1e18) / assetPrice) * (10000 - configurator.redemptionFee())) / 10000;
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L164

```solidity
238: return (borrowed[user] * configurator.vaultMintFeeApy(address(this)) * (block.timestamp - feeUpdatedAt[user])) / (86400 * 365) / 10000;
```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L238



</details>



### <a href="#QA Summary">[NC&#x2011;10]</a><a name="NC&#x2011;10"> Use named function calls

Code base has an extensive use of named function calls, but it somehow missed one instance where this would be appropriate.

It should use named function calls on function call, as such:
```solidity
  lido.submit{value: msg.value}({
    _configurator: address(configurator)
  });
```

#### <ins>Proof Of Concept</ins>


<details>

```solidity
32: uint256 sharesAmount = lido.submit{value: msg.value}(address(configurator));

```

https://github.com/code-423n4/2023-06-lybra/tree/main/contracts/lybra/pools/LybraWstETHVault.sol#L32



</details>

### <a href="#QA Summary">[NC&#x2011;11]</a><a name="NC&#x2011;11"> `maxSupply` should be set as a `constant`

The `maxSupply` should be set a `constant` since it is not going to change.

#### <ins>Proof Of Concept</ins>

```solidity
uint256 maxSupply = 100_000_000 * 1e18;
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/esLBR.sol#L20

```solidity
uint256 maxSupply = 100_000_000 * 1e18;
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol#L15