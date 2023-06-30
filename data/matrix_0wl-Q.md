## Non Critical Issues

|       | Issue                                                                                                  |
| ----- | :----------------------------------------------------------------------------------------------------- |
| NC-1  | Be explicit declaring types                                                                            |
| NC-2  | Boolean equality                                                                                       |
| NC-3  | GENERATE PERFECT CODE HEADERS EVERY TIME                                                               |
| NC-4  | USE A SINGLE FILE FOR ALL SYSTEM-WIDE CONSTANTS                                                        |
| NC-5  | INCONSISTENT SOLIDITY VERSIONS                                                                         |
| NC-6  | Use camel case for all functions, parameters and variables and snake case for constants                |
| NC-7  | LARGE MULTIPLES OF TEN SHOULD USE SCIENTIFIC NOTATION                                                  |
| NC-8  | FOR EXTENDED “USING-FOR” USAGE, USE THE LATEST PRAGMA VERSION                                          |
| NC-9  | NO SAME VALUE INPUT CONTROL                                                                            |
| NC-10 | Add parameter to event-emit                                                                            |
| NC-11 | SOLIDITY COMPILER OPTIMIZATIONS CAN BE PROBLEMATIC                                                     |
| NC-12 | USE A MORE RECENT VERSION OF SOLIDITY                                                                  |
| NC-13 | `require()` / `revert()` statements should have descriptive reason strings                             |
| NC-14 | FOR FUNCTIONS AND VARIABLES FOLLOW SOLIDITY STANDARD NAMING CONVENTIONS                                |
| NC-15 | Functions not used internally could be marked external                                                 |
| NC-16 | VARIABLE NAMES THAT CONSIST OF ALL CAPITAL LETTERS SHOULD BE RESERVED FOR CONSTANT/IMMUTABLE VARIABLES |

### [NC-1] Be explicit declaring types

#### Description:

Instead of uint use uint256.

#### **Proof Of Concept**

```solidity
File: contracts/lybra/miner/EUSDMiningIncentives.sol

94:         for (uint i = 0; i < _pools.length; i++) {

138:         for (uint i = 0; i < pools.length; i++) {

140:             uint borrowed = pool.getBorrowedOf(user);

153:         uint256 etherInLp = (IEUSD(0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2).balanceOf(ethlbrLpToken) * uint(etherPrice)) / 1e8;

154:         uint256 lbrInLp = (IEUSD(LBR).balanceOf(ethlbrLpToken) * uint(lbrPrice)) / 1e8;

```

```solidity
File: contracts/lybra/miner/ProtocolRewardsPool.sol

31:     uint public rewardPerTokenStored;

33:     mapping(address => uint) public userRewardPerTokenPaid;

35:     mapping(address => uint) public rewards;

36:     mapping(address => uint) public time2fullRedemption;

37:     mapping(address => uint) public unstakeRatio;

38:     mapping(address => uint) public lastWithdrawTime;

167:     function earned(address _account) public view returns (uint) {

191:         uint reward = rewards[msg.sender];

227:     function notifyRewardAmount(uint amount, uint tokenType) external {

227:     function notifyRewardAmount(uint amount, uint tokenType) external {

```

```solidity
File: contracts/lybra/token/LBR.sol

16:     uint internal immutable ld2sdRatio;

38:     function circulatingSupply() public view virtual override returns (uint) {

49:     function _debitFrom(address _from, uint16, bytes32, uint _amount) internal virtual override returns (uint) {

49:     function _debitFrom(address _from, uint16, bytes32, uint _amount) internal virtual override returns (uint) {

56:     function _creditTo(uint16, address _toAddress, uint _amount) internal virtual override returns (uint) {

56:     function _creditTo(uint16, address _toAddress, uint _amount) internal virtual override returns (uint) {

61:     function _transferFrom(address _from, address _to, uint _amount) internal virtual override returns (uint) {

61:     function _transferFrom(address _from, address _to, uint _amount) internal virtual override returns (uint) {

70:     function _ld2sdRatio() internal view virtual override returns (uint) {

```

```solidity
File: contracts/lybra/token/PeUSD.sol

9:     uint internal immutable ld2sdRatio;

20:     function circulatingSupply() public view virtual override returns (uint) {

31:     function _debitFrom(address _from, uint16, bytes32, uint _amount) internal virtual override returns (uint) {

31:     function _debitFrom(address _from, uint16, bytes32, uint _amount) internal virtual override returns (uint) {

38:     function _creditTo(uint16, address _toAddress, uint _amount) internal virtual override returns (uint) {

38:     function _creditTo(uint16, address _toAddress, uint _amount) internal virtual override returns (uint) {

43:     function _transferFrom(address _from, address _to, uint _amount) internal virtual override returns (uint) {

43:     function _transferFrom(address _from, address _to, uint _amount) internal virtual override returns (uint) {

52:     function _ld2sdRatio() internal view virtual override returns (uint) {

```

```solidity
File: contracts/lybra/token/PeUSDMainnetStableVision.sol

32:     uint internal immutable ld2sdRatio;

171:     function circulatingSupply() public view virtual override returns (uint) {

184:     function _debitFrom(address _from, uint16, bytes32, uint _amount) internal virtual override returns (uint) {

184:     function _debitFrom(address _from, uint16, bytes32, uint _amount) internal virtual override returns (uint) {

191:     function _creditTo(uint16, address _toAddress, uint _amount) internal virtual override returns (uint) {

191:     function _creditTo(uint16, address _toAddress, uint _amount) internal virtual override returns (uint) {

196:     function _transferFrom(address _from, address _to, uint _amount) internal virtual override returns (uint) {

196:     function _transferFrom(address _from, address _to, uint _amount) internal virtual override returns (uint) {

205:     function _ld2sdRatio() internal view virtual override returns (uint) {

```

### [NC-2] Boolean equality

#### Description:

Boolean variables can be checked within conditionals directly without the use of equality operators to true/false.

Referrer: https://github.com/crytic/slither/wiki/Detector-Documentation#boolean-equality

#### **Proof Of Concept**

```solidity
File: contracts/lybra/governance/LybraGovernance.sol

82:         require(receipt.hasVoted == false, "GovernorBravo::castVoteInternal: voter already voted");

```

### [NC-3] GENERATE PERFECT CODE HEADERS EVERY TIME

#### Description:

I recommend using header for Solidity code layout and readability for all contracts:
https://github.com/transmissions11/headers

```solidity
/*//////////////////////////////////////////////////////////////
                           TESTING 123
//////////////////////////////////////////////////////////////*/
```

### [NC-4] USE A SINGLE FILE FOR ALL SYSTEM-WIDE CONSTANTS

#### Description:

There are many constants used in the system. It is recommended to put the most used ones in one file (for example constants.sol, use inheritance to access these values).

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

### [NC-5] INCONSISTENT SOLIDITY VERSIONS

#### Description:

The project is compiled with different versions of Solidity, which is not recommended because it can lead to undefined behaviors.

It is better to use one Solidity compiler version across all contracts instead of different versions with different bugs and security checks.

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

### [NC-6] Use camel case for all functions, parameters and variables and snake case for constants

#### Description:

Use camel case for all functions, parameters and variables and snake case for constants.

#### **Proof Of Concept**

```solidity
File: contracts/lybra/configuration/LybraConfigurator.sol

33:     function get_dy_underlying(int128 i, int128 j, uint256 dx) external view returns (uint256);

34:     function exchange_underlying(int128 i, int128 j, uint256 dx, uint256 min_dy) external returns(uint256);

34:     function exchange_underlying(int128 i, int128 j, uint256 dx, uint256 min_dy) external returns(uint256);

```

```solidity
File: contracts/lybra/governance/LybraGovernance.sol

151:      function CLOCK_MODE() public override view returns (string memory){

156:     function COUNTING_MODE() public override view virtual returns (string memory){

```

### [NC-7] LARGE MULTIPLES OF TEN SHOULD USE SCIENTIFIC NOTATION

#### Description:

Use (e.g. 1e6) rather than decimal literals (e.g. 100000), for better code readability.

#### **Proof Of Concept**

```solidity
File: contracts/lybra/miner/EUSDMiningIncentives.sol

189:         return (stakedLBRLpValue(user) * 10000) / stakedOf(user) < 500;

210:             uint256 biddingFee = (reward * biddingFeeRatio) / 10000;

```

```solidity
File: contracts/lybra/miner/ProtocolRewardsPool.sol

134:         LBR.burn(msg.sender, (amount * grabFeeRatio) / 10000);

```

```solidity
File: contracts/lybra/pools/LybraStETHVault.sol

66:         uint256 payAmount = (((realAmount * getAssetPrice()) / 1e18) * getDutchAuctionDiscountPrice()) / 10000;

103:         if (time < 30 minutes) return 10000;

104:         return 10000 - (time / 30 minutes - 1) * 100;

```

```solidity
File: contracts/lybra/pools/base/LybraEUSDVaultBase.sol

239:         uint256 collateralAmount = (((eusdAmount * 1e18) / assetPrice) * (10000 - configurator.redemptionFee())) / 10000;

301:         return (poolTotalEUSDCirculation * configurator.vaultMintFeeApy(address(this)) * (block.timestamp - lastReportTime)) / (86400 * 365) / 10000;

```

```solidity
File: contracts/lybra/pools/base/LybraPeUSDVaultBase.sol

164:         uint256 collateralAmount = (((peusdAmount * 1e18) / assetPrice) * (10000 - configurator.redemptionFee())) / 10000;

238:         return (borrowed[user] * configurator.vaultMintFeeApy(address(this)) * (block.timestamp - feeUpdatedAt[user])) / (86400 * 365) / 10000;

```

### [NC-8] FOR EXTENDED “USING-FOR” USAGE, USE THE LATEST PRAGMA VERSION

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

### [NC-9] NO SAME VALUE INPUT CONTROL

#### **Proof Of Concept**

```solidity
File: contracts/lybra/miner/EUSDMiningIncentives.sol

85:         LBR = _lbr;

86:         esLBR = _eslbr;

97:         pools = _pools;

121:         duration = _duration;

```

```solidity
File: contracts/lybra/miner/stakerewardV2pool.sol

123:         duration = _duration;

```

### [NC-10] Add parameter to event-emit

#### Description:

Throughout the codebase, events are generally emitted when sensitive changes are made to the contracts. However, some events are missing important parameters.

#### **Proof Of Concept**

```solidity
File: contracts/lybra/configuration/LybraConfigurator.sol

189:         emit RedemptionFeeChanged(newFee);

255:         emit FlashloanFeeUpdated(fee);

```

### [NC-11] SOLIDITY COMPILER OPTIMIZATIONS CAN BE PROBLEMATIC

#### Description:

Protocol has enabled optional compiler optimizations in Solidity. There have been several optimization bugs with security implications. Moreover, optimizations are actively being developed. Solidity compiler optimizations are disabled by default, and it is unclear how many contracts in the wild actually use them.

Therefore, it is unclear how well they are being tested and exercised. High-severity security issues due to optimization bugs have occurred in the past. A high-severity bug in the emscripten-generated solc-js compiler used by Truffle and Remix persisted until late 2018. The fix for this bug was not reported in the Solidity CHANGELOG.

Another high-severity optimization bug resulting in incorrect bit shift results was patched in Solidity 0.5.6. More recently, another bug due to the incorrect caching of keccak256 was reported. A compiler audit of Solidity from November 2018 concluded that the optional optimizations may not be safe. It is likely that there are latent bugs related to optimization and that new bugs will be introduced due to future optimizations.

#### Exploit Scenario:

A latent or future bug in Solidity compiler optimizations—or in the Emscripten transpilation to solc-js—causes a security vulnerability in the contracts.

#### **Proof Of Concept**

```solidity
File: hardhat.config.ts

  solidity: {
    version: "0.8.17",
    settings: {
      optimizer: {
        enabled: true,
        runs: 200,
      },
    },
  },
```

#### Recommended Mitigation Steps

Short term, measure the gas savings from optimizations and carefully weigh them against the possibility of an optimization-related bug.

Long term, monitor the development and adoption of Solidity compiler optimizations to assess their maturity.

### [NC-12] USE A MORE RECENT VERSION OF SOLIDITY

#### Description:

For security, it is best practice to use the latest Solidity version. For the security fix list in the versions; https://github.com/ethereum/solidity/blob/develop/Changelog.md

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

### [NC-13] `require()` / `revert()` statements should have descriptive reason strings

#### **Proof Of Concept**

```solidity
File: contracts/lybra/miner/ProtocolRewardsPool.sol

228:         require(msg.sender == address(configurator));

```

```solidity
File: contracts/lybra/pools/LybraRETHVault.sol

42:         require(configurator.hasRole(keccak256("TIMELOCK"), msg.sender));

```

