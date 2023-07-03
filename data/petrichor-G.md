|   no   |  issue  | Instance |
|--------|---------|----------|
| [G-01] | Can Make The Variable Outside The Loop To Save Gas | 1 |
|[G-02]|  Use assembly to write address storage values | 12 |
| [G-03] |  Amounts should be checked for 0 before calling a transfer| 5 |
| [G-04] |  Use assembly to check for address(0) | 17 |
| [G-05] |  Using calldata instead of memory for read-only arguments in external functions saves gas | 1 |
| [G-06] |  State variables with values known at compile time should be constants  | 2 |
| [G-07] |  A modifier used only once and not being inherited should be inlined to save gas | 3 |
| [G-08] |  State variables can be packed into fewer storage slots | 1 |
| [G-09] |  Use constants instead of type(uintx).max | 1 |
| [G-10] |  use Mappings Instead of Arrays | 2 |
| [G-11] |  Use hardcode address instead address(this) | 24 |
| [G-12] |  internal functions not called by the contract should be removed to save deployment gas | 4 |
| [G-13] |  Make 3 event parameters indexed when possible | 8 |
| [G-14] |  Use nested if statements instead of && | 4 |


## [G-01] Can Make The Variable Outside The Loop To Save Gas 

When you declare a variable inside a loop, Solidity creates a new instance of the variable for each iteration of the loop. This can lead to unnecessary gas costs, especially if the loop is executed frequently or iterates over a large number of elements.


```solidity
140    uint borrowed = pool.getBorrowedOf(user);
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L140



## [G-02] Use assembly to write address storage values

In Ethereum smart contracts, storing data in the contract storage is expensive in terms of gas costs. One way to reduce gas costs is to use assembly to write address storage values.

```solidity
81    GovernanceTimelock = IGovernanceTimelock(_dao);
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L81


```solidity
82    curvePool = ICurvePool(_curvePool);
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L82

```solidity
138   lybraProtocolRewardsPool = IProtocolRewardsPool(addr);
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L138

```solidity
148   eUSDMiningIncentives = IeUSDMiningIncentives(addr);
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L148

```solidity
262   stableToken = _token;
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L262

```solidity
86   esLBR = _eslbr;
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L86

```solidity

90   lbrPriceFeed = AggregatorV3Interface(_lbrOracle);
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L90


```solidity

116  esLBRBoost = IesLBRBoost(_boost);
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L116

```solidity
125  ethlbrStakePool = _pool;
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L125


```solidity
126  ethlbrLpToken = _lp;
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L126

```solidity
55  esLBRBoost = IesLBRBoost(_boost);
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L55

```solidity
43   rkPool = IRkPool(addr);
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraRETHVault.sol#L43

## [G-03] Amounts should be checked for 0 before calling a transfer

When calling a transfer function, the function will typically revert if the sender's balance is less than the amount being transferred. However, if the amount being transferred is 0, the transfer function will still execute, resulting in wasted gas fees. Additionally, if the contract relies on the transfer function to perform certain actions, calling the transfer function with an amount of 0 can potentially cause unexpected behavior or errors.

```solidity
70   bool success = EUSD.transferFrom(msg.sender, address(configurator), income);
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L70

```solidity
85   bool success = EUSD.transferFrom(msg.sender, address(configurator), payAmount);
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L85

```solidity
93   collateralAsset.transfer(msg.sender, realAmount);
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L93


```solidity
82   bool success = EUSD.transferFrom(user, address(this), eusdAmount);
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L82

```solidity
131  EUSD.transferShares(address(receiver), shareAmount);
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L131

## [G-04] Use assembly to check for address(0)


The reason for this is that the == operator generates additional instructions in the EVM bytecode, which can increase the gas cost of your contract. By using assembly, you can perform the zero address check more efficiently and reduce the overall gas cost of your contract.


```solidity

99         if (address(EUSD) == address(0)) EUSD = IEUSD(_eusd);
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L99


```solidity
100        if (address(peUSD) == address(0)) peUSD = IEUSD(_peusd);

```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L100

```solidity

76     if (_account != address(0))

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L76

```solidity

60      if (_account != address(0))

```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L60

```solidity
99     require(onBehalfOf != address(0), "TZA");
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L99

```solidity
127    require(onBehalfOf != address(0), "MINT_TO_THE_ZERO_ADDRESS");
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L127

```solidity
141    require(onBehalfOf != address(0), "BURN_TO_THE_ZERO_ADDRESS");
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#141 

```solidity

83       require(onBehalfOf != address(0), "TZA");

97       require(onBehalfOf != address(0), "TZA");

111      require(onBehalfOf != address(0), "TZA");

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L83

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L97 

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L111

