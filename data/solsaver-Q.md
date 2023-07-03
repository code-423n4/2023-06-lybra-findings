## The used interface can be `IPeUSD`

The interface used is `IEUSD`, but it can be `IPeUSD`.

```
55:    IEUSD public peUSD;
```

Link: https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L55

## `tokenMinerChanges` should be renamed to `TokenMinerChanges` to be consistent with the naming convention of other event


```
70:    event tokenMinerChanges(address indexed pool, bool status);
```
Link: https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L70

## Incorrect documentation - `initToken` can actually be called twice

The documentation states that the following function can only be executed once, but in reality it can be called twice if we set one address to non-zero address at a time, like `initToken(0, 0x123); initToken(0x123, 0);`

```
95:    /**
96:     * @notice Initializes the eUSD and peUSD address. This function can only be executed once.
97:     */
98:    function initToken(address _eusd, address _peusd) external onlyRole(DAO) {
99:        if (address(EUSD) == address(0)) EUSD = IEUSD(_eusd);
100:       if (address(peUSD) == address(0)) peUSD = IEUSD(_peusd);
101:   }
```

Link: https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L98-L101

## The used interface can be `IPeUSD`

The interface used is `IEUSD`, but it can be `IPeUSD`.

```
100:       if (address(peUSD) == address(0)) peUSD = IEUSD(_peusd);
```

Link: https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L100

## Documentation: "`assetAmount` Must be higher than 0." is actually implemented as >= 1 eth

The documentation states that the `assetAmount` should be higher than 0, but the actual check in the code is that the amount should be greater than 1 ether.

```
73:        require(assetAmount >= 1 ether, "Deposit should not be less than 1 stETH.");
```

Link: https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L73

```
59: require(assetAmount >= 1 ether, "Deposit should not be less than 1 collateral asset.");
```

Link: https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L59

## Missing check to ensure `amount` is higher than 0

The documentation states that `amount` should be higher than 0, but there are no checks in place for it.

```
132:    /**
133:     * @notice Burn the amount of EUSD and payback the amount of minted EUSD
134:     * Emits a `Burn` event.
135:     * Requirements:
136:     * - `onBehalfOf` cannot be the zero address.
137:     * - `amount` Must be higher than 0.
138:     * @dev Calling the internal`_repay`function.
139:     */
140:    function burn(address onBehalfOf, uint256 amount) external {
141:        require(onBehalfOf != address(0), "BURN_TO_THE_ZERO_ADDRESS");
142:        _repay(msg.sender, onBehalfOf, amount);
143:    }
```

Link: https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L132-L143

## Missing error message

`require` statements should always include some relevant error messages if the requirements fails ot get satisfied

```
62:    require(collateralAsset.balanceOf(address(this)) >= preBalance + assetAmount, "");
```

Link: https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L62

## Misleading modifier names

`MintPaused` should be `MintNotPaused`

```
83:   modifier MintPaused() {
84:        require(!configurator.vaultMintPaused(msg.sender), "MPP");
85:        _;
86:    }
```

Link: https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L83-L86

`BurnPaused` should be `BurnNotPaused`
```
87:    modifier BurnPaused() {
88:        require(!configurator.vaultBurnPaused(msg.sender), "BPP");
89:        _;
90:    }
```

Link: https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L87-L90

## Broken documentation link

The links should be updated to use:
```
https://github.com/OpenZeppelin/openzeppelin-contracts/blob/cb4bf950df5ae43356c4935b3900446f6dc20261/contracts/token/ERC20/IERC20.sol#L62
or
https://github.com/ethereum/EIPs/blob/master/EIPS/eip-20.md#approve
```

```
240:    * https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/IERC20.sol#L42
```

Link: https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L240

```
259:    * https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/IERC20.sol#L42
```

Link: https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L259

## Documentation: Mentioned `_sharesAmount` but cannot find it anywhere in the code

The documentation states that "Creates `_sharesAmount` shares", but this is not found in the code.

```
402:    /**
403:     * @notice Creates `_sharesAmount` shares and assigns them to `_recipient`, increasing the total amount of shares.
404:     * @dev This operation also increases the total supply of tokens.
405:     *
406:     * Requirements:
407:     *
408:     * - `_recipient` cannot be the zero address.
409:     * - the contract must not be paused.
410:     */
411:    function mint(address _recipient, uint256 _mintAmount) external onlyMintVault MintPaused returns (uint256 newTotalShares) {
412:        require(_recipient != address(0), "MINT_TO_THE_ZERO_ADDRESS");
413:
414:        uint256 sharesAmount = getSharesByMintedEUSD(_mintAmount);
415:        if (sharesAmount == 0) {
416:            //EUSD totalSupply is 0: assume that shares correspond to EUSD 1-to-1
417:            sharesAmount = _mintAmount;
418:        }
419:
420:        newTotalShares = _totalShares.add(sharesAmount);
421:        _totalShares = newTotalShares;
422:
423:        shares[_recipient] = shares[_recipient].add(sharesAmount);
424:
425:        _totalSupply += _mintAmount;
426:
427:        emit Transfer(address(0), _recipient, _mintAmount);
428:    }
```

Link: https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L402-L428