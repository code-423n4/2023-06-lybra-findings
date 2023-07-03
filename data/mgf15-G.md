
## Summary

| |Issue|Instances|
|-|:-|:-:|
| [GAS-1](#GAS-1) | Use assembly to check for `address(0)` | 23 |
| [GAS-2](#GAS-2) | Functions guaranteed to revert when called by normal users can be marked payable | 25 |
| [GAS-3](#GAS-3) | `keccak256()` EXPRESSIONS WHICH ARE FIXED, CAN BE PRECALCULATED AND ASSIGNED TO THE CONSTANT VARIABLES. | 7 |
| [GAS-4](#GAS-4) | Long revert strings | 39 |
| [GAS-5](#GAS-5) | `>=` costs less gas than `>` | 60 |
| [GAS-6](#GAS-6) | Use nested if and, avoid multiple check combinations | 4 |
| [GAS-7](#GAS-7) | Using `private` rather than `public` for constants, saves gas | 7 |
| [GAS-8](#GAS-8) | Splitting `require()` statements that use && saves gas | 3 |
| [GAS-9](#GAS-9) | Inverting the condition of an if-else-statement wastes gas | 9 |
| [GAS-10](#GAS-10) | Use double `if` statements instead of `&&` | 7 |
| [GAS-11](#GAS-11) | Use constants instead of `type(uintx).max` | 1 |
| [GAS-12](#GAS-12) | Use shift Right/Left instead of division/multiplication if possible | 81 |
| [GAS-13](#GAS-13) | Don't compare boolean expressions to boolean literals | 1 |
| [GAS-14](#GAS-14) | Write direct outcome, instead of performing mathematical operations  | 3 |
| [GAS-15](#GAS-15) | USE BYTES32 INSTEAD OF STRING | 5 |
| [GAS-16](#GAS-16) | Compute known value `off-chain` | 7 |
| [GAS-17](#GAS-17) | Part of the code can be pre calculated | 9 |
| [GAS-18](#GAS-18) | Ternary operation is cheaper than if-else statement | 15 |
| [GAS-19](#GAS-19) | Sort Solidity operations using short-circuit mode | 2 |


### **[GAS-1]** Use assembly to check for `address(0)`

Saves 6 gas per instance if using assembly to check for address(0)
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/contracts/lybra/token/PeUSD.sol
46:        if (_from != address(this) && _from != spender)
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/token/PeUSD.sol#L46
```solidity
File:2023-06-lybra/contracts/lybra/token/LBR.sol
64:        if (_from != address(this) && _from != spender)
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/token/LBR.sol#L64
```solidity
File:2023-06-lybra/contracts/lybra/miner/stakerewardV2pool.sol
60:        if (_account != address(0)) {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/miner/stakerewardV2pool.sol#L60
```solidity
File:2023-06-lybra/contracts/lybra/token/PeUSDMainnetStableVision.sol
64:        require(to != address(0), "TZA");
80:        require(_msgSender() == user || _msgSender() == address(this), "MDM");
199:        if (_from != address(this) && _from != spender)
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/token/PeUSDMainnetStableVision.sol#L64
```solidity
File:2023-06-lybra/contracts/lybra/miner/ProtocolRewardsPool.sol
228:        require(msg.sender == address(configurator));
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/miner/ProtocolRewardsPool.sol#L228
```solidity
File:2023-06-lybra/contracts/lybra/token/EUSD.sol
367:        require(_owner != address(0), "APPROVE_FROM_ZERO_ADDRESS");
368:        require(_spender != address(0), "APPROVE_TO_ZERO_ADDRESS");
392:        require(_sender != address(0), "TRANSFER_FROM_THE_ZERO_ADDRESS");
393:        require(_recipient != address(0), "TRANSFER_TO_THE_ZERO_ADDRESS");
412:        require(_recipient != address(0), "MINT_TO_THE_ZERO_ADDRESS");
441:        require(_account != address(0), "BURN_FROM_THE_ZERO_ADDRESS");
460:        require(_account != address(0), "BURN_FROM_THE_ZERO_ADDRESS");
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/token/EUSD.sol#L367
```solidity
File:2023-06-lybra/contracts/lybra/configuration/LybraConfigurator.sol
99:        if (address(EUSD) == address(0)) EUSD = IEUSD(_eusd);
100:        if (address(peUSD) == address(0)) peUSD = IEUSD(_peusd);
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/configuration/LybraConfigurator.sol#L99
```solidity
File:2023-06-lybra/contracts/lybra/miner/EUSDMiningIncentives.sol
76:        if (_account != address(0)) {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/miner/EUSDMiningIncentives.sol#L76
```solidity
File:2023-06-lybra/contracts/lybra/pools/base/LybraEUSDVaultBase.sol
99:        require(onBehalfOf != address(0), "TZA");
127:        require(onBehalfOf != address(0), "MINT_TO_THE_ZERO_ADDRESS");
141:        require(onBehalfOf != address(0), "BURN_TO_THE_ZERO_ADDRESS");
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L99
```solidity
File:2023-06-lybra/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol
83:        require(onBehalfOf != address(0), "TZA");
97:        require(onBehalfOf != address(0), "TZA");
111:        require(onBehalfOf != address(0), "TZA");
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L83

*Instances (23)*

### **[GAS-2]** Functions guaranteed to revert when called by normal users can be marked payable

If a function modifier such as `onlyOwner` is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided.
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/contracts/lybra/miner/esLBRBoost.sol
33:    function addLockSetting(esLBRLockSetting memory setting) external onlyOwner {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/miner/esLBRBoost.sol#L33
```solidity
File:2023-06-lybra/contracts/lybra/miner/stakerewardV2pool.sol
121:    function setRewardsDuration(uint256 _duration) external onlyOwner {
127:    function setBoost(address _boost) external onlyOwner {
132:    function notifyRewardAmount(uint256 _amount) external onlyOwner updateReward(address(0)) {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/miner/stakerewardV2pool.sol#L121
```solidity
File:2023-06-lybra/contracts/lybra/token/PeUSDMainnetStableVision.sol
63:    function mint(address to, uint256 amount) external onlyMintVault MintPaused returns (bool) {
69:    function burn(address account, uint256 amount) external onlyMintVault BurnPaused returns (bool) {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/token/PeUSDMainnetStableVision.sol#L63
```solidity
File:2023-06-lybra/contracts/lybra/miner/ProtocolRewardsPool.sol
52:    function setTokenAddress(address _eslbr, address _lbr, address _boost) external onlyOwner {
58:    function setGrabCost(uint256 _ratio) external onlyOwner {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/miner/ProtocolRewardsPool.sol#L52
```solidity
File:2023-06-lybra/contracts/lybra/token/EUSD.sol
411:    function mint(address _recipient, uint256 _mintAmount) external onlyMintVault MintPaused returns (uint256 newTotalShares) {
440:    function burn(address _account, uint256 _burnAmount) external onlyMintVault BurnPaused returns (uint256 newTotalShares) {
459:    function burnShares(address _account, uint256 _sharesAmount) external onlyMintVault BurnPaused returns (uint256 newTotalShares) {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/token/EUSD.sol#L411
```solidity
File:2023-06-lybra/contracts/lybra/configuration/LybraConfigurator.sol
98:    function initToken(address _eusd, address _peusd) external onlyRole(DAO) {
109:    function setMintVault(address pool, bool isActive) external onlyRole(DAO) {
119:    function setMintVaultMaxSupply(address pool, uint256 maxSupply) external onlyRole(DAO) {
126:    function setBadCollateralRatio(address pool, uint256 newRatio) external onlyRole(DAO) {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/configuration/LybraConfigurator.sol#L98
```solidity
File:2023-06-lybra/contracts/lybra/miner/EUSDMiningIncentives.sol
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
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/miner/EUSDMiningIncentives.sol#L84

*Instances (25)*

### **[GAS-3]** `keccak256()` EXPRESSIONS WHICH ARE FIXED, CAN BE PRECALCULATED AND ASSIGNED TO THE CONSTANT VARIABLES.

Instead of calculating the keccak256() when the contract is made, pre calculate them before and only give the result to a constant
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/contracts/lybra/governance/GovernanceTimelock.sol
10:    bytes32 public constant DAO = keccak256("DAO");
11:    bytes32 public constant TIMELOCK = keccak256("TIMELOCK");
12:    bytes32 public constant ADMIN = keccak256("ADMIN");
13:    bytes32 public constant GOV = keccak256("GOV");
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/governance/GovernanceTimelock.sol#L10
```solidity
File:2023-06-lybra/contracts/lybra/configuration/LybraConfigurator.sol
76:    bytes32 public constant DAO = keccak256("DAO");
77:    bytes32 public constant TIMELOCK = keccak256("TIMELOCK");
78:    bytes32 public constant ADMIN = keccak256("ADMIN");
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/configuration/LybraConfigurator.sol#L76

*Instances (7)*

### **[GAS-4]** Long revert strings

Revert strings that are longer than 32 bytes require at least one additional mstore, along with additional overhead for computing memory offset, etc.
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/contracts/lybra/token/esLBR.sol
32:        require(totalSupply() + amount <= maxSupply, "exceeding the maximum supply quantity.");
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/token/esLBR.sol#L32
```solidity
File:2023-06-lybra/contracts/lybra/token/PeUSD.sol
13:        require(_sharedDecimals <= decimals, "OFT: sharedDecimals must be <= decimals");
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/token/PeUSD.sol#L13
```solidity
File:2023-06-lybra/contracts/lybra/miner/esLBRBoost.sol
42:            require(userStatus.duration <= _setting.duration, "Your lock-in period has not ended, and the term can only be extended, not reduced.");
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/miner/esLBRBoost.sol#L42
```solidity
File:2023-06-lybra/contracts/lybra/token/LBR.sol
21:        require(_sharedDecimals <= decimals, "OFT: sharedDecimals must be <= decimals");
27:        require(totalSupply() + amount <= maxSupply, "exceeding the maximum supply quantity.");
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/token/LBR.sol#L21
```solidity
File:2023-06-lybra/contracts/lybra/pools/LybraStETHVault.sol
26:        require(configurator.hasRole(keccak256("ADMIN"), msg.sender), "not authorized");
64:        require(excessAmount > 0 && stETHAmount > 0, "Only LSD excess income can be exchanged");
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/pools/LybraStETHVault.sol#L26
```solidity
File:2023-06-lybra/contracts/lybra/governance/LybraGovernance.sol
78:        require(state(proposalId) == ProposalState.Active, "GovernorBravo::castVoteInternal: voting is closed");
79:        require(support <= 2, "GovernorBravo::castVoteInternal: invalid vote type");
82:        require(receipt.hasVoted == false, "GovernorBravo::castVoteInternal: voter already voted");
107:        require(GovernanceTimelock.checkOnlyRole(keccak256("TIMELOCK"), msg.sender), "not authorized");
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/governance/LybraGovernance.sol#L78
```solidity
File:2023-06-lybra/contracts/lybra/token/PeUSDMainnetStableVision.sol
59:        require(_sharedDecimals <= decimals, "OFT: sharedDecimals must be <= decimals");
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/token/PeUSDMainnetStableVision.sol#L59
```solidity
File:2023-06-lybra/contracts/lybra/miner/ProtocolRewardsPool.sol
88:        require(block.timestamp >= esLBRBoost.getUnlockTime(msg.sender), "Your lock-in period has not ended. You can't convert your esLBR now.");
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/miner/ProtocolRewardsPool.sol#L88
```solidity
File:2023-06-lybra/contracts/lybra/token/EUSD.sol
271:        require(currentAllowance >= subtractedValue, "ERC20: decreased allowance below zero");
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/token/EUSD.sol#L271
```solidity
File:2023-06-lybra/contracts/lybra/configuration/LybraConfigurator.sol
200:            require(newRatio >= 160 * 1e18, "eUSD vault safe collateralRatio should more than 160%");
202:            require(newRatio >= vaultBadCollateralRatio[pool] + 1e19, "PeUSD vault safe collateralRatio should more than bad collateralRatio");
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/configuration/LybraConfigurator.sol#L200
```solidity
File:2023-06-lybra/contracts/lybra/miner/EUSDMiningIncentives.sol
193:        require(!isOtherEarningsClaimable(msg.sender), "Insufficient DLP, unable to claim rewards");
203:        require(isOtherEarningsClaimable(user), "The rewards of the user cannot be bought out");
205:            require(isEUSDBuyoutAllowed, "The purchase using EUSD is not permitted.");
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/miner/EUSDMiningIncentives.sol#L193
```solidity
File:2023-06-lybra/contracts/lybra/pools/base/LybraEUSDVaultBase.sol
73:        require(assetAmount >= 1 ether, "Deposit should not be less than 1 stETH.");
101:        require(depositedAsset[msg.sender] >= amount, "Withdraw amount exceeds deposited amount.");
157:        require(onBehalfOfCollateralRatio < badCollateralRatio, "Borrowers collateral ratio should below badCollateralRatio");
159:        require(assetAmount * 2 <= depositedAsset[onBehalfOf], "a max of 50% collateral can be liquidated");
160:        require(EUSD.allowance(provider, address(this)) > 0, "provider should authorize to provide liquidation EUSD");
189:        require((totalDepositedAsset * assetPrice * 100) / poolTotalEUSDCirculation < badCollateralRatio, "overallCollateralRatio should below 150%");
191:        require(onBehalfOfCollateralRatio < 125 * 1e18, "borrowers collateralRatio should below 125%");
192:        require(assetAmount <= depositedAsset[onBehalfOf], "total of collateral can be liquidated at most");
197:        require(EUSD.allowance(provider, address(this)) >= eusdAmount, "provider should authorize to provide liquidation EUSD");
233:        require(configurator.isRedemptionProvider(provider), "provider is not a RedemptionProvider");
234:        require(borrowed[provider] >= eusdAmount, "eusdAmount cannot surpass providers debt");
237:        require(providerCollateralRatio >= 100 * 1e18, "provider's collateral ratio should more than 100%");
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L73
```solidity
File:2023-06-lybra/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol
59:        require(assetAmount >= 1 ether, "Deposit should not be less than 1 collateral asset.");
128:        require(onBehalfOfCollateralRatio < configurator.getBadCollateralRatio(address(this)), "Borrowers collateral ratio should below badCollateralRatio");
130:        require(assetAmount * 2 <= depositedAsset[onBehalfOf], "a max of 50% collateral can be liquidated");
131:        require(PeUSD.allowance(provider, address(this)) > 0, "provider should authorize to provide liquidation EUSD");
158:        require(configurator.isRedemptionProvider(provider), "provider is not a RedemptionProvider");
159:        require(borrowed[provider] >= peusdAmount, "peusdAmount cannot surpass providers debt");
162:        require(providerCollateralRatio >= 100 * 1e18, "provider's collateral ratio should more than 100%");
213:        require(depositedAsset[_provider] >= _amount, "Withdraw amount exceeds deposited amount.");
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L59

*Instances (39)*

### **[GAS-5]** `>=` costs less gas than `>`

The compiler uses opcodes GT and ISZERO for solidity code that uses >, but only requires LT for >=, which saves 3 gas
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/contracts/lybra/pools/LybraWbETHVault.sol
27:        if (mintAmount > 0) {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/pools/LybraWbETHVault.sol#L27
```solidity
File:2023-06-lybra/contracts/lybra/pools/LybraWstETHVault.sol
33:        require(sharesAmount > 0, "ZERO_DEPOSIT");
41:        if (mintAmount > 0) {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/pools/LybraWstETHVault.sol#L33
```solidity
File:2023-06-lybra/contracts/lybra/pools/LybraRETHVault.sol
34:        if (mintAmount > 0) {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/pools/LybraRETHVault.sol#L34
```solidity
File:2023-06-lybra/contracts/lybra/miner/esLBRBoost.sol
41:        if (userStatus.unlockTime > block.timestamp) {
66:            uint256 time = block.timestamp > finishAt ? finishAt : block.timestamp;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/miner/esLBRBoost.sol#L41
```solidity
File:2023-06-lybra/contracts/lybra/pools/LybraStETHVault.sol
41:        require(sharesAmount > 0, "ZERO_DEPOSIT");
47:        if (mintAmount > 0) {
64:        require(excessAmount > 0 && stETHAmount > 0, "Only LSD excess income can be exchanged");
65:        uint256 realAmount = stETHAmount > excessAmount ? excessAmount : stETHAmount;
69:        if (payAmount > income) {
103:        if (time < 30 minutes) return 10000;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/pools/LybraStETHVault.sol#L41
```solidity
File:2023-06-lybra/contracts/lybra/miner/stakerewardV2pool.sol
84:        require(_amount > 0, "amount = 0");
94:        require(_amount > 0, "amount = 0");
113:        if (reward > 0) {
122:        require(finishAt < block.timestamp, "reward duration not finished");
140:        require(rewardRatio > 0, "reward ratio = 0");
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/miner/stakerewardV2pool.sol#L84
```solidity
File:2023-06-lybra/contracts/lybra/governance/LybraGovernance.sol
67:        return proposalData[proposalId].supportVotes[1] > proposalData[proposalId].supportVotes[0];
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/governance/LybraGovernance.sol#L67
```solidity
File:2023-06-lybra/contracts/lybra/token/PeUSDMainnetStableVision.sol
115:        require(peusdAmount <= userConvertInfo[msg.sender].mintedPeUSD &&peusdAmount > 0, "PCE");
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/token/PeUSDMainnetStableVision.sol#L115
```solidity
File:2023-06-lybra/contracts/lybra/miner/ProtocolRewardsPool.sol
92:        if (time2fullRedemption[msg.sender] > block.timestamp) {
102:        if (amount > 0) {
114:        require(block.timestamp + exitCycle - 3 days > time2fullRedemption[msg.sender], "ENW");
117:        if (amount > 0) {
132:        require(amount > 0, "QMG");
156:        if (time2fullRedemption[user] > lastWithdrawTime[user]) {
157:            amount = block.timestamp > time2fullRedemption[user] ? unstakeRatio[user] * (time2fullRedemption[user] - lastWithdrawTime[user]) : unstakeRatio[user] * (block.timestamp - lastWithdrawTime[user]);
162:        if (time2fullRedemption[user] > block.timestamp) {
192:        if (reward > 0) {
198:            if(reward > eUSDShare) {
205:                    if(peUSDBalance > 0) {
230:        require(amount > 0, "amount = 0");
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/miner/ProtocolRewardsPool.sol#L92
```solidity
File:2023-06-lybra/contracts/lybra/configuration/LybraConfigurator.sol
236:        for (uint256 i = 0; i < _contracts.length; i++) {
254:        if (fee > 10_000) revert('EL');
296:        if (balance > 1e21) {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/configuration/LybraConfigurator.sol#L236
```solidity
File:2023-06-lybra/contracts/lybra/miner/EUSDMiningIncentives.sol
94:        for (uint i = 0; i < _pools.length; i++) {
120:        require(finishAt < block.timestamp, "reward duration not finished");
138:        for (uint i = 0; i < pools.length; i++) {
189:        return (stakedLBRLpValue(user) * 10000) / stakedOf(user) < 500;
195:        if (reward > 0) {
208:        if (reward > 0) {
229:        require(amount > 0, "amount = 0");
237:        require(rewardRatio > 0, "reward ratio = 0");
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/miner/EUSDMiningIncentives.sol#L94
```solidity
File:2023-06-lybra/contracts/lybra/pools/base/LybraEUSDVaultBase.sol
82:        if (mintAmount > 0) {
100:        require(amount > 0, "ZERO_WITHDRAW");
108:        if (borrowed[msg.sender] > 0) {
128:        require(amount > 0, "ZERO_MINT");
157:        require(onBehalfOfCollateralRatio < badCollateralRatio, "Borrowers collateral ratio should below badCollateralRatio");
160:        require(EUSD.allowance(provider, address(this)) > 0, "provider should authorize to provide liquidation EUSD");
189:        require((totalDepositedAsset * assetPrice * 100) / poolTotalEUSDCirculation < badCollateralRatio, "overallCollateralRatio should below 150%");
191:        require(onBehalfOfCollateralRatio < 125 * 1e18, "borrowers collateralRatio should below 125%");
292:        if (((depositedAsset[_user] * _assetPrice * 100) / borrowed[_user]) < configurator.getSafeCollateralRatio(address(this))) revert("collateralRatio is Below safeCollateralRatio");
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L82
```solidity
File:2023-06-lybra/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol
65:        if (mintAmount > 0) {
84:        require(amount > 0, "ZA");
98:        require(amount > 0, "ZA");
112:        require(amount > 0, "ZA");
128:        require(onBehalfOfCollateralRatio < configurator.getBadCollateralRatio(address(this)), "Borrowers collateral ratio should below badCollateralRatio");
131:        require(PeUSD.allowance(provider, address(this)) > 0, "provider should authorize to provide liquidation EUSD");
216:        if (getBorrowedOf(_provider) > 0) {
226:        if (((depositedAsset[user] * price * 100) / getBorrowedOf(user)) < configurator.getSafeCollateralRatio(address(this)))
231:        if (block.timestamp > feeUpdatedAt[user]) {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L65

*Instances (60)*


### **[GAS-6]** Use nested if and, avoid multiple check combinations

Using nested is cheaper than using && multiple check combinations. There are more advantages, such as easier to read code and better coverage reports.
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/contracts/lybra/token/PeUSD.sol
46:        if (_from != address(this) && _from != spender)
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/token/PeUSD.sol#L46
```solidity
File:2023-06-lybra/contracts/lybra/token/LBR.sol
64:        if (_from != address(this) && _from != spender)
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/token/LBR.sol#L64
```solidity
File:2023-06-lybra/contracts/lybra/token/PeUSDMainnetStableVision.sol
199:        if (_from != address(this) && _from != spender)
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/token/PeUSDMainnetStableVision.sol#L199
```solidity
File:2023-06-lybra/contracts/lybra/pools/base/LybraEUSDVaultBase.sol
204:        if (msg.sender != provider && onBehalfOfCollateralRatio >= 1e20 + configurator.vaultKeeperRatio(address(this)) * 1e18) {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L204

*Instances (4)*

### **[GAS-7]** Using `private` rather than `public` for constants, saves gas

If needed, the values can be read from the verified contract source code, or if there are multiple values there can be a single getter function that [returns a tuple](https://github.com/code-423n4/2022-08-frax/blob/90f55a9ce4e25bceed3a74290b854341d8de6afa/src/contracts/FraxlendPair.sol#L156-L178) of the values of all currently-public constants. Saves **3406-3606 gas** in deployment gas due to the compiler not having to create non-payable getter functions for deployment calldata, not having to store the bytes of the value outside of where it's used, and not adding another entry to the method ID table
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/contracts/lybra/governance/GovernanceTimelock.sol
10:    bytes32 public constant DAO = keccak256("DAO");
11:    bytes32 public constant TIMELOCK = keccak256("TIMELOCK");
12:    bytes32 public constant ADMIN = keccak256("ADMIN");
13:    bytes32 public constant GOV = keccak256("GOV");
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/governance/GovernanceTimelock.sol#L10
```solidity
File:2023-06-lybra/contracts/lybra/configuration/LybraConfigurator.sol
76:    bytes32 public constant DAO = keccak256("DAO");
77:    bytes32 public constant TIMELOCK = keccak256("TIMELOCK");
78:    bytes32 public constant ADMIN = keccak256("ADMIN");
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/configuration/LybraConfigurator.sol#L76

*Instances (7)*

### **[GAS-8]** Splitting `require()` statements that use && saves gas

Instead of using the && operator in a single require statement to check multiple conditions,using multiple require statements with 1 condition per require statement will save 3 GAS per &&
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/contracts/lybra/pools/LybraStETHVault.sol
64:        require(excessAmount > 0 && stETHAmount > 0, "Only LSD excess income can be exchanged");
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/pools/LybraStETHVault.sol#L64
```solidity
File:2023-06-lybra/contracts/lybra/token/PeUSDMainnetStableVision.sol
115:        require(peusdAmount <= userConvertInfo[msg.sender].mintedPeUSD &&peusdAmount > 0, "PCE");
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/token/PeUSDMainnetStableVision.sol#L115
```solidity
File:2023-06-lybra/contracts/lybra/configuration/LybraConfigurator.sol
127:        require(newRatio >= 130 * 1e18 && newRatio <= 150 * 1e18 && newRatio <= vaultSafeCollateralRatio[pool] + 1e19, "LNA");
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/configuration/LybraConfigurator.sol#L127

*Instances (3)*


### **[GAS-9]** Inverting the condition of an if-else-statement wastes gas

Flipping the `true` and `false` blocks instead saves 3 gas
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/contracts/lybra/miner/esLBRBoost.sol
66:            uint256 time = block.timestamp > finishAt ? finishAt : block.timestamp;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/miner/esLBRBoost.sol#L66
```solidity
File:2023-06-lybra/contracts/lybra/pools/LybraStETHVault.sol
65:        uint256 realAmount = stETHAmount > excessAmount ? excessAmount : stETHAmount;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/pools/LybraStETHVault.sol#L65
```solidity
File:2023-06-lybra/contracts/lybra/miner/stakerewardV2pool.sol
148:        return x <= y ? x : y;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/miner/stakerewardV2pool.sol#L148
```solidity
File:2023-06-lybra/contracts/lybra/miner/ProtocolRewardsPool.sol
157:            amount = block.timestamp > time2fullRedemption[user] ? unstakeRatio[user] * (time2fullRedemption[user] - lastWithdrawTime[user]) : unstakeRatio[user] * (block.timestamp - lastWithdrawTime[user]);
196:            uint256 eUSDShare = balance >= reward ? reward : reward - balance;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/miner/ProtocolRewardsPool.sol#L157
```solidity
File:2023-06-lybra/contracts/lybra/miner/EUSDMiningIncentives.sol
245:        return x <= y ? x : y;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/miner/EUSDMiningIncentives.sol#L245
```solidity
File:2023-06-lybra/contracts/lybra/pools/base/LybraEUSDVaultBase.sol
115:        withdrawal = block.timestamp - 3 days >= depositedTime[user] ? amount : (amount * 999) / 1000;
277:        uint256 amount = borrowed[_onBehalfOf] >= _amount ? _amount : borrowed[_onBehalfOf];
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L115
```solidity
File:2023-06-lybra/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol
196:        uint256 amount = borrowed[_onBehalfOf] + totalFee >= _amount ? _amount : borrowed[_onBehalfOf] + totalFee;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L196

*Instances (9)*

### **[GAS-10]** Use double `if` statements instead of `&&`

If the if statement has a logical AND and is not followed by an else statement, it can be replaced with 2 if statements.
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/contracts/lybra/token/PeUSD.sol
46:        if (_from != address(this) && _from != spender)
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/token/PeUSD.sol#L46
```solidity
File:2023-06-lybra/contracts/lybra/token/LBR.sol
64:        if (_from != address(this) && _from != spender)
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/token/LBR.sol#L64
```solidity
File:2023-06-lybra/contracts/lybra/pools/LybraStETHVault.sol
64:        require(excessAmount > 0 && stETHAmount > 0, "Only LSD excess income can be exchanged");
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/pools/LybraStETHVault.sol#L64
```solidity
File:2023-06-lybra/contracts/lybra/token/PeUSDMainnetStableVision.sol
115:        require(peusdAmount <= userConvertInfo[msg.sender].mintedPeUSD &&peusdAmount > 0, "PCE");
199:        if (_from != address(this) && _from != spender)
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/token/PeUSDMainnetStableVision.sol#L115
```solidity
File:2023-06-lybra/contracts/lybra/configuration/LybraConfigurator.sol
127:        require(newRatio >= 130 * 1e18 && newRatio <= 150 * 1e18 && newRatio <= vaultSafeCollateralRatio[pool] + 1e19, "LNA");
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/configuration/LybraConfigurator.sol#L127
```solidity
File:2023-06-lybra/contracts/lybra/pools/base/LybraEUSDVaultBase.sol
204:        if (msg.sender != provider && onBehalfOfCollateralRatio >= 1e20 + configurator.vaultKeeperRatio(address(this)) * 1e18) {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L204

*Instances (7)*

### **[GAS-11]** Use constants instead of `type(uintx).max`

it uses more gas in the distribution process and also for each transaction than constant usage.
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/contracts/lybra/token/EUSD.sol
179:        if (currentAllowance != type(uint256).max) {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/token/EUSD.sol#L179

*Instances (1)*

### **[GAS-12]** Use shift Right/Left instead of division/multiplication if possible

A division/multiplication by any number x being a power of 2 can be calculated by shifting log2(x) to the right/left. While the DIV opcode uses 5 gas, the SHR opcode only uses 3 gas. Furthermore, Solidity's division operation also includes a division-by-0 prevention which is bypassed using shifting.
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/contracts/lybra/pools/LybraWbETHVault.sol
35:        return (_etherPrice() * IWBETH(address(collateralAsset)).exchangeRatio()) / 1e18;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/pools/LybraWbETHVault.sol#L35
```solidity
File:2023-06-lybra/contracts/lybra/token/esLBR.sol
20:    uint256 maxSupply = 100_000_000 * 1e18;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/token/esLBR.sol#L20
```solidity
File:2023-06-lybra/contracts/lybra/pools/LybraWstETHVault.sol
49:        return (_etherPrice() * IWstETH(address(collateralAsset)).stEthPerToken()) / 1e18;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/pools/LybraWstETHVault.sol#L49
```solidity
File:2023-06-lybra/contracts/lybra/pools/LybraRETHVault.sol
47:        return (_etherPrice() * IRETH(address(collateralAsset)).getExchangeRatio()) / 1e18;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/pools/LybraRETHVault.sol#L47
```solidity
File:2023-06-lybra/contracts/lybra/miner/esLBRBoost.sol
26:        esLBRLockSettings.push(esLBRLockSetting(30 days, 20 * 1e18));
27:        esLBRLockSettings.push(esLBRLockSetting(90 days, 30 * 1e18));
28:        esLBRLockSettings.push(esLBRLockSetting(180 days, 50 * 1e18));
29:        esLBRLockSettings.push(esLBRLockSetting(365 days, 100 * 1e18));
67:            return ((boostEndTime - userUpdatedAt) * maxBoost) / (time - userUpdatedAt);
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/miner/esLBRBoost.sol#L26
```solidity
File:2023-06-lybra/contracts/lybra/token/LBR.sol
15:    uint256 maxSupply = 100_000_000 * 1e18;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/token/LBR.sol#L15
```solidity
File:2023-06-lybra/contracts/lybra/pools/LybraStETHVault.sol
66:        uint256 payAmount = (((realAmount * getAssetPrice()) / 1e18) * getDutchAuctionDiscountPrice()) / 10000;
104:        return 10000 - (time / 30 minutes - 1) * 100;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/pools/LybraStETHVault.sol#L66
```solidity
File:2023-06-lybra/contracts/lybra/miner/stakerewardV2pool.sol
79:        return rewardPerTokenStored + (rewardRatio * (lastTimeRewardApplicable() - updatedAt) * 1e18) / totalSupply;
102:        return 100 * 1e18 + esLBRBoost.getUserBoost(_account, userUpdatedAt[_account], finishAt);
107:        return ((balanceOf[_account] * getBoost(_account) * (rewardPerToken() - userRewardPerTokenPaid[_account])) / 1e38) + rewards[_account];
134:            rewardRatio = _amount / duration;
136:            uint256 remainingRewards = (finishAt - block.timestamp) * rewardRatio;
137:            rewardRatio = (_amount + remainingRewards) / duration;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/miner/stakerewardV2pool.sol#L79
```solidity
File:2023-06-lybra/contracts/lybra/governance/LybraGovernance.sol
52:        return esLBR.getPastTotalSupply(timepoint) / 3;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/governance/LybraGovernance.sol#L52
```solidity
File:2023-06-lybra/contracts/lybra/token/PeUSDMainnetStableVision.sol
117:        uint256 share = (userConvertInfo[msg.sender].depositedEUSDShares * peusdAmount) / userConvertInfo[msg.sender].mintedPeUSD;
157:        return (share * configurator.flashloanFee()) / 10_000;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/token/PeUSDMainnetStableVision.sol#L117
```solidity
File:2023-06-lybra/contracts/lybra/miner/ProtocolRewardsPool.sol
93:            total += unstakeRatio[msg.sender] * (time2fullRedemption[msg.sender] - block.timestamp);
95:        unstakeRatio[msg.sender] = total / exitCycle;
134:        LBR.burn(msg.sender, (amount * grabFeeRatio) / 10000);
152:        amount = (a * (75e18 - ((time2fullRedemption[user] - block.timestamp) * 70e18) / (exitCycle / 1 days - 3) / 1 days)) / 100e18;
157:            amount = block.timestamp > time2fullRedemption[user] ? unstakeRatio[user] * (time2fullRedemption[user] - lastWithdrawTime[user]) : unstakeRatio[user] * (block.timestamp - lastWithdrawTime[user]);
163:            amount = unstakeRatio[user] * (time2fullRedemption[user] - block.timestamp);
168:        return ((stakedOf(_account) * (rewardPerTokenStored - userRewardPerTokenPaid[_account])) / 1e18) + rewards[_account];
209:                    uint256 tokenAmount = (reward - eUSDShare - peUSDBalance) * token.decimals() / 1e18;
233:            rewardPerTokenStored = rewardPerTokenStored + (share * 1e18) / totalStaked();
236:            rewardPerTokenStored = rewardPerTokenStored + (amount * 1e36 / token.decimals()) / totalStaked();
238:            rewardPerTokenStored = rewardPerTokenStored + (amount * 1e18) / totalStaked();
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/miner/ProtocolRewardsPool.sol#L93
```solidity
File:2023-06-lybra/contracts/lybra/configuration/LybraConfigurator.sol
127:        require(newRatio >= 130 * 1e18 && newRatio <= 150 * 1e18 && newRatio <= vaultSafeCollateralRatio[pool] + 1e19, "LNA");
200:            require(newRatio >= 160 * 1e18, "eUSD vault safe collateralRatio should more than 160%");
303:                uint256 amount = curvePool.exchange_underlying(0, 2, balance, balance * price * 998 / 1e21);
334:        if (vaultSafeCollateralRatio[pool] == 0) return 160 * 1e18;
357:        return (EUSD.totalSupply() * maxStableRatio) / 10_000;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/configuration/LybraConfigurator.sol#L127
```solidity
File:2023-06-lybra/contracts/lybra/miner/EUSDMiningIncentives.sol
50:    uint256 public extraRatio = 50 * 1e18;
51:    uint256 public peUSDExtraRatio = 10 * 1e18;
142:                borrowed = borrowed * (1e20 + peUSDExtraRatio) / 1e20;
153:        uint256 etherInLp = (IEUSD(0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2).balanceOf(ethlbrLpToken) * uint(etherPrice)) / 1e8;
154:        uint256 lbrInLp = (IEUSD(LBR).balanceOf(ethlbrLpToken) * uint(lbrPrice)) / 1e8;
156:        return (userStaked * (lbrInLp + etherInLp)) / totalLp;
168:        return rewardPerTokenStored + (rewardRatio * (lastTimeRewardApplicable() - updatedAt) * 1e18) / totalStaked();
181:        return 100 * 1e18 + redemptionBoost + esLBRBoost.getUserBoost(_account, userUpdatedAt[_account], finishAt);
185:        return ((stakedOf(_account) * getBoost(_account) * (rewardPerToken() - userRewardPerTokenPaid[_account])) / 1e38) + rewards[_account];
189:        return (stakedLBRLpValue(user) * 10000) / stakedOf(user) < 500;
210:            uint256 biddingFee = (reward * biddingFeeRatio) / 10000;
213:                biddingFee = biddingFee * uint256(lbrPrice) / 1e8;
231:            rewardRatio = amount / duration;
233:            uint256 remainingRewards = (finishAt - block.timestamp) * rewardRatio;
234:            rewardRatio = (amount + remainingRewards) / duration;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/miner/EUSDMiningIncentives.sol#L50
```solidity
File:2023-06-lybra/contracts/lybra/pools/base/LybraEUSDVaultBase.sol
21:    uint256 public immutable badCollateralRatio = 150 * 1e18;
115:        withdrawal = block.timestamp - 3 days >= depositedTime[user] ? amount : (amount * 999) / 1000;
156:        uint256 onBehalfOfCollateralRatio = (depositedAsset[onBehalfOf] * assetPrice * 100) / borrowed[onBehalfOf];
159:        require(assetAmount * 2 <= depositedAsset[onBehalfOf], "a max of 50% collateral can be liquidated");
161:        uint256 eusdAmount = (assetAmount * assetPrice) / 1e18;
164:        uint256 reducedAsset = (assetAmount * 11) / 10;
171:            reward2keeper = (reducedAsset * configurator.vaultKeeperRatio(address(this))) / 110;
189:        require((totalDepositedAsset * assetPrice * 100) / poolTotalEUSDCirculation < badCollateralRatio, "overallCollateralRatio should below 150%");
190:        uint256 onBehalfOfCollateralRatio = (depositedAsset[onBehalfOf] * assetPrice * 100) / borrowed[onBehalfOf];
191:        require(onBehalfOfCollateralRatio < 125 * 1e18, "borrowers collateralRatio should below 125%");
193:        uint256 eusdAmount = (assetAmount * assetPrice) / 1e18;
195:            eusdAmount = (eusdAmount * 1e20) / onBehalfOfCollateralRatio;
204:        if (msg.sender != provider && onBehalfOfCollateralRatio >= 1e20 + configurator.vaultKeeperRatio(address(this)) * 1e18) {
205:            reward2keeper = ((assetAmount * configurator.vaultKeeperRatio(address(this))) * 1e18) / onBehalfOfCollateralRatio;
236:        uint256 providerCollateralRatio = (depositedAsset[provider] * assetPrice * 100) / borrowed[provider];
237:        require(providerCollateralRatio >= 100 * 1e18, "provider's collateral ratio should more than 100%");
239:        uint256 collateralAmount = (((eusdAmount * 1e18) / assetPrice) * (10000 - configurator.redemptionFee())) / 10000;
292:        if (((depositedAsset[_user] * _assetPrice * 100) / borrowed[_user]) < configurator.getSafeCollateralRatio(address(this))) revert("collateralRatio is Below safeCollateralRatio");
301:        return (poolTotalEUSDCirculation * configurator.vaultMintFeeApy(address(this)) * (block.timestamp - lastReportTime)) / (86400 * 365) / 10000;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L21
```solidity
File:2023-06-lybra/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol
127:        uint256 onBehalfOfCollateralRatio = (depositedAsset[onBehalfOf] * assetPrice * 100) / getBorrowedOf(onBehalfOf);
130:        require(assetAmount * 2 <= depositedAsset[onBehalfOf], "a max of 50% collateral can be liquidated");
132:        uint256 peusdAmount = (assetAmount * assetPrice) / 1e18;
135:        uint256 reducedAsset = (assetAmount * 11) / 10;
141:            reward2keeper = (reducedAsset * configurator.vaultKeeperRatio(address(this))) / 110;
161:        uint256 providerCollateralRatio = (depositedAsset[provider] * assetPrice * 100) / borrowed[provider];
162:        require(providerCollateralRatio >= 100 * 1e18, "provider's collateral ratio should more than 100%");
164:        uint256 collateralAmount = (((peusdAmount * 1e18) / assetPrice) * (10000 - configurator.redemptionFee())) / 10000;
226:        if (((depositedAsset[user] * price * 100) / getBorrowedOf(user)) < configurator.getSafeCollateralRatio(address(this)))
238:        return (borrowed[user] * configurator.vaultMintFeeApy(address(this)) * (block.timestamp - feeUpdatedAt[user])) / (86400 * 365) / 10000;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L127

*Instances (81)*

### **[GAS-13]** Don't compare boolean expressions to boolean literals

if (<x> == true) => if (<x>), if (<x> == false) => if (!<x>).
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/contracts/lybra/governance/LybraGovernance.sol
82:        require(receipt.hasVoted == false, "GovernorBravo::castVoteInternal: voter already voted");
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/governance/LybraGovernance.sol#L82

*Instances (1)*

### **[GAS-14]** Write direct outcome, instead of performing mathematical operations 

Write direct outcome instead ofperforming mathematical operations saves some gas
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/contracts/lybra/miner/EUSDMiningIncentives.sol
50:    uint256 public extraRatio = 50 * 1e18;
51:    uint256 public peUSDExtraRatio = 10 * 1e18;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/miner/EUSDMiningIncentives.sol#L50
```solidity
File:2023-06-lybra/contracts/lybra/pools/base/LybraEUSDVaultBase.sol
21:    uint256 public immutable badCollateralRatio = 150 * 1e18;
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L21

*Instances (3)*

### **[GAS-15]** USE BYTES32 INSTEAD OF STRING

Use bytes32 instead of string to save gas whenever possible. String is a dynamic data structure and therefore is more gas consuming then bytes32.
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/contracts/lybra/governance/LybraGovernance.sol
38:      constructor(string memory name_, TimelockController timelock_, address _esLBR) GovernorTimelockControl(timelock_)  Governor(name_) {
151:     function CLOCK_MODE() public override view returns (string memory){
156:    function COUNTING_MODE() public override view virtual returns (string memory){
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/governance/LybraGovernance.sol#L38
```solidity
File:2023-06-lybra/contracts/lybra/token/EUSD.sol
99:    function name() public pure returns (string memory) {
107:    function symbol() public pure returns (string memory) {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/token/EUSD.sol#L99

*Instances (5)*

### **[GAS-16]** Compute known value `off-chain`

If You know what data to hash, there is no need to consume more computational power to hash it using keccak256 , you’ll end up consuming 2x amount of gas.
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/contracts/lybra/governance/GovernanceTimelock.sol
10:    bytes32 public constant DAO = keccak256("DAO");
11:    bytes32 public constant TIMELOCK = keccak256("TIMELOCK");
12:    bytes32 public constant ADMIN = keccak256("ADMIN");
13:    bytes32 public constant GOV = keccak256("GOV");
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/governance/GovernanceTimelock.sol#L10
```solidity
File:2023-06-lybra/contracts/lybra/configuration/LybraConfigurator.sol
76:    bytes32 public constant DAO = keccak256("DAO");
77:    bytes32 public constant TIMELOCK = keccak256("TIMELOCK");
78:    bytes32 public constant ADMIN = keccak256("ADMIN");
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/configuration/LybraConfigurator.sol#L76

*Instances (7)*

### **[GAS-17]** Part of the code can be pre calculated

If You know what data to hash, there is no need to consume more computational power to hash it using keccak256 , you’ll end up consuming 2x amount of gas.
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/contracts/lybra/governance/GovernanceTimelock.sol
10:    bytes32 public constant DAO = keccak256("DAO");
11:    bytes32 public constant TIMELOCK = keccak256("TIMELOCK");
12:    bytes32 public constant ADMIN = keccak256("ADMIN");
13:    bytes32 public constant GOV = keccak256("GOV");
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/governance/GovernanceTimelock.sol#L10
```solidity
File:2023-06-lybra/contracts/lybra/pools/LybraStETHVault.sol
26:        require(configurator.hasRole(keccak256("ADMIN"), msg.sender), "not authorized");
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/pools/LybraStETHVault.sol#L26
```solidity
File:2023-06-lybra/contracts/lybra/governance/LybraGovernance.sol
107:        require(GovernanceTimelock.checkOnlyRole(keccak256("TIMELOCK"), msg.sender), "not authorized");
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/governance/LybraGovernance.sol#L107
```solidity
File:2023-06-lybra/contracts/lybra/configuration/LybraConfigurator.sol
76:    bytes32 public constant DAO = keccak256("DAO");
77:    bytes32 public constant TIMELOCK = keccak256("TIMELOCK");
78:    bytes32 public constant ADMIN = keccak256("ADMIN");
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/configuration/LybraConfigurator.sol#L76

*Instances (9)*


### **[GAS-18]** Ternary operation is cheaper than if-else statement

There are instances where a ternary operation can be used instead of if-else statement. In these cases, using ternary operation will save modest amounts of gas.
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/contracts/lybra/miner/esLBRBoost.sol
65:        } else {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/miner/esLBRBoost.sol#L65
```solidity
File:2023-06-lybra/contracts/lybra/pools/LybraStETHVault.sol
84:        } else {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/pools/LybraStETHVault.sol#L84
```solidity
File:2023-06-lybra/contracts/lybra/miner/stakerewardV2pool.sol
135:        } else {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/miner/stakerewardV2pool.sol#L135
```solidity
File:2023-06-lybra/contracts/lybra/miner/ProtocolRewardsPool.sol
204:                } else {
213:            } else {
237:        } else {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/miner/ProtocolRewardsPool.sol#L204
```solidity
File:2023-06-lybra/contracts/lybra/token/EUSD.sol
303:        } else {
314:        } else {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/token/EUSD.sol#L303
```solidity
File:2023-06-lybra/contracts/lybra/configuration/LybraConfigurator.sol
201:        } else {
301:            } else {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/configuration/LybraConfigurator.sol#L201
```solidity
File:2023-06-lybra/contracts/lybra/miner/EUSDMiningIncentives.sol
217:            } else {
232:        } else {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/miner/EUSDMiningIncentives.sol#L217
```solidity
File:2023-06-lybra/contracts/lybra/pools/base/LybraEUSDVaultBase.sol
170:        } else {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L170
```solidity
File:2023-06-lybra/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol
140:        } else {
201:        } else {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L140

*Instances (15)*

### **[GAS-19]** Sort Solidity operations using short-circuit mode

Short-circuiting is a solidity contract development model that uses OR/AND logic to sequence different cost operations. It puts low gas cost operations in the front and high gas cost operations in the back, so that if the front is low If the cost operation is feasible, you can skip (short-circuit) the subsequent high-cost Ethereum virtual machine operation.
#### <ins>Proof Of Concept</ins>

```solidity
File:2023-06-lybra/contracts/lybra/miner/esLBRBoost.sol
60:        if (userUpdatedAt >= boostEndTime || userUpdatedAt >= finishAt) {
63:        if (finishAt <= boostEndTime || block.timestamp <= boostEndTime) {
```

#### <ins>Code Snippet</ins>
https://github.com/code-423n4/2023-06-lybra/contracts/lybra/miner/esLBRBoost.sol#L60

*Instances (2)*
