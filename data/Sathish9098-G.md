# GAS OPTIMIZATION

##

## [G-] Optimizing gas consumption with tight variable packing

If variables occupying the same slot are both written the same function or by the constructor, avoids a separate Gsset (20000 gas). Reads of the variables can also be cheaper

### ``EUSDMiningIncentives.sol``: ``duration ,finishAt, updatedAt, rewardRatio, extraRatio ,peUSDExtraRatio `` Variables can be uint128 instead of uint256.``biddingFeeRatio`` can be uint94 instead of uint256 :     Saves ``8000 GAS , 4 SLOT`` 

- ``duration,finishAt,updatedAt,rewardRatio`` can be changed from ``uint256`` to ``uint128``. The maximum value that a uint128 variable can store is 18,446,744,073,709,551,615 (2^128 - 1). The block timestamp will not reach this value for over 280 billion years. Therefore, a uint128 variable is enough to store the block timestamp for the foreseeable future. ``Saves 4000 gas , 2 SLOT ``

- ``extraRatio, peUSDExtraRatio`` can be changed from ``uint256`` to ``uint128``. The values of ``extraRatio, peUSDExtraRatio`` not exceeds 1e20 as per require condition checks ``require(ratio <= 1e20, "BCE"), require(ratio <= 1e20, "BCE")``. So uint128 alone enough instead of uin256. ``Saves 2000 gas , 1 SLOT `` 

- ``biddingFeeRatio `` can be uint94 instead of uitn256.  ``biddingFeeRatio`` value not exceeds 8000 as per ``require(_biddingRatio <= 8000, "BCE") `` this check. ``Saves 2000 gas , 1 SLOT `` 

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/EUSDMiningIncentives.sol#L35-L54


```diff
FILE: Breadcrumbs2023-06-lybra/contracts/lybra/miner/EUSDMiningIncentives.sol

35: // Duration of rewards to be paid out (in seconds)
- 36:    uint256 public duration = 2_592_000;
+ 36:    uint128 public duration = 2_592_000;
37:    // Timestamp of when the rewards finish
- 38:    uint256 public finishAt;
+ 38:    uint128 public finishAt;
39:    // Minimum of last updated time and reward finish time
- 40:    uint256 public updatedAt;
+ 40:    uint128 public updatedAt;
41:    // Reward to be paid out per second
- 42:    uint256 public rewardRatio;
+ 42:    uint128 public rewardRatio;
43:    // Sum of (reward ratio * dt * 1e18 / total supply)
44:    uint256 public rewardPerTokenStored;
45:    // User address => rewardPerTokenStored
46:    mapping(address => uint256) public userRewardPerTokenPaid;
47:    // User address => rewards to be claimed
48:    mapping(address => uint256) public rewards;
49:    mapping(address => uint256) public userUpdatedAt;
- 50:    uint256 public extraRatio = 50 * 1e18;
+ 50:    uint128 public extraRatio = 50 * 1e18;
- 51:    uint256 public peUSDExtraRatio = 10 * 1e18;
+ 51:    uint128 public peUSDExtraRatio = 10 * 1e18;
- 52:    uint256 public biddingFeeRatio = 3000;
+ 52:    uint94 public biddingFeeRatio = 3000;
53:    address public ethlbrStakePool;
54:    address public ethlbrLpToken;

```

### ``LybraConfigurator.sol``: ``redemptionFee ,flashloanFee ,maxStableRatio `` can be uint94 instead of uint256. Saves ``6000 GAS, 3 SLOT``

- ``redemptionFee`` is not exceeds`` 500`` because of this check ``require(newFee <= 500, "Max Redemption Fee is 5%");``

- ``flashloanFee `` is not exceeds ``10_000`` because of this check ``if (fee > 10_000) revert('EL');``

- ``maxStableRatio `` is not exceeds ``10_000`` because of this check  ``require(_ratio <= 10_000, "The maximum value is 10000");``

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/configuration/LybraConfigurator.sol#L49-L61


```diff
FILE: 2023-06-lybra/contracts/lybra/configuration/LybraConfigurator.sol

- 49:  uint256 public redemptionFee = 50;
+ 49:  uint94 public redemptionFee = 50;
50:    IGovernanceTimelock public GovernanceTimelock;
51:
52:    IeUSDMiningIncentives public eUSDMiningIncentives;
53:    IProtocolRewardsPool public lybraProtocolRewardsPool;
54:    IEUSD public EUSD;
55:    IEUSD public peUSD;
- 56:    uint256 public flashloanFee = 500;
- 56:    uint94 public flashloanFee = 500;
57:    // Limiting the maximum percentage of eUSD that can be cross-chain transferred to L2 in relation to the total supply.
- 58:    uint256 maxStableRatio = 5_000;
+ 58:    uint94 maxStableRatio = 5_000;
59:    address public stableToken;

```

### ``ProtocolRewardsPool.sol``: ``grabFeeRatio`` can be uint94 instead of uint256 since the value not exceeds ``8000 `` as per this check ``require(_ratio <= 8000, "BCE");`` : Saves ``2000 GAS, 1 SLOD``

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/ProtocolRewardsPool.sol#L27-L41

```diff
FILE: 2023-06-lybra/contracts/lybra/miner/ProtocolRewardsPool.sol

26: IesLBR public esLBR;
27:    IesLBR public LBR;
+ 41:    uint94 public grabFeeRatio = 3000;
28:    IesLBRBoost public esLBRBoost;
29:
30:    // Sum of (reward ratio * dt * 1e18 / total supply)
31:    uint public rewardPerTokenStored;

39: uint256 immutable exitCycle = 90 days;
40:    uint256 public grabableAmount;
- 41:    uint256 public grabFeeRatio = 3000;
42:    event Restake(address indexed user, uint256 amount, uint256 time);

```

### ``stakerewardV2pool.sol``: ``duration ,finishAt, updatedAt, rewardRatio  `` Variables can be uint128 instead of uint256: Saves ``4000 Gas , 2 SLOT``

- ``duration,finishAt,updatedAt,rewardRatio`` can be changed from ``uint256`` to ``uint128``. The maximum value that a uint128 variable can store is 18,446,744,073,709,551,615 (2^128 - 1). The block timestamp will not reach this value for over 280 billion years. Therefore, a uint128 variable is enough to store the block timestamp for the foreseeable future. ``Saves 4000 gas , 2 SLOT ``

```diff
FILE: 2023-06-lybra/contracts/lybra/miner/stakerewardV2pool.sol

- 23:  uint256 public duration = 2_592_000;
+ 23:  uint128 public duration = 2_592_000;
24:    // Timestamp of when the rewards finish
- 25:    uint256 public finishAt;
+ 25:    uint128 public finishAt;
26:    // Minimum of last updated time and reward finish time
- 27:    uint256 public updatedAt;
+ 27:    uint128 public updatedAt;
28:    // Reward to be paid out per second
- 29:    uint256 public rewardRatio;
+ 29:    uint128 public rewardRatio;
30:    // Sum of (reward ratio * dt * 1e18 / total supply)
31:    uint256 public rewardPerTokenStored;

```

##

## [G-] Structs can be packed into fewer storage slots

Each slot saved can avoid an extra Gsset (20000 gas) for the first setting of the struct.
Subsequent reads as well as writes have smaller gas savings.


### ``esLBRBoost.sol``: ``unlockTime,duration`` can be uint128 instead of uint256 : Saves ``2000 GAS, 1 SLOT``

The maximum value that a uint128 variable can store is 18,446,744,073,709,551,615 (2^128 - 1). The block timestamp will not reach this value for over 280 billion years. Therefore, a uint128 variable is enough to store the block timestamp for the foreseeable future

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/esLBRBoost.sol#L18-L22 


```diff
FILE: 2023-06-lybra/contracts/lybra/miner/esLBRBoost.sol

18: struct LockStatus {
- 19:        uint256 unlockTime;
+ 19:        uint128 unlockTime;
- 20:        uint256 duration;
+ 20:        uint128 duration;
21:        uint256 miningBoost;
22:    }

```

##

## [G-] ``maxSupply`` can be declared as constant to save large volume of gas 

The ``maxSupply`` value not changed any where inside the contract. This value only used for checking ``totalSupply() + amount <= maxSupply``

The maxSupply variable can be declared as a constant to save gas. This is because constants are stored in memory, which is a much cheaper operation than storing variables in storage.

The expression 100_000_000 * 1e18 evaluates to a very large number, which would require a lot of gas to store in storage. By declaring maxSupply as a constant, we can avoid this gas cost.

| Contract | Gas Cost|
|---|---|
| ``maxSupply`` as a variable | ``21,000 gas`` |
| ``maxSupply`` as a constant | ``1,500 gas`` |

### ``esLBR.sol``,``LBR.sol``: ``maxSupply`` should be declared as constant instead of state variable : Saves ``42000 GAS``, ``200 GAS `` for each call 

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/esLBR.sol#L20

```diff
FILE: 2023-06-lybra/contracts/lybra/token/esLBR.sol

- 20: uint256 maxSupply = 100_000_000 * 1e18;
+ 20: uint256 public constant maxSupply = 100_000_000 * 1e18;

```

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/LBR.sol#L15

```diff
FILE: 2023-06-lybra/contracts/lybra/token/LBR.sol

- 15: uint256 maxSupply = 100_000_000 * 1e18;
+ 20: uint256 public constant maxSupply = 100_000_000 * 1e18;

```

##

## [G-] Avoiding Unnecessary Storage of rkPool Address 

The constructor of the LybraRETHVault contract takes the address of the rkPool contract as a parameter. This means that the value of rkPool is already known to the contract when it is deployed. There is no need to store the value of rkPool in the state of the contract, as it can be retrieved from the parameter of the constructor

Storing the value of rkPool in the state of the contract would consume unnecessary gas. The gas cost of storing a 20-byte address in the state of a contract is 200 gas. This means that storing the value of rkPool would consume 200 gas, even though the value of rkPool is already known to the contract 

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/LybraRETHVault.sol#L18

```solidity
FILE: 2023-06-lybra/contracts/lybra/pools/LybraRETHVault.sol

18: IRkPool rkPool = IRkPool(0xDD3f50F8A6CafbE9b31a427582963f465E745AF8);

```

##

## [G-] Using storage instead of memory for structs/arrays saves gas

When fetching data from a storage location, assigning the data to a memory variable causes all fields of the struct/array to be read from storage, which incurs a Gcoldsload (2100 gas) for each field of the struct/array. If the fields are read from the new memory variable, they incur an additional MLOAD rather than a cheap stack read. Instead of declearing the variable with the memory keyword, declaring the variable with the storage keyword and caching any fields that need to be re-read in stack variables, will be much cheaper, only incuring the Gcoldsload for the fields actually read. The only time it makes sense to read the whole struct/array into a memory variable, is if the full struct/array is being returned by the function, is being passed to a function that requires memory, or if the array/struct is being read from another memory array/struct

### NOTE: These instance is not found by bot race

### ``esLBRBoost.sol``: ``storage`` should be used instead of ```memory: Saves ``4100 GAS  ``

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/esLBRBoost.sol#L39

```solidity
FILE: 2023-06-lybra/contracts/lybra/miner/esLBRBoost.sol

- 39: esLBRLockSetting memory _setting = esLBRLockSettings[id];
+ 39: esLBRLockSetting strorage _setting = esLBRLockSettings[id];

```

##

## [G-] Using ``calldata`` instead of ``memory`` for read-only arguments in external functions saves gas

When a function with a memory array is called externally, the abi.decode ()  step has to use a for-loop to copy each index of the calldata to the memory index. Each iteration of this for-loop costs at least 60 gas (i.e. 60 * <mem_array>.length). Using calldata directly, obliviates the need for such a loop in the contract code and runtime execution

### ``EUSDMiningIncentives.sol``: Use ``calldata`` instead of ``memory``: Saves ``312 GAS per iteration``

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/EUSDMiningIncentives.sol#L93

```diff
FILE: 2023-06-lybra/contracts/lybra/miner/EUSDMiningIncentives.sol

- 93: function setPools(address[] memory _pools) external onlyOwner {
+ 93: function setPools(address[] calldata _pools) external onlyOwner {
94:        for (uint i = 0; i < _pools.length; i++) {
95:            require(configurator.mintVault(_pools[i]), "NOT_VAULT");
96:        }
97:        pools = _pools;
98:    }

```

### ``esLBRBoost.sol``: Use ``calldata`` instead of ``memory``: Saves ``312 GAS ``

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/esLBRBoost.sol#L33


```diff
FILE: 2023-06-lybra/contracts/lybra/miner/esLBRBoost.sol

- 33: function addLockSetting(esLBRLockSetting memory setting) external onlyOwner {
+ 33: function addLockSetting(esLBRLockSetting calldata setting) external onlyOwner {
34:        esLBRLockSettings.push(setting);
35:    }

```
















##