```solidity

367           require(_owner != address(0), "APPROVE_FROM_ZERO_ADDRESS");
368           require(_spender != address(0), "APPROVE_TO_ZERO_ADDRESS");
392           require(_sender != address(0), "TRANSFER_FROM_THE_ZERO_ADDRESS");
393           require(_recipient != address(0), "TRANSFER_TO_THE_ZERO_ADDRESS");
412           require(_recipient != address(0), "MINT_TO_THE_ZERO_ADDRESS");
441           require(_account != address(0), "BURN_FROM_THE_ZERO_ADDRESS");
460           require(_account != address(0), "BURN_FROM_THE_ZERO_ADDRESS");
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L367

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L368

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L392

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L393

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L412

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L441

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L460

## [G-05] Using calldata instead of memory for read-only arguments in external functions saves gas

When you specify a data location as memory, that value will be copied into memory. When you specify the location as calldata, the value will stay static within calldata. If the value is a large, complex type, using memory may result in extra memory expansion costs.
```solidity

33      function addLockSetting(esLBRLockSetting memory setting) external onlyOwner 

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol#L33

## [G-06]   State variables with values known at compile time should be constants 

Constants are variables that are assigned a value at compile time and cannot be changed during runtime. By declaring state variables as constants, the compiler can optimize the code to reduce the number of storage reads and writes required, resulting in lower gas costs.

```solidity

39      uint256 immutable exitCycle = 90 days;

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L39


```solidity

14     uint256 public lidoRebaseTime = 12 hours;

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L14

## [G-07]   A modifier used only once and not being inherited should be inlined to save gas

Modifiers are used in Solidity to add restrictions or checks to functions. When a modifier is used, the code of the modifier is inserted into the function before the function is compiled. This can result in additional gas costs, especially if the modifier is used frequently or if it contains complex logic.

```solidity 

83         modifier MintPaused() {
        require(!configurator.vaultMintPaused(msg.sender), "MPP");
        _;
    }
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L83-L86

```solidity

46    modifier MintPaused() {
        require(!configurator.vaultMintPaused(msg.sender), "MPP");
        _;
    }
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L46-L49

```solidity
50         modifier BurnPaused() {
        require(!configurator.vaultBurnPaused(msg.sender), "BPP");
        _;
    }

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L50-L53

## [G-08] State variables can be packed into fewer storage slots


When state variables are declared in a smart contract, they are automatically assigned to storage slots. Each storage slot can hold up to 256 bits of data. If a state variable uses less than 256 bits, the remaining bits in the storage slot are left unused, resulting in wasted storage space.


```solidity
31  address public esLBR;
    address public LBR;
    address[] public pools;

    // Duration of rewards to be paid out (in seconds)
    uint256 public duration = 2_592_000;
    // Timestamp of when the rewards finish
    uint256 public finishAt;
    // Minimum of last updated time and reward finish time
    uint256 public updatedAt;
    // Reward to be paid out per second
    uint256 public rewardRatio;
    // Sum of (reward ratio * dt * 1e18 / total supply)
    uint256 public rewardPerTokenStored;
    // User address => rewardPerTokenStored
    mapping(address => uint256) public userRewardPerTokenPaid;
    // User address => rewards to be claimed
    mapping(address => uint256) public rewards;
    mapping(address => uint256) public userUpdatedAt;
    uint256 public extraRatio = 50 * 1e18;
    uint256 public peUSDExtraRatio = 10 * 1e18;
    uint256 public biddingFeeRatio = 3000;
    address public ethlbrStakePool;
    address public ethlbrLpToken;
    AggregatorV3Interface internal etherPriceFeed;
    AggregatorV3Interface internal lbrPriceFeed;
    bool public isEUSDBuyoutAllowed = true;
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L31-L57

## [G-09] Use constants instead of type(uintx).max
using type(uintX).max can result in unnecessary gas costs, especially if the function is called multiple times in the contract. This is because the function call involves a computation that can consume gas, even though the value returned by the function is constant and does not change during runtime.



```solidity
179   if (currentAllowance != type(uint256).max) {
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L179

## [G-10] use Mappings Instead of Arrays

Mappings, on the other hand, are useful when you need to store and access data based on a key, rather than an index. Mappings have a lower gas cost for read and write operations, especially when the size of the mapping is large, since Solidity can perform these operations based on the key directly, without needing to iterate over the entire data structure.

```solidity
8    esLBRLockSetting[] public esLBRLockSettings;
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol#L8

