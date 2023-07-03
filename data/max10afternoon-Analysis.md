Due to some time constraint, I was only able to spend 5 of the 10 days available on this audit.
Therefor I've exclusively focused on the main vault's functionalities, ignoring the governance capabilities of the protocol. Actually I ended up focusing almost exclusively on the withdraw mechanisms (of all the vaults, with the relative internal calls) and on the redistribution/rebasing of the stETH vault. This enabled me to find what I believe could be four interesting findings, despite of the time limitations (due to the same time constraint I've not included a scripted PoC for one out of the four findings, since I believe that the logic behind it was simple enough not to require one, hopefully this won't cause any trouble in judging that reported issue). As a beginner this audit gave me the opportunity to deepen my knowledge of the inner workings of a stable coin.
All the analysis was done manually.

### Time spent:
25 hours