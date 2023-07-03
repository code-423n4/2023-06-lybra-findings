## Approach
1. Read Scope description of each contract on the C4 page to get a general overview on the architecture of the protocol. 
2. After that, I delved into the contract code, starting from non-rebase vaults to rebase vaults i.e. LybraPeUSDVaultBase.sol and its child contracts to LybraEUSDVaultBase.sol and its child contracts.
3. The remaining contracts fall into 4 categories: Timelock contracts, Governance contracts, Reward contracts and Token contracts.
4. The LybraConfigurator.sol contract is a standalone contract, which is controlled by the DAO. This contract sets the various parameters and control functionalities of the Lybra Protocol.	

This was the order in which I audited the contracts. In places where I could not understand a certain functionality, I dmed the sponsors and referred to the documentation to seek additional context. 

## Architecture Recommendations
The protocol is designed in an easy to understand way. There are certain contracts that can improve the way they are designed. For example, in one of my findings, I've reported three (LBR, PeUSD, PeUSDMainnetStableVision) token contracts that could directly inherit from the LayerZero OFTV2.sol contract instead of copy pasting its functions (keeping the endpoints simple through which cross-chain communication occurs is recommended). Another one where a setter function was not implemented for upgradeable token contracts in specific LST vaults. These minute architecture recommendations have been included in the QA report I submitted. Personally, architecture-wise I would rate it on a Medium level.

### What is unique in the architecture?
The use of the LayerZero OFT contracts, which enable cross chain communication is unique. I did not delve deeper on the OFT contracts side but just checked the return values and some of the internal functions utilized.

## Codebase quality analysis
With the absence of NatSpec comments and a fully complete documentation, it was difficult to understand the assumptions being made on the developer side. For example, how stake, unstake and withdraw mechanism would work. The errors related to staking and unstaking have been submitted as HMs. Other than that, there were several low issues and non-critical ones I've included. These include the inconsistent patterns used in the codebase such as inconsistent event emissions, using unchecked in decreaseAllowance but not in increaseAllowance and the require checks being used in depositAssetAmount() being different in both LybraPeUSDVaultBase.sol and LybraEUSDVaultBase.sol contracts. Overall, from my point of view the codebase was not ready for an audit due to the presence of barely any tests, absence of in-depth documentation and typo errors in external function calls. 

## Centralization risks
Some degree of centralization is introduced through the Governance contracts but the presence of timelock contracts on them tilt it towards some degree of decentralisation. Mainly the access control exists in the LybraConfigurator, which I think does not pose much risk. I think this is a common pattern used in most protocols. On the other hand, the owner in the reward contracts poses a centralization risk since it has control over most functions which could destabilize the system. Additionally not using a two-step change for critical addresses like the owner can lead to the contract ownership being transferred to an unwanted address.

## Systemic risks
From the issues I've found the only systemic risks existing are those in the ProtocolRewardsPool.sol contract and PeUSDMainnetStableVision.sol. In the former, one can tamper with the stake/unstake functionality while in the latter the system is at risk due to the absence of rate limiting when borrowing a flash loan. There are several forms of DOS existing in the contract logic itself, which prevent users from utilizing a service in the protocol (included in findings). In addition to that, the centralization risks above pose a systemic risk as well.

## Mechanical Review
Here is a mechanical mindmap to understand the architecture and some interactions of the protocol:
https://drive.google.com/file/d/1EwQTqNQWdRx1zAMkVbvmGg1ySGncJwRe/view?usp=sharing
I have provided edit access to draw on top of the image if necessary. 

## New insights and learnings from this audit
1. This was my first time auditing a LSD-based vault. Auditing this protocol has helped me in understanding how LSDs work on a deeper level. For example, business logic for excess income distribution, redemption time for staking, and how token boosts work.
2. Auditing without tests is a bit difficult. Learning how to write tests in foundry through Patrick Collins' YT is something I am focusing on right now after this audit is over. It surely puts you one step ahead when writing POCs and finding bugs.
3. There are some OZ libraries in this protocol that I had not delved into before. Understanding how they work while auditing the protocol provided some insight on how it fits in.
4. Lastly, this is the first time I've seen LayerZero contracts being used in a protocol. Understanding the endpoints from which cross-chain communication occurs in this project is something new I learned from an architectural standpoint.


### Time spent:
70 hours