[QA-1] redundant code at excessIncomeDistribution can be refactored

these 3 lines exist in both `if` and `else`
```solidity
            bool success = EUSD.transferFrom(msg.sender, address(configurator), income);
            require(success, "TF");

            try configurator.distributeRewards() {} catch {}
```

## Recommendation
```solidity
uint256 toTransfer = payAmount > income ? income : payAmount.
bool success = EUSD.transferFrom(msg.sender, address(configurator), toTransfer);
require(success, "TF");
try configurator.distributeRewards() {} catch {}
```



2. 