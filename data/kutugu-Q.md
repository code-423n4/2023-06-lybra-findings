# Findings Summary

| ID     | Title                                                                                 | Severity     |
| ------ | ------------------------------------------------------------------------------------- | ------------ |
| [L-01] | The collateralAmount and getClaimAbleLBR have an accuracy error                       | Low          |
| [L-02] | The extraRatio and rewardRatio updates should pass through the time lock              | Low          |
| [L-03] | There is arbitrage space in direct using of oracle price to mint                      | Low          |
| [L-04] | StakedLBRLpValue value can be manipulated                                             | Low          |
| [L-05] | There are accuracy problems in EUSD internal accounting calculations                  | Low          |
| [N-01] | Decimals for collateral and USD may not be equal                                      | Non-Critical |
| [N-02] | ProtocolRewardsPool getReward event parameter miscalculation                          | Non-Critical |

# [L-01] The collateralAmount and getClaimAbleLBR have an accuracy error

## Description

```solidity
uint256 collateralAmount = (((eusdAmount * 1e18) / assetPrice) * (10000 - configurator.redemptionFee())) / 10000;
```

```solidity
    function getClaimAbleLBR(address user) public view returns (uint256 amount) {
        if (time2fullRedemption[user] > lastWithdrawTime[user]) {
            amount = block.timestamp > time2fullRedemption[user] ? unstakeRatio[user] * (time2fullRedemption[user] - lastWithdrawTime[user]) : unstakeRatio[user] * (block.timestamp - lastWithdrawTime[user]);
        }
    }
```

The collateralAmount and getClaimAbleLBR have precision error in dividing before multiplying.

## Recommendations

Multiply before divide

# [L-02] The extraRatio and rewardRatio updates should pass through the time lock

## Description

The calculation of incentives in EUSDMiningIncentives relies on extraRatio and rewardRatio, whose updates depend on the owner instead of the time lock.    
If the owner updates the extraRatio and rewardRatio in an instant, such as reducing them, the user will not have time to claim the previous reward and will have less to claim later.     
But a small number of people like searchers will listen to mempool to frontrun to claim the previous reward, which is unfair.    
It should be updated with a time lock so that most users have time to claim their previous rewards.   

## Recommendations

The extraRatio and rewardRatio updates should pass through the time lock

# [L-03] There is arbitrage space in direct using of oracle price to mint

## Description

There is a [threshold limit](https://data.chain.link/ethereum/mainnet/crypto-usd/eth-usd) for the price update of the oracle. Arbitrageurs can take advantage of spot price differences.    
For `ETH/USD`, the update threshold limit is `0.5%`, this means that when the ETH oracle price is 1000, the spot price is between 995 and 1005, and there is a space for arbitrage. Arbitrageurs can use the old price to mint tokens in vault and then sell them in the spot market for arbitrage, or take the opposite path.    

## Recommendations

Add a 0.5% mint fee

# [L-04] StakedLBRLpValue value can be manipulated

## Description

StakedLBRLpValue using the balance in the uniswap lp pool to calculate the value is not a problem for internal temporary use, because even if staker temporarily manipulates the price to avoid liquidation, it would not be sustainable for long.   
However, if an external contract invokes this interface, it may cause a loss. It is better to use reserve rather than balance to calculate the lp value.

## Recommendations

Use reserve rather than balance to calculate the lp value.  

# [L-05] There are accuracy problems in EUSD internal accounting calculations

## Description

The core problem is that the `getSharesByMintedEUSD` function has an accuracy error. In EUSD, due to the presence of `burnShares`, the exchangeRate of `balance / share` will increase. Assume exchangeRate increase to 1.1.       
When a user or other contract interacts with EUSD, if the input amount is 1, it is converted to 0 shares, that is, the amount of the user account will not change, which can cause a series of problems for external contracts.      
For example, tranfer and burn trigger the transfer event, which causes the external contract to think that the transfer has been made, but the actual user amount remains unchanged.     

## Recommendations

The precision error is reduced to 0 and a zero-value event should be triggered

# [N-01] Decimals for collateral and USD may not be equal

## Description

Code do not take into account collateral and usd decimals may not equal. Although the LST used now is all 18.

## Recommendations

Add decimals convert.

# [N-02] ProtocolRewardsPool getReward event parameter miscalculation

## Description

```solidity
    } else {
        if(peUSDBalance > 0) {
            peUSD.transfer(msg.sender, peUSDBalance);
        }
        ERC20 token = ERC20(configurator.stableToken());
        uint256 tokenAmount = (reward - eUSDShare - peUSDBalance) * token.decimals() / 1e18;
        token.transfer(msg.sender, tokenAmount);
        emit ClaimReward(msg.sender, EUSD.getMintedEUSDByShares(eUSDShare), address(token), reward - eUSDShare, block.timestamp);
    }
} else {
    emit ClaimReward(msg.sender, EUSD.getMintedEUSDByShares(eUSDShare), address(0), 0, block.timestamp);
}
```

For other stable token, tokenAmount is `reward - eUSDShare - peUSDBalance`, not `reward - eUSDShare`; For EUSD, tokenAmount is `reward` not `0` and address is `address(EUSD)`.

## Recommendations

Fix event parameter
