# **GAS OPTIMIZATIONS**

## [G-01] Unused Imports
The following files were imported but were not used. These files would costs gas during deployment and is a bad coding practice .


- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWbETHVault.sol#L5
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWbETHVault.sol#L7
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWstETHVault.sol#L5
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWstETHVault.sol#L7
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraRETHVault.sol#L5
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraRETHVault.solL7
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#5


## [G-02] Use bit shifting for multiplication by 2

- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L130


## [G-03] Sort Solidity operations using short-circuit mode
Short-circuiting is a solidity contract development model that uses OR/AND logic to sequence different cost operations. It puts low gas cost operations in the front and high gas cost operations in the back, so that if the front is low If the cost operation is feasible, you can skip (short-circuit) the subsequent high-cost Ethereum virtual machine operation.

//f(x) is a low gas cost operation 
//g(y) is a high gas cost operation 

//Sort operations with different gas costs as follows 
f(x) || g(y) 
f(x) && g(y)

The following instances could be restructured to use short-circuit mode
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSD.sol#L46
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol#L64
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L199
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#298



## [G-04] Reorder the require statements to have the less gas consuming before the expensive one

Checks that involve constants should come before checks that involve state variables, function calls, and calculations. In scenarios where this cheap checks would fail the function is able to revert before wasting alot of gas in a function that may ultimately revert in the unhappy case. 
### Proof of concept
consider the following scenario:
```solidity
function mint(address onBehalfOf, uint256 amount) external virtual {
        require(onBehalfOf != address(0), "TZA");
        require(amount > 0, "ZA");
        _mintPeUSD(msg.sender, onBehalfOf, amount, getAssetPrice());
}
```
The require(amount > 0) statement cost lesser gas than the require(onBehalfOf != address(0)) so in scenarios where the require(amount > 0) would fail the function would consume more a lot more gas than if it were re-arranged. so the above code snippet could be re-written as:
```solidity
function mint(address onBehalfOf, uint256 amount) external virtual {
        require(amount > 0, "ZA");
        require(onBehalfOf != address(0), "TZA");
        _mintPeUSD(msg.sender, onBehalfOf, amount, getAssetPrice());
}
```
(7 Instances)
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L83
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L97
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L111
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L99
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L126
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L223
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol#L7kv8


## [G-05] Storage variables should be cached in stack variables rather than re-reading them from storage
In the scenarios below storage variables are read in a conditional statement. Depending on if the conditional statement results to a true or false the state variable could be re-read from storage thereby causing a Gwarmaccess which cost 100 gas. You should consider caching the storage variable before the conditionals this replaces each Gwarmaccess (100 gas) with a much cheaper stack read.

### Proof of concept
if the condition results to true the storage value time2fullRedemption[msg.sender] would be read twice.
```solidity
if (time2fullRedemption[msg.sender] > block.timestamp) {
            total += unstakeRatio[msg.sender] * (time2fullRedemption[msg.sender] - block.timestamp);
}
```
This can be done instead
```solidity
uint time2fullR = time2fullRedemption[msg.sender];
if (time2fullR > block.timestamp) {
            total += unstakeRatio[msg.sender] * (time2fullR - block.timestamp);
}
```

### Depending on if the conditionals result to true (13 instances)
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L92-#L93 
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L156-#L157  
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L162-#L163
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L91-#L92
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L156-#L157
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L162-#L163
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L334-#L335
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L339-#L340
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L156-#L159
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L190-#L202
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L234-#L236
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L127-#L136
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L159-#L161

### Depending on if the conditionals resut to false (3 instances)
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L133-#L136
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L277
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L196

### The below instances do not involve conditionals but the storage variables should be cached
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol#L58
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L168


## [G-06] Make storage variable constant 
storage variable constant whose values do not change should be constant
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/esLBR.sol#L20


## [G-07] Using function call to access a global variables is more expensive than using the actual variable
Using function calls to access a global variables is more expensive than using the actual variable use msg.sender instead of calling _msgSender().

### Proof of concept
consider the code below:
```solidity
contract myERC20 is ERC20{
    
    mapping(address => uint256) private _balances;

    mapping(address => mapping(address => uint256)) private _allowances;

    uint256 private _totalSupply = 1000;

    string private _name;
    string private _symbol;

    constructor() ERC20("MyERC20", "MyErc"){
        _mint(msg.sender, 200);
    }

    

    function transfer2(address to, uint256 amount) public virtual returns (bool) {
        address owner = msg.sender;  // msg.sender 
        _transfer(owner, to, amount);
        return true;
    }

    function warmStorage() public{
        transfer(0xfa458421F9E92B677D1724e2A0F0bF378940333e, 2);
    }
    
}
```
the test script:
```javascript
const {ethers} = require('hardhat');
const assert = require('assert');


describe("Gas Tests", async function() {
    it("Comparing Transfer functions", async function(){

        const myErcFactory = await ethers.getContractFactory("myERC20");
        const myErc20 = await myErcFactory.deploy();

        await myErc20.warmStorage();
        await myErc20.transfer2("0xfa458421F9E92B677D1724e2A0F0bF378940333e", 10);
        await myErc20.transfer("0xfa458421F9E92B677D1724e2A0F0bF378940333e", 10); //inherited transfer function uses _msgSender() 
        
        
    })
})
```
the test result:

```bash
·············|···············|··············|·············|·············|···············|··············
|  Contract  ·  Method       ·  Min         ·  Max        ·  Avg        ·  # calls      ·  usd (avg)  │
·············|···············|··············|·············|·············|···············|··············
|  myERC20   ·  transfer     ·           -  ·          -  ·      35017  ·            1  ·          -  │
·············|···············|··············|·············|·············|···············|··············
|  myERC20   ·  transfer2    ·           -  ·          -  ·      34983  ·            1  ·          -  │
·············|···············|··············|·············|·············|···············|··············
|  myERC20   ·  warmStorage  ·           -  ·          -  ·      51027  ·            1  ·          -  │
·············|···············|··············|·············|·············|···············|··············
|  Deployments               ·                                          ·  % of limit   ·             │
·····························|··············|·············|·············|···············|··············
|  myERC20                   ·           -  ·          -  ·    1225730  ·        4.1 %  ·          -  │
·----------------------------|--------------|-------------|-------------|---------------|-------------·
```
transfer() = 35017;
transfer2() = 34983
difference = 34 gas units

### 16 Instances
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L154
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L201
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L227
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L249
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#269
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L335
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L197
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L185
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L142
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L103
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L104
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L80
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol#L50
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol#L62
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSD.sol#L44
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSD.sol#L32

total gas saved = 34 * 16 = 544 gas units