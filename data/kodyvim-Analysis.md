# Moonwell Analysis Report

Moonwell is an open lending and borrowing DeFi protocol built on Base, Moonbeam, and Moonriver. It allows users to leverage their assets through the lending and borrowing process, providing them with an intuitive and seamless experience. The protocol operates on Moonbeam & Moonriver and utilizes mToken, a compound fork, in comparison to cToken. Users can earn rewards by participating in the protocol passively, either by lending mTokens or borrowing them.

## Architecture Recommendation:

2.1 Token Rescue Limitation: To ensure protocol solvency, Moonwell should restrict the withdrawal of tokens that are whitelisted as collateral. Allowing this could lead to potential insolvency risks.

2.2 Accurate Reward Calculation: External View functions should accurately return rewards to facilitate third-party integration. This involves accruing interest before returning data to enhance transparency and usability.

2.3 Chainlink Best Practices: Moonwell must strictly adhere to all Chainlink best practices to avoid using stale prices. Given the highly volatile nature of the crypto market, relying on outdated prices can be dangerous.

## Centralization Risks:

3.1 Admin and Governance Control: The presence of an admin controlled by governance raises concerns. This entity can access the withdrawal and rescue of stranded tokens, potentially affecting the protocol's decentralization.

3.2 Asset Price Setting: The owner's ability to set underlying asset prices as a fallback for Chainlink unavailability poses a centralization risk. Transparent price feeds should be prioritized.

3.3 Protocol Pausing: While the ability to pause the protocol serves as a safety measure, it can impact users' ability to borrow, lend, and initiate liquidations. Striking a balance between safety and protocol availability is essential for maintaining a permissionless nature.

## Time Spent:
The audit process for Moonwell took a total of 3 days, with the following breakdown:

1st Day: Understanding protocol documentation.
2nd Day: Focusing on linking documentation to logic and typing reports for identified vulnerabilities, including a review of various Chainlink contracts.
3rd Day: Summarizing the audit by completing a QA report and final analysis.

### Time spent:
60 hours