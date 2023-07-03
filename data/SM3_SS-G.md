

# Summary


# Gas Optimization    
no | Issue |Instances||
|-|:-|:-:|:-:|
| [G-01] |  A modifier used only once and not being inherited should be inlined to save gas | 3 |
| [G-02] |  State variables with values known at compile time should be constants  | 2 |
| [G-03] |  Using calldata instead of memory for read-only arguments in external functions saves gas | 1 |
| [G-04] |  Use assembly to check for address(0) | 17 |
| [G-05] |  Amounts should be checked for 0 before calling a transfer| 5 |
| [G-06] |  Use calldata instead of memory | 2 |
| [G-07] |  Can Make The Variable Outside The Loop To Save Gas | 1 |
| [G-08] |  Use assembly to write address storage values | 12 |
| [G-09] |  Use nested if statements instead of && | 4 |
| [G-10] |  Make 3 event parameters indexed when possible | 8 |
| [G-11] |  internal functions not called by the contract should be removed to save deployment gas | 4 |
| [G-12] |  Use hardcode address instead address(this) | 24 |
| [G-13] |  use Mappings Instead of Arrays | 2 |
| [G-14] |  Use constants instead of type(uintx).max | 1 |
| [G-15] |  State variables can be packed into fewer storage slots | 1 |




## [G-01]   A modifier used only once and not being inherited should be inlined to save gas

When you use a modifier in Solidity, Solidity generates code to check the conditions of the modifier and execute the modified function if the conditions are met. This generated code can consume gas, especially if the modifier is used frequently or if the modified function is called multiple times.

By inlining a modifier that is used only once and not being inherited, you can eliminate the overhead of the generated code and reduce the gas cost of your contract.

```solidity 
file:   lybra/token/EUSD.sol

83         modifier MintPaused() {
        require(!configurator.vaultMintPaused(msg.sender), "MPP");
        _;
    }
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L83-L86

```solidity
file:   lybra/token/PeUSDMainnetStableVision.sol

46    modifier MintPaused() {
        require(!configurator.vaultMintPaused(msg.sender), "MPP");
        _;
    }

50         modifier BurnPaused() {
        require(!configurator.vaultBurnPaused(msg.sender), "BPP");
        _;
    }

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L46-L49

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L50-L53

## [G-02]   State variables with values known at compile time should be constants 

Variables with values known at compile time and that do not change at runtime should be declared as constant.

```solidity
file:   lybra/miner/ProtocolRewardsPool.sol

39      uint256 immutable exitCycle = 90 days;

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L39


```solidity
file:  lybra/pools/LybraStETHVault.sol

14     uint256 public lidoRebaseTime = 12 hours;

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L14


## [G-03] Using calldata instead of memory for read-only arguments in external functions saves gas

When designating a data location as "memory," the assigned value will be duplicated into the memory space. However, when opting for the "calldata" location, the value remains fixed within the calldata area. It is important to note that if the value happens to be a sizable and intricate type, selecting "memory" might lead to additional expenses related to memory expansion.

Note: that modifying these instances from "memory" to "calldata" can be executed without triggering a stack too deep error.

```solidity
file:   lybra/miner/esLBRBoost.sol  

33      function addLockSetting(esLBRLockSetting memory setting) external onlyOwner 

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol#L33


## [G-04] Use assembly to check for address(0)

In terms of gas efficiency, employing assembly to verify a zero address (address(0)) generally outperforms using the == operator.

The rationale behind this lies in the fact that the == operator generates additional instructions in the EVM bytecode, leading to an increased gas cost for your contract. By resorting to assembly, you can execute the zero address check more efficiently, resulting in a reduction of the overall gas expenditure in your contract.

Here's an adjusted example showcasing how assembly can be used to perform a zero address check:

```
contract MyContract {
    function isZeroAddress(address addr) public pure returns (bool) {
        uint256 addrInt = uint256(addr);
        
        assembly {
            // Load the zero address into memory
            let zero := mload(0x00)
            
            // Compare the address to the zero address
            let isZero := eq(addrInt, zero)
            
            // Return the result
            mstore(0x00, isZero)
            return(0, 0x20)
        }
    }
}
```
In the given example, we have a function called isZeroAddress that accepts an address as input and returns a boolean value indicating whether the address is equal to the zero address. Within the function, we convert the address to an integer using uint256(addr), and then employ assembly to compare the integer to the zero address.

By utilizing assembly for the zero address check, we can enhance the gas efficiency of our code and reduce the overall cost of our contract. However, it's important to note that assembly can be more challenging to read and maintain compared to Solidity code. Therefore, it should be used judiciously and only when necessary to optimize gas consumption.

Saves 6 gas per instance:

```solidity

file:     LybraConfigurator.sol

99         if (address(EUSD) == address(0)) EUSD = IEUSD(_eusd);
100        if (address(peUSD) == address(0)) peUSD = IEUSD(_peusd);

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L99

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L100

```solidity
file:  lybra/miner/EUSDMiningIncentives.sol

76     if (_account != address(0))

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L76

```solidity
file:   lybra/miner/stakerewardV2pool.sol

60      if (_account != address(0))

```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L60

```solidity

file:  lybra/pools/base/LybraEUSDVaultBase.sol

99     require(onBehalfOf != address(0), "TZA");

127    require(onBehalfOf != address(0), "MINT_TO_THE_ZERO_ADDRESS");

141    require(onBehalfOf != address(0), "BURN_TO_THE_ZERO_ADDRESS");

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L99

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L127

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#141 

```solidity
file:    lybra/pools/base/LybraPeUSDVaultBase.sol

83       require(onBehalfOf != address(0), "TZA");

97       require(onBehalfOf != address(0), "TZA");

111      require(onBehalfOf != address(0), "TZA");

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L83

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L97 

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L111

```solidity
file:        lybra/token/EUSD.sol

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

## [G-05] Amounts should be checked for 0 before calling a transfer

It is considered a best practice to verify for zero values before executing any transfers within smart contract functions. This approach helps prevent unnecessary external calls and can effectively reduce gas costs.

This practice is particularly crucial when dealing with the transfer of tokens or ether, as sending these assets to an address with a zero value will result in the loss of those assets.

In Solidity, you can determine whether a value is zero by utilizing the == operator. Here's an example demonstrating how to check for a zero value before performing a transfer:

```solidity
Copy code
function transfer(address payable recipient, uint256 amount) public {
    require(amount > 0, "Amount must be greater than zero");
    recipient.transfer(amount);
}
```
In the provided example, we ensure that the amount parameter is greater than zero before executing the transfer to the designated recipient address. If the amount is zero or negative, the function will revert, preventing the transfer from taking place.


```solidity
File: lybra/pools/LybraStETHVault.sol
70   bool success = EUSD.transferFrom(msg.sender, address(configurator), income);

85   bool success = EUSD.transferFrom(msg.sender, address(configurator), payAmount);

93   collateralAsset.transfer(msg.sender, realAmount);
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L70

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L85

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L93


```solidity
File: lybra/token/PeUSDMainnetStableVision.sol
82   bool success = EUSD.transferFrom(user, address(this), eusdAmount);

131  EUSD.transferShares(address(receiver), shareAmount);
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L82

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L131



## [G-06] Use calldata instead of memory
Variables declared as function parameters are either stored at calldata or memory. One key difference between memory and calldata is that memory can be modified by the function, while calldata is immutable.

Here the principle is to use calldata instead of memory if the function argument is read-only. This avoids unnecessary copies from function calldata to memory.

```
contract C {
 function add(uint[] memory arr) external returns (uint sum){
     uint length = arr.length;
     for (uint i = 0; i < arr.length; i++) 
     {
        sum += arr[i];
     }
 }
}
```
In this example, which uses the memory keyword, the array values are kept in encoded calldata and are copied to memory during ABI decoding. The execution cost is 3,694 gas units for this code block.
```
contract C {
   function add(uint[] calldata arr) external returns (uint sum) {
     uint length = arr.length;
     for (uint i = 0; i < arr.length; i++){
       sum += arr[i];
     }
  }
}
```
In the second example, the value is directly read from calldata and there are no intermediate memory operations. This adjustment results in an execution cost of only 2,413 gas units for this code block, marking a 35% improvement in gas efficiency.

```solidity
File: 
106   function _execute(uint256 /* proposalId */, address[] memory targets, uint256[] memory values, bytes[] memory calldatas, bytes32 descriptionHash) internal virtual override {
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol#L

```solidity
File: lybra/miner/EUSDMiningIncentives.sol
93   function setPools(address[] memory _pools) external onlyOwner {
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L93



## [G-07] Can Make The Variable Outside The Loop To Save Gas 

When a variable is declared inside a loop in Solidity, a new instance of that variable is created for each iteration of the loop. This can result in unnecessary gas costs, especially when the loop is executed frequently or iterates over a large number of elements.

To mitigate these gas costs, it is recommended to declare the variable outside the loop. By doing so, you avoid the creation of multiple instances of the variable, leading to a reduction in the overall gas cost of your contract. Here's an example illustrating this approach:

```solidity
Copy code
contract MyContract {
    function sum(uint256[] memory values) public pure returns (uint256) {
        uint256 total = 0;
        
        for (uint256 i = 0; i < values.length; i++) {
            total += values[i];
        }
        
        return total;
    }
}
```
In the provided example, the variable total is declared outside the loop. This ensures that only one instance of the variable is created throughout the loop iterations. As a result, unnecessary gas costs associated with creating multiple instances of the variable are avoided, improving the gas efficiency of the contract.

```solidity
File: lybra/miner/EUSDMiningIncentives.sol
140    uint borrowed = pool.getBorrowedOf(user);
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L140


## [G-08] Use assembly to write address storage values

By leveraging assembly to write to address storage values, you can bypass certain operations and reduce the gas cost associated with storing values. Assembly code grants you direct access to the Ethereum Virtual Machine (EVM), enabling you to perform low-level operations that are not feasible in Solidity.

Here's an example illustrating the utilization of assembly to write to address storage values:

```solidity
Copy code
contract MyContract {
    address private myAddress;
    
    function setAddressUsingAssembly(address newAddress) public {
        assembly {
            sstore(0, newAddress)
        }
    }
}
```
In the provided example, the contract MyContract includes a private address variable called myAddress. The function setAddressUsingAssembly utilizes assembly to store a new address value directly in the storage slot at index 0. By employing assembly's low-level capabilities, the contract can achieve efficient and cost-effective storage updates.

It's important to note that assembly introduces complexities and should only be utilized when necessary, as it may make the code less readable and maintainable. Additionally, careful consideration should be given to potential security risks and the potential for introducing vulnerabilities when working with assembly code.

```solidity
File: lybra/configuration/LybraConfigurator.sol
81    GovernanceTimelock = IGovernanceTimelock(_dao);

82    curvePool = ICurvePool(_curvePool);

138   lybraProtocolRewardsPool = IProtocolRewardsPool(addr);

148   eUSDMiningIncentives = IeUSDMiningIncentives(addr);

262   stableToken = _token;
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L81

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L82

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L138

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L148

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L262

```solidity
File: lybra/miner/EUSDMiningIncentives.sol
86   esLBR = _eslbr;

90   lbrPriceFeed = AggregatorV3Interface(_lbrOracle);

116  esLBRBoost = IesLBRBoost(_boost);

125  ethlbrStakePool = _pool;

126  ethlbrLpToken = _lp;
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L86

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L90

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L116

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L125

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L126


```solidity
File: lybra/miner/ProtocolRewardsPool.sol
55  esLBRBoost = IesLBRBoost(_boost);
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L55

```solidity
File: lybra/pools/LybraRETHVault.sol
43   rkPool = IRkPool(addr);
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraRETHVault.sol#L43





## [G-09] Use nested if statements instead of &&
statement, it can be rewritten using two separate if statements.

This technique involves splitting the original if statement into two individual if statements, each containing one part of the logical AND condition. By doing so, the code becomes more explicit and easier to read, while maintaining the same logical behavior and save gas cost.

```
contract NestedIfTest {

    //Execution cost: 22334 gas
    function funcBad(uint256 input) public pure returns (string memory) { 
       if (input<10 && input>0 && input!=6){ 
           return "If condition passed";
       } 

   }

    //Execution cost: 22294 gas
    function funcGood(uint256 input) public pure returns (string memory) { 
    if (input<10) { 
        if (input>0){
            if (input!=6){
                return "If condition passed";
            }
        }
    }
   }
}
   
```


```solidity
File:   lybra/pools/base/LybraEUSDVaultBase.sol
204    if (msg.sender != provider && onBehalfOfCollateralRatio >= 1e20 + configurator.vaultKeeperRatio(address(this)) * 1e18) {
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L204

```solidity
File: lybra/token/LBR.sol
64   if (_from != address(this) && _from != spender)
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol#L64

```solidity
File: lybra/token/PeUSD.sol
46   if (_from != address(this) && _from != spender)
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSD.sol#L46

```solidity
File: lybra/token/PeUSDMainnetStableVision.sol
199   if (_from != address(this) && _from != spender)
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L199


## [G-10] Make 3 event parameters indexed when possible
It’s the most gas efficient to make up to 3 event parameters indexed. If there are less than 3 parameters, you need to make all parameters indexed.

```solidity
File: lybra/miner/EUSDMiningIncentives.sol
59    event ClaimReward(address indexed user, uint256 amount, uint256 time);

60    event ClaimedOtherEarnings(address indexed user, address indexed Victim, uint256 buyAmount, uint256 biddingFee, bool useEUSD, uint256 time);
 
61    event NotifyRewardChanged(uint256 addAmount, uint256 time);
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L59

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L60

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L61


```solidity
File: lybra/configuration/LybraConfigurator.sol
63  event RedemptionFeeChanged(uint256 newSlippage);
    event SafeCollateralRatioChanged(address indexed pool, uint256 newRatio);
    event RedemptionProvider(address indexed user, bool status);
    event ProtocolRewardsPoolChanged(address indexed pool, uint256 timestamp);
    event EUSDMiningIncentivesChanged(address indexed pool, uint256 timestamp);
    event BorrowApyChanged(address indexed pool, uint256 newApy);
    event KeeperRatioChanged(address indexed pool, uint256 newSlippage);
70  event tokenMinerChanges(address indexed pool, bool status);

74  event FlashloanFeeUpdated(uint256 fee);
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L63-L70

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L74

```solidity
File: lybra/miner/ProtocolRewardsPool.sol
42   event Restake(address indexed user, uint256 amount, uint256 time);
    event StakeLBR(address indexed user, uint256 amount, uint256 time);
    event UnstakeLBR(address indexed user, uint256 amount, uint256 time);
    event WithdrawLBR(address indexed user, uint256 amount, uint256 time);
    event ClaimReward(address indexed user, uint256 eUSDAmount, address token, uint256 tokenAmount, uint256 time);
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L42-L46

```solidity
File: lybra/miner/stakerewardV2pool.sol
44  event StakeToken(address indexed user, uint256 amount, uint256 time);
    event WithdrawToken(address indexed user, uint256 amount, uint256 time);
    event ClaimReward(address indexed user, uint256 amount, uint256 time);
    event NotifyRewardChanged(uint256 addAmount, uint256 time);

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L44-L47

```solidity
File: lybra/pools/base/LybraEUSDVaultBase.sol
44    event DepositEther(address indexed onBehalfOf, address asset, uint256 etherAmount, uint256 assetAmount, uint256 timestamp);

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
File: lybra/pools/base/LybraPeUSDVaultBase.sol
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

## [G-11] internal functions not called by the contract should be removed to save deployment gas
When you declare an internal function within a Solidity contract, Solidity generates corresponding code for that function and incorporates it into the contract's bytecode. However, if the contract itself or any of its external functions do not invoke the internal function, including its code in the contract bytecode becomes redundant and can result in increased deployment gas costs.

To mitigate this, it is advisable to remove unused internal functions. By doing so, you can reduce the size of your contract's bytecode, leading to lower overall gas costs during contract deployment.

If the internal functions are required by an interface, the contract should inherit from that interface and utilize the override keyword. This ensures that the contract adheres to the interface's specifications while maintaining the necessary functionality.

By eliminating unused internal functions and properly implementing interfaces when required, you can optimize your contract's bytecode size and minimize gas costs associated with deployment. This approach contributes to a more efficient and cost-effective smart contract deployment process.



```solidity
File: lybra/governance/LybraGovernance.sol
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



## [G-12] Use hardcode address instead address(this)
it can be more gas-efficient to use a hardcoded address instead of the address(this) expression, especially if you need to use the same address multiple times in your contract.

The reason for this is that using address(this) requires an additional EXTCODESIZE operation to retrieve the contract's address from its bytecode, which can increase the gas cost of your contract. By pre-calculating and using a hardcoded address, you can avoid this additional operation and reduce the overall gas cost of your contract.

Here's an example of how you can use a hardcoded address instead of address(this):

```
contract MyContract {
    address public myAddress = 0x1234567890123456789012345678901234567890;
    
    function doSomething() public {
        // Use myAddress instead of address(this)
        require(msg.sender == myAddress, "Caller is not authorized");
        
        // Do something
    }
}
```
In the above example, we have a contract MyContract with a public address variable myAddress. Instead of using address(this) to retrieve the contract's address, we have pre-calculated and hardcoded the address in the variable. This can help to reduce the gas cost of our contract and make our code more efficient.

[References](https://book.getfoundry.sh/reference/forge-std/compute-create-address)


```solidity
File:lybra/pools/base/LybraEUSDVaultBase.sol
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
File: lybra/pools/base/LybraPeUSDVaultBase.sol
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
File: lybra/token/PeUSDMainnetStableVision.sol
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

## [G-13] use Mappings Instead of Arrays
Arrays are a valuable tool for maintaining an ordered list of data that can be iterated over. However, they do incur a higher gas cost for read and write operations, particularly when the array size is substantial. This is primarily due to the need for Solidity to iterate over the entire array to execute certain operations, such as locating a specific element or removing an element.

On the other hand, mappings offer a convenient solution when you require data storage and access based on a key rather than an index. Mappings tend to have a lower gas cost for read and write operations, especially when the mapping size is significant. This advantage arises from Solidity's ability to directly perform these operations based on the provided key, eliminating the necessity to iterate over the entire data structure.

By carefully considering the nature of your data and the desired operations, you can determine whether to utilize arrays or mappings. Arrays are suitable for maintaining ordered lists that require iteration, while mappings are more efficient for key-based data storage and retrieval, particularly when dealing with larger datasets.

```solidity
File: lybra/miner/esLBRBoost.sol
8    esLBRLockSetting[] public esLBRLockSettings;
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol#L8

```solidity
File: lybra/miner/EUSDMiningIncentives.sol
33   address[] public pools;
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L33


## [G-14] Use constants instead of type(uintx).max
It is generally more gas-efficient to use constants instead of the expression type(uintX).max when setting the maximum value for an unsigned integer type.

The reason behind this lies in the fact that type(uintX).max involves a runtime computation, whereas a constant is evaluated at compile-time. As a result, utilizing type(uintX).max can lead to additional gas costs for each transaction involving the expression.

By substituting type(uintX).max with a constant, you can circumvent these additional gas costs and optimize the efficiency of your code.

Here's an example demonstrating the usage of a constant instead of type(uintX).max:
```solidity 
contract MyContract {
    uint120 constant MAX_VALUE = 2**120 - 1;
    
    function doSomething(uint120 value) public {
        require(value <= MAX_VALUE, "Value exceeds maximum");
        
        // Perform desired actions
    }
}

```
In the provided example, the contract includes a constant called MAX_VALUE, representing the maximum value for a uint120. Within the doSomething function, a parameter value is checked against MAX_VALUE using the <= operator to ensure it does not exceed the predefined maximum.

By utilizing a constant rather than type(uint120).max, we enhance the efficiency of our code and reduce the gas cost of our contract.

It is essential to note that employing constants can enhance code readability and maintainability, as the value is defined in one place and can be easily updated when required. However, it is important to use constants with caution and only when the value is known at compile-time.


```solidity
File: lybra/token/EUSD.sol
179   if (currentAllowance != type(uint256).max) {
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L179


## [G-15] State variables can be packed into fewer storage slots
If multiple variables occupy the same storage slot and are written within the same function or by the constructor, it can result in a more efficient gas usage by avoiding a separate gas cost for each variable write operation (20000 gas). Additionally, reads of these variables can also be performed more cost-effectively.

By grouping the write operations of variables that share the same storage slot within the same function or constructor, you can optimize the gas consumption of your smart contract.


```solidity
File:lybra/miner/EUSDMiningIncentives.sol
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
