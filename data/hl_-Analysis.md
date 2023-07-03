## Table of Contents

1. Protocol Summary
2. Audit Methodology
3. Codebase Analysis
4. Architecture Comments
5. Closing Comments

### 1. Protocol Summary

Lybra Finance introduces an innovative way to provide interest-bearing stablecoins, powered by LSD income from a user's deposited collateral (ETH/stETH/rETH). 

The protocol segregates 2 types of LSDs: 
- Rebasing (number of tokens increase over time)
- Non-Rebasing (value of tokens increase over time)

Users can deposit their LSD or ETH into the protocol. In the case of ETH, the protocol will perform the convesion to a LSD. When users deposit their LSD into the protocol, they are able to borrow the protocol's stablecoins if the relevant collateral ratio is met. There are various collateral ratios, which governs the amounts of stablecoins that can be borrowed, and when a user can be liquidated: 
- min collateral ratio
- safe collateral ratio
- recommended collateral ratio

The governance token (LBR) can be staked for esLBR to earn profits over time.

### 2. Audit Methodology
1. Understand background and business logic of protocol - study docs/whitepaper/general information 
2. Review codebase to understand flow of functions
3. Review codebase to check for deviations against best practices 
4. Deep-dive into codebase to check for errors and possible exploitation loopholes (function flow/accounting/decimals/formulas/etc.), drawing up hypothetical scenarios for testing where relevant

### 3. Codebase Analysis

In my review, more focus was placed on LSD and vaults. 

With a general understanding of the business logic and careful examination of the code, the functions written are understandable. Nonetheless, more NatSpec would be appreciated to clearly define the intents of each function so that more time can be spent on value added work like error review/ review for attack vectors, instead of figuring out the intent.

### 4. Architecture Comments

- It was noted that the error messages to the `require` functions (an example appended [here](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/LybraStETHVault.sol#L38)) are not readable. While the purpose of the checks can be understood to those who review the code, it is recommended to put in error messages that can also be understood by end users, who are ultimately the clients/beneficiaries of the protocol so as to ease their troubleshoot and rectification of the errors faced. This serves to enhance the user experience for them - which is important to retain them as clients of the protocol. 

- Another point noted was that the min. deposit amount required is [1 ether](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/LybraStETHVault.sol#L38). While there could be a good reason for this min amount to be set (which I am not aware of), I would like to highlight that this may become a barrier of entry for users to use this protocol, and even more so as the price of ETH increases in the future. Perhaps the protocol team could look into a way to reduce this barrier so that the protocol can be accessible by everyone. 

- As Lybra Finance interacts with external protocols, there will be systemic risks. In the implementation of fail-safe mechanisms to mitigate such risks, careful consideration should be done to weigh the advantages of decentralization over security. 

### 5. Closing Comments

Reviewing this protocol has been a good experience - not just on the audit of the codebase, but in learning the business logic and process flows of the protocol. It is always exciting to see fresh innovations coming up in this space, and at the same time assuring to see how the protocol teams place security as a priority - by setting aside time and resources for audits like this. 

### Time spent:
20 hours