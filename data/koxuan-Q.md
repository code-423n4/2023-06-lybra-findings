# Report


## Non Critical Issues


| |Issue|Instances|
|-|:-|:-:|
| [NC-1](#NC-1) | Expressions for constant values such as a call to keccak256(), should use immutable rather than constant | 7 |
| [NC-2](#NC-2) | Constants in comparisons should appear on the left side | 31 |
| [NC-3](#NC-3) | delete keyword can be used instead of setting to 0 | 9 |
| [NC-4](#NC-4) | Lines are too long | 74 |
| [NC-5](#NC-5) |  `require()` / `revert()` statements should have descriptive reason strings | 3 |
| [NC-6](#NC-6) | Return values of `approve()` not checked | 14 |
| [NC-7](#NC-7) | Event is missing `indexed` fields | 59 |
| [NC-8](#NC-8) | Constants should be defined rather than using magic numbers | 17 |
| [NC-9](#NC-9) | Functions not used internally could be marked external | 47 |
| [NC-10](#NC-10) | Variable names don't follow the Solidity style guide | 9 |
| [NC-11](#NC-11) | Variables need not be initialized to zero | 2 |
### <a name="NC-1"></a>[NC-1] Expressions for constant values such as a call to keccak256(), should use immutable rather than constant
constants should be used for literal values written into the code, and immutable variables should be used for expressions, or values calculated in, or passed into the constructor.

*Instances (7)*:
```solidity
File: lybra/configuration/LybraConfigurator.sol

76:     bytes32 public constant DAO = keccak256("DAO");

77:     bytes32 public constant TIMELOCK = keccak256("TIMELOCK");

78:     bytes32 public constant ADMIN = keccak256("ADMIN");

```

```solidity
File: lybra/governance/GovernanceTimelock.sol

10:     bytes32 public constant DAO = keccak256("DAO");

11:     bytes32 public constant TIMELOCK = keccak256("TIMELOCK");

12:     bytes32 public constant ADMIN = keccak256("ADMIN");

13:     bytes32 public constant GOV = keccak256("GOV");

```

### <a name="NC-2"></a>[NC-2] Constants in comparisons should appear on the left side
Constants should appear on the left side of comparisons, to avoid accidental assignment

*Instances (31)*:
```solidity
File: OFT/OFTCoreV2.sol

181:             require(_adapterParams.length == 0, "OFTCore: _adapterParams must be empty.");

205:         require(_payload.toUint8(0) == PT_SEND && _payload.length == 41, "OFTCore: invalid payload");

```

```solidity
File: OFT/libraries/LzLib.sol

22:         if (_airdropParams.airdropAmount == 0 && _airdropParams.airdropAddress == bytes32(0x0)) {

48:         require(_adapterParams.length == 34 || _adapterParams.length > 66, "Invalid adapterParams");

56:         require(_adapterParams.length == 34 || _adapterParams.length > 66, "Invalid adapterParams");

61:         require(txType == 1 || txType == 2, "Unsupported txType");

61:         require(txType == 1 || txType == 2, "Unsupported txType");

64:         if (txType == 2) {

```

```solidity
File: OFT/lzApp/LzApp.sol

72:         if (payloadSizeLimit == 0) { // use default if not set

```

```solidity
File: OFT/util/BitLib.sol

19:         if(x == 0) return 0;

```

```solidity
File: lybra/configuration/LybraConfigurator.sol

199:         if(IVault(pool).vaultType() == 0) {

334:         if (vaultSafeCollateralRatio[pool] == 0) return 160 * 1e18;

339:         if(vaultBadCollateralRatio[pool] == 0) return vaultSafeCollateralRatio[pool] - 1e19;

```

```solidity
File: lybra/miner/EUSDMiningIncentives.sol

143:             if (pool.getVaultType() == 1) {

167:         if (totalStaked() == 0) {

```

```solidity
File: lybra/miner/ProtocolRewardsPool.sol

151:         if (a == 0) return 0;

229:         if (totalStaked() == 0) return;

231:         if(tokenType == 0) {

234:         } else if(tokenType == 1) {

```

```solidity
File: lybra/miner/stakerewardV2pool.sol

75:         if (totalSupply == 0) {

```

```solidity
File: lybra/pools/LybraStETHVault.sol

76:             if (sharesAmount == 0) {

```

```solidity
File: lybra/token/EUSD.sol

301:         if (totalMintedEUSD == 0) {

312:         if (_totalShares == 0) {

415:         if (sharesAmount == 0) {

```

```solidity
File: mocks/LZEndpointMock.sol

115:         require(_path.length == 40, "LayerZeroMock: incorrect remote address size"); // only support evm chains

380:         if (txType == 2) {

```

```solidity
File: mocks/mockEUSD.sol

333:         if (totalMintedEUSD == 0) {

346:         if (_totalShares == 0) {

470:         if (sharesAmount == 0) {

```

```solidity
File: mocks/stETHMock.sol

323:         if (totalMintedEUSD == 0) {

337:         if (totalShares == 0) {

```

### <a name="NC-3"></a>[NC-3] delete keyword can be used instead of setting to 0
It's clearer and reflects the intention of the programmer

*Instances (9)*:
```solidity
File: lybra/miner/EUSDMiningIncentives.sol

199:             rewards[msg.sender] = 0;

```

```solidity
File: lybra/miner/ProtocolRewardsPool.sol

120:         unstakeRatio[msg.sender] = 0;

121:         time2fullRedemption[msg.sender] = 0;

144:         unstakeRatio[msg.sender] = 0;

145:         time2fullRedemption[msg.sender] = 0;

193:             rewards[msg.sender] = 0;

```

```solidity
File: lybra/miner/stakerewardV2pool.sol

114:             rewards[msg.sender] = 0;

```

```solidity
File: mocks/LZEndpointMock.sol

230:         sp.payloadLength = 0;

304:         sp.payloadLength = 0;

```

### <a name="NC-4"></a>[NC-4] Lines are too long
Recommended by solidity docs to keep lines to 120 characters or lesser

*Instances (74)*:
```solidity
File: OFT/BaseOFTV2.sol

17:     function sendFrom(address _from, uint16 _dstChainId, bytes32 _toAddress, uint _amount, LzCallParams calldata _callParams) public payable virtual override {

18:         _send(_from, _dstChainId, _toAddress, _amount, _callParams.refundAddress, _callParams.zroPaymentAddress, _callParams.adapterParams);

21:     function sendAndCall(address _from, uint16 _dstChainId, bytes32 _toAddress, uint _amount, bytes calldata _payload, uint64 _dstGasForCall, LzCallParams calldata _callParams) public payable virtual override {

22:         _sendAndCall(_from, _dstChainId, _toAddress, _amount, _payload, _dstGasForCall, _callParams.refundAddress, _callParams.zroPaymentAddress, _callParams.adapterParams);

32:     function estimateSendFee(uint16 _dstChainId, bytes32 _toAddress, uint _amount, bool _useZro, bytes calldata _adapterParams) public view virtual override returns (uint nativeFee, uint zroFee) {

36:     function estimateSendAndCallFee(uint16 _dstChainId, bytes32 _toAddress, uint _amount, bytes calldata _payload, uint64 _dstGasForCall, bool _useZro, bytes calldata _adapterParams) public view virtual override returns (uint nativeFee, uint zroFee) {

```

```solidity
File: OFT/ICommonOFT.sol

26:     function estimateSendFee(uint16 _dstChainId, bytes32 _toAddress, uint _amount, bool _useZro, bytes calldata _adapterParams) external view returns (uint nativeFee, uint zroFee);

28:     function estimateSendAndCallFee(uint16 _dstChainId, bytes32 _toAddress, uint _amount, bytes calldata _payload, uint64 _dstGasForCall, bool _useZro, bytes calldata _adapterParams) external view returns (uint nativeFee, uint zroFee);

```

```solidity
File: OFT/IOFTReceiverV2.sol

15:     function onOFTReceived(uint16 _srcChainId, bytes calldata _srcAddress, uint64 _nonce, bytes32 _from, uint _amount, bytes calldata _payload) external;

```

```solidity
File: OFT/IOFTV2.sol

22:     function sendFrom(address _from, uint16 _dstChainId, bytes32 _toAddress, uint _amount, LzCallParams calldata _callParams) external payable;

24:     function sendAndCall(address _from, uint16 _dstChainId, bytes32 _toAddress, uint _amount, bytes calldata _payload, uint64 _dstGasForCall, LzCallParams calldata _callParams) external payable;

```

```solidity
File: OFT/OFTCoreV2.sol

51:     function callOnOFTReceived(uint16 _srcChainId, bytes calldata _srcAddress, uint64 _nonce, bytes32 _from, address _to, uint _amount, bytes calldata _payload, uint _gasForCall) public virtual {

70:     function _estimateSendFee(uint16 _dstChainId, bytes32 _toAddress, uint _amount, bool _useZro, bytes memory _adapterParams) internal view virtual returns (uint nativeFee, uint zroFee) {

76:     function _estimateSendAndCallFee(uint16 _dstChainId, bytes32 _toAddress, uint _amount, bytes memory _payload, uint64 _dstGasForCall, bool _useZro, bytes memory _adapterParams) internal view virtual returns (uint nativeFee, uint zroFee) {

82:     function _nonblockingLzReceive(uint16 _srcChainId, bytes memory _srcAddress, uint64 _nonce, bytes memory _payload) internal virtual override {

94:     function _send(address _from, uint16 _dstChainId, bytes32 _toAddress, uint _amount, address payable _refundAddress, address _zroPaymentAddress, bytes memory _adapterParams) internal virtual returns (uint amount) {

119:     function _sendAndCall(address _from, uint16 _dstChainId, bytes32 _toAddress, uint _amount, bytes memory _payload, uint64 _dstGasForCall, address payable _refundAddress, address _zroPaymentAddress, bytes memory _adapterParams) internal virtual returns (uint amount) {

133:     function _sendAndCallAck(uint16 _srcChainId, bytes memory _srcAddress, uint64 _nonce, bytes memory _payload) internal virtual {

134:         (bytes32 from, address to, uint64 amountSD, bytes memory payloadForCall, uint64 gasForCall) = _decodeSendAndCallPayload(_payload);

162:         (bool success, bytes memory reason) = address(this).excessivelySafeCall(gasleft(), 150, abi.encodeWithSelector(this.callOnOFTReceived.selector, srcChainId, srcAddress, nonce, from_, to_, amount_, payloadForCall_, gas));

177:     function _checkAdapterParams(uint16 _dstChainId, uint16 _pkType, bytes memory _adapterParams, uint _extraGas) internal virtual {

211:     function _encodeSendAndCallPayload(address _from, bytes32 _toAddress, uint64 _amountSD, bytes memory _payload, uint64 _dstGasForCall) internal virtual view returns (bytes memory) {

222:     function _decodeSendAndCallPayload(bytes memory _payload) internal virtual view returns (bytes32 from, address to, uint64 amountSD, bytes memory payload, uint64 dstGasForCall) {

```

```solidity
File: OFT/interfaces/ILayerZeroEndpoint.sol

15:     function send(uint16 _dstChainId, bytes calldata _destination, bytes calldata _payload, address payable _refundAddress, address _zroPaymentAddress, bytes calldata _adapterParams) external payable;

24:     function receivePayload(uint16 _srcChainId, bytes calldata _srcAddress, address _dstAddress, uint64 _nonce, uint _gasLimit, bytes calldata _payload) external;

41:     function estimateFees(uint16 _dstChainId, address _userApplication, bytes calldata _payload, bool _payInZRO, bytes calldata _adapterParam) external view returns (uint nativeFee, uint zroFee);

78:     function getConfig(uint16 _version, uint16 _chainId, address _userApplication, uint _configType) external view returns (bytes memory);

```

```solidity
File: OFT/libraries/LzLib.sol

21:     function buildAdapterParams(LzLib.AirdropParams memory _airdropParams, uint _uaGasLimit) internal pure returns (bytes memory adapterParams) {

55:     function decodeAdapterParams(bytes memory _adapterParams) internal pure returns (uint16 txType, uint uaGas, uint airdropAmount, address payable airdropAddress) {

```

```solidity
File: OFT/lzApp/LzApp.sol

35:     function lzReceive(uint16 _srcChainId, bytes calldata _srcAddress, uint64 _nonce, bytes calldata _payload) public virtual override {

41:         require(_srcAddress.length == trustedRemote.length && trustedRemote.length > 0 && keccak256(_srcAddress) == keccak256(trustedRemote), "LzApp: invalid source sending contract");

47:     function _blockingLzReceive(uint16 _srcChainId, bytes memory _srcAddress, uint64 _nonce, bytes memory _payload) internal virtual;

49:     function _lzSend(uint16 _dstChainId, bytes memory _payload, address payable _refundAddress, address _zroPaymentAddress, bytes memory _adapterParams, uint _nativeFee) internal virtual {

53:         lzEndpoint.send{value: _nativeFee}(_dstChainId, trustedRemote, _payload, _refundAddress, _zroPaymentAddress, _adapterParams);

56:     function _checkGasLimit(uint16 _dstChainId, uint16 _type, bytes memory _adapterParams, uint _extraGas) internal view virtual {

84:     function setConfig(uint16 _version, uint16 _chainId, uint _configType, bytes calldata _config) external override onlyOwner {

```

```solidity
File: OFT/lzApp/NonblockingLzApp.sol

24:     function _blockingLzReceive(uint16 _srcChainId, bytes memory _srcAddress, uint64 _nonce, bytes memory _payload) internal virtual override {

25:         (bool success, bytes memory reason) = address(this).excessivelySafeCall(gasleft(), 150, abi.encodeWithSelector(this.nonblockingLzReceive.selector, _srcChainId, _srcAddress, _nonce, _payload));

32:     function _storeFailedMessage(uint16 _srcChainId, bytes memory _srcAddress, uint64 _nonce, bytes memory _payload, bytes memory _reason) internal virtual {

37:     function nonblockingLzReceive(uint16 _srcChainId, bytes calldata _srcAddress, uint64 _nonce, bytes calldata _payload) public virtual {

44:     function _nonblockingLzReceive(uint16 _srcChainId, bytes memory _srcAddress, uint64 _nonce, bytes memory _payload) internal virtual;

46:     function retryMessage(uint16 _srcChainId, bytes calldata _srcAddress, uint64 _nonce, bytes calldata _payload) public payable virtual {

```

```solidity
File: lybra/configuration/LybraConfigurator.sol

202:             require(newRatio >= vaultBadCollateralRatio[pool] + 1e19, "PeUSD vault safe collateralRatio should more than bad collateralRatio");

```

```solidity
File: lybra/governance/AdminTimelock.sol

8:     constructor(uint256 minDelay, address[] memory proposers, address[] memory executors, address admin) TimelockController(minDelay, proposers, executors, admin) {}

```

```solidity
File: lybra/governance/GovernanceTimelock.sol

15:     constructor(uint256 minDelay, address[] memory proposers, address[] memory executors, address admin) TimelockController(minDelay, proposers, executors, admin) {

```

```solidity
File: lybra/governance/LybraGovernance.sol

38:       constructor(string memory name_, TimelockController timelock_, address _esLBR) GovernorTimelockControl(timelock_)  Governor(name_) {

60:         return proposalData[proposalId].supportVotes[1] + proposalData[proposalId].supportVotes[2] >= quorum(proposalSnapshot(proposalId));

106:     function _execute(uint256 /* proposalId */, address[] memory targets, uint256[] memory values, bytes[] memory calldatas, bytes32 descriptionHash) internal virtual override {

112:     function proposals(uint256 proposalId) external view returns (uint256 id, address proposer, uint256 eta, uint256 startBlock, uint256 endBlock, uint256 forVotes, uint256 againstVotes, uint256 abstainVotes, bool canceled, bool executed) {

```

```solidity
File: lybra/miner/EUSDMiningIncentives.sol

60:     event ClaimedOtherEarnings(address indexed user, address indexed Victim, uint256 buyAmount, uint256 biddingFee, bool useEUSD, uint256 time);

188:         return ((stakedOf(_account) * getBoost(_account) * (rewardPerToken() - userRewardPerTokenPaid[_account])) / 1e38) + rewards[_account];

```

```solidity
File: lybra/miner/ProtocolRewardsPool.sol

88:         require(block.timestamp >= esLBRBoost.getUnlockTime(msg.sender), "Your lock-in period has not ended. You can't convert your esLBR now.");

152:         amount = (a * (75e18 - ((time2fullRedemption[user] - block.timestamp) * 70e18) / (exitCycle / 1 days - 3) / 1 days)) / 100e18;

157:             amount = block.timestamp > time2fullRedemption[user] ? unstakeRatio[user] * (time2fullRedemption[user] - lastWithdrawTime[user]) : unstakeRatio[user] * (block.timestamp - lastWithdrawTime[user]);

```

```solidity
File: lybra/miner/esLBRBoost.sol

42:             require(userStatus.duration <= _setting.duration, "Your lock-in period has not ended, and the term can only be extended, not reduced.");

```

```solidity
File: lybra/miner/stakerewardV2pool.sol

107:         return ((balanceOf[_account] * getBoost(_account) * (rewardPerToken() - userRewardPerTokenPaid[_account])) / 1e38) + rewards[_account];

```

```solidity
File: lybra/pools/LybraWstETHVault.sol

25:     constructor(address _lido, address _peusd, address _oracle, address _asset, address _config) LybraPeUSDVaultBase(_peusd, _oracle, _asset,_config) {

```

```solidity
File: lybra/pools/base/LybraEUSDVaultBase.sol

41:     event LiquidationRecord(address provider, address keeper, address indexed onBehalfOf, uint256 eusdamount, uint256 liquidateEtherAmount, uint256 keeperReward, bool superLiquidation, uint256 timestamp);

43:     event RigidRedemption(address indexed caller, address indexed provider, uint256 eusdAmount, uint256 collateralAmount, uint256 timestamp);

189:         require((totalDepositedAsset * assetPrice * 100) / poolTotalEUSDCirculation < badCollateralRatio, "overallCollateralRatio should below 150%");

292:         if (((depositedAsset[_user] * _assetPrice * 100) / borrowed[_user]) < configurator.getSafeCollateralRatio(address(this))) revert("collateralRatio is Below safeCollateralRatio");

```

```solidity
File: lybra/pools/base/LybraPeUSDVaultBase.sol

32:     event LiquidationRecord(address provider, address keeper, address indexed onBehalfOf, uint256 eusdamount, uint256 LiquidateAssetAmount, uint256 keeperReward, bool superLiquidation, uint256 timestamp);

34:     event RigidRedemption(address indexed caller, address indexed provider, uint256 peusdAmount, uint256 assetAmount, uint256 timestamp);

128:         require(onBehalfOfCollateralRatio < configurator.getBadCollateralRatio(address(this)), "Borrowers collateral ratio should below badCollateralRatio");

```

```solidity
File: lybra/token/EUSD.sol

77:     event SharesBurnt(address indexed account, uint256 preRebaseTokenAmount, uint256 postRebaseTokenAmount, uint256 sharesAmount);

459:     function burnShares(address _account, uint256 _sharesAmount) external onlyMintVault BurnPaused returns (uint256 newTotalShares) {

```

```solidity
File: lybra/token/LBR.sol

18:     constructor(address _config, uint8 _sharedDecimals, address _lzEndpoint) ERC20("LBR", "LBR") BaseOFTV2(_sharedDecimals, _lzEndpoint) {

```

```solidity
File: lybra/token/PeUSDMainnetStableVision.sol

55:     constructor(address _config, uint8 _sharedDecimals, address _lzEndpoint) ERC20("peg-eUSD", "PeUSD") BaseOFTV2(_sharedDecimals, _lzEndpoint) {

```

```solidity
File: mocks/LZEndpointMock.sol

14: - adapter parameters: allows UAs to add arbitrary transaction params in the send() function, like airdrop on destination chain.

114:     function send(uint16 _chainId, bytes memory _path, bytes calldata _payload, address payable _refundAddress, address _zroPaymentAddress, bytes memory _adapterParams) external payable override sendNonReentrant {

154:     function receivePayload(uint16 _srcChainId, bytes calldata _path, address _dstAddress, uint64 _nonce, uint _gasLimit, bytes calldata _payload) external override receiveNonReentrant {

188:             try ILayerZeroReceiver(_dstAddress).lzReceive{gas: _gasLimit}(_srcChainId, _path, _nonce, _payload) {} catch (bytes memory reason) {

205:     function estimateFees(uint16 _dstChainId, address _userApplication, bytes memory _payload, bool _payInZRO, bytes memory _adapterParams) public view override returns (uint nativeFee, uint zroFee) {

329:     function setRelayerPrice(uint128 _dstPriceRatio, uint128 _dstGasPriceInWei, uint128 _dstNativeAmtCap, uint64 _baseGas, uint64 _gasPerByte) external {

```

### <a name="NC-5"></a>[NC-5]  `require()` / `revert()` statements should have descriptive reason strings

*Instances (3)*:
```solidity
File: OFT/util/ExcessivelySafeCall.sol

124:         require(_buf.length >= 4);

```

```solidity
File: lybra/miner/ProtocolRewardsPool.sol

228:         require(msg.sender == address(configurator));

```

```solidity
File: lybra/pools/LybraRETHVault.sol

42:         require(configurator.hasRole(keccak256("TIMELOCK"), msg.sender));

```

### <a name="NC-6"></a>[NC-6] Return values of `approve()` not checked
Not all IERC20 implementations `revert()` when there's a failure in `approve()`. The function signature has a boolean return value and they indicate errors that way instead. By not checking the return value, operations that should have marked as failed, may potentially go through without actually approving anything

*Instances (14)*:
```solidity
File: lybra/configuration/LybraConfigurator.sol

302:                 EUSD.approve(address(curvePool), balance);

```

```solidity
File: lybra/pools/LybraWstETHVault.sol

35:         lido.approve(address(collateralAsset), msg.value);

```

```solidity
File: lybra/token/EUSD.sol

182:                 _approve(owner, spender, currentAllowance - amount);

202:         _approve(owner, _spender, _amount);

250:         _approve(owner, _spender, allowances[owner][_spender].add(_addedValue));

273:             _approve(owner, spender, currentAllowance - subtractedValue);

```

```solidity
File: mocks/mockEUSD.sol

201:                 _approve(owner, spender, currentAllowance - amount);

221:         _approve(owner, _spender, _amount);

276:         _approve(

303:             _approve(owner, spender, currentAllowance - subtractedValue);

```

```solidity
File: mocks/stETHMock.sol

207:         _approve(msg.sender, _spender, _amount);

243:         _approve(_sender, msg.sender, currentAllowance.sub(_amount));

264:         _approve(

295:         _approve(msg.sender, _spender, currentAllowance.sub(_subtractedValue));

```

### <a name="NC-7"></a>[NC-7] Event is missing `indexed` fields
Index event fields make the field more quickly accessible to off-chain tools that parse events. However, note that each index field costs extra gas during emission, so it's not necessarily best to index the maximum allowed per event (three fields). Each event should use three indexed fields if there are three or more fields, and gas usage is not particularly of concern for the events in question. If there are fewer than three fields, all of the fields should be indexed.

*Instances (59)*:
```solidity
File: OFT/OFTCoreV2.sol

35:     event ReceiveFromChain(uint16 indexed _srcChainId, address indexed _to, uint _amount);

37:     event SetUseCustomAdapterParams(bool _useCustomAdapterParams);

39:     event CallOFTReceivedSuccess(uint16 indexed _srcChainId, bytes _srcAddress, uint64 _nonce, bytes32 _hash);

41:     event NonContractAddress(address _address);

```

```solidity
File: OFT/interfaces/IStargateWidget.sol

14:     event WidgetSwapped(bytes2 indexed partnerId, uint256 tenthBps, uint256 widgetFee);

```

```solidity
File: OFT/lzApp/LzApp.sol

26:     event SetPrecrime(address precrime);

27:     event SetTrustedRemote(uint16 _remoteChainId, bytes _path);

28:     event SetTrustedRemoteAddress(uint16 _remoteChainId, bytes _remoteAddress);

29:     event SetMinDstGas(uint16 _dstChainId, uint16 _type, uint _minDstGas);

```

```solidity
File: OFT/lzApp/NonblockingLzApp.sol

20:     event MessageFailed(uint16 _srcChainId, bytes _srcAddress, uint64 _nonce, bytes _payload, bytes _reason);

21:     event RetryMessageSuccess(uint16 _srcChainId, bytes _srcAddress, uint64 _nonce, bytes32 _payloadHash);

```

```solidity
File: lybra/configuration/LybraConfigurator.sol

63:     event RedemptionFeeChanged(uint256 newSlippage);

64:     event SafeCollateralRatioChanged(address indexed pool, uint256 newRatio);

65:     event RedemptionProvider(address indexed user, bool status);

66:     event ProtocolRewardsPoolChanged(address indexed pool, uint256 timestamp);

67:     event EUSDMiningIncentivesChanged(address indexed pool, uint256 timestamp);

68:     event BorrowApyChanged(address indexed pool, uint256 newApy);

69:     event KeeperRatioChanged(address indexed pool, uint256 newSlippage);

70:     event tokenMinerChanges(address indexed pool, bool status);

74:     event FlashloanFeeUpdated(uint256 fee);

```

```solidity
File: lybra/miner/EUSDMiningIncentives.sol

59:     event ClaimReward(address indexed user, uint256 amount, uint256 time);

60:     event ClaimedOtherEarnings(address indexed user, address indexed Victim, uint256 buyAmount, uint256 biddingFee, bool useEUSD, uint256 time);

61:     event NotifyRewardChanged(uint256 addAmount, uint256 time);

```

```solidity
File: lybra/miner/ProtocolRewardsPool.sol

42:     event Restake(address indexed user, uint256 amount, uint256 time);

43:     event StakeLBR(address indexed user, uint256 amount, uint256 time);

44:     event UnstakeLBR(address indexed user, uint256 amount, uint256 time);

45:     event WithdrawLBR(address indexed user, uint256 amount, uint256 time);

46:     event ClaimReward(address indexed user, uint256 eUSDAmount, address token, uint256 tokenAmount, uint256 time);

```

```solidity
File: lybra/miner/stakerewardV2pool.sol

44:     event StakeToken(address indexed user, uint256 amount, uint256 time);

45:     event WithdrawToken(address indexed user, uint256 amount, uint256 time);

46:     event ClaimReward(address indexed user, uint256 amount, uint256 time);

47:     event NotifyRewardChanged(uint256 addAmount, uint256 time);

```

```solidity
File: lybra/pools/base/LybraEUSDVaultBase.sol

34:     event DepositEther(address indexed onBehalfOf, address asset, uint256 etherAmount, uint256 assetAmount, uint256 timestamp);

36:     event DepositAsset(address indexed onBehalfOf, address asset, uint256 amount, uint256 timestamp);

38:     event WithdrawAsset(address sponsor, address asset, address indexed onBehalfOf, uint256 amount, uint256 timestamp);

39:     event Mint(address sponsor, address indexed onBehalfOf, uint256 amount, uint256 timestamp);

40:     event Burn(address sponsor, address indexed onBehalfOf, uint256 amount, uint256 timestamp);

41:     event LiquidationRecord(address provider, address keeper, address indexed onBehalfOf, uint256 eusdamount, uint256 liquidateEtherAmount, uint256 keeperReward, bool superLiquidation, uint256 timestamp);

42:     event LSDValueCaptured(uint256 stETHAdded, uint256 payoutEUSD, uint256 discountRate, uint256 timestamp);

43:     event RigidRedemption(address indexed caller, address indexed provider, uint256 eusdAmount, uint256 collateralAmount, uint256 timestamp);

44:     event FeeDistribution(address indexed feeAddress, uint256 feeAmount, uint256 timestamp);

```

```solidity
File: lybra/pools/base/LybraPeUSDVaultBase.sol

26:     event DepositEther(address indexed onBehalfOf, address asset, uint256 etherAmount, uint256 assetAmount, uint256 timestamp);

28:     event DepositAsset(address indexed onBehalfOf, address asset, uint256 amount, uint256 timestamp);

29:     event WithdrawAsset(address sponsor, address indexed onBehalfOf, address asset, uint256 amount, uint256 timestamp);

30:     event Mint(address sponsor, address indexed onBehalfOf, uint256 amount, uint256 timestamp);

31:     event Burn(address sponsor, address indexed onBehalfOf, uint256 amount, uint256 timestamp);

32:     event LiquidationRecord(address provider, address keeper, address indexed onBehalfOf, uint256 eusdamount, uint256 LiquidateAssetAmount, uint256 keeperReward, bool superLiquidation, uint256 timestamp);

34:     event RigidRedemption(address indexed caller, address indexed provider, uint256 peusdAmount, uint256 assetAmount, uint256 timestamp);

35:     event FeeDistribution(address indexed feeAddress, uint256 feeAmount, uint256 timestamp);

```

```solidity
File: lybra/token/EUSD.sol

63:     event TransferShares(address indexed from, address indexed to, uint256 sharesValue);

77:     event SharesBurnt(address indexed account, uint256 preRebaseTokenAmount, uint256 postRebaseTokenAmount, uint256 sharesAmount);

```

```solidity
File: lybra/token/PeUSDMainnetStableVision.sol

40:     event Flashloaned(FlashBorrower indexed receiver, uint256 borrowShares, uint256 burnAmount);

```

```solidity
File: mocks/LZEndpointMock.sol

92:     event UaForceResumeReceive(uint16 chainId, bytes srcAddress);

93:     event PayloadCleared(uint16 srcChainId, bytes srcAddress, uint64 nonce, address dstAddress);

94:     event PayloadStored(uint16 srcChainId, bytes srcAddress, address dstAddress, uint64 nonce, bytes payload, bytes reason);

```

```solidity
File: mocks/mockEUSD.sol

64:     event TransferShares(

82:     event SharesBurnt(

```

```solidity
File: mocks/stETHMock.sol

65:     event TransferShares(

83:     event SharesBurnt(

```

### <a name="NC-8"></a>[NC-8] Constants should be defined rather than using magic numbers

*Instances (17)*:
```solidity
File: OFT/OFTCoreV2.sol

207:         to = _payload.toAddress(13); // drop the first 12 bytes of bytes32

208:         amountSD = _payload.toUint64(33);

225:         to = _payload.toAddress(13); // drop the first 12 bytes of bytes32

226:         amountSD = _payload.toUint64(33);

227:         from = _payload.toBytes32(41);

228:         dstGasForCall = _payload.toUint64(73);

229:         payload = _payload.slice(81, _payload.length - 81);

```

```solidity
File: OFT/util/BytesLib.sol

84:             not(31) // Round down to the nearest 32 bytes.

281:                 mstore(0x40, and(add(mc, 31), not(31)))

```

```solidity
File: lybra/miner/ProtocolRewardsPool.sol

152:         amount = (a * (75e18 - ((time2fullRedemption[user] - block.timestamp) * 70e18) / (exitCycle / 1 days - 3) / 1 days)) / 100e18;

```

```solidity
File: lybra/miner/esLBRBoost.sol

26:         esLBRLockSettings.push(esLBRLockSetting(30 days, 20 * 1e18));

27:         esLBRLockSettings.push(esLBRLockSetting(90 days, 30 * 1e18));

28:         esLBRLockSettings.push(esLBRLockSetting(180 days, 50 * 1e18));

29:         esLBRLockSettings.push(esLBRLockSetting(365 days, 100 * 1e18));

```

```solidity
File: lybra/pools/base/LybraEUSDVaultBase.sol

301:         return (poolTotalEUSDCirculation * configurator.vaultMintFeeApy(address(this)) * (block.timestamp - lastReportTime)) / (86400 * 365) / 10000;

```

```solidity
File: lybra/pools/base/LybraPeUSDVaultBase.sol

238:         return (borrowed[user] * configurator.vaultMintFeeApy(address(this)) * (block.timestamp - feeUpdatedAt[user])) / (86400 * 365) / 10000;

```

```solidity
File: mocks/LZEndpointMock.sol

110:         defaultAdapterParams = LzLib.buildDefaultAdapterParams(200000);

```

### <a name="NC-9"></a>[NC-9] Functions not used internally could be marked external

*Instances (47)*:
```solidity
File: lybra/governance/GovernanceTimelock.sol

25:     function checkRole(bytes32 role, address _sender) public view  returns(bool){

29:     function checkOnlyRole(bytes32 role, address _sender) public view  returns(bool){

```

```solidity
File: lybra/governance/LybraGovernance.sol

143:     function votingPeriod() public pure override returns (uint256){

147:      function votingDelay() public pure override returns (uint256){

151:      function CLOCK_MODE() public override view returns (string memory){

172:     function proposalThreshold() public pure override returns (uint256) {

```

```solidity
File: lybra/pools/base/LybraPeUSDVaultBase.sol

44:     function totalDepositedAsset() public view returns (uint256) {

257:     function getPoolTotalPeUSDCirculation() public view returns (uint256) {

```

```solidity
File: lybra/token/EUSD.sol

99:     function name() public pure returns (string memory) {

107:     function symbol() public pure returns (string memory) {

114:     function decimals() public pure returns (uint8) {

124:     function totalSupply() public view returns (uint256) {

134:     function balanceOf(address _account) public view returns (uint256) {

153:     function transfer(address _recipient, uint256 _amount) public returns (bool) {

200:     function approve(address _spender, uint256 _amount) public returns (bool) {

226:     function transferFrom(address from, address to, uint256 amount) public returns (bool) {

248:     function increaseAllowance(address _spender, uint256 _addedValue) public returns (bool) {

285:     function getTotalShares() public view returns (uint256) {

292:     function sharesOf(address _account) public view returns (uint256) {

334:     function transferShares(address _recipient, uint256 _sharesAmount) public returns (uint256) {

```

```solidity
File: lybra/token/PeUSDMainnetStableVision.sol

129:     function executeFlashloan(FlashBorrower receiver, uint256 eusdAmount, bytes calldata data) public payable {

141:     function transferFrom(address from, address to, uint256 amount) public override returns (bool) {

165:     function getAccruedEUSDInterest(

```

```solidity
File: mocks/mockEUSD.sol

112:     function name() public pure returns (string memory) {

120:     function symbol() public pure returns (string memory) {

127:     function decimals() public pure returns (uint8) {

137:     function totalSupply() public view returns (uint256) {

147:     function balanceOf(address _account) public view returns (uint256) {

166:     function transfer(

219:     function approve(address _spender, uint256 _amount) public returns (bool) {

245:     function transferFrom(

271:     function increaseAllowance(

315:     function getTotalShares() public view returns (uint256) {

322:     function sharesOf(address _account) public view returns (uint256) {

368:     function transferShares(

```

```solidity
File: mocks/stETHMock.sol

133:     function decimals() public pure returns (uint8) {

143:     function totalSupply() public view returns (uint256) {

153:     function balanceOf(address _account) public view returns (uint256) {

172:     function transfer(

186:     function allowance(

206:     function approve(address _spender, uint256 _amount) public returns (bool) {

231:     function transferFrom(

260:     function increaseAllowance(

286:     function decreaseAllowance(

305:     function getTotalShares() public view returns (uint256) {

312:     function sharesOf(address _account) public view returns (uint256) {

360:     function transferShares(

```

### <a name="NC-10"></a>[NC-10] Variable names don't follow the Solidity style guide
Constant variable should be all caps  each word should use all capital letters, with underscores separating each word (CONSTANT_CASE)

*Instances (9)*:
```solidity
File: OFT/lzApp/LzApp.sol

18:     uint constant public DEFAULT_PAYLOAD_SIZE_LIMIT = 10000;

```

```solidity
File: OFT/util/BitLib.sol

9:     uint256 constant m1  = 0x5555555555555555555555555555555555555555555555555555555555555555;

10:     uint256 constant m2  = 0x3333333333333333333333333333333333333333333333333333333333333333;

11:     uint256 constant m4  = 0x0F0F0F0F0F0F0F0F0F0F0F0F0F0F0F0F0F0F0F0F0F0F0F0F0F0F0F0F0F0F0F0F;

12:     uint256 constant m8  = 0x00FF00FF00FF00FF00FF00FF00FF00FF00FF00FF00FF00FF00FF00FF00FF00FF;

13:     uint256 constant m16 = 0x0000FFFF0000FFFF0000FFFF0000FFFF0000FFFF0000FFFF0000FFFF0000FFFF;

14:     uint256 constant m32 = 0x00000000FFFFFFFF00000000FFFFFFFF00000000FFFFFFFF00000000FFFFFFFF;

15:     uint256 constant m64 = 0x0000000000000000FFFFFFFFFFFFFFFF0000000000000000FFFFFFFFFFFFFFFF;

16:     uint256 constant m128= 0x00000000000000000000000000000000FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF;

```

### <a name="NC-11"></a>[NC-11] Variables need not be initialized to zero
Default value is zero which makes the initialization unnecessary.

*Instances (2)*:
```solidity
File: OFT/OFTCoreV2.sol

14:     uint public constant NO_EXTRA_GAS = 0;

17:     uint8 public constant PT_SEND = 0;

```


## Low Issues


| |Issue|Instances|
|-|:-|:-:|
| [L-1](#L-1) | Divide before Multiplication | 9 |
| [L-2](#L-2) | Empty Function Body - Consider commenting why | 32 |
| [L-3](#L-3) | ERC20 tokens that do not implement optional decimals method cannot be used | 9 |
| [L-4](#L-4) | _safeMint() should be used rather than _mint() | 13 |
| [L-5](#L-5) | Unsafe ERC20 operation(s) | 36 |
| [L-6](#L-6) | Unspecific compiler version pragma | 10 |
### <a name="L-1"></a>[L-1] Divide before Multiplication
Unnecessary loss of precision caused by divide before multiplication

*Instances (9)*:
```solidity
File: lybra/miner/EUSDMiningIncentives.sol

147: 	    amount = amount / (1e20 + extraRatio) * 1e20;

```

```solidity
File: lybra/pools/LybraStETHVault.sol

66:         uint256 payAmount = (((realAmount * getAssetPrice()) / 1e18) * getDutchAuctionDiscountPrice()) / 10000;

104:         return 10000 - (time / 30 minutes - 1) * 100;

```

```solidity
File: lybra/pools/base/LybraEUSDVaultBase.sol

239:         uint256 collateralAmount = (((eusdAmount * 1e18) / assetPrice) * (10000 - configurator.redemptionFee())) / 10000;

301:         return (poolTotalEUSDCirculation * configurator.vaultMintFeeApy(address(this)) * (block.timestamp - lastReportTime)) / (86400 * 365) / 10000;

```

```solidity
File: lybra/pools/base/LybraPeUSDVaultBase.sol

164:         uint256 collateralAmount = (((peusdAmount * 1e18) / assetPrice) * (10000 - configurator.redemptionFee())) / 10000;

238:         return (borrowed[user] * configurator.vaultMintFeeApy(address(this)) * (block.timestamp - feeUpdatedAt[user])) / (86400 * 365) / 10000;

```

```solidity
File: mocks/LZEndpointMock.sol

390:         uint basePrice = (totalRemoteToken * relayerFeeConfig.dstPriceRatio) / 10**10;

393:         uint pricePerByte = (relayerFeeConfig.dstGasPriceInWei * relayerFeeConfig.gasPerByte * relayerFeeConfig.dstPriceRatio) / 10**10;

```

### <a name="L-2"></a>[L-2] Empty Function Body - Consider commenting why

*Instances (32)*:
```solidity
File: OFT/lzApp/NonblockingLzApp.sol

16:     constructor(address _endpoint) LzApp(_endpoint) {}

```

```solidity
File: OFT/util/BytesLib.sol

489:                         for {} eq(add(lt(mc, end), cb), 2) {

```

```solidity
File: lybra/Proxy/LybraProxy.sol

9:     constructor(address _logic,address _admin,bytes memory _data) TransparentUpgradeableProxy(_logic, _admin, _data) {}

```

```solidity
File: lybra/Proxy/LybraProxyAdmin.sol

6: contract LybraProxyAdmin is ProxyAdmin {}

```

```solidity
File: lybra/governance/AdminTimelock.sol

8:     constructor(uint256 minDelay, address[] memory proposers, address[] memory executors, address admin) TimelockController(minDelay, proposers, executors, admin) {}

```

```solidity
File: lybra/miner/EUSDMiningIncentives.sol

177:     function refreshReward(address _account) external updateReward(_account) {}

219:                 try configurator.distributeRewards() {} catch {}

219:                 try configurator.distributeRewards() {} catch {}

```

```solidity
File: lybra/miner/ProtocolRewardsPool.sol

184:     function refreshReward(address _account) external updateReward(_account) {}

```

```solidity
File: lybra/pools/LybraStETHVault.sol

73:             try configurator.distributeRewards() {} catch {}

73:             try configurator.distributeRewards() {} catch {}

87:             try configurator.distributeRewards() {} catch {}

87:             try configurator.distributeRewards() {} catch {}

```

```solidity
File: lybra/pools/LybraWbETHVault.sol

18:         LybraPeUSDVaultBase(_peusd, _oracle, _asset, _config) {}

```

```solidity
File: lybra/pools/base/LybraEUSDVaultBase.sol

261:         try configurator.refreshMintReward(_provider) {} catch {}

261:         try configurator.refreshMintReward(_provider) {} catch {}

280:         try configurator.refreshMintReward(_onBehalfOf) {} catch {}

280:         try configurator.refreshMintReward(_onBehalfOf) {} catch {}

```

```solidity
File: lybra/pools/base/LybraPeUSDVaultBase.sol

177:         try configurator.refreshMintReward(_provider) {} catch {}

177:         try configurator.refreshMintReward(_provider) {} catch {}

193:         try configurator.refreshMintReward(_onBehalfOf) {} catch {}

193:         try configurator.refreshMintReward(_onBehalfOf) {} catch {}

205:         try configurator.distributeRewards() {} catch {}

205:         try configurator.distributeRewards() {} catch {}

```

```solidity
File: lybra/token/esLBR.sol

33:         try IProtocolRewardsPool(configurator.getProtocolRewardsPool()).refreshReward(user) {} catch {}

33:         try IProtocolRewardsPool(configurator.getProtocolRewardsPool()).refreshReward(user) {} catch {}

40:         try IProtocolRewardsPool(configurator.getProtocolRewardsPool()).refreshReward(user) {} catch {}

40:         try IProtocolRewardsPool(configurator.getProtocolRewardsPool()).refreshReward(user) {} catch {}

```

```solidity
File: mocks/LZEndpointMock.sol

188:             try ILayerZeroReceiver(_dstAddress).lzReceive{gas: _gasLimit}(_srcChainId, _path, _nonce, _payload) {} catch (bytes memory reason) {

287:     ) external override {}

291:     ) external override {}

295:     ) external override {}

```

### <a name="L-3"></a>[L-3] ERC20 tokens that do not implement optional decimals method cannot be used
Underlying token that does not implement optional decimals method cannot be used 
 
 > [EIP-20](https://eips.ethereum.org/EIPS/eip-20#decimals) OPTIONAL - This method can be used to improve usability, but interfaces and other contracts MUST NOT expect these values to be present.

*Instances (9)*:
```solidity
File: lybra/miner/ProtocolRewardsPool.sol

209:                     uint256 tokenAmount = (reward - eUSDShare - peUSDBalance) * token.decimals() / 1e18;

236:             rewardPerTokenStored = rewardPerTokenStored + (amount * 1e36 / token.decimals()) / totalStaked();

```

```solidity
File: lybra/token/EUSD.sol

114:     function decimals() public pure returns (uint8) {

```

```solidity
File: lybra/token/LBR.sol

20:         uint8 decimals = decimals();

```

```solidity
File: lybra/token/PeUSD.sol

12:         uint8 decimals = decimals();

```

```solidity
File: lybra/token/PeUSDMainnetStableVision.sol

58:         uint8 decimals = decimals();

```

```solidity
File: mocks/mockEUSD.sol

127:     function decimals() public pure returns (uint8) {

```

```solidity
File: mocks/mockEtherPriceOracle.sol

6:     function decimals() external view returns (uint8){

```

```solidity
File: mocks/stETHMock.sol

133:     function decimals() public pure returns (uint8) {

```

### <a name="L-4"></a>[L-4] _safeMint() should be used rather than _mint()
_mint() is discouraged in favor of _safeMint() which ensures that the recipient is either an EOA or implements IERC721Receiver. Both OpenZeppelin and solmate have versions of this function

*Instances (13)*:
```solidity
File: lybra/miner/EUSDMiningIncentives.sol

70: 	_mint(0x0000000);

```

```solidity
File: lybra/token/LBR.sol

28:         _mint(user, amount);

57:         _mint(_toAddress, _amount);

```

```solidity
File: lybra/token/PeUSD.sol

39:         _mint(_toAddress, _amount);

```

```solidity
File: lybra/token/PeUSDMainnetStableVision.sol

65:         _mint(to, amount);

87:         _mint(user, eusdAmount);

192:         _mint(_toAddress, _amount);

```

```solidity
File: lybra/token/esLBR.sol

34:         _mint(user, amount);

```

```solidity
File: mocks/mockUSDC.sol

33:         _mint(msg.sender, 1000000*1e18);

37:         _mint(msg.sender, 10000*1e18);

```

```solidity
File: mocks/mockWstETH.sol

67:         _mint(msg.sender, wstETHAmount);

73:         _mint(msg.sender, 100*1e18);

98:         _mint(msg.sender, shares);

```

### <a name="L-5"></a>[L-5] Unsafe ERC20 operation(s)

*Instances (36)*:
```solidity
File: lybra/configuration/LybraConfigurator.sol

292:             peUSD.transfer(address(lybraProtocolRewardsPool), peUSDBalance);

299:                 EUSD.transfer(address(lybraProtocolRewardsPool), balance);

302:                 EUSD.approve(address(curvePool), balance);

304:                 IEUSD(stableToken).transfer(address(lybraProtocolRewardsPool), amount);

```

```solidity
File: lybra/miner/EUSDMiningIncentives.sol

217:                 bool success = EUSD.transferFrom(msg.sender, address(configurator), biddingFee);

```

```solidity
File: lybra/miner/ProtocolRewardsPool.sol

202:                     peUSD.transfer(msg.sender, reward - eUSDShare);

206:                         peUSD.transfer(msg.sender, peUSDBalance);

210:                     token.transfer(msg.sender, tokenAmount);

```

```solidity
File: lybra/miner/stakerewardV2pool.sol

85:         bool success = stakingToken.transferFrom(msg.sender, address(this), _amount);

97:         stakingToken.transfer(msg.sender, _amount);

```

```solidity
File: lybra/pools/LybraStETHVault.sol

70:             bool success = EUSD.transferFrom(msg.sender, address(configurator), income);

85:             bool success = EUSD.transferFrom(msg.sender, address(configurator), payAmount);

93:         collateralAsset.transfer(msg.sender, realAmount);

```

```solidity
File: lybra/pools/LybraWstETHVault.sol

35:         lido.approve(address(collateralAsset), msg.value);

```

```solidity
File: lybra/pools/base/LybraEUSDVaultBase.sol

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
File: lybra/pools/base/LybraPeUSDVaultBase.sol

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
File: lybra/token/PeUSDMainnetStableVision.sol

82:         bool success = EUSD.transferFrom(user, address(this), eusdAmount);

133:         bool success = EUSD.transferFrom(address(receiver), address(this), EUSD.getMintedEUSDByShares(shareAmount));

```

```solidity
File: mocks/mockCurve.sol

24:         eusd.transferFrom(msg.sender, address(this), dx);

25:         usdc.transfer(msg.sender, min_dy);

```

```solidity
File: mocks/mockWstETH.sol

68:         stETH.transferFrom(msg.sender, address(this), _stETHAmount);

89:         stETH.transfer(msg.sender, stETHAmount);

```

### <a name="L-6"></a>[L-6] Unspecific compiler version pragma

*Instances (10)*:
```solidity
File: OFT/ICommonOFT.sol

3: pragma solidity >=0.5.0;

```

```solidity
File: OFT/IOFTReceiverV2.sol

3: pragma solidity >=0.5.0;

```

```solidity
File: OFT/IOFTV2.sol

3: pragma solidity >=0.5.0;

```

```solidity
File: OFT/interfaces/ILayerZeroEndpoint.sol

3: pragma solidity >=0.5.0;

```

```solidity
File: OFT/interfaces/ILayerZeroReceiver.sol

3: pragma solidity >=0.5.0;

```

```solidity
File: OFT/interfaces/ILayerZeroUserApplicationConfig.sol

3: pragma solidity >=0.5.0;

```

```solidity
File: OFT/libraries/LzLib.sol

3: pragma solidity >=0.6.0;

```

```solidity
File: OFT/util/BitLib.sol

2: pragma solidity >=0.5.0;

```

```solidity
File: OFT/util/BytesLib.sol

9: pragma solidity >=0.8.0 <0.9.0;

```

```solidity
File: OFT/util/ExcessivelySafeCall.sol

2: pragma solidity >=0.7.6;

```


## Medium Issues


| |Issue|Instances|
|-|:-|:-:|
| [M-1](#M-1) | Centralization Risk for trusted owners | 37 |
### <a name="M-1"></a>[M-1] Centralization Risk for trusted owners

#### Impact:
Contracts have owners with privileged rights to perform admin tasks and need to be trusted to not perform malicious updates or drain funds.

*Instances (37)*:
```solidity
File: OFT/OFTCoreV2.sol

62:     function setUseCustomAdapterParams(bool _useCustomAdapterParams) public virtual onlyOwner {

```

```solidity
File: OFT/lzApp/LzApp.sol

14: abstract contract LzApp is Ownable, ILayerZeroReceiver, ILayerZeroUserApplicationConfig {

84:     function setConfig(uint16 _version, uint16 _chainId, uint _configType, bytes calldata _config) external override onlyOwner {

88:     function setSendVersion(uint16 _version) external override onlyOwner {

92:     function setReceiveVersion(uint16 _version) external override onlyOwner {

96:     function forceResumeReceive(uint16 _srcChainId, bytes calldata _srcAddress) external override onlyOwner {

102:     function setTrustedRemote(uint16 _remoteChainId, bytes calldata _path) external onlyOwner {

107:     function setTrustedRemoteAddress(uint16 _remoteChainId, bytes calldata _remoteAddress) external onlyOwner {

118:     function setPrecrime(address _precrime) external onlyOwner {

123:     function setMinDstGas(uint16 _dstChainId, uint16 _packetType, uint _minGas) external onlyOwner {

130:     function setPayloadSizeLimit(uint16 _dstChainId, uint _size) external onlyOwner {

```

```solidity
File: lybra/configuration/LybraConfigurator.sol

85:     modifier onlyRole(bytes32 role) {

98:     function initToken(address _eusd, address _peusd) external onlyRole(DAO) {

109:     function setMintVault(address pool, bool isActive) external onlyRole(DAO) {

119:     function setMintVaultMaxSupply(address pool, uint256 maxSupply) external onlyRole(DAO) {

126:     function setBadCollateralRatio(address pool, uint256 newRatio) external onlyRole(DAO) {

```

```solidity
File: lybra/miner/EUSDMiningIncentives.sol

27: contract EUSDMiningIncentives is Ownable {

86:     function setToken(address _lbr, address _eslbr) external onlyOwner {

91:     function setLBROracle(address _lbrOracle) external onlyOwner {

95:     function setPools(address[] memory _pools) external onlyOwner {

102:     function setBiddingCost(uint256 _biddingRatio) external onlyOwner {

107:     function setExtraRatio(uint256 ratio) external onlyOwner {

112:     function setPeUSDExtraRatio(uint256 ratio) external onlyOwner {

117:     function setBoost(address _boost) external onlyOwner {

121:     function setRewardsDuration(uint256 _duration) external onlyOwner {

126:     function setEthlbrStakeInfo(address _pool, address _lp) external onlyOwner {

130:     function setEUSDBuyoutAllowed(bool _bool) external onlyOwner {

231:     ) external onlyOwner updateReward(address(0)) {

```

```solidity
File: lybra/miner/ProtocolRewardsPool.sol

24: contract ProtocolRewardsPool is Ownable {

52:     function setTokenAddress(address _eslbr, address _lbr, address _boost) external onlyOwner {

58:     function setGrabCost(uint256 _ratio) external onlyOwner {

```

```solidity
File: lybra/miner/esLBRBoost.sol

7: contract esLBRBoost is Ownable {

33:     function addLockSetting(esLBRLockSetting memory setting) external onlyOwner {

```

```solidity
File: lybra/miner/stakerewardV2pool.sol

16: contract StakingRewardsV2 is Ownable {

121:     function setRewardsDuration(uint256 _duration) external onlyOwner {

127:     function setBoost(address _boost) external onlyOwner {

132:     function notifyRewardAmount(uint256 _amount) external onlyOwner updateReward(address(0)) {

```