```solidity
33   address[] public pools;
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L33

## [G-11] Use hardcode address instead address(this)
using address(this) can result in unnecessary gas costs, especially if the expression is used multiple times in the contract. This is because the expression involves a call to the EVM opcode ADDRESS, which can consume gas.

```solidity
75   bool success = collateralAsset.transferFrom(msg.sender, address(this), assetAmount);

160  require(EUSD.allowance(provider, address(this)) > 0, "provider should authorize to provide liquidation EUSD");

171   reward2keeper = (reducedAsset * configurator.vaultKeeperRatio(address(this))) / 110;

197   require(EUSD.allowance(provider, address(this)) >= eusdAmount, "provider should authorize to provide liquidation EUSD");

204   if (msg.sender != provider && onBehalfOfCollateralRatio >= 1e20 + configurator.vaultKeeperRatio(address(this)) * 1e18) {

205   reward2keeper = ((assetAmount * configurator.vaultKeeperRatio(address(this))) * 1e18) / onBehalfOfCollateralRatio;

260   require(poolTotalEUSDCirculation + _mintAmount <= configurator.mintVaultMaxSupply(address(this)), "ESL");

292   if (((depositedAsset[_user] * _assetPrice * 100) / borrowed[_user]) < configurator.getSafeCollateralRatio(address(this))) revert("collateralRatio is Below safeCollateralRatio");

301   return (poolTotalEUSDCirculation * configurator.vaultMintFeeApy(address(this)) * (block.timestamp - lastReportTime)) / (86400 * 365) / 10000;
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
45   return collateralAsset.balanceOf(address(this));

60   uint256 preBalance = collateralAsset.balanceOf(address(this));

61   collateralAsset.transferFrom(msg.sender, address(this), assetAmount);

62    require(collateralAsset.balanceOf(address(this)) >= preBalance + assetAmount, "");

128   require(onBehalfOfCollateralRatio < configurator.getBadCollateralRatio(address(this)), "Borrowers collateral ratio should below badCollateralRatio");

131  require(PeUSD.allowance(provider, address(this)) > 0, "provider should authorize to provide liquidation EUSD");

141  reward2keeper = (reducedAsset * configurator.vaultKeeperRatio(address(this))) / 110;

174   require(poolTotalPeUSDCirculation + _mintAmount <= configurator.mintVaultMaxSupply(address(this)), "ESL");

226   if (((depositedAsset[user] * price * 100) / getBorrowedOf(user)) < configurator.getSafeCollateralRatio(address(this))) 

238   return (borrowed[user] * configurator.vaultMintFeeApy(address(this)) * (block.timestamp - feeUpdatedAt[user])) / (86400 * 365) / 10000;
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L45

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L60

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L61

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L62

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L128

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L131

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L141

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L174

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L226

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L138



```solidity
80    require(_msgSender() == user || _msgSender() == address(this), "MDM");

81    require(EUSD.balanceOf(address(this)) + eusdAmount <= configurator.getEUSDMaxLocked(),"ESL");
82    bool success = EUSD.transferFrom(user, address(this), eusdAmount);

133   bool success = EUSD.transferFrom(address(receiver), address(this), EUSD.getMintedEUSDByShares(shareAmount));

176   return address(this);

199   if (_from != address(this) && _from != spender)
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L80

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L81

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L82

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L133

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L176

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L199

## [G-12] internal functions not called by the contract should be removed to save deployment gas
The size of the contract's bytecode is an important factor in determining the gas cost of deploying the contract to the network. Each byte of bytecode that is included in the contract requires gas to be paid for its deployment. By removing unused internal functions, the size of the contract's bytecode can be reduced, resulting in lower deployment gas costs.

```solidity
59    function _quorumReached(uint256 proposalId) internal view override returns (bool){
        return proposalData[proposalId].supportVotes[1] + proposalData[proposalId].supportVotes[2] >= quorum(proposalSnapshot(proposalId));
    }

66   function _voteSucceeded(uint256 proposalId) internal view override returns (bool){
        return proposalData[proposalId].supportVotes[1] > proposalData[proposalId].supportVotes[0];
    }

76   function _countVote(uint256 proposalId, address account, uint8 support, uint256 weight, bytes memory) internal override {

98   function _getVotes(address account, uint256 timepoint, bytes memory) internal view override returns (uint256){    
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol#L59

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol#L60

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol#L76

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol#L98

## [G-13] Make 3 event parameters indexed when possible


When possible, it is recommended to make event parameters indexed to reduce the amount of data that needs to be stored in the event log. By reducing the size of the event log, gas costs associated with emitting events can be minimized.
```solidity
59    event ClaimReward(address indexed user, uint256 amount, uint256 time);
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L59

```solidity
60    event ClaimedOtherEarnings(address indexed user, address indexed Victim, uint256 buyAmount, uint256 biddingFee, bool useEUSD, uint256 time);
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L60

```solidity
61    event NotifyRewardChanged(uint256 addAmount, uint256 time);
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L61


```solidity
63  event RedemptionFeeChanged(uint256 newSlippage);
    event SafeCollateralRatioChanged(address indexed pool, uint256 newRatio);
    event RedemptionProvider(address indexed user, bool status);
    event ProtocolRewardsPoolChanged(address indexed pool, uint256 timestamp);
    event EUSDMiningIncentivesChanged(address indexed pool, uint256 timestamp);
    event BorrowApyChanged(address indexed pool, uint256 newApy);
    event KeeperRatioChanged(address indexed pool, uint256 newSlippage);
70  event tokenMinerChanges(address indexed pool, bool status);

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L63-L70


```solidity
74  event FlashloanFeeUpdated(uint256 fee);
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L74

```solidity
42   event Restake(address indexed user, uint256 amount, uint256 time);
    event StakeLBR(address indexed user, uint256 amount, uint256 time);
    event UnstakeLBR(address indexed user, uint256 amount, uint256 time);
    event WithdrawLBR(address indexed user, uint256 amount, uint256 time);
    event ClaimReward(address indexed user, uint256 eUSDAmount, address token, uint256 tokenAmount, uint256 time);
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L42-L46

```solidity
44  event StakeToken(address indexed user, uint256 amount, uint256 time);
    event WithdrawToken(address indexed user, uint256 amount, uint256 time);
    event ClaimReward(address indexed user, uint256 amount, uint256 time);
    event NotifyRewardChanged(uint256 addAmount, uint256 time);

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L44-L47

```solidity
   event DepositEther(address indexed onBehalfOf, address asset, uint256 etherAmount, uint256 assetAmount, uint256 timestamp);

    event DepositAsset(address indexed onBehalfOf, address asset, uint256 amount, uint256 timestamp);

    event WithdrawAsset(address sponsor, address asset, address indexed onBehalfOf, uint256 amount, uint256 timestamp);
    event Mint(address sponsor, address indexed onBehalfOf, uint256 amount, uint256 timestamp);
    event Burn(address sponsor, address indexed onBehalfOf, uint256 amount, uint256 timestamp);
    event LiquidationRecord(address provider, address keeper, address indexed onBehalfOf, uint256 eusdamount, uint256 liquidateEtherAmount, uint256 keeperReward, bool superLiquidation, uint256 timestamp);
    event LSDValueCaptured(uint256 stETHAdded, uint256 payoutEUSD, uint256 discountRate, uint256 timestamp);
    event RigidRedemption(address indexed caller, address indexed provider, uint256 eusdAmount, uint256 collateralAmount, uint256 timestamp);
    event FeeDistribution(address indexed feeAddress, uint256 feeAmount, uint256 timestamp);
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L34-L44

```solidity
26    event DepositEther(address indexed onBehalfOf, address asset, uint256    etherAmount, uint256 assetAmount, uint256 timestamp);

    event DepositAsset(address indexed onBehalfOf, address asset, uint256 amount, uint256 timestamp);
    event WithdrawAsset(address sponsor, address indexed onBehalfOf, address asset, uint256 amount, uint256 timestamp);
    event Mint(address sponsor, address indexed onBehalfOf, uint256 amount, uint256 timestamp);
    event Burn(address sponsor, address indexed onBehalfOf, uint256 amount, uint256 timestamp);
    event LiquidationRecord(address provider, address keeper, address indexed onBehalfOf, uint256 eusdamount, uint256 LiquidateAssetAmount, uint256 keeperReward, bool superLiquidation, uint256 timestamp);

    event RigidRedemption(address indexed caller, address indexed provider, uint256 peusdAmount, uint256 assetAmount, uint256 timestamp);
    event FeeDistribution(address indexed feeAddress, uint256 feeAmount, uint256 timestamp);
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L26-L35

## [G-14] Use nested if statements instead of &&
To avoid these unnecessary gas costs, it is recommended to use nested if statements instead of the logical AND operator to check multiple conditions. By using nested if statements, the second condition is only evaluated if the first condition is true, which can result in lower gas costs.



```solidity
204    if (msg.sender != provider && onBehalfOfCollateralRatio >= 1e20 + configurator.vaultKeeperRatio(address(this)) * 1e18) {
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L204

```solidity
64   if (_from != address(this) && _from != spender)
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol#L64

```solidity
46   if (_from != address(this) && _from != spender)
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSD.sol#L46

```solidity
File: lybra/token/PeUSDMainnetStableVision.sol
199   if (_from != address(this) && _from != spender)
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L199
