## Summary

### Gas Optimization

no | Issue |Instances||
|-|:-|:-:|:-:|
| [G-01] | Using fixed bytes is cheaper than using string | 1 | - |
| [G-02] | Can make the variable outside the loop to save gas | 1 | - |
| [G-03] | Using calldata instead of memory for read-only arguments in external functions saves gas | 2 | - |
| [G-04] | Use assembly to check for address(0) | 5 | - |
| [G-05] | Before transfer of  some functions, we should check some variables for possible gas save | 5 | - |
| [G-06] | State variables can be packed to use fewer storage slots | 1 | - |
| [G-07] | Use constants instead of type(uintx).max | 1 | - |
| [G-08] | Use nested if and, avoid multiple check combinations | 3 | - |
| [G-09] | Use assembly to write address storage values | 9 | - |
| [G-10] | Sort Solidity operations using short-circuit mode | 3 | - |
| [G-11] | Use hardcode address instead address(this) | 8 | - |
| [G-12] | Use assembly for math (add, sub, mul, div) | 2 | - |
| [G-13] | Don't apply the same value to state variables | 4 | - |
| [G-14] |  Minimize external calls if possible | more files | - |




## Gas Optimizations  

## [G-1]  Using fixed bytes is cheaper than using string

### Details

As a rule of thumb, use bytes for arbitrary-length raw byte data and string for arbitrary-length string (UTF-8) data.
If you can limit the length to a certain number of bytes, always use one of bytes1 to bytes32 because they are much cheaper

```solidity
file: /contracts/lybra/token/EUSD.sol

107        function symbol() public pure returns (string memory) {
108          return "eUSD";

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L107


## [G-2]  Can make the variable outside the loop to save gas

Consider making the stack variables before the loop which gonna save gas 

```solidity
file: /contracts/lybra/miner/EUSDMiningIncentives.sol

140    uint borrowed = pool.getBorrowedOf(user);

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L140


## [G-3]  Using calldata instead of memory for read-only arguments in external functions saves gas 

calldata must be used when declaring  an external function's dynamic parameters

When a function with a memory array is called externally, the abi.decode ()  step has to use a for-loop to copy each index of the calldata to the memory index. Each iteration of this for-loop costs at least 60 gas (i.e. 60 * <mem_array>.length). Using calldata directly, obliviates the need for such a loop in the contract code and runtime execution.

```solidity
file: /contracts/lybra/miner/esLBRBoost.sol

33       function addLockSetting(esLBRLockSetting memory setting) external onlyOwner {

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol#L33


```solidity
file: /contracts/lybra/miner/EUSDMiningIncentives.sol

93        function setPools(address[] memory _pools) external onlyOwner {

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L93


## [G-4]  Use assembly to check for address(0)

  Save 6 gas per instance.

```solidity
file: /contracts/lybra/miner/stakerewardV2pool.sol

60       if (_account != address(0)) {

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L60


```solidity
file: /contracts/lybra/token/PeUSDMainnetStableVision.sol

64      require(to != address(0), "TZA");

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L64


```solidity
file: /contracts/lybra/configuration/LybraConfigurator.sol

99         if (address(EUSD) == address(0)) EUSD = IEUSD(_eusd);
100        if (address(peUSD) == address(0)) peUSD = IEUSD(_peusd);

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L99-L100


```solidity
file: /contracts/lybra/miner/EUSDMiningIncentives.sol

76        if (_account != address(0)) {

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L76

## [G-5] Before transfer of  some functions, we should check some variables for possible gas save

Before transfer, we should check for amount being 0 so the function doesn't run when its not gonna do anything.


```solidity
file: /contracts/lybra/token/PeUSDMainnetStableVision.sol
/// @audit before of tranfering, should check by if()/require().

133        bool success = EUSD.transferFrom(address(receiver), address(this), EUSD.getMintedEUSDByShares(shareAmount));

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L133

```solidity
file: /contracts/lybra/miner/ProtocolRewardsPool.sol#

210       token.transfer(msg.sender, tokenAmount);

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L210

```solidity
file: /contracts/lybra/token/EUSD.sol

339          emit Transfer(owner, _recipient, tokensAmount);

351          emit Transfer(_sender, _recipient, _amount);

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L339


## [G-6] State variables can be packed to use fewer storage slots

### Details

The EVM works with 32 byte words. Variables less than 32 bytes can be declared next to eachother in storage and this will pack the values together into a single 32 byte storage slot (if the values combined are <= 32 bytes). If the variables packed together are retrieved together in functions we will effectively save ~2000 gas with every subsequent SLOAD for that storage slot. This is due to us incurring a Gwarmaccess (100 gas) versus a Gcoldsload (2100 gas).
If variables occupying the same slot are both written the same function or by the constructor, avoids a separate Gsset (20000 gas),Reads of the variables are also cheaper.

### Instances
```solidity
file: /contracts/lybra/configuration/LybraConfigurator.sol

38    mapping(address => bool) public mintVault;
39    mapping(address => uint256) public mintVaultMaxSupply;
40    mapping(address => bool) public vaultMintPaused;
41    mapping(address => bool) public vaultBurnPaused;
42    mapping(address => uint256) vaultSafeCollateralRatio;
43    mapping(address => uint256) vaultBadCollateralRatio;
44    mapping(address => uint256) public vaultMintFeeApy;
45    mapping(address => uint256) public vaultKeeperRatio;
46    mapping(address => bool) redemptionProvider;
47    mapping(address => bool) public tokenMiner;

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L38-L47

### Recommended

Sort of Variables to use fewer storage slots.
```solidiy
38    mapping(address => bool) public mintVault;
39    mapping(address => bool) public vaultMintPaused;
40    mapping(address => bool) public vaultBurnPaused;
41    mapping(address => bool) redemptionProvider;
42    mapping(address => bool) public tokenMiner;
43    mapping(address => uint256) public mintVaultMaxSupply;
44    mapping(address => uint256) vaultSafeCollateralRatio;
45    mapping(address => uint256) vaultBadCollateralRatio;
46    mapping(address => uint256) public vaultMintFeeApy;
47    mapping(address => uint256) public vaultKeeperRatio;
```

## [G-7] Use constants instead of type(uintx).max

type(uint120).max or type(uint112).max, etc. it uses more gas in the distribution process and also for each transaction than constant usage.


```solidity
file: /contracts/lybra/token/EUSD.sol

179        if (currentAllowance != type(uint256).max) {

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L179


## [G-8] Use nested if and, avoid multiple check combinations

Using nested if is cheaper than using && multiple check combinations. There are more advantages, such as easier to read code and better coverage reports.


```solidity
file: /contracts/lybra/token/PeUSD.sol

46        if (_from != address(this) && _from != spender)

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSD.sol#L46

```solidity
file: /contracts/lybra/token/LBR.sol

64        if (_from != address(this) && _from != spender)

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol#L64

```solidity
file: /contracts/lybra/token/PeUSDMainnetStableVision.sol

199        if (_from != address(this) && _from != spender)

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L199



### Recommended
```solidity
  - 46:      if (_from != address(this) && _from != spender)
  +           if ( from != address(this)){
  +               if( _from != spender){
  +
  +               }
  +           }

```

## [G-9]  Use assembly to write address storage values

By using assembly to write to address storage values, you can bypass some of these operations and lower the gas cost of writing to storage. Assembly code allows you to directly access the Ethereum Virtual Machine (EVM) and perform low-level operations that are not possible in Solidity.

example of using assembly to write to address storage values:
```solidity
contract MyContract {
    address private myAddress;
    
    function setAddressUsingAssembly(address newAddress) public {
        assembly {
            sstore(0, newAddress)
        }
    }
}
```
Instances:
```solidity
file: /contracts/lybra/governance/GovernanceTimelock.sol

25    function checkRole(bytes32 role, address _sender) public view  returns(bool){

29    function checkOnlyRole(bytes32 role, address _sender) public view  returns(bool){    

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/GovernanceTimelock.sol#L25

```solidity
file: /contracts/lybra/token/PeUSD.sol

31    function _debitFrom(address _from, uint16, bytes32, uint _amount) internal virtual override returns (uint) {

38    function _creditTo(uint16, address _toAddress, uint _amount) internal virtual override returns (uint) {

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSD.sol#L31


```solidity
file: /contracts/lybra/token/LBR.sol

56    function _creditTo(uint16, address _toAddress, uint _amount) internal virtual override returns (uint) {

61    function _transferFrom(address _from, address _to, uint _amount) internal virtual override returns (uint) {    

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol#L56


```solidity
file: /contracts/lybra/miner/stakerewardV2pool.sol

101    function getBoost(address _account) public view returns (uint256) {

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L101


```solidity
file: /contracts/lybra/token/PeUSDMainnetStableVision.sol

191    function _creditTo(uint16, address _toAddress, uint _amount) internal virtual override returns (uint) {

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L191


```solidity
file: /contracts/lybra/token/EUSD.sol

200    function approve(address _spender, uint256 _amount) public returns (bool) {

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L200


## [G-10]  Sort Solidity operations using short-circuit mode

Short-circuiting is a solidity contract development model that uses OR/AND logic to sequence different cost operations. It puts low gas cost operations in the front and high gas cost operations in the back, so that if the front is low If the cost operation is feasible, you can skip (short-circuit) the subsequent high-cost Ethereum virtual machine operation.

```solidity
//f(x) is a low gas cost operation 
//g(y) is a high gas cost operation 
//Sort operations with different gas costs as follows
 f(x) || g(y) 
 f(x) && g(y)

```

Instaces:
```solidity
file: /contracts/lybra/miner/esLBRBoost.sol

60        if (userUpdatedAt >= boostEndTime || userUpdatedAt >= finishAt) {

63        if (finishAt <= boostEndTime || block.timestamp <= boostEndTime) {    

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol#L60


```solidity
file: /contracts/lybra/token/PeUSDMainnetStableVision.sol

80        require(_msgSender() == user || _msgSender() == address(this), "MDM");

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L80


## [G-11]  Use hardcode address instead address(this)
 
 Instead of using address(this), it is more gas-efficient to pre-calculate and use the hardcoded address. Foundry’s script.sol and solmate’s LibRlp.sol contracts can help achieve this.
References: https://book.getfoundry.sh/reference/forge-std/compute-create-address


Here's an example :
```solidity
contract MyContract {
    address constant public CONTRACT_ADDRESS = 0x1234567890123456789012345678901234567890;
    
    function getContractAddress() public view returns (address) {
        return CONTRACT_ADDRESS;
    }
}
```

```solidity
file: /contracts/lybra/pools/LybraStETHVault.sol

63        uint256 excessAmount = collateralAsset.balanceOf(address(this)) - totalDepositedAsset;

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L63


```solidity
file: /contracts/lybra/miner/stakerewardV2pool.sol

85        bool success = stakingToken.transferFrom(msg.sender, address(this), _amount);

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L85


```solidity
file: /contracts/lybra/token/PeUSDMainnetStableVision.sol

82         bool success = EUSD.transferFrom(user, address(this), eusdAmount);

133        bool success = EUSD.transferFrom(address(receiver), address(this), EUSD.getMintedEUSDByShares(shareAmount));

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L82


```solidity
file: /contracts/lybra/miner/ProtocolRewardsPool.sol

195            uint256 balance = EUSD.sharesOf(address(this));

200            uint256 peUSDBalance = peUSD.balanceOf(address(this));

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L195

```solidity
file: /contracts/lybra/configuration/LybraConfigurator.sol

290        uint256 peUSDBalance = peUSD.balanceOf(address(this));

295        uint256 balance = EUSD.balanceOf(address(this));

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L290



## [G-12] Use assembly for math (add, sub, mul, div)

Use assembly for math instead of Solidity. You can check for overflow/underflow in assembly to ensure safety. If using Solidity versions < 0.8.0 and you are using Safemath, you can gain significant gas savings by using assembly to calculate values and checking for overflow/underflow.


Examples: Multiplication:
```solidity
 //multiplication in Solidity
  function mulTest(uint256 a, uint256 b) public pure {
    uint256 c = a * b;
 }
```
Gas: 325
```solidity
//multiplication in assembly
function mulAssemblyTest(uint256 a, uint256 b) public pure {
    assembly {
        let c := mul(a, b)
        if lt(c, a) {
            mstore(0x00, "overflow")
            revert(0x00, 0x20)
        }
    }
}
```
Gas: 265

Division:
```solidity
///division in Solidity
function divTest(uint256 a, uint256 b) public pure {
    uint256 c = a / b;
}
```
Gas: 325
```solidity
///division in assembly
function divAssemblyTest(uint256 a, uint256 b) public pure {
    assembly {
        let c := div(a, b)
        if gt(c, a) {
            mstore(0x00, "underflow")
            revert(0x00, 0x20)
        }
    }
}
```
`Instances`:
```solidity
file: /contracts/lybra/pools/LybraStETHVault.sol

104         return 10000 - (time / 30 minutes - 1) * 100;

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L104

```solidity
file: /contracts/lybra/miner/EUSDMiningIncentives.sol

210         uint256 biddingFee = (reward * biddingFeeRatio) / 10000;

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L210


### Recommended Code
```solidity
    uint256 value;
    assembly {
        // Calculate time / 30 minutes
        let quotient := div(time, 1800)

        // Subtract 1 from quotient
        let subtracted := sub(quotient, 1)

        // Multiply subtracted by 100
        let multiplied := mul(subtracted, 100)

        // Subtract multiplied from 10000
        value := sub(10000, multiplied)
    }
    return value;
}
```

## [G-13] Don't apply the same value to state variables

Assigning the same value to a state variable repeatedly can cause unnecessary gas costs, because each assignment operation incurs a gas cost. Additionally, if the value being assigned is the same as the current value of the state variable, the assignment operation does not actually change anything, which can waste gas.


```solidity
file: /contracts/lybra/configuration/LybraConfigurator.sol

/// @audit the premiumTradingEnabled and vaultMintPaused[pool] are state variable, which have same value.

168        premiumTradingEnabled = isActive;
178        vaultMintPaused[pool] = isActive;


204        vaultSafeCollateralRatio[pool] = newRatio;
226        vaultKeeperRatio[pool] = newRatio;

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L168

```solidity
file: /contracts/lybra/miner/ProtocolRewardsPool.sol

120        unstakeRatio[msg.sender] = 0;
121        time2fullRedemption[msg.sender] = 0;

144        unstakeRatio[msg.sender] = 0;
145        time2fullRedemption[msg.sender] = 0;

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L120-L121


## [G-14]  Minimize external calls if possible

 External calls to other contracts are expensive in terms of gas. Try to minimize external calls by using in-line code or caching data in memory.


```solidity
file: more file of lybra Finance

            More files of lybra Finance have external functions and call.

```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/