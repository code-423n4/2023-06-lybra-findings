1. mockCurve.sol

 Unused function parameter:int128 i, int128 j, uint256 dx

function get_dy_underlying(int128 i, int128 j, uint256 dx) external view returns (uint256) {
        return price;
}

 Unused function parameter: int128 i, int128 j

function exchange_underlying(int128 i, int128 j, uint256 dx, uint256 min_dy) external returns(uint256) {
        eusd.transferFrom(msg.sender, address(this), dx);
        usdc.transfer(msg.sender, min_dy);
        return min_dy;
}

3. adding public - a visibility specifier - to a constructor has no effect.

mockWstETH.sol

  constructor(IStETH _stETH)
        public
        ERC20Permit("Wrapped liquid staked Ether 2.0")
        ERC20("Wrapped liquid staked Ether 2.0", "wstETH")
    {
        stETH = _stETH;
    }

4. IesLBRBoost interface

- IesLBRBoost (contracts/lybra/miner/EUSDMiningIncentives.sol#19-25) 	
- IesLBRBoost (contracts/lybra/miner/ProtocolRewardsPool.sol#18-22) 	
- IesLBRBoost (contracts/lybra/miner/stakerewardV2pool.sol#8-14)

"Don't Repeat Yourself" (DRY)
The same interface, called "IesLBRBoost", is reused in several contracts.

You have declared the IesLBRBoost interface in these three contracts, and it is redundant. To solve this problem, you need to ensure that the interface is declared only once, and that the contracts use it from this single declaration.

5. LybraEUSDVaultBase.sol 

The IPriceFeed interface is duplicated twice in the code in the :LybraEUSDVaultBase and LybraPeUSDVaultBase contracts.

you call the function collateralAsset.transfer(onBehalfOf,withdrawal) in your code on line 107, but you don't check the return value of this function

function liquidation
same error on line 173

function superLiquidation: same error on line 208
function rigidRedemption: same error on line 242
6. LybraConfigurator.sol

You call the EUSD.approve() function in your code at line 302, but you don't check the return value of this function. Ignoring it can lead to potential problems, as you don't know whether the approval was successful.

7. ProtocolRewardsPool.sol

the ProtocolRewardsPool.getReward() function in the ProtocolRewardsPool.sol contract, specifically on lines 190 to 218, ignores the return value of the call to EUSD.transferShares(msg.sender, eUSDShare) on line 197.

The error occurs because EUSD's transferShares function returns a Boolean value, indicating whether the transfer was successful or not. In your code, you call this function, but ignore its result, which can lead to potential errors or undesirable behavior.

To resolve this error, you need to take into account the return value of the transferShares function and handle cases where the transfer fails accordingly.

8. stakerewardV2pool.sol

You call the `transfer` function of `stakingToken` to transfer `_amount` to `msg.sender`. If the `stakingToken` contract is an external contract, this could allow that contract to call the `withdraw` function before it terminates, which can lead to undesirable behavior.

Be sure to check that the `stakingToken` contract is secure and does not cause reentrance when executing the token transfer.