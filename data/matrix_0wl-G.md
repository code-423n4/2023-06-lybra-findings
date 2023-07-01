## Gas Optimizations

|        | Issue                                                                                                                                               |
| ------ | :-------------------------------------------------------------------------------------------------------------------------------------------------- |
| GAS-1  | Use assembly to check for `address(0)`                                                                                                              |
| GAS-2  | BEFORE SOME FUNCTIONS, WE SHOULD CHECK SOME VARIABLES FOR POSSIBLE GAS SAVE                                                                         |
| GAS-3  | USING CALLDATA INSTEAD OF MEMORY FOR READ-ONLY ARGUMENTS IN EXTERNAL FUNCTIONS SAVES GAS                                                            |
| GAS-4  | Setting the constructor to payable                                                                                                                  |
| GAS-5  | DON’T USE \_MSGSENDER() IF NOT SUPPORTING EIP-2771                                                                                                  |
| GAS-6  | USE FUNCTION INSTEAD OF MODIFIERS                                                                                                                   |
| GAS-7  | IT COSTS MORE GAS TO INITIALIZE NON-CONSTANT/NON-IMMUTABLE VARIABLES TO ZERO THAN TO LET THE DEFAULT OF ZERO BE APPLIED                             |
| GAS-8  | INSTEAD OF CALCULATING A STATEVAR WITH KECCAK256() EVERY TIME THE CONTRACT IS MADE PRE CALCULATE THEM BEFORE AND ONLY GIVE THE RESULT TO A CONSTANT |
| GAS-9  | INTERNAL FUNCTIONS NOT CALLED BY THE CONTRACT SHOULD BE REMOVED TO SAVE DEPLOYMENT GAS                                                              |
| GAS-10 | KECCAK256() SHOULD ONLY NEED TO BE CALLED ON A SPECIFIC STRING LITERAL ONCE                                                                         |
| GAS-11 | The increment in for loop postcondition can be made unchecked                                                                                       |
| GAS-12 | Proper data types                                                                                                                                   |
| GAS-13 | Use a more recent version of solidity                                                                                                               |
| GAS-14 | TERNARY OPERATION IS CHEAPER THAN IF-ELSE STATEMENT                                                                                                 |
| GAS-15 | USE BYTES32 INSTEAD OF STRING                                                                                                                       |
| GAS-16 | Can make the variable outside the loop to save gas                                                                                                  |

### [GAS-1] Use assembly to check for `address(0)`

#### **Proof Of Concept**

```solidity
File: contracts/lybra/configuration/LybraConfigurator.sol

99:         if (address(EUSD) == address(0)) EUSD = IEUSD(_eusd);

100:         if (address(peUSD) == address(0)) peUSD = IEUSD(_peusd);

```

```solidity
File: contracts/lybra/miner/EUSDMiningIncentives.sol

76:         if (_account != address(0)) {

```

```solidity
File: contracts/lybra/miner/stakerewardV2pool.sol

60:         if (_account != address(0)) {

```

```solidity
File: contracts/lybra/pools/base/LybraEUSDVaultBase.sol

99:         require(onBehalfOf != address(0), "TZA");

127:         require(onBehalfOf != address(0), "MINT_TO_THE_ZERO_ADDRESS");

141:         require(onBehalfOf != address(0), "BURN_TO_THE_ZERO_ADDRESS");

```

```solidity
File: contracts/lybra/pools/base/LybraPeUSDVaultBase.sol

83:         require(onBehalfOf != address(0), "TZA");

97:         require(onBehalfOf != address(0), "TZA");

111:         require(onBehalfOf != address(0), "TZA");

```

```solidity
File: contracts/lybra/token/EUSD.sol

367:         require(_owner != address(0), "APPROVE_FROM_ZERO_ADDRESS");

368:         require(_spender != address(0), "APPROVE_TO_ZERO_ADDRESS");

392:         require(_sender != address(0), "TRANSFER_FROM_THE_ZERO_ADDRESS");

393:         require(_recipient != address(0), "TRANSFER_TO_THE_ZERO_ADDRESS");

412:         require(_recipient != address(0), "MINT_TO_THE_ZERO_ADDRESS");

441:         require(_account != address(0), "BURN_FROM_THE_ZERO_ADDRESS");

460:         require(_account != address(0), "BURN_FROM_THE_ZERO_ADDRESS");

```

```solidity
File: contracts/lybra/token/PeUSDMainnetStableVision.sol

64:         require(to != address(0), "TZA");

```

### [GAS-2] BEFORE SOME FUNCTIONS, WE SHOULD CHECK SOME VARIABLES FOR POSSIBLE GAS SAVE

#### Description:

Before transfer, we should check for amount being 0 so the function doesnt run when its not gonna do anything.

#### **Proof Of Concept**

```solidity
File: contracts/lybra/configuration/LybraConfigurator.sol

292:             peUSD.transfer(address(lybraProtocolRewardsPool), peUSDBalance);

299:                 EUSD.transfer(address(lybraProtocolRewardsPool), balance);

304:                 IEUSD(stableToken).transfer(address(lybraProtocolRewardsPool), amount);

```

```solidity
File: contracts/lybra/miner/EUSDMiningIncentives.sol

214:                 bool success = EUSD.transferFrom(msg.sender, address(configurator), biddingFee);

```

```solidity
File: contracts/lybra/miner/ProtocolRewardsPool.sol

202:                     peUSD.transfer(msg.sender, reward - eUSDShare);

206:                         peUSD.transfer(msg.sender, peUSDBalance);

210:                     token.transfer(msg.sender, tokenAmount);

```

```solidity
File: contracts/lybra/miner/stakerewardV2pool.sol

85:         bool success = stakingToken.transferFrom(msg.sender, address(this), _amount);

97:         stakingToken.transfer(msg.sender, _amount);

```

```solidity
File: contracts/lybra/pools/LybraStETHVault.sol

70:             bool success = EUSD.transferFrom(msg.sender, address(configurator), income);

85:             bool success = EUSD.transferFrom(msg.sender, address(configurator), payAmount);

93:         collateralAsset.transfer(msg.sender, realAmount);

```

```solidity
File: contracts/lybra/pools/base/LybraEUSDVaultBase.sol

75:         bool success = collateralAsset.transferFrom(msg.sender, address(this), assetAmount);

107:         collateralAsset.transfer(onBehalfOf, withdrawal);

169:             collateralAsset.transfer(msg.sender, reducedAsset);

172:             collateralAsset.transfer(provider, reducedAsset - reward2keeper);

173:             collateralAsset.transfer(msg.sender, reward2keeper);

206:             collateralAsset.transfer(msg.sender, reward2keeper);

208:         collateralAsset.transfer(provider, assetAmount - reward2keeper);

242:         collateralAsset.transfer(msg.sender, collateralAmount);

```

```solidity
File: contracts/lybra/pools/base/LybraPeUSDVaultBase.sol

61:         collateralAsset.transferFrom(msg.sender, address(this), assetAmount);

139:             collateralAsset.transfer(msg.sender, reducedAsset);

142:             collateralAsset.transfer(provider, reducedAsset - reward2keeper);

143:             collateralAsset.transfer(msg.sender, reward2keeper);

166:         collateralAsset.transfer(msg.sender, collateralAmount);

199:             PeUSD.transferFrom(_provider, address(configurator), totalFee);

203:             PeUSD.transferFrom(_provider, address(configurator), amount);

215:         collateralAsset.transfer(_onBehalfOf, _amount);

```

```solidity
File: contracts/lybra/token/PeUSDMainnetStableVision.sol

82:         bool success = EUSD.transferFrom(user, address(this), eusdAmount);

133:         bool success = EUSD.transferFrom(address(receiver), address(this), EUSD.getMintedEUSDByShares(shareAmount));

```

### [GAS-3] USING CALLDATA INSTEAD OF MEMORY FOR READ-ONLY ARGUMENTS IN EXTERNAL FUNCTIONS SAVES GAS

#### Description:

Mark data types as `calldata` instead of `memory` where possible. This makes it so that the data is not automatically loaded into memory. If the data passed into the function does not need to be changed (like updating values in an array), it can be passed in as `calldata`. The one exception to this is if the argument must later be passed into another function that takes an argument that specifies `memory` storage.

[Source](https://forum.openzeppelin.com/t/a-collection-of-gas-optimisation-tricks/19966/6)
[Source 2](https://dev.to/juanxavier/advanced-gas-optimizations-tips-for-solidity-1j2f)

#### **Proof Of Concept**

```solidity
File: contracts/lybra/Proxy/LybraProxy.sol

9:     constructor(address _logic,address _admin,bytes memory _data) TransparentUpgradeableProxy(_logic, _admin, _data) {}

```

```solidity
File: contracts/lybra/governance/AdminTimelock.sol

8:     constructor(uint256 minDelay, address[] memory proposers, address[] memory executors, address admin) TimelockController(minDelay, proposers, executors, admin) {}

8:     constructor(uint256 minDelay, address[] memory proposers, address[] memory executors, address admin) TimelockController(minDelay, proposers, executors, admin) {}

```

```solidity
File: contracts/lybra/governance/GovernanceTimelock.sol

15:     constructor(uint256 minDelay, address[] memory proposers, address[] memory executors, address admin) TimelockController(minDelay, proposers, executors, admin) {

15:     constructor(uint256 minDelay, address[] memory proposers, address[] memory executors, address admin) TimelockController(minDelay, proposers, executors, admin) {

```

```solidity
File: contracts/lybra/governance/LybraGovernance.sol

38:       constructor(string memory name_, TimelockController timelock_, address _esLBR) GovernorTimelockControl(timelock_)  Governor(name_) {

```

```solidity
File: contracts/lybra/miner/EUSDMiningIncentives.sol

93:     function setPools(address[] memory _pools) external onlyOwner {

```

```solidity
File: contracts/lybra/miner/esLBRBoost.sol

33:     function addLockSetting(esLBRLockSetting memory setting) external onlyOwner {

```

### [GAS-4] Setting the constructor to payable

#### Description:

Saves ~13 gas per instance

#### **Proof Of Concept**

```solidity
File: contracts/lybra/Proxy/LybraProxy.sol

9:     constructor(address _logic,address _admin,bytes memory _data) TransparentUpgradeableProxy(_logic, _admin, _data) {}

```

```solidity
File: contracts/lybra/configuration/LybraConfigurator.sol

80:     constructor(address _dao, address _curvePool) {

```

```solidity
File: contracts/lybra/governance/AdminTimelock.sol

8:     constructor(uint256 minDelay, address[] memory proposers, address[] memory executors, address admin) TimelockController(minDelay, proposers, executors, admin) {}

```

```solidity
File: contracts/lybra/governance/GovernanceTimelock.sol

15:     constructor(uint256 minDelay, address[] memory proposers, address[] memory executors, address admin) TimelockController(minDelay, proposers, executors, admin) {

```

```solidity
File: contracts/lybra/governance/LybraGovernance.sol

38:       constructor(string memory name_, TimelockController timelock_, address _esLBR) GovernorTimelockControl(timelock_)  Governor(name_) {

```

```solidity
File: contracts/lybra/miner/EUSDMiningIncentives.sol

64:     constructor(address _config, address _boost, address _etherOracle, address _lbrOracle) {

```

```solidity
File: contracts/lybra/miner/ProtocolRewardsPool.sol

48:     constructor(address _config) {

```

```solidity
File: contracts/lybra/miner/esLBRBoost.sol

25:     constructor() {

```

```solidity
File: contracts/lybra/miner/stakerewardV2pool.sol

49:     constructor(address _stakingToken, address _rewardToken, address _boost) {

```

```solidity
File: contracts/lybra/pools/LybraRETHVault.sol

22:     constructor(address _peusd, address _config, address _rETH, address _oracle, address _rkPool)

```

```solidity
File: contracts/lybra/pools/LybraStETHVault.sol

18:     constructor(address _config, address _stETH, address _oracle) LybraEUSDVaultBase(_stETH, _oracle, _config) {

```

```solidity
File: contracts/lybra/pools/LybraWbETHVault.sol

17:     constructor(address _peusd, address _oracle, address _asset, address _config)

```

```solidity
File: contracts/lybra/pools/LybraWstETHVault.sol

25:     constructor(address _lido, address _peusd, address _oracle, address _asset, address _config) LybraPeUSDVaultBase(_peusd, _oracle, _asset,_config) {

```

```solidity
File: contracts/lybra/pools/base/LybraEUSDVaultBase.sol

46:     constructor(address _collateralAsset, address _etherOracle, address _configurator) {

```

```solidity
File: contracts/lybra/pools/base/LybraPeUSDVaultBase.sol

37:     constructor(address _peusd, address _etherOracle, address _collateral, address _configurator) {

```

```solidity
File: contracts/lybra/token/EUSD.sol

92:     constructor(address _config) {

```

```solidity
File: contracts/lybra/token/LBR.sol

18:     constructor(address _config, uint8 _sharedDecimals, address _lzEndpoint) ERC20("LBR", "LBR") BaseOFTV2(_sharedDecimals, _lzEndpoint) {

```

```solidity
File: contracts/lybra/token/PeUSD.sol

11:     constructor(uint8 _sharedDecimals, address _lzEndpoint) ERC20("peg-eUSD", "peUSD") BaseOFTV2(_sharedDecimals, _lzEndpoint) {

```

```solidity
File: contracts/lybra/token/PeUSDMainnetStableVision.sol

55:     constructor(address _config, uint8 _sharedDecimals, address _lzEndpoint) ERC20("peg-eUSD", "PeUSD") BaseOFTV2(_sharedDecimals, _lzEndpoint) {

```

```solidity
File: contracts/lybra/token/esLBR.sol

22:     constructor(address _config) ERC20Permit("esLBR") ERC20("esLBR", "esLBR") {

```

### [GAS-5] DON’T USE \_MSGSENDER() IF NOT SUPPORTING EIP-2771

#### Description:

Use msg.sender if the code does not implement EIP-2771 trusted forwarder support.

#### **Proof Of Concept**

```solidity
File: contracts/lybra/token/EUSD.sol

154:         address owner = _msgSender();

201:         address owner = _msgSender();

227:         address spender = _msgSender();

249:         address owner = _msgSender();

269:         address owner = _msgSender();

335:         address owner = _msgSender();

```

```solidity
File: contracts/lybra/token/LBR.sol

50:         address spender = _msgSender();

62:         address spender = _msgSender();

```

```solidity
File: contracts/lybra/token/PeUSD.sol

32:         address spender = _msgSender();

44:         address spender = _msgSender();

```

```solidity
File: contracts/lybra/token/PeUSDMainnetStableVision.sol

80:         require(_msgSender() == user || _msgSender() == address(this), "MDM");

80:         require(_msgSender() == user || _msgSender() == address(this), "MDM");

103:         convertToPeUSD(_msgSender(), eusdAmount);

104:         sendFrom(_msgSender(), dstChainId, toAddress, eusdAmount, callParams);

142:         address spender = _msgSender();

185:         address spender = _msgSender();

197:         address spender = _msgSender();

```

### [GAS-6] USE FUNCTION INSTEAD OF MODIFIERS

#### **Proof Of Concept**

```solidity
File: contracts/lybra/configuration/LybraConfigurator.sol

85:     modifier onlyRole(bytes32 role) {

90:     modifier checkRole(bytes32 role) {

```

```solidity
File: contracts/lybra/miner/EUSDMiningIncentives.sol

72:     modifier updateReward(address _account) {

```

```solidity
File: contracts/lybra/miner/ProtocolRewardsPool.sol

178:     modifier updateReward(address account) {

```

```solidity
File: contracts/lybra/miner/stakerewardV2pool.sol

56:     modifier updateReward(address _account) {

```

```solidity
File: contracts/lybra/token/EUSD.sol

79:     modifier onlyMintVault() {

83:     modifier MintPaused() {

87:     modifier BurnPaused() {

```

```solidity
File: contracts/lybra/token/PeUSDMainnetStableVision.sol

42:     modifier onlyMintVault() {

46:     modifier MintPaused() {

50:     modifier BurnPaused() {

```

### [GAS-7] IT COSTS MORE GAS TO INITIALIZE NON-CONSTANT/NON-IMMUTABLE VARIABLES TO ZERO THAN TO LET THE DEFAULT OF ZERO BE APPLIED

#### Description:

If a variable is not set/initialized, the default value is assumed (0, false, 0x0 … depending on the data type). You are simply wasting gas if you directly initialize it with its default value.

#### **Proof Of Concept**

```solidity
File: contracts/lybra/pools/base/LybraEUSDVaultBase.sol

30:     uint8 immutable vaultType = 0;

```

### [GAS-8] INSTEAD OF CALCULATING A STATEVAR WITH KECCAK256() EVERY TIME THE CONTRACT IS MADE PRE CALCULATE THEM BEFORE AND ONLY GIVE THE RESULT TO A CONSTANT

#### **Proof Of Concept**

```solidity
File: contracts/lybra/configuration/LybraConfigurator.sol

76:     bytes32 public constant DAO = keccak256("DAO");

77:     bytes32 public constant TIMELOCK = keccak256("TIMELOCK");

78:     bytes32 public constant ADMIN = keccak256("ADMIN");

```

```solidity
File: contracts/lybra/governance/GovernanceTimelock.sol

10:     bytes32 public constant DAO = keccak256("DAO");

11:     bytes32 public constant TIMELOCK = keccak256("TIMELOCK");

12:     bytes32 public constant ADMIN = keccak256("ADMIN");

13:     bytes32 public constant GOV = keccak256("GOV");

```

```solidity
File: contracts/lybra/governance/LybraGovernance.sol

107:         require(GovernanceTimelock.checkOnlyRole(keccak256("TIMELOCK"), msg.sender), "not authorized");

```

```solidity
File: contracts/lybra/pools/LybraRETHVault.sol

42:         require(configurator.hasRole(keccak256("TIMELOCK"), msg.sender));

```

```solidity
File: contracts/lybra/pools/LybraStETHVault.sol

26:         require(configurator.hasRole(keccak256("ADMIN"), msg.sender), "not authorized");

```

### [GAS-9] INTERNAL FUNCTIONS NOT CALLED BY THE CONTRACT SHOULD BE REMOVED TO SAVE DEPLOYMENT GAS

#### Description:

If the functions are required by an interface, the contract should inherit from that interface and use the override keyword

#### **Proof Of Concept**

```solidity
File: contracts/lybra/governance/LybraGovernance.sol

59:     function _quorumReached(uint256 proposalId) internal view override returns (bool){

66:     function _voteSucceeded(uint256 proposalId) internal view override returns (bool){

76:     function _countVote(uint256 proposalId, address account, uint8 support, uint256 weight, bytes memory) internal override {

98:      function _getVotes(address account, uint256 timepoint, bytes memory) internal view override returns (uint256){

106:     function _execute(uint256 /* proposalId */, address[] memory targets, uint256[] memory values, bytes[] memory calldatas, bytes32 descriptionHash) internal virtual override {

```

```solidity
File: contracts/lybra/miner/EUSDMiningIncentives.sol

132:     function totalStaked() internal view returns (uint256) {

```

```solidity
File: contracts/lybra/pools/base/LybraEUSDVaultBase.sol

307:     function _etherPrice() internal returns (uint256) {

```

```solidity
File: contracts/lybra/pools/base/LybraPeUSDVaultBase.sol

244:     function _etherPrice() internal returns (uint256) {

```

```solidity
File: contracts/lybra/token/EUSD.sol

177:     function _spendAllowance(address owner, address spender, uint256 amount) internal virtual {

```

```solidity
File: contracts/lybra/token/LBR.sol

49:     function _debitFrom(address _from, uint16, bytes32, uint _amount) internal virtual override returns (uint) {

56:     function _creditTo(uint16, address _toAddress, uint _amount) internal virtual override returns (uint) {

61:     function _transferFrom(address _from, address _to, uint _amount) internal virtual override returns (uint) {

70:     function _ld2sdRatio() internal view virtual override returns (uint) {

```

```solidity
File: contracts/lybra/token/PeUSD.sol

31:     function _debitFrom(address _from, uint16, bytes32, uint _amount) internal virtual override returns (uint) {

38:     function _creditTo(uint16, address _toAddress, uint _amount) internal virtual override returns (uint) {

43:     function _transferFrom(address _from, address _to, uint _amount) internal virtual override returns (uint) {

52:     function _ld2sdRatio() internal view virtual override returns (uint) {

```

```solidity
File: contracts/lybra/token/PeUSDMainnetStableVision.sol

184:     function _debitFrom(address _from, uint16, bytes32, uint _amount) internal virtual override returns (uint) {

191:     function _creditTo(uint16, address _toAddress, uint _amount) internal virtual override returns (uint) {

196:     function _transferFrom(address _from, address _to, uint _amount) internal virtual override returns (uint) {

205:     function _ld2sdRatio() internal view virtual override returns (uint) {

```

```solidity
File: contracts/lybra/token/esLBR.sol

26:     function _transfer(address, address, uint256) internal virtual override {

```

### [GAS-10] KECCAK256() SHOULD ONLY NEED TO BE CALLED ON A SPECIFIC STRING LITERAL ONCE

#### Description:

It should be saved to an immutable variable, and the variable used instead. If the hash is being used as a part of a function selector, the cast to bytes4 should also only be done once.

#### **Proof Of Concept**

```solidity
File: contracts/lybra/configuration/LybraConfigurator.sol

76:     bytes32 public constant DAO = keccak256("DAO");

77:     bytes32 public constant TIMELOCK = keccak256("TIMELOCK");

78:     bytes32 public constant ADMIN = keccak256("ADMIN");

```

```solidity
File: contracts/lybra/governance/GovernanceTimelock.sol

10:     bytes32 public constant DAO = keccak256("DAO");

11:     bytes32 public constant TIMELOCK = keccak256("TIMELOCK");

12:     bytes32 public constant ADMIN = keccak256("ADMIN");

```

```solidity
File: contracts/lybra/governance/LybraGovernance.sol

107:         require(GovernanceTimelock.checkOnlyRole(keccak256("TIMELOCK"), msg.sender), "not authorized");

```

```solidity
File: contracts/lybra/pools/LybraRETHVault.sol

42:         require(configurator.hasRole(keccak256("TIMELOCK"), msg.sender));

```

```solidity
File: contracts/lybra/pools/LybraStETHVault.sol

26:         require(configurator.hasRole(keccak256("ADMIN"), msg.sender), "not authorized");

```

### [GAS-11] The increment in for loop postcondition can be made unchecked

#### Description:

This is only relevant if you are using the default solidity checked arithmetic.
the for loop postcondition, i.e., `i++` involves checked arithmetic, which is not required. This is because the value of i is always strictly less than `length <= 2**256 - 1`. Therefore, the theoretical maximum value of i to enter the for-loop body is `2**256 - 2`. This means that the `i++` in the for loop can never overflow. Regardless, the overflow checks are performed by the compiler.

Unfortunately, the Solidity optimizer is not smart enough to detect this and remove the checks.One can manually do this.

#### **Proof Of Concept**

```solidity
File: contracts/lybra/configuration/LybraConfigurator.sol

236:         for (uint256 i = 0; i < _contracts.length; i++) {

```

```solidity
File: contracts/lybra/miner/EUSDMiningIncentives.sol

94:         for (uint i = 0; i < _pools.length; i++) {

138:         for (uint i = 0; i < pools.length; i++) {

```

### [GAS-12] Proper data types

#### Description:

In Solidity, some data types are more expensive than others. It’s important to be aware of the most efficient type that can be used. Here are a few rules about data types.

Type uint256 takes less gas to store than uint8

Type bytes should be used over byte[]

#### **Proof Of Concept**

```solidity
File: contracts/lybra/configuration/LybraConfigurator.sol

29:     function vaultType() external view returns (uint8);

33:     function get_dy_underlying(int128 i, int128 j, uint256 dx) external view returns (uint256);

34:     function exchange_underlying(int128 i, int128 j, uint256 dx, uint256 min_dy) external returns(uint256);

```

```solidity
File: contracts/lybra/governance/LybraGovernance.sol

20:         uint8 support;

76:     function _countVote(uint256 proposalId, address account, uint8 support, uint256 weight, bytes memory) internal override {

106:     function _execute(uint256 /* proposalId */, address[] memory targets, uint256[] memory values, bytes[] memory calldatas, bytes32 descriptionHash) internal virtual override {

129:     function getReceipt(uint256 proposalId, address account) external view returns (bool voted, uint8 support, uint256 votes){

161:     function clock() public override view returns (uint48){

```

```solidity
File: contracts/lybra/miner/ProtocolRewardsPool.sol

31:     uint public rewardPerTokenStored;

33:     mapping(address => uint) public userRewardPerTokenPaid;

167:     function earned(address _account) public view returns (uint) {

191:         uint reward = rewards[msg.sender];

227:     function notifyRewardAmount(uint amount, uint tokenType) external {

```

```solidity
File: contracts/lybra/pools/base/LybraEUSDVaultBase.sol

30:     uint8 immutable vaultType = 0;

323:     function getVaultType() external pure returns (uint8) {

```

```solidity
File: contracts/lybra/pools/base/LybraPeUSDVaultBase.sol

18:     uint8 immutable vaultType = 1;

265:     function getVaultType() external pure returns (uint8) {

```

```solidity
File: contracts/lybra/token/LBR.sol

16:     uint internal immutable ld2sdRatio;

18:     constructor(address _config, uint8 _sharedDecimals, address _lzEndpoint) ERC20("LBR", "LBR") BaseOFTV2(_sharedDecimals, _lzEndpoint) {

20:         uint8 decimals = decimals();

38:     function circulatingSupply() public view virtual override returns (uint) {

49:     function _debitFrom(address _from, uint16, bytes32, uint _amount) internal virtual override returns (uint) {

56:     function _creditTo(uint16, address _toAddress, uint _amount) internal virtual override returns (uint) {

61:     function _transferFrom(address _from, address _to, uint _amount) internal virtual override returns (uint) {

70:     function _ld2sdRatio() internal view virtual override returns (uint) {

```

```solidity
File: contracts/lybra/token/PeUSD.sol

9:     uint internal immutable ld2sdRatio;

11:     constructor(uint8 _sharedDecimals, address _lzEndpoint) ERC20("peg-eUSD", "peUSD") BaseOFTV2(_sharedDecimals, _lzEndpoint) {

12:         uint8 decimals = decimals();

20:     function circulatingSupply() public view virtual override returns (uint) {

31:     function _debitFrom(address _from, uint16, bytes32, uint _amount) internal virtual override returns (uint) {

38:     function _creditTo(uint16, address _toAddress, uint _amount) internal virtual override returns (uint) {

43:     function _transferFrom(address _from, address _to, uint _amount) internal virtual override returns (uint) {

52:     function _ld2sdRatio() internal view virtual override returns (uint) {

```

```solidity
File: contracts/lybra/token/PeUSDMainnetStableVision.sol

32:     uint internal immutable ld2sdRatio;

55:     constructor(address _config, uint8 _sharedDecimals, address _lzEndpoint) ERC20("peg-eUSD", "PeUSD") BaseOFTV2(_sharedDecimals, _lzEndpoint) {

58:         uint8 decimals = decimals();

99:         uint16 dstChainId,

129:     function executeFlashloan(FlashBorrower receiver, uint256 eusdAmount, bytes calldata data) public payable {

171:     function circulatingSupply() public view virtual override returns (uint) {

184:     function _debitFrom(address _from, uint16, bytes32, uint _amount) internal virtual override returns (uint) {

191:     function _creditTo(uint16, address _toAddress, uint _amount) internal virtual override returns (uint) {

196:     function _transferFrom(address _from, address _to, uint _amount) internal virtual override returns (uint) {

205:     function _ld2sdRatio() internal view virtual override returns (uint) {

```

### [GAS-13] Use a more recent version of solidity

#### **Proof Of Concept**

```solidity
File: contracts/lybra/Proxy/LybraProxy.sol

3: pragma solidity ^0.8.17;

```

```solidity
File: contracts/lybra/Proxy/LybraProxyAdmin.sol

3: pragma solidity ^0.8.17;

```

```solidity
File: contracts/lybra/configuration/LybraConfigurator.sol

15: pragma solidity ^0.8.17;

```

```solidity
File: contracts/lybra/governance/AdminTimelock.sol

3: pragma solidity ^0.8.17;

```

```solidity
File: contracts/lybra/governance/GovernanceTimelock.sol

3: pragma solidity ^0.8.17;

```

```solidity
File: contracts/lybra/governance/LybraGovernance.sol

3: pragma solidity ^0.8.17;

```

```solidity
File: contracts/lybra/miner/EUSDMiningIncentives.sol

3: pragma solidity ^0.8.17;

```

```solidity
File: contracts/lybra/miner/ProtocolRewardsPool.sol

3: pragma solidity ^0.8.17;

```

```solidity
File: contracts/lybra/miner/esLBRBoost.sol

3: pragma solidity ^0.8.17;

```

```solidity
File: contracts/lybra/miner/stakerewardV2pool.sol

2: pragma solidity ^0.8;

```

```solidity
File: contracts/lybra/pools/LybraRETHVault.sol

3: pragma solidity ^0.8.17;

```

```solidity
File: contracts/lybra/pools/LybraStETHVault.sol

3: pragma solidity ^0.8.17;

```

```solidity
File: contracts/lybra/pools/LybraWbETHVault.sol

3: pragma solidity ^0.8.17;

```

```solidity
File: contracts/lybra/pools/LybraWstETHVault.sol

3: pragma solidity ^0.8.17;

```

```solidity
File: contracts/lybra/pools/base/LybraEUSDVaultBase.sol

3: pragma solidity ^0.8.17;

```

```solidity
File: contracts/lybra/pools/base/LybraPeUSDVaultBase.sol

3: pragma solidity ^0.8.17;

```

```solidity
File: contracts/lybra/token/EUSD.sol

3: pragma solidity ^0.8.17;

```

```solidity
File: contracts/lybra/token/LBR.sol

3: pragma solidity ^0.8.17;

```

```solidity
File: contracts/lybra/token/PeUSD.sol

3: pragma solidity ^0.8.17;

```

```solidity
File: contracts/lybra/token/PeUSDMainnetStableVision.sol

14: pragma solidity ^0.8.17;

```

```solidity
File: contracts/lybra/token/esLBR.sol

3: pragma solidity ^0.8.17;

```

### [GAS-14] TERNARY OPERATION IS CHEAPER THAN IF-ELSE STATEMENT

#### Description:

There are instances where a ternary operation can be used instead of if-else statement. In these cases, using ternary operation will save modest amounts of gas.
Note that this optimization seems to be dependent on usage of a more recent Solidity version. The following gas savings are on version 0.8.17 and above.

#### **Proof Of Concept**

```solidity
File: contracts/lybra/configuration/LybraConfigurator.sol

199:    if(IVault(pool).vaultType() == 0) {
            require(newRatio >= 160 * 1e18, "eUSD vault safe collateralRatio should more than 160%");
        } else {
            require(newRatio >= vaultBadCollateralRatio[pool] + 1e19, "PeUSD vault safe collateralRatio should more than bad collateralRatio");
        }

```

```solidity
File: contracts/lybra/token/EUSD.sol

301:     if (totalMintedEUSD == 0) {
            return 0;
        } else {
            return _EUSDAmount.mul(_totalShares).div(totalMintedEUSD);
        }

312:    if (_totalShares == 0) {
            return 0;
        } else {
            return _sharesAmount.mul(_totalSupply).div(_totalShares);
        }

```

### [GAS-15] USE BYTES32 INSTEAD OF STRING

#### Description:

Use bytes32 instead of string to save gas whenever possible. String is a dynamic data structure and therefore is more gas consuming then bytes32.

#### **Proof Of Concept**

```solidity
File: contracts/lybra/governance/LybraGovernance.sol

38:       constructor(string memory name_, TimelockController timelock_, address _esLBR) GovernorTimelockControl(timelock_)  Governor(name_) {

151:      function CLOCK_MODE() public override view returns (string memory){

156:     function COUNTING_MODE() public override view virtual returns (string memory){

```

```solidity
File: contracts/lybra/token/EUSD.sol

99:     function name() public pure returns (string memory) {

107:     function symbol() public pure returns (string memory) {

```

### [GAS-16] Can make the variable outside the loop to save gas

#### Description:

Make it outside and only use it inside.

#### **Proof Of Concept**

```solidity
File: contracts/lybra/miner/EUSDMiningIncentives.sol

138:         for (uint i = 0; i < pools.length; i++) {
                ILybra pool = ILybra(pools[i]);
                uint borrowed = pool.getBorrowedOf(user);

```
