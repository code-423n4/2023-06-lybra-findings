## Summary
Low Issues , Non Critical

| |Issue|Instances|
|-|:-|:-:|
| [Low-1](#Low-1) | Unsafe ERC20 operation(s) | 32 |
| [Low-2](#Low-2) | The `owner` is a single point of failure and a centralization risk | 16 |
| [Low-4](#Low-4) | Void constructor | 2 |
| [Low-5](#Low-5) | `Keccak` Constant values should used to `immutable` rather than `constant` | 7 |
| [Low-6](#Low-6) | Unbounded loop | 3 |
| [Low-8](#Low-8) | Use `safetransfer` Instead Of `transfer` | 20 |
| [Low-9](#Low-9) | Use `_safeMint` instead of `_mint` | 7 |
| [Low-10](#Low-10) | Division before multiplication causing significant loss of precision | 6 |
| [Low-11](#Low-11) | Use Ownable2Step's transfer function rather than Ownable's for transfers of ownership | 4 |
| [Low-12](#Low-12) | UNSAFE `decimals()` CALL FOR ASSET TOKENS THAT DO NOT IMPLEMENT `decimals()` | 2 |
| [Low-13](#Low-13) | Minting tokens to the zero address should be avoided | 19 |
| [Low-14](#Low-14) | Functions return bool true but cannot return false | 12 |
| [Low-15](#Low-15) | PREVENT DIV BY 0 | 25 |
| [Low-16](#Low-16) | AVOID TRANSFER()/SEND() AS REENTRANCY MITIGATIONS | 20 |
| [Low-17](#Low-17) | Use `safeERC20.safeApprove()` instead of `approve()` | 2 |
| [Low-18](#Low-18) | Use of immutable variables | 1 |



### **[Low-1]** Unsafe ERC20 operation(s)

ERC20 operations can be unsafe due to different implementations and vulnerabilities in the standard. It is therefore recommended to always either use OpenZeppelin's SafeERC20 library or at least to wrap each operation in a require statement.
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/LybraWstETHVault.sol
35:        lido.approve(address(collateralAsset), msg.value);
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWstETHVault.sol#L35
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol
70:            bool success = EUSD.transferFrom(msg.sender, address(configurator), income);
85:            bool success = EUSD.transferFrom(msg.sender, address(configurator), payAmount);
93:        collateralAsset.transfer(msg.sender, realAmount);
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L70
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol
85:        bool success = stakingToken.transferFrom(msg.sender, address(this), _amount);
97:        stakingToken.transfer(msg.sender, _amount);
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L85
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol
82:        bool success = EUSD.transferFrom(user, address(this), eusdAmount);
133:        bool success = EUSD.transferFrom(address(receiver), address(this), EUSD.getMintedEUSDByShares(shareAmount));
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L82
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol
202:                    peUSD.transfer(msg.sender, reward - eUSDShare);
206:                        peUSD.transfer(msg.sender, peUSDBalance);
210:                    token.transfer(msg.sender, tokenAmount);
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L202
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol
292:            peUSD.transfer(address(lybraProtocolRewardsPool), peUSDBalance);
299:                EUSD.transfer(address(lybraProtocolRewardsPool), balance);
302:                EUSD.approve(address(curvePool), balance);
304:                IEUSD(stableToken).transfer(address(lybraProtocolRewardsPool), amount);
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L292
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol
214:                bool success = EUSD.transferFrom(msg.sender, address(configurator), biddingFee);
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L214
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol
75:        bool success = collateralAsset.transferFrom(msg.sender, address(this), assetAmount);
107:        collateralAsset.transfer(onBehalfOf, withdrawal);
169:            collateralAsset.transfer(msg.sender, reducedAsset);
172:            collateralAsset.transfer(provider, reducedAsset - reward2keeper);
173:            collateralAsset.transfer(msg.sender, reward2keeper);
206:            collateralAsset.transfer(msg.sender, reward2keeper);
208:        collateralAsset.transfer(provider, assetAmount - reward2keeper);
242:        collateralAsset.transfer(msg.sender, collateralAmount);
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L75
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol
61:        collateralAsset.transferFrom(msg.sender, address(this), assetAmount);
139:            collateralAsset.transfer(msg.sender, reducedAsset);
142:            collateralAsset.transfer(provider, reducedAsset - reward2keeper);
143:            collateralAsset.transfer(msg.sender, reward2keeper);
166:        collateralAsset.transfer(msg.sender, collateralAmount);
199:            PeUSD.transferFrom(_provider, address(configurator), totalFee);
203:            PeUSD.transferFrom(_provider, address(configurator), amount);
215:        collateralAsset.transfer(_onBehalfOf, _amount);
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L61

*Instances (32)*

### **[Low-2]** The `owner` is a single point of failure and a centralization risk

Having a single EOA as the only owner of contracts is a large centralization risk and a single point of failure. A single private key may be taken in a hack, or the sole holder of the key may become unable to retrieve the key when necessary. Consider changing to a multi-signature setup, or having a role-based authorization model.
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol
33:    function addLockSetting(esLBRLockSetting memory setting) external onlyOwner {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol#L33
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol
121:    function setRewardsDuration(uint256 _duration) external onlyOwner {
127:    function setBoost(address _boost) external onlyOwner {
132:    function notifyRewardAmount(uint256 _amount) external onlyOwner updateReward(address(0)) {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L121
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol
52:    function setTokenAddress(address _eslbr, address _lbr, address _boost) external onlyOwner {
58:    function setGrabCost(uint256 _ratio) external onlyOwner {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L52
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol
84:    function setToken(address _lbr, address _eslbr) external onlyOwner {
89:    function setLBROracle(address _lbrOracle) external onlyOwner {
93:    function setPools(address[] memory _pools) external onlyOwner {
100:    function setBiddingCost(uint256 _biddingRatio) external onlyOwner {
105:    function setExtraRatio(uint256 ratio) external onlyOwner {
110:    function setPeUSDExtraRatio(uint256 ratio) external onlyOwner {
115:    function setBoost(address _boost) external onlyOwner {
119:    function setRewardsDuration(uint256 _duration) external onlyOwner {
124:    function setEthlbrStakeInfo(address _pool, address _lp) external onlyOwner {
128:    function setEUSDBuyoutAllowed(bool _bool) external onlyOwner {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L84

*Instances (16)*

### **[Low-4]** Void constructor

Detect the call to a constructor that is not implemented.
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/blob/main/contracts/lybra/Proxy/LybraProxy.sol
9:    constructor(address _logic,address _admin,bytes memory _data) TransparentUpgradeableProxy(_logic, _admin, _data) {}
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/Proxy/LybraProxy.sol#L9
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/governance/AdminTimelock.sol
8:    constructor(uint256 minDelay, address[] memory proposers, address[] memory executors, address admin) TimelockController(minDelay, proposers, executors, admin) {}
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/AdminTimelock.sol#L8

*Instances (2)*

### **[Low-5]** `Keccak` Constant values should used to `immutable` rather than `constant`

There is a difference between constant variables and immutable variables, and they should each be used in their appropriate contexts.
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/blob/main/contracts/lybra/governance/GovernanceTimelock.sol
10:    bytes32 public constant DAO = keccak256("DAO");
11:    bytes32 public constant TIMELOCK = keccak256("TIMELOCK");
12:    bytes32 public constant ADMIN = keccak256("ADMIN");
13:    bytes32 public constant GOV = keccak256("GOV");
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/GovernanceTimelock.sol#L10
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol
76:    bytes32 public constant DAO = keccak256("DAO");
77:    bytes32 public constant TIMELOCK = keccak256("TIMELOCK");
78:    bytes32 public constant ADMIN = keccak256("ADMIN");
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L76

*Instances (7)*

### **[Low-6]** Unbounded loop

While looping large collections, it's possible to run out of gas - causing a DOS condition
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol
236:        for (uint256 i = 0; i < _contracts.length; i++) {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L236
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol
94:        for (uint i = 0; i < _pools.length; i++) {
138:        for (uint i = 0; i < pools.length; i++) {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L94

*Instances (3)*

### **[Low-8]** Use `safetransfer` Instead Of `transfer`


#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol
93:        collateralAsset.transfer(msg.sender, realAmount);
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L93
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol
97:        stakingToken.transfer(msg.sender, _amount);
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L97
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol
202:                    peUSD.transfer(msg.sender, reward - eUSDShare);
206:                        peUSD.transfer(msg.sender, peUSDBalance);
210:                    token.transfer(msg.sender, tokenAmount);
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L202
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol
292:            peUSD.transfer(address(lybraProtocolRewardsPool), peUSDBalance);
299:                EUSD.transfer(address(lybraProtocolRewardsPool), balance);
304:                IEUSD(stableToken).transfer(address(lybraProtocolRewardsPool), amount);
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L292
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol
107:        collateralAsset.transfer(onBehalfOf, withdrawal);
169:            collateralAsset.transfer(msg.sender, reducedAsset);
172:            collateralAsset.transfer(provider, reducedAsset - reward2keeper);
173:            collateralAsset.transfer(msg.sender, reward2keeper);
206:            collateralAsset.transfer(msg.sender, reward2keeper);
208:        collateralAsset.transfer(provider, assetAmount - reward2keeper);
242:        collateralAsset.transfer(msg.sender, collateralAmount);
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L107
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol
139:            collateralAsset.transfer(msg.sender, reducedAsset);
142:            collateralAsset.transfer(provider, reducedAsset - reward2keeper);
143:            collateralAsset.transfer(msg.sender, reward2keeper);
166:        collateralAsset.transfer(msg.sender, collateralAmount);
215:        collateralAsset.transfer(_onBehalfOf, _amount);
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L139

*Instances (20)*

### **[Low-9]** Use `_safeMint` instead of `_mint`


#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/blob/main/contracts/lybra/token/esLBR.sol
34:        _mint(user, amount);
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/esLBR.sol#L34
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/token/PeUSD.sol
39:        _mint(_toAddress, _amount);
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSD.sol#L39
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol
28:        _mint(user, amount);
57:        _mint(_toAddress, _amount);
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol#L28
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol
65:        _mint(to, amount);
87:        _mint(user, eusdAmount);
192:        _mint(_toAddress, _amount);
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L65

*Instances (7)*

### **[Low-10]** Division before multiplication causing significant loss of precision

First divides and then multiplies again, there is a significant loss of precision
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol
66:        uint256 payAmount = (((realAmount * getAssetPrice()) / 1e18) * getDutchAuctionDiscountPrice()) / 10000;
104:        return 10000 - (time / 30 minutes - 1) * 100;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L66
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol
239:        uint256 collateralAmount = (((eusdAmount * 1e18) / assetPrice) * (10000 - configurator.redemptionFee())) / 10000;
301:        return (poolTotalEUSDCirculation * configurator.vaultMintFeeApy(address(this)) * (block.timestamp - lastReportTime)) / (86400 * 365) / 10000;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L239
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol
164:        uint256 collateralAmount = (((peusdAmount * 1e18) / assetPrice) * (10000 - configurator.redemptionFee())) / 10000;
238:        return (borrowed[user] * configurator.vaultMintFeeApy(address(this)) * (block.timestamp - feeUpdatedAt[user])) / (86400 * 365) / 10000;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L164

*Instances (6)*

### **[Low-11]** Use Ownable2Step's transfer function rather than Ownable's for transfers of ownership

First divides and then multiplies again, there is a significant loss of precision
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol
7:contract esLBRBoost is Ownable {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol#L7
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol
16:contract StakingRewardsV2 is Ownable {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L16
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol
24:contract ProtocolRewardsPool is Ownable {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L24
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol
27:contract EUSDMiningIncentives is Ownable {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L27

*Instances (4)*

### **[Low-12]** UNSAFE `decimals()` CALL FOR ASSET TOKENS THAT DO NOT IMPLEMENT `decimals()`

helper functions like [BoringCrypto's](https://github.com/boringcrypto/BoringSolidity/blob/ccb743d4c3363ca37491b87c6c9b24b1f5fa25dc/contracts/libraries/BoringERC20.sol#L52-L55) safeDecimals can be used instead of directly calling decimals()
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol
209:                    uint256 tokenAmount = (reward - eUSDShare - peUSDBalance) * token.decimals() / 1e18;
236:            rewardPerTokenStored = rewardPerTokenStored + (amount * 1e36 / token.decimals()) / totalStaked();
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L209

*Instances (2)*

### **[Low-13]** Minting tokens to the zero address should be avoided

The core function mint is used by users to mint an option position by providing token1 as collateral and borrowing the max amount of liquidity. Address(0) check is missing in both this function and the internal function _mint, which is triggered to mint the tokens to the to address. Consider applying a check in the function to ensure tokens aren't minted to the zero address.
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/LybraWbETHVault.sol
28:            _mintPeUSD(msg.sender, msg.sender, mintAmount, getAssetPrice());
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWbETHVault.sol#L28
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/token/esLBR.sol
34:        _mint(user, amount);
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/esLBR.sol#L34
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/LybraWstETHVault.sol
42:            _mintPeUSD(msg.sender, msg.sender, mintAmount, getAssetPrice());
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWstETHVault.sol#L42
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/LybraRETHVault.sol
35:            _mintPeUSD(msg.sender, msg.sender, mintAmount, getAssetPrice());
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraRETHVault.sol#L35
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/token/PeUSD.sol
39:        _mint(_toAddress, _amount);
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSD.sol#L39
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol
28:        _mint(user, amount);
57:        _mint(_toAddress, _amount);
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol#L28
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol
48:            _mintEUSD(msg.sender, msg.sender, mintAmount, getAssetPrice());
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L48
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol
65:        _mint(to, amount);
87:        _mint(user, eusdAmount);
192:        _mint(_toAddress, _amount);
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L65
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol
83:            _mintEUSD(msg.sender, msg.sender, mintAmount, getAssetPrice());
129:        _mintEUSD(msg.sender, onBehalfOf, amount, getAssetPrice());
260:        require(poolTotalEUSDCirculation + _mintAmount <= configurator.mintVaultMaxSupply(address(this)), "ESL");
268:        emit Mint(msg.sender, _onBehalfOf, _mintAmount, block.timestamp);
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L83
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol
67:            _mintPeUSD(msg.sender, msg.sender, mintAmount, assetPrice);
99:        _mintPeUSD(msg.sender, onBehalfOf, amount, getAssetPrice());
174:        require(poolTotalPeUSDCirculation + _mintAmount <= configurator.mintVaultMaxSupply(address(this)), "ESL");
184:        emit Mint(_provider, _onBehalfOf, _mintAmount, block.timestamp);
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L67

*Instances (19)*

### **[Low-14]** Functions return bool true but cannot return false


#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/blob/main/contracts/lybra/token/esLBR.sol
35:        return true;
42:        return true;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/esLBR.sol#L35
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol
29:        return true;
35:        return true;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol#L29
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol
66:        return true;
71:        return true;
147:        return true;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L66
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol
156:        return true;
203:        return true;
232:        return true;
251:        return true;
276:        return true;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L156

*Instances (12)*

### **[Low-15]** PREVENT DIV BY 0


#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol
79:        return rewardPerTokenStored + (rewardRatio * (lastTimeRewardApplicable() - updatedAt) * 1e18) / totalSupply;
134:            rewardRatio = _amount / duration;
137:            rewardRatio = (_amount + remainingRewards) / duration;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L79
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol
117:        uint256 share = (userConvertInfo[msg.sender].depositedEUSDShares * peusdAmount) / userConvertInfo[msg.sender].mintedPeUSD;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L117
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol
95:        unstakeRatio[msg.sender] = total / exitCycle;
233:            rewardPerTokenStored = rewardPerTokenStored + (share * 1e18) / totalStaked();
236:            rewardPerTokenStored = rewardPerTokenStored + (amount * 1e36 / token.decimals()) / totalStaked();
238:            rewardPerTokenStored = rewardPerTokenStored + (amount * 1e18) / totalStaked();
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L95
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol
156:        return (userStaked * (lbrInLp + etherInLp)) / totalLp;
168:        return rewardPerTokenStored + (rewardRatio * (lastTimeRewardApplicable() - updatedAt) * 1e18) / totalStaked();
189:        return (stakedLBRLpValue(user) * 10000) / stakedOf(user) < 500;
231:            rewardRatio = amount / duration;
234:            rewardRatio = (amount + remainingRewards) / duration;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L156
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol
156:        uint256 onBehalfOfCollateralRatio = (depositedAsset[onBehalfOf] * assetPrice * 100) / borrowed[onBehalfOf];
189:        require((totalDepositedAsset * assetPrice * 100) / poolTotalEUSDCirculation < badCollateralRatio, "overallCollateralRatio should below 150%");
190:        uint256 onBehalfOfCollateralRatio = (depositedAsset[onBehalfOf] * assetPrice * 100) / borrowed[onBehalfOf];
195:            eusdAmount = (eusdAmount * 1e20) / onBehalfOfCollateralRatio;
205:            reward2keeper = ((assetAmount * configurator.vaultKeeperRatio(address(this))) * 1e18) / onBehalfOfCollateralRatio;
236:        uint256 providerCollateralRatio = (depositedAsset[provider] * assetPrice * 100) / borrowed[provider];
239:        uint256 collateralAmount = (((eusdAmount * 1e18) / assetPrice) * (10000 - configurator.redemptionFee())) / 10000;
292:        if (((depositedAsset[_user] * _assetPrice * 100) / borrowed[_user]) < configurator.getSafeCollateralRatio(address(this))) revert("collateralRatio is Below safeCollateralRatio");
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L156
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol
127:        uint256 onBehalfOfCollateralRatio = (depositedAsset[onBehalfOf] * assetPrice * 100) / getBorrowedOf(onBehalfOf);
161:        uint256 providerCollateralRatio = (depositedAsset[provider] * assetPrice * 100) / borrowed[provider];
164:        uint256 collateralAmount = (((peusdAmount * 1e18) / assetPrice) * (10000 - configurator.redemptionFee())) / 10000;
226:        if (((depositedAsset[user] * price * 100) / getBorrowedOf(user)) < configurator.getSafeCollateralRatio(address(this)))
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L127

*Instances (25)*

### **[Low-16]** AVOID TRANSFER()/SEND() AS REENTRANCY MITIGATIONS

Although transfer() and send() have been recommended as a security best-practice to prevent reentrancy attacks because they only forward 2300 gas, the gas repricing of opcodes may break deployed contracts. Use call() instead, without hardcoded gas limits along with checks-effects-interactions pattern or reentrancy guards for reentrancy protection.
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol
93:        collateralAsset.transfer(msg.sender, realAmount);
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L93
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol
97:        stakingToken.transfer(msg.sender, _amount);
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L97
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol
202:                    peUSD.transfer(msg.sender, reward - eUSDShare);
206:                        peUSD.transfer(msg.sender, peUSDBalance);
210:                    token.transfer(msg.sender, tokenAmount);
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L202
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol
292:            peUSD.transfer(address(lybraProtocolRewardsPool), peUSDBalance);
299:                EUSD.transfer(address(lybraProtocolRewardsPool), balance);
304:                IEUSD(stableToken).transfer(address(lybraProtocolRewardsPool), amount);
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L292
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol
107:        collateralAsset.transfer(onBehalfOf, withdrawal);
169:            collateralAsset.transfer(msg.sender, reducedAsset);
172:            collateralAsset.transfer(provider, reducedAsset - reward2keeper);
173:            collateralAsset.transfer(msg.sender, reward2keeper);
206:            collateralAsset.transfer(msg.sender, reward2keeper);
208:        collateralAsset.transfer(provider, assetAmount - reward2keeper);
242:        collateralAsset.transfer(msg.sender, collateralAmount);
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L107
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol
139:            collateralAsset.transfer(msg.sender, reducedAsset);
142:            collateralAsset.transfer(provider, reducedAsset - reward2keeper);
143:            collateralAsset.transfer(msg.sender, reward2keeper);
166:        collateralAsset.transfer(msg.sender, collateralAmount);
215:        collateralAsset.transfer(_onBehalfOf, _amount);
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L139

*Instances (20)*

### **[Low-17]** Use `safeERC20.safeApprove()` instead of `approve()`

Note that approve() will fail for certain token implementations that do not return a boolean value . Hence it is recommend to use safeApprove().
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/LybraWstETHVault.sol
35:        lido.approve(address(collateralAsset), msg.value);
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWstETHVault.sol#L35
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol
302:                EUSD.approve(address(curvePool), balance);
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L302

*Instances (2)*

### **[Low-18]** Use of immutable variables

Immutable variables are like constants. Values of immutable variables can be set inside the constructor but cannot be modified afterwards.

#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol

18:    uint8 immutable vaultType = 1;

```
#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L18


| |Issue|Instances|
|-|:-|:-:|
| [NC-1](#NC-1) | Return values of `approve()` not checked | 2 |
| [NC-2](#NC-2) | Constants should be defined rather than using magic numbers | 7 |
| [NC-3](#NC-3) | Functions guaranteed to revert when called by normal users can be marked `payable` | 25 |
| [NC-4](#NC-4) | Floating pragma | 21 |
| [NC-5](#NC-5) | Unspecific compiler version pragma | 21 |
| [NC-6](#NC-6) | Use a more recent version of solidity | 21 |
| [NC-7](#NC-7) | Inconsistent spacing in comments | 9 |
| [NC-8](#NC-8) | `Revert` should be used on some functions instead of `return` | 1 |
| [NC-9](#NC-9) | Some number values can be refactored with `_` | 10 |
| [NC-10](#NC-10) | Compute known value `off-chain` | 7 |
| [NC-11](#NC-11) | Use a single file for all system-wide constants | 7 |
| [NC-12](#NC-12) | Skip `== true` and `== false` checks | 1 |
| [NC-13](#NC-13) | Not using the named return variables anywhere in the function is confusing | 30 |
| [NC-14](#NC-14) | Use modifier instead of require/if for special `msg.sender` | 1 |
| [NC-15](#NC-15) | Unsigned divisions can be marked as `unchecked` | 19 |
| [NC-16](#NC-16) | Uppercase immutable variables | 22 |
| [NC-17](#NC-17) | Contract name must be in CamelCase. | 2 |
| [NC-18](#NC-18) | `block.timestamp` used as time proxy | 74 |
| [NC-19](#NC-19) | Interfaces should be indicated with an I prefix in the contract name. | 2 |
| [NC-20](#NC-20) | Event name must be in CamelCase | 1 |
| [NC-21](#NC-21) | Empty Catch Block | 10 |
| [NC-22](#NC-22) | Interfaces should be defined in separate files from their usage | 18 |
| [NC-23](#NC-23) | Explicit Variable Not Used | 19 |
| [NC-24](#NC-24) | Lack Of Brace Spacing | 13 |
| [NC-25](#NC-25) | `decimals()` is an optional method | 2 |
| [NC-26](#NC-26) | Wrong value of days slightly affects precision | 1 |
| [NC-27](#NC-27) | Multiples of 10 should use scientific notation | 12 |
| [NC-28](#NC-28) | Use Solidity units when possible | 2 |
| [NC-29](#NC-29) | Consider bounding input array length | 3 |
| [NC-30](#NC-30) | point at a memory location | 2 |


### **[NC-1]** Return values of `approve()` not checked

Not all IERC20 implementations `revert()` when there's a failure in `approve()`. The function signature has a boolean return value and they indicate errors that way instead. By not checking the return value, operations that should have marked as failed, may potentially go through without actually approving anything
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/LybraWstETHVault.sol
35:        lido.approve(address(collateralAsset), msg.value);
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWstETHVault.sol#L35
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol
302:                EUSD.approve(address(curvePool), balance);
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L302
</details>
*Instances (2)*

### **[NC-2]** Constants should be defined rather than using magic numbers

Even [assembly](https://github.com/code-423n4/2022-05-opensea-seaport/blob/9d7ce4d08bf3c3010304a0476a785c70c0e90ae7/contracts/lib/TokenTransferrer.sol#L35-L39) can benefit from using readable constants instead of hex/numeric literals
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol
26:        esLBRLockSettings.push(esLBRLockSetting(30 days, 20 * 1e18));
27:        esLBRLockSettings.push(esLBRLockSetting(90 days, 30 * 1e18));
28:        esLBRLockSettings.push(esLBRLockSetting(180 days, 50 * 1e18));
29:        esLBRLockSettings.push(esLBRLockSetting(365 days, 100 * 1e18));
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol#L26
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol
152:        amount = (a * (75e18 - ((time2fullRedemption[user] - block.timestamp) * 70e18) / (exitCycle / 1 days - 3) / 1 days)) / 100e18;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L152
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol
301:        return (poolTotalEUSDCirculation * configurator.vaultMintFeeApy(address(this)) * (block.timestamp - lastReportTime)) / (86400 * 365) / 10000;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L301
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol
238:        return (borrowed[user] * configurator.vaultMintFeeApy(address(this)) * (block.timestamp - feeUpdatedAt[user])) / (86400 * 365) / 10000;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L238
</details>
*Instances (7)*

### **[NC-3]** Functions guaranteed to revert when called by normal users can be marked `payable`

If a function modifier such as `onlyOwner` is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided.
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol
33:    function addLockSetting(esLBRLockSetting memory setting) external onlyOwner {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol#L33
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol
121:    function setRewardsDuration(uint256 _duration) external onlyOwner {
127:    function setBoost(address _boost) external onlyOwner {
132:    function notifyRewardAmount(uint256 _amount) external onlyOwner updateReward(address(0)) {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L121
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol
63:    function mint(address to, uint256 amount) external onlyMintVault MintPaused returns (bool) {
69:    function burn(address account, uint256 amount) external onlyMintVault BurnPaused returns (bool) {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L63
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol
52:    function setTokenAddress(address _eslbr, address _lbr, address _boost) external onlyOwner {
58:    function setGrabCost(uint256 _ratio) external onlyOwner {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L52
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol
411:    function mint(address _recipient, uint256 _mintAmount) external onlyMintVault MintPaused returns (uint256 newTotalShares) {
440:    function burn(address _account, uint256 _burnAmount) external onlyMintVault BurnPaused returns (uint256 newTotalShares) {
459:    function burnShares(address _account, uint256 _sharesAmount) external onlyMintVault BurnPaused returns (uint256 newTotalShares) {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L411
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol
98:    function initToken(address _eusd, address _peusd) external onlyRole(DAO) {
109:    function setMintVault(address pool, bool isActive) external onlyRole(DAO) {
119:    function setMintVaultMaxSupply(address pool, uint256 maxSupply) external onlyRole(DAO) {
126:    function setBadCollateralRatio(address pool, uint256 newRatio) external onlyRole(DAO) {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L98
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol
84:    function setToken(address _lbr, address _eslbr) external onlyOwner {
89:    function setLBROracle(address _lbrOracle) external onlyOwner {
93:    function setPools(address[] memory _pools) external onlyOwner {
100:    function setBiddingCost(uint256 _biddingRatio) external onlyOwner {
105:    function setExtraRatio(uint256 ratio) external onlyOwner {
110:    function setPeUSDExtraRatio(uint256 ratio) external onlyOwner {
115:    function setBoost(address _boost) external onlyOwner {
119:    function setRewardsDuration(uint256 _duration) external onlyOwner {
124:    function setEthlbrStakeInfo(address _pool, address _lp) external onlyOwner {
128:    function setEUSDBuyoutAllowed(bool _bool) external onlyOwner {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L84
</details>
*Instances (25)*

### **[NC-4]** Floating pragma

use fixed solidity version
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/blob/main/contracts/lybra/Proxy/LybraProxyAdmin.sol
3:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/Proxy/LybraProxyAdmin.sol#L3
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/Proxy/LybraProxy.sol
3:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/Proxy/LybraProxy.sol#L3
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/governance/AdminTimelock.sol
3:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/AdminTimelock.sol#L3
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/governance/GovernanceTimelock.sol
3:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/GovernanceTimelock.sol#L3
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/LybraWbETHVault.sol
3:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWbETHVault.sol#L3
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/token/esLBR.sol
3:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/esLBR.sol#L3
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/LybraWstETHVault.sol
3:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWstETHVault.sol#L3
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/LybraRETHVault.sol
3:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraRETHVault.sol#L3
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/token/PeUSD.sol
3:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSD.sol#L3
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol
3:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol#L3
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol
3:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol#L3
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol
3:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L3
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol
2:pragma solidity ^0.8;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L2
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol
3:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol#L3
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol
14:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L14
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol
3:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L3
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol
3:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L3
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol
15:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L15
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol
3:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L3
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol
3:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L3
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol
3:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L3
</details>
*Instances (21)*

### **[NC-5]** Unspecific compiler version pragma


#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/blob/main/contracts/lybra/Proxy/LybraProxyAdmin.sol
3:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/Proxy/LybraProxyAdmin.sol#L3
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/Proxy/LybraProxy.sol
3:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/Proxy/LybraProxy.sol#L3
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/governance/AdminTimelock.sol
3:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/AdminTimelock.sol#L3
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/governance/GovernanceTimelock.sol
3:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/GovernanceTimelock.sol#L3
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/LybraWbETHVault.sol
3:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWbETHVault.sol#L3
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/token/esLBR.sol
3:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/esLBR.sol#L3
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/LybraWstETHVault.sol
3:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWstETHVault.sol#L3
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/LybraRETHVault.sol
3:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraRETHVault.sol#L3
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/token/PeUSD.sol
3:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSD.sol#L3
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol
3:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol#L3
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol
3:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol#L3
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol
3:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L3
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol
2:pragma solidity ^0.8;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L2
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol
3:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol#L3
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol
14:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L14
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol
3:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L3
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol
3:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L3
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol
15:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L15
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol
3:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L3
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol
3:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L3
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol
3:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L3
</details>
*Instances (21)*

### **[NC-6]** Use a more recent version of solidity


#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/blob/main/contracts/lybra/Proxy/LybraProxyAdmin.sol
3:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/Proxy/LybraProxyAdmin.sol#L3
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/Proxy/LybraProxy.sol
3:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/Proxy/LybraProxy.sol#L3
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/governance/AdminTimelock.sol
3:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/AdminTimelock.sol#L3
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/governance/GovernanceTimelock.sol
3:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/GovernanceTimelock.sol#L3
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/LybraWbETHVault.sol
3:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWbETHVault.sol#L3
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/token/esLBR.sol
3:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/esLBR.sol#L3
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/LybraWstETHVault.sol
3:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWstETHVault.sol#L3
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/LybraRETHVault.sol
3:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraRETHVault.sol#L3
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/token/PeUSD.sol
3:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSD.sol#L3
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol
3:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol#L3
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol
3:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol#L3
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol
3:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L3
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol
2:pragma solidity ^0.8;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L2
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol
3:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol#L3
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol
14:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L14
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol
3:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L3
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol
3:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L3
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol
15:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L15
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol
3:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L3
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol
3:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L3
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol
3:pragma solidity ^0.8.17;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L3
</details>
*Instances (21)*

### **[NC-7]** Inconsistent spacing in comments

Some lines use `// x` and some use `//x`. The instances below point out the usages that don't follow the majority, within each file
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/LybraWbETHVault.sol
16:    //WBETH = 0xae78736Cd615f374D3085123A210448E74Fc6393
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWbETHVault.sol#L16
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/LybraWstETHVault.sol
23:    //WstETH = 0x7f39C581F595B53c5cb19bD0b3f8dA6c935E2Ca0;
24:    //Lido = 0xae7ab96520DE3A18E5e111B5EaAb095312D7fE84;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWstETHVault.sol#L23
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol
39:        //convert to steth
77:                //EUSD totalSupply is 0: assume that shares correspond to EUSD 1-to-1
80:            //Income is distributed to LBR staker.
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L39
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol
43:    ///events
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L43
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol
416:            //EUSD totalSupply is 0: assume that shares correspond to EUSD 1-to-1
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L416
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol
63:    //etherOracle = 0x5f4eC3Df9cbd43714FE2740f5E3616155c5b8419
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L63
</details>
*Instances (9)*

### **[NC-12]** `Revert` should be used on some functions instead of `return`

Some instances just return without doing anything, consider applying revert statement instead with a descriptive string why it does that. 
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol
229:        if (totalStaked() == 0) return;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L229
</details>
*Instances (1)*

### **[NC-9]** Some number values can be refactored with `_`

Consider using underscores for number values to improve readability.
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol
66:        uint256 payAmount = (((realAmount * getAssetPrice()) / 1e18) * getDutchAuctionDiscountPrice()) / 10000;
103:        if (time < 30 minutes) return 10000;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L66
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol
41:    uint256 public grabFeeRatio = 3000;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L41
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol
52:    uint256 public biddingFeeRatio = 3000;
210:            uint256 biddingFee = (reward * biddingFeeRatio) / 10000;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L52
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol
115:        withdrawal = block.timestamp - 3 days >= depositedTime[user] ? amount : (amount * 999) / 1000;
239:        uint256 collateralAmount = (((eusdAmount * 1e18) / assetPrice) * (10000 - configurator.redemptionFee())) / 10000;
301:        return (poolTotalEUSDCirculation * configurator.vaultMintFeeApy(address(this)) * (block.timestamp - lastReportTime)) / (86400 * 365) / 10000;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L115
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol
164:        uint256 collateralAmount = (((peusdAmount * 1e18) / assetPrice) * (10000 - configurator.redemptionFee())) / 10000;
238:        return (borrowed[user] * configurator.vaultMintFeeApy(address(this)) * (block.timestamp - feeUpdatedAt[user])) / (86400 * 365) / 10000;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L164
</details>
*Instances (10)*

### **[NC-10]** Compute known value `off-chain`

If You know what data to hash, there is no need to consume more computational power to hash it using keccak256 , you’ll end up consuming 2x amount of gas.
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/blob/main/contracts/lybra/governance/GovernanceTimelock.sol
10:    bytes32 public constant DAO = keccak256("DAO");
11:    bytes32 public constant TIMELOCK = keccak256("TIMELOCK");
12:    bytes32 public constant ADMIN = keccak256("ADMIN");
13:    bytes32 public constant GOV = keccak256("GOV");
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/GovernanceTimelock.sol#L10
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol
76:    bytes32 public constant DAO = keccak256("DAO");
77:    bytes32 public constant TIMELOCK = keccak256("TIMELOCK");
78:    bytes32 public constant ADMIN = keccak256("ADMIN");
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L76
</details>
*Instances (7)*

### **[NC-11]** Use a single file for all system-wide constants

Consider defining in only one contract so that values cannot become out of sync when only one location is updated. A [cheap way](https://medium.com/coinmonks/gas-cost-of-solidity-library-functions-dbe0cedd4678) to store constants in a single location is to create an internal constant in a library. If the variable is a local cache of another contract’s value, consider making the cache variable internal or private, which will require external users to query the contract with the source of truth, so that callers don’t get out of sync.
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/blob/main/contracts/lybra/governance/GovernanceTimelock.sol
10:    bytes32 public constant DAO = keccak256("DAO");
11:    bytes32 public constant TIMELOCK = keccak256("TIMELOCK");
12:    bytes32 public constant ADMIN = keccak256("ADMIN");
13:    bytes32 public constant GOV = keccak256("GOV");
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/GovernanceTimelock.sol#L10
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol
76:    bytes32 public constant DAO = keccak256("DAO");
77:    bytes32 public constant TIMELOCK = keccak256("TIMELOCK");
78:    bytes32 public constant ADMIN = keccak256("ADMIN");
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L76
</details>
*Instances (7)*

### **[NC-12]** Skip `== true` and `== false` checks

It’s a tad more efficient to skip checking expressions that evaluate to booleans against `true` / `false`.
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol
82:        require(receipt.hasVoted == false, "GovernorBravo::castVoteInternal: voter already voted");
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol#L82
</details>
*Instances (1)*

### **[NC-13]** Not using the named return variables anywhere in the function is confusing

Consider changing the variable to be an unnamed one, since the variable is never assigned, nor is it returned by name. If the optimizer is not turned on, leaving the code as it is will also waste gas for the stack variable.
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/LybraWbETHVault.sol
35:        return (_etherPrice() * IWBETH(address(collateralAsset)).exchangeRatio()) / 1e18;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWbETHVault.sol#L35
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/LybraWstETHVault.sol
49:        return (_etherPrice() * IWstETH(address(collateralAsset)).stEthPerToken()) / 1e18;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWstETHVault.sol#L49
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/LybraRETHVault.sol
47:        return (_etherPrice() * IRETH(address(collateralAsset)).getExchangeRatio()) / 1e18;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraRETHVault.sol#L47
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/token/PeUSD.sol
35:        return _amount;
40:        return _amount;
49:        return _amount;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSD.sol#L35
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol
67:            return ((boostEndTime - userUpdatedAt) * maxBoost) / (time - userUpdatedAt);
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol#L67
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol
53:        return _amount;
58:        return _amount;
67:        return _amount;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol#L53
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol
108:        return _etherPrice();
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L108
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol
70:        return _min(finishAt, block.timestamp);
107:        return ((balanceOf[_account] * getBoost(_account) * (rewardPerToken() - userRewardPerTokenPaid[_account])) / 1e38) + rewards[_account];
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L70
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol
157:        return (share * configurator.flashloanFee()) / 10_000;
188:        return _amount;
193:        return _amount;
202:        return _amount;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L157
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol
168:        return ((stakedOf(_account) * (rewardPerTokenStored - userRewardPerTokenPaid[_account])) / 1e18) + rewards[_account];
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L168
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol
125:        return _totalSupply;
286:        return _totalShares;
293:        return _sharesOf(_account);
304:            return _EUSDAmount.mul(_totalShares).div(totalMintedEUSD);
315:            return _sharesAmount.mul(_totalSupply).div(_totalShares);
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L125
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol
357:        return (EUSD.totalSupply() * maxStableRatio) / 10_000;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L357
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol
156:        return (userStaked * (lbrInLp + etherInLp)) / totalLp;
160:        return _min(finishAt, block.timestamp);
185:        return ((stakedOf(_account) * getBoost(_account) * (rewardPerToken() - userRewardPerTokenPaid[_account])) / 1e38) + rewards[_account];
189:        return (stakedLBRLpValue(user) * 10000) / stakedOf(user) < 500;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L156
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol
301:        return (poolTotalEUSDCirculation * configurator.vaultMintFeeApy(address(this)) * (block.timestamp - lastReportTime)) / (86400 * 365) / 10000;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L301
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol
238:        return (borrowed[user] * configurator.vaultMintFeeApy(address(this)) * (block.timestamp - feeUpdatedAt[user])) / (86400 * 365) / 10000;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L238
</details>
*Instances (30)*

### **[NC-14]** Use modifier instead of require/if for special `msg.sender`

If functions are only allowed to be called by the special actor, modifier should be used instead of checking with require statement, if actor is the msg.sender calling the function.
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol
228:        require(msg.sender == address(configurator));
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L228
</details>
*Instances (1)*

### **[NC-15]** Unsigned divisions can be marked as `unchecked`

Divisions which do not divide by -1 cannot overflow or overflow so such operations can be unchecked to save gas
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol
66:        uint256 payAmount = (((realAmount * getAssetPrice()) / 1e18) * getDutchAuctionDiscountPrice()) / 10000;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L66
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol
117:        uint256 share = (userConvertInfo[msg.sender].depositedEUSDShares * peusdAmount) / userConvertInfo[msg.sender].mintedPeUSD;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L117
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol
209:                    uint256 tokenAmount = (reward - eUSDShare - peUSDBalance) * token.decimals() / 1e18;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L209
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol
303:                uint256 amount = curvePool.exchange_underlying(0, 2, balance, balance * price * 998 / 1e21);
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L303
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol
153:        uint256 etherInLp = (IEUSD(0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2).balanceOf(ethlbrLpToken) * uint(etherPrice)) / 1e8;
154:        uint256 lbrInLp = (IEUSD(LBR).balanceOf(ethlbrLpToken) * uint(lbrPrice)) / 1e8;
210:            uint256 biddingFee = (reward * biddingFeeRatio) / 10000;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L153
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol
156:        uint256 onBehalfOfCollateralRatio = (depositedAsset[onBehalfOf] * assetPrice * 100) / borrowed[onBehalfOf];
161:        uint256 eusdAmount = (assetAmount * assetPrice) / 1e18;
164:        uint256 reducedAsset = (assetAmount * 11) / 10;
190:        uint256 onBehalfOfCollateralRatio = (depositedAsset[onBehalfOf] * assetPrice * 100) / borrowed[onBehalfOf];
193:        uint256 eusdAmount = (assetAmount * assetPrice) / 1e18;
236:        uint256 providerCollateralRatio = (depositedAsset[provider] * assetPrice * 100) / borrowed[provider];
239:        uint256 collateralAmount = (((eusdAmount * 1e18) / assetPrice) * (10000 - configurator.redemptionFee())) / 10000;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L156
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol
127:        uint256 onBehalfOfCollateralRatio = (depositedAsset[onBehalfOf] * assetPrice * 100) / getBorrowedOf(onBehalfOf);
132:        uint256 peusdAmount = (assetAmount * assetPrice) / 1e18;
135:        uint256 reducedAsset = (assetAmount * 11) / 10;
161:        uint256 providerCollateralRatio = (depositedAsset[provider] * assetPrice * 100) / borrowed[provider];
164:        uint256 collateralAmount = (((peusdAmount * 1e18) / assetPrice) * (10000 - configurator.redemptionFee())) / 10000;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L127
</details>
*Instances (19)*

### **[NC-16]** Uppercase immutable variables

Variable does not follow [Solidity naming best pratices](https://docs.soliditylang.org/en/v0.4.24/style-guide.html#constants)
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/blob/main/contracts/lybra/token/esLBR.sol
18:    Iconfigurator public immutable configurator;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/esLBR.sol#L18
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/LybraWstETHVault.sol
22:    Ilido immutable lido;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWstETHVault.sol#L22
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/token/PeUSD.sol
9:    uint internal immutable ld2sdRatio;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSD.sol#L9
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol
14:    Iconfigurator public immutable configurator;
16:    uint internal immutable ld2sdRatio;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol#L14
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol
18:    IERC20 public immutable stakingToken;
19:    IesLBR public immutable rewardsToken;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L18
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol
31:    Iconfigurator public immutable configurator;
32:    uint internal immutable ld2sdRatio;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L31
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol
25:    Iconfigurator public immutable configurator;
39:    uint256 immutable exitCycle = 90 days;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L25
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol
39:    Iconfigurator public immutable configurator;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L39
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol
28:    Iconfigurator public immutable configurator;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L28
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol
19:    IERC20 public immutable collateralAsset;
20:    Iconfigurator public immutable configurator;
21:    uint256 public immutable badCollateralRatio = 150 * 1e18;
22:    IPriceFeed immutable etherOracle;
30:    uint8 immutable vaultType = 0;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L19
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol
15:    IERC20 public immutable collateralAsset;
16:    Iconfigurator public immutable configurator;
18:    uint8 immutable vaultType = 1;
19:    IPriceFeed immutable etherOracle;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L15
</details>
*Instances (22)*

### **[NC-17]** Contract name must be in CamelCase.

Consider renaming to camelCase
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/blob/main/contracts/lybra/token/esLBR.sol
17:contract esLBR is ERC20Votes {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/esLBR.sol#L17
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol
7:contract esLBRBoost is Ownable {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol#L7
</details>
*Instances (2)*

### **[NC-18]** block.timestamp used as time proxy

block.timestamp is not an ideal proxy for time because of issues with synchronization, miner manipulation and changing block times.
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/LybraWbETHVault.sol
31:        emit DepositEther(msg.sender, address(collateralAsset), msg.value,balance - preBalance, block.timestamp);
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWbETHVault.sol#L31
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/LybraWstETHVault.sol
45:        emit DepositEther(msg.sender, address(collateralAsset), msg.value,wstETHAmount, block.timestamp);
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWstETHVault.sol#L45
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/LybraRETHVault.sol
38:        emit DepositEther(msg.sender, address(collateralAsset), msg.value,balance - preBalance, block.timestamp);
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraRETHVault.sol#L38
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol
41:        if (userStatus.unlockTime > block.timestamp) {
44:        userLockStatus[msg.sender] = LockStatus(block.timestamp + _setting.duration, _setting.duration, _setting.miningBoost);
63:        if (finishAt <= boostEndTime || block.timestamp <= boostEndTime) {
66:            uint256 time = block.timestamp > finishAt ? finishAt : block.timestamp;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol#L41
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol
45:        depositedTime[msg.sender] = block.timestamp;
51:        emit DepositEther(msg.sender, address(collateralAsset), msg.value, msg.value, block.timestamp);
83:            emit FeeDistribution(address(configurator), income, block.timestamp);
89:            emit FeeDistribution(address(configurator), payAmount, block.timestamp);
92:        lastReportTime = block.timestamp;
94:        emit LSDValueCaptured(realAmount, payAmount, getDutchAuctionDiscountPrice(), block.timestamp);
102:        uint256 time = (block.timestamp - lidoRebaseTime) % 1 days;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L45
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol
63:            userUpdatedAt[_account] = block.timestamp;
70:        return _min(finishAt, block.timestamp);
89:        emit StakeToken(msg.sender, _amount, block.timestamp);
98:        emit WithdrawToken(msg.sender, _amount, block.timestamp);
116:            emit ClaimReward(msg.sender, reward, block.timestamp);
122:        require(finishAt < block.timestamp, "reward duration not finished");
133:        if (block.timestamp >= finishAt) {
136:            uint256 remainingRewards = (finishAt - block.timestamp) * rewardRatio;
142:        finishAt = block.timestamp + duration;
143:        updatedAt = block.timestamp;
144:        emit NotifyRewardChanged(_amount, block.timestamp);
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L63
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol
76:        emit StakeLBR(msg.sender, amount, block.timestamp);
88:        require(block.timestamp >= esLBRBoost.getUnlockTime(msg.sender), "Your lock-in period has not ended. You can't convert your esLBR now.");
92:        if (time2fullRedemption[msg.sender] > block.timestamp) {
93:            total += unstakeRatio[msg.sender] * (time2fullRedemption[msg.sender] - block.timestamp);
96:        time2fullRedemption[msg.sender] = block.timestamp + exitCycle;
97:        emit UnstakeLBR(msg.sender, amount, block.timestamp);
105:        lastWithdrawTime[user] = block.timestamp;
106:        emit WithdrawLBR(user, amount, block.timestamp);
114:        require(block.timestamp + exitCycle - 3 days > time2fullRedemption[msg.sender], "ENW");
146:        emit Restake(msg.sender, amount, block.timestamp);
152:        amount = (a * (75e18 - ((time2fullRedemption[user] - block.timestamp) * 70e18) / (exitCycle / 1 days - 3) / 1 days)) / 100e18;
157:            amount = block.timestamp > time2fullRedemption[user] ? unstakeRatio[user] * (time2fullRedemption[user] - lastWithdrawTime[user]) : unstakeRatio[user] * (block.timestamp - lastWithdrawTime[user]);
162:        if (time2fullRedemption[user] > block.timestamp) {
163:            amount = unstakeRatio[user] * (time2fullRedemption[user] - block.timestamp);
203:                    emit ClaimReward(msg.sender, EUSD.getMintedEUSDByShares(eUSDShare), address(peUSD), reward - eUSDShare, block.timestamp);
211:                    emit ClaimReward(msg.sender, EUSD.getMintedEUSDByShares(eUSDShare), address(token), reward - eUSDShare, block.timestamp);
214:                emit ClaimReward(msg.sender, EUSD.getMintedEUSDByShares(eUSDShare), address(0), 0, block.timestamp);
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L76
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol
139:        emit ProtocolRewardsPoolChanged(addr, block.timestamp);
149:        emit EUSDMiningIncentivesChanged(addr, block.timestamp);
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L139
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol
79:            userUpdatedAt[_account] = block.timestamp;
120:        require(finishAt < block.timestamp, "reward duration not finished");
160:        return _min(finishAt, block.timestamp);
198:            emit ClaimReward(msg.sender, reward, block.timestamp);
222:            emit ClaimedOtherEarnings(msg.sender, user, reward, biddingFee, useEUSD, block.timestamp);
230:        if (block.timestamp >= finishAt) {
233:            uint256 remainingRewards = (finishAt - block.timestamp) * rewardRatio;
239:        finishAt = block.timestamp + duration;
240:        updatedAt = block.timestamp;
241:        emit NotifyRewardChanged(amount, block.timestamp);
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L79
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol
80:        depositedTime[msg.sender] = block.timestamp;
85:        emit DepositAsset(msg.sender, address(collateralAsset), assetAmount, block.timestamp);
111:        emit WithdrawAsset(msg.sender, address(collateralAsset), onBehalfOf, withdrawal, block.timestamp);
115:        withdrawal = block.timestamp - 3 days >= depositedTime[user] ? amount : (amount * 999) / 1000;
175:        emit LiquidationRecord(provider, msg.sender, onBehalfOf, eusdAmount, reducedAsset, reward2keeper, false, block.timestamp);
210:        emit LiquidationRecord(provider, msg.sender, onBehalfOf, eusdAmount, assetAmount, reward2keeper, true, block.timestamp);
243:        emit RigidRedemption(msg.sender, provider, eusdAmount, collateralAmount, block.timestamp);
268:        emit Mint(msg.sender, _onBehalfOf, _mintAmount, block.timestamp);
285:        emit Burn(_provider, _onBehalfOf, amount, block.timestamp);
297:        lastReportTime = block.timestamp;
301:        return (poolTotalEUSDCirculation * configurator.vaultMintFeeApy(address(this)) * (block.timestamp - lastReportTime)) / (86400 * 365) / 10000;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L80
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol
69:        emit DepositAsset(msg.sender, address(collateralAsset), assetAmount, block.timestamp);
145:        emit LiquidationRecord(provider, msg.sender, onBehalfOf, peusdAmount, reducedAsset, reward2keeper, false, block.timestamp);
167:        emit RigidRedemption(msg.sender, provider, peusdAmount, collateralAmount, block.timestamp);
184:        emit Mint(_provider, _onBehalfOf, _mintAmount, block.timestamp);
209:        emit Burn(_provider, _onBehalfOf, amount, block.timestamp);
219:        emit WithdrawAsset(_provider, address(collateralAsset), _onBehalfOf, _amount, block.timestamp);
231:        if (block.timestamp > feeUpdatedAt[user]) {
233:            feeUpdatedAt[user] = block.timestamp;
238:        return (borrowed[user] * configurator.vaultMintFeeApy(address(this)) * (block.timestamp - feeUpdatedAt[user])) / (86400 * 365) / 10000;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L69
</details>
*Instances (74)*

### **[NC-19]** Interfaces should be indicated with an I prefix in the contract name.

Consider renaming to camelCase
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol
21:interface FlashBorrower {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L21
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol
9:interface LbrStakingPool {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L9
</details>
*Instances (2)*

### **[NC-20]** Event name must be in CamelCase

Consider renaming to camelCase
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol
70:    event tokenMinerChanges(address indexed pool, bool status);
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L70
</details>
*Instances (1)*

### **[NC-21]** Empty Catch Block

Each of the following try/catch instances has an empty catch block when making an external function call. Consider refactoring them to correspondingly emit their anticipated logged error.
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/blob/main/contracts/lybra/token/esLBR.sol
33:        try IProtocolRewardsPool(configurator.getProtocolRewardsPool()).refreshReward(user) {} catch {}
40:        try IProtocolRewardsPool(configurator.getProtocolRewardsPool()).refreshReward(user) {} catch {}
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/esLBR.sol#L33
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol
73:            try configurator.distributeRewards() {} catch {}
87:            try configurator.distributeRewards() {} catch {}
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L73
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol
216:                try configurator.distributeRewards() {} catch {}
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L216
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol
261:        try configurator.refreshMintReward(_provider) {} catch {}
280:        try configurator.refreshMintReward(_onBehalfOf) {} catch {}
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L261
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol
177:        try configurator.refreshMintReward(_provider) {} catch {}
193:        try configurator.refreshMintReward(_onBehalfOf) {} catch {}
205:        try configurator.distributeRewards() {} catch {}
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L177
</details>
*Instances (10)*

### **[NC-22]** Interfaces should be defined in separate files from their usage

The interfaces below should be defined in separate files, so that it's easier for future projects to import them, and to avoid duplication later on if they need to be used elsewhere in the project
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/LybraWbETHVault.sol
9:interface IWBETH {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWbETHVault.sol#L9
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/token/esLBR.sol
13:interface IProtocolRewardsPool {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/esLBR.sol#L13
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/LybraWstETHVault.sol
9:interface IWstETH {
15:interface Ilido {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWstETHVault.sol#L9
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/LybraRETHVault.sol
9:interface IRETH {
13:interface IRkPool {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraRETHVault.sol#L9
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol
8:interface Ilido {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L8
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol
8:interface IesLBRBoost {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L8
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol
21:interface FlashBorrower {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L21
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol
18:interface IesLBRBoost {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L18
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol
20:interface IProtocolRewardsPool {
24:interface IeUSDMiningIncentives {
28:interface IVault {
32:interface ICurvePool{
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L20
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol
19:interface IesLBRBoost {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L19
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol
9:interface LbrStakingPool {
13:interface IPriceFeed {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L9
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol
9:interface IPriceFeed {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L9
</details>
*Instances (18)*

### **[NC-23]** Explicit Variable Not Used

As described in the [Solidity documentation](https://docs.soliditylang.org/en/v0.4.21/types.html#integers): 
#### <ins>"uint and int are aliases for uint256 and int256, respectively</ins>
There are moments in the codebase where uint / int is used instead of the explicit uint256 / int256. It is best to be explicit with variable names to lessen confusion. Consider replacing instances of uint / int with uint256 / int256.".
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/blob/main/contracts/lybra/token/PeUSD.sol
9:    uint internal immutable ld2sdRatio;
31:    function _debitFrom(address _from, uint16, bytes32, uint _amount) internal virtual override returns (uint) {
38:    function _creditTo(uint16, address _toAddress, uint _amount) internal virtual override returns (uint) {
43:    function _transferFrom(address _from, address _to, uint _amount) internal virtual override returns (uint) {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSD.sol#L9
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol
16:    uint internal immutable ld2sdRatio;
49:    function _debitFrom(address _from, uint16, bytes32, uint _amount) internal virtual override returns (uint) {
56:    function _creditTo(uint16, address _toAddress, uint _amount) internal virtual override returns (uint) {
61:    function _transferFrom(address _from, address _to, uint _amount) internal virtual override returns (uint) {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol#L16
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol
32:    uint internal immutable ld2sdRatio;
184:    function _debitFrom(address _from, uint16, bytes32, uint _amount) internal virtual override returns (uint) {
191:    function _creditTo(uint16, address _toAddress, uint _amount) internal virtual override returns (uint) {
196:    function _transferFrom(address _from, address _to, uint _amount) internal virtual override returns (uint) {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L32
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol
31:    uint public rewardPerTokenStored;
191:        uint reward = rewards[msg.sender];
227:    function notifyRewardAmount(uint amount, uint tokenType) external {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L31
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol
140:            uint borrowed = pool.getBorrowedOf(user);
151:        (, int etherPrice, , , ) = etherPriceFeed.latestRoundData();
152:        (, int lbrPrice, , , ) = lbrPriceFeed.latestRoundData();
212:                (, int lbrPrice, , , ) = lbrPriceFeed.latestRoundData();
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L140
</details>
*Instances (19)*

### **[NC-24]** Lack Of Brace Spacing

There are many instances of non-spaced function / conditional braces. Consider adding a space between the function / conditional closing parenthesis and brace.
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/blob/main/contracts/lybra/governance/GovernanceTimelock.sol
25:    function checkRole(bytes32 role, address _sender) public view  returns(bool){
29:    function checkOnlyRole(bytes32 role, address _sender) public view  returns(bool){
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/GovernanceTimelock.sol#L25
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol
51:    function quorum(uint256 timepoint) public view override returns (uint256){
59:    function _quorumReached(uint256 proposalId) internal view override returns (bool){
66:    function _voteSucceeded(uint256 proposalId) internal view override returns (bool){
98:     function _getVotes(address account, uint256 timepoint, bytes memory) internal view override returns (uint256){
129:    function getReceipt(uint256 proposalId, address account) external view returns (bool voted, uint8 support, uint256 votes){
143:    function votingPeriod() public pure override returns (uint256){
147:     function votingDelay() public pure override returns (uint256){
151:     function CLOCK_MODE() public override view returns (string memory){
156:    function COUNTING_MODE() public override view virtual returns (string memory){
161:    function clock() public override view returns (uint48){
165:    function hasVoted(uint256 proposalId, address account) public override view virtual returns (bool){
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol#L51
</details>
*Instances (13)*

### **[NC-25]** decimals() is an optional method

Quoting [EIP-20](https://eips.ethereum.org/EIPS/eip-20#methods).
```OPTIONAL - This method can be used to improve usability, but interfaces and other contracts MUST NOT expect these values to be present.```
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol
209:                    uint256 tokenAmount = (reward - eUSDShare - peUSDBalance) * token.decimals() / 1e18;
236:            rewardPerTokenStored = rewardPerTokenStored + (amount * 1e36 / token.decimals()) / totalStaked();
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L209
</details>
*Instances (2)*

### **[NC-26]** Wrong value of days slightly affects precision

Formula uses a hardcoded value of 365 (days) which would be wrong applied in a leap year (366 days).
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol
29:        esLBRLockSettings.push(esLBRLockSetting(365 days, 100 * 1e18));
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol#L29
</details>
*Instances (1)*

### **[NC-27]** Multiples of 10 should use scientific notation

Using scientific notation for multiples of 10 can improve code readability.
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol
66:        uint256 payAmount = (((realAmount * getAssetPrice()) / 1e18) * getDutchAuctionDiscountPrice()) / 10000;
103:        if (time < 30 minutes) return 10000;
104:        return 10000 - (time / 30 minutes - 1) * 100;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L66
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol
134:        LBR.burn(msg.sender, (amount * grabFeeRatio) / 10000);
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L134
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol
247:        require(_ratio <= 10_000, "The maximum value is 10000");
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L247
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol
189:        return (stakedLBRLpValue(user) * 10000) / stakedOf(user) < 500;
210:            uint256 biddingFee = (reward * biddingFeeRatio) / 10000;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L189
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol
115:        withdrawal = block.timestamp - 3 days >= depositedTime[user] ? amount : (amount * 999) / 1000;
239:        uint256 collateralAmount = (((eusdAmount * 1e18) / assetPrice) * (10000 - configurator.redemptionFee())) / 10000;
301:        return (poolTotalEUSDCirculation * configurator.vaultMintFeeApy(address(this)) * (block.timestamp - lastReportTime)) / (86400 * 365) / 10000;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L115
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol
164:        uint256 collateralAmount = (((peusdAmount * 1e18) / assetPrice) * (10000 - configurator.redemptionFee())) / 10000;
238:        return (borrowed[user] * configurator.vaultMintFeeApy(address(this)) * (block.timestamp - feeUpdatedAt[user])) / (86400 * 365) / 10000;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L164
</details>
*Instances (12)*

### **[NC-28]** Use Solidity units when possible

You should consider using [Solidity units](https://docs.soliditylang.org/en/v0.8.17/units-and-global-variables.html) whenever possible to improve readability of the code..
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol
301:        return (poolTotalEUSDCirculation * configurator.vaultMintFeeApy(address(this)) * (block.timestamp - lastReportTime)) / (86400 * 365) / 10000;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L301
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol
238:        return (borrowed[user] * configurator.vaultMintFeeApy(address(this)) * (block.timestamp - feeUpdatedAt[user])) / (86400 * 365) / 10000;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L238
</details>
*Instances (2)*

### **[NC-29]** Consider bounding input array length

The functions below take in an unbounded array, and make function calls for entries in the array. While the function will revert if it eventually runs out of gas, it may be a nicer user experience to require() that the length of the array is below some reasonable maximum, so that the user doesn't have to use up a full transaction's gas only to see that the transaction reverts.
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol
236:        for (uint256 i = 0; i < _contracts.length; i++) {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L236
```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol
94:        for (uint i = 0; i < _pools.length; i++) {
138:        for (uint i = 0; i < pools.length; i++) {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L94
</details>
*Instances (3)*

### **[NC-30]** point at a memory location

During memory allocation, when you point at a memory location, There is no guarantee that the memory has not been used before and thus you cannot assume that its contents are zero bytes. There is no built-in mechanism to release or free allocated memory
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol
39:        esLBRLockSetting memory _setting = esLBRLockSettings[id];
40:        LockStatus memory userStatus = userLockStatus[msg.sender];
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol#L39
</details>
*Instances (2)*
