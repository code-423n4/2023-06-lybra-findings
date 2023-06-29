[N-01]No need to use SafeMath library in solidity ^0.8.17;
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/EUSD.sol#L38

Dev use SafeMath in solidity version ^0.8.17, after 0.8, solidity has included overflow and underflow check, no need to use SafeMath anymore
