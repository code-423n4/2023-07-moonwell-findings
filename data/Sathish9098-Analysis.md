# Analysis - Moonwell Protocol 
# Summary

| List |Head |Details|
|:--|:----------------|:------|
|1 | Overview of Moonwell Protocol| overview of the key components and features of Moonwell  |
|2 | My Thoughts | My own thoughts about future of the this protocol |
|3 |Audit approach | Process and steps i followed  |
|4 |Learnings | Learnings from this protocol|
|5 |Possible Systemic Risks | The possible systemic risks based on my analysis |
|6 |Code Commentary | Suggestions for existing code base |
|7 | Centralization risks | Concerns associated with centralized systems |
|8 |Gas Optimizations | Details about my gas optimizations findings and gas savings  |
|9 |Risks as per Analysis | Possible risks |
|10 |Non-functional aspects | General suggestions |
|11 |Time spent on analysis  | The Over all time spend for this reports |

## Overview

``Moonwell Protocol`` is a fork of ``Compound v2`` with features.

Moonwell is an open lending and borrowing protocol built on ``Base``, ``Moonbeam``, and ``Moonriver``.The protocol's intuitive design ensures a seamless user experience, enabling users to easily navigate through various features and perform operations effortlessly.

#### Important Contracts: 
  - ``ChainlinkCompositeOracle``
    - which aggregates mulitple exchange rates together
  - ``MultiRewardDistributor``
    - Allow distributing and rewarding users with multiple tokens per MToken. Parts of this system that require special attention are what happens when hooks fail in the Comptroller
 - ``Comptroller``
   - This contract is based on the Flywheel logic
 - ``TemporalGovernor ``
   - which is the cross chain governance contract. Specific areas of concern include delays, the pause guardian, putting the contract into a state where it cannot be updated

#### Special features of Moonwell Protocol 

  - Non-Custodial
  - User Experience
  - Security 
  - Transparency
  - Onchain Governance

### My Thoughts
Moonwell Protocol may changes ``Lending and Borrowing `` to next level 

Moonwell's approach to lending and borrowing in DeFi can be a catalyst for positive changes in the ecosystem. By setting a standard for user experience, security, transparency, and governance, it may inspire other projects to improve and innovate, leading to a more mature, inclusive, and trustworthy DeFi lending and borrowing landscape in the future.

Decentralization and Governance: Moonwell's onchain governance model can pave the way for more decentralized decision-making processes in other DeFi projects. As protocols adopt similar governance mechanisms, the community's voice and participation may play a more significant role in shaping the future of DeFi platforms.

Trust and Transparency: The emphasis on transparency in Moonwell's onchain operations can foster trust among users. As other protocols adopt similar transparency practices, users may feel more confident in interacting with DeFi platforms, leading to increased trust within the ecosystem.

Non-Custodial Solutions: Moonwell's non-custodial approach to lending and borrowing may become a trend in the DeFi space. More protocols might adopt non-custodial models to allow users to retain control over their assets, aligning with the core principles of DeFi.

Global Reach: Moonwell's focus on accessibility and usability may lead to increased global adoption of DeFi lending and borrowing. As the protocol reaches users in different regions, DeFi's impact could become more widespread and inclusive.

## Audit approach

I followed below steps while analyzing and auditing the code base.

1. Read the contest Readme.md and took the required notes.

  - Moonwell Protocol protocol uses 
   - inheritance
   - Tests only covered 80%
   - Multi-Chain
   - ERC-20 Token
   - Timelock function
   - Need to understand moonbeam and temporal governance to understand the governance system 
   - Uses Chainlink price oracle 
   - This protocol if fork of Compound with MRD
    

2. Analyzed the over all codebase one iterations very fast

3. Study of documentation to understand each contract purposes, its functionality, how it is connected with other contracts, etc.

4. Then i read old audits and already known findings. Then go through the bot races findings 

5. Then setup my testing environment things. Run the tests to checks all test passed. I used foundry to test moonwell protocol. I used forge comments for testing 

6. Finally, I started with the auditing the code base in depth way I started understanding line by line code and took the necessary notes to ask some questions to sponsors.

## Stages of audit

- ``The first stage of the audit``

During the initial stage of the audit for Moonwell Protocol, the primary focus was on analyzing gas usage and quality assurance (QA) aspects. This phase of the audit aimed to ensure the efficiency of gas consumption and verify the robustness of the platform.

Found [14 QA Findings](https://code4rena.com/contests/2023-07-moonwell/submit?issue=115)

- ``The second stage of the audit``

In the second stage of the audit for Moonwell Protocol, the focus shifted towards understanding the protocol usage in more detail. This involved identifying and analyzing the important contracts and functions within the system. By examining these key components, the audit aimed to gain a comprehensive understanding of the protocol's functionality and potential risks. 

- ``The third stage of the audit``

During the third stage of the audit for Moonwell Protocol, the focus was on thoroughly examining and marking any doubtful or vulnerable areas within the protocol. This stage involved conducting comprehensive vulnerability assessments and identifying potential weaknesses in the system. Found ``60-70`` ``vulnerable`` and ``weakness`` code parts all marked with ``@audit tags``.

- ``The fourth stage of the audit``

During the fourth stage of the audit for Moonwell Protocol, a comprehensive analysis and testing of the previously identified doubtful and vulnerable areas were conducted. This stage involved diving deeper into these areas, performing in-depth examinations, and subjecting them to rigorous testing, including fuzzing with various inputs. Finally concluded findings after all research's and tests. Then i reported C4 with proper formats 

Found one medium risk findings [``admin`` may receive less token than expected because of this check  ``_amount == type(uint256).max``](https://code4rena.com/contests/2023-07-moonwell/submit?issue=119)


## Learnings
 The Moonwell Protocol team can draw lessons from other DeFi protocols that have faced similar challenges. Studying security incidents, best practices, and successful governance mechanisms from other projects can provide valuable insights for building a more robust and resilient platform.

By prioritizing security, conducting thorough risk assessments, and actively involving the community in governance decisions, the Moonwell Protocol can proactively address these areas of concern and strengthen its position as a secure and community-driven DeFi lending and borrowing platform

## Possible Systemic Risks

- ``Smart Contract Vulnerabilities``: The use of inheritance and integration of multiple features can introduce potential smart contract vulnerabilities. Bugs, logic flaws, or improper implementations could lead to exploits and financial losses.

- ``Test Coverage Risks``:  With only 80% test coverage, the protocol may have undiscovered vulnerabilities. Inadequate security audits could leave critical issues unnoticed, increasing the risk of attacks

- ``Governance Challenges``: Understanding Moonbeam and Temporal Governance systems may be complex for some users, potentially leading to governance inefficiencies or suboptimal decision-making.

- ``Chainlink Oracle Risks``: Relying on a single oracle provider, like Chainlink, exposes the protocol to potential risks associated with oracle failures, data manipulation, or centralization issues.

- ``Protocol Fork Risks``: Forking from other protocols may inherit vulnerabilities present in the original codebase. Failure to address these risks could result in potential attacks.

- ``Liquidity and Market Risks``: Insufficient liquidity or market manipulation risks can affect the protocol's stability and overall performance.


## Code Commentary

- Use consistant ``SPDX-License`` for all contracts 
 
- Inconsistance ``solidity versions`` used 

- Ensure that variable and function names follow ``consistent naming conventions`` for better ``readability`` and ``maintainability``. For example, use camelCase or snake_case consistently throughout the contract

- In the ``Status`` and ``DistributionStatus`` structs, consider using an ``enum type`` for stage to improve readability and avoid potential bugs caused by using numbers directly

- Some functions like ``updateMarketSupplyIndexAndDisburseSupplierRewards`` and ``updateMarketBorrowIndexAndDisburseBorrowerRewards`` are similar. You can ``consolidate them into a single function`` that takes an additional parameter to specify the action (supplier rewards or borrower rewards)

- Instead of importing the ``whole`` contract code for ``IERC20`` and ``MToken``, use interfaces to define only the ``required functions``. This reduces the gas cost and enhances code readability

-  Add    inline comments    to explain complex logic, especially in mathematical calculations, to make the code more understandable

-  Add appropriate    error messages     in require statements to provide more ``informative feedback`` to users when a transaction fails

-  In some functions like ``exitMarket``, multiple emit statements are used for the ``same event``. ``Consolidate`` the event emissions to a ``single location`` to reduce ``code duplication`` and improve ``readability``.

- It's a good practice to add more detailed information in the ``event logs``, such as the account addresses involved in certain actions.

-  Some functions return error codes ``(e.g., uint(Error.NO_ERROR))``, but the exact meaning of these ``error codes`` is not explicitly defined. Proper error code documentation or the use of error enums would enhance ``clarity`` and ``usability``.

-  In the ``addressToBytes`` function, you can use ``bytes20(addr)`` directly instead of ``casting`` it as a ``bytes20``. It will not have any impact on gas costs, but it ``simplifies the code``

-  In the ``setTrustedSenders`` and ``unSetTrustedSenders`` functions, avoid encoding and decoding the same data by directly using the addr parameter as the key for the mapping

- ``Emit`` events for significant ``contract state changes``, as it helps in tracking contract activities and provides transparency to external applications

- Consider ``refactoring`` the code to break down ``complex functions`` into smaller, more manageable pieces, following the principles of ``modularity``

- Instead of using separate bool variables like ``guardianPauseAllowed``, you can use an enum to represent different contract states, such as`` enum ContractState`` ``{ Active, Paused, GuardianRevoked }``. This can help make the contract state more explicit and easier to understand
    
## Centralization risks

A single point of failure is not acceptable for this project Centrality risk is high in the project, the role of ``onlyOwner``detailed below has very critical and important powers

Project and funds may be compromised by a malicious or stolen private key ``onlyOwner`` ``msg.sender``

```solidity
File: src/core/Governance/TemporalGovernor.sol

266:     function fastTrackProposalExecution(bytes memory VAA) external onlyOwner {

274:     function togglePause() external onlyOwner {

```

https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Governance/TemporalGovernor.sol#L266-L266


## Gas Optimizations

1. Using ``token.balanceOf(address(this))`` every time can potentially reduce the contract's performance. Frequent external calls to other contracts, such as reading the balance of the token contract, can be expensive in terms of gas costs and execution time. This can lead to higher transaction costs and slower contract execution

2. Explicitly ``initializing default values`` for variables consumes ``additional ga``s during contract deployment. If you rely on the EVM's automatic default initialization, you can save gas costs associated with these unnecessary operations

3. ``Minimize the number of state variable reads and writes``. Use ``local variables`` when possible to avoid additional storage operations

4. ``Limit`` the number of iterations in ``loops`` to prevent ``excessive gas consumption``. Consider using other patterns like ``mapping``, where possible, to reduce the need for loops

5. Whenever applicable, consider ``batching multiple operations`` together to save on gas costs. For example, you can combine multiple token transfers into a single function call

6. Remove any ``unused or commented-out code`` to save space and simplify contract logic

7. Check if ``modifiers`` can be ``combined`` or ``reused across multiple functions`` to reduce the number of modifier calls. This can save gas by avoiding redundant checks.

8.  The ``enterMarkets`` function currently loops through the list of ``mTokens`` and ``emits an event`` for each entry. Consider ``batching`` the ``events`` or using other gas-saving techniques to reduce transaction costs

9. In the ``addToMarketInternal`` function, you can optimize the code by storing ``markets[address(mToken)] ``in a local variable, rather than repeatedly accessing it within the function

10.  Some operations, such as updating ``supply and borrow indexes``, are performed in a loop for each market in the ``claimReward`` function. This could lead to exceeding the block gas limit if the number of markets is substantial

11. Evaluate whether certain loops can be simplified or avoided altogether to reduce gas costs. For instance, consider whether iterating over the allMarkets array in the ``_addMarketInternal`` function can be optimized

12. ``Reuse variables`` instead of creating new ones when possible. This can reduce ``memory usage`` and, in turn, save gas

13. For functions that ``don't modify state``, use the ``view or pure`` modifiers to avoid unnecessary gas costs associated with state changes

14. For ``small and simple functions``, consider ``inlining`` them within other functions to save gas costs associated with function calls

15. Use gas-efficient math libraries, if available, to perform arithmetic operations. Check if there are any ``gas-efficient`` replacements for the current ``math operations``

16. If certain variables, such as ``admin, borrowCapGuardian, pauseGuardian,`` etc., are not intended to be changed after deployment, mark them as ``immutable`` to enhance ``security`` and ``gas efficiency`` 

17. Simplify code logic and eliminate ``redundant calculations``

18. In the ``executeProposal`` function, there are two function calls to ``trustedSenders[vm.emitterChainId]``.``contains(vm.emitterAddress)``. Consider storing the result of this call in a variable and using that variable to avoid calling the function ``twice``

19. In the ``setTrustedSenders`` and ``unSetTrustedSenders`` functions, you can move the ``msg.sender == address(this)`` check outside the loop to minimize ``storage access`` in each iteration saves gas

20. In the ``constructor``, you can use emit to emit the ``TrustedSenderUpdated`` event instead of using emit in the loop. This can reduce the gas cost by avoiding multiple event emissions

21. In the ``executeProposal`` function, the return value ``returnData`` is not used. You can ``remove`` it if it's not required.

## Risks as per Analysis

- Lack of integer validations in ``_setEmissionCap()`` function leads unexpected behavior 

- if (_amount == type(uint256).max) check is wrong . There is no gurantee that ``type(uint256).max`` and contract balance or same 
- ``updateMarketSupplyIndexInternal()``,``updateMarketSupplyIndexInternal()`` there is no limits checked when updating totalsupply   

- ``Inline assembly`` should be used with caution as it can introduce security risks. Whenever possible, prefer using ``built-in Solidity functions`` and ``libraries``.

``MultiRewardDistributor.sol``

- The contract interacts with external contracts, such as ``comptroller, IERC20, and MToken``, through external calls. These external calls are not checked for`` success or failure``, and there is no handling for potential exceptions or revert reasons, which can lead to ``unexpected results``.

- The contract involves various ``mathematical calculations``, such as ``index updates`` and ``reward distributions``. It's crucial to ensure that these operations are handled correctly and do not result in ``integer overflows`` or ``underflows``.

- Some of the functions in the contract, such as ``updateMarketSupplyIndexInternal``, ``updateMarketBorrowIndexInternal``, ``disburseSupplierRewardsInternal``, and ``disburseBorrowerRewardsInternal``, perform calculations and iterations that could potentially consume a significant amount of gas. It's essential to ensure that these functions are optimized to prevent gas limitations and high transaction costs.

- The contract uses ``uint224`` data type for some of the index values ``(_globalSupplyIndex, _globalBorrowIndex, etc.)``. If these indices grow ``too large``, it could potentially lead to overflow issues and incorrect ``reward calculations```.

- The ``sendReward`` function performs a token transfer and is marked as ``nonReentrant``, but it's essential to ensure that all external contract calls, especially token transfers, are safe against reentrancy attacks

- The contract allows the owner to update ``supply`` and ``borrow emission speeds``. There should be careful consideration of the emission rates and their effects on the overall ``token economy`` to avoid unintended consequences

- The contract has two functions ``(calculateSupplyRewardsForUser and calculateBorrowRewardsForUser)`` for calculating rewards for ``suppliers and borrowers``, respectively. Ensure that both functions use consistent calculations and share the ``same index`` values to`` prevent discrepancies``

- Consider using ``upgradeable contract patterns`` to allow for ``future updates`` without redeploying the entire contract

``Comptroller.sol``

- In the transferAllowed function, there's a comment mentioning the use of local vars to ``avoid stack-depth limits`` in calculating account liquidity. While it's a good practice to use ``local variables`` for ``complex calculations``, it's important to ensure that the ``overall stack depth`` is managed appropriately, especially in functions that could be called in ``nested scenarios``

- The contract appears to have some administrative functions like ``_setPriceOracle, _setCloseFactor, _setCollateralFactor,`` etc., which should only be callable by the ``admin``. However, the access control checks are not consistently applied in all these functions, potentially allowing unauthorized users to modify critical parameters

- Several functions call external contracts, like the ``rewardDistributor``,`` oracle``, and token contracts, but they do ``not have proper error handling`` for potential failures. Lack of error handling can lead to unexpected behavior and vulnerabilities

- The contract has several functions for pausing different actions ``(_setMintPaused, _setBorrowPaused, _setTransferPaused, _setSeizePaused)``. However, there is no mechanism for ``time-based unpausing or emergency unpause``, which could be essential for the contract's safety

- The ``liquidateCalculateSeizeTokens`` function, used during liquidation, performs arithmetic calculations using external data (e.g., exchangeRateMantissa). Failing to ``handle or validate external data correctly`` could lead to vulnerabilities in the liquidation process

- The ``claimReward`` function, responsible for distributing rewards, has a complex design with nested loops, and it calls an external contract multiple times. High complexity increases the risk of errors and makes the contract harder to audit

-  The ``getBlockTimestamp`` function retrieves the current block timestamp using block.timestamp. It's worth noting that relying solely on block timestamps for time-sensitive operations can be manipulated by miners to some extent

- Certain functions that modify contract parameters (e.g., _setPriceOracle, _setCloseFactor, etc.) do not include ``time limitations`` for changes. Adding ``time restrictions`` for critical parameter modifications could prevent`` malicious or mistaken changes``

``TemporalGovernor.sol``

- The contract relies on trusted emitters from the Wormhole bridge using ``isTrustedSender``. However, relying solely on a ``chain ID`` and a 32-byte emitter address might not be sufficient to ensure the ``authenticity`` of the signed messages. Consider using a more ``robust signature verification`` mechanism to ensure the validity of messages

- The contract doesn't have any explicit ``reentrancy protection``. Ensure that critical state changes are performed before any external calls to prevent potential ``reentrancy attacks``

- The contract uses ``uint248`` for ``lastPauseTime``, which might be susceptible to integer overflow if the pause timestamp becomes larger than the ``maximum value``

- The fastTrackProposalExecution function allows the owner to bypass the usual delay for proposal execution. This can be risky as it doesn't have any checks on the validity of the proposal. It should be handled with caution and only used in specific emergency scenarios

- The function ``allTrustedSender``s iterates through the entire trustedSenders set to return the list of trusted senders for a chain. This operation may become inefficient as the size of the set grows. Consider alternative data structures or optimizations if needed

``ChainlinkCompositeOracle.sol``

- The contract relies on ``Chainlink oracles`` to provide accurate price data. If a Chainlink oracle is ``compromised`` or ``provides incorrect data``, the ChainlinkCompositeOracle contract could also be compromised

- The contract does not have any mechanisms in place to ``prevent front-running`` or`` other forms of market manipulation``. This could allow malicious actors to exploit the contract and profit at the expense of other users

- The contract relies on the ``decimals`` value returned by Chainlink oracles. If the decimals value is misaligned with the ``contract's expectations``, it could result in ``incorrect price calculations`` and pose a vulnerability


## Non-functional aspects

- Aim for high test coverage to validate the contract's behavior and catch potential bugs or vulnerabilities
- The protocol execution flow is not explained in efficient way. 
- Its best to explain over all protocol with architecture is easy to understandings 
- Consider designing the contract to be upgradable or allow for versioning. This can help address issues, introduce new features, or adapt to evolving requirements without disrupting the entire system

## Time spent on analysis 
``15 Hours``

































































### Time spent:
13 hours