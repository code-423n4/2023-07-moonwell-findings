# 1. Approach taken in evaluating the codebase
- Sorted the contracts by lines of code and went through them, starting with the contracts having the lowest lines of code.
- Starting with small contracts allowed me to mentally warm up and get some first context before diving into bigger contracts.
- Did a line-by-line review of each contract and left audit markers for each interesting finding (issue candidates, QA findings, etc.) to report later.
- When approaching a contract, I read the documentation .md file to get context if available.
- Checked the bigger contracts, like Comptroller.sol, that were in audit scope by downloading the sources of the forked contracts (from Compound and Benqi) and did a diff using VSCode to see the parts that would need my attention.
- Watched the walkthrough videos that were available on YouTube for additional context.
- Did a deep-dive on MultiRewardDistributor.sol by copying the file and removing all logic that I completely understood and did not find issues in to leave only the parts containing accounting logic. Allowed me to focus better. I could still jump back to the original file to occasionally look up pieces I had removed.
- Once in a while, when I was losing focus, I collected the findings and reported them to have a constant flow of reporting findings. I learned from previous contests that, in the end, time could be tight, so I made sure to now wait with reports until the final day.

# 2. Architecture recommendations
- The approach with the MErc20Delegator.sol and MErc20Delegate.sol felt a bit odd. I have done a few audits already, and this kind of setup, where I contract basically repeats all the functions of the other and delegates them, I have not seen before. There potentially is a more elegant approach to this. To better judge this, I would have needed to dive deeper into the proxy/upgradeable setup of the protocol, which I did not focus on.

- The Chainlink Oracle implementation does not follow Compound's expected way of error handling (returning code 0 instead of reverting in some cases). Since the project is heavily based on Compound logic, I would suggest conforming to Compound standards. I checked all the places where the Oracle is used, and in every case, the 0 return code was handled correctly with an Error.PRICE_ERROR.

- The approach to map the symbol of an underlying token for a market (mToken) I would suggest reviewing it. It is not guaranteed by EIP-20 standards that a token returns a symbol, and it is also possible that a token's symbol will arbitrarily change, which would break the mapping/lose the oracle until it is fixed. I encourage you to rethink this design decision and replace it with an address-to-address mapping (token address -> Chainlink price feed address).

- I would suggest documenting what an "index" is in the context of the accounting math and why you changed from the Compound way of accounting to using "emissions per second". Please also extend the very critical MultiRewardDistributor.Sol contract with a lot more inline comments. This will allow an auditor to quicker grasp the accounting logic and spend more time on breaking things than on contract comprehension. In MToken.sol, this is way better. Generally, it would be very beneficial if the accounting logic was laid out in writing (e.g., in a whitepaper).

- Having an admin controlled overwrite for retrieving a price via the Chainlink Oracle and making the admin price the default case was discovered the first time within Moonbeam. It was not seen in previous audits, and nearly every protocol used a Chainlink oracle. Please rethink if you cannot invert this to use the Chainlink oracle first and fall back to the price set by admin.

# 3. Codebase quality analysis
- Generally, the codebase feels quite mature
- There is some commented-out logic in critical places (e.g., calculateSupplyRewardsForUser(), calculateBorrowRewardsForUser() of MultiRewardDistributor.sol) that need to be cleaned up. There is a risk they make it into the final code by accident.
- More inline comments in critical contracts (especially MultiRewardDistributor.sol) are missing
- Unit test coverage of 80% is a bit on the lower end and needs improvement
- Fuzz testing is used, which is positive
- Types are used a bit inconsistently (e.g., uint vs. uint256). Setting some standards and enforcing them (through static analysis, pre-commit hooks) would be great.
- Some leftovers of copy-pasting (e.g., comments of setter functions in getter functions that don't alter state variables) can be found and should be cleaned up
- In some places, checks for the 0 address are done as a second check within a require statement, but the zero address can never happen as the caller can never be the zero address. These things give a bit of the impression that the conditions were not thought through completely, which diminishes the feeling of a mature codebase a bit.
- There are no signs of static analysis tools being used for the protocol (e.g., slither). This should be done. Also, the codebase should be more rigorously be checked for typos. Both the audit contest botrace as well as the manual review of the codebase found typos.

# 4. Insights & Learnings
- In this audit it was attempted to break the TemporalGovernor.sol by moving mTokens between calls of claimReward(address holder, MToken[] memory mTokens) in Comtroller.sol. The idea was that the amount of rewards granted to an account is influenced by mToken.balanceOf(_supplier). The "testDifferentNumOfSuppliers" from MultiRewardDistributor.t.sol was modified to attempt a proof of concept. "forge-std/console.sol" was used to log the rewards being distributed, but the results did not show a sign this could be exploited. It looks like the custom "transferToken" implementation in mToken.sol makes sure that the rewards are distributed before moving the tokens to the new address.

# 5. Centralization risks
- The protocol is not completely permissionless, which imposes centralization risk
- Critical contracts either have an admin and/or additional parties granted critical rights (governor, guardian)
- It is important to ensure these parties are incentivized to behave in the users'/protocol's favor (game theory)
- It is important to not use individual accounts but multi sigs with a high enough threshold/quorum


### Time spent:
32 hours