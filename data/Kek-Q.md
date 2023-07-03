

# [L-01] WBETH address is incorrect

WBETH Address @ [LybraWbETHVault.sol#L16](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/LybraWbETHVault.sol#L16) is incorrect.

[etherscan | 0xae78736Cd615f374D3085123A210448E74Fc6393](https://etherscan.io/address/0xae78736Cd615f374D3085123A210448E74Fc6393) is an reth address.

I believe [0xa2E3356610840701BDf5611a53974510Ae27E2e1](https://etherscan.io/token/0xa2E3356610840701BDf5611a53974510Ae27E2e1) should be the correct address, however, the functions it implements don't line up with the LybraWbETHVault contract as implemented.

This has no impact in the code base as it is a comment, however, I wanted to point this out in the event this value is copy/pasted later by the developers; which may result in some major issues such as a broken deployment. 