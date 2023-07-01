- Approach taken in evaluating the codebase

I have started with identifying the different workflows on the system such as the deposit/withdraw mechanism, after that I have linked each pool token to it's base pool. and then identified the entry points for the user.

I started to review the contracts at base vaults `/base/*` which give me high level overview about what vaults supposed to serve. From this points I went through the pools of each collateral assets and start to analyze the code snippets that are unique to each pool if any.

For each pool I had to confirm/check:

1. The purpose of the functions is working as excepted by analyzing the deposit, withdraw, and math calculation. 

2. Liquidation and repay is working as expected based on the intended design of each pool

In addition, I reviewed the miner contracts `/miner/*` and the different mechanism of each contract and the difference between them.


- Codebase quality analysis

Code, execution flow, and contracts linked to each other all this is very clear.

- Centralization risks

The system has high permission over the vaults which could leads to some attack vectors as explained on my issue #91



### Time spent:
30 hours