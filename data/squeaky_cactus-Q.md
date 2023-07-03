# QA Report : Lybra Finance

| Id    | Issue                                                                                                                                |
|-------|--------------------------------------------------------------------------------------------------------------------------------------|
| L-1   | [Governance Quorum assumes extremely high participation](#l-1-governance-quorum-assumes-extremely-high-participation)                |
| L-2   | [Governance Quorum cannot be called with current block number ](#l-2-governance-quorum-cannot-be-called-with-current-block-number)   |
| L-3   | [Governance Quorum assumes extremely high participation](#l-3-governance-getvotes-cannot-be-called-with-current-block-number)        |
| L-4   | [EUSDMiningIncentives has incorrect name in contract NatSpec](#l-4-eusdminingincentives-has-incorrect-name-in-contract-natspec)      |
| L-5   | [Unguarded loop over a dynamically sized array](#l-5-unguarded-loop-over-a-dynamically-sized-array)                                  |
| L-6   | [Function modifier side affects](#l-6-function-modifier-side-affects)                                                                |
| L-7   | [Greater Than or Equal To is used instead of Greater Than](#l-7-greater-than-or-equal-to-is-used-instead-of-greater-than)            |
| L-8   | [Assuming unlimited allowance grant](#l-8-assuming-unlimited-allowance-grant)                                                        |
| L-9   | [Modifier name is the opposite of behaviour](#l-9-modifier-name-is-the-opposite-of-behaviour)                                        |
| NC-1  | [Commented out constructor](#nc-1-commented-out-constructor)                                                                         |
| NC-2  | [Commented out execute](#nc-2-commented-out-execute )                                                                                |
| NC-3  | [Commented out timelock](#nc-3-commented-out-timelock )                                                                              |
| NC-4  | [Sponsor in event should be the provider](#nc-4-sponsor-in-event-should-be-the-provider )                                            |
| NC-5  | [Misleading event on bad collateral ratio change](#nc-5-misleading-event-on-bad-collateral-ratio-change)                             |
| NC-6  | [Revert message refers to eUSD instead of PeUSD](#nc-6-revert-message-refers-to-eusd-instead-of-peusd)                               |
| NC-7  | [Function NatSpec refers to non-existent parameter](#nc-7-function-natspec-refers-to-non-existent-parameter)                         |
| NC-8  | [Misleading revert message](#nc-8-misleading-revert-message)                                                                         |





## Low Risk

### L-1 Governance Quorum assumes extremely high participation

The quorum amount uses 33% of the esLBR total supply, meaning an extremely high participation rate is being assumed.

```    
/contracts/lybra/governance/LybraGovernance:52
51    function quorum(uint256 timepoint) public view override returns (uint256){
52        return esLBR.getPastTotalSupply(timepoint) / 3;
```
The [OpenZepplin Governor](https://docs.openzeppelin.com/contracts/4.x/governance#governor) states that most governors use 4% of total supply.


### L-2 Governance Quorum cannot be called with current block number

The public function `quorum()` can only be called with previous (non-current) `block.number`. As `quorum()` is a public function, without any NatSpec or signature detailing the requirement that `block.number` is invalid input, it is reasonable to assume the function would not revert.

Using the `block.number` for the clock, parameter `timepoint` is a `block.number` and `quorum()` cannot be called with the current `block.number` as the `getPastTotalSupply()` reverts unless `timepoint` is in the past.

```    
/contracts/lybra/governance/LybraGovernance:52
51    function quorum(uint256 timepoint) public view override returns (uint256){
52        return esLBR.getPastTotalSupply(timepoint) / 3;
```

```    
/@openzeppelin/contracts/token/ERC20/extensions/ERC20Votes.sol:96
95    function getPastTotalSupply(uint256 timepoint) public view virtual override returns (uint256) {
96        require(timepoint < clock(), "ERC20Votes: future lookup");
```

### L-3 Governance GetVotes cannot be called with current block number

The public function `getVotes()` invokes `_getVotes()` that can only be called with previous (non-current) `block.number`. As `getVotes()` is a public function, without any NatSpec or signature detailing the requirement thaty `block.number` is invalid input, it is reasonable to assume the function would not revert.

Using the `block.number` as the clock, `timepoint` is a `block.number` and `_getVotes()` cannot be called with the current `block.number` as the `getPastVotes()` reverts unless `timepoint` is in the past.

```    
/contracts/lybra/governance/LybraGovernance:100
98     function _getVotes(address account, uint256 timepoint, bytes memory) internal view override returns (uint256){
99
100        return esLBR.getPastVotes(account, timepoint);
```

'getVotes()' is from OZ Governor whose NatSpec inherits from the IGovernor interface:
```    
/@openzeppelin/contracts/governance/IGovernor.sol:190
190    /**
191     * @notice module:reputation
192     * @dev Voting power of an `account` at a specific `timepoint`.
193     *
194     * Note: this can be implemented in a number of ways, for example by reading the delegated balance from one (or
195     * multiple), {ERC20Votes} tokens.
196     */
197    function getVotes(address account, uint256 timepoint) public view virtual returns (uint256);
```

### L-4 EUSDMiningIncentives has incorrect name in contract NatSpec

The contract is referred to as `tokenMiner`, while the contract is currently named `EUSDMiningIncentives`:
```
/contracts/lybra/miner/EUSDMiningIncentives.sol:6
5   /**
6   * @title tokenMiner is a stripped down version of Synthetix StakingRewards.sol, to reward esLBR to EUSD minters.
7   * Differences from the original contract,
```

### L-5 Unguarded loop over a dynamically sized array

The `stakedOf` function on `EUSDMiningIncentives` uses a loop during calculation, fine when called from an EOA, 
however without any programmatic guard or NatSpec warning, calling the function from another contract could cause 
reverts due to unknown gas usage.

```
/contracts/lybra/miner/EUSDMiningIncentives.sol:6

    function stakedOf(address user) public view returns (uint256) {
        uint256 amount;
        for (uint i = 0; i < pools.length; i++) {
            ILybra pool = ILybra(pools[i]);
            uint borrowed = pool.getBorrowedOf(user);
            if (pool.getVaultType() == 1) {
                borrowed = borrowed * (1e20 + peUSDExtraRatio) / 1e20;
            }
            amount += borrowed;
        }
        return amount;
    }
```

Recommend adding a NatSpec comment with a warning about not calling from another contract due to looping 
and associated unknown gas usage.


### L-6 Function modifier side affects

Best practice is to have no side effect in function modifiers, but rather to keep their usage strictly as guards, as part of the Check, 
Effect and Interact pattern [ConsenSys recommendation](https://consensys.github.io/smart-contract-best-practices/development-recommendations/solidity-specific/modifiers-as-guards/)

3 instances:

```
/contracts/lybra/miner/EUSDMiningIncentives.sol:73:83

73    modifier updateReward(address _account) {
74        rewardPerTokenStored = rewardPerToken();
75        updatedAt = lastTimeRewardApplicable();
76
77        if (_account != address(0)) {
78            rewards[_account] = earned(_account);
79            userRewardPerTokenPaid[_account] = rewardPerTokenStored;
80            userUpdatedAt[_account] = block.timestamp;
81        }
82        _;
83    }
```

```
/contracts/lybra/miner/ProtocolRewardsPool.sol:175-182

175    /**
176     * @dev Call this function when deposit or withdraw ETH on Lybra and update the status of corresponding user.
177     */
178    modifier updateReward(address account) {
179        rewards[account] = earned(account);
180        userRewardPerTokenPaid[account] = rewardPerTokenStored;
181        _;
182    }
```

```
/contracts/lybra/miner/ProtocolRewardsPool.sol:56-66
56    modifier updateReward(address _account) {
57        rewardPerTokenStored = rewardPerToken();
58        updatedAt = lastTimeRewardApplicable();
59
60        if (_account != address(0)) {
61            rewards[_account] = earned(_account);
62            userRewardPerTokenPaid[_account] = rewardPerTokenStored;
63            userUpdatedAt[_account] = block.timestamp;
64        }
65        _;
66    }
```

Recommendation is to move the logic into a function with explicit calls after any existing checks.


### L-7 Greater Than or Equal To is used instead of Greater Than

`unsteak()` in `ProtocolRewardsPool` has a requirement stated in the function NatSpec to use greater than, but the 
implementation is using greater than or equal to.

```
/contracts/lybra/miner/ProtocolRewardsPool.sol:82-88
82     * Requirements:
83     * The current time must be greater than the unlock time retrieved from the boost contract for the user.
84     * Effects:
85     * Resets the user's vesting data, entering a new vesting period, when converting to LBR.
86     */
87    function unstake(uint256 amount) external {
88        require(block.timestamp >= esLBRBoost.getUnlockTime(msg.sender), "Your lock-in period has not ended. You can't convert your esLBR now.");
```

### L-8 Assuming unlimited allowance grant

`ProtocolRewardsPool`, `LybraPeUSDVaultBase` and `LybraEUSDVaultBase` are assuming unlimited allowance,
rather than checking the known amount of allowance is granted.
If the user has granted an amount, the transaction would revert later on, however it could be reverted here instead 
(earlier on, saving some gas).

There is a requirement in the NatSpec in `ProtocolRewardsPool` stating there must be an allowance granted, not that allowance must be unlimited.

```
/contracts/lybra/miner/ProtocolRewardsPool.sol:151
151     * - provider should authorize Lybra to utilize EUSD
```

3 Instances

```
/contracts/lybra/miner/ProtocolRewardsPool.sol:160-161

160        require(EUSD.allowance(provider, address(this)) > 0, "provider should authorize to provide liquidation EUSD");
161        uint256 eusdAmount = (assetAmount * assetPrice) / 1e18;
```

```
/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol:131

131        require(PeUSD.allowance(provider, address(this)) > 0, "provider should authorize to provide liquidation EUSD");
132        uint256 peusdAmount = (assetAmount * assetPrice) / 1e18;
```

```
/contracts/lybra/pools/base/LybraEUSDVaultBase.sol:160

 160       require(EUSD.allowance(provider, address(this)) > 0, "provider should authorize to provide liquidation EUSD");
 161       uint256 eusdAmount = (assetAmount * assetPrice) / 1e18;
```



Recommend `eusdAmount` is calculated before require statement and the check for that amount of allowance being granted.


### L-9 Modifier name is the opposite of behaviour

A modifier has the purpose of not only prefixes code to a function, but also provide implicit documentation, through 
the name the modifier has. 
When the modifier name does not reflect the behaviour, then it'll cause problems due to assumptions, especially if elsewhere
the names do reflect the behaviour.

In `EUSD` and `PeUSDMainnetStableVision` there are two function where the logic is the inversion of the name.

Instances: 4

```
/contracts/lybra/token/EUSD.sol:83-86

83    modifier MintPaused() {
84        require(!configurator.vaultMintPaused(msg.sender), "MPP");
85        _;
86      }
```

```
/contracts/lybra/token/EUSD.sol:46-49

46    modifier BurnPaused() {
47        require(!configurator.vaultBurnPaused(msg.sender), "BPP");
48        _;
49    }
```

```
/contracts/lybra/token/PeUSDMainnetStableVision.sol:83-86

83    modifier MintPaused() {
84        require(!configurator.vaultMintPaused(msg.sender), "MPP");
85        _;
86      }
```

```
/contracts/lybra/token/PeUSDMainnetStableVision.sol:50-53

50    modifier BurnPaused() {
51        require(!configurator.vaultBurnPaused(msg.sender), "BPP");
52        _;
53    }
```

Recommendation: rename `MintPaused` to `mintEnabled` and `BurnPaused` to `burnEnabled`.





## Non-Critical


### NC-1 Commented out constructor

Comment of ``// contructor()`` does not apply to subsequent code and should be removed.

```    
/contracts/lybra/governance/GovernanceTimelock:9
9      // contructor()
10     bytes32 public constant DAO = keccak256("DAO");
```

### NC-2 Commented out execute

Dead code, it is a duplicate of the logic that is executed in `super.execute()` called prior to the comment, however 
as only a close function brace follow this comment line, it serves no purpose.

```    
/contracts/lybra/governance/LybraGovernance:109
108        super._execute(1, targets, values, calldatas, descriptionHash);
109        // _timelock.executeBatch{value: msg.value}(targets, values, calldatas, 0, descriptionHash);
```

### NC-3 Commented out timelock

Dead code, it seems previously a field and reference assignment were used, however those lines now unused should be removed.

```    
/contracts/lybra/governance/GovernanceTimelock:37,38
37        // TimelockController timelockAddress;
39        // timelock = timelock_;
```


### NC-4 Sponsor in event should be the provider

The first argument for the `Mint` event is `sponsor`, which in `_mintEUSD` corresponds to the parameter `_provider`.

Although `msg.sender` is the value sent as `_provider`, it is better practice to use the `_provider` as the code would 
read nicely and if the function is ever called elsewhere (e.g. inherited) with a value other than
`msg.sender` passed as `_provider` the log event would still be correct.

```    
/contracts/lybra/pools/base/LybraEUSDVaultBase:259-270

259    function _mintEUSD(address _provider, address _onBehalfOf, uint256 _mintAmount, uint256 _assetPrice) internal virtual {
260        require(poolTotalEUSDCirculation + _mintAmount <= configurator.mintVaultMaxSupply(address(this)), "ESL");
261        try configurator.refreshMintReward(_provider) {} catch {}
262        borrowed[_provider] += _mintAmount;
263
264        EUSD.mint(_onBehalfOf, _mintAmount);
265        _saveReport();
266        poolTotalEUSDCirculation += _mintAmount;
267        _checkHealth(_provider, _assetPrice);
268
269        emit Mint(msg.sender, _onBehalfOf, _mintAmount, block.timestamp);
270    }
```


### NC-5 Misleading event on bad collateral ratio change

When the bad collateral ratio is changed for a pool in `Configurator`, the SafeCollateralRatioChanged event is emitted,
which is misleading as there the safe collateral has a different meaning and is also configurable separately, where 
the same event is also being emitted.

```
/contracts/lybra/configuration/Configurator:129

126    function setBadCollateralRatio(address pool, uint256 newRatio) external onlyRole(DAO) {
127        require(newRatio >= 130 * 1e18 && newRatio <= 150 * 1e18 && newRatio <= vaultSafeCollateralRatio[pool] + 1e19, "LNA");
128        vaultBadCollateralRatio[pool] = newRatio;
129        emit SafeCollateralRatioChanged(pool, newRatio);
130    }
```

Recommend creating a `BadCollateralRatioChanged` event and using that instead.


### NC-6 Revert message refers to eUSD instead of PeUSD

The provider needs to have granted allowance on the PeUSD token contract, not the eUSD token contract.

```
/contracts/lybra/pools/base/LybraPeUSDVaultBase:131

131        require(PeUSD.allowance(provider, address(this)) > 0, "provider should authorize to provide liquidation EUSD");
```

Recommend changing `EUSD` to `PeUSD` in the revert message.


### NC-7 Function NatSpec refers to non-existent parameter

The NatSpec for `_repay` refers to `_provideramount`, which does not exist in the parameter set.

```
/contracts/lybra/pools/base/LybraPeUSDVaultBase:187-192

187    /**
188     * @notice Burn _provideramount PeUSD to payback minted PeUSD for _onBehalfOf.
189     *
190     * @dev Refresh LBR reward before reducing providers debt. Refresh Lybra generated service fee before reducing totalPeUSDCirculation.
191     */
192    function _repay(address _provider, address _onBehalfOf, uint256 _amount) internal virtual {
```

Recommend changing `@notice` description to: Burn upto _amount PeUSD from _provider to payback a portion of borrowed by _onBehalfOf


### NC-8 Misleading revert message

Revert message of `not authorized` implies the caller lacks the authorization, which is misleading as the function is 
simply unsupported.

```
/contracts/lybra/token/esLBR:27

26    function _transfer(address, address, uint256) internal virtual override {
27        revert("not authorized");
28    }
```

Recommendation: change revert message to `unsupported`

