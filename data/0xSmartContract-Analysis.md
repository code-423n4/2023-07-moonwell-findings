# Analysis  - Moonwell  Project 
### Summary
| List |Head |Details|
|:--|:----------------|:------|
|a) |The approach I followed when reviewing the code | Stages in my code review and analysis |
|b) |Analysis of the code base | What is unique? How are the existing patterns used? |
|c) |Test analysis | Test scope of the project and quality of tests |
|d) |Architectural | Architecture feedback |
|e) |Documents  | What is the scope and quality of documentation for Users and Administrators? |
|f) |Centralization risks | How was the risk of centralization handled in the project, what could be alternatives? |
|g) |Systemic risks | Potential systemic risks in the project |
|h) |Competition analysis| What are similar projects? |
|i) |Security Approach of the Project | Audit approach of the Project |
|j) |Other Audit Reports and Automated Findings | What are the previous Audit reports and their analysis |
|k) |Gas Optimization | Gas usage approach of the project and alternative solutions to it |
|l) |New insights and learning from this audit | Things learned from the project |


## a) The approach I followed when reviewing the code

First, by examining the scope of the code, I determined my code review and analysis strategy.
https://github.com/code-423n4/2023-07-moonwell#scope

Accordingly, I analyzed and audited the subject in the following steps;

| Number |Stage |Details|Information|
|:--|:----------------|:------|:------|
|1|Compile and Run Test|[Installation](https://github.com/code-423n4/2023-07-moonwell#running--development)|Test and installation structure is simple, cleanly designed|
|2|Architecture Review| [Moonwell Protocol](https://docs.moonwell.fi/moonwell/introduction/about-moonwell) |Provides a basic architectural teaching for General Architecture|
|3|Graphical Analysis  |Graphical Analysis with [Solidity-metrics](https://github.com/ConsenSys/solidity-metrics)|A visual view has been made to dominate the general structure of the codes of the project.|
|4|Slither Analysis  | [Slither](https://github.com/crytic/slither)| |
|5|Test Suits|[Tests](https://github.com/code-423n4/2023-07-moonwell/tree/main/test)|In this section, the scope and content of the tests of the project are analyzed.|
|6|Manuel Code Review|[Scope](https://github.com/code-423n4/2023-07-moonwell#scope)||
|7|Infographic|[Figma](https://www.figma.com/)|I made Visual drawings to understand the hard-to-understand mechanisms|
|8|Special focus on Areas of  Concern|[Areas of Concern](https://github.com/code-423n4/2023-07-moonwell#moonwell-protocol-v2)||

## b) Analysis of the code base
The main most important contracts of the project;


####  MultiRewardDistributor.sol:

`MultiRewardDistributor`is that manages the distribution of rewards to users of the Moonwell protocol. The contract tracks the supply and borrows of mTokens, which are tokens that represent a user's share of a particular market. The contract then uses this information to calculate the user's rewards and distribute them to the user's address.

#### Comptroller.sol

`Comptroller.sol`  is a contract that manages the operations of the Moonwell protocol. The contract tracks the supply and borrows of mTokens, which are tokens that represent a user's share of a particular market. The contract also manages the minting and redeeming of mTokens, as well as the distribution of rewards to users.

#### TemporalGovernor.sol
`TemporalGovernor.sol` is a contract that governs the Base deployment of moonwell leveraging the wormhole bridge as the source of truth. Wormhole will be fed in actions from the moonbeam chain and this contract will execute them on base.


#### Unitroller.sol
`Unitroller.sol` is a contract that manages the operations of the Comptroller contract. The Comptroller contract is a core contract in the Compound protocol, and it manages the supply and borrows of cTokens, which are tokens that represent a user's share of a particular market.


The _setPendingImplementation function allows the current admin of the contract to set a new pending implementation. The new pending implementation will become the active implementation once it calls the _acceptImplementation function.

The _acceptImplementation function allows the pending implementation to accept its role as the active implementation. Once this function is called, the new implementation will be able to perform all of the actions that the Comptroller contract can perform.

The _setPendingAdmin function allows the current admin of the contract to set a new pending admin. The new pending admin will become the active admin once it calls the _acceptAdmin function.

The _acceptAdmin function allows the pending admin to accept its role as the active admin. Once this function is called, the new admin will be able to perform all of the actions that the Unitroller contract can perform.

The fallback function is the default function that is called when the Unitroller contract receives a message. This function simply delegates the execution of the message to the current implementation of the Comptroller contract.

## c) Test analysis

What did the project do differently? ;
There are quality and detailed Unit and Integration tests in the project.

What could they have done better?;

It seems that the scope of the test has 80%. Test coverage must be 100%, remember that the weakest part of the project in terms of security may be the remaining 20%.

When designing the tests, I recommend modeling the project with the Threat Model, writing the tests accordingly, and documenting these threat models, this way the test coverage will be more understandable.

## d) Architectural 


<img width="1518" alt="image" src="https://github.com/code-423n4/2023-07-moonwell/assets/104318932/390abb5b-1bc5-4013-ba03-0fbc98082156">





## e) Documents 
Documentation should be increased further, it is recommended to add the architectural design to the documents as infographic


##  f) Centralization risks 
There is a risk of centrality in the project as the onlyOwner
The owner role has a single point of failure and onlyOwner can use critical functions, posing a centralization issue. There is always a chance for owner keys to be stolen, and in such a case, the attacker can cause serious damage to the project due to important functions.

The code detail of this topic is specified in automatic finding;
https://gist.github.com/liveactionllama/0a27b77de2aa56abd2e215c82a39f86d#m04-the-owner-is-a-single-point-of-failure-and-a-centralization-risk

##  g) Systemic risks 
The most important system risk in the project; Risks arising from the use of Oracle, the project uses Chainlink, oracles, security exploits from oracles have been increasing recently, so using Oracle is a risk in itself and it is important to minimize this risk.


## h) Competition analysis
The Moonwell Protocol is a fork of Benqi, which is a fork of Compound v2 with things like borrow caps and multi-token emissions.

BENQI Liquid Staking is a liquid staking protocol built on Avalanche.

It tokenizes staked AVAX and allows users to freely use it within Decentralized Finance dApps such as automated market makers (AMMs), lending & borrowing protocols, yield aggregators, etc.


## i) Security Approach of the Project

Successful current security understanding of the project;
1- The first audit was conducted in a reputable audit firm such as Halborn and all security concerns were resolved, this is the case in both audit reports;

https://github.com/code-423n4/2023-07-moonwell/blob/main/audits/Moonwell_Finance_Smart_Contract_Security_Audit_Report_Halborn_Final.pdf

https://github.com/code-423n4/2023-07-moonwell/blob/main/audits/Moonwell_Finance_Contracts_V2_Smart_Contract_Security_Assessment_Report_Halborn_Final.pdf

2- They manage the 2nd audit process with an innovative audit such as Code4rena, in which many auditors examine the codes.



What the project should add in the understanding of Security;
1- By distributing the project to testnets, ensuring that the audits are carried out in onchain audit. (This will increase coverage)
2- After the project is published on the mainnet, there should be emergency action plans (not found in the documents)
3- After the inspection, the concept of "continuous inspection" should be adhered to with bug bounty programs such as ImmuneFi.


## j) Other Audit Reports and Automated Findings 

**Automated Findings:**
https://gist.github.com/liveactionllama/0a27b77de2aa56abd2e215c82a39f86d

Especially Medium detections in the Automated Finding Report should be taken into account;


### Medium Risk Issues

| |Issue|Instances|
|-|:-|:-:|
| [[M&#x2011;01](#m01-unsafe-use-of-transfertransferfrom-with-ierc20)] | Unsafe use of `transfer()`/`transferFrom()` with `IERC20` | 2 | 
| [[M&#x2011;02](#m02-insufficient-oracle-validation)] | Insufficient oracle validation | 2 | 
| [[M&#x2011;03](#m03-missing-checks-for-whether-the-l2-sequencer-is-active)] | Missing checks for whether the L2 Sequencer is active | 2 | 
| [[M&#x2011;04](#m04-the-owner-is-a-single-point-of-failure-and-a-centralization-risk)] | The `owner` is a single point of failure and a centralization risk | 2 | 
| [[M&#x2011;05](#m05-some-tokens-may-revert-when-zero-value-transfers-are-made)] | Some tokens may revert when zero value transfers are made | 2 | 

**Audit Reports :**
[Halborn Audit v1](https://github.com/code-423n4/2023-07-moonwell/blob/main/audits/Moonwell_Finance_Smart_Contract_Security_Audit_Report_Halborn_Final.pdf)

[Halborn Audit v2](https://github.com/code-423n4/2023-07-moonwell/blob/main/audits/Moonwell_Finance_Contracts_V2_Smart_Contract_Security_Assessment_Report_Halborn_Final.pdf)

## k) Gas Optimization
The project is generally efficient in terms of gas optimizations, many generally accepted gas optimizations have been implemented, gas optimizations with minor effects are already mentioned in automatic finding, but gas optimizations will not be a priority considering code readability and code base size




## l) New insights and learning from this audit 

- Provided a detailed look at Compound v2 with features like borrow caps and multi-token emissions systems

- The project is a fork of BENQI Liquid Staking, in this context, the general codes of the BENQI project were examined.

- Allowed me to get a broad overview of the Comptroller architecture

### Time spent:
10 hours