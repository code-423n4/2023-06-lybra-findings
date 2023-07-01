## [L-01] DO NOT PERFORM `ARITHMETIC` OPERATION INSIDE THE emit OF AN event

It is not recommended to perform arithmetic operations inside the emit of an event. The emits are placed to log the function results for the use of the off-chain tools and not to perform arithmetic operations. The arithmetic opeations should be performed outside the emit and only the result should be logged and emitted in an event for the use of off-chain tools.

```solidity
File: lybra/pools/LybraWbETHVault.sol

/// @audit   balance - preBalance don't perfrom arithmetic
31        emit DepositEther(msg.sender, address(collateralAsset), msg.value,balance - preBalance, block.timestamp);

```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWbETHVault.sol#L31

```solidity
File: lybra/pools/LybraRETHVault.sol

/// @audit   balance - preBalance don't perfrom arithmetic
38        emit DepositEther(msg.sender, address(collateralAsset), msg.value,balance - preBalance, block.timestamp);

```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraRETHVault.sol#L38

```solidity
File: lybra/miner/ProtocolRewardsPool.sol

/// @audit  reward - eUSDShare
203                    emit ClaimReward(msg.sender, EUSD.getMintedEUSDByShares(eUSDShare), address(peUSD), reward - eUSDShare, block.timestamp);

```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L203

## [L-02] CONDUCT THE `MULTIPLICATION` BEFORE `DIVISON` WHEN PERFORMING ARITHMETIC OPERATIONS TO AVOID ROUNDING ERROR.

When arithmetic operations are performed in solidity, it is recommended to perform the multiplicatons before division. This is because the division results are rounded down to the neares interger value in solidity. which introduces rounding error. And if the multiplication is followed, it will inflate the rounding error to a much larger error which could be detrimental to the functionality of the protocol.

```solidity
File: lybra/pools/LybraStETHVault.sol

104        return 10000 - (time / 30 minutes - 1) * 100;

```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L104

## [L-03] In the `LybraWbETHVault::getAssetPrice` function there is potential inaccurate asset price calculation

The getAssetPrice function calculates the asset price based on the ether price and the exchange ratio of the collateral asset. However, if the underlying functions `_etherPrice` or `exchangeRatio` are not properly implemented , it could lead to inaccurate price calculations, potentially impacting the overall functionality.

```solidity
File:  lybra/pools/LybraWbETHVault.sol

34    function getAssetPrice() public override returns (uint256) {
35        return (_etherPrice() * IWBETH(address(collateralAsset)).exchangeRatio()) / 1e18;
36    }
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWbETHVault.sol#L34C1-L36C6

## [L-04] There is no access control in `esLBR::_transfer` Function

the function reverts with a generic "not authorized" message, but it does not enforce any specific access control mechanism. This can potentially allow unauthorized parties to trigger the revert and disrupt the contract's functionality.

```solidity
File: lybra/token/esLBR.sol

26    function _transfer(address, address, uint256) internal virtual override {
27        revert("not authorized");
28    }

```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/esLBR.sol#L26

## [L-05] The `rkPool` variable is initialized twice in the constructor of the LybraRETHVault contract, which leads to redundant initialization

The LybraRETHVault contract initializes the rkPool variable twice in the constructor, which leads to redundant initialization and potential confusion. The variable is first initialized as a state variable and then reassigned using the provided `_rkPool` parameter.

```solidity
File: lybra/pools/LybraRETHVault.sol

18    IRkPool rkPool = IRkPool(0xDD3f50F8A6CafbE9b31a427582963f465E745AF8);
19
20    // rkPool = 0xDD3f50F8A6CafbE9b31a427582963f465E745AF8
21    // rETH = 0xae78736Cd615f374D3085123A210448E74Fc6393
22    constructor(address _peusd, address _config, address _rETH, address _oracle, address _rkPool)
23        LybraPeUSDVaultBase(_peusd, _oracle, _rETH, _config) {
24            rkPool = IRkPool(_rkPool);
25        }
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraRETHVault.sol

## [L-06] `esLBRBoost::setLockStatus` it's good to validated id parameter without validation an attacker could provide an invalid or out-of-range index as input

This absence of input validation can lead to unexpected behavior or potential errors if an invalid or out-of-range index is provided as input. Attackers could exploit this vulnerability to manipulate the lock settings or disrupt the contract's functionality.

```solidity
File: lybra/miner/esLBRBoost.sol


38    function setLockStatus(uint256 id) external {
39:        esLBRLockSetting memory _setting = esLBRLockSettings[id];//@audit id param lack of input valid
40       LockStatus memory userStatus = userLockStatus[msg.sender];
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol#L38C1-L40C67