### [NC-14] FOR FUNCTIONS AND VARIABLES FOLLOW SOLIDITY STANDARD NAMING CONVENTIONS

#### Description:

Solidity’s standard naming convention for internal and private functions and variables (apart from constants): the mixedCase format starting with an underscore (\_mixedCase starting with an underscore)

Solidity’s standard naming convention for internal and private constants variables: the snake_case format starting with an underscore (\_mixedCase starting with an underscore) and use ALL_CAPS for naming them.

#### **Proof Of Concept**

```solidity
File: contracts/lybra/miner/ProtocolRewardsPool.sol

64:     function totalStaked() internal view returns (uint256) {

69:     function stakedOf(address staker) internal view returns (uint256) {

```

```solidity
File: contracts/lybra/pools/base/LybraEUSDVaultBase.sol

114:     function checkWithdrawal(address user, uint256 amount) internal view returns (uint256 withdrawal) {

```

### [NC-33] MINE - USE UNDERSCORES FOR NUMBER LITERALS / SHOWING THE ACTUAL VALUES OF NUMBERS IN NATSPEC COMMENTS MAKES CHECKING AND READING CODE EASIER

#### Description:

[code4arena example - USE UNDERSCORES FOR NUMBER LITERALS](https://code4rena.com/reports/2022-12-caviar/#n-07-use-underscores-for-number-literals)

[code4arena example - SHOWING THE ACTUAL VALUES OF NUMBERS IN NATSPEC COMMENTS MAKES CHECKING AND READING CODE EASIER](https://code4rena.com/reports/2022-12-caviar/#n-11-showing-the-actual-values-of-numbers-in-natspec-comments-makes-checking-and-reading-code-easier)

[code4arena example - USE UNDERSCORES FOR NUMBER LITERALS](https://code4rena.com/reports/2022-09-y2k-finance#non-critical-issues)

#### **Proof Of Concept**

```solidity
File: contracts/lybra/configuration/LybraConfigurator.sol

298:             if(!premiumTradingEnabled || price <= 1005000) {

```

```solidity
File: contracts/lybra/miner/EUSDMiningIncentives.sol

101:         require(_biddingRatio <= 8000, "BCE");

189:         return (stakedLBRLpValue(user) * 10000) / stakedOf(user) < 500;

```

```solidity
File: contracts/lybra/miner/ProtocolRewardsPool.sol

41:     uint256 public grabFeeRatio = 3000;

59:         require(_ratio <= 8000, "BCE");

```

```solidity
File: contracts/lybra/pools/LybraStETHVault.sol

66:         uint256 payAmount = (((realAmount * getAssetPrice()) / 1e18) * getDutchAuctionDiscountPrice()) / 10000;

103:         if (time < 30 minutes) return 10000;

```

```solidity
File: contracts/lybra/pools/base/LybraEUSDVaultBase.sol

115:         withdrawal = block.timestamp - 3 days >= depositedTime[user] ? amount : (amount * 999) / 1000;

239:         uint256 collateralAmount = (((eusdAmount * 1e18) / assetPrice) * (10000 - configurator.redemptionFee())) / 10000;

301:         return (poolTotalEUSDCirculation * configurator.vaultMintFeeApy(address(this)) * (block.timestamp - lastReportTime)) / (86400 * 365) / 10000;

```

```solidity
File: contracts/lybra/pools/base/LybraPeUSDVaultBase.sol

164:         uint256 collateralAmount = (((peusdAmount * 1e18) / assetPrice) * (10000 - configurator.redemptionFee())) / 10000;

238:         return (borrowed[user] * configurator.vaultMintFeeApy(address(this)) * (block.timestamp - feeUpdatedAt[user])) / (86400 * 365) / 10000;

```

### [NC-15] Functions not used internally could be marked external

#### **Proof Of Concept**

```solidity
File: contracts/lybra/governance/GovernanceTimelock.sol

25:     function checkRole(bytes32 role, address _sender) public view  returns(bool){

29:     function checkOnlyRole(bytes32 role, address _sender) public view  returns(bool){

```

```solidity
File: contracts/lybra/governance/LybraGovernance.sol

143:     function votingPeriod() public pure override returns (uint256){

147:      function votingDelay() public pure override returns (uint256){

151:      function CLOCK_MODE() public override view returns (string memory){

172:     function proposalThreshold() public pure override returns (uint256) {

```

```solidity
File: contracts/lybra/pools/base/LybraPeUSDVaultBase.sol

44:     function totalDepositedAsset() public view returns (uint256) {

257:     function getPoolTotalPeUSDCirculation() public view returns (uint256) {

```

```solidity
File: contracts/lybra/token/EUSD.sol

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
File: contracts/lybra/token/PeUSDMainnetStableVision.sol

129:     function executeFlashloan(FlashBorrower receiver, uint256 eusdAmount, bytes calldata data) public payable {

165:     function getAccruedEUSDInterest(

```

### [NC-16] VARIABLE NAMES THAT CONSIST OF ALL CAPITAL LETTERS SHOULD BE RESERVED FOR CONSTANT/IMMUTABLE VARIABLES

#### **Proof Of Concept**

```solidity
File: contracts/lybra/configuration/LybraConfigurator.sol

18: import "../interfaces/IEUSD.sol";

54:     IEUSD public EUSD;

55:     IEUSD public peUSD;

```

```solidity
File: contracts/lybra/miner/EUSDMiningIncentives.sol

14: import "../interfaces/IEUSD.sol";

32:     address public LBR;

```

```solidity
File: contracts/lybra/miner/ProtocolRewardsPool.sol

194:             IEUSD EUSD = IEUSD(configurator.getEUSDAddress());

```

```solidity
File: contracts/lybra/pools/base/LybraEUSDVaultBase.sol

18:     IEUSD public immutable EUSD;

19:     IERC20 public immutable collateralAsset;

```

```solidity
File: contracts/lybra/token/PeUSDMainnetStableVision.sol

30:     IEUSD public immutable EUSD;

```

## Low Issues

|      | Issue                                                                                 |
| ---- | :------------------------------------------------------------------------------------ |
| L-1  | DOS WITH BLOCK GAS LIMIT                                                              |
| L-2  | Avoid `transfer()`/`send()` as reentrancy mitigations                                 |
| L-3  | CRITICAL CHANGES SHOULD USE TWO-STEP PROCEDURE                                        |
| L-4  | DECODING AN IPFS HASH USING A FIXED HASH FUNCTION AND LENGTH OF THE HASH              |
| L-5  | LOSS OF PRECISION DUE TO ROUNDING                                                     |
| L-6  | UNIFY RETURN CRITERIA                                                                 |
| L-7  | REQUIRE MESSAGES ARE TOO SHORT AND UNCLEAR                                            |
| L-8  | Block values as a proxy for time                                                      |
| L-9  | USE `_SAFEMINT` INSTEAD OF `_MINT`                                                    |
| L-10 | USE `SAFETRANSFER` INSTEAD OF `TRANSFER`                                              |
| L-11 | SHOULD AN AIRDROP TOKEN ARRIVE ON THE LybraWstETHVault.sol CONTRACT, IT WILL BE STUCK |

### [L-1] AVOID HARDCODED VALUES

#### Description:

It is not good practice to hardcode values.

#### **Proof Of Concept**

```solidity
File: contracts/lybra/pools/base/LybraEUSDVaultBase.sol

21:     uint256 public immutable badCollateralRatio = 150 * 1e18;

```

### [L-2] Avoid `transfer()`/`send()` as reentrancy mitigations

#### Description:

Although `transfer()` and `send()` have been recommended as a security best-practice to prevent reentrancy attacks because they only forward 2300 gas, the gas repricing of opcodes may break deployed contracts. Use `call()` instead, without hardcoded gas limits along with checks-effects-interactions pattern or reentrancy guards for reentrancy protection.

#### **Proof Of Concept**

```solidity
File: contracts/lybra/configuration/LybraConfigurator.sol

292:             peUSD.transfer(address(lybraProtocolRewardsPool), peUSDBalance);

299:                 EUSD.transfer(address(lybraProtocolRewardsPool), balance);

304:                 IEUSD(stableToken).transfer(address(lybraProtocolRewardsPool), amount);

```

```solidity
File: contracts/lybra/miner/ProtocolRewardsPool.sol

202:                     peUSD.transfer(msg.sender, reward - eUSDShare);

206:                         peUSD.transfer(msg.sender, peUSDBalance);

210:                     token.transfer(msg.sender, tokenAmount);

```

```solidity
File: contracts/lybra/miner/stakerewardV2pool.sol

97:         stakingToken.transfer(msg.sender, _amount);

```

```solidity
File: contracts/lybra/pools/LybraStETHVault.sol

93:         collateralAsset.transfer(msg.sender, realAmount);

```

```solidity
File: contracts/lybra/pools/base/LybraEUSDVaultBase.sol

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

139:             collateralAsset.transfer(msg.sender, reducedAsset);

142:             collateralAsset.transfer(provider, reducedAsset - reward2keeper);

143:             collateralAsset.transfer(msg.sender, reward2keeper);

166:         collateralAsset.transfer(msg.sender, collateralAmount);

215:         collateralAsset.transfer(_onBehalfOf, _amount);

```

### [L-3] CRITICAL CHANGES SHOULD USE TWO-STEP PROCEDURE

#### Description:

The critical procedures should be two step process.

#### **Proof Of Concept**

```solidity
File: contracts/lybra/configuration/LybraConfigurator.sol

109:     function setMintVault(address pool, bool isActive) external onlyRole(DAO) {

119:     function setMintVaultMaxSupply(address pool, uint256 maxSupply) external onlyRole(DAO) {

126:     function setBadCollateralRatio(address pool, uint256 newRatio) external onlyRole(DAO) {

137:     function setProtocolRewardsPool(address addr) external checkRole(TIMELOCK) {

147:     function setEUSDMiningIncentives(address addr) external checkRole(TIMELOCK) {

158:     function setvaultBurnPaused(address pool, bool isActive) external checkRole(TIMELOCK) {

167:     function setPremiumTradingEnabled(bool isActive) external checkRole(TIMELOCK) {

177:     function setvaultMintPaused(address pool, bool isActive) external checkRole(ADMIN) {

186:     function setRedemptionFee(uint256 newFee) external checkRole(TIMELOCK) {

198:     function setSafeCollateralRatio(address pool, uint256 newRatio) external checkRole(TIMELOCK) {

213:     function setBorrowApy(address pool, uint256 newApy) external checkRole(TIMELOCK) {

224:     function setKeeperRatio(address pool,uint256 newRatio) external checkRole(TIMELOCK) {

235:     function setTokenMiner(address[] calldata _contracts, bool[] calldata _bools) external checkRole(TIMELOCK) {

246:     function setMaxStableRatio(uint256 _ratio) external checkRole(TIMELOCK) {

253:     function setFlashloanFee(uint256 fee) external checkRole(TIMELOCK) {

261:     function setProtocolRewardsToken(address _token) external checkRole(TIMELOCK) {

```

```solidity
File: contracts/lybra/miner/EUSDMiningIncentives.sol

84:     function setToken(address _lbr, address _eslbr) external onlyOwner {

89:     function setLBROracle(address _lbrOracle) external onlyOwner {

93:     function setPools(address[] memory _pools) external onlyOwner {

100:     function setBiddingCost(uint256 _biddingRatio) external onlyOwner {

105:     function setExtraRatio(uint256 ratio) external onlyOwner {

110:     function setPeUSDExtraRatio(uint256 ratio) external onlyOwner {

115:     function setBoost(address _boost) external onlyOwner {

119:     function setRewardsDuration(uint256 _duration) external onlyOwner {

124:     function setEthlbrStakeInfo(address _pool, address _lp) external onlyOwner {

128:     function setEUSDBuyoutAllowed(bool _bool) external onlyOwner {

```

```solidity
File: contracts/lybra/miner/ProtocolRewardsPool.sol

52:     function setTokenAddress(address _eslbr, address _lbr, address _boost) external onlyOwner {

58:     function setGrabCost(uint256 _ratio) external onlyOwner {

```

```solidity
File: contracts/lybra/miner/esLBRBoost.sol

38:     function setLockStatus(uint256 id) external {

```

```solidity
File: contracts/lybra/miner/stakerewardV2pool.sol

121:     function setRewardsDuration(uint256 _duration) external onlyOwner {

127:     function setBoost(address _boost) external onlyOwner {

```

```solidity
File: contracts/lybra/pools/LybraRETHVault.sol

41:     function setRkPool(address addr) external {

46:     function getAssetPrice() public override returns (uint256) {

```

```solidity
File: contracts/lybra/pools/LybraStETHVault.sol

25:     function setLidoRebaseTime(uint256 _time) external {

```

#### Recommended Mitigation Steps:

Lack of two-step procedure for critical operations leaves them error-prone. Consider adding two step procedure on the critical functions.

### [L-4] DECODING AN IPFS HASH USING A FIXED HASH FUNCTION AND LENGTH OF THE HASH

#### **Proof Of Concept**

```solidity
File: contracts/lybra/miner/EUSDMiningIncentives.sol

153:         uint256 etherInLp = (IEUSD(0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2).balanceOf(ethlbrLpToken) * uint(etherPrice)) / 1e8;

```

```solidity
File: contracts/lybra/pools/LybraRETHVault.sol

18:     IRkPool rkPool = IRkPool(0xDD3f50F8A6CafbE9b31a427582963f465E745AF8);

```

#### Recommended Mitigation Steps:

Consider using a more generic implementation that can handle different hash functions and lengths and allow the user to choose.

### [L-5] LOSS OF PRECISION DUE TO ROUNDING

#### **Proof Of Concept**

```solidity
File: contracts/lybra/configuration/LybraConfigurator.sol

303:                 uint256 amount = curvePool.exchange_underlying(0, 2, balance, balance * price * 998 / 1e21);

357:         return (EUSD.totalSupply() * maxStableRatio) / 10_000;

```

```solidity
File: contracts/lybra/miner/EUSDMiningIncentives.sol

142:                 borrowed = borrowed * (1e20 + peUSDExtraRatio) / 1e20;

153:         uint256 etherInLp = (IEUSD(0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2).balanceOf(ethlbrLpToken) * uint(etherPrice)) / 1e8;

154:         uint256 lbrInLp = (IEUSD(LBR).balanceOf(ethlbrLpToken) * uint(lbrPrice)) / 1e8;

156:         return (userStaked * (lbrInLp + etherInLp)) / totalLp;

168:         return rewardPerTokenStored + (rewardRatio * (lastTimeRewardApplicable() - updatedAt) * 1e18) / totalStaked();

185:         return ((stakedOf(_account) * getBoost(_account) * (rewardPerToken() - userRewardPerTokenPaid[_account])) / 1e38) + rewards[_account];

189:         return (stakedLBRLpValue(user) * 10000) / stakedOf(user) < 500;

210:             uint256 biddingFee = (reward * biddingFeeRatio) / 10000;

213:                 biddingFee = biddingFee * uint256(lbrPrice) / 1e8;

```

```solidity
File: contracts/lybra/miner/ProtocolRewardsPool.sol

152:         amount = (a * (75e18 - ((time2fullRedemption[user] - block.timestamp) * 70e18) / (exitCycle / 1 days - 3) / 1 days)) / 100e18;

209:                     uint256 tokenAmount = (reward - eUSDShare - peUSDBalance) * token.decimals() / 1e18;

233:             rewardPerTokenStored = rewardPerTokenStored + (share * 1e18) / totalStaked();

236:             rewardPerTokenStored = rewardPerTokenStored + (amount * 1e36 / token.decimals()) / totalStaked();

238:             rewardPerTokenStored = rewardPerTokenStored + (amount * 1e18) / totalStaked();

```

```solidity
File: contracts/lybra/miner/esLBRBoost.sol

67:             return ((boostEndTime - userUpdatedAt) * maxBoost) / (time - userUpdatedAt);

```

```solidity
File: contracts/lybra/miner/stakerewardV2pool.sol

79:         return rewardPerTokenStored + (rewardRatio * (lastTimeRewardApplicable() - updatedAt) * 1e18) / totalSupply;

107:         return ((balanceOf[_account] * getBoost(_account) * (rewardPerToken() - userRewardPerTokenPaid[_account])) / 1e38) + rewards[_account];

137:             rewardRatio = (_amount + remainingRewards) / duration;

```

```solidity
File: contracts/lybra/pools/LybraRETHVault.sol

47:         return (_etherPrice() * IRETH(address(collateralAsset)).getExchangeRatio()) / 1e18;

```

```solidity
File: contracts/lybra/pools/LybraStETHVault.sol

66:         uint256 payAmount = (((realAmount * getAssetPrice()) / 1e18) * getDutchAuctionDiscountPrice()) / 10000;

```

```solidity
File: contracts/lybra/pools/LybraWbETHVault.sol

35:         return (_etherPrice() * IWBETH(address(collateralAsset)).exchangeRatio()) / 1e18;

```

```solidity
File: contracts/lybra/pools/LybraWstETHVault.sol

49:         return (_etherPrice() * IWstETH(address(collateralAsset)).stEthPerToken()) / 1e18;

```

```solidity
File: contracts/lybra/pools/base/LybraEUSDVaultBase.sol

115:         withdrawal = block.timestamp - 3 days >= depositedTime[user] ? amount : (amount * 999) / 1000;

156:         uint256 onBehalfOfCollateralRatio = (depositedAsset[onBehalfOf] * assetPrice * 100) / borrowed[onBehalfOf];

161:         uint256 eusdAmount = (assetAmount * assetPrice) / 1e18;

164:         uint256 reducedAsset = (assetAmount * 11) / 10;

171:             reward2keeper = (reducedAsset * configurator.vaultKeeperRatio(address(this))) / 110;

189:         require((totalDepositedAsset * assetPrice * 100) / poolTotalEUSDCirculation < badCollateralRatio, "overallCollateralRatio should below 150%");

190:         uint256 onBehalfOfCollateralRatio = (depositedAsset[onBehalfOf] * assetPrice * 100) / borrowed[onBehalfOf];

193:         uint256 eusdAmount = (assetAmount * assetPrice) / 1e18;

195:             eusdAmount = (eusdAmount * 1e20) / onBehalfOfCollateralRatio;

205:             reward2keeper = ((assetAmount * configurator.vaultKeeperRatio(address(this))) * 1e18) / onBehalfOfCollateralRatio;

236:         uint256 providerCollateralRatio = (depositedAsset[provider] * assetPrice * 100) / borrowed[provider];

239:         uint256 collateralAmount = (((eusdAmount * 1e18) / assetPrice) * (10000 - configurator.redemptionFee())) / 10000;

292:         if (((depositedAsset[_user] * _assetPrice * 100) / borrowed[_user]) < configurator.getSafeCollateralRatio(address(this))) revert("collateralRatio is Below safeCollateralRatio");

301:         return (poolTotalEUSDCirculation * configurator.vaultMintFeeApy(address(this)) * (block.timestamp - lastReportTime)) / (86400 * 365) / 10000;

```

```solidity
File: contracts/lybra/pools/base/LybraPeUSDVaultBase.sol

127:         uint256 onBehalfOfCollateralRatio = (depositedAsset[onBehalfOf] * assetPrice * 100) / getBorrowedOf(onBehalfOf);

132:         uint256 peusdAmount = (assetAmount * assetPrice) / 1e18;

135:         uint256 reducedAsset = (assetAmount * 11) / 10;

141:             reward2keeper = (reducedAsset * configurator.vaultKeeperRatio(address(this))) / 110;

161:         uint256 providerCollateralRatio = (depositedAsset[provider] * assetPrice * 100) / borrowed[provider];

164:         uint256 collateralAmount = (((peusdAmount * 1e18) / assetPrice) * (10000 - configurator.redemptionFee())) / 10000;

226:         if (((depositedAsset[user] * price * 100) / getBorrowedOf(user)) < configurator.getSafeCollateralRatio(address(this)))

238:         return (borrowed[user] * configurator.vaultMintFeeApy(address(this)) * (block.timestamp - feeUpdatedAt[user])) / (86400 * 365) / 10000;

```

```solidity
File: contracts/lybra/token/PeUSDMainnetStableVision.sol

117:         uint256 share = (userConvertInfo[msg.sender].depositedEUSDShares * peusdAmount) / userConvertInfo[msg.sender].mintedPeUSD;

157:         return (share * configurator.flashloanFee()) / 10_000;

```

### [L-6] UNIFY RETURN CRITERIA

#### Description:

In contracts, sometimes the name of the return variable is not defined and sometimes is, unifying the way of writing the code makes the code more uniform and readable.

#### **Proof Of Concept**

```solidity
File: contracts/lybra/governance/LybraGovernance.sol

51:     function quorum(uint256 timepoint) public view override returns (uint256){

59:     function _quorumReached(uint256 proposalId) internal view override returns (bool){

66:     function _voteSucceeded(uint256 proposalId) internal view override returns (bool){

98:      function _getVotes(address account, uint256 timepoint, bytes memory) internal view override returns (uint256){

112:     function proposals(uint256 proposalId) external view returns (uint256 id, address proposer, uint256 eta, uint256 startBlock, uint256 endBlock, uint256 forVotes, uint256 againstVotes, uint256 abstainVotes, bool canceled, bool executed) {

129:     function getReceipt(uint256 proposalId, address account) external view returns (bool voted, uint8 support, uint256 votes){

143:     function votingPeriod() public pure override returns (uint256){

147:      function votingDelay() public pure override returns (uint256){

151:      function CLOCK_MODE() public override view returns (string memory){

156:     function COUNTING_MODE() public override view virtual returns (string memory){

161:     function clock() public override view returns (uint48){

165:     function hasVoted(uint256 proposalId, address account) public override view virtual returns (bool){

172:     function proposalThreshold() public pure override returns (uint256) {

179:     function supportsInterface(bytes4 interfaceId) public view virtual override(GovernorTimelockControl) returns (bool) {

```

```solidity
File: contracts/lybra/miner/ProtocolRewardsPool.sol

21:     ) external view returns (uint256 unlockTime);

64:     function totalStaked() internal view returns (uint256) {

69:     function stakedOf(address staker) internal view returns (uint256) {

149:     function getPreUnlockableAmount(address user) public view returns (uint256 amount) {

155:     function getClaimAbleLBR(address user) public view returns (uint256 amount) {

161:     function getReservedLBRForVesting(address user) public view returns (uint256 amount) {

167:     function earned(address _account) public view returns (uint) {

171:     function getClaimAbleUSD(address user) external view returns (uint256 amount) {

```

```solidity
File: contracts/lybra/miner/esLBRBoost.sol

48:     function getUnlockTime(address user) external view returns (uint256 unlockTime) {

57:     function getUserBoost(address user, uint256 userUpdatedAt, uint256 finishAt) external view returns (uint256) {

```

```solidity
File: contracts/lybra/pools/LybraStETHVault.sol

9:     function submit(address _referral) external payable returns (uint256 StETH);

101:     function getDutchAuctionDiscountPrice() public view returns (uint256) {

107:     function getAssetPrice() public override returns (uint256) {

```

```solidity
File: contracts/lybra/pools/LybraWstETHVault.sol

10:     function stEthPerToken() external view returns (uint256);

12:     function wrap(uint256 _stETHAmount) external returns (uint256);

16:     function submit(address _referral) external payable returns (uint256 StETH);

18:     function approve(address spender, uint256 amount) external returns (bool);

48:     function getAssetPrice() public override returns (uint256) {

```

```solidity
File: contracts/lybra/pools/base/LybraEUSDVaultBase.sol

14:     function fetchPrice() external returns (uint256);

114:     function checkWithdrawal(address user, uint256 amount) internal view returns (uint256 withdrawal) {

300:     function _newFee() internal view returns (uint256) {

307:     function _etherPrice() internal returns (uint256) {

311:     function getBorrowedOf(address user) external view returns (uint256) {

315:     function getPoolTotalEUSDCirculation() external view returns (uint256) {

319:     function getAsset() external view virtual returns (address) {

323:     function getVaultType() external pure returns (uint8) {

327:     function getAssetPrice() public virtual returns (uint256);

```

```solidity
File: contracts/lybra/token/EUSD.sol

99:     function name() public pure returns (string memory) {

107:     function symbol() public pure returns (string memory) {

114:     function decimals() public pure returns (uint8) {

124:     function totalSupply() public view returns (uint256) {

134:     function balanceOf(address _account) public view returns (uint256) {

153:     function transfer(address _recipient, uint256 _amount) public returns (bool) {

165:     function allowance(address _owner, address _spender) public view returns (uint256) {

200:     function approve(address _spender, uint256 _amount) public returns (bool) {

226:     function transferFrom(address from, address to, uint256 amount) public returns (bool) {

248:     function increaseAllowance(address _spender, uint256 _addedValue) public returns (bool) {

268:     function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {

285:     function getTotalShares() public view returns (uint256) {

292:     function sharesOf(address _account) public view returns (uint256) {

299:     function getSharesByMintedEUSD(uint256 _EUSDAmount) public view returns (uint256) {

311:     function getMintedEUSDByShares(uint256 _sharesAmount) public view returns (uint256) {

334:     function transferShares(address _recipient, uint256 _sharesAmount) public returns (uint256) {

377:     function _sharesOf(address _account) internal view returns (uint256) {

411:     function mint(address _recipient, uint256 _mintAmount) external onlyMintVault MintPaused returns (uint256 newTotalShares) {

440:     function burn(address _account, uint256 _burnAmount) external onlyMintVault BurnPaused returns (uint256 newTotalShares) {

459:     function burnShares(address _account, uint256 _sharesAmount) external onlyMintVault BurnPaused returns (uint256 newTotalShares) {

464:     function _onlyBurnShares(address _account, uint256 _sharesAmount) private returns (uint256 newTotalShares) {

```

```solidity
File: contracts/lybra/token/PeUSDMainnetStableVision.sol

63:     function mint(address to, uint256 amount) external onlyMintVault MintPaused returns (bool) {

69:     function burn(address account, uint256 amount) external onlyMintVault BurnPaused returns (bool) {

141:     function transferFrom(address from, address to, uint256 amount) public override returns (bool) {

156:     function getFee(uint256 share) public view returns (uint256) {

167:     ) public view returns (uint256 eusdAmount) {

171:     function circulatingSupply() public view virtual override returns (uint) {

175:     function token() public view virtual override returns (address) {

184:     function _debitFrom(address _from, uint16, bytes32, uint _amount) internal virtual override returns (uint) {

191:     function _creditTo(uint16, address _toAddress, uint _amount) internal virtual override returns (uint) {

196:     function _transferFrom(address _from, address _to, uint _amount) internal virtual override returns (uint) {

205:     function _ld2sdRatio() internal view virtual override returns (uint) {

```

#### Recommended Mitigation Steps:

add `{return x}` if you want to return the updated value or else remove `returns(uint)` from the `function(){}` if no value you wanted to return

### [L-7] REQUIRE MESSAGES ARE TOO SHORT AND UNCLEAR

#### Description:

The correct and clear error description explains to the user why the function reverts, but the error descriptions below in the project are not self-explanatory. These error descriptions are very important in the debug features of DApps like Tenderly. Error definitions should be added to the require block, not exceeding 32 bytes.

#### **Proof Of Concept**

```solidity
File: contracts/lybra/miner/ProtocolRewardsPool.sol

228:         require(msg.sender == address(configurator));

```

```solidity
File: contracts/lybra/pools/LybraRETHVault.sol

42:         require(configurator.hasRole(keccak256("TIMELOCK"), msg.sender));

```

```solidity
File: contracts/lybra/pools/base/LybraPeUSDVaultBase.sol

62:         require(collateralAsset.balanceOf(address(this)) >= preBalance + assetAmount, "");

```

#### Recommended Mitigation Steps:

Error definitions should be added to the require block, not exceeding 32 bytes or we should use custom errors

### [L-8] Block values as a proxy for time

#### Description:

Contracts often need access to time values to perform certain types of functionality. Values such as block.timestamp, and block.number can give you a sense of the current time or a time delta, however, they are not safe to use for most purposes.

In the case of block.timestamp, developers often attempt to use it to trigger time-dependent events. As Ethereum is decentralized, nodes can synchronize time only to some degree. Moreover, malicious miners can alter the timestamp of their blocks, especially if they can gain advantages by doing so. However, miners cant set a timestamp smaller than the previous one (otherwise the block will be rejected), nor can they set the timestamp too far ahead in the future. Taking all of the above into consideration, developers cant rely on the preciseness of the provided timestamp.

As for block.number, considering the block time on Ethereum is generally about 14 seconds, it`s possible to predict the time delta between blocks. However, block times are not constant and are subject to change for a variety of reasons, e.g. fork reorganisations and the difficulty bomb. Due to variable block times, block.number should also not be relied on for precise calculations of time.
Reference: https://swcregistry.io/docs/SWC-116

Reference: (https://github.com/kadenzipfel/smart-contract-vulnerabilities/blob/master/vulnerabilities/timestamp-dependence.md)

#### **Proof Of Concept**

```solidity
File: contracts/lybra/governance/LybraGovernance.sol

152:        require(clock() == block.number, "Votes: broken clock mode");

```

```solidity
File: contracts/lybra/miner/EUSDMiningIncentives.sol

79:             userUpdatedAt[_account] = block.timestamp;

120:         require(finishAt < block.timestamp, "reward duration not finished");

160:         return _min(finishAt, block.timestamp);

230:         if (block.timestamp >= finishAt) {

233:             uint256 remainingRewards = (finishAt - block.timestamp) * rewardRatio;

239:         finishAt = block.timestamp + duration;

240:         updatedAt = block.timestamp;

```

```solidity
File: contracts/lybra/miner/ProtocolRewardsPool.sol

88:         require(block.timestamp >= esLBRBoost.getUnlockTime(msg.sender), "Your lock-in period has not ended. You can't convert your esLBR now.");

92:         if (time2fullRedemption[msg.sender] > block.timestamp) {

93:             total += unstakeRatio[msg.sender] * (time2fullRedemption[msg.sender] - block.timestamp);

96:         time2fullRedemption[msg.sender] = block.timestamp + exitCycle;

105:         lastWithdrawTime[user] = block.timestamp;

114:         require(block.timestamp + exitCycle - 3 days > time2fullRedemption[msg.sender], "ENW");

152:         amount = (a * (75e18 - ((time2fullRedemption[user] - block.timestamp) * 70e18) / (exitCycle / 1 days - 3) / 1 days)) / 100e18;

157:             amount = block.timestamp > time2fullRedemption[user] ? unstakeRatio[user] * (time2fullRedemption[user] - lastWithdrawTime[user]) : unstakeRatio[user] * (block.timestamp - lastWithdrawTime[user]);

157:             amount = block.timestamp > time2fullRedemption[user] ? unstakeRatio[user] * (time2fullRedemption[user] - lastWithdrawTime[user]) : unstakeRatio[user] * (block.timestamp - lastWithdrawTime[user]);

162:         if (time2fullRedemption[user] > block.timestamp) {

163:             amount = unstakeRatio[user] * (time2fullRedemption[user] - block.timestamp);

```

```solidity
File: contracts/lybra/miner/esLBRBoost.sol

41:         if (userStatus.unlockTime > block.timestamp) {

44:         userLockStatus[msg.sender] = LockStatus(block.timestamp + _setting.duration, _setting.duration, _setting.miningBoost);

63:         if (finishAt <= boostEndTime || block.timestamp <= boostEndTime) {

66:             uint256 time = block.timestamp > finishAt ? finishAt : block.timestamp;

66:             uint256 time = block.timestamp > finishAt ? finishAt : block.timestamp;

```

```solidity
File: contracts/lybra/miner/stakerewardV2pool.sol

63:             userUpdatedAt[_account] = block.timestamp;

70:         return _min(finishAt, block.timestamp);

122:         require(finishAt < block.timestamp, "reward duration not finished");

133:         if (block.timestamp >= finishAt) {

136:             uint256 remainingRewards = (finishAt - block.timestamp) * rewardRatio;

142:         finishAt = block.timestamp + duration;

143:         updatedAt = block.timestamp;

```

```solidity
File: contracts/lybra/pools/LybraStETHVault.sol

45:         depositedTime[msg.sender] = block.timestamp;

92:         lastReportTime = block.timestamp;

102:         uint256 time = (block.timestamp - lidoRebaseTime) % 1 days;

```

```solidity
File: contracts/lybra/pools/base/LybraEUSDVaultBase.sol

80:         depositedTime[msg.sender] = block.timestamp;

115:         withdrawal = block.timestamp - 3 days >= depositedTime[user] ? amount : (amount * 999) / 1000;

297:         lastReportTime = block.timestamp;

301:         return (poolTotalEUSDCirculation * configurator.vaultMintFeeApy(address(this)) * (block.timestamp - lastReportTime)) / (86400 * 365) / 10000;

```

```solidity
File: contracts/lybra/pools/base/LybraPeUSDVaultBase.sol

231:         if (block.timestamp > feeUpdatedAt[user]) {

233:             feeUpdatedAt[user] = block.timestamp;

238:         return (borrowed[user] * configurator.vaultMintFeeApy(address(this)) * (block.timestamp - feeUpdatedAt[user])) / (86400 * 365) / 10000;

```

### [L-9] USE `_SAFEMINT` INSTEAD OF `_MINT`

#### Description:

According to openzepplin’s ERC721, the use of `_mint` is discouraged, use `safeMint` whenever possible.

https://docs.openzeppelin.com/contracts/3.x/api/token/erc721#ERC721-mint-address-uint256-

#### **Proof Of Concept**

```solidity
File: contracts/lybra/token/LBR.sol

28:         _mint(user, amount);

57:         _mint(_toAddress, _amount);

```

```solidity
File: contracts/lybra/token/PeUSD.sol

39:         _mint(_toAddress, _amount);

```

```solidity
File: contracts/lybra/token/PeUSDMainnetStableVision.sol

65:         _mint(to, amount);

87:         _mint(user, eusdAmount);

192:         _mint(_toAddress, _amount);

```

```solidity
File: contracts/lybra/token/esLBR.sol

34:         _mint(user, amount);

```

#### Recommended Mitigation Steps:

Use `_safeMint` whenever possible instead of `_mint`

### [L-10] USE `SAFETRANSFER` INSTEAD OF `TRANSFER`

#### Description:

It is good to add a `require()` statement that checks the return value of token transfers or to use something like OpenZeppelin’s `safeTransfer`/`safeTransferFrom` unless one is sure the given token reverts in case of a failure. Failure to do so will cause silent failures of transfers and affect token accounting in contract.

For example, Some tokens do not implement the ERC20 standard properly but are still accepted by most code that accepts ERC20 tokens. For example Tether (USDT)‘s transfer() and transferFrom() functions do not return booleans as the specification requires, and instead have no return value. When these sorts of tokens are cast to IERC20, their function signatures do not match and therefore the calls made, revert.

#### **Proof Of Concept**

```solidity
File: contracts/lybra/configuration/LybraConfigurator.sol

292:             peUSD.transfer(address(lybraProtocolRewardsPool), peUSDBalance);

299:                 EUSD.transfer(address(lybraProtocolRewardsPool), balance);

304:                 IEUSD(stableToken).transfer(address(lybraProtocolRewardsPool), amount);

```

```solidity
File: contracts/lybra/miner/ProtocolRewardsPool.sol

202:                     peUSD.transfer(msg.sender, reward - eUSDShare);

206:                         peUSD.transfer(msg.sender, peUSDBalance);

210:                     token.transfer(msg.sender, tokenAmount);

```

```solidity
File: contracts/lybra/miner/stakerewardV2pool.sol

97:         stakingToken.transfer(msg.sender, _amount);

```

```solidity
File: contracts/lybra/pools/LybraStETHVault.sol

93:         collateralAsset.transfer(msg.sender, realAmount);

```

```solidity
File: contracts/lybra/pools/base/LybraEUSDVaultBase.sol

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

139:             collateralAsset.transfer(msg.sender, reducedAsset);

142:             collateralAsset.transfer(provider, reducedAsset - reward2keeper);

143:             collateralAsset.transfer(msg.sender, reward2keeper);

166:         collateralAsset.transfer(msg.sender, collateralAmount);

215:         collateralAsset.transfer(_onBehalfOf, _amount);

```

```solidity
File: contracts/lybra/token/EUSD.sol

153:     function transfer(address _recipient, uint256 _amount) public returns (bool) {

155:         _transfer(owner, _recipient, _amount);

231:         _transfer(from, to, amount);

348:     function _transfer(address _sender, address _recipient, uint256 _amount) internal {

```

```solidity
File: contracts/lybra/token/LBR.sol

66:         _transfer(_from, _to, _amount);

```

```solidity
File: contracts/lybra/token/PeUSD.sol

48:         _transfer(_from, _to, _amount);

```

```solidity
File: contracts/lybra/token/PeUSDMainnetStableVision.sol

146:         _transfer(from, to, amount);

201:         _transfer(_from, _to, _amount);

```

```solidity
File: contracts/lybra/token/esLBR.sol

26:     function _transfer(address, address, uint256) internal virtual override {

```

#### Recommended Mitigation Steps:

Consider using `safeTransfer`/`safeTransferFrom` or `require()` consistently.

### [L-11] SHOULD AN AIRDROP TOKEN ARRIVE ON THE LybraWstETHVault.sol CONTRACT, IT WILL BE STUCK

#### Description:

With the wrap() function, tokens are transferred to the contract and in case of airdrop due to these tokens, it will be stuck in the contract as there is no function to take these airdrop tokens from the contract.

A common method for airdrops is to collect airdrops with claim, so the Pair.sol contract can be considered upgradagable, adding a function to make claim.

#### **Proof Of Concept**

```solidity
File: contracts/lybra/pools/LybraWstETHVault.sol

12:     function wrap(uint256 _stETHAmount) external returns (uint256);

37:         uint256 wstETHAmount = IWstETH(address(collateralAsset)).wrap(msg.value);

```
