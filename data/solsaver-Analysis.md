# Analysis Report
The codebase was decent sized, and it took me the whole duration to get a hang of the contract and find possible issues. Missing tests did pose some testing challenges and slowdown, but I was able to write up some tests from scratch to validate the possible issues.

## Audit Process
* I made a note of all the contracts in scope.
* Took a first pass through half of the contracts.
* Read through the documentation https://docs.lybra.finance/lybra-v2-technical-beta/overview/the-lybra-v2-smart-contracts
* Resumed the audit, and went through the whole codebase.
* As I went through the contracts, I left notes for myself.
* Towards the end I started to feel burn out, so took one day off.
* I wanted to take a second pass, but I was out of time.
* Started to go over the notes, removing the invalid ones, and preparing reports with the valid ones. Also had written some tests for medium issues.

## Quality of the Codebase
* The codebase was well organized, had good variable names, comments and documentation references where ever applicable. That did make the audit process better.
* The methods were short, and had mostly consistent pattern on how to handle errors, define methods, and perform various checks.
* Like all the other wardens, I missed having the tests in place

## Centralization Risks
There were several possible centralization as pointed out in the automated findings: https://gist.github.com/liveactionllama/27513952718ec3cbcf9de0fda7fef49c#m04-the-owner-is-a-single-point-of-failure-and-a-centralization-risk

## Time spent on the audit

Since the start date, I have spent around 3-4 hours everyday days on this codebase after my work.
I spent time on this codebase everyday, so almost clocking in 30-35 hours on this. I might have spent additional 5 hours on the documentation.
I had to spent a day to figure out how to write the tests in hardhat, but that wasnt too bad.

### Time spent:
40 hours