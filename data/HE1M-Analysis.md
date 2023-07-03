 - Some parts of the code (especially the `LybraEUSDVaultBase.sol` and `LybraPeUSDVaultBase`) are copy of each other that should be carefully considered as there is big difference in their working mechanism.
 - Since the protocol variables are dynamic and strongly related to each other, having a flashloan feature can be risky. Better to have reentrancy guard especially for cross contract reentrancy attacks.


### Time spent:
40 hours