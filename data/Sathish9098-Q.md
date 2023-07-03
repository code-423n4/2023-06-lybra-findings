# LOW FINDINGS

| LOW COUNT | ISSUES | INSTANCES |
|-----------------|-----------------|-----------------|
| [L-1] | ``assetAmount * 2 <= depositedAsset[onBehalfOf]`` check is conflict with docs  | 1 |
| [L-2] | Lack of ``check-effects-interactions (CEI) pattern`` is vulnerable to ``reentrancy attacks`` | 3 |
| [L-3] | User could deposit a small amount of ``ether`` and then ``mint`` a large amount of ``PEUSD``, which could create a security risk | 1 |
| [L-4] | Use the ``safeAllowance()`` function instead of the normal ``approve()`` function | 1 |
| [L-5] | Use ``call`` instead of ``transfer`` to send ethers | 3 |
| [L-6] | ``Timelocks`` not implemented as per documentation  | - |
| [L-7] | ``depositAssetToMint()`` does not check for a maximum limit on asset deposits | 1 |
| [L-8] | Project Upgrade and Stop Scenario should be | - |
| [L-9] | Lack of ``nonReentrant`` modifiers for critical external functions  | 1 |
| [L-10] | ``Divide by zero`` should be avoided  | 2 |
| [L-11] | ````name masking`` should be avoided to ``prevent errors``  | 2 |
| [L-12] | ``Division`` before ``multiplication`` can lead to ``precision errors`` | 2 |
| [L-13] | ``Hardcoding addresses`` in smart contracts can make them more ``vulnerable to attack``| 1 |
| [L-14] | ``NATSPEC comments`` should be ``increased`` in contracts| - |
| [L-15] | ``Negative values`` may return the ``unexpected`` values when using ``uint256(int)``conversion| 1|
| [L-16] | ``Array`` does not have a ``pop function``| 1 |
| [L-17] | ``Setters`` should have ``initial value check``| 1 |
| [L-18] | ``External calls`` in an ``un-bounded`` for-loop may result in a ``DOS``| 2 |

##

## [L-1] ``assetAmount * 2 <= depositedAsset[onBehalfOf]`` check is conflict with docs 

``* - collateralAmount should be less than 50% of collateral``

### Impact 
The comment for the function states that the collateral amount should be ``less than 50%`` of the ``deposited asset``. However, the ``implementation`` of the function actually allows for the ``collateral amount`` to be ``equal to 50%`` of the ``deposited asset``

This is a potential ``security vulnerability``, as it could allow a user to ``liquidate`` more collateral than they are ``entitled`` to.

### POC

```solidity
FILE: 2023-06-lybra/contracts/lybra/pools/base/LybraEUSDVaultBase.sol

* - collateralAmount should be less than 50% of collateral

159:require(assetAmount * 2 <= depositedAsset[onBehalfOf], "a max of 50% collateral can be liquidated");

```
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L159

### Recommended Mitigation
it would be ``advisable`` to change the implementation of the function to require that the ``collateral amount`` be ``less than 50%`` of the ``deposited asset``, ``as stated in the comment``


##

## [L-2] Lack of ``check-effects-interactions (CEI) pattern`` is vulnerable to ``reentrancy attacks``

### Impact
In the function ``depositEtherToMint``, the state change of minting ``PEUSD`` is performed after the external call to the IWBETH contract. This means that an attacker could call the IWBETH contract and then call the ``_mintPeUSD`` function before the state change of minting PEUSD has been completed. This could allow the attacker to mint more PEUSD than they are entitled to, which could create a security risk

### POC

```solidity
FILE: Breadcrumbs2023-06-lybra/contracts/lybra/pools/LybraWbETHVault.sol

23: IWBETH(address(collateralAsset)).deposit{value: msg.value}(address(configurator));
24:        uint256 balance = collateralAsset.balanceOf(address(this));
25:        depositedAsset[msg.sender] += balance - preBalance;
26:
27:        if (mintAmount > 0) {
28:            _mintPeUSD(msg.sender, msg.sender, mintAmount, getAssetPrice());
29:        }

```
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/LybraWbETHVault.sol#L23-L29

```solidity
FILE: 2023-06-lybra/contracts/lybra/pools/LybraRETHVault.sol

 rkPool.deposit{value: msg.value}();
        uint256 balance = collateralAsset.balanceOf(address(this));
        depositedAsset[msg.sender] += balance - preBalance;

        if (mintAmount > 0) {
            _mintPeUSD(msg.sender, msg.sender, mintAmount, getAssetPrice());
        }

```
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/LybraRETHVault.sol#L30-L36


https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L58-L70

### Recommended Mitigation

Use check-effects-interactions (CEI) pattern 

##

##

## [L-3] User could deposit a small amount of ``ether`` and then ``mint`` a large amount of ``PEUSD``, which could create a security risk

### Impact
if a user deposits ``1 ether`` and mints ``100,000 PEUSD``, they would have a large amount of ``PEUSD`` that they could then use to buy up a large amount of ``esLBR``. This could then give them control of a large portion of the ``ProtocolRewardsPool``contract, which could allow them to manipulate the rewards distribution.

### POC

```solidity
FILE: 2023-06-lybra/contracts/lybra/pools/LybraWbETHVault.sol

function depositEtherToMint(uint256 mintAmount) external payable override {
        require(msg.value >= 1 ether, "DNL");
        uint256 preBalance = collateralAsset.balanceOf(address(this));
        IWBETH(address(collateralAsset)).deposit{value: msg.value}(address(configurator));
        uint256 balance = collateralAsset.balanceOf(address(this));
        depositedAsset[msg.sender] += balance - preBalance;

        if (mintAmount > 0) {
            _mintPeUSD(msg.sender, msg.sender, mintAmount, getAssetPrice());
        }

```
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/LybraWbETHVault.sol#L20-L32

### Recommended Mitigation
The ``depositEtherToMint()`` function could be modified to limit the amount of PEUSD that a user can mint. The function could be modified to only allow a user to ``mint`` a certain ``percentage of the amount`` of ``ether`` that they ``deposit``.

##

## [L-4] Use the ``safeAllowance()`` function instead of the normal ``approve()`` function

### Impact
- It does not check if the ``spender`` has ``already transferred`` the requested ``amount`` of tokens
- It is not as secure as the ``safeAllowance()`` function

The ``safeAllowance()`` function first checks if the spender has already been approved for the requested amount of tokens. If they have not, the function approves the spender for the requested amount of tokens and then checks if the spender has already transferred the requested amount of tokens. If they have, the function reverts. This helps to prevent malicious spenders from transferring tokens that they have not been approved to transfer.

- It is more complex than the safeAllowance() function

### POC

```solidity
FILE: 2023-06-lybra/contracts/lybra/pools/LybraWstETHVault.sol

35: lido.approve(address(collateralAsset), msg.value);

```
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/LybraWstETHVault.sol#L35C59-L35C59

### Recommended Mitigation
Use ``safeAllowance()`` for better security 

##

## [L-5] Use ``call`` instead of ``transfer`` to send ethers

 ### Impact
``.transfer`` will relay ``2300 gas`` and ``.call`` will relay all the gas. If the receive/fallback function from the recipient proxy contract has complex logic, using .transfer will fail, causing ``integration`` issues

### POC

```solidity
FILE: 2023-06-lybra/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol

139: collateralAsset.transfer(msg.sender, reducedAsset);
142: collateralAsset.transfer(provider, reducedAsset - reward2keeper);
143: collateralAsset.transfer(msg.sender, reward2keeper);

```

### Recommended Mitigation
Replace ``.transfer`` with ``.call``. Note that the result of ``.call`` need to be ``checked``

##

## [L-6] ``Timelocks`` not implemented as per documentation 

`` Does it use a timelock function?:  True``

### Impact 
- If a user accidentally transfers their tokens to the wrong address, or if a malicious actor gains access to a user's wallet, the tokens can be transferred to the attacker's address without any delay.

### Recommended Mitigation
Implement time locks as per documentations 

##

## [L-7] ``depositAssetToMint()`` does not check for a maximum limit on asset deposits

### This means that a user could theoretically deposit an unlimited amount of collateral asset into the contract. This could potentially lead to a number of problems,

- The contract could become too centralized if a single user or group of users deposits a large enough amount of collateral asset
- The contract could run out of gas if a user deposits a very large amount of collateral asset
- The contract could be used to launder money or finance illegal activities

### POC

```solidity
FILE: 2023-06-lybra/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol

function depositAssetToMint(uint256 assetAmount, uint256 mintAmount) external virtual {
        require(assetAmount >= 1 ether, "Deposit should not be less than 1 collateral asset.");
        uint256 preBalance = collateralAsset.balanceOf(address(this));
        collateralAsset.transferFrom(msg.sender, address(this), assetAmount);
        require(collateralAsset.balanceOf(address(this)) >= preBalance + assetAmount, "");

        depositedAsset[msg.sender] += assetAmount;
        if (mintAmount > 0) {
            uint256 assetPrice = getAssetPrice();
            _mintPeUSD(msg.sender, msg.sender, mintAmount, assetPrice);
        }
        emit DepositAsset(msg.sender, address(collateralAsset), assetAmount, block.timestamp);
    }


```
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L58-L70

### Recommended Mitigation
 It would be advisable to add a maximum limit on asset deposits to the ``depositAssetToMint`` function

##

## [L-8] Project Upgrade and Stop Scenario should be

At the start of the project, the system may need to be stopped or upgraded, I suggest you have a script beforehand and add it to the documentation. This can also be called an ” EMERGENCY STOP (CIRCUIT BREAKER) PATTERN “.

https://github.com/maxwoe/solidity_patterns/blob/master/security/EmergencyStop.sol

##

## [L-9] Lack of ``nonReentrant`` modifiers for critical external functions 

### Impact

Apply the nonReentrant modifier to critical external functions to ensure they are not susceptible to reentrancy attacks

 nonReentrant modifier, you can help protect your contract from reentrancy vulnerabilities. It's important to apply this modifier to functions that involve state changes or interact with external contracts, especially if they involve transfers of Ether or tokens

### POC

```solidity
FILE: Breadcrumbs2023-06-lybra/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol

82: function withdraw(address onBehalfOf, uint256 amount) external virtual {

```
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L82C77-L82C77

### Recommended Mitigation
It would be advisable to add the ``nonReentrant`` modifier

##

## [L-10] ``Divide by zero`` should be avoided 

### Impact
Division by zero should be avoided in Solidity and in any programming language. Division by zero is an undefined operation, and it can lead to unexpected and unpredictable results. In Solidity, division by zero will cause the contract to revert. This means that the transaction will be canceled, and the caller will not receive any funds or tokens

### POC

```solidity
FILE: 2023-06-lybra/contracts/lybra/miner/esLBRBoost.sol

67: return ((boostEndTime - userUpdatedAt) * maxBoost) / (time - userUpdatedAt);

```
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/esLBRBoost.sol#L67C89-L67C89

```solidity
FILE: 2023-06-lybra/contracts/lybra/pools/base/LybraEUSDVaultBase.sol

156: uint256 onBehalfOfCollateralRatio = (depositedAsset[onBehalfOf] * assetPrice * 100) / borrowed[onBehalfOf];

```
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L190

### Recommended Mitigation
- Use the SafeMath library
- Check the divisor before dividing

##

## [L-11] ``name masking`` should be avoided to prevent errors

### Impact

Name masking can be a source of errors, so it is important to be aware of the potential for name masking when writing Solidity contracts. By giving variables unique names and avoiding name masking, you can help to prevent errors in your contracts.

If a variable is declared with the same name as a function, the variable will shadow the function. This means that the variable will be used instead of the function when the name is referenced.

### POC

```solidity
FILE: 2023-06-lybra/contracts/lybra/token/PeUSD.sol

12: uint8 decimals = decimals();

```
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/PeUSD.sol#L12

```solidity
FILE: 2023-06-lybra/contracts/lybra/token/LBR.sol

20: uint8 decimals = decimals();

```
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/LBR.sol#L20

### Recommended Mitigation
Avoid using the same name for a variable and a function

##

## [L-12] ``Division`` before ``multiplication`` can lead to ``precision errors``

### Impact
Because Solidity integer division may truncate, it is often preferable to do multiplication before division to prevent precision loss.

### POC

```solidity
FILE: Breadcrumbs2023-06-lybra/contracts/lybra/miner/EUSDMiningIncentives.sol

142: borrowed = borrowed * (1e20 + peUSDExtraRatio) / 1e20;

```
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/EUSDMiningIncentives.sol#L142

### Recommended Mitigation

```solidity

142: borrowed = (borrowed * (1e20 + peUSDExtraRatio)) / 1e20;

```

##

## [L-13] ``Hardcoding addresses`` in smart contracts can make them more ``vulnerable to attack``

### Impact
- It makes the contract more difficult to audit. This is because it is more difficult to understand how the contract works if the addresses are hardcoded.

- It makes the contract more vulnerable to attack. This is because it is easier for an attacker to find a way to manipulate the values of the variables and get an incorrect result.

- It makes the contract more difficult to update. This is because it is necessary to update the hardcoded addresses if the addresses of the contracts change.

### POC
```solidity
FILE: Breadcrumbs2023-06-lybra/contracts/lybra/miner/EUSDMiningIncentives.sol

153: uint256 etherInLp = (IEUSD(0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2).balanceOf(ethlbrLpToken) * uint(etherPrice)) / 1e8;

```
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/EUSDMiningIncentives.sol#L153

### Recommended Mitigation
``Use dynamic addresses``. Dynamic addresses are addresses that are not hardcoded into the contract. Instead, they are generated at runtime and stored in a state variable. This makes it more difficult for an attacker to manipulate the values of the variables and get an incorrect result

##

## [L-14] NATSPEC comments should be increased in contracts

It is recommended that Solidity contracts are fully annotated using NatSpec for all public interfaces (everything in the ABI). It is clearly stated in the Solidity official documentation.
In complex projects such as Defi, the interpretation of all functions and their arguments and returns is important for code readability and auditability.
https://docs.soliditylang.org/en/v0.8.15/natspec-format.html

Recommendation
NatSpec comments should be increased in Contracts

##

## [L-15] ``Negative values`` may return the ``unexpected`` values when using ``uint256(int)`` conversion

### Impact
When converting a negative int256 value to a uint256 using ``uint256(int256)``, the result will be unexpected. if you try to convert a negative ``int256`` value to a ``uint256``, the result will be ``interpreted`` as the two's complement representation of the negative value. This means that the resulting uint256 value will be a ``large positive number``.

PROOF:
We attempt to convert the negative value ``-10`` to a ``uint256``. However, the resulting value will not be the expected value of ``-10`` but rather a ``large positive number``.

If you execute the function, the returned value will be ``115792089237316195423570985008687907853269984665640564039457584007913129639934``. This result corresponds to the two's complement representation of ``-10`` when ``interpreted`` as an ``unsigned uint256``.

### POC

```solidity
FILE: Breadcrumbs2023-06-lybra/contracts/lybra/miner/EUSDMiningIncentives.sol

212: (, int lbrPrice, , , ) = lbrPriceFeed.latestRoundData();
213: biddingFee = biddingFee * uint256(lbrPrice) / 1e8;

```

### Recommended Mitigation
Carefully handle ``conversions between signed and unsigned integer`` types and ensure that the values are within the ``valid ranges`` of the ``target type``.

##

## [L-16] ``Array`` does not have a ``pop function``

### Impact
Arrays without the pop operation in Solidity can lead to inefficient memory management and increase the likelihood of out-of-gas errors.

### POC

```solidity
FILE: 2023-06-lybra/contracts/lybra/miner/esLBRBoost.sol

26: esLBRLockSettings.push(esLBRLockSetting(30 days, 20 * 1e18));
27:        esLBRLockSettings.push(esLBRLockSetting(90 days, 30 * 1e18));
28:        esLBRLockSettings.push(esLBRLockSetting(180 days, 50 * 1e18));
29:        esLBRLockSettings.push(esLBRLockSetting(365 days, 100 * 1e18));

34:   esLBRLockSettings.push(setting);

```

### Recommended Mitigation
Add ``pop`` function to avoid ``out-of-gas errors``

##

## [L-17] ``Setters`` should have ``initial value check``

### Impact
Setters should have initial value check to prevent assigning wrong value to the variable. Assginment of wrong value can lead to unexpected behavior of the contract.

### POC

```solidity
FILE: Breadcrumbs2023-06-lybra/contracts/lybra/configuration/LybraConfigurator.sol

function setMintVaultMaxSupply(address pool, uint256 maxSupply) external onlyRole(DAO) {
        mintVaultMaxSupply[pool] = maxSupply;
    }


```

### Recommended Mitigation
``maxSupply`` add initial value check then assign to ``mintVaultMaxSupply[pool]``

##

## [L-18] External calls in an un-bounded for-loop may result in a DOS

### Impact
Consider limiting the number of iterations in for-loops that make external calls

### POC

```solidity
FILE: Breadcrumbs2023-06-lybra/contracts/lybra/miner/EUSDMiningIncentives.sol

for (uint i = 0; i < _pools.length; i++) {
            require(configurator.mintVault(_pools[i]), "NOT_VAULT"); //@audit external call
        }

for (uint i = 0; i < pools.length; i++) {
            ILybra pool = ILybra(pools[i]);
            uint borrowed = pool.getBorrowedOf(user); //@audit external call
            if (pool.getVaultType() == 1) {
                borrowed = borrowed * (1e20 + peUSDExtraRatio) / 1e20;
            }
            amount += borrowed;

```
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/EUSDMiningIncentives.sol#L94-L96

### Recommended Mitigation
Ensure that loops are bounded and their iterations are reasonable and predictable

##

















