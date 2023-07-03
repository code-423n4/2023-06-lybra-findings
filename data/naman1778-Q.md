## [N-01] Using Vulnerable Version of OpenZeppelin

The package.json configuration file says that the project is using 4.9.1 of OpenZeppelin which has a vulnerability.

Although there is no security vulnerability covering the project, it is recommended to use the latest version 4.9.2.

There is 1 instance of this issue in 1 file:

    File: package.json

    13:"@openzeppelin/contracts": "^4.9.1",

https://github.com/code-423n4/2023-06-lybra/blob/main/package.json

## [N-02] Use a modifier for access control

Consider using a modifier to implement access control instead of inlining the condition/requirement in the function’s body.

There are 9 instances of this issue in 7 files:

    File: contracts/lybra/token/esLBR.sol 

    31: require(configurator.tokenMiner(msg.sender), "not authorized");

    39: require(configurator.tokenMiner(msg.sender), "not authorized");

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/esLBR.sol

    File: contracts/lybra/pools/LybraRETHVault.sol

    42: require(configurator.hasRole(keccak256("TIMELOCK"), msg.sender));

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraRETHVault.sol

    File: contracts/lybra/token/LBR.sol	

    25: function mint(address user, uint256 amount) external returns (bool) {
    26:     require(configurator.tokenMiner(msg.sender), "not authorized");
    27:     require(totalSupply() + amount <= maxSupply, "exceeding the maximum supply quantity.");
    28:     _mint(user, amount);
    29:     return true;
    30: }

    32: function burn(address user, uint256 amount) external returns (bool) {
    33:     require(configurator.tokenMiner(msg.sender), "not authorized");
    34:     _burn(user, amount);
    35:     return true;
    36: }

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol

    File: contracts/lybra/pools/LybraStETHVault.sol 💰 📤 🧮 ♻️	

    25: function setLidoRebaseTime(uint256 _time) external {
    26:     require(configurator.hasRole(keccak256("ADMIN"), msg.sender), "not authorized");
    27:    lidoRebaseTime = _time;
    28: }

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol

    File: contracts/lybra/governance/LybraGovernance.sol 🧮	  

    14: function _execute(uint256 /* proposalId */, address[] memory targets, uint256[] memory values, bytes[] memory calldatas, bytes32 descriptionHash) internal virtual override {
    15:     require(GovernanceTimelock.checkOnlyRole(keccak256("TIMELOCK"), msg.sender), "not authorized");
    16:     super._execute(1, targets, values, calldatas, descriptionHash);
    17:     // _timelock.executeBatch{value: msg.value}(targets, values, calldatas, 0, descriptionHash);
    18: }

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol

    File: contracts/lybra/token/PeUSDMainnetStableVision.sol 💰	

    79: function convertToPeUSD(address user, uint256 eusdAmount) public {
    80:     require(_msgSender() == user || _msgSender() == address(this), "MDM");
    81:     require(EUSD.balanceOf(address(this)) + eusdAmount <= configurator.getEUSDMaxLocked(),"ESL");
    82:     bool success = EUSD.transferFrom(user, address(this), eusdAmount);
    83:     require(success, "TF");
    84:     uint256 share = EUSD.getSharesByMintedEUSD(eusdAmount);
    85:     userConvertInfo[user].depositedEUSDShares += share;
    86:     userConvertInfo[user].mintedPeUSD += eusdAmount;
    87:     _mint(user, eusdAmount);
    88: }

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol

    File: contracts/lybra/miner/ProtocolRewardsPool.sol 📤	

    227: function notifyRewardAmount(uint amount, uint tokenType) external {
    228:     require(msg.sender == address(configurator));
    229:     if (totalStaked() == 0) return;
    230:     require(amount > 0, "amount = 0");
    231:     if(tokenType == 0) {
    232:         uint256 share = IEUSD(configurator.getEUSDAddress()).getSharesByMintedEUSD(amount);
    233:         rewardPerTokenStored = rewardPerTokenStored + (share * 1e18) / totalStaked();
    234:     } else if(tokenType == 1) {
    235:         ERC20 token = ERC20(configurator.stableToken());
    236:         rewardPerTokenStored = rewardPerTokenStored + (amount * 1e36 / token.decimals()) / totalStaked();
    237:     } else {
    238:         rewardPerTokenStored = rewardPerTokenStored + (amount * 1e18) / totalStaked();
    239:     }
    240: }

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol

## [N-03] Use scientific notation (e.g. 1e18) rather than exponentiation (e.g. 10**18)

While the compiler knows to optimize away the exponentiation, it’s still better coding practice to use idioms that do not require compiler optimization, if they exist

There are 3 instances of this issue in 3 files:

    File: contracts/lybra/token/PeUSD.sol	

    14: ld2sdRatio = 10 ** (decimals - _sharedDecimals);

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSD.sol

    File: contracts/lybra/token/LBR.sol	

    22: ld2sdRatio = 10 ** (decimals - _sharedDecimals);

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol

    File: contracts/lybra/token/PeUSDMainnetStableVision.sol

    60: ld2sdRatio = 10 ** (decimals - _sharedDecimals);

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol

## [N-04] According to the syntax rules, use => mapping ( instead of => mapping( using spaces as keyword

There is 1 instance of this issue in 1 file:

    File: contracts/lybra/token/EUSD.sol

    56: mapping(address => mapping(address => uint256)) private allowances;

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol

## [N-05] Inconsistent Solidity Versions

Instead of the below mentioned file, all other files use 0.8.17 version of solidity.

There is 1 instance of this issue in 1 file:

    File: contracts/lybra/miner/stakerewardV2pool.sol

    2: pragma solidity ^0.8;

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol

## [N-06] Empty/Unused Function Parameters

Empty or unused function parameters should be commented out as a better and declarative way to silence runtime warning messages. As an example, the following function may have these parameters refactored to:

There are 9 instances of this issue in 4 files:

    File: contracts/lybra/token/PeUSD.sol	

    31: function _debitFrom(address _from, uint16, bytes32, uint _amount) internal virtual override returns (uint) {

    38: function _creditTo(uint16, address _toAddress, uint _amount) internal virtual override returns (uint) {

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSD.sol

    File: contracts/lybra/token/LBR.sol	

    49: function _debitFrom(address _from, uint16, bytes32, uint _amount) internal virtual override returns (uint) {

    56: function _creditTo(uint16, address _toAddress, uint _amount) internal virtual override returns (uint) {

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol

    File: contracts/lybra/governance/LybraGovernance.sol 🧮	

    76: function _countVote(uint256 proposalId, address account, uint8 support, uint256 weight, bytes memory) internal override {

    98: function _getVotes(address account, uint256 timepoint, bytes memory) internal view override returns (uint256){

    106: function _execute(uint256 /* proposalId */, address[] memory targets, uint256[] memory values, bytes[] memory calldatas, bytes32 descriptionHash) internal virtual override {

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol

    File: contracts/lybra/token/PeUSDMainnetStableVision.sol 💰	

    184: function _debitFrom(address _from, uint16, bytes32, uint _amount) internal virtual override returns (uint) {

    191: function _creditTo(uint16, address _toAddress, uint _amount) internal virtual override returns (uint) {

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol