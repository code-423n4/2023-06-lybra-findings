# Issue
rkPool is not validated on change.

# Explantaion
In the [LybraRETHVault](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraRETHVault.sol#L41), `setRkPool(address)` does not validate the authenticity of the provided address. This could result in a loss of funds or a DOS attack should an incorrect contract address be provided.

# Solution
To prevent a DOS attack, a try-catch loop could be used (though this is gas-heavy). Example:
```sol
function setRkPool(address addr) external {
    require(configurator.hasRole(keccak256("TIMELOCK"), msg.sender));
    try IRkPool(addr).getBalance() returns (uint256) {
      rkPool = IRkPool(addr);
    catch (bytes memory /* data */) {
      revert();
    }
}
```
