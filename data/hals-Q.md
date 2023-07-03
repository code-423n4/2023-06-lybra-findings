# Findings Summary

| ID              | Title                                                                       | Severity     |
| --------------- | --------------------------------------------------------------------------- | ------------ |
| [L-01](#l-01)   | `isOtherEarningsClaimable(address user)`: division by zero is not prevented | Low          |
| [L-02](#l-02)   | Different pools might use different `esLBRBoost` contract                   | Low          |
| [L-03](#l-03)   | `ProtocolRewardsPool` contract: any user can free mint 3 esLBR tokens       | Low          |
| [L-04](#l-04)   | LBR & esLBR tokens addresses might be changed in different contracts        | Low          |
| [NC-01](#nc-01) | Boolean is compared to a boolean                                            | Non Critical |
| [NC-02](#nc-02) | `_checkHealth` function visibility must be public                           | Non Critical |
| [NC-03](#nc-03) | Wrong error message                                                         | Non Critical |

# Low

## [L-01] `isOtherEarningsClaimable(address user)`: division by zero is not prevented <a id="l-01" ></a>

## Details

In `EUSDMiningIncentives.sol`/`isOtherEarningsClaimable(address user)` function : the result of stakedOf(user) could be zero if he doesn't have any borrowed amount in any pool.

## Impact

This will cause run-time error when invoking `isOtherEarningsClaimable(address user)` or `getReward()` functions with an address with zero borrowed amounts.

## Proof of Concept

```solidity
File: 2023-06-lybra/contracts/lybra/miner/EUSDMiningIncentives.sol
Line 188-190:
   function isOtherEarningsClaimable(address user) public view returns (bool) {
        return (stakedLBRLpValue(user) * 10000) / stakedOf(user) < 500;
    }
```

```solidity
File: 2023-06-lybra/contracts/lybra/miner/EUSDMiningIncentives.sol
Line 193:
    require(!isOtherEarningsClaimable(msg.sender), "Insufficient DLP, unable to claim rewards");
```

```solidity
File: 2023-06-lybra/contracts/lybra/miner/EUSDMiningIncentives.sol
Line 136-147:
 function stakedOf(address user) public view returns (uint256) {
        uint256 amount;
        for (uint i = 0; i < pools.length; i++) {
            ILybra pool = ILybra(pools[i]);
            uint borrowed = pool.getBorrowedOf(user);
            if (pool.getVaultType() == 1) {
                borrowed = borrowed * (1e20 + peUSDExtraRatio) / 1e20;
            }
            amount += borrowed;
        }
        return amount; //this amount will be zero if the user has no debts in any pool
    }
```

## Tools Used

Manual Testing.

## Recommended Mitigation Steps

- In `isOtherEarningsClaimable(address user)` function:
  check if `stakedOf(user)` is zero before doing the division:

```solidity
   function isOtherEarningsClaimable(address user) public view returns (bool) {
    uint256 userDebts= stakedOf(user);
        return userDebts==0 ? false : (stakedLBRLpValue(user) * 10000) / userDebts < 500;
    }
```

#

## [L-02] Different pools might use different `esLBRBoost` contract <a id="l-02" ></a>

## Details

-In `EUSDMiningIncentives.sol`, `ProtocolRewardsPool.sol` & ` stakerewardV2pool.sol` contracts:
each of the aforementioned contracts has a `setBoost` function that changes the address of `esLBRBoost`; **while `esLBRBoost` is the same contract that is going to be used across all pools** ; then changing the address in one might un-sync with the others.

## Impact

Changing the address of `esLBRBoost` in one contract might un-sync with the others and causing problems.

## Proof of Concept

Instances: 3

- In `EUSDMiningIncentives.sol` :

```solidity
File: 2023-06-lybra/contracts/lybra/miner/EUSDMiningIncentives.sol
Line 115-117:
  function setBoost(address _boost) external onlyOwner {
        esLBRBoost = IesLBRBoost(_boost);
    }
```

- In `ProtocolRewardsPool.sol` :

```solidity
File: 2023-06-lybra/contracts/lybra/miner/ProtocolRewardsPool.sol
Line 52-56:
    function setTokenAddress(address _eslbr, address _lbr, address _boost) external onlyOwner {
        esLBR = IesLBR(_eslbr);
        LBR = IesLBR(_lbr);
        esLBRBoost = IesLBRBoost(_boost);
    }
```

- In `stakerewardV2pool.sol` :

```solidity
File: 2023-06-lybra/contracts/lybra/miner/stakerewardV2pool.sol
Line 127-129:
   function setBoost(address _boost) external onlyOwner {
        esLBRBoost = IesLBRBoost(_boost);
    }
```

## Tools Used

Manual Testing.

## Recommended Mitigation Steps

Allow setting the `esLBRBoost` address in the `LybraConfigurator` contract only; so other pools can only interact with this global state variable without each pool being able to set it.

#

## [L-03] `ProtocolRewardsPool` contract: any user can free mint 3 esLBR tokens <a id="l-03" ></a>

## Details

- In `grabEsLBR(uint256 amount)` function: users can free mint 3 esLBR tokens if grabEsLBR(3) since there's no check on the amount of LBR tokens being burnt from the user .
- In this line of code `LBR.burn(msg.sender, (amount * grabFeeRatio) / 10000);`:
  1. the value of `grabFeeRatio` is set to `3000` (can be updated by the owner to a value <= 8000).
  2. so if the user send an amount of 3: then the `LBR.burn(msg.sender,3*3000/10000)` which is burning zero LBR tokens.
  3. `grabableAmount` will be updated to reflect burning this **dust** amount of tokens;while these tokens are actually not burnt and still in circulation.
  4. also the totalSupply of esLBR tokens will be increased by this **dust** amount.

## Impact

- This **dust** amount of the freely minted esLBR token will cause imbalances of LBR tokens being in circulation
- Users can use this amount to stake;thus burning more LBR tokens.

## Proof of Concept

Instances: 1

```solidity
File: 2023-06-lybra/contracts/lybra/miner/ProtocolRewardsPool.sol
Line 134: LBR.burn(msg.sender, (amount * grabFeeRatio) / 10000);
```

## Tools Used

Manual Testing.

## Recommended Mitigation Steps

In `grabEsLBR(uint256 amount)` function : Check that the burned LBR tokens > 0.

#

## [L-04] LBR & esLBR tokens addresses might be changed in different contracts <a id="l-04" ></a>

## Details

- LBR & esLBR tokens addresses can be set (infinite times):
  1. In `EUSDMiningIncentives.sol` contract by the owner.
  2. In `ProtocolRewardsPool.sol` contract by the owner.
- Alos it was spotted that different contracts access the address of these tokens separately instead of extracting/importing the address from one central contract (`LybraConfigurator`)

## Impact

The tokens addresses might un-sync with other contracts which will cause chaos of calculations where the returned values of token.balanceOf(address) might be incorrect since the user has been minted esLBR from an address and the balance is retreived from other contract address.

## Proof of Concept

Instances: 2

- In `EUSDMiningIncentives.sol`:

```solidity
File:2023-06-lybra/contracts/lybra/miner/EUSDMiningIncentives.sol
Line 84-87:
   function setToken(address _lbr, address _eslbr) external onlyOwner {
        LBR = _lbr;
        esLBR = _eslbr;
    }
```

- In `ProtocolRewardsPool.sol`:

```solidity
File:2023-06-lybra/contracts/lybra/miner/ProtocolRewardsPool.sol
Line 52-56:
     function setTokenAddress(address _eslbr, address _lbr, address _boost) external onlyOwner {
        esLBR = IesLBR(_eslbr);
        LBR = IesLBR(_lbr);
        esLBRBoost = IesLBRBoost(_boost);
    }
```

## Tools Used

Manual Testing.

## Recommended Mitigation Steps

Allow setting the LBR & esLBR tokens addresses in the `LybraConfigurator` contract only; so other contracts can only interact with these global state variables without any contract being able to set their addresses.

#

# Non Critical

## [NC-01] Boolean is compared to a boolean <a id="nc-01" ></a>

## Proof of Concept

Instances: 1

```solidity
File:2023-06-lybra/contracts/lybra/governance/LybraGovernance.sol
Line 82: require(receipt.hasVoted == false, "GovernorBravo::castVoteInternal: voter already voted");
```

## Tools Used

Manual Testing.

## Recommended Mitigation Steps

require(!receipt.hasVoted, "GovernorBravo::castVoteInternal: voter already voted")

#

## [NC-02] `_checkHealth` function visibility must be public<a id="nc-02" ></a>

## Details

`_checkHealth` function visibility must be public to enable liquidators from tracking & checking positions and liquidate the unhealthy spotted positions.

## Proof of Concept

Instances: 2

```solidity
File:2023-06-lybra/contracts/lybra/pools/base/LybraEUSDVaultBase.sol
Line 291: function _checkHealth(address _user, uint256 _assetPrice) internal view {
```

```solidity
File:2023-06-lybra/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol
Line 225: function _checkHealth(address user, uint256 price) internal view {
```

## Tools Used

Manual Testing.

## Recommended Mitigation Steps

Change `_checkHealth` function visibility to public.

#

## [NC-03] Wrong error message<a id="nc-03" ></a>

## Details

In `LybraPeUSDVaultBase.sol`: wrong error message when checking for peUSD allowance of the providerto the pool:
"provider should authorize to provide liquidation EUSD"  
while it should be:  
"provider should authorize to provide liquidation PEUSD"

## Proof of Concept

Instances: 1

```solidity
File:2023-06-lybra/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol
Line 131: require(PeUSD.allowance(provider, address(this)) > 0, "provider should authorize to provide liquidation EUSD");
```

## Tools Used

Manual Testing.

## Recommended Mitigation Steps

Modify error message to : "provider should authorize to provide liquidation PEUSD".
