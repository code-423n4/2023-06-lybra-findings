# GAS OPTIMIZATION

# SUMMERY

|      |  issue  | Instance |
|------|---------|----------|
|[G-01]|  Avoid contract existence checks by using low level calls | 11 |
|[G-02]| Use calldata instead of memory | 2 |
|[G-03]| Use assembly to write address storage values | 12 |
|[G-04]| A modifier used only once and not being inherited should be inlined to save gas | 3
|[G-05]| Use nested if statements instead of && | 4 |
|[G-06]| Can Make The Variable Outside The Loop To Save Gas | 1 |
|[G-07]| internal functions not called by the contract should be removed to save deployment gas | 4 |
|[G-08]| Amounts should be checked for 0 before calling a transfer | 5 |
|[G-09]| Use hardcode address instead address(this)| 25 |
|[G-10]| Use Assembly To Check For address(0)|4|
|[G-11]| Use constants instead of type(uintx).max|1|
|[G-12]| State variables can be packed into fewer storage slots|1|
|[G-13]| Make 3 event parameters indexed when possible | 8 |
|[G-14]| Using fixed bytes is cheaper than using string|4|


## [G‑01] Avoid contract existence checks by using low level calls
Prior to 0.8.10 the compiler inserted extra code, including EXTCODESIZE (100 gas), to check for contract existence for external function calls. In more recent solidity versions, the compiler will not insert these checks if the external call has a return value. Similar behavior can be achieved in earlier versions by using low-level calls, since low level calls never check for contract existence.

```solidity
File: /contracts/lybra/configuration/LybraConfigurator.sol
199     if(IVault(pool).vaultType() == 0) {

304     IEUSD(stableToken).transfer(address(lybraProtocolRewardsPool), amount);    
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L199


```solidity
File: /contracts/lybra/miner/ProtocolRewardsPool.sol
172    amount = IEUSD(configurator.getEUSDAddress()).getMintedEUSDByShares(earned(user));

232   uint256 share = IEUSD(configurator.getEUSDAddress()).getSharesByMintedEUSD(amount);
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L172



```solidity
File: /contracts/lybra/pools/LybraRETHVault.sol
47   return (_etherPrice() * IRETH(address(collateralAsset)).getExchangeRatio()) / 1e18;
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraRETHVault.sol#L47


```solidity
File: /contracts/lybra/pools/LybraWbETHVault.sol
23   IWBETH(address(collateralAsset)).deposit{value: msg.value}(address(configurator));

35   return (_etherPrice() * IWBETH(address(collateralAsset)).exchangeRatio()) / 1e18;
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWbETHVault.sol#L23


```solidity
File: /contracts/lybra/pools/LybraWstETHVault.sol
37    uint256 wstETHAmount = IWstETH(address(collateralAsset)).wrap(msg.value)

49    return (_etherPrice() * IWstETH(address(collateralAsset)).stEthPerToken()) / 1e18;
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWstETHVault.sol#L37

```solidity
File: /contracts/lybra/token/esLBR.sol
33   try IProtocolRewardsPool(configurator.getProtocolRewardsPool()).refreshReward(user) {} catch {}

40   try IProtocolRewardsPool(configurator.getProtocolRewardsPool()).refreshReward(user) {} catch {}
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/esLBR.sol#L33


## [G-02] Use calldata instead of memory
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
File: /contracts/lybra/governance/LybraGovernance.sol
106   function _execute(uint256 /* proposalId */, address[] memory targets, uint256[] memory values, bytes[] memory calldatas, bytes32 descriptionHash) internal virtual override {
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol#L106

```solidity
File: /contracts/lybra/miner/EUSDMiningIncentives.sol
93   function setPools(address[] memory _pools) external onlyOwner {
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L93



## [G-03] Use assembly to write address storage values

By using assembly to write to address storage values, you can bypass some of these operations and lower the gas cost of writing to storage. Assembly code allows you to directly access the Ethereum Virtual Machine (EVM) and perform low-level operations that are not possible in Solidity.

example of using assembly to write to address storage values:
```
contract MyContract {
    address private myAddress;
    
    function setAddressUsingAssembly(address newAddress) public {
        assembly {
            sstore(0, newAddress)
        }
    }
}
```

```solidity
File: /contracts/lybra/configuration/LybraConfigurator.sol
81    GovernanceTimelock = IGovernanceTimelock(_dao);

82    curvePool = ICurvePool(_curvePool);

138   lybraProtocolRewardsPool = IProtocolRewardsPool(addr);

148   eUSDMiningIncentives = IeUSDMiningIncentives(addr);

262   stableToken = _token;
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L81

```solidity
File: /contracts/lybra/miner/EUSDMiningIncentives.sol
86   esLBR = _eslbr;

90   lbrPriceFeed = AggregatorV3Interface(_lbrOracle);

116  esLBRBoost = IesLBRBoost(_boost);

125  ethlbrStakePool = _pool;

126  ethlbrLpToken = _lp;
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L86

```solidity
File: /contracts/lybra/miner/ProtocolRewardsPool.sol
55  esLBRBoost = IesLBRBoost(_boost);
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L55

```solidity
File: /contracts/lybra/pools/LybraRETHVault.sol
43   rkPool = IRkPool(addr);
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraRETHVault.sol#L43




## [G-04] A modifier used only once and not being inherited should be inlined to save gas
When you use a modifier in Solidity, Solidity generates code to check the conditions of the modifier and execute the modified function if the conditions are met. This generated code can consume gas, especially if the modifier is used frequently or if the modified function is called multiple times.

By inlining a modifier that is used only once and not being inherited, you can eliminate the overhead of the generated code and reduce the gas cost of your contract.


```solidity
File: /contracts/lybra/token/EUSD.sol
83   modifier MintPaused() {
        require(!configurator.vaultMintPaused(msg.sender), "MPP");
        _;
    }
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L83

```solidity
File: /contracts/lybra/token/PeUSDMainnetStableVision.sol
46    modifier MintPaused() {
        require(!configurator.vaultMintPaused(msg.sender), "MPP");
        _;
    }


50   modifier BurnPaused() {
        require(!configurator.vaultBurnPaused(msg.sender), "BPP");
        _;
    }
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L46




## [G-05]Use nested if statements instead of &&
If the if statement has a logical AND and is not followed by an else statement, it can be replaced with 2 if statements.

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
File: /contracts/lybra/pools/base/LybraEUSDVaultBase.sol
204    if (msg.sender != provider && onBehalfOfCollateralRatio >= 1e20 + configurator.vaultKeeperRatio(address(this)) * 1e18) {
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L204

```solidity
File: /contracts/lybra/token/LBR.sol
64   if (_from != address(this) && _from != spender)
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol#L64

```solidity
File: /contracts/lybra/token/PeUSD.sol
46   if (_from != address(this) && _from != spender)
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSD.sol#L46

```solidity
File: /contracts/lybra/token/PeUSDMainnetStableVision.sol
199   if (_from != address(this) && _from != spender)
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L199

## [G-06] Can Make The Variable Outside The Loop To Save Gas 
When you declare a variable inside a loop, Solidity creates a new instance of the variable for each iteration of the loop. This can lead to unnecessary gas costs, especially if the loop is executed frequently or iterates over a large number of elements.

By declaring the variable outside the loop, you can avoid the creation of multiple instances of the variable and reduce the gas cost of your contract. Here's an example:

```
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
```solidity
File: /contracts/lybra/miner/EUSDMiningIncentives.sol
140    uint borrowed = pool.getBorrowedOf(user);
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L140


## [G-07] internal functions not called by the contract should be removed to save deployment gas
When you define an internal function in a Solidity contract, Solidity generates code for that function and includes it in the contract bytecode. If the function is not called by the contract itself or any of its external functions, then including the code for that function in the contract bytecode is unnecessary and can lead to higher deployment gas costs.

By removing unused internal functions, you can reduce the size of your contract bytecode and lower the overall deployment gas cost of your contract.

If the functions are required by an interface, the contract should inherit from that interface and use the override keyword



```solidity
File: /contracts/lybra/governance/LybraGovernance.sol
59  function _quorumReached(uint256 proposalId) internal view override returns (bool){
        return proposalData[proposalId].supportVotes[1] + proposalData[proposalId].supportVotes[2] >= quorum(proposalSnapshot(proposalId));
    }

66   function _voteSucceeded(uint256 proposalId) internal view override returns (bool){
        return proposalData[proposalId].supportVotes[1] > proposalData[proposalId].supportVotes[0];
    }

76   function _countVote(uint256 proposalId, address account, uint8 support, uint256 weight, bytes memory) internal override {

98   function _getVotes(address account, uint256 timepoint, bytes memory) internal view override returns (uint256){    
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol#L59

## [G-08] Amounts should be checked for 0 before calling a transfer
It is generally a good practice to check for zero values before making any transfers in smart contract functions. This can help to avoid unnecessary external calls and can save gas costs.

Checking for zero values is especially important when transferring tokens or ether, as sending these assets to an address with a zero value will result in the loss of those assets.

In Solidity, you can check whether a value is zero by using the == operator. Here's an example of how you can check for a zero value before making a transfer:

```
function transfer(address payable recipient, uint256 amount) public {
    require(amount > 0, "Amount must be greater than zero");
    recipient.transfer(amount);
}
```
In the above example, we check to make sure that the amount parameter is greater than zero before making the transfer to the recipient address. If the amount is zero or negative, the function will revert and the transfer will not be made.



```solidity
File: /contracts/lybra/pools/LybraStETHVault.sol
70   bool success = EUSD.transferFrom(msg.sender, address(configurator), income);

85   bool success = EUSD.transferFrom(msg.sender, address(configurator), payAmount);

93   collateralAsset.transfer(msg.sender, realAmount);
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L70


```solidity
File: /contracts/lybra/token/PeUSDMainnetStableVision.sol
82   bool success = EUSD.transferFrom(user, address(this), eusdAmount);

131  EUSD.transferShares(address(receiver), shareAmount);
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L82


## [G-09] Use hardcode address instead address(this)
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
File: /contracts/lybra/pools/base/LybraEUSDVaultBase.sol
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

```solidity
File: /contracts/lybra/pools/base/LybraPeUSDVaultBase.sol
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

```solidity
File: /contracts/lybra/token/PeUSDMainnetStableVision.sol
80    require(_msgSender() == user || _msgSender() == address(this), "MDM");

81    require(EUSD.balanceOf(address(this)) + eusdAmount <= configurator.getEUSDMaxLocked(),"ESL");

82    bool success = EUSD.transferFrom(user, address(this), eusdAmount);

133   bool success = EUSD.transferFrom(address(receiver), address(this), EUSD.getMintedEUSDByShares(shareAmount));

176   return address(this);

199   if (_from != address(this) && _from != spender)
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L80

## [G-10] Use Assembly To Check For address(0)
it's generally more gas-efficient to use assembly to check for a zero address (address(0)) than to use the == operator.

The reason for this is that the == operator generates additional instructions in the EVM bytecode, which can increase the gas cost of your contract. By using assembly, you can perform the zero address check more efficiently and reduce the overall gas cost of your contract.

Here's an example of how you can use assembly to check for a zero address:

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
In the above example, we have a function isZeroAddress that takes an address as input and returns a boolean value indicating whether the address is equal to the zero address. Inside the function, we convert the address to an integer using uint256(addr), and then use assembly to compare the integer to the zero address.

By using assembly to perform the zero address check, we can make our code more gas-efficient and reduce the overall cost of our contract. It's important to note that assembly can be more difficult to read and maintain than Solidity code, so it should be used with caution and only when necessary

Saves 6 gas per instance:
```solidity
File: /contracts/lybra/configuration/LybraConfigurator.sol
99   if (address(EUSD) == address(0)) EUSD = IEUSD(_eusd);

100  if (address(peUSD) == address(0)) peUSD = IEUSD(_peusd);
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L99

```solidity
File: /contracts/lybra/miner/EUSDMiningIncentives.sol
76   if (_account != address(0)) {    
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L76

```solidity
File: /contracts/lybra/miner/stakerewardV2pool.sol
60   if (_account != address(0)) {
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L60


## [G-11] Use constants instead of type(uintx).max
 it's generally more gas-efficient to use constants instead of type(uintX).max when you need to set the maximum value of an unsigned integer type.

The reason for this is that the type(uintX).max expression involves a computation at runtime, whereas a constant is evaluated at compile-time. This means that using type(uintX).max can result in additional gas costs for each transaction that involves the expression.

By using a constant instead of type(uintX).max, you can avoid these additional gas costs and make your code more efficient.

Here's an example of how you can use a constant instead of type(uintX).max:
```
contract MyContract {
    uint120 constant MAX_VALUE = 2**120 - 1;
    
    function doSomething(uint120 value) public {
        require(value <= MAX_VALUE, "Value exceeds maximum");
        
        // Do something
    }
}
```
In the above example, we have a contract with a constant MAX_VALUE that represents the maximum value of a uint120. When the doSomething function is called with a value parameter, it checks whether the value is less than or equal to MAX_VALUE using the <= operator.

By using a constant instead of type(uint120).max, we can make our code more efficient and reduce the gas cost of our contract.

It's important to note that using constants can make your code more readable and maintainable, since the value is defined in one place and can be easily updated if necessary. However, constants should be used with caution and only when their value is known at compile-time.


```solidity
File: /contracts/lybra/token/EUSD.sol
179   if (currentAllowance != type(uint256).max) {
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L179


## [G-12] State variables can be packed into fewer storage slots
If variables occupying the same slot are both written the same function or by the constructor, avoids a separate Gsset (20000 gas). Reads of the variables are also cheaper.


```solidity
File: /contracts/lybra/miner/EUSDMiningIncentives.sol
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

## [G-13] Make 3 event parameters indexed when possible
It’s the most gas efficient to make up to 3 event parameters indexed. If there are less than 3 parameters, you need to make all parameters indexed.

```solidity
File: /contracts/lybra/miner/EUSDMiningIncentives.sol
59    event ClaimReward(address indexed user, uint256 amount, uint256 time);

60    event ClaimedOtherEarnings(address indexed user, address indexed Victim, uint256 buyAmount, uint256 biddingFee, bool useEUSD, uint256 time);
 
61    event NotifyRewardChanged(uint256 addAmount, uint256 time);
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L59

```solidity
File: /contracts/lybra/configuration/LybraConfigurator.sol
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

```solidity
File: /contracts/lybra/miner/ProtocolRewardsPool.sol
42   event Restake(address indexed user, uint256 amount, uint256 time);
    event StakeLBR(address indexed user, uint256 amount, uint256 time);
    event UnstakeLBR(address indexed user, uint256 amount, uint256 time);
    event WithdrawLBR(address indexed user, uint256 amount, uint256 time);
    event ClaimReward(address indexed user, uint256 eUSDAmount, address token, uint256 tokenAmount, uint256 time);
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L42-L46

```solidity
File: /contracts/lybra/miner/stakerewardV2pool.sol
44  event StakeToken(address indexed user, uint256 amount, uint256 time);
    event WithdrawToken(address indexed user, uint256 amount, uint256 time);
    event ClaimReward(address indexed user, uint256 amount, uint256 time);
    event NotifyRewardChanged(uint256 addAmount, uint256 time);

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L44

```solidity
File: /contracts/lybra/pools/base/LybraEUSDVaultBase.sol
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
File: /contracts/lybra/pools/base/LybraPeUSDVaultBase.sol
    event DepositEther(address indexed onBehalfOf, address asset, uint256    etherAmount, uint256 assetAmount, uint256 timestamp);

    event DepositAsset(address indexed onBehalfOf, address asset, uint256 amount, uint256 timestamp);
    event WithdrawAsset(address sponsor, address indexed onBehalfOf, address asset, uint256 amount, uint256 timestamp);
    event Mint(address sponsor, address indexed onBehalfOf, uint256 amount, uint256 timestamp);
    event Burn(address sponsor, address indexed onBehalfOf, uint256 amount, uint256 timestamp);
    event LiquidationRecord(address provider, address keeper, address indexed onBehalfOf, uint256 eusdamount, uint256 LiquidateAssetAmount, uint256 keeperReward, bool superLiquidation, uint256 timestamp);

    event RigidRedemption(address indexed caller, address indexed provider, uint256 peusdAmount, uint256 assetAmount, uint256 timestamp);
    event FeeDistribution(address indexed feeAddress, uint256 feeAmount, uint256 timestamp);
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L26-L35


## [G-14] Using fixed bytes is cheaper than using string
As a rule of thumb, use bytes for arbitrary-length raw byte data and string for arbitrary-length string (UTF-8) data. If you can limit the length to a certain number of bytes, always use one of bytes1 to bytes32 because they are much cheaper.

```solidity
File: /contracts/lybra/governance/LybraGovernance.sol
38   constructor(string memory name_, TimelockController timelock_, address _esLBR) GovernorTimelockControl(timelock_)  Governor(name_) {

151   function CLOCK_MODE() public override view returns (string memory){

156   function COUNTING_MODE() public override view virtual returns (string memory){            
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol#L38

```solidity
File: /contracts/lybra/token/EUSD.sol
99   function name() public pure returns (string memory) {

107  function symbol() public pure returns (string memory) {    
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L99