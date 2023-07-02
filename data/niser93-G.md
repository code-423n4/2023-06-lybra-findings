## Gas Optimizations
|       |Issue  |Instances|Total Gas Saved|
|:-----:|:-----:|:-------:|:-------------:|
|GO-01|```>=``` is cheaper than ```>``` (and ```<=``` is cheaper than ```<```)|2|6|
|GO-02|Use alternative formulation in order to avoid NOT using AND instead of OR or vice versa|3|9|
|GO-03|Multiplication/division by 2 should use bit shifting|2|4|
|GO-04|Using ```private``` rather than ```public``` for constants, saves gas|16|-|
|GO-05|```require()``` or ```revert()``` statements that check input arguments should be at the top of the function|1|-|

### Comment
Due to the lack of test, we aren't be able to report gas saving tables.
Some report titles are same of [winning bot](https://gist.github.com/liveactionllama/27513952718ec3cbcf9de0fda7fef49c), but we report instances whose are't already found.

## GO-01
### ```>=``` is cheaper than ```>``` (and ```<=``` is cheaper than ```<```)
#### Description
Non-strict inequalities (>=) are cheaper than strict ones (>). This is due to some supplementary checks (ISZERO, 3 gas)).
 n>i is equivalent to n>=i-1 when n and i are integer


```diff
- for (uint256 i = 0; i < count + 10; i++) {
+ for (uint256 i = 0; i <= count + 9; i++) {
```            

```diff
- for (uint256 i = 100; i > count + 10; i--) {
+ for (uint256 i = 100; i >= count + 11; i--) {
```            

```
File: EUSDMiningIncentives.sol

  
  189         return (stakedLBRLpValue(user) * 10000) / stakedOf(user) < 500;

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L189](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L189)

```
File: LybraConfigurator.sol

  
  254         if (fee > 10_000) revert('EL');

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L254](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L254)

#### Mitigation
```diff
File: EUSDMiningIncentives.sol

- return (stakedLBRLpValue(user) * 10000) / stakedOf(user) < 500;
+ return (stakedLBRLpValue(user) * 10000) / stakedOf(user) <= 499;

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L189](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L189)


```diff
File: LybraConfigurator.sol

- if (fee > 10_000) revert('EL');
+ if (fee >= 10_001) revert('EL');

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L254](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L254)


## GO-02
### Use alternative formulation in order to avoid NOT using AND instead of OR or vice versa
#### Description
It's possible to change boolean formula in order to use AND instead of OR or vice versa.

In this way, ```>``` could be substituted with ```>=```, ```!=``` with ```==```

 In general

#### Two variables

```
(a || b) = !(!(a || b)) = !(!a && !b)
```

| a   | b   | (a or b)  | !a and !b  | !(!a and !b)|
|:---:|:---:|:---------:|:----------:|:------------:|
| 0  | 0  | 0         | 1          | 0            |
| 0  | 1  | 1         | 0          | 1            |
| 1  | 0  | 1         | 0          | 1            |
| 1  | 1  | 1         | 0          | 1            |


#### Three variables

```
(a || b || c) = !(!(a || b || c)) = !(!a && !b && !c)
```

| a   | b   | c   | (a or b or c)  | !a and !b and !c  | !(!a and !b and !c)|
|:---:|:---:|:---:|:--------------:|:-----------------:|:------------------:|
| 0  | 0  | 0 | 0              | 1                 | 0                  |
| 0  | 0  | 1 | 1              | 0                 | 1                  |
| 0  | 1  | 0 | 1              | 0                 | 1                  |
| 0  | 1  | 1 | 1              | 0                 | 1                  |
| 1  | 0  | 0 | 1              | 0                 | 1                  |
| 1  | 0  | 1 | 1              | 0                 | 1                  |
| 1  | 1  | 0 | 1              | 0                 | 1                  |
| 1  | 1  | 1 | 1              | 0                 | 1                  |


```diff
- if(a>b && c>d)
+ if(!(a<=b || c<=d))
```            

```diff
- if(a>b || c>d)
+ if(!(a<=b && c<=d))
```            

```
File: LBR.sol

  
  64         if (_from != address(this) && _from != spender)
  65             _spendAllowance(_from, spender, _amount);

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol#L64-L65](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol#L64-L65)

```
File: PeUSD.sol

  
  46         if (_from != address(this) && _from != spender)
  47             _spendAllowance(_from, spender, _amount);

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSD.sol#L46-L47](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSD.sol#L46-L47)

```
File: PeUSDMainnetStableVision.sol

  
  199         if (_from != address(this) && _from != spender)
  200             _spendAllowance(_from, spender, _amount);

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L199-L200](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L199-L200)

#### Mitigation

```diff
File: LBR.sol

- if (_from != address(this) && _from != spender)
+ if ((_from == address(this) || _from == spender))
      _spendAllowance(_from, spender, _amount);

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol#L64-L65](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol#L64-L65)

```diff
File: PeUSD.sol

  
- if (_from != address(this) && _from != spender)
+ if ((_from == address(this) || _from == spender))
      _spendAllowance(_from, spender, _amount);

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSD.sol#L46-L47](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSD.sol#L46-L47)

```diff
File: PeUSDMainnetStableVision.sol

  
- if (_from != address(this) && _from != spender)
+ if ((_from == address(this) || _from == spender))
      _spendAllowance(_from, spender, _amount);

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L199-L200](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L199-L200)

## GO-03
### Multiplication/division by 2 should use bit shifting
#### Description
```<x> * 2``` is equivalent to ```<x> << 1``` and ```<x> / 2``` is the same as ```<x> >> 1```. The ```MUL``` and ```DIV``` opcodes cost 5 gas, whereas ```SHL``` and ```SHR``` only cost 3 gas.



```diff
- n*2
+ n << 1
```            

```diff
- n/2
+ n >> 1
```            

```
File: LybraEUSDVaultBase.sol

  
  159         require(assetAmount * 2 <= depositedAsset[onBehalfOf], "a max of 50% collateral can be liquidated");

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L159](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L159)

```
File: LybraPeUSDVaultBase.sol

  
  130         require(assetAmount * 2 <= depositedAsset[onBehalfOf], "a max of 50% collateral can be liquidated");

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L130](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L130)

#### Mitigation

```diff
File: LybraEUSDVaultBase.sol

- require(assetAmount * 2 <= depositedAsset[onBehalfOf], "a max of 50% collateral can be liquidated");
+ require((assetAmount << 1) <= depositedAsset[onBehalfOf], "a max of 50% collateral can be liquidated");

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L159](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L159)

```diff
File: LybraPeUSDVaultBase.sol

- require(assetAmount * 2 <= depositedAsset[onBehalfOf], "a max of 50% collateral can be liquidated");
+ require((assetAmount << 1) <= depositedAsset[onBehalfOf], "a max of 50% collateral can be liquidated");

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L130](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L130)


## GO-4
### Using ```private``` rather than ```public``` for constants, saves gas
#### Description
If needed, the values can be read from the verified contract source code, or if there are multiple values there can be a single getter function that [returns a tuple](https://github.com/code-423n4/2022-08-frax/blob/90f55a9ce4e25bceed3a74290b854341d8de6afa/src/contracts/FraxlendPair.sol#L156-L178) of the values of all currently-public constants. Saves *3406-3606* gas in deployment gas due to the compiler not having to create non-payable getter functions for deployment calldata, not having to store the bytes of the value outside of where it's used, and not adding another entry to the method ID table


<details>
<summary>There are 24 instances of this issue:</summary>

```
File: EUSD.sol

  
  39     Iconfigurator public immutable configurator;

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L39](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L39)

```
File: EUSDMiningIncentives.sol

  
  28     Iconfigurator public immutable configurator;

  
  30     IEUSD public immutable EUSD;

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L28](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L28)



```
File: LBR.sol

  
  14     Iconfigurator public immutable configurator;

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol#L14](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol#L14)



```
File: LybraEUSDVaultBase.sol

  
  18     IEUSD public immutable EUSD;

  
  19     IERC20 public immutable collateralAsset;

  
  20     Iconfigurator public immutable configurator;

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L18](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L18)

```
File: LybraPeUSDVaultBase.sol

  
  14     IPeUSD public immutable PeUSD;

  
  15     IERC20 public immutable collateralAsset;

  
  16     Iconfigurator public immutable configurator;

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L14](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L14)

```
File: PeUSDMainnetStableVision.sol

  
  30     IEUSD public immutable EUSD;

  
  31     Iconfigurator public immutable configurator;

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L30](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L30)

```
File: ProtocolRewardsPool.sol

  
  25     Iconfigurator public immutable configurator;

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L25](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L25)

```
File: esLBR.sol

  
  18     Iconfigurator public immutable configurator;

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/esLBR.sol#L18](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/esLBR.sol#L18)

```
File: stakerewardV2pool.sol

  
  18     IERC20 public immutable stakingToken;

  
  19     IesLBR public immutable rewardsToken;

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L18](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L18)

            
</details>

## GO-05
### ```require()``` or ```revert()``` statements that check input arguments should be at the top of the function
#### Description
Checks that involve constants should come before checks that involve state variables, function calls, and calculations. By doing these checks first, the function is able to revert before wasting a Gcoldsload (*2100 gas*) in a function that may ultimately revert in the unhappy case.


```
File: LBR.sol

  
  21         require(_sharedDecimals <= decimals, "OFT: sharedDecimals must be <= decimals");

```
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol#L21](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol#L21)

