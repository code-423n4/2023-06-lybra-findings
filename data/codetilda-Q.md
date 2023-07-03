# NC-001 Inconsistency between the comment and the code in the definition of maxSupply of esLBR token

[esLBR.sol#L6](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/esLBR.sol#L6)
    
    * - The maximum amount that can be minted through the esLBRMinter contract is 55 million.

[esLBR.sol#L20](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/esLBR.sol#L20)

    uint256 maxSupply = 100_000_000 * 1e18;

