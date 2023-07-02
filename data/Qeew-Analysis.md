
1. Codebase Analysis

The current codebase represents an upgrade (V2) of the existing V1 Contracts. Its primary objective is to allow users to deposit various Liquidity Stake Tokens (LSD) like wBETH, rETH, etc., in order to mint the stablecoin, PeUSD. This is a significant upgrade from the V1 system, which allowed minting of eUSD exclusively with a single LSD token, stETH. Additionally, V2 has been designed to permit the use of PeUSD on L2 chains.

2. Architecture Evaluation

The system architecture appears to be meticulously planned and concise. The codebase is neatly structured and well-formatted, contributing to the overall simplicity of the system.

3. Centralization Risks

The current system exposes the codebase to centralization risks by utilizing a single EOA as the exclusive contract owner. It is recommended that this issue be addressed by incorporating a multi-signature setup or role-based authorization to mitigate such risks.

4. Systemic Risks

The contract inherently depends on functions provided by LSD tokens by implementing their respective interfaces. However, it's been observed that some LSD tokens like rETH and wBETH have their interfaces incorrectly implemented, presenting a potential systemic risk.

5. Other Recommendations

As a user of the V1 contract, I empathize with the community's anticipation for V2's timely release. However, the current codebase gives an impression of hastiness on the part of the developers, leading to simple, avoidable errors. Thus, it is recommended that the team ensures sufficient time is allocated to work on V2 to prevent rushed execution and related errors.

6. Time Spent

The auditing of the codebase took over 72 hours.

### Time spent:
72 hours