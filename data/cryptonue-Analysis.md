# Overview

Moonwell is an open lending and borrowing protocol built on Base, Moonbeam, and Moonriver which is derived from Benqi, a fork of Compound v2. Based on Protocol comment on Discord, this current code4rena audit contest will be specific for Base deployment.

Moonwell implement additional functionalities such as borrow caps and multi-token emissions. The v2 release of the Moonwell Protocol is an upgrade to use solidity 0.8.17, add supply caps, and a number of improvements for user experience (things like mintWithPermit and claimAllRewards).

Supply caps are essential limits placed on the total amount of a particular asset that can be minted or supplied within the protocol. These caps are designed to regulate the growth and expansion of the protocol, providing a controlled environment for participants and mitigating the risk of potential over-exposure. In Compound v2, this supply caps is one of the issue exist, thus applying the caps is consider a good approach.

The implementation of "Claim All Rewards" is aimed at enhancing user convenience by enabling users to claim all their accrued rewards in one single action. This significantly reduces the number of individual transactions required for claiming rewards, making the rewards redemption process more efficient and user-friendly.

# Audit Approach

In short:

- Research into project description, purpose, and its architecture design
- Read previous audit reports
- Read fork or similar project's audit reports
- Smart Contract manual code review and walkthrough

_I skipped using any automated audit tools, as every protocol I assume will perform this action before delivering for audit contest / review._

---

First step for an audit, is to understand what is Moonwell, how does it works, and why does exist. This is to make sure we look at the right place while auditing, by reading [docs](https://docs.moonwell.fi/) even though the v2 changes didn't included on it.

Next is to read and verify Moonwell's previous audit, and make sure any issues from it was cleared. The previous audit report from Moonwell are from Halborn

1.  [January 24th, 2022 - February 1st, 2022](https://github.com/code-423n4/2023-07-moonwell/blob/main/audits/Moonwell_Finance_Smart_Contract_Security_Audit_Report_Halborn_Final.pdf)
2.  [July 16th, 2023 - July 24th, 2023](https://github.com/code-423n4/2023-07-moonwell/blob/main/audits/Moonwell_Finance_Contracts_V2_Smart_Contract_Security_Assessment_Report_Halborn_Final.pdf)

By reading those two previous audit from Halborn, I want to make sure every issues or findings on it already covered on this current audit codebase.

Mentioning the Moonwell is a derived project from Benqi, a fork of Compound v2, it's a common path for auditor to check the following approach:

1. Check previous Benqi project audit report & analysis: https://docs.benqi.fi/resources/risks#audits
2. Check Compound v2 well-known issues:
3. Check any Compound (or Benqi) forks and read their audit reports: https://defillama.com/forks/Compound

After cross check any related forks and similar codebase compared to Moonwell's codebase, I made my notes of similar or likely findings, and then goes manually read through code. At this point, any rising issue can be noted.

# Codebase Analysis

The Audit contest of Moonwell Protocol v2 is limiting the scope to specific areas:

1. Oracle
2. Reward Distributor
3. Temporal Governor

Other in scope contracts would be the MToken & Comptroller implementation and Interest Rate Models (Jump Rate Model) which also derived from BENQI. The team protocol didn't focus on those other contracts due to it was heavily forked and a battle tested contract. But if it's in the scope, we need to check any diff and potential issue even though it's not part of the 3 specific areas.

## 1. Oracle

The Oracle Moonwell use will be from Chainlink. There are two contracts in scope for audit

- [ChainlinkCompositeOracle](https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Oracles/ChainlinkCompositeOracle.sol) : Chainlink composite oracle, combines 2 or 3 chainlink oracle feeds into a single composite price
- [ChainlinkOracle](https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Oracles/ChainlinkOracle.sol) : Stores all chainlink oracle addresses for each respective underlying asset

Since the protocol will be deployed on Base (L2 from Coinbase), which currently there is no exist Chainlink feed on it, it will be hard to test the live data. The implementation of oracle might slightly changed when later the Chainlink deployed their feed live on Base.

For example, the current Oracle code didn't mention any sequencer status, while Chainlink in L2 introduce [this](https://docs.chain.link/data-feeds/l2-sequencer-feeds).

> If a sequencer becomes unavailable, it is impossible to access read/write APIs that consumers are using and applications on the L2 network will be down for most users without interacting directly through the L1 optimistic rollup contracts

This definitely raise a concern for Moonwell on the Oracle part.

## 2. Reward Distributor

From Code4rena readme info, the `MultiRewardDistributor.sol` is in scope and need special attention. This contract is being used to allow distributing and rewarding users with multiple tokens per MToken.

This contract along with the Moonwell Comptroller, is responsible for distribution of rewards and managing the index calculations. The rewards disbursal and index calculations occur at both the global market level and the individual user level within those markets. The logic used in this contract closely resembles that of Compound, but it has been generalized to ensure that transfers do not result in immediate failures if certain elements cannot be sent out. Instead, any excess rewards are accumulated in the system's records to be sent out at a later time when feasible.

For each market within the Moonwell exists an array of configurations. Each configuration corresponds to a unique emission token. These emission tokens is critical point in the reward mechanism. The owner/admin of each emission token has the authority to adjust various parameters, including supply and borrow emissions, end times, and other settings related to the reward distribution process. This level of flexibility allows market owners to fine-tune and customize the rewards to align with their specific goals and strategies, fostering a dynamic and adaptable ecosystem within the Moonwell.

This MultiRewardDistributor contains logic that is modified and heavily inspired by Compound Flywheel.

## 3. Temporal Governor

`TemporalGovernor` is the cross chain governance contract. Specific areas of concern include delays, the pause guardian, putting the contract into a state where it cannot be updated.

This single Governor contract, https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Governance/TemporalGovernor.sol, is the contract that governs the Base deployment of moonwell leveraging the wormhole bridge as the source of truth. Wormhole will be fed in actions from the moonbeam chain and this contract will execute them on base.

Not much information on how Moonwell integrates with Wormhole, but there are a few assumptions that are made in this contract:

1. Wormhole is secure and will not send malicious messages or be deactivated.
2. Moonbeam is secure.
3. Governance on Moonbeam cannot be compromised.

\*_I did't explore too deep on this part_

# Architecture Design

Moonwell is derived from the Compound protocol, and as a result, both the MToken (cToken) and Comptroller design have undergone extensive battle testing, making them undoubtedly robust and reliable.

Even the `comments` and minor issue from Comptroller contract from Compound v2 are still there. For example:

1. Comments of **`Shh - currently unused`**
2. The `if (msg.sender != pendingAdmin || msg.sender == address(0))` which is from Compound v2, a well-known `msg.sender == address(0)` which is wrong as `msg.sender`` will never be address(0)

Thus I believe, Comptroller and MToken design are not really changed much from the Compound (or Benqi).

For the Oracle, Moonwell try to combine multiple chainlink oracle prices together allows combination of either 2 or 3 chainlink oracles. But the main concern here is currently Chainlink doesn't have any support on Base chain. Even though Chainlink is the 'de facto' on-chain Oracle, it's recommended to have a backup oracle in case of failure or issue on the new Chainlink deployment on Base.

Related with overall flow design, based on https://github.com/code-423n4/2023-07-moonwell/blob/main/CROSSCONTRACTINTERACTION.md, the interactions that occur, in the path of:

> MToken -> Comptroller -> MultiRewardDistributor -> MToken

which is quite straight forward.

Again, the core architecture design of this protocol is heavily inspired by Compound V2. Thus, I don't have any comment or issues as it's a well-known protocol.

# Centralization risks

Since Moonwell is derived from Compound v2, and add some features from BENQI, the centralization risk in the big picture is basically following the Compound protocol risk. Comptroller's design allowed the governance team to introduce changes without requiring direct approval from token holders or stakeholders. This situation led to discussions and debates about the degree of decentralization within the protocol and the implications it could have on the platform's long-term sustainability and governance model.

In the Moonwell protocol, there is a privileged admin account that plays a critical role in governing and regulating the system-wide operations. It also has the privilege to control or govern the flow of assets managed by this protocol this privileged account needs to be scrutinized.

If the privileged admin account is a plain EOA account, this may be worrisome and pose counter-party risk to the exchange users. Note that a multi-sig account could greatly alleviate this concern, though it is still far from perfect. Specifically, a better approach is to eliminate the administration key concern by transferring the role to a community-governed DAO. In the meantime, a timelock-based mechanism can also be considered as mitigation.

Recommendation is to transfer the privileged account to the intended DAO-like governance contract. All changed to privileged operations may need to be mediated with necessary timelocks. Eventually, activate the normal on-chain community-based governance life-cycle and ensure the intended trustless nature and high-quality distributed governance.

In short, centralization risk of Moonwell protocol are:

- The Unitroller admin may set a new implementation contract.
- The Comptroller admin may withdraw all Well tokens from the contract.
- The MToken admin may withdraw the contract's underlying assets acquired reserves.

Using timelock, non EOA, and/plus distributed Goverance for any changes to Moonwell is the best way to minimize this centralization issue.

If centralization risks also cover centralization issue for overall architecture of Moonwell, we can also mention these risks:

1. Oracle is using Chainlink which is deployed on Base: It's recommended to have a backup oracle.
2. Base chain is a product of Coinbase: I'm not really sure if this Base chain is similar to BNB chain which is a 'centralized blockchain', if so, then deploying Moonwell might be having this centralization chain.

# Notes & Additional Information

## Oracle implementation on Base chain (L2)

As of now, Chainlink's live data feeds are not available on Base L2. However, there have been reports of testnet deployments. Given that the Base chain is relatively new and lacks a fully deployed Chainlink instance (not just the testnet), the data feeds may not be deemed entirely stable. To enhance the protocol's reliability, it is recommended to incorporate a secondary backup on-chain Oracle.

Although this approach might introduce some redundancy, having a secondary oracle will provide an additional layer of robustness to the protocol. This ensures that the protocol remains robust and resilient, even in situations where the primary Chainlink data feeds experience temporary instability.

## Markets can’t be unlisted

The current implementation lacks the ability to deactivate the `isListed` flag for markets, though some markets might become deprecated and remain in the `allMarkets` storage variable. Additionally, the code frequently utilizes for loops around the `allMarkets` array, preventing effective unlisting or removal of markets from the array.

This will leads to increased gas costs for regular operations as all markets in the array are processed independently, even if some are deprecated or no longer needed. To address this issue and enhance overall gas efficiency and storage utilization, it is suggested to initiate a discussion on potential code refactoring.

_\*I Understand that Moonwell audit contest on Code4rena didn't have any Gas Optimization allocation ($0), but I just want to share some opinion on it_


### Time spent:
24 hours