### Issues Template

|Letter |Name|Description|
|--|-------|-------|
| NC |  Non-critical | Non risky findings |
| R  | Refactor | Changing the code |

| Total Found Issues | 4 |
|:--:|:--:|

### Non-Critical Template

| Count | Explanation | Instances |
|:--:|:-------|:--:|
| [L-01] | Missing checks | 2 |

### Refactor Template

| Count | Explanation | Instances |
|:--:|:-------|:--:|
| [R-01] | Documentation Mismatch | 1 |
| [R-02] | Misleading comments | 2 |


### [L-01] Missing checks
- According to the comment, the `amount` should be higher than 0. But there is no check to ensure this
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L137
```solidity
    /**
     * @notice Burn the amount of EUSD and payback the amount of minted EUSD
     * Emits a `Burn` event.
     * Requirements:
     * - `onBehalfOf` cannot be the zero address.
     * - `amount` Must be higher than 0.
     * @dev Calling the internal`_repay`function.
     */
    function burn(address onBehalfOf, uint256 amount) external {
        require(onBehalfOf != address(0), "BURN_TO_THE_ZERO_ADDRESS");
        require(amount>0,"ZERO_AMOUNT"); //Add this check
        _repay(msg.sender, onBehalfOf, amount);
    }
```
- In `LybraEUSDVaultBase.sol`, the check to ensure that "individual mint amount shouldn't surpass 10% when the circulation reaches 10_000_000" is missing.
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L118-L130
```solidity

    /**
     * @notice The mint amount number of EUSD is minted to the address
     * Emits a `Mint` event.
     *
     * Requirements:
     * - `onBehalfOf` cannot be the zero address.
     * - `amount` Must be higher than 0. Individual mint amount shouldn't surpass 10% when the circulation reaches 10_000_000
     */
     
    function mint(address onBehalfOf, uint256 amount) external {
        require(onBehalfOf != address(0), "MINT_TO_THE_ZERO_ADDRESS");
        require(amount > 0, "ZERO_MINT");
        _mintEUSD(msg.sender, onBehalfOf, amount, getAssetPrice());
    }
```

### [R-01] Documentation Mismatch
- As per the code, the comments must mention that: *"`assetAmount` Must be higher than 1."*
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L69
```solidity
	/*
     * Requirements:
     * - `assetAmount` Must be higher than 0.
     * - `mintAmount` Send 0 if doesn't mint EUSD
     */
    function depositAssetToMint(uint256 assetAmount, uint256 mintAmount) external virtual {
        require(assetAmount >= 1 ether, "Deposit should not be less than 1 stETH.");

        bool success = collateralAsset.transferFrom(msg.sender, address(this), assetAmount);
        require(success, "TF");
```


### [R-02] Misleading Comments
- In `EUSD.sol` , the comment *"the contract must not be paused"*, is used for following functions.
- Recommend removing them as there is no pausing functionality
```solidity
function transfer
function approve
function transferFrom
function increaseAllowance
function decreaseAllowance
function transferShares
function _approve
function _transferShares
function mint
function burn
function burnShares
```
- In `EUSD.sol`, The example has mentioned wrong amount of tokens
```solidity
/**
 * @title Interest-bearing ERC20-like token for Lybra protocol.
 *
 * EUSD balances are dynamic and represent the holder's share in the total amount
 * of Ether controlled by the protocol. Account shares aren't normalized, so the
 * contract also stores the sum of all shares to calculate each account's token balance
 * which equals to:
 *
 *   shares[account] * totalSupply / _totalShares
 *
 * For example, assume that we have:
 *
 *   _getTotalMintedEUSD() -> 1000 EUSD
 *   sharesOf(user1) -> 100
 *   sharesOf(user2) -> 400
 *
 * Therefore:
 *
 *   balanceOf(user1) -> 2 tokens which corresponds 200 EUSD  <- Should be 200 instead of 2
 *   balanceOf(user2) -> 8 tokens which corresponds 800 EUSD  <- Should be 800 instead of 8
 *
 * Since balances of all token holders change when the amount of total shares
 * changes, this token cannot fully implement ERC20 standard: it only emits `Transfer`
 * events upon explicit transfer between holders. In contrast, when total amount of
 * pooled Ether increases, no `Transfer` events are generated: doing so would require
 * emitting an event for each token holder and thus running an unbounded loop.
 */
```