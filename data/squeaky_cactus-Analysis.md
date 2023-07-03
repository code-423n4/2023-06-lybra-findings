**Any comments for the judge to contextualize your findings**
Previously I've only used Hardhat with TypeScript for test to explore potential bugs and this was my first time writing tests using Foundry.
Setting up Foundry and configuring a HardHat project to support both technologies was an interesting experience, while
Foundry does have limitations, manipulating the evm block.number and block.timestamps is much easier.

In all honesty, although I've covered all the code at the superficial level, getting up to speed with Foundry did impact
my pace and there were sections (such as liquidation) where I simply lacked the time to investigate them sufficiently.

Bing new to auditing, I'm still getting a grasp on what precisely qualifies for each of the severity categories, 
if I've put anything in way off base, feel free to correct it ...and let me know & I'll adjust my finding for next time.


**Approach taken in evaluating the codebase**
Overview, bottom up then top down.

Initially reading through the Lybra docs to get an idea of the project and the moving parts, then reading through codebase
to identify QA category of issues and mark area's of logic that either I could not make sense of, or looked questionable and
area's to test out for potential exploits.
Revising the docs and discord channel to answer to address the unknown logic and requirements, then exploratory testing, 
writing up any findings as issues, QA report and this analysis.


### Time spent:
40 hours