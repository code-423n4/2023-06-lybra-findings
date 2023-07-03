No | Issue |Instances|
|-|:-|:-:|:-:|
|G-1 |bytes constants are more eficient than string constans|5
|G-2 |Using calldata instead of memory for read-only arguments in external functions saves gas|3
|G-3 |Use assembly to check for address(0)|18
|G-4 |Use hardcode address instead address(this)|21
|G-5 |Use do while loops instead of for loops|3
|G-6 |Use assembly to perform efficient back-to-back calls|1
|G-7 |Unnecessary look up in if condition|3
|G-8 |Avoid emitting constants.|3
|G-9 |Using delete instead of setting info struct to 0 saves gas|6
|G-10|Ternary operation is cheaper than if-else statement|11
|G-11|Use function instead of modifiers |5
## [G-1] bytes constants are more eficient than string constans
If the data can fit in 32 bytes, the bytes32 data type can be used instead of bytes or strings, as it is less robust in terms of robustness.

```solidity
file: LybraGovernance.sol
 
38  constructor(string memory name_, TimelockController timelock_, address _esLBR) GovernorTimelockControl(timelock_)  Governor(name_) 

151   function CLOCK_MODE() public override view returns (string memory)

156     function COUNTING_MODE() public override view virtual returns (string memory)
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol#L38
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol#L151
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol#L156

```solidity
file: lybra/token/EUSD.sol

99    function name() public pure returns (string memory) 

107    function symbol() public pure returns (string memory)
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L99
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L107

## [G‑2] Using calldata instead of memory for read-only arguments in external functions saves gas
When a function with a memory array is called externally, the abi.decode() step has to use a for-loop to copy each index of the calldata to the memory index. Each iteration of this for-loop costs at least 60 gas
```solidity

file: lybra/miner/esLBRBoost.sol

33    function addLockSetting(esLBRLockSetting memory setting) external onlyOwner {
        esLBRLockSettings.push(setting);
    }

38   function setLockStatus(uint256 id) external {
        esLBRLockSetting memory _setting = esLBRLockSettings[id];
        LockStatus memory userStatus = userLockStatus[msg.sender];
        if (userStatus.unlockTime > block.timestamp) {
            require(userStatus.duration <= _setting.duration, "Your lock-in period has not ended, and the term can only be extended, not reduced.");
        }
        userLockStatus[msg.sender] = LockStatus(block.timestamp + _setting.duration, _setting.duration, _setting.miningBoost);
    }

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol#L33
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol#L38-L40

```solidity

file: lybra/miner/EUSDMiningIncentives.sol

93  function setPools(address[] memory _pools) external onlyOwner

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L93

## [G-03] Use assembly to check for address(0)
Saves 6 gas per instance

```solidity

file: lybra/miner/stakerewardV2pool.sol

60   if (_account != address(0))

132   function notifyRewardAmount(uint256 _amount) external onlyOwner updateReward(address(0))
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L60
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L132

```solidity

file: lybra/token/PeUSDMainnetStableVision.sol

63   function mint(address to, uint256 amount) external onlyMintVault MintPaused returns (bool) {
62        require(to != address(0), "TZA");
        _mint(to, amount);
        return true;
    }
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L64

```solidity

file: lybra/miner/ProtocolRewardsPool.sol

213 else {
                emit ClaimReward(msg.sender, EUSD.getMintedEUSDByShares(eUSDShare), address(0), 0, block.timestamp);
            }
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L213

```solidity

file: lybra/token/EUSD.sol

366     function _approve(address _owner, address _spender, uint256 _amount) internal {
        require(_owner != address(0), "APPROVE_FROM_ZERO_ADDRESS");
        require(_spender != address(0), "APPROVE_TO_ZERO_ADDRESS");

        allowances[_owner][_spender] = _amount;
        emit Approval(_owner, _spender, _amount);
372    }

366   function _transferShares(address _sender, address _recipient, uint256 _sharesAmount) internal {
        require(_sender != address(0), "TRANSFER_FROM_THE_ZERO_ADDRESS");
        require(_recipient != address(0), "TRANSFER_TO_THE_ZERO_ADDRESS");

        uint256 currentSenderShares = shares[_sender];
        require(_sharesAmount <= currentSenderShares, "TRANSFER_AMOUNT_EXCEEDS_BALANCE");

        shares[_sender] = currentSenderShares.sub(_sharesAmount);
        shares[_recipient] = shares[_recipient].add(_sharesAmount);
400    }

411 
    function mint(address _recipient, uint256 _mintAmount) external onlyMintVault MintPaused returns (uint256 newTotalShares) {
        require(_recipient != address(0), "MINT_TO_THE_ZERO_ADDRESS");

427        emit Transfer(address(0), _recipient, _mintAmount);  

440 function burnShares(address _account, uint256 _sharesAmount) external onlyMintVault BurnPaused returns (uint256 newTotalShares) {
        require(_account != address(0), "BURN_FROM_THE_ZERO_ADDRESS");
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L366-L372
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L391-L400
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L411
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L427
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L440

```solidity

file: lybra/configuration/LybraConfigurator.sol

98   function initToken(address _eusd, address _peusd) external onlyRole(DAO) {
        if (address(EUSD) == address(0)) EUSD = IEUSD(_eusd);
        if (address(peUSD) == address(0)) peUSD = IEUSD(_peusd);
101    }


```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L98-L101

```solidity
file: lybra/miner/EUSDMiningIncentives.sol#L

76       if (_account != address(0)) {
            rewards[_account] = earned(_account);
            userRewardPerTokenPaid[_account] = rewardPerTokenStored;
            userUpdatedAt[_account] = block.timestamp;
80        }

226    function notifyRewardAmount(
        uint256 amount
228    ) external onlyOwner updateReward(address(0))

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L76-L80
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L226-L228

```solidity

file: lybra/pools/base/LybraEUSDVaultBase.sol

98    function withdraw(address onBehalfOf, uint256 amount) external virtual {
        require(onBehalfOf != address(0), "TZA");
126  function mint(address onBehalfOf, uint256 amount) external {
        require(onBehalfOf != address(0), "MINT_TO_THE_ZERO_ADDRESS");
140   function burn(address onBehalfOf, uint256 amount) external {
        require(onBehalfOf != address(0), "BURN_TO_THE_ZERO_ADDRESS");
        _repay(msg.sender, onBehalfOf, amount);
    }

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L98
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L126
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L140

```solidity

file: /lybra/pools/base/LybraPeUSDVaultBase.sol

82     function withdraw(address onBehalfOf, uint256 amount) external virtual {
        require(onBehalfOf != address(0), "TZA");
        require(amount > 0, "ZA");
        _withdraw(msg.sender, onBehalfOf, amount);
85    }

96    function mint(address onBehalfOf, uint256 amount) external virtual {
        require(onBehalfOf != address(0), "TZA");

 110  function burn(address onBehalfOf, uint256 amount) external virtual {
        require(onBehalfOf != address(0), "TZA");       

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L82
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L96
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L110

## [G-04] Use hardcode address instead address(this)
Instead of using address(this), it is more gas-efficient to pre-calculate and use the hardcoded address.

```solidity

file: lybra/governance/GovernanceTimelock.sol

20 _grantRole(DAO, address(this));


```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/GovernanceTimelock.sol#L20

```solidity

file: 

22 
        uint256 preBalance = collateralAsset.balanceOf(address(this));
        IWBETH(address(collateralAsset)).deposit{value: msg.value}(address(configurator));
24        uint256 balance = collateralAsset.balanceOf(address(this));
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWbETHVault.sol#L22-L24

```solidity
file: 
29 
        uint256 preBalance = collateralAsset.balanceOf(address(this));
        rkPool.deposit{value: msg.value}();
31        uint256 balance = collateralAsset.balanceOf(address(this));

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraRETHVault.sol#L29-L31

```solidity

file: lybra/miner/stakerewardV2pool.sol
85   bool success = stakingToken.transferFrom(msg.sender, address(this), _amount);

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L85

```solidity

file:lybra/token/PeUSDMainnetStableVision.sol

80      require(_msgSender() == user || _msgSender() == address(this), "MDM");
81        require(EUSD.balanceOf(address(this)) + eusdAmount <= configurator.getEUSDMaxLocked(),"ESL");
82      bool success = EUSD.transferFrom(user, address(this), eusdAmount);

133       bool success = EUSD.transferFrom(address(receiver), address(this), EUSD.getMintedEUSDByShares(shareAmount));

175     function token() public view virtual override returns (address) {
        return address(this);
    }

199    if (_from != address(this) && _from != spender)

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L80-L82
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L133
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L175
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L199

```solidity
file: lybra/miner/ProtocolRewardsPool.sol

195    uint256 balance = EUSD.sharesOf(address(this));

200    uint256 peUSDBalance = peUSD.balanceOf(address(this));

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L195
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L200

```solidity
file: lybra/configuration/LybraConfigurator.sol

290    uint256 peUSDBalance = peUSD.balanceOf(address(this));

295   uint256 balance = EUSD.balanceOf(address(this));
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L290
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L295

```solidity

file: lybra/pools/base/LybraEUSDVaultBase.sol#

75 
        bool success = collateralAsset.transferFrom(msg.sender, address(this), assetAmount);

160 
        require(EUSD.allowance(provider, address(this)) > 0, "provider should authorize to provide liquidation EUSD");   

171    reward2keeper = (reducedAsset * configurator.vaultKeeperRatio(address(this))) / 110;    

197   require(EUSD.allowance(provider, address(this)) >= eusdAmount, "provider should authorize to provide liquidation EUSD");

204  if (msg.sender != provider && onBehalfOfCollateralRatio >= 1e20 + configurator.vaultKeeperRatio(address(this)) * 1e18) {
            reward2keeper = ((assetAmount * configurator.vaultKeeperRatio(address(this))) * 1e18) / onBehalfOfCollateralRatio;
260    require(poolTotalEUSDCirculation + _mintAmount <= configurator.mintVaultMaxSupply(address(this)), "ESL");

292   if (((depositedAsset[_user] * _assetPrice * 100) / borrowed[_user]) < configurator.getSafeCollateralRatio(address(this))) revert("collateralRatio is Below safeCollateralRatio");

300  function _newFee() internal view returns (uint256) {
        return (poolTotalEUSDCirculation * configurator.vaultMintFeeApy(address(this)) * (block.timestamp - lastReportTime)) / (86400 * 365) / 10000;
    }
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L75
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L160
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L171
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L197
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L204-L205
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L260
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L292
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L300

## [G-05] Use do while loops instead of for loops

A do while loop will cost less gas since the condition is not being checked for the first iteration.

```solidity

file: LybraConfigurator.sol

236    for (uint256 i = 0; i < _contracts.length; i++) {
            tokenMiner[_contracts[i]] = _bools[i];
            emit tokenMinerChanges(_contracts[i], _bools[i]);
        }


```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L236

```solidity

file: lybra/miner/EUSDMiningIncentives.sol

94     for (uint i = 0; i < _pools.length; i++) {
            require(configurator.mintVault(_pools[i]), "NOT_VAULT");
        }

138  for (uint i = 0; i < pools.length; i++) 
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L94
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L138

## [G-6] Use assembly to perform efficient back-to-back calls
If a similar external call is performed back-to-back, we can use assembly to reuse any function signatures and function parameters that stay the same. In addition, we can also reuse the same memory space for each function call (scratch space + free memory pointer + zero slot), which can potentially allow us to avoid memory expansion costs.

Note: In order to do this optimization safely we will cache the free memory pointer value and restore it once we are done with our function calls. We will also set the zero slot back to 0 if neccessary.

```solidity

file: lybra/pools/LybraWstETHVault.sol

9 interface IWstETH {
    function stEthPerToken() external view returns (uint256);

    function wrap(uint256 _stETHAmount) external returns (uint256);
}

interface Ilido {
    function submit(address _referral) external payable returns (uint256 StETH);

    function approve(address spender, uint256 amount) external returns (bool);
19 }


```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWstETHVault.sol#L9-L19

## [G-7] Unnecessary look up in if condition
If the || condition isn’t required, the second condition will have been looked up unnecessarily.

```solidity

file: lybra/miner/esLBRBoost.sol

60 if (userUpdatedAt >= boostEndTime || userUpdatedAt >= finishAt)

63   if (finishAt <= boostEndTime || block.timestamp <= boostEndTime)

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol#L60
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol#L63

```solidity

file: LybraConfigurator.sol

298   if(!premiumTradingEnabled || price <= 1005000)


```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L298

## [G-08] Avoid emitting constants.
A log topic (declared with indexed) has a gas cost of Glogtopic (375 gas).

```solidity

file: lybra/miner/ProtocolRewardsPool.sol

76  emit StakeLBR(msg.sender, amount, block.timestamp);

106    emit WithdrawLBR(user, amount, block.timestamp);

```
ithub.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L76
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L106

```solidity

file: lybra/pools/base/LybraEUSDVaultBase.sol

111     emit WithdrawAsset(msg.sender, address(collateralAsset), onBehalfOf, withdrawal, block.timestamp);


```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L111

## [G-09] Using delete instead of setting info struct to 0 saves gas

```solidity

file: lybra/token/PeUSDMainnetStableVision.sol

33   mapping(address => ConvertInfo) public userConvertInfo;

35    struct ConvertInfo {
        uint256 depositedEUSDShares;
        uint256 mintedPeUSD;
    }

85  userConvertInfo[user].depositedEUSDShares += share;
        userConvertInfo[user].mintedPeUSD += eusdAmount;

114    function convertToEUSD(uint256 peusdAmount) external {
        require(peusdAmount <= userConvertInfo[msg.sender].mintedPeUSD &&peusdAmount > 0, "PCE");
        _burn(msg.sender, peusdAmount);
        uint256 share = (userConvertInfo[msg.sender].depositedEUSDShares * peusdAmount) / userConvertInfo[msg.sender].mintedPeUSD;
        userConvertInfo[msg.sender].mintedPeUSD -= peusdAmount;
        userConvertInfo[msg.sender].depositedEUSDShares -= share;
        EUSD.transferShares(msg.sender, share);
121    }

168 
        return EUSD.getMintedEUSDByShares(userConvertInfo[user].depositedEUSDShares) - userConvertInfo[user].mintedPeUSD;    

          
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L33
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L35
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L85-L86
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L114-L121
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L168

```solidity

file: ybra/miner/EUSDMiningIncentives.sol

124 124    function setEthlbrStakeInfo(address _pool, address _lp) external onlyOwner {
        ethlbrStakePool = _pool;
        ethlbrLpToken = _lp;
    }  

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L124

## [G-10] Ternary operation is cheaper than if-else statement
There are instances where a ternary operation can be used instead of if-else statement. In these cases, using ternary operation will save modest amounts of gas.

```solidity

file: lybra/miner/esLBRBoost.sol

63      if (finishAt <= boostEndTime || block.timestamp <= boostEndTime) {
            return maxBoost;
        } else {
            uint256 time = block.timestamp > finishAt ? finishAt : block.timestamp;
            return ((boostEndTime - userUpdatedAt) * maxBoost) / (time - userUpdatedAt);
68        }

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol#L63L68

```solidity

file: lybra/pools/LybraStETHVault.sol

76         if (sharesAmount == 0) {
                //EUSD totalSupply is 0: assume that shares correspond to EUSD 1-to-1
                sharesAmount = (payAmount - income);
            }
            //Income is distributed to LBR staker.
            EUSD.burnShares(msg.sender, sharesAmount);
            feeStored = 0;
            emit FeeDistribution(address(configurator), income, block.timestamp);
        } else {
            bool success = EUSD.transferFrom(msg.sender, address(configurator), payAmount);
            require(success, "TF");
            try configurator.distributeRewards() {} catch {}
            feeStored = income - payAmount;
            emit FeeDistribution(address(configurator), payAmount, block.timestamp);
90        }

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L76

```solidity

file: ybra/miner/ProtocolRewardsPool.sol

198          if(reward > eUSDShare) {
                ERC20 peUSD = ERC20(configurator.peUSD());
                uint256 peUSDBalance = peUSD.balanceOf(address(this));
                if(peUSDBalance >= reward - eUSDShare) {
                    peUSD.transfer(msg.sender, reward - eUSDShare);
                    emit ClaimReward(msg.sender, EUSD.getMintedEUSDByShares(eUSDShare), address(peUSD), reward - eUSDShare, block.timestamp);
                } else {
                    if(peUSDBalance > 0) {
                        peUSD.transfer(msg.sender, peUSDBalance);
                    }
                    ERC20 token = ERC20(configurator.stableToken());
                    uint256 tokenAmount = (reward - eUSDShare - peUSDBalance) * token.decimals() / 1e18;
                    token.transfer(msg.sender, tokenAmount);
                    emit ClaimReward(msg.sender, EUSD.getMintedEUSDByShares(eUSDShare), address(token), reward - eUSDShare, block.timestamp);
                }
            } else {
                emit ClaimReward(msg.sender, EUSD.getMintedEUSDByShares(eUSDShare), address(0), 0, block.timestamp);
            }
229        if (totalStaked() == 0) return;
        require(amount > 0, "amount = 0");
        if(tokenType == 0) {
            uint256 share = IEUSD(configurator.getEUSDAddress()).getSharesByMintedEUSD(amount);
            rewardPerTokenStored = rewardPerTokenStored + (share * 1e18) / totalStaked();
        } else if(tokenType == 1) {
            ERC20 token = ERC20(configurator.stableToken());
            rewardPerTokenStored = rewardPerTokenStored + (amount * 1e36 / token.decimals()) / totalStaked();
        } else {
            rewardPerTokenStored = rewardPerTokenStored + (amount * 1e18) / totalStaked();
239        }

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L198L215
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L229L239

```solidity

file: ybra/blob/main/contracts/lybra/token/EUSD.sol

299     function getSharesByMintedEUSD(uint256 _EUSDAmount) public view returns (uint256) {
        uint256 totalMintedEUSD = _totalSupply;
        if (totalMintedEUSD == 0) {
            return 0;
        } else {
            return _EUSDAmount.mul(_totalShares).div(totalMintedEUSD);
        }
    }

    /**
     * @return the amount of EUSD that corresponds to `_sharesAmount` token shares.
     */
    function getMintedEUSDByShares(uint256 _sharesAmount) public view returns (uint256) {
        if (_totalShares == 0) {
            return 0;
        } else {
            return _sharesAmount.mul(_totalSupply).div(_totalShares);
316        }

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L299L316

```solidity

file: lybra/configuration/LybraConfigurator.sol

198   function setSafeCollateralRatio(address pool, uint256 newRatio) external checkRole(TIMELOCK) {
        if(IVault(pool).vaultType() == 0) {
            require(newRatio >= 160 * 1e18, "eUSD vault safe collateralRatio should more than 160%");
        } else {
            require(newRatio >= vaultBadCollateralRatio[pool] + 1e19, "PeUSD vault safe collateralRatio should more than bad collateralRatio");
        }

298             if(!premiumTradingEnabled || price <= 1005000) {
                EUSD.transfer(address(lybraProtocolRewardsPool), balance);
                lybraProtocolRewardsPool.notifyRewardAmount(balance, 0);
            } else {
                EUSD.approve(address(curvePool), balance);
                uint256 amount = curvePool.exchange_underlying(0, 2, balance, balance * price * 998 / 1e21);
                IEUSD(stableToken).transfer(address(lybraProtocolRewardsPool), amount);
                lybraProtocolRewardsPool.notifyRewardAmount(amount, 1);
306            }
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L198L203
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L298L306

```solidity

file: 

211      if(useEUSD) {
                (, int lbrPrice, , , ) = lbrPriceFeed.latestRoundData();
                biddingFee = biddingFee * uint256(lbrPrice) / 1e8;
                bool success = EUSD.transferFrom(msg.sender, address(configurator), biddingFee);
                require(success, "TF");
                try configurator.distributeRewards() {} catch {}
            } else {
                IesLBR(LBR).burn(msg.sender, biddingFee);
217            }


230   if (block.timestamp >= finishAt) {
            rewardRatio = amount / duration;
        } else {
            uint256 remainingRewards = (finishAt - block.timestamp) * rewardRatio;
            rewardRatio = (amount + remainingRewards) / duration;
35        }
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L211-L2019
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L230-L235

```solidity

file: 

168     if (provider == msg.sender) {
            collateralAsset.transfer(msg.sender, reducedAsset);
        } else {
            reward2keeper = (reducedAsset * configurator.vaultKeeperRatio(address(this))) / 110;
            collateralAsset.transfer(provider, reducedAsset - reward2keeper);
            collateralAsset.transfer(msg.sender, reward2keeper);
174        }

```
ithub.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L168-L174

```solidity

file: ybra/pools/base/LybraPeUSDVaultBase.sol

138       if (provider == msg.sender) {
            collateralAsset.transfer(msg.sender, reducedAsset);
        } else {
            reward2keeper = (reducedAsset * configurator.vaultKeeperRatio(address(this))) / 110;
            collateralAsset.transfer(provider, reducedAsset - reward2keeper);
            collateralAsset.transfer(msg.sender, reward2keeper);
144        }

197       if(amount >= totalFee) {
            feeStored[_onBehalfOf] = 0;
            PeUSD.transferFrom(_provider, address(configurator), totalFee);
            PeUSD.burn(_provider, amount - totalFee);
        } else {
            feeStored[_onBehalfOf] = totalFee - amount;
            PeUSD.transferFrom(_provider, address(configurator), amount);
204        }

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L138-L144
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L197-L204

## [G-11] Use function instead of modifiers 
   
   Deployment. Gas Saved: 177 805

```solidity

file: 

42     modifier onlyMintVault() {
        require(configurator.mintVault(msg.sender), "RCP");
        _;
    }
    modifier MintPaused() {
        require(!configurator.vaultMintPaused(msg.sender), "MPP");
        _;
    }
    modifier BurnPaused() {
        require(!configurator.vaultBurnPaused(msg.sender), "BPP");
        _;
53    }


```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L42-L53

```solidity

file: lybra/miner/ProtocolRewardsPool.sol
 
178     modifier updateReward(address account) {
        rewards[account] = earned(account);
        userRewardPerTokenPaid[account] = rewardPerTokenStored;
        _;
    }

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L178

```solidity

file: lybra/token/EUSD.sol

79 
    modifier onlyMintVault() {
        require(configurator.mintVault(msg.sender), "RCP");
        _;
    }
    modifier MintPaused() {
        require(!configurator.vaultMintPaused(msg.sender), "MPP");
        _;
    }
    modifier BurnPaused() {
        require(!configurator.vaultBurnPaused(msg.sender), "BPP");
        _;
90    }

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L79-L90

```solidity

file:

85     modifier onlyRole(bytes32 role) {
        GovernanceTimelock.checkOnlyRole(role, msg.sender);
        _;
    }

    modifier checkRole(bytes32 role) {
        GovernanceTimelock.checkRole(role, msg.sender);
        _;
90    }


```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L85-L90

```solidity

file: lybra/miner/EUSDMiningIncentives.sol

72 modifier updateReward(address _account) 

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L72