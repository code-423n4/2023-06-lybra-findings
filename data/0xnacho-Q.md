# [L-01]: LybraWBETHVault: Misleading WBETH address specified in the comments

The WBETH address specified in the [LybraWBETHVault comments](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWbETHVault.sol#L16-L17) is misleading. The displayed address is actually the `rETH` mainnet address (https://etherscan.io/token/0xae78736Cd615f374D3085123A210448E74Fc6393).

# [L-02]: LybraRETHVault: Initial value of `rkPool` variable is not valid on all the chains

The address which is used to initialize the `rkPool` variable is only valid on `mainnet`, but it doesn't correspond to a RocketPool Deposit pool on `Arbitrum` or other L2s.

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraRETHVault.sol#L18

```
IRkPool rkPool = IRkPool(0xDD3f50F8A6CafbE9b31a427582963f465E745AF8);
```

Therefore, removing the `rkPool` initialization is highly recommended.

# [L-03]: LybraPeUSDVaultBase#liquidation: redundant allowance check

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L131

```
require(PeUSD.allowance(provider, address(this)) > 0, "provider should authorize to provide liquidation EUSD");
```

1. This is checked for ulterior `PeUSD.transferFrom` (https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L197-L204) in the `_repay` function.
2. The `transferFrom` method doesn't check for allowance if the vault is already a mint vault.
3. `LybraPeUSDVaultBase` is a mint vault, so the allowance is not checked

Given these, the allowance check is redundant.

# [L-04]: LybraPeUSDVaultBase: emitted `WithdrawAsset` - wrong params order

Declaration:

```
event WithdrawAsset(address sponsor, address indexed onBehalfOf, address asset, uint256 amount, uint256 timestamp);
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L219

```
  emit WithdrawAsset(
    _provider,
    address(collateralAsset),  --|
                                    switched params
    _onBehalfOf,               --|
    _amount,
    block.timestamp
);
```

The correct emit statement:

```
emit WithdrawAsset(
    _provider,
    address(collateralAsset),
    _onBehalfOf,
    _amount,
    block.timestamp)
```

---

# [NC-01]: LybraWBETHVault conditionates the deposit ETH amount to be higher than WBETH accepts

The `WBETH#deposit` function allows users to deposit any ETH amount higher than 0:

```
/**
     * @dev Function to deposit eth to the contract for wBETH
     * @param referral The referral address
     */
    function deposit(address referral) external payable {
        require(msg.value > 0, "zero ETH amount");

        // msg.value and exchangeRate are all scaled by 1e18
        uint256 wBETHUnit = 10 ** uint256(decimals);
        uint256 wBETHAmount = msg.value.mul(wBETHUnit).div(exchangeRate());

        _mint(msg.sender, wBETHAmount);

        emit DepositEth(msg.sender, msg.value, wBETHAmount, referral);
    }
```

However, `LybraWBETHVault#depositEtherToMint` [doesn't allow ETH amount smaller than 1 ether](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWbETHVault.sol#L21), which doesn't serve users that want to deposit a smaller ETH amount even if there is WBETH support for doing this.

# [NC-02]: Unclear/misleading error messages accorss the codebase

**Examples:**

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L62

```
require(
            collateralAsset.balanceOf(address(this)) >=
                preBalance + assetAmount,
            ""
        );
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L83-L84

```
require(onBehalfOf != address(0), "TZA");
        require(amount > 0, "ZA");
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L174

```
require(poolTotalPeUSDCirculation + _mintAmount <= configurator.mintVaultMaxSupply(address(this)), "ESL");
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraRETHVault.sol#L28

```
require(msg.value >= 1 ether, "DNL");
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L76

```
require(success, "TF");
```

# [NC-03]: Misleading comments containing addresses with regards to the deployment scope of Lybra vaults

There are multiple comments in the vaults code containing addresses to be used when deploying each smart contract. Although these addresses are helpful when deploying the vaults on the mainnet, they are misleading for the deployments on other L2s.
This happens because of the addresses not matching their L2s equivalent addresses.

**Examples:**

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraRETHVault.sol#L20-L21

```
 // rkPool = 0xDD3f50F8A6CafbE9b31a427582963f465E745AF8
 // rETH = 0xae78736Cd615f374D3085123A210448E74Fc6393
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L16-L17

```
// stETH = 0xae7ab96520DE3A18E5e111B5EaAb095312D7fE84
// oracle = 0x4c517D4e2C851CA76d7eC94B805269Df0f2201De
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWbETHVault.sol#L16

```
  //WBETH = 0xae78736Cd615f374D3085123A210448E74Fc6393
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWstETHVault.sol#L23-L24

```
  //WstETH = 0x7f39C581F595B53c5cb19bD0b3f8dA6c935E2Ca0;
  //Lido = 0xae7ab96520DE3A18E5e111B5EaAb095312D7fE84;
```

**Recommendation:** remove the comments containing the smart contract addresses. You can also keep a separate json file that would contain the addresses of each dependency, batched by chain. This could be used in the deployment and deployment verification processes.

Example of json:

```
{
  [mainnet_chain_id]: {
    "rkPool": "0xDD3f50F8A6CafbE9b31a427582963f465E745AF8",
    "rETH": "0xae78736Cd615f374D3085123A210448E74Fc6393",
     "stETH": "0xae7ab96520DE3A18E5e111B5EaAb095312D7fE84",
     "ethPriceOracle": "0x4c517D4e2C851CA76d7eC94B805269Df0f2201De",

     ...
  },
  [arbitrum_chain_id]: {
    "rkPool": "<rkPool arbitrum address>",
    "rETH": "<rETH arbitrum address>",
     "stETH": "<stETH arbitrum address>",
     "ethPriceOracle": "<eth price oracle address>",
     ...
  },
  ...
}
```

# [NC-04]: `FeeDistribution` event is not used in LybraPeUSDVaultBase

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L35

```
event FeeDistribution(
        address indexed feeAddress,
        uint256 feeAmount,
        uint256 timestamp
    );
```

# [NC-05]: There are multiple places (coments, variable names) in the non-rebase vaults where `eUSD` is mentioned, but `peUSD` should be mentioned

**Examples:**

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L32

```
  event LiquidationRecord(
        address provider,
        address keeper,
        address indexed onBehalfOf,
        uint256 eusdamount,              ----> should be peUSD amount
        uint256 LiquidateAssetAmount,    ----> should start with lower case letter
        uint256 keeperReward,
        bool superLiquidation,
        uint256 timestamp
    );
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L222-L224

```
    /**
     * @dev Get USD value of current collateral asset and minted EUSD through price oracle / Collateral asset USD value must higher than safe Collateral Ratio.
     */
```
