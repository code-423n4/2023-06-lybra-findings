## QA
---

### Layout Order [1]

- The best-practices for layout within a contract is the following order: state variables, events, modifiers, constructor and functions.

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol

```solidity
72:    modifier updateReward(address _account) {
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol

```solidity
// modifier should come before constructor
178:    modifier updateReward(address account) {
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol

```solidity
// modifier coming after constructor
56:    modifier updateReward(address _account) {
```

---

### Function Visibility [2]

- Order of Functions: Ordering helps readers identify which functions they can call and to find the constructor and fallback definitions easier. Functions should be grouped according to their visibility and ordered: constructor, receive function (if exists), fallback function (if exists), public, external, internal, private. Within a grouping, place the view and pure functions last.

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol

```solidity
// internal function coming before some public and external ones
173:    function _mintPeUSD(address _provider, address _onBehalfOf, uint256 _mintAmount, uint256 _assetPrice) internal virtual {
192:    function _repay(address _provider, address _onBehalfOf, uint256 _amount) internal virtual {
212:    function _withdraw(address _provider, address _onBehalfOf, uint256 _amount) internal {
225:    function _checkHealth(address user, uint256 price) internal view {
230:    function _updateFee(address user) internal {
237:    function _newFee(address user) internal view returns (uint256) {
244:    function _etherPrice() internal returns (uint256) {

// public functions coming after internal and external ones
253:    function getBorrowedOf(address user) public view returns (uint256) {
257:    function getPoolTotalPeUSDCirculation() public view returns (uint256) {
269:    function getAssetPrice() public virtual returns (uint256);
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol


```solidity
// internal function coming before public and external ones
114:    function checkWithdrawal(address user, uint256 amount) internal view returns (uint256 withdrawal) {
259:    function _mintEUSD(address _provider, address _onBehalfOf, uint256 _mintAmount, uint256 _assetPrice) internal virtual {
276:    function _repay(address _provider, address _onBehalfOf, uint256 _amount) internal virtual {
291:    function _checkHealth(address _user, uint256 _assetPrice) internal view {
295:    function _saveReport() internal {
300:    function _newFee() internal view returns (uint256) {
307:    function _etherPrice() internal returns (uint256) {

// public function coming after external and internal functions
327:    function getAssetPrice() public virtual returns (uint256);
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol

```solidity
// internal function coming before public and external ones
132:    function totalStaked() internal view returns (uint256) {

// public functions coming after external ones
136:    function stakedOf(address user) public view returns (uint256) {
149:    function stakedLBRLpValue(address user) public view returns (uint256) {
159:    function lastTimeRewardApplicable() public view returns (uint256) {
163:    function rewardPerToken() public view returns (uint256) {
176:    function getBoost(address _account) public view returns (uint256) {
184:    function earned(address _account) public view returns (uint256) {
188:    function isOtherEarningsClaimable(address user) public view returns (bool) {
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol

```solidity
// modifier coming after constructor
85:    modifier onlyRole(bytes32 role) {
90:    modifier checkRole(bytes32 role) {
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol

```solidity
// internal functions coming before public and external ones
64:    function totalStaked() internal view returns (uint256) {
69:    function stakedOf(address staker) internal view returns (uint256) {

// public functions coming after external and internal ones
100:    function withdraw(address user) public {
149:    function getPreUnlockableAmount(address user) public view returns (uint256 amount) {
155:    function getClaimAbleLBR(address user) public view returns (uint256 amount) {
161:    function getReservedLBRForVesting(address user) public view returns (uint256 amount) {
167:    function earned(address _account) public view returns (uint) {
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol

```solidity
// public coming after external functions
79:    function convertToPeUSD(address user, uint256 eusdAmount) public {
129:    function executeFlashloan(FlashBorrower receiver, uint256 eusdAmount, bytes calldata data) public payable {
141:    function transferFrom(address from, address to, uint256 amount) public override returns (bool) {
156:    function getFee(uint256 share) public view returns (uint256) {
165:    function getAccruedEUSDInterest(
171:    function circulatingSupply() public view virtual override returns (uint) {
175:    function token() public view virtual override returns (address) {
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol


```solidity
// internal function coming before external and public ones
59:    function _quorumReached(uint256 proposalId) internal view override returns (bool){
66:    function _voteSucceeded(uint256 proposalId) internal view override returns (bool){
76:    function _countVote(uint256 proposalId, address account, uint8 support, uint256 weight, bytes memory) internal override {
98:     function _getVotes(address account, uint256 timepoint, bytes memory) internal view override returns (uint256){
106:    function _execute(uint256 /* proposalId */, address[] memory targets, uint256[] memory values, bytes[] memory calldatas, bytes32 descriptionHash) internal virtual override {

// public functions coming after some external and internal ones.
143:    function votingPeriod() public pure override returns (uint256){
147:     function votingDelay() public pure override returns (uint256){
151:     function CLOCK_MODE() public override view returns (string memory){
156:    function COUNTING_MODE() public override view virtual returns (string memory){
161:    function clock() public override view returns (uint48){
165:    function hasVoted(uint256 proposalId, address account) public override view virtual returns (bool){
172:    function proposalThreshold() public pure override returns (uint256) {
179:    function supportsInterface(bytes4 interfaceId) public view virtual override(GovernorTimelockControl) returns (bool) {
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol

```solidity
// external functions coming before some public ones
83:    function stake(uint256 _amount) external updateReward(msg.sender) {
93:    function withdraw(uint256 _amount) external updateReward(msg.sender) {
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol

```solidity
// public functions coming after external ones
101:    function getDutchAuctionDiscountPrice() public view returns (uint256) {
107:    function getAssetPrice() public override returns (uint256) {
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraRETHVault.sol

```solidity
// public function coming after external ones
46:    function getAssetPrice() public override returns (uint256) {
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWstETHVault.sol

```solidity
// external view coming before non external view
10:    function stEthPerToken() external view returns (uint256);

// public function coming after external function
48:    function getAssetPrice() public override returns (uint256) {
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/esLBR.sol

```solidity
// internal function coming before external ones.
26:    function _transfer(address, address, uint256) internal virtual override {
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWbETHVault.sol

```solidity
// view coming after non-view external
10:    function exchangeRatio() external view returns (uint256);

// external coming before public function
20:    function depositEtherToMint(uint256 mintAmount) external payable override {
```

---

### natSpec missing [3]

Some functions are missing @params or @returns. Specification Format.” These are written with a triple slash (///) or a double asterisk block(/** ... */) directly above function declarations or statements to generate documentation in JSON format for developers and end-users. It is recommended that Solidity contracts are fully annotated using NatSpec for all public interfaces (everything in the ABI). These comments contain different types of tags:
- @title: A title that should describe the contract/interface @author: The name of the author (for contract, interface) 
- @notice: Explain to an end user what this does (for contract, interface, function, public state variable, event) 
- @dev: Explain to a developer any extra details (for contract, interface, function, state variable, event) 
- @param: Documents a parameter (just like in doxygen) and must be followed by parameter name (for function, event)
- @return: Documents the return variables of a contract’s function (function, public state variable)
- @inheritdoc: Copies all missing tags from the base function and must be followed by the contract name (for function, public state variable)
- @custom…: Custom tag, semantics is application-defined (for everywhere)

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol

```solidity
9:  interface IPriceFeed {
10:    function fetchPrice() external returns (uint256);
13: abstract contract LybraPeUSDVaultBase {
26:    event DepositEther(address indexed onBehalfOf, address asset, uint256 etherAmount, uint256 assetAmount, uint256 timestamp);
28:    event DepositAsset(address indexed onBehalfOf, address asset, uint256 amount, uint256 timestamp);
29:    event WithdrawAsset(address sponsor, address indexed onBehalfOf, address asset, uint256 amount, uint256 timestamp);
30:    event Mint(address sponsor, address indexed onBehalfOf, uint256 amount, uint256 timestamp);
31:    event Burn(address sponsor, address indexed onBehalfOf, uint256 amount, uint256 timestamp);
32:    event LiquidationRecord(address provider, address keeper, address indexed onBehalfOf, uint256 eusdamount, uint256 LiquidateAssetAmount, uint256 keeperReward, bool superLiquidation, uint256 timestamp);
34:    event RigidRedemption(address indexed caller, address indexed provider, uint256 peusdAmount, uint256 assetAmount, uint256 timestamp);
35:    event FeeDistribution(address indexed feeAddress, uint256 feeAmount, uint256 timestamp);
37:    constructor(address _peusd, address _etherOracle, address _collateral, address _configurator) {
44:    function totalDepositedAsset() public view returns (uint256) {
48:    function depositEtherToMint(uint256 mintAmount) external payable virtual;
58:    function depositAssetToMint(uint256 assetAmount, uint256 mintAmount) external virtual {
82:    function withdraw(address onBehalfOf, uint256 amount) external virtual {
96:    function mint(address onBehalfOf, uint256 amount) external virtual {
110:    function burn(address onBehalfOf, uint256 amount) external virtual {
125:    function liquidation(address provider, address onBehalfOf, uint256 assetAmount) external virtual {
157:    function rigidRedemption(address provider, uint256 peusdAmount) external virtual {
173:    function _mintPeUSD(address _provider, address _onBehalfOf, uint256 _mintAmount, uint256 _assetPrice) internal virtual {
192:    function _repay(address _provider, address _onBehalfOf, uint256 _amount) internal virtual {
212:    function _withdraw(address _provider, address _onBehalfOf, uint256 _amount) internal {
225:    function _checkHealth(address user, uint256 price) internal view {
230:    function _updateFee(address user) internal {
237:    function _newFee(address user) internal view returns (uint256) {
244:    function _etherPrice() internal returns (uint256) {
253:    function getBorrowedOf(address user) public view returns (uint256) {
257:    function getPoolTotalPeUSDCirculation() public view returns (uint256) {
261:    function getAsset() external view returns (address) {
265:    function getVaultType() external pure returns (uint8) {
269:    function getAssetPrice() public virtual returns (uint256);
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol

```solidity
9:  interface LbrStakingPool {
10:    function notifyRewardAmount(uint256 amount) external;

13: interface IPriceFeed {
14:    function fetchPrice() external returns (uint256);

17: abstract contract LybraEUSDVaultBase {
34:    event DepositEther(address indexed onBehalfOf, address asset, uint256 etherAmount, uint256 assetAmount, uint256 timestamp);
36:    event DepositAsset(address indexed onBehalfOf, address asset, uint256 amount, uint256 timestamp);
38:    event WithdrawAsset(address sponsor, address asset, address indexed onBehalfOf, uint256 amount, uint256 timestamp);
39:    event Mint(address sponsor, address indexed onBehalfOf, uint256 amount, uint256 timestamp);
40:    event Burn(address sponsor, address indexed onBehalfOf, uint256 amount, uint256 timestamp);
41:    event LiquidationRecord(address provider, address keeper, address indexed onBehalfOf, uint256 eusdamount, uint256 liquidateEtherAmount, uint256 keeperReward, bool superLiquidation, uint256 timestamp);
42:    event LSDValueCaptured(uint256 stETHAdded, uint256 payoutEUSD, uint256 discountRate, uint256 timestamp);
43:    event RigidRedemption(address indexed caller, address indexed provider, uint256 eusdAmount, uint256 collateralAmount, uint256 timestamp);
44:    event FeeDistribution(address indexed feeAddress, uint256 feeAmount, uint256 timestamp);
46:    constructor(address _collateralAsset, address _etherOracle, address _configurator) {
114:    function checkWithdrawal(address user, uint256 amount) internal view returns (uint256 withdrawal) {

// @param missing
62    function depositEtherToMint(uint256 mintAmount) external payable virtual;
72:    function depositAssetToMint(uint256 assetAmount, uint256 mintAmount) external virtual {
98:    function withdraw(address onBehalfOf, uint256 amount) external virtual {
126:    function mint(address onBehalfOf, uint256 amount) external {
140:    function burn(address onBehalfOf, uint256 amount) external {
154:    function liquidation(address provider, address onBehalfOf, uint256 assetAmount) external virtual {
187:    function superLiquidation(address provider, address onBehalfOf, uint256 assetAmount) external virtual {
221:    function excessIncomeDistribution(uint256 payAmount) external virtual;
232:    function rigidRedemption(address provider, uint256 eusdAmount) external virtual {
276:    function _repay(address _provider, address _onBehalfOf, uint256 _amount) internal virtual {
291:    function _checkHealth(address _user, uint256 _assetPrice) internal view {

295:    function _saveReport() internal {
300:    function _newFee() internal view returns (uint256) {
311:    function getBorrowedOf(address user) external view returns (uint256) {
315:    function getPoolTotalEUSDCirculation() external view returns (uint256) {
319:    function getAsset() external view virtual returns (address) {
323:    function getVaultType() external pure returns (uint8) {
327:    function getAssetPrice() public virtual returns (uint256);

// @return missing
307:    function _etherPrice() internal returns (uint256) {

```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol

```solidity
19: interface IesLBRBoost {
20:    function getUserBoost(
27: contract EUSDMiningIncentives is Ownable {
59:    event ClaimReward(address indexed user, uint256 amount, uint256 time);
60:    event ClaimedOtherEarnings(address indexed user, address indexed Victim, uint256 buyAmount, uint256 biddingFee, bool useEUSD, uint256 time);
61:    event NotifyRewardChanged(uint256 addAmount, uint256 time);
64:    constructor(address _config, address _boost, address _etherOracle, address _lbrOracle) {
72:    modifier updateReward(address _account) {
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
132:    function totalStaked() internal view returns (uint256) {
136:    function stakedOf(address user) public view returns (uint256) {
149:    function stakedLBRLpValue(address user) public view returns (uint256) {
159:    function lastTimeRewardApplicable() public view returns (uint256) {
163:    function rewardPerToken() public view returns (uint256) {
174:    function refreshReward(address _account) external updateReward(_account) {}
176:    function getBoost(address _account) public view returns (uint256) {
184:    function earned(address _account) public view returns (uint256) {
188:    function isOtherEarningsClaimable(address user) public view returns (bool) {
192:    function getReward() external updateReward(msg.sender) {
202:    function purchaseOtherEarnings(address user, bool useEUSD) external updateReward(user) {
226:    function notifyRewardAmount(
244:    function _min(uint256 x, uint256 y) private pure returns (uint256) {
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol

```solidity
20: interface IProtocolRewardsPool {
21:    function notifyRewardAmount(uint256 amount, uint256 tokenType) external;

24: interface IeUSDMiningIncentives {
25:    function refreshReward(address user) external;

28: interface IVault {
29:    function vaultType() external view returns (uint8);

32: interface ICurvePool{
33:    function get_dy_underlying(int128 i, int128 j, uint256 dx) external view returns (uint256);
34:    function exchange_underlying(int128 i, int128 j, uint256 dx, uint256 min_dy) external returns(uint256);

37: contract Configurator {
63:    event RedemptionFeeChanged(uint256 newSlippage);
64:    event SafeCollateralRatioChanged(address indexed pool, uint256 newRatio);
65:    event RedemptionProvider(address indexed user, bool status);
66:    event ProtocolRewardsPoolChanged(address indexed pool, uint256 timestamp);
67:    event EUSDMiningIncentivesChanged(address indexed pool, uint256 timestamp);
68:    event BorrowApyChanged(address indexed pool, uint256 newApy);
69:    event KeeperRatioChanged(address indexed pool, uint256 newSlippage);
70:    event tokenMinerChanges(address indexed pool, bool status);
80:    constructor(address _dao, address _curvePool) {
85:    modifier onlyRole(bytes32 role) {
90:    modifier checkRole(bytes32 role) {

// @param missing
98:    function initToken(address _eusd, address _peusd) external onlyRole(DAO) {
126:    function setBadCollateralRatio(address pool, uint256 newRatio) external onlyRole(DAO) {
198:    function setSafeCollateralRatio(address pool, uint256 newRatio) external checkRole(TIMELOCK) {
268:    function becomeRedemptionProvider(bool _bool) external {
277:    function refreshMintReward(address user) external {

338:    function getBadCollateralRatio(address pool) external view returns(uint256) {
360:    function hasRole(bytes32 role, address caller) external view returns (bool) {
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol

```solidity
79:    modifier onlyMintVault() {
83:    modifier MintPaused() {
87:    modifier BurnPaused() {
92:    constructor(address _config) {

// @param missing
134:    function balanceOf(address _account) public view returns (uint256) {
153:    function transfer(address _recipient, uint256 _amount) public returns (bool) {
165:    function allowance(address _owner, address _spender) public view returns (uint256) {
177:    function _spendAllowance(address owner, address spender, uint256 amount) internal virtual {
200:    function approve(address _spender, uint256 _amount) public returns (bool) {
226:    function transferFrom(address from, address to, uint256 amount) public returns (bool) {
292:    function sharesOf(address _account) public view returns (uint256) {
299:    function getSharesByMintedEUSD(uint256 _EUSDAmount) public view returns (uint256) {
311:    function getMintedEUSDByShares(uint256 _sharesAmount) public view returns (uint256) {
334:    function transferShares(address _recipient, uint256 _sharesAmount) public returns (uint256) {
348:    function _transfer(address _sender, address _recipient, uint256 _amount) internal {
366:    function _approve(address _owner, address _spender, uint256 _amount) internal {
377:    function _sharesOf(address _account) internal view returns (uint256) {
391:    function _transferShares(address _sender, address _recipient, uint256 _sharesAmount) internal {

// @param and @return missing
248:    function increaseAllowance(address _spender, uint256 _addedValue) public returns (bool) {
268:    function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
411:    function mint(address _recipient, uint256 _mintAmount) external onlyMintVault MintPaused returns (uint256 newTotalShares) {
440:    function burn(address _account, uint256 _burnAmount) external onlyMintVault BurnPaused returns (uint256 newTotalShares) {
459:    function burnShares(address _account, uint256 _sharesAmount) external onlyMintVault BurnPaused returns (uint256 newTotalShares) {
464:    function _onlyBurnShares(address _account, uint256 _sharesAmount) private returns (uint256 newTotalShares) {
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol

```solidity
18: interface IesLBRBoost {
19:    function getUnlockTime(

24: contract ProtocolRewardsPool is Ownable {
48:    constructor(address _config) {
52:    function setTokenAddress(address _eslbr, address _lbr, address _boost) external onlyOwner {
58:    function setGrabCost(uint256 _ratio) external onlyOwner {
64:    function totalStaked() internal view returns (uint256) {
69:    function stakedOf(address staker) internal view returns (uint256) {
73:    function stake(uint256 amount) external {
100:    function withdraw(address user) public {
149:    function getPreUnlockableAmount(address user) public view returns (uint256 amount) {
155:    function getClaimAbleLBR(address user) public view returns (uint256 amount) {
161:    function getReservedLBRForVesting(address user) public view returns (uint256 amount) {
167:    function earned(address _account) public view returns (uint) {
171:    function getClaimAbleUSD(address user) external view returns (uint256 amount) {
178:    modifier updateReward(address account) {
184:    function refreshReward(address _account) external updateReward(_account) {}
    
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol

```solidity
21: interface FlashBorrower {

29: contract PeUSDMainnet is BaseOFTV2, ERC20 {
35:    struct ConvertInfo {
42:    modifier onlyMintVault() {
46:    modifier MintPaused() {
50:    modifier BurnPaused() {
55:    constructor(address _config, uint8 _sharedDecimals, address _lzEndpoint) ERC20("peg-eUSD", "PeUSD") BaseOFTV2(_sharedDecimals, _lzEndpoint) {
63:    function mint(address to, uint256 amount) external onlyMintVault MintPaused returns (bool) {
69:    function burn(address account, uint256 amount) external onlyMintVault BurnPaused returns (bool) {
141:    function transferFrom(address from, address to, uint256 amount) public override returns (bool) {
171:    function circulatingSupply() public view virtual override returns (uint) {
175:    function token() public view virtual override returns (address) {
184:    function _debitFrom(address _from, uint16, bytes32, uint _amount) internal virtual override returns (uint) {
191:    function _creditTo(uint16, address _toAddress, uint _amount) internal virtual override returns (uint) {
196:    function _transferFrom(address _from, address _to, uint _amount) internal virtual override returns (uint) {
205:    function _ld2sdRatio() internal view virtual override returns (uint) {

// @param missing
156:    function getFee(uint256 share) public view returns (uint256) {

```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol

```solidity
9:  contract LybraGovernance is GovernorTimelockControl {
27:    struct ProposalExtraData {
38:      constructor(string memory name_, TimelockController timelock_, address _esLBR) GovernorTimelockControl(timelock_)  Governor(name_) {

// @param and @return missing
51:    function quorum(uint256 timepoint) public view override returns (uint256){
59:    function _quorumReached(uint256 proposalId) internal view override returns (bool){
66:    function _voteSucceeded(uint256 proposalId) internal view override returns (bool){
98:     function _getVotes(address account, uint256 timepoint, bytes memory) internal view override returns (uint256){
106:    function _execute(uint256 /* proposalId */, address[] memory targets, uint256[] memory values, bytes[] memory calldatas, bytes32 descriptionHash) internal virtual override {
165:    function hasVoted(uint256 proposalId, address account) public override view virtual returns (bool){
179:    function supportsInterface(bytes4 interfaceId) public view virtual override(GovernorTimelockControl) returns (bool) {


112:    function proposals(uint256 proposalId) external view returns (uint256 id, address proposer, uint256 eta, uint256 startBlock, uint256 endBlock, uint256 forVotes, uint256 againstVotes, uint256 abstainVotes, bool canceled, bool executed) {
129:    function getReceipt(uint256 proposalId, address account) external view returns (bool voted, uint8 support, uint256 votes){  

// @return missing
143:    function votingPeriod() public pure override returns (uint256){
147:     function votingDelay() public pure override returns (uint256){
151:     function CLOCK_MODE() public override view returns (string memory){
156:    function COUNTING_MODE() public override view virtual returns (string memory){
161:    function clock() public override view returns (uint48){
172:    function proposalThreshold() public pure override returns (uint256) {


// @param missing
76:    function _countVote(uint256 proposalId, address account, uint8 support, uint256 weight, bytes memory) internal override {
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol

```solidity
8:  interface IesLBRBoost {
9:    function getUserBoost(

16: contract StakingRewardsV2 is Ownable {
49:    constructor(address _stakingToken, address _rewardToken, address _boost) {
56:    modifier updateReward(address _account) {
69:    function lastTimeRewardApplicable() public view returns (uint256) {
74:    function rewardPerToken() public view returns (uint256) {
83:    function stake(uint256 _amount) external updateReward(msg.sender) {
93:    function withdraw(uint256 _amount) external updateReward(msg.sender) {
101:    function getBoost(address _account) public view returns (uint256) {
106:    function earned(address _account) public view returns (uint256) {
111:    function getReward() external updateReward(msg.sender) {
121:    function setRewardsDuration(uint256 _duration) external onlyOwner {
127:    function setBoost(address _boost) external onlyOwner {
132:    function notifyRewardAmount(uint256 _amount) external onlyOwner updateReward(address(0)) {
147:    function _min(uint256 x, uint256 y) private pure returns (uint256) {
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol

```solidity
8:  interface Ilido {
9:    function submit(address _referral) external payable returns (uint256 StETH);

12: contract LybraStETHDepositVault is LybraEUSDVaultBase {
18:    constructor(address _config, address _stETH, address _oracle) LybraEUSDVaultBase(_stETH, _oracle, _config) {

// @param missing
25:    function setLidoRebaseTime(uint256 _time) external {
62:    function excessIncomeDistribution(uint256 stETHAmount) external override {

// @return missing
101:    function getDutchAuctionDiscountPrice() public view returns (uint256) {
107:    function getAssetPrice() public override returns (uint256) {
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol

```solidity
13: contract LBR is BaseOFTV2, ERC20 {
18:    constructor(address _config, uint8 _sharedDecimals, address _lzEndpoint) ERC20("LBR", "LBR") BaseOFTV2(_sharedDecimals, _lzEndpoint) {
25:    function mint(address user, uint256 amount) external returns (bool) {
32:    function burn(address user, uint256 amount) external returns (bool) {
38:    function circulatingSupply() public view virtual override returns (uint) {
42:    function token() public view virtual override returns (address) {
49:    function _debitFrom(address _from, uint16, bytes32, uint _amount) internal virtual override returns (uint) {
56:    function _creditTo(uint16, address _toAddress, uint _amount) internal virtual override returns (uint) {
61:    function _transferFrom(address _from, address _to, uint _amount) internal virtual override returns (uint) {
70:    function _ld2sdRatio() internal view virtual override returns (uint) {
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol

```solidity
7:      contract esLBRBoost is Ownable {

// @param missing
33:    function addLockSetting(esLBRLockSetting memory setting) external onlyOwner {
38:    function setLockStatus(uint256 id) external {

// @param and @return missing
48:    function getUnlockTime(address user) external view returns (uint256 unlockTime) {
57:    function getUserBoost(address user, uint256 userUpdatedAt, uint256 finishAt) external view returns (uint256) {
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSD.sol

```solidity
8:  contract PeUSD is BaseOFTV2, ERC20 {
20:    function circulatingSupply() public view virtual override returns (uint) {
24:    function token() public view virtual override returns (address) {
31:    function _debitFrom(address _from, uint16, bytes32, uint _amount) internal virtual override returns (uint) {
38:    function _creditTo(uint16, address _toAddress, uint _amount) internal virtual override returns (uint) {
43:    function _transferFrom(address _from, address _to, uint _amount) internal virtual override returns (uint) {
52:    function _ld2sdRatio() internal view virtual override returns (uint) {
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraRETHVault.sol

```solidity
9:  interface IRETH {
10:    function getExchangeRatio() external view returns (uint256);

13: interface IRkPool {
14:    function deposit() external payable;

17: contract LybraRETHVault is LybraPeUSDVaultBase {
22:    constructor(address _peusd, address _config, address _rETH, address _oracle, address _rkPool)
27:    function depositEtherToMint(uint256 mintAmount) external payable override {
41:    function setRkPool(address addr) external {
47:    function getAssetPrice() public override returns (uint256) {
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWstETHVault.sol

```solidity
9:  interface IWstETH {
10:    function stEthPerToken() external view returns (uint256);
12:    function wrap(uint256 _stETHAmount) external returns (uint256);

15:  interface Ilido {
16:    function submit(address _referral) external payable returns (uint256 StETH);
18:    function approve(address spender, uint256 amount) external returns (bool);

21:  contract LybraWstETHVault is LybraPeUSDVaultBase {
25:    constructor(address _lido, address _peusd, address _oracle, address _asset, address _config) LybraPeUSDVaultBase(_peusd, _oracle, _asset,_config) {
29:    function depositEtherToMint(uint256 mintAmount) external payable override {
48:    function getAssetPrice() public override returns (uint256) {
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/esLBR.sol

```solidity
13:  interface IProtocolRewardsPool {
14:    function refreshReward(address user) external;

17:  contract esLBR is ERC20Votes {
22:    constructor(address _config) ERC20Permit("esLBR") ERC20("esLBR", "esLBR") {
26:    function _transfer(address, address, uint256) internal virtual override {
30:    function mint(address user, uint256 amount) external returns (bool) {
38:    function burn(address user, uint256 amount) external returns (bool) {
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWbETHVault.sol

```solidity
9:  interface IWBETH {
10:    function exchangeRatio() external view returns (uint256);
12:    function deposit(address referral) external payable;
15:    contract LybraWBETHVault is LybraPeUSDVaultBase {
17:    constructor(address _peusd, address _oracle, address _asset, address _config)
20:    function depositEtherToMint(uint256 mintAmount) external payable override {
34:    function getAssetPrice() public override returns (uint256) {
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/GovernanceTimelock.sol

```solidity
8:  contract GovernanceTimelock is TimelockController {
15:    constructor(uint256 minDelay, address[] memory proposers, address[] memory executors, address admin) TimelockController(minDelay, proposers, executors, admin) {
25:    function checkRole(bytes32 role, address _sender) public view  returns(bool){
29:    function checkOnlyRole(bytes32 role, address _sender) public view  returns(bool){
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/AdminTimelock.sol

```solidity
7:  contract AdminTimelock is TimelockController {
8:    constructor(uint256 minDelay, address[] memory proposers, address[] memory executors, address admin) TimelockController(minDelay, proposers, executors, admin) {}
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/Proxy/LybraProxy.sol

```solidity
8:    contract LybraProxy is TransparentUpgradeableProxy {
9:    constructor(address _logic,address _admin,bytes memory _data) TransparentUpgradeableProxy(_logic, _admin, _data) {}
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/Proxy/LybraProxyAdmin.sol

```solidity
6:   contract LybraProxyAdmin is ProxyAdmin {}
```

---

### State variable and function names [4]

- Variables should be named according to their specifications
- private and internal `variables` should preppend with `underline`
- private and internal `functions` should preppend with `underline`
- constant state variables should be UPPER_CASE

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol

```solidity
//  private and internal `variables` should preppend with `underline`
55:    AggregatorV3Interface internal etherPriceFeed;
56:    AggregatorV3Interface internal lbrPriceFeed;

//  private and internal `functions` should preppend with `underline`
132:    function totalStaked() internal view returns (uint256) {
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol

```solidity
// private and internal `variables` should preppend with `underline`
51:    mapping(address => uint256) private shares;
56:    mapping(address => mapping(address => uint256)) private allowances;
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol

```solidity
// private and internal `variables` should preppend with `underline`
32:    uint internal immutable ld2sdRatio;
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSD.sol

```solidity
// private and internal `variables` should preppend with `underline`
9:    uint internal immutable ld2sdRatio;
```

---

### Version [5]

- Pragma versions should be standardized and avoid floating pragma `( ^ )`.

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol

```solidity
// remove the ^ for a stable version
3:  pragma solidity ^0.8.17;
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol

```solidity
// remove the ^ for a stable version
3:  pragma solidity ^0.8.17;
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol

```solidity
// remove the ^ for a stable version
3:  pragma solidity ^0.8.17;
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol

```solidity
// remove the ^ for a stable version
15: pragma solidity ^0.8.17;
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol

```solidity
// remove the ^ for a stable version
3:  pragma solidity ^0.8.17;
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol

```solidity
// remove the ^ for a stable version
3:  pragma solidity ^0.8.17;
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol

```solidity
// remove the ^ for a stable version
14: pragma solidity ^0.8.17;
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol

```solidity
// remove the ^ for a stable version
3:  pragma solidity ^0.8.17;
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol

```solidity
// remove the ^ for a stable version, also this version is different from the other ones from the repo
2:  pragma solidity ^0.8;
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol

```solidity
// remove the ^ for a stable version
3:    pragma solidity ^0.8.17;
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol

```solidity
// remove the ^ for a stable version
3:  pragma solidity ^0.8.17;
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol

```solidity
// remove the ^ for a stable version
3:  pragma solidity ^0.8.17;
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSD.sol

```solidity
// remove the ^ for a stable version
3:  pragma solidity ^0.8.17;
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraRETHVault.sol

```solidity
// remove the ^ for a stable version
3:  pragma solidity ^0.8.17;
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWstETHVault.sol

```solidity
// remove the ^ for a stable version
3:  pragma solidity ^0.8.17;
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/esLBR.sol

```solidity
// remove the ^ for a stable version
3:  pragma solidity ^0.8.17;
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWbETHVault.sol

```solidity
// remove the ^ for a stable version
3:  pragma solidity ^0.8.17;
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/GovernanceTimelock.sol

```solidity
// remove the ^ for a stable version
3:  pragma solidity ^0.8.17;
```
  
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/AdminTimelock.sol

```solidity
// remove the ^ for a stable version
3:  pragma solidity ^0.8.17;
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/Proxy/LybraProxy.sol

```solidity
// remove the ^ for a stable version
3:  pragma solidity ^0.8.17;
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/Proxy/LybraProxyAdmin.sol

```solidity
// remove the ^ for a stable version
3:  pragma solidity ^0.8.17;
```

---

### Initialized variables [6]

- Variables are default initialized with 0 for `uint / int`, 0x0 for `address` and false for `bool`


---

### Observations [7]

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/GovernanceTimelock.sol

```solidity
// remove this comment
9:    // contructor()
```

