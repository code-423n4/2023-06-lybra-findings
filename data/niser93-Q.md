## Low Risk Issues
|       |Issue  |Instances|
|:-----:|:-----:|:-------:|
|L-01|Division by zero not prevented|9|
|L-02|Setters should have initial value check|32|
|L-03|Use of floating pragma|21|

## Not Critical Issues
|       |Issue  |Instances|
|:-----:|:-----:|:-------:|
|NC-01|Use scientific notation (e.g. ```1e18```) rather than exponentiation (e.g. ```10**18```)|13|
|NC-02|Inconsistent spacing in comments|2|
|NC-03|Typos|2|
|NC-04|Imports could be organized more systematically|11|
|NC-05|```public``` functions not called by the contract should be declared ```external``` instead|5|
|NC-06|Empty Function Body. Considering commenting why.|6|
|NC-07|Strings should use double quotes rather than single quotes|1|
|NC-08|Function always returns true (or false)|11|


### Comment
Some report titles are same of [winning bot](https://gist.github.com/liveactionllama/27513952718ec3cbcf9de0fda7fef49c), but we report instances whose are't already found.

# Low Risk Issues
## L-01
### Division by zero not prevented
#### Description
The divisions below take an input parameter which does not have any zero-value checks, which may lead to the functions reverting when zero is passed.


<details>
<summary>There are 9 instances of this issue:</summary>

```
File: EUSDMiningIncentives.sol

  
  189         return (stakedLBRLpValue(user) * 10000) / stakedOf(user) < 500;

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L189](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L189)

```
File: LybraEUSDVaultBase.sol

  
  156         uint256 onBehalfOfCollateralRatio = (depositedAsset[onBehalfOf] * assetPrice * 100) / borrowed[onBehalfOf];

  
  190         uint256 onBehalfOfCollateralRatio = (depositedAsset[onBehalfOf] * assetPrice * 100) / borrowed[onBehalfOf];

  
  236         uint256 providerCollateralRatio = (depositedAsset[provider] * assetPrice * 100) / borrowed[provider];

  
  292         if (((depositedAsset[_user] * _assetPrice * 100) / borrowed[_user]) < configurator.getSafeCollateralRatio(address(this))) revert("collateralRatio is Below safeCollateralRatio");

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L156](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L156)

```
File: LybraPeUSDVaultBase.sol

  
  127         uint256 onBehalfOfCollateralRatio = (depositedAsset[onBehalfOf] * assetPrice * 100) / getBorrowedOf(onBehalfOf);

  
  161         uint256 providerCollateralRatio = (depositedAsset[provider] * assetPrice * 100) / borrowed[provider];

  
  226         if (((depositedAsset[user] * price * 100) / getBorrowedOf(user)) < configurator.getSafeCollateralRatio(address(this))) 

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L127](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L127)

```
File: esLBRBoost.sol

  
  67             return ((boostEndTime - userUpdatedAt) * maxBoost) / (time - userUpdatedAt);

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol#L67](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol#L67)

            
</details>

## L-02
### Setters should have initial value check
#### Description
Setters should have initial value check to prevent assigning wrong value to the variable. Assginment of wrong value can lead to unexpected behavior of the contract.


<details>
<summary>There are 32 instances of this issue:</summary>

```
File: EUSDMiningIncentives.sol

  @audit: unchecked values _lbr, _eslbr
  84     function setToken(address _lbr, address _eslbr) external onlyOwner {

  @audit: unchecked values _lbrOracle
  89     function setLBROracle(address _lbrOracle) external onlyOwner {

  @audit: unchecked values _pools
  93     function setPools(address[] memory _pools) external onlyOwner {

  @audit: unchecked values _biddingRatio
  100     function setBiddingCost(uint256 _biddingRatio) external onlyOwner {

  @audit: unchecked values ratio
  105     function setExtraRatio(uint256 ratio) external onlyOwner {

  @audit: unchecked values ratio
  110     function setPeUSDExtraRatio(uint256 ratio) external onlyOwner {

  @audit: unchecked values _boost
  115     function setBoost(address _boost) external onlyOwner {

  @audit: unchecked values _duration
  119     function setRewardsDuration(uint256 _duration) external onlyOwner {

  @audit: unchecked values _pool, _lp
  124     function setEthlbrStakeInfo(address _pool, address _lp) external onlyOwner {

  @audit: unchecked values _bool
  128     function setEUSDBuyoutAllowed(bool _bool) external onlyOwner {

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L84](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L84)

```
File: LybraConfigurator.sol

  @audit: unchecked values pool, isActive
  109     function setMintVault(address pool, bool isActive) external onlyRole(DAO) {

  @audit: unchecked values pool, maxSupply
  119     function setMintVaultMaxSupply(address pool, uint256 maxSupply) external onlyRole(DAO) {

  @audit: unchecked values pool, newRatio
  126     function setBadCollateralRatio(address pool, uint256 newRatio) external onlyRole(DAO) {

  @audit: unchecked values addr
  137     function setProtocolRewardsPool(address addr) external checkRole(TIMELOCK) {

  @audit: unchecked values addr
  147     function setEUSDMiningIncentives(address addr) external checkRole(TIMELOCK) {

  @audit: unchecked values pool, isActive
  158     function setvaultBurnPaused(address pool, bool isActive) external checkRole(TIMELOCK) {

  @audit: unchecked values isActive
  167     function setPremiumTradingEnabled(bool isActive) external checkRole(TIMELOCK) {

  @audit: unchecked values pool, isActive
  177     function setvaultMintPaused(address pool, bool isActive) external checkRole(ADMIN) {

  @audit: unchecked values newFee
  186     function setRedemptionFee(uint256 newFee) external checkRole(TIMELOCK) {

  @audit: unchecked values newRatio
  198     function setSafeCollateralRatio(address pool, uint256 newRatio) external checkRole(TIMELOCK) {

  @audit: unchecked values pool, newApy
  213     function setBorrowApy(address pool, uint256 newApy) external checkRole(TIMELOCK) {

  @audit: unchecked values pool, newRatio
  224     function setKeeperRatio(address pool,uint256 newRatio) external checkRole(TIMELOCK) {

  @audit: unchecked values _contracts, _bools
  235     function setTokenMiner(address[] calldata _contracts, bool[] calldata _bools) external checkRole(TIMELOCK) {

  @audit: unchecked values _ratio
  246     function setMaxStableRatio(uint256 _ratio) external checkRole(TIMELOCK) {

  @audit: unchecked values _token
  261     function setProtocolRewardsToken(address _token) external checkRole(TIMELOCK) {

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L109](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L109)

```
File: LybraRETHVault.sol

  @audit: unchecked values addr
  41     function setRkPool(address addr) external {

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraRETHVault.sol#L41](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraRETHVault.sol#L41)

```
File: LybraStETHVault.sol

  @audit: unchecked values _time
  25     function setLidoRebaseTime(uint256 _time) external {

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L25](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L25)

```
File: ProtocolRewardsPool.sol

  @audit: unchecked values _eslbr, _lbr, _boost
  52     function setTokenAddress(address _eslbr, address _lbr, address _boost) external onlyOwner {

  @audit: unchecked values _ratio
  58     function setGrabCost(uint256 _ratio) external onlyOwner {

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L52](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L52)

```
File: esLBRBoost.sol

  @audit: unchecked values id
  38     function setLockStatus(uint256 id) external {

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol#L38](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol#L38)

```
File: stakerewardV2pool.sol

  @audit: unchecked values _duration
  121     function setRewardsDuration(uint256 _duration) external onlyOwner {

  @audit: unchecked values _boost
  127     function setBoost(address _boost) external onlyOwner {

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L121](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L121)

            
</details>

## L-03
### Use of floating pragma
#### Description
Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.[(https://swcregistry.io/docs/SWC-103)](https://swcregistry.io/docs/SWC-103)


<details>
<summary>There are 21 instances of this issue:</summary>

```
File: AdminTimelock.sol

  
  3 pragma solidity ^0.8.17;

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/AdminTimelock.sol#L3](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/AdminTimelock.sol#L3)

```
File: EUSD.sol

  
  3 pragma solidity ^0.8.17;

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L3](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L3)

```
File: EUSDMiningIncentives.sol

  
  3 pragma solidity ^0.8.17;

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L3](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L3)

```
File: GovernanceTimelock.sol

  
  3 pragma solidity ^0.8.17;

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/GovernanceTimelock.sol#L3](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/GovernanceTimelock.sol#L3)

```
File: LBR.sol

  
  3 pragma solidity ^0.8.17;

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol#L3](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol#L3)

```
File: LybraConfigurator.sol

  
  15 pragma solidity ^0.8.17;

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L15](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L15)

```
File: LybraEUSDVaultBase.sol

  
  3 pragma solidity ^0.8.17;

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L3](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L3)

```
File: LybraGovernance.sol

  
  3 pragma solidity ^0.8.17;

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol#L3](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol#L3)

```
File: LybraPeUSDVaultBase.sol

  
  3 pragma solidity ^0.8.17;

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L3](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L3)

```
File: LybraProxy.sol

  
  3 pragma solidity ^0.8.17;

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/Proxy/LybraProxy.sol#L3](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/Proxy/LybraProxy.sol#L3)

```
File: LybraProxyAdmin.sol

  
  3 pragma solidity ^0.8.17;

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/Proxy/LybraProxyAdmin.sol#L3](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/Proxy/LybraProxyAdmin.sol#L3)

```
File: LybraRETHVault.sol

  
  3 pragma solidity ^0.8.17;

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraRETHVault.sol#L3](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraRETHVault.sol#L3)

```
File: LybraStETHVault.sol

  
  3 pragma solidity ^0.8.17;

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L3](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L3)

```
File: LybraWbETHVault.sol

  
  3 pragma solidity ^0.8.17;

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWbETHVault.sol#L3](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWbETHVault.sol#L3)

```
File: LybraWstETHVault.sol

  
  3 pragma solidity ^0.8.17;

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWstETHVault.sol#L3](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWstETHVault.sol#L3)

```
File: PeUSD.sol

  
  3 pragma solidity ^0.8.17;

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSD.sol#L3](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSD.sol#L3)

```
File: PeUSDMainnetStableVision.sol

  
  14 pragma solidity ^0.8.17;

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L14](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L14)

```
File: ProtocolRewardsPool.sol

  
  3 pragma solidity ^0.8.17;

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L3](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L3)

```
File: esLBR.sol

  
  3 pragma solidity ^0.8.17;

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/esLBR.sol#L3](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/esLBR.sol#L3)

```
File: esLBRBoost.sol

  
  3 pragma solidity ^0.8.17;

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol#L3](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol#L3)

```
File: stakerewardV2pool.sol

  
  2 pragma solidity ^0.8;

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L2](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L2)

            
</details>


# Not Critical Issues
## NC-01
### Use scientific notation (e.g. ```1e18```) rather than exponentiation (e.g. ```10**18```)
#### Description
While the compiler knows to optimize away the exponentiation, it's still better coding practice to use idioms that do not require compiler optimization, if they exist


<details>
<summary>There are 13 instances of this issue:</summary>

```
File: EUSDMiningIncentives.sol

  
  189         return (stakedLBRLpValue(user) * 10000) / stakedOf(user) < 500;

  
  210             uint256 biddingFee = (reward * biddingFeeRatio) / 10000;

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L189](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L189)

```
File: LybraEUSDVaultBase.sol

  
  115         withdrawal = block.timestamp - 3 days >= depositedTime[user] ? amount : (amount * 999) / 1000;

  
  239         uint256 collateralAmount = (((eusdAmount * 1e18) / assetPrice) * (10000 - configurator.redemptionFee())) / 10000;

  
  239         uint256 collateralAmount = (((eusdAmount * 1e18) / assetPrice) * (10000 - configurator.redemptionFee())) / 10000;

  
  301         return (poolTotalEUSDCirculation * configurator.vaultMintFeeApy(address(this)) * (block.timestamp - lastReportTime)) / (86400 * 365) / 10000;

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L115](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L115)

```
File: LybraPeUSDVaultBase.sol

  
  164         uint256 collateralAmount = (((peusdAmount * 1e18) / assetPrice) * (10000 - configurator.redemptionFee())) / 10000;

  
  164         uint256 collateralAmount = (((peusdAmount * 1e18) / assetPrice) * (10000 - configurator.redemptionFee())) / 10000;

  
  238         return (borrowed[user] * configurator.vaultMintFeeApy(address(this)) * (block.timestamp - feeUpdatedAt[user])) / (86400 * 365) / 10000;

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L164](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L164)

```
File: LybraStETHVault.sol

  
  66         uint256 payAmount = (((realAmount * getAssetPrice()) / 1e18) * getDutchAuctionDiscountPrice()) / 10000;

  
  103         if (time < 30 minutes) return 10000;

  
  104         return 10000 - (time / 30 minutes - 1) * 100;

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L66](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L66)

```
File: ProtocolRewardsPool.sol

  
  134         LBR.burn(msg.sender, (amount * grabFeeRatio) / 10000);

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L134](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L134)

            
</details>

## NC-02
### Inconsistent spacing in comments
#### Description
Some lines use ```// x``` and some use ```//x```. The instances below point out the usages that don't follow the majority, within each file. Therefore, with respect [NatFormat](https://docs.soliditylang.org/en/v0.8.20/natspec-format.html), we suggest to use only one space or same indentation for NatSpec comments.


There are 2 instances of this issue:


```
File: LybraConfigurator.sol

  
  208     /**
  209      * @notice  Set the borrowing annual percentage yield (APY) for a asset pool.
  210      * @param pool The address of the pool to set the borrowing APY for.
  211      * @param newApy The new borrowing APY to set, limited to a maximum of 2%.
  212      */

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L208-L212](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L208-L212)



```
File: LybraWbETHVault.sol

  
  16     //WBETH = 0xae78736Cd615f374D3085123A210448E74Fc6393

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWbETHVault.sol#L16](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWbETHVault.sol#L16)



## NC-03
### Typos
#### Description



```
File: LybraEUSDVaultBase.sol

  @audit: liquity/liquidity
  304     /**
  305      * @dev Return USD value of current ETH through Liquity PriceFeed Contract.
  306      */

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L304-L306](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L304-L306)

```
File: LybraPeUSDVaultBase.sol

  @audit: liquity/liquidity
  241     /**
  242      * @dev Return USD value of current ETH through Liquity PriceFeed Contract.
  243      */

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L241-L243](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L241-L243)
## NC-04
### Imports could be organized more systematically
#### Description
The contract's interface should be imported first, followed by each of the interfaces it uses, followed by all other files. We report only interface reports whose should be imported first


<details>
<summary>There are 11 instances of this issue:</summary>

```
File: EUSD.sol

  
  6 import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L6](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L6)

```
File: EUSDMiningIncentives.sol

  
  14 import "../interfaces/IEUSD.sol";

  
  15 import "../interfaces/ILybra.sol";

  
  17 import "@chainlink/contracts/src/v0.8/interfaces/AggregatorV3Interface.sol";

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L14](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L14)

```
File: LybraEUSDVaultBase.sol

  
  7 import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L7](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L7)

```
File: LybraGovernance.sol

  
  7 import "../interfaces/IGovernanceTimelock.sol";

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol#L7](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol#L7)

```
File: LybraPeUSDVaultBase.sol

  
  6 import "../../interfaces/IPeUSD.sol";

  
  7 import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L6](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L6)

```
File: PeUSDMainnetStableVision.sol

  
  19 import "../interfaces/IEUSD.sol";

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L19](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L19)

```
File: ProtocolRewardsPool.sol

  
  14 import "../interfaces/IEUSD.sol";

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L14](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L14)

```
File: stakerewardV2pool.sol

  
  6 import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L6](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L6)

            
</details>

## NC-05
### ```public``` functions not called by the contract should be declared ```external``` instead
#### Description
Contracts [are allowed](https://docs.soliditylang.org/en/latest/contracts.html#function-overriding) to override their parents' functions and change the visibility from ```external``` to ```public```.


<details>
<summary>There are 5 instances of this issue:</summary>

```
File: EUSD.sol

  
  114     function decimals() public pure returns (uint8) {

  
  124     function totalSupply() public view returns (uint256) {


```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L114](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L114)

```
File: GovernanceTimelock.sol

  
  25     function checkRole(bytes32 role, address _sender) public view  returns(bool){

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/GovernanceTimelock.sol#L25](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/GovernanceTimelock.sol#L25)

```
File: LybraPeUSDVaultBase.sol

  
  44     function totalDepositedAsset() public view returns (uint256) {

  
  257     function getPoolTotalPeUSDCirculation() public view returns (uint256) {

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L44](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L44)

  
</details>

## NC-06
### Empty Function Body. Considering commenting why.
#### Description
There are functions without documentation


<details>
<summary>There are 6 instances of this issue:</summary>

```
File: AdminTimelock.sol

  
  8     constructor(uint256 minDelay, address[] memory proposers, address[] memory executors, address admin) TimelockController(minDelay, proposers, executors, admin) {}

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/AdminTimelock.sol#L8](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/AdminTimelock.sol#L8)

```
File: EUSDMiningIncentives.sol

  
  174     function refreshReward(address _account) external updateReward(_account) {}

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L174](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L174)

```
File: LybraProxy.sol

  
  9     constructor(address _logic,address _admin,bytes memory _data) TransparentUpgradeableProxy(_logic, _admin, _data) {}

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/Proxy/LybraProxy.sol#L9](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/Proxy/LybraProxy.sol#L9)

```
File: LybraStETHVault.sol

  
  18     constructor(address _config, address _stETH, address _oracle) LybraEUSDVaultBase(_stETH, _oracle, _config) {
  19     }

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L18-L19](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L18-L19)

```
File: LybraWbETHVault.sol

  
  17     constructor(address _peusd, address _oracle, address _asset, address _config)
  18         LybraPeUSDVaultBase(_peusd, _oracle, _asset, _config) {}

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWbETHVault.sol#L17-L18](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWbETHVault.sol#L17-L18)

```
File: ProtocolRewardsPool.sol

  
  184     function refreshReward(address _account) external updateReward(_account) {}

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L184](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L184)

            
</details>

## NC-07
### Strings should use double quotes rather than single quotes
#### Description
See the [Solidity Style Guide](https://docs.soliditylang.org/en/v0.8.20/style-guide.html#other-recommendations)


```
File: LybraConfigurator.sol

  
  254         if (fee > 10_000) revert('EL');

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L254](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L254)

## NC-08
### Function always returns true (or false)
#### Description
When function (which not overrides other function) returns always the same value, it could be avoid


<details>
<summary>There are 11 instances of this issue:</summary>

```
File: EUSD.sol

  
  153     function transfer(address _recipient, uint256 _amount) public returns (bool) {
  154         address owner = _msgSender();
  155         _transfer(owner, _recipient, _amount);
  156         return true;
  157     }

  
  200     function approve(address _spender, uint256 _amount) public returns (bool) {
  201         address owner = _msgSender();
  202         _approve(owner, _spender, _amount);
  203         return true;
  204     }

  
  226     function transferFrom(address from, address to, uint256 amount) public returns (bool) {
  227         address spender = _msgSender();
  228         if (!configurator.mintVault(spender)) {
  229             _spendAllowance(from, spender, amount);
  230         }
  231         _transfer(from, to, amount);
  232         return true;
  233     }

  
  248     function increaseAllowance(address _spender, uint256 _addedValue) public returns (bool) {
  249         address owner = _msgSender();
  250         _approve(owner, _spender, allowances[owner][_spender].add(_addedValue));
  251         return true;
  252     }

  
  268     function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
  269         address owner = _msgSender();
  270         uint256 currentAllowance = allowance(owner, spender);
  271         require(currentAllowance >= subtractedValue, "ERC20: decreased allowance below zero");
  272         unchecked {
  273             _approve(owner, spender, currentAllowance - subtractedValue);
  274         }
  275 
  276         return true;
  277     }

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L153-L157](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L153-L157)

```
File: LBR.sol

  
  25     function mint(address user, uint256 amount) external returns (bool) {
  26         require(configurator.tokenMiner(msg.sender), "not authorized");
  27         require(totalSupply() + amount <= maxSupply, "exceeding the maximum supply quantity.");
  28         _mint(user, amount);
  29         return true;
  30     }

  
  32     function burn(address user, uint256 amount) external returns (bool) {
  33         require(configurator.tokenMiner(msg.sender), "not authorized");
  34         _burn(user, amount);
  35         return true;
  36     }

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol#L25-L30](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol#L25-L30)

```
File: PeUSDMainnetStableVision.sol

  
  63     function mint(address to, uint256 amount) external onlyMintVault MintPaused returns (bool) {
  64         require(to != address(0), "TZA");
  65         _mint(to, amount);
  66         return true;
  67     }

  
  69     function burn(address account, uint256 amount) external onlyMintVault BurnPaused returns (bool) {
  70         _burn(account, amount);
  71         return true;
  72     }

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L63-L67](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L63-L67)

```
File: esLBR.sol

  
  30     function mint(address user, uint256 amount) external returns (bool) {
  31         require(configurator.tokenMiner(msg.sender), "not authorized");
  32         require(totalSupply() + amount <= maxSupply, "exceeding the maximum supply quantity.");
  33         try IProtocolRewardsPool(configurator.getProtocolRewardsPool()).refreshReward(user) {} catch {}
  34         _mint(user, amount);
  35         return true;
  36     }

  
  38     function burn(address user, uint256 amount) external returns (bool) {
  39         require(configurator.tokenMiner(msg.sender), "not authorized");
  40         try IProtocolRewardsPool(configurator.getProtocolRewardsPool()).refreshReward(user) {} catch {}
  41         _burn(user, amount);
  42         return true;
  43     }

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/esLBR.sol#L30-L36](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/esLBR.sol#L30-L36)

            
</details>


