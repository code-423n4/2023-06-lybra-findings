## Gas Optimizations List

| Number | Optimization Details | Instances |
|:------:|:--------------------|:---------:|
| [G-01] | Expressions for constant values such as calling `keccak256()` should use `immutable` rather than `constant` | 7 |
| [G-02] | `keccak256()` should only need to be called on a specific string literal once | 3 |
| [G-03] | Check `Require()` / `Revert()` statements that use less gas first | 1 |
| [G-04] | Hardcode address instead of using `address(this)` | 40 |
| [G-05] | Structs can be packed into fewer storage slots | 2 |
| [G-06] | Change `public` function visibility to `external` when appropriate | 32 |
| [G-07] | Avoid multiple check combinations by using nested `if` statements | 4 |
| [G-08] | Using `uint`/`int`s smaller than 32 bytes incurs overhead | 7 |


### [G-01] Expressions for constant values such as calling `keccak256()` should use `immutable` rather than `constant`
Constant values for function calls result in increased gas costs as `immutable`s are evaluated at deployment time, in contrast to `constants` which are executed at runtime for each invocation.

*There are 7 instances of this issue:*
```solidity
File: LybraConfigurator.sol
76:     bytes32 public constant DAO = keccak256("DAO");
77:     bytes32 public constant TIMELOCK = keccak256("TIMELOCK");
78:     bytes32 public constant ADMIN = keccak256("ADMIN");
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L76-L78

```solidity
File: GovernanceTimelock.sol
10:     bytes32 public constant DAO = keccak256("DAO");
11:     bytes32 public constant TIMELOCK = keccak256("TIMELOCK");
12:     bytes32 public constant ADMIN = keccak256("ADMIN");
13:     bytes32 public constant GOV = keccak256("GOV");
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/GovernanceTimelock.sol#L10-L13

### [G-02] `keccak256()` should only need to be called on a specific string literal once

It should be saved to an immutable variable, and the variable used instead.

*There are 3 instances of this issue*
```solidity
File: LybraGovernance.sol
107:         require(GovernanceTimelock.checkOnlyRole(keccak256("TIMELOCK"), msg.sender), "not authorized");
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol#L107

```solidity
File: LybraRETHVault.sol
42:         require(configurator.hasRole(keccak256("TIMELOCK"), msg.sender));
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraRETHVault.sol#L42

```solidity
File: LybraStETHVault.sol
26:         require(configurator.hasRole(keccak256("ADMIN"), msg.sender), "not authorized");
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L26



### [G-03] Check `Require()` / `Revert()` statements that use less gas first

Checks that cost less gas, such as checking function parameters, should be placed before more expensive checks such as ones that invoke `msg.sender`, because the function will be able to revert before checking more expensive statements. In this case, `require(msg.sender == address(configurator));` should be placed after the other `require(amount > 0, "amount = 0");` statement which use less gas, to prevent wasting around **24680 gas**.

*There is 1 instance of this issue:*
```solidity
File: ProtocolRewardsPool.sol
228:         require(msg.sender == address(configurator));
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L228
### [G-04] Hardcode address instead of using `address(this)`

Instead of address(this), it is more gas-efficient to pre-calculate and use the address with a hardcode. Foundry's script.sol and solmate ```LibRlp.sol``` contracts can do this.

References:
- https://book.getfoundry.sh/reference/forge-std/compute-create-address
- https://twitter.com/transmissions11/status/1518507047943245824

*There are 40 instances of this issue*


```solidity
File: GovernanceTimelock.sol
  20:         _grantRole(DAO, address(this));
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/GovernanceTimelock.sol#L20
```solidity
File: LybraEUSDVaultBase.sol
  75:         bool success = collateralAsset.transferFrom(msg.sender, address(this), assetAmount);
  160:         require(EUSD.allowance(provider, address(this)) > 0, "provider should authorize to provide liquidation EUSD");
  171:             reward2keeper = (reducedAsset * configurator.vaultKeeperRatio(address(this))) / 110;
  197:         require(EUSD.allowance(provider, address(this)) >= eusdAmount, "provider should authorize to provide liquidation EUSD");
  204:         if (msg.sender != provider && onBehalfOfCollateralRatio >= 1e20 + configurator.vaultKeeperRatio(address(this)) * 1e18) {
  205:             reward2keeper = ((assetAmount * configurator.vaultKeeperRatio(address(this))) * 1e18) / onBehalfOfCollateralRatio;
  260:         require(poolTotalEUSDCirculation + _mintAmount <= configurator.mintVaultMaxSupply(address(this)), "ESL");
  292:         if (((depositedAsset[_user] * _assetPrice * 100) / borrowed[_user]) < configurator.getSafeCollateralRatio(address(this))) revert("collateralRatio is Below safeCollateralRatio");
  301:         return (poolTotalEUSDCirculation * configurator.vaultMintFeeApy(address(this)) * (block.timestamp - lastReportTime)) / (86400 * 365) / 10000;
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L75
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L160
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L171
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L197
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L204
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L205
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L260
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L292
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L301
```solidity
File: LybraPeUSDVaultBase.sol
  45:         return collateralAsset.balanceOf(address(this));
  60:         uint256 preBalance = collateralAsset.balanceOf(address(this));
  61:         collateralAsset.transferFrom(msg.sender, address(this), assetAmount);
  62:         require(collateralAsset.balanceOf(address(this)) >= preBalance + assetAmount, "");
  128:         require(onBehalfOfCollateralRatio < configurator.getBadCollateralRatio(address(this)), "Borrowers collateral ratio should below badCollateralRatio");
  131:         require(PeUSD.allowance(provider, address(this)) > 0, "provider should authorize to provide liquidation EUSD");
  141:             reward2keeper = (reducedAsset * configurator.vaultKeeperRatio(address(this))) / 110;
  174:         require(poolTotalPeUSDCirculation + _mintAmount <= configurator.mintVaultMaxSupply(address(this)), "ESL");
  226:         if (((depositedAsset[user] * price * 100) / getBorrowedOf(user)) < configurator.getSafeCollateralRatio(address(this))) 
  238:         return (borrowed[user] * configurator.vaultMintFeeApy(address(this)) * (block.timestamp - feeUpdatedAt[user])) / (86400 * 365) / 10000;
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L45
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L60
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L61
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L128
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L131
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L141
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L174
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L226
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L238
```solidity
File: LybraRETHVault.sol
  29:         uint256 preBalance = collateralAsset.balanceOf(address(this));
  31:         uint256 balance = collateralAsset.balanceOf(address(this));
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraRETHVault.sol#L29
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraRETHVault.sol#L31
```solidity
File: LybraStETHVault.sol
  63:         uint256 excessAmount = collateralAsset.balanceOf(address(this)) - totalDepositedAsset;
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L63
```solidity
File: LybraWbETHVault.sol
  22:         uint256 preBalance = collateralAsset.balanceOf(address(this));
  24:         uint256 balance = collateralAsset.balanceOf(address(this));
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWbETHVault.sol#L22
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWbETHVault.sol#L24
```solidity
File: LBR.sol
  43:         return address(this);
  64:         if (_from != address(this) && _from != spender)
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol#L43
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol#L64
```solidity
File: PeUSD.sol
  25:         return address(this);
  46:         if (_from != address(this) && _from != spender)
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSD.sol#L25
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSD.sol#L46
```solidity
File: PeUSDMainnetStableVision.sol
  80:         require(_msgSender() == user || _msgSender() == address(this), "MDM");
  81:         require(EUSD.balanceOf(address(this)) + eusdAmount <= configurator.getEUSDMaxLocked(),"ESL");
  82:         bool success = EUSD.transferFrom(user, address(this), eusdAmount);
  133:         bool success = EUSD.transferFrom(address(receiver), address(this), EUSD.getMintedEUSDByShares(shareAmount));
  176:         return address(this);
  199:         if (_from != address(this) && _from != spender)
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L80
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L81
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L82
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L133
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L176
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L199
```solidity
File: ProtocolRewardsPool.sol
  195:             uint256 balance = EUSD.sharesOf(address(this));
  200:                 uint256 peUSDBalance = peUSD.balanceOf(address(this));
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L195
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L200
```solidity
File: stakerewardV2pool.sol
  85:         bool success = stakingToken.transferFrom(msg.sender, address(this), _amount);
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L85
```solidity
File: LybraConfigurator.sol
  290:         uint256 peUSDBalance = peUSD.balanceOf(address(this));
  295:         uint256 balance = EUSD.balanceOf(address(this));
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L290
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L295


### [G-05] Structs can be packed into fewer storage slots

The `LockStatus` struct can be packed as suggested below:

*There are 2 instances of this issue:*
```solidity
File: esLBRBoost.sol
17:     // Define a struct for the user's lock status
18:     struct LockStatus {
19:         uint256 unlockTime;
20:         uint256 duration;
21:         uint256 miningBoost;
22:     }
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/esLBRBoost.sol#L19-L20

```diff
File: esLBRBoost.sol
17:     // Define a struct for the user's lock status
18:     struct LockStatus {
-19:         uint256 unlockTime;
+19:         uint64 unlockTime;
-20:         uint256 duration;
+20:         uint64 duration;
21:         uint256 miningBoost;
22:     }
```

And if `miningBoost` is not required to be any more than `115792089237316195423570985008687907853269984665640564039457584007913129639935`, then it can be further packet into:

```diff
File: esLBRBoost.sol
17:     // Define a struct for the user's lock status
18:     struct LockStatus {
-19:         uint256 unlockTime;
+19:         uint64 unlockTime;
-20:         uint256 duration;
+20:         uint64 duration;
-21:         uint256 miningBoost;
+21:         uint128 miningBoost;
22:     }
```

And `esLBRLockSetting` can be packed into
```solidity
File: esLBRBoost.sol
11:     // Define a struct for the lock settings
12:     struct esLBRLockSetting {
13:         uint256 duration;
14:         uint256 miningBoost;
15:     }
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/esLBRBoost.sol#L13-L14

```diff
File: esLBRBoost.sol
11:     // Define a struct for the lock settings
12:     struct esLBRLockSetting {
-13:         uint256 duration;
+13:         uint64 duration;
-14:         uint256 miningBoost;
+14:         uint128 miningBoost;
15:     }
```




### [G-06] Change `public` function visibility to `external` when appropriate

Since these functions are not called from within the contract, their visibility can be made from `public` to `external`, saving gas.

*There are 32 instances of this issue:*
```solidity
File: GovernanceTimelock.sol
25:     function checkRole(bytes32 role, address _sender) public view  returns(bool){
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/GovernanceTimelock.sol#L25
```solidity
File: GovernanceTimelock.sol
29:     function checkOnlyRole(bytes32 role, address _sender) public view  returns(bool){
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/GovernanceTimelock.sol#L29
```solidity
File: LybraGovernance.sol
143:     function votingPeriod() public pure override returns (uint256){
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol#L143
```solidity
File: LybraGovernance.sol
147:      function votingDelay() public pure override returns (uint256){
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol#L147
```solidity
File: LybraGovernance.sol
151:      function CLOCK_MODE() public override view returns (string memory){
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol#L151
```solidity
File: LybraGovernance.sol
156:     function COUNTING_MODE() public override view virtual returns (string memory){
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol#L156
```solidity
File: LybraGovernance.sol
165:     function hasVoted(uint256 proposalId, address account) public override view virtual returns (bool){
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol#L165
```solidity
File: LybraGovernance.sol
172:     function proposalThreshold() public pure override returns (uint256) {
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol#L172

```solidity
File: LybraPeUSDVaultBase.sol
44:     function totalDepositedAsset() public view returns (uint256) {
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/vaults/LybraPeUSDVaultBase.sol#L44

```solidity
File: LybraPeUSDVaultBase.sol
257:     function getPoolTotalPeUSDCirculation() public view returns (uint256) {
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/vaults/LybraPeUSDVaultBase.sol#L257

```solidity
File: EUSD.sol
99:     function name() public pure returns (string memory) {
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/tokens/EUSD.sol#L99

```solidity
File: EUSD.sol
107:     function symbol() public pure returns (string memory) {
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/tokens/EUSD.sol#L107

```solidity
File: EUSD.sol
114:     function decimals() public pure returns (uint8) {
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/tokens/EUSD.sol#L114

```solidity
File: EUSD.sol
124:     function totalSupply() public view returns (uint256) {
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/tokens/EUSD.sol#L124

```solidity
File: EUSD.sol
134:     function balanceOf(address _account) public view returns (uint256) {
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/tokens/EUSD.sol#L134

```solidity
File: EUSD.sol
153:     function transfer(address _recipient, uint256 _amount) public returns (bool) {
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/tokens/EUSD.sol#L153

```solidity
File: EUSD.sol
200:     function approve(address _spender, uint256 _amount) public returns (bool) {
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/tokens/EUSD.sol#L200

```solidity
File: EUSD.sol
226:     function transferFrom(address from, address to, uint256 amount) public returns (bool) {
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/tokens/EUSD.sol#L226

```solidity
File: EUSD.sol
248:     function increaseAllowance(address _spender, uint256 _addedValue) public returns (bool) {
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/tokens/EUSD.sol#L248

```solidity
File: EUSD.sol
268:     function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/tokens/EUSD.sol#L268

```solidity
File: EUSD.sol
285:     function getTotalShares() public view returns (uint256) {
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/tokens/EUSD.sol#L285

```solidity
File: EUSD.sol
292:     function sharesOf(address _account) public view returns (uint256) {
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/tokens/EUSD.sol#L292

```solidity
File: EUSD.sol
334:     function transferShares(address _recipient, uint256 _sharesAmount) public returns (uint256) {
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/tokens/EUSD.sol#L334

```solidity
File: LBR.sol
38:     function circulatingSupply() public view virtual override returns (uint) {
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/tokens/LBR.sol#L38

```solidity
File: LBR.sol
42:     function token() public view virtual override returns (address) {
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/tokens/LBR.sol#L42

```solidity
File: PeUSD.sol
20:     function circulatingSupply() public view virtual override returns (uint) {
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/tokens/PeUSD.sol#L20

```solidity
File: PeUSD.sol
24:     function token() public view virtual override returns (address) {
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/tokens/PeUSD.sol#L24

```solidity
File: PeUSDMainnetStableVision.sol
129:     function executeFlashloan(FlashBorrower receiver, uint256 eusdAmount, bytes calldata data) public payable {
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/vision/PeUSDMainnetStableVision.sol#L129

```solidity
File: PeUSDMainnetStableVision.sol
141:     function transferFrom(address from, address to, uint256 amount) public override returns (bool) {
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/vision/PeUSDMainnetStableVision.sol#L141

```solidity
File: PeUSDMainnetStableVision.sol
165:     function getAccruedEUSDInterest(
166:         address user
167:     ) public view returns (uint256 eusdAmount) {
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/vision/PeUSDMainnetStableVision.sol#L165

```solidity
File: PeUSDMainnetStableVision.sol
171:     function circulatingSupply() public view virtual override returns (uint) {
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/vision/PeUSDMainnetStableVision.sol#L171

```solidity
File: PeUSDMainnetStableVision.sol
175:     function token() public view virtual override returns (address) {
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/vision/PeUSDMainnetStableVision.sol#L175


### [G-07] Avoid multiple check combinations by using nested `if` statements

Using nested `if` statements is cheaper than using multiple check combinations with `&&`.

*There are 4 instances of this issue:*
```solidity
File: LybraEUSDVaultBase.sol
204:         if (msg.sender != provider && onBehalfOfCollateralRatio >= 1e20 + configurator.vaultKeeperRatio(address(this)) * 1e18) {
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L204
```solidity
File: LBR.sol
64:         if (_from != address(this) && _from != spender)
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol#L64
```solidity
File: PeUSD.sol
46:         if (_from != address(this) && _from != spender)
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSD.sol#L46
```solidity
File: PeUSDMainnetStableVision.sol
199:         if (_from != address(this) && _from != spender)
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L199



### [G-08] Using `uint`/`int`s smaller than 32 bytes incurs overhead

Using elements smaller than 32 bytes can lead to increased gas usage, because the EVM processes data in 32-byte chunks. When working with smaller elements such as `uint16`, the EVM requires additional operations to adjust the size from 32 bytes to the desired size, resulting in higher gas consumption.

*There are 7 instances of this issue:*
```solidity
File: PeUSDMainnetStableVision.sol
99:         uint16 dstChainId,
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L99

```solidity
File: LybraGovernance.sol
20:         uint8 support;
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol#L20

```solidity
File: LybraEUSDVaultBase.sol
30:     uint8 immutable vaultType = 0;
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L30

```solidity
File: LybraPeUSDVaultBase.sol
18:     uint8 immutable vaultType = 1;
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L18

```solidity
File: LBR.sol
20:         uint8 decimals = decimals();
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol#L20

```solidity
File: PeUSD.sol
12:         uint8 decimals = decimals();
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSD.sol#L12

```solidity
File: PeUSDMainnetStableVision.sol
58:         uint8 decimals = decimals();
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L58
