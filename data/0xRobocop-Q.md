The following criteria are used for suggested severity:

- L - Low Severity -> Some risk

- NC - Non-Critical / Informational -> Minor suggestions

# L-01 Normal liquidations at the EUSD and PeUSD vaults do not take into account providers preferences over their funds.

The `liquidation` function in the EUSD and PeUSD vault contracts only validates that the provider has granted an allowance to the vault greater than zero, which may impact the liquidation provider's preferences since they cannot specify how much eUSD they are willing to spend to participate as liquidations providers. This behavior is also inconsistent with super liquidations, which validate that the provider has approved at least the amount of eUSD that will be repaid with their funds.

# L-02 setvaultMintPaused at the configurator cannot be called by the TIMELOCK

The comments above `setvaultMintPaused` says:

```solidity
@dev This function can only be called by accounts with ADMIN or higher privilege.
```

The function is implemented as:

```solidity
function setvaultMintPaused(address pool, bool isActive) external checkRole(ADMIN) {
    vaultMintPaused[pool] = isActive;
}
```

The `checkRole` modifier is implemented as:

```solidity
modifier checkRole(bytes32 role) {
   GovernanceTimelock.checkRole(role, msg.sender);
   _;
}
```

The `checkRole` at the governance timelock is implemented as:

```solidity
function checkRole(bytes32 role, address _sender) public view  returns(bool){
        return hasRole(role, _sender) || hasRole(DAO, _sender);
}
```

It means that only admin or dao can call it, althoug timelock is a role greater than admin.


# L-03 getClaimableUSD view function at the ProtocolRewardsPool contract returns an incorrect value.

The function is coded as follows:

```solidity
function getClaimAbleUSD(address user) external view returns (uint256 amount) {
   amount = IEUSD(configurator.getEUSDAddress()).getMintedEUSDByShares(earned(user));
}
```

The function treats all the tokens a user earns as EUSD shares which is false since some rewards are PeUSD and USDC tokens. Because it is expected that 1 EUSD share > 1 EUSD token, then this function may return a greater value than it should. The incorrect returned value may cause integration issues with 3rd party applications.
