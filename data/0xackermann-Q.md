# QA Report
[Exclusive findings](#exclusive-findings) is below the [automated findings](#automated-findings).

---

## Automated Findings
**Note:** There is a section for disputed findings below the gas report section

## Summary

### Medium Risk Issues

| |Issue|Instances|
|-|:-|:-:|
| [[M&#x2011;01](#m01-unsafe-use-of-transfertransferfrom-with-ierc20)] | Unsafe use of `transfer()`/`transferFrom()` with `IERC20` | 2 | 
| [[M&#x2011;02](#m02-insufficient-oracle-validation)] | Insufficient oracle validation | 2 | 
| [[M&#x2011;03](#m03-missing-checks-for-whether-the-l2-sequencer-is-active)] | Missing checks for whether the L2 Sequencer is active | 2 | 
| [[M&#x2011;04](#m04-the-owner-is-a-single-point-of-failure-and-a-centralization-risk)] | The `owner` is a single point of failure and a centralization risk | 2 | 
| [[M&#x2011;05](#m05-some-tokens-may-revert-when-zero-value-transfers-are-made)] | Some tokens may revert when zero value transfers are made | 2 | 

Total: 10 instances over 5 issues

### Low Risk Issues

| |Issue|Instances|
|-|:-|:-:|
| [[L&#x2011;01](#l01-loss-of-precision)] | Loss of precision | 3 | 
| [[L&#x2011;02](#l02-empty-receivepayable-fallback-function-does-not-authorize-requests)] | Empty `receive()`/`payable fallback()` function does not authorize requests | 1 | 
| [[L&#x2011;03](#l03-use-ownable2step-rather-than-ownable)] | Use `Ownable2Step` rather than `Ownable` | 1 | 
| [[L&#x2011;04](#l04-missing-contract-existence-checks-before-low-level-calls)] | Missing contract-existence checks before low-level calls | 1 | 
| [[L&#x2011;05](#l05-external-call-recipient-may-consume-all-transaction-gas)] | External call recipient may consume all transaction gas | 1 | 
| [[L&#x2011;06](#l06-functions-calling-contractsaddresses-with-transfer-hooks-are-missing-reentrancy-guards)] | Functions calling contracts/addresses with transfer hooks are missing reentrancy guards | 7 | 
| [[L&#x2011;07](#l07-unusedempty-receivefallback-function)] | Unused/empty `receive()`/`fallback()` function | 1 | 
| [[L&#x2011;08](#l08-external-calls-in-an-un-bounded-for-loop-may-result-in-a-dos)] | External calls in an un-bounded `for-`loop may result in a DOS | 6 | 
| [[L&#x2011;09](#l09-initialization-can-be-front-run)] | Initialization can be front-run | 1 | 
| [[L&#x2011;10](#l10-missing-checks-for-address0x0-when-updating-address-state-variables)] | Missing checks for `address(0x0)` when updating `address` state variables | 13 | 
| [[L&#x2011;11](#l11-missing-checks-for-address0x0-in-the-constructor)] | Missing checks for `address(0x0)` in the constructor | 5 | 
| [[L&#x2011;12](#l12-upgradeable-contract-uses-non-upgradeable-version-of-the-openzeppelin-librariescontracts)] | Upgradeable contract uses non-upgradeable version of the OpenZeppelin libraries/contracts | 5 | 
| [[L&#x2011;13](#l13-symbol-is-not-a-part-of-the-erc-20-standard)] | `symbol()` is not a part of the ERC-20 standard | 1 | 
| [[L&#x2011;14](#l14-decimals-is-not-a-part-of-the-erc-20-standard)] | `decimals()` is not a part of the ERC-20 standard | 1 | 
| [[L&#x2011;15](#l15-sweeping-may-break-accounting-if-tokens-with-multiple-addresses-are-used)] | Sweeping may break accounting if tokens with multiple addresses are used | 1 | 
| [[L&#x2011;16](#l16-consider-implementing-two-step-procedure-for-updating-protocol-addresses)] | Consider implementing two-step procedure for updating protocol addresses | 1 | 
| [[L&#x2011;17](#l17-a-year-is-not-always-365-days)] | A year is not always 365 days | 1 | 

Total: 50 instances over 17 issues

### Non-critical Issues

| |Issue|Instances|
|-|:-|:-:|
| [[N&#x2011;01](#n01-adding-a-return-statement-when-the-function-defines-a-named-return-variable-is-redundant)] | Adding a `return` statement when the function defines a named return variable, is redundant | 1 | 
| [[N&#x2011;02](#n02-public-functions-not-called-by-the-contract-should-be-declared-external-instead)] | `public` functions not called by the contract should be declared `external` instead | 20 | 
| [[N&#x2011;03](#n03-constants-should-be-defined-rather-than-using-magic-numbers)] | `constant`s should be defined rather than using magic numbers | 31 | 
| [[N&#x2011;04](#n04-event-is-not-properly-indexed)] | Event is not properly `indexed` | 12 | 
| [[N&#x2011;05](#n05-duplicated-requirerevert-checks-should-be-refactored-to-a-modifier-or-function)] | Duplicated `require()`/`revert()` checks should be refactored to a modifier or function | 9 | 
| [[N&#x2011;06](#n06-import-declarations-should-import-specific-identifiers-rather-than-the-whole-file)] | Import declarations should import specific identifiers, rather than the whole file | 27 | 
| [[N&#x2011;07](#n07-events-that-mark-critical-parameter-changes-should-contain-both-the-old-and-the-new-value)] | Events that mark critical parameter changes should contain both the old and the new value | 4 | 
| [[N&#x2011;08](#n08-constant-redefined-elsewhere)] | Constant redefined elsewhere | 1 | 
| [[N&#x2011;09](#n09-inconsistent-spacing-in-comments)] | Inconsistent spacing in comments | 2 | 
| [[N&#x2011;10](#n10-lines-are-too-long)] | Lines are too long | 107 | 
| [[N&#x2011;11](#n11-variable-names-that-consist-of-all-capital-letters-should-be-reserved-for-constantimmutable-variables)] | Variable names that consist of all capital letters should be reserved for `constant`/`immutable` variables | 1 | 
| [[N&#x2011;12](#n12-non-libraryinterface-files-should-use-fixed-compiler-versions-not-floating-ones)] | Non-library/interface files should use fixed compiler versions, not floating ones | 2 | 
| [[N&#x2011;13](#n13-typos)] | Typos | 6 | 
| [[N&#x2011;14](#n14-file-does-not-contain-an-spdx-identifier)] | File does not contain an SPDX Identifier | 1 | 
| [[N&#x2011;15](#n15-natspec-param-is-missing)] | NatSpec `@param` is missing | 35 | 
| [[N&#x2011;16](#n16-natspec-return-argument-is-missing)] | NatSpec `@return` argument is missing | 28 | 
| [[N&#x2011;17](#n17-function-definition-modifier-order-does-not-follow-solidity-style-guide)] | Function definition modifier order does not follow Solidity style guide | 96 | 
| [[N&#x2011;18](#n18-large-assembly-blocks-should-have-extensive-comments)] | Large assembly blocks should have extensive comments | 2 | 
| [[N&#x2011;19](#n19-function-ordering-does-not-follow-the-solidity-style-guide)] | Function ordering does not follow the Solidity style guide | 35 | 
| [[N&#x2011;20](#n20-contract-does-not-follow-the-solidity-style-guides-suggested-layout-ordering)] | Contract does not follow the Solidity style guide's suggested layout ordering | 7 | 
| [[N&#x2011;21](#n21-control-structures-do-not-follow-the-solidity-style-guide)] | Control structures do not follow the Solidity Style Guide | 4 | 
| [[N&#x2011;22](#n22-imports-should-use-double-quotes-rather-than-single-quotes)] | Imports should use double quotes rather than single quotes | 1 | 
| [[N&#x2011;23](#n23-mapping-definitions-do-not-follow-the-solidity-style-guide)] | `mapping` definitions do not follow the Solidity Style Guide | 2 | 
| [[N&#x2011;24](#n24-numeric-values-having-to-do-with-time-should-use-time-units-for-readability)] | Numeric values having to do with time should use time units for readability | 3 | 
| [[N&#x2011;25](#n25-consider-using-delete-rather-than-assigning-zerofalse-to-clear-values)] | Consider using `delete` rather than assigning zero/false to clear values | 1 | 
| [[N&#x2011;26](#n26-contracts-should-have-full-test-coverage)] | Contracts should have full test coverage | 1 | 
| [[N&#x2011;27](#n27-large-or-complicated-code-bases-should-implement-invariant-tests)] | Large or complicated code bases should implement invariant tests | 1 | 
| [[N&#x2011;28](#n28-enable-ir-based-code-generation)] | Enable IR-based code generation | 1 | 
| [[N&#x2011;29](#n29-custom-errors-should-be-used-rather-than-revertrequire)] | Custom errors should be used rather than `revert()`/`require()` | 118 | 
| [[N&#x2011;30](#n30-array-indicies-should-be-referenced-via-enums-rather-than-via-numeric-literals)] | Array indicies should be referenced via `enum`s rather than via numeric literals | 1 | 
| [[N&#x2011;31](#n31-chainlink-oracle-roundid-and-answeredin-variables-no-longer-contain-useful-information)] | Chainlink oracle `roundId` and `answeredIn` variables no longer contain useful information | 1 | 
| [[N&#x2011;32](#n32-variable-names-for-constants-dont-follow-the-solidity-style-guide)] | Variable names for constants don't follow the Solidity style guide | 8 | 
| [[N&#x2011;33](#n33-use-openzeppelins-reentrancyguardreentrancyguardupgradeable-rather-than-writing-your-own)] | Use OpenZeppelin's `ReentrancyGuard`/`ReentrancyGuardUpgradeable` rather than writing your own | 2 | 
| [[N&#x2011;34](#n34-events-are-missing-sender-information)] | Events are missing sender information | 35 | 
| [[N&#x2011;35](#n35-variables-need-not-be-initialized-to-zero)] | Variables need not be initialized to zero | 48 | 
| [[N&#x2011;36](#n36-consider-using-named-mappings)] | Consider using named mappings | 5 | 
| [[N&#x2011;37](#n37-consider-adding-a-blockdeny-list)] | Consider adding a block/deny-list | 8 | 
| [[N&#x2011;38](#n38-non-externalpublic-variable-and-function-names-should-begin-with-an-underscore)] | Non-`external`/`public` variable and function names should begin with an underscore | 41 | 
| [[N&#x2011;39](#n39-non-externalpublic-variable-names-should-begin-with-an-underscore)] | Non-`external`/`public` variable names should begin with an underscore | 6 | 
| [[N&#x2011;40](#n40-use-of-override-is-unnecessary)] | Use of `override` is unnecessary | 91 | 
| [[N&#x2011;41](#n41-array-is-pushed-but-not-poped)] | Array is `push()`ed but not `pop()`ed | 3 | 
| [[N&#x2011;42](#n42-consider-using-safetransferlibsafetransfereth-or-addresssendvalue-for-clearer-semantic-meaning)] | Consider using `SafeTransferLib.safeTransferETH()` or `Address.sendValue()` for clearer semantic meaning | 1 | 
| [[N&#x2011;43](#n43-complex-casting)] | Complex casting | 2 | 
| [[N&#x2011;44](#n44-large-numeric-literals-should-use-underscores-for-readability)] | Large numeric literals should use underscores for readability | 1 | 
| [[N&#x2011;45](#n45-unused-contract-variables)] | Unused contract variables | 2 | 
| [[N&#x2011;46](#n46-unused-modifers)] | Unused `modifer`s | 1 | 
| [[N&#x2011;47](#n47-use-abiencodecall-instead-of-abiencodewithsignatureabiencodewithselector)] | Use `abi.encodeCall()` instead of `abi.encodeWithSignature()`/`abi.encodeWithSelector()` | 38 | 
| [[N&#x2011;48](#n48-abiencodesignature-does-not-work-properly-with-aliases)] | `abi.encodeSignature()` does not work properly with aliases | 38 | 
| [[N&#x2011;49](#n49-consider-using-descriptive-constants-when-passing-zero-as-a-function-argument)] | Consider using descriptive `constant`s when passing zero as a function argument | 6 | 
| [[N&#x2011;50](#n50-assembly-block-creates-dirty-bits)] | Assembly block creates dirty bits | 2 | 
| [[N&#x2011;51](#n51-constants-in-comparisons-should-appear-on-the-left-side)] | Constants in comparisons should appear on the left side | 55 | 
| [[N&#x2011;52](#n52-consider-disabling-renounceownership)] | Consider disabling `renounceOwnership()` | 1 | 
| [[N&#x2011;53](#n53-else-block-not-required)] | `else`-block not required | 6 | 
| [[N&#x2011;54](#n54-consider-adding-emergency-stop-functionality)] | Consider adding emergency-stop functionality | 1 | 
| [[N&#x2011;55](#n55-cast-to-bytes-or-bytes32-for-clearer-semantic-meaning)] | Cast to `bytes` or `bytes32` for clearer semantic meaning | 4 | 
| [[N&#x2011;56](#n56-use-stringconcat-on-strings-instead-of-abiencodepacked-for-clearer-semantic-meaning)] | Use `string.concat()` on strings instead of `abi.encodePacked()` for clearer semantic meaning | 4 | 
| [[N&#x2011;57](#n57-events-may-be-emitted-out-of-order-due-to-reentrancy)] | Events may be emitted out of order due to reentrancy | 4 | 
| [[N&#x2011;58](#n58-consider-bounding-input-array-length)] | Consider bounding input array length | 9 | 
| [[N&#x2011;59](#n59-if-statement-can-be-converted-to-a-ternary)] | `if`-statement can be converted to a ternary | 4 | 
| [[N&#x2011;60](#n60-consider-moving-msgsender-checks-to-a-common-authorization-modifier)] | Consider moving `msg.sender` checks to a common authorization `modifier` | 15 | 
| [[N&#x2011;61](#n61-setters-should-prevent-re-setting-of-the-same-value)] | Setters should prevent re-setting of the same value | 2 | 
| [[N&#x2011;62](#n62-imports-could-be-organized-more-systematically)] | Imports could be organized more systematically | 2 | 
| [[N&#x2011;63](#n63-polymorphic-functions-make-security-audits-more-time-consuming-and-error-prone)] | Polymorphic functions make security audits more time-consuming and error-prone | 5 | 
| [[N&#x2011;64](#n64-long-functions-should-be-refactored-into-multiple-smaller-functions)] | Long functions should be refactored into multiple, smaller, functions | 2 | 
| [[N&#x2011;65](#n65-mixed-usage-of-intuint-with-int256uint256)] | Mixed usage of `int`/`uint` with `int256`/`uint256` | 578 | 
| [[N&#x2011;66](#n66-use-the-latest-solidity-prior-to-0820-if-on-l2s-for-deployment)] | Use the latest solidity (prior to 0.8.20 if on L2s) for deployment | 14 | 

Total: 1605 instances over 66 issues

### Disputed Issues

The issues below may be reported by other bots/wardens, but can be penalized/ignored since either the rule or the specified instances are invalid

| |Issue|Instances|
|-|:-|:-:|
| [[D&#x2011;01](#d01-mintburn-missing-access-control)] | ~~`mint()`/`burn()` missing access control~~ | 3 | 
| [[D&#x2011;02](#d02-return-values-of-transfertransferfrom-not-checked)] | ~~Return values of `transfer()`/`transferFrom()` not checked~~ | 2 | 
| [[D&#x2011;03](#d03-some-tokens-may-revert-when-zero-value-transfers-are-made)] | ~~Some tokens may revert when zero value transfers are made~~ | 8 | 
| [[D&#x2011;04](#d04-unsafe-downcast)] | ~~Unsafe downcast~~ | 3 | 
| [[D&#x2011;05](#d05-require-should-be-used-instead-of-assert)] | ~~`require()` should be used instead of `assert()`~~ | 4 | 
| [[D&#x2011;06](#d06-missing-contract-existence-checks-before-low-level-calls)] | ~~Missing contract-existence checks before low-level calls~~ | 1 | 
| [[D&#x2011;07](#d07-external-call-recipient-may-consume-all-transaction-gas)] | ~~External call recipient may consume all transaction gas~~ | 1 | 
| [[D&#x2011;08](#d08-division-by-zero-not-prevented)] | ~~Division by zero not prevented~~ | 1 | 
| [[D&#x2011;09](#d09-solidity-version-0820-may-not-work-on-other-chains-due-to-push0)] | ~~Solidity version 0.8.20 may not work on other chains due to `PUSH0`~~ | 2 | 
| [[D&#x2011;10](#d10-error-messages-should-descriptive-rather-that-cryptic)] | ~~Error messages should descriptive, rather that cryptic~~ | 13 | 
| [[D&#x2011;11](#d11-shorten-the-array-rather-than-copying-to-a-new-one)] | ~~Shorten the array rather than copying to a new one~~ | 5 | 
| [[D&#x2011;12](#d12-storage-write-removal-bug-on-conditional-early-termination)] | ~~Storage Write Removal Bug On Conditional Early Termination~~ | 4 | 
| [[D&#x2011;13](#d13-bad-bot-rules)] | ~~Bad bot rules~~ | 1 | 
| [[D&#x2011;14](#d14-missing-contract-existence-checks-before-low-level-calls)] | ~~Missing contract-existence checks before low-level calls~~ | 1 | 
| [[D&#x2011;15](#d15-function-result-should-be-cached)] | ~~Function result should be cached~~ | 2 | 
| [[D&#x2011;16](#d16-use-delete-instead-of-setting-mappingstate-variable-to-zero-to-save-gas)] | ~~Use delete instead of setting mapping/state variable to zero, to save gas~~ | 5 | 
| [[D&#x2011;17](#d17-event-names-should-use-camelcase)] | ~~Event names should use CamelCase~~ | 24 | 
| [[D&#x2011;18](#d18-contracts-do-not-work-with-fee-on-transfer-tokens)] | ~~Contracts do not work with fee-on-transfer tokens~~ | 4 | 
| [[D&#x2011;19](#d19-multiplications-of-powers-of-can-be-replaced-by-a-left-shift-operation-to-save-gas)] | ~~Multiplications of powers of can be replaced by a left shift operation to save gas~~ | 1 | 
| [[D&#x2011;20](#d20-contracts-are-not-using-their-oz-upgradeable-counterparts)] | ~~Contracts are not using their OZ Upgradeable counterparts~~ | 10 | 
| [[D&#x2011;21](#d21-decimals-is-not-a-part-of-the-erc-20-standard)] | ~~`decimals()` is not a part of the ERC-20 standard~~ | 3 | 
| [[D&#x2011;22](#d22-symbol-is-not-a-part-of-the-erc-20-standard)] | ~~`symbol()` is not a part of the ERC-20 standard~~ | 2 | 
| [[D&#x2011;23](#d23-change-public-function-visibility-to-external-to-save-gas)] | ~~Change `public` function visibility to `external` to save gas~~ | 23 | 
| [[D&#x2011;24](#d24-use-replace-and-pop-instead-of-the-delete-keyword-to-removing-an-item-from-an-array)] | ~~Use replace and pop instead of the delete keyword to removing an item from an array~~ | 1 | 
| [[D&#x2011;25](#d25-do-not-use-underscore-at-the-end-of-variable-name)] | ~~Do not use underscore at the end of variable name~~ | 12 | 
| [[D&#x2011;26](#d26-save-gas-with-the-use-of-specific-import-statements)] | ~~Save gas with the use of specific import statements~~ | 27 | 

Total: 163 instances over 26 issues




## Medium Risk Issues


### [M&#x2011;01] Unsafe use of `transfer()`/`transferFrom()` with `IERC20`
Some tokens do not implement the ERC20 standard properly but are still accepted by most code that accepts ERC20 tokens.  For example Tether (USDT)'s `transfer()` and `transferFrom()` functions on L1 do not return booleans as the specification requires, and instead have no return value. When these sorts of tokens are cast to `IERC20`, their [function signatures](https://medium.com/coinmonks/missing-return-value-bug-at-least-130-tokens-affected-d67bf08521ca) do not match and therefore the calls made, revert (see [this](https://gist.github.com/IllIllI000/2b00a32e8f0559e8f386ea4f1800abc5) link for a test case). Use OpenZeppelin’s `SafeERC20`'s `safeTransfer()`/`safeTransferFrom()` instead

*There are 2 instances of this issue:*

```solidity
File: src/core/Comptroller.sol

965:              token.transfer(admin, token.balanceOf(address(this)));

967:              token.transfer(admin, _amount);

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Comptroller.sol#L965


### [M&#x2011;02] Insufficient oracle validation
There is no freshness check on the timestamp of the prices fetched from the Chainlink oracle, so old prices may be used if [OCR](https://docs.chain.link/architecture-overview/off-chain-reporting) was unable to push an update in time. Add a staleness threshold number of seconds configuration parameter, and ensure that the price fetched is from within that time range.

*There are 2 instances of this issue:*

```solidity
File: src/core/Oracles/ChainlinkCompositeOracle.sol

183          (
184              uint80 roundId,
185              int256 price,
186              ,
187              ,
188              uint80 answeredInRound
189:         ) = AggregatorV3Interface(oracleAddress).latestRoundData();

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkCompositeOracle.sol#L183-L189

```solidity
File: src/core/Oracles/ChainlinkOracle.sol

100          (, int256 answer, , uint256 updatedAt, ) = AggregatorV3Interface(feed)
101:             .latestRoundData();

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkOracle.sol#L100-L101


### [M&#x2011;03] Missing checks for whether the L2 Sequencer is active
Chainlink recommends that users using price oracles, check whether the Arbitrum Sequencer is [active](https://docs.chain.link/data-feeds/l2-sequencer-feeds#arbitrum). If the sequencer goes down, the Chainlink oracles will have stale prices from before the downtime, until a new L2 OCR transaction goes through. Users who submit their transactions via the [L1 Dealyed Inbox](https://developer.arbitrum.io/tx-lifecycle#1b--or-from-l1-via-the-delayed-inbox) will be able to take advantage of these stale prices. Use a [Chainlink oracle](https://blog.chain.link/how-to-use-chainlink-price-feeds-on-arbitrum/#almost_done!_meet_the_l2_sequencer_health_flag) to determine whether the sequencer is offline or not, and don't allow operations to take place while the sequencer is offline.

*There are 2 instances of this issue:*

```solidity
File: src/core/Oracles/ChainlinkCompositeOracle.sol

189:         ) = AggregatorV3Interface(oracleAddress).latestRoundData();

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkCompositeOracle.sol#L189-L189

```solidity
File: src/core/Oracles/ChainlinkOracle.sol

100          (, int256 answer, , uint256 updatedAt, ) = AggregatorV3Interface(feed)
101:             .latestRoundData();

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkOracle.sol#L100-L101


### [M&#x2011;04] The `owner` is a single point of failure and a centralization risk
Having a single EOA as the only owner of contracts is a large centralization risk and a single point of failure. A single private key may be taken in a hack, or the sole holder of the key may become unable to retrieve the key when necessary. Consider changing to a multi-signature setup, or having a role-based authorization model.

*There are 2 instances of this issue:*

```solidity
File: src/core/Governance/TemporalGovernor.sol

266:     function fastTrackProposalExecution(bytes memory VAA) external onlyOwner {

274:     function togglePause() external onlyOwner {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Governance/TemporalGovernor.sol#L266-L266


### [M&#x2011;05] Some tokens may revert when zero value transfers are made
In spite of the fact that EIP-20 [states](https://github.com/ethereum/EIPs/blob/46b9b698815abbfa628cd1097311deee77dd45c5/EIPS/eip-20.md?plain=1#L116) that zero-valued transfers must be accepted, some tokens, such as LEND will revert if this is attempted, which may cause transactions that involve other tokens (such as batch operations) to fully revert. Consider skipping the transfer if the amount is zero, which will also save gas.

*There are 2 instances of this issue:*

```solidity
File: src/core/Comptroller.sol

964          if (_amount == type(uint).max) {
965              token.transfer(admin, token.balanceOf(address(this)));
966          } else {
967:             token.transfer(admin, _amount);

962          IERC20 token = IERC20(_tokenAddress);
963          // Similar to mTokens, if this is uint.max that means "transfer everything"
964          if (_amount == type(uint).max) {
965:             token.transfer(admin, token.balanceOf(address(this)));

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Comptroller.sol#L964-L967

## Low Risk Issues


### [L&#x2011;01] Loss of precision
Division by large numbers may result in the result being zero, due to solidity not supporting fractions. Consider requiring a minimum amount for the numerator to ensure that it is always larger than the denominator

*There are 3 instances of this issue:*

```solidity
File: src/core/Oracles/ChainlinkCompositeOracle.sol

95:           return ((basePrice * priceMultiplier) / scalingFactor).toUint256();

158:              ((firstPrice * secondPrice * thirdPrice) / scalingFactor)

212:                  price / (10 ** uint256(priceDecimals - expectedDecimals)).toInt256();

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkCompositeOracle.sol#L95


### [L&#x2011;02] Empty `receive()`/`payable fallback()` function does not authorize requests
If the intention is for the Ether to be used, the function should call another function, otherwise it should revert (e.g. `require(msg.sender == address(weth))`). Having no access control on the function means that someone may send Ether to the contract, and have no way to get anything back out, which is a loss of funds. If the concern is having to spend a small amount of gas to check the sender against an immutable address, the code should at least have a function to rescue unused Ether.

*There is one instance of this issue:*

```solidity
File: src/core/router/WETHRouter.sol

65:       receive() external payable {}

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/router/WETHRouter.sol#L65


### [L&#x2011;03] Use `Ownable2Step` rather than `Ownable`
[`Ownable2Step`](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/3d7a93876a2e5e1d7fe29b5a0e96e222afdc4cfa/contracts/access/Ownable2Step.sol#L31-L56) and [`Ownable2StepUpgradeable`](https://github.com/OpenZeppelin/openzeppelin-contracts-upgradeable/blob/25aabd286e002a1526c345c8db259d57bdf0ad28/contracts/access/Ownable2StepUpgradeable.sol#L47-L63) prevent the contract ownership from mistakenly being transferred to an address that cannot handle it (e.g. due to a typo in the address), by requiring that the recipient of the owner permissions actively accept via a contract call of its own.

*There is one instance of this issue:*

```solidity
File: src/core/Governance/TemporalGovernor.sol

27:   contract TemporalGovernor is ITemporalGovernor, Ownable, Pausable {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Governance/TemporalGovernor.sol#L27


### [L&#x2011;04] Missing contract-existence checks before low-level calls
Low-level calls return success if there is no code present at the specified address. In addition to the zero-address checks, add a check to verify that `<address>.code.length > 0`

*There is one instance of this issue:*

```solidity
File: src/core/Governance/TemporalGovernor.sol

400              (bool success, bytes memory returnData) = target.call{value: value}(
401                  data
402              );
403  
404              /// revert on failure with error message if any
405:             require(success, string(returnData));

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Governance/TemporalGovernor.sol#L400-L405


### [L&#x2011;05] External call recipient may consume all transaction gas
There is no limit specified on the amount of gas used, so the recipient can use up all of the transaction's gas, causing it to revert. Use `addr.call{gas: <amount>}("")` or [this](https://github.com/nomad-xyz/ExcessivelySafeCall) library instead.

*There is one instance of this issue:*

```solidity
File: src/core/Governance/TemporalGovernor.sol

/// @audit `_executeProposal()`
400:             (bool success, bytes memory returnData) = target.call{value: value}(

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Governance/TemporalGovernor.sol#L400-L400


### [L&#x2011;06] Functions calling contracts/addresses with transfer hooks are missing reentrancy guards
Even if the function follows the best practice of check-effects-interaction, not using a reentrancy guard when there may be transfer hooks will open the users of this protocol up to [read-only reentrancies](https://chainsecurity.com/curve-lp-oracle-manipulation-post-mortem/) with no way to protect against it, except by block-listing the whole protocol.

*There are 7 instances of this issue:*

```solidity
File: src/core/Comptroller.sol

/// @audit `_rescueFunds()`
967:             token.transfer(admin, _amount);

/// @audit `_rescueFunds()`
965:             token.transfer(admin, token.balanceOf(address(this)));

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Comptroller.sol#L967-L967

```solidity
File: src/core/MErc20.sol

/// @audit `sweepToken()`
152:     	token.transfer(admin, balance);

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20.sol#L152-L152

```solidity
File: src/core/MultiRewardDistributor/MultiRewardDistributor.sol

/// @audit `_rescueFunds()`
483:             token.safeTransfer(comptroller.admin(), _amount);

/// @audit `_rescueFunds()`
478:             token.safeTransfer(

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L483-L483

```solidity
File: src/core/router/WETHRouter.sol

/// @audit `mint()`
36:          IERC20(address(mToken)).safeTransfer(

/// @audit `redeem()`
46:          IERC20(address(mToken)).safeTransferFrom(

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/router/WETHRouter.sol#L36-L36


### [L&#x2011;07] Unused/empty `receive()`/`fallback()` function
If the intention is for the Ether to be used, the function should call another function or emit an event, otherwise it should revert.

*There is one instance of this issue:*

```solidity
File: src/core/router/WETHRouter.sol

65:      receive() external payable {}

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/router/WETHRouter.sol#L65-L65


### [L&#x2011;08] External calls in an un-bounded `for-`loop may result in a DOS
Consider limiting the number of iterations in `for-`loops that make external calls

*There are 6 instances of this issue:*

```solidity
File: src/core/Comptroller.sol

578:             (oErr, vars.mTokenBalance, vars.borrowBalance, vars.exchangeRateMantissa) = asset.getAccountSnapshot(account);

586:             vars.oraclePriceMantissa = oracle.getUnderlyingPrice(asset);

1039:                rewardDistributor.updateMarketSupplyIndex(mToken);

1041:                    rewardDistributor.disburseSupplierRewards(mToken, holders[holderIndex], true);

1047:                rewardDistributor.updateMarketBorrowIndex(mToken);

1049:                    rewardDistributor.disburseBorrowerRewards(mToken, holders[holderIndex], true);

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Comptroller.sol#L578-L578


### [L&#x2011;09] Initialization can be front-run
The `initialize()` functions below are not called by another contract atomically after the contract is deployed, so it's possible for a malicious user to call `initialize()` which, if it's noticed in time, would require the project to re-deploy the contract in order to properly initialize. Consider creating a factory contract, which will `new` and `initialize()` each contract atomically.

*There is one instance of this issue:*

```solidity
File: src/core/MultiRewardDistributor/MultiRewardDistributor.sol

88       function initialize(
89           address _comptroller,
90           address _pauseGuardian
91:      ) external initializer {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L88-L91


### [L&#x2011;10] Missing checks for `address(0x0)` when updating `address` state variables


*There are 13 instances of this issue:*

```solidity
File: src/core/Comptroller.sol

675:         oracle = newOracle;

832:         borrowCapGuardian = newBorrowCapGuardian;

869:         supplyCapGuardian = newSupplyCapGuardian;

889:         pauseGuardian = newPauseGuardian;

906:         rewardDistributor = newRewardDistributor;

1017:        holders[0] = holder;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Comptroller.sol#L675-L675

```solidity
File: src/core/MErc20.sol

34:          underlying = underlying_;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20.sol#L34-L34

```solidity
File: src/core/MErc20Delegator.sol

69:          implementation = implementation_;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20Delegator.sol#L69-L69

```solidity
File: src/core/MultiRewardDistributor/MultiRewardDistributor.sol

760:         emissionConfig.config.owner = _newOwner;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L760-L760

```solidity
File: src/core/Oracles/ChainlinkOracle.sol

175:         admin = newAdmin;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkOracle.sol#L175-L175

```solidity
File: src/core/Unitroller.sol

47:          pendingComptrollerImplementation = newPendingImplementation;

96:          pendingAdmin = newPendingAdmin;

120:         admin = pendingAdmin;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Unitroller.sol#L47-L47


### [L&#x2011;11] Missing checks for `address(0x0)` in the constructor


*There are 5 instances of this issue:*

```solidity
File: src/core/Oracles/ChainlinkCompositeOracle.sol

40:          base = baseAddress;

41:          multiplier = multiplierAddress;

42:          secondMultiplier = secondMultiplierAddress;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkCompositeOracle.sol#L40-L40

```solidity
File: src/core/router/WETHRouter.sol

24:          weth = _weth;

25:          mToken = _mToken;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/router/WETHRouter.sol#L24-L24


### [L&#x2011;12] Upgradeable contract uses non-upgradeable version of the OpenZeppelin libraries/contracts
OpenZeppelin has an [Upgradeable](https://github.com/OpenZeppelin/openzeppelin-contracts-upgradeable/tree/master/contracts/utils) variants of each of its libraries and contracts, and upgradeable contracts should use those variants.

*There are 5 instances of this issue:*

```solidity
File: src/core/MultiRewardDistributor/MultiRewardDistributor.sol

4:   import {ReentrancyGuard} from "@openzeppelin/contracts/security/ReentrancyGuard.sol";

5:   import {Initializable} from "@openzeppelin/contracts/proxy/utils/Initializable.sol";

6:   import {SafeERC20} from "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

7:   import {Pausable} from "@openzeppelin/contracts/security/Pausable.sol";

8:   import {IERC20} from "@openzeppelin/contracts/token/ERC20/IERC20.sol";

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L4-L4


### [L&#x2011;13] `symbol()` is not a part of the ERC-20 standard
The `symbol()` function is not a part of the [ERC-20 standard](https://eips.ethereum.org/EIPS/eip-20), and was added later as an [optional extension](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/extensions/IERC20Metadata.sol). As such, some valid ERC20 tokens do not support this interface, so it is unsafe to blindly cast all tokens to this interface, and then call this function.

*There is one instance of this issue:*

```solidity
File: src/core/Oracles/ChainlinkOracle.sol

82:              price = getChainlinkPrice(getFeed(token.symbol()));

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkOracle.sol#L82-L82


### [L&#x2011;14] `decimals()` is not a part of the ERC-20 standard
The `decimals()` function is not a part of the [ERC-20 standard](https://eips.ethereum.org/EIPS/eip-20), and was added later as an [optional extension](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/extensions/IERC20Metadata.sol). As such, some valid ERC20 tokens do not support this interface, so it is unsafe to blindly cast all tokens to this interface, and then call this function.

*There is one instance of this issue:*

```solidity
File: src/core/Oracles/ChainlinkOracle.sol

85:          uint256 decimalDelta = uint256(18).sub(uint256(token.decimals()));

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkOracle.sol#L85-L85


### [L&#x2011;15] Sweeping may break accounting if tokens with multiple addresses are used
There have been [cases](https://blog.openzeppelin.com/compound-tusd-integration-issue-retrospective/) in the past where a token mistakenly had two addresses that could control its balance, and transfers using one address impacted the balance of the other. To protect against this potential scenario, sweep functions should ensure that the balance of the non-sweepable token does not change after the transfer of the swept tokens.

*There is one instance of this issue:*

```solidity
File: src/core/MErc20.sol

148      function sweepToken(EIP20NonStandardInterface token) override external {
149          require(msg.sender == admin, "MErc20::sweepToken: only admin can sweep tokens");
150          require(address(token) != underlying, "MErc20::sweepToken: can not sweep underlying token");
151      	uint256 balance = token.balanceOf(address(this));
152      	token.transfer(admin, balance);
153:     }

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20.sol#L148-L153


### [L&#x2011;16] Consider implementing two-step procedure for updating protocol addresses
A copy-paste error or a typo may end up bricking protocol functionality, or sending tokens to an address with no known private key. Consider implementing a two-step procedure for updating protocol addresses, where the recipient is set as pending, and must 'accept' the assignment by making an affirmative call. A straight forward way of doing this would be to have the target contracts implement [EIP-165](https://eips.ethereum.org/EIPS/eip-165), and to have the 'set' functions ensure that the recipient is of the right interface type.

*There is one instance of this issue:*

```solidity
File: src/core/Oracles/ChainlinkOracle.sol

173      function setAdmin(address newAdmin) external onlyAdmin {
174          address oldAdmin = admin;
175          admin = newAdmin;
176  
177          emit NewAdmin(oldAdmin, newAdmin);
178:     }

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkOracle.sol#L173-L178


### [L&#x2011;17] A year is not always 365 days
On leap years, the number of days is 366, so calculations during those years will return the wrong value

*There is one instance of this issue:*

```solidity
File: src/core/IRModels/JumpRateModel.sol

20:      uint public constant timestampsPerYear = 60 * 60 * 24 * 365;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/IRModels/JumpRateModel.sol#L20-L20

## Non-critical Issues


### [N&#x2011;01] Adding a `return` statement when the function defines a named return variable, is redundant
Once the return variable has been assigned (or has its default value), there is no need to explicitly return it at the end of the function, since it's returned automatically.

*There is one instance of this issue:*

```solidity
File: src/core/Oracles/ChainlinkOracle.sol

90:               return price;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkOracle.sol#L90


### [N&#x2011;02] `public` functions not called by the contract should be declared `external` instead
Contracts [are allowed](https://docs.soliditylang.org/en/latest/contracts.html#function-overriding) to override their parents' functions and change the visibility from `external` to `public`.

*There are 20 instances of this issue:*

```solidity
File: src/core/MultiRewardDistributor/MultiRewardDistributor.sol

340       function getGlobalSupplyIndex(
341           address mToken,
342           uint256 index
343:      ) public view returns (uint256) {

355       function getGlobalBorrowIndex(
356           address mToken,
357           uint256 index
358:      ) public view returns (uint256) {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L340-L343

```solidity
File: src/core/Comptroller.sol

516:      function getAccountLiquidity(address account) public view returns (uint, uint, uint) {

542       function getHypotheticalAccountLiquidity(
543           address account,
544           address mTokenModify,
545           uint redeemTokens,
546:          uint borrowAmount) public view returns (uint, uint, uint) {

665:      function _setPriceOracle(PriceOracle newOracle) public returns (uint) {

880:      function _setPauseGuardian(address newPauseGuardian) public returns (uint) {

901:      function _setRewardDistributor(MultiRewardDistributor newRewardDistributor) public {

911:      function _setMintPaused(MToken mToken, bool state) public returns (bool) {

921:      function _setBorrowPaused(MToken mToken, bool state) public returns (bool) {

931:      function _setTransferPaused(bool state) public returns (bool) {

940:      function _setSeizePaused(bool state) public returns (bool) {

949:      function _become(Unitroller unitroller) public {

1060:     function getAllMarkets() public view returns (MToken[] memory) {

1064:     function getBlockTimestamp() public view returns (uint) {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Comptroller.sol#L516

```solidity
File: src/core/Unitroller.sol

39:       function _setPendingImplementation(address newPendingImplementation) public returns (uint) {

59:       function _acceptImplementation() public returns (uint) {

86:       function _setPendingAdmin(address newPendingAdmin) public returns (uint) {

109:      function _acceptAdmin() public returns (uint) {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Unitroller.sol#L39

```solidity
File: src/core/Governance/TemporalGovernor.sol

237:      function executeProposal(bytes memory VAA) public whenNotPaused {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Governance/TemporalGovernor.sol#L237

```solidity
File: src/core/MErc20.sol

23        function initialize(address underlying_,
24                            ComptrollerInterface comptroller_,
25                            InterestRateModel interestRateModel_,
26                            uint initialExchangeRateMantissa_,
27                            string memory name_,
28                            string memory symbol_,
29:                           uint8 decimals_) public {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20.sol#L23-L29


### [N&#x2011;03] `constant`s should be defined rather than using magic numbers
Even [assembly](https://github.com/code-423n4/2022-05-opensea-seaport/blob/9d7ce4d08bf3c3010304a0476a785c70c0e90ae7/contracts/lib/TokenTransferrer.sol#L35-L39) can benefit from using readable constants instead of hex/numeric literals

*There are 31 instances of this issue:*

<details>
<summary>see instances</summary>


```solidity
File: src/core/MultiRewardDistributor/MultiRewardDistributor.sol

/// @audit 100e18
110:          emissionCap = 100e18;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L110

```solidity
File: src/core/Unitroller.sol

/// @audit 0x40
141:                let free_mem_ptr := mload(0x40)

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Unitroller.sol#L141

```solidity
File: src/core/Governance/TemporalGovernor.sol

/// @audit 96
130:          return bytes32(bytes20(addr)) >> 96;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Governance/TemporalGovernor.sol#L130

```solidity
File: src/core/MErc20.sol

/// @audit 32
199:                      returndatacopy(0, 0, 32)

/// @audit 32
198:                  case 32 {                      // This is a compliant ERC-20

/// @audit 32
234:                      returndatacopy(0, 0, 32)

/// @audit 32
233:                  case 32 {                     // This is a compliant ERC-20

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20.sol#L199

```solidity
File: src/core/MErc20Delegator.sol

/// @audit 0x20
459:                  revert(add(returnData, 0x20), returndatasize())

/// @audit 0x20
486:                  revert(add(returnData, 0x20), returndatasize())

/// @audit 0x40
503:              let free_mem_ptr := mload(0x40)

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20Delegator.sol#L459

```solidity
File: src/core/IRModels/WhitePaperInterestRateModel.sol

/// @audit 1e18
38:           baseRatePerTimestamp = baseRatePerYear.mul(1e18).div(timestampsPerYear).div(1e18);

/// @audit 1e18
39:           multiplierPerTimestamp = multiplierPerYear.mul(1e18).div(timestampsPerYear).div(1e18);

/// @audit 1e18
57:           return borrows.mul(1e18).div(cash.add(borrows).sub(reserves));

/// @audit 1e18
69:           return ur.mul(multiplierPerTimestamp).div(1e18).add(baseRatePerTimestamp);

/// @audit 1e18
81:           uint oneMinusReserveFactor = uint(1e18).sub(reserveFactorMantissa);

/// @audit 1e18
83:           uint rateToPool = borrowRate.mul(oneMinusReserveFactor).div(1e18);

/// @audit 1e18
84:           return utilizationRate(cash, borrows, reserves).mul(rateToPool).div(1e18);

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/IRModels/WhitePaperInterestRateModel.sol#L38

```solidity
File: src/core/IRModels/JumpRateModel.sol

/// @audit 1e18
53:           baseRatePerTimestamp = baseRatePerYear.mul(1e18).div(timestampsPerYear).div(1e18);

/// @audit 1e18
54:           multiplierPerTimestamp = multiplierPerYear.mul(1e18).div(timestampsPerYear).div(1e18);

/// @audit 1e18
55:           jumpMultiplierPerTimestamp = jumpMultiplierPerYear.mul(1e18).div(timestampsPerYear).div(1e18);

/// @audit 1e18
74:           return borrows.mul(1e18).div(cash.add(borrows).sub(reserves));

/// @audit 1e18
88:               return util.mul(multiplierPerTimestamp).div(1e18).add(baseRatePerTimestamp);

/// @audit 1e18
90:               uint normalRate = kink.mul(multiplierPerTimestamp).div(1e18).add(baseRatePerTimestamp);

/// @audit 1e18
92:               return excessUtil.mul(jumpMultiplierPerTimestamp).div(1e18).add(normalRate);

/// @audit 1e18
105:          uint oneMinusReserveFactor = uint(1e18).sub(reserveFactorMantissa);

/// @audit 1e18
107:          uint rateToPool = borrowRate.mul(oneMinusReserveFactor).div(1e18);

/// @audit 1e18
108:          return utilizationRate(cash, borrows, reserves).mul(rateToPool).div(1e18);

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/IRModels/JumpRateModel.sol#L53

```solidity
File: src/core/Oracles/ChainlinkCompositeOracle.sol

/// @audit 18
109:              expectedDecimals > uint8(0) && expectedDecimals <= uint8(18),

/// @audit 18
140:              expectedDecimals > uint8(0) && expectedDecimals <= uint8(18),

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkCompositeOracle.sol#L109

```solidity
File: src/core/Oracles/ChainlinkOracle.sol

/// @audit 18
85:           uint256 decimalDelta = uint256(18).sub(uint256(token.decimals()));

/// @audit 18
106:          uint256 decimalDelta = uint256(18).sub(feed.decimals());

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkOracle.sol#L85

</details>

### [N&#x2011;04] Event is not properly `indexed`
Index event fields make the field more quickly accessible [to off-chain tools](https://ethereum.stackexchange.com/questions/40396/can-somebody-please-explain-the-concept-of-event-indexing) that parse events. This is especially useful when it comes to filtering based on an address. However, note that each index field costs extra gas during emission, so it's not necessarily best to index the maximum allowed per event (three fields). Where applicable, each `event` should use three `indexed` fields if there are three or more fields, and gas usage is not particularly of concern for the events in question. If there are fewer than three applicable fields, all of the applicable fields should be indexed.

*There are 12 instances of this issue:*

```solidity
File: src/core/Comptroller.sol

20:       event MarketEntered(MToken mToken, address account);

23:       event MarketExited(MToken mToken, address account);

38:       event NewPauseGuardian(address oldPauseGuardian, address newPauseGuardian);

50:       event NewBorrowCapGuardian(address oldBorrowCapGuardian, address newBorrowCapGuardian);

56:       event NewSupplyCapGuardian(address oldSupplyCapGuardian, address newSupplyCapGuardian);

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Comptroller.sol#L20

```solidity
File: src/core/Unitroller.sol

16:       event NewPendingImplementation(address oldPendingImplementation, address newPendingImplementation);

21:       event NewImplementation(address oldImplementation, address newImplementation);

26:       event NewPendingAdmin(address oldPendingAdmin, address newPendingAdmin);

31:       event NewAdmin(address oldAdmin, address newAdmin);

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Unitroller.sol#L16

```solidity
File: src/core/Oracles/ChainlinkOracle.sol

30        event PricePosted(
31            address asset,
32            uint256 previousPriceMantissa,
33            uint256 requestedPriceMantissa,
34            uint256 newPriceMantissa
35:       );

38:       event NewAdmin(address oldAdmin, address newAdmin);

41:       event FeedSet(address feed, string symbol);

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkOracle.sol#L30-L35


### [N&#x2011;05] Duplicated `require()`/`revert()` checks should be refactored to a modifier or function
The compiler will inline the function, which will avoid `JUMP` instructions usually associated with functions

*There are 9 instances of this issue:*

```solidity
File: src/core/Comptroller.sol

960:          require(msg.sender == admin, "Unauthorized");

922:          require(markets[address(mToken)].isListed, "cannot pause a market that is not listed");

923:          require(msg.sender == pauseGuardian || msg.sender == admin, "only pause guardian and admin can pause");

924:          require(msg.sender == admin || state == true, "only admin can unpause");

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Comptroller.sol#L960

```solidity
File: src/core/Governance/TemporalGovernor.sol

167           require(
168               msg.sender == address(this),
169               "TemporalGovernor: Only this contract can update trusted senders"
170:          );

352:          require(valid, reason); /// ensure VAA parsing verification succeeded

370           require(
371               trustedSenders[vm.emitterChainId].contains(vm.emitterAddress), /// allow multiple per chainid
372               "TemporalGovernor: Invalid Emitter Address"
373:          );

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Governance/TemporalGovernor.sol#L167-L170

```solidity
File: src/core/MToken.sol

263:          require(accrueInterest() == uint(Error.NO_ERROR), "accrue interest failed");

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MToken.sol#L263

```solidity
File: src/core/Oracles/ChainlinkCompositeOracle.sol

139           require(
140               expectedDecimals > uint8(0) && expectedDecimals <= uint8(18),
141               "CLCOracle: Invalid expected decimals"
142:          );

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkCompositeOracle.sol#L139-L142


### [N&#x2011;06] Import declarations should import specific identifiers, rather than the whole file
Using import declarations of the form `import {<identifier_name>} from "some/file.sol"` avoids polluting the symbol namespace making flattened files smaller, and speeds up compilation (but does not save any gas)

*There are 27 instances of this issue:*

<details>
<summary>see instances</summary>


```solidity
File: src/core/Comptroller.sol

4:    import "./MToken.sol";

5:    import "./ErrorReporter.sol";

6:    import "./Oracles/PriceOracle.sol";

7:    import "./ComptrollerInterface.sol";

8:    import "./ComptrollerStorage.sol";

9:    import "./Unitroller.sol";

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Comptroller.sol#L4

```solidity
File: src/core/Unitroller.sol

4:    import "./ErrorReporter.sol";

5:    import "./ComptrollerStorage.sol";

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Unitroller.sol#L4

```solidity
File: src/core/MToken.sol

4:    import "./ComptrollerInterface.sol";

5:    import "./MTokenInterfaces.sol";

6:    import "./ErrorReporter.sol";

7:    import "./Exponential.sol";

8:    import "./EIP20Interface.sol";

9:    import "./IRModels/InterestRateModel.sol";

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MToken.sol#L4

```solidity
File: src/core/MErc20.sol

4:    import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

5:    import "./MToken.sol";

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20.sol#L4

```solidity
File: src/core/MErc20Delegator.sol

4:    import "./MTokenInterfaces.sol";

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20Delegator.sol#L4

```solidity
File: src/core/MErc20Delegate.sol

4:    import "./MErc20.sol";

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20Delegate.sol#L4

```solidity
File: src/core/IRModels/WhitePaperInterestRateModel.sol

4:    import "./InterestRateModel.sol";

5:    import "../SafeMath.sol";

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/IRModels/WhitePaperInterestRateModel.sol#L4

```solidity
File: src/core/IRModels/JumpRateModel.sol

4:    import "./InterestRateModel.sol";

5:    import "../SafeMath.sol";

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/IRModels/JumpRateModel.sol#L4

```solidity
File: src/core/Oracles/ChainlinkOracle.sol

4:    import "./PriceOracle.sol";

5:    import "../MErc20.sol";

6:    import "../EIP20Interface.sol";

7:    import  "../SafeMath.sol";

8:    import "./AggregatorV3Interface.sol";

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkOracle.sol#L4

</details>

### [N&#x2011;07] Events that mark critical parameter changes should contain both the old and the new value
This should especially be done if the new value is not required to be different from the old value

*There are 4 instances of this issue:*

```solidity
File: src/core/Governance/TemporalGovernor.sol

/// @audit setTrustedSenders()
151                   emit TrustedSenderUpdated(
152                       _trustedSenders[i].chainId,
153                       _trustedSenders[i].addr,
154                       true /// added to list
155:                  );

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Governance/TemporalGovernor.sol#L151-L155

```solidity
File: src/core/Oracles/ChainlinkOracle.sol

/// @audit setUnderlyingPrice()
123           emit PricePosted(
124               asset,
125               prices[asset],
126               underlyingPriceMantissa,
127               underlyingPriceMantissa
128:          );

/// @audit setDirectPrice()
136:          emit PricePosted(asset, prices[asset], price, price);

/// @audit setFeed()
149:          emit FeedSet(feed, symbol);

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkOracle.sol#L123-L128


### [N&#x2011;08] Constant redefined elsewhere
Consider defining in only one contract so that values cannot become out of sync when only one location is updated. A [cheap way](https://medium.com/coinmonks/gas-cost-of-solidity-library-functions-dbe0cedd4678) to store constants in a single location is to create an `internal constant` in a `library`. If the variable is a local cache of another contract's value, consider making the cache variable internal or private, which will require external users to query the contract with the source of truth, so that callers don't get out of sync.

*There is one instance of this issue:*

```solidity
File: src/core/IRModels/JumpRateModel.sol

/// @audit seen in src/core/IRModels/WhitePaperInterestRateModel.sol 
20:       uint public constant timestampsPerYear = 60 * 60 * 24 * 365;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/IRModels/JumpRateModel.sol#L20


### [N&#x2011;09] Inconsistent spacing in comments
Some lines use `// x` and some use `//x`. The instances below point out the usages that don't follow the majority, within each file

*There are 2 instances of this issue:*

```solidity
File: src/core/MultiRewardDistributor/MultiRewardDistributor.sol

839:              userSupplyIndex = initialIndexConstant; //_globalSupplyIndex;

878:              userBorrowIndex = initialIndexConstant; //userBorrowIndex = _globalBorrowIndex;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L839


### [N&#x2011;10] Lines are too long
Usually lines in source code are limited to [80](https://softwareengineering.stackexchange.com/questions/148677/why-is-80-characters-the-standard-limit-for-code-width) characters. Today's screens are much larger so it's reasonable to stretch this in some cases. The solidity style guide recommends a maximumum line length of [120 characters](https://docs.soliditylang.org/en/v0.8.17/style-guide.html#maximum-line-length), so the lines below should be split when they reach that length.

*There are 107 instances of this issue:*

<details>
<summary>see instances</summary>


```solidity
File: src/core/MultiRewardDistributor/MultiRewardDistributor.sol

904:       *      it's simply mToken.totalSupply(), while on the borrow side it's (mToken.totalBorrows() / mToken.borrowIndex())

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L904

```solidity
File: src/core/Comptroller.sol

59:       event NewRewardDistributor(MultiRewardDistributor oldRewardDistributor, MultiRewardDistributor newRewardDistributor);

274:          (Error err, , uint shortfall) = getHypotheticalAccountLiquidityInternal(redeemer, MToken(mToken), redeemTokens, 0);

292:      function redeemVerify(address mToken, address redeemer, uint redeemAmount, uint redeemTokens) override pure external {

344:          (Error err, , uint shortfall) = getHypotheticalAccountLiquidityInternal(borrower, MToken(mToken), 0, borrowAmount);

472:      function transferAllowed(address mToken, address src, address dst, uint transferTokens) override external returns (uint) {

517:          (Error err, uint liquidity, uint shortfall) = getHypotheticalAccountLiquidityInternal(account, MToken(address(0)), 0, 0);

547:          (Error err, uint liquidity, uint shortfall) = getHypotheticalAccountLiquidityInternal(account, MToken(mTokenModify), redeemTokens, borrowAmount);

578:              (oErr, vars.mTokenBalance, vars.borrowBalance, vars.exchangeRateMantissa) = asset.getAccountSnapshot(account);

599:              vars.sumBorrowPlusEffects = mul_ScalarTruncateAddUInt(vars.oraclePrice, vars.borrowBalance, vars.sumBorrowPlusEffects);

605:                  vars.sumBorrowPlusEffects = mul_ScalarTruncateAddUInt(vars.tokensToDenom, redeemTokens, vars.sumBorrowPlusEffects);

609:                  vars.sumBorrowPlusEffects = mul_ScalarTruncateAddUInt(vars.oraclePrice, borrowAmount, vars.sumBorrowPlusEffects);

629:      function liquidateCalculateSeizeTokens(address mTokenBorrowed, address mTokenCollateral, uint actualRepayAmount) override external view returns (uint, uint) {

802:        * @notice Set the given borrow caps for the given mToken markets. Borrowing that brings total borrows to or above borrow cap will revert.

803:        * @dev Admin or borrowCapGuardian function to set the borrow caps. A borrow cap of 0 corresponds to unlimited borrowing.

805:        * @param newBorrowCaps The new borrow cap values in underlying to be set. A value of 0 corresponds to unlimited borrowing.

808:      	require(msg.sender == admin || msg.sender == borrowCapGuardian, "only admin or borrow cap guardian can set borrow caps");

839:        * @notice Set the given supply caps for the given mToken markets. Supplying that brings total supplies to or above supply cap will revert.

840:        * @dev Admin or supplyCapGuardian function to set the supply caps. A supply cap of 0 corresponds to unlimited supplying.

842:        * @param newSupplyCaps The new supply cap values in underlying to be set. A value of 0 corresponds to unlimited supplying.

845:          require(msg.sender == admin || msg.sender == supplyCapGuardian, "only admin or supply cap guardian can set supply caps");

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Comptroller.sol#L59

```solidity
File: src/core/Unitroller.sol

19:         * @notice Emitted when pendingComptrollerImplementation is accepted, which means comptroller implementation is updated

82:         * @dev Admin function to begin change of admin. The newPendingAdmin must call `_acceptAdmin` to finalize the transfer.

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Unitroller.sol#L19

```solidity
File: src/core/MToken.sol

258:       * @notice Accrue interest to updated borrowIndex and then calculate account's borrow balance using the updated borrowIndex

426:              return failOpaque(Error.MATH_ERROR, FailureInfo.ACCRUE_INTEREST_SIMPLE_INTEREST_FACTOR_CALCULATION_FAILED, uint(mathErr));

431:              return failOpaque(Error.MATH_ERROR, FailureInfo.ACCRUE_INTEREST_ACCUMULATED_INTEREST_CALCULATION_FAILED, uint(mathErr));

436:              return failOpaque(Error.MATH_ERROR, FailureInfo.ACCRUE_INTEREST_NEW_TOTAL_BORROWS_CALCULATION_FAILED, uint(mathErr));

439:          (mathErr, totalReservesNew) = mulScalarTruncateAddUInt(Exp({mantissa: reserveFactorMantissa}), interestAccumulated, reservesPrior);

441:              return failOpaque(Error.MATH_ERROR, FailureInfo.ACCRUE_INTEREST_NEW_TOTAL_RESERVES_CALCULATION_FAILED, uint(mathErr));

446:              return failOpaque(Error.MATH_ERROR, FailureInfo.ACCRUE_INTEREST_NEW_BORROW_INDEX_CALCULATION_FAILED, uint(mathErr));

469:       * @return (uint, uint) An error code (0=success, otherwise a failure, see ErrorReporter.sol), and the actual mint amount.

496:       * @return (uint, uint) An error code (0=success, otherwise a failure, see ErrorReporter.sol), and the actual mint amount.

536:          (vars.mathErr, vars.mintTokens) = divScalarByExpTruncate(vars.actualMintAmount, Exp({mantissa: vars.exchangeRateMantissa}));

611:       * @param redeemTokensIn The number of mTokens to redeem into underlying (only one of redeemTokensIn or redeemAmountIn may be non-zero)

612:       * @param redeemAmountIn The number of underlying tokens to receive from redeeming mTokens (only one of redeemTokensIn or redeemAmountIn may be non-zero)

639:              (vars.mathErr, vars.redeemAmount) = mulScalarTruncate(Exp({mantissa: vars.exchangeRateMantissa}), vars.redeemTokens);

641:                  return failOpaque(Error.MATH_ERROR, FailureInfo.REDEEM_EXCHANGE_TOKENS_CALCULATION_FAILED, uint(vars.mathErr));

652:                  (vars.mathErr, vars.redeemAmount) = mulScalarTruncate(Exp({mantissa: vars.exchangeRateMantissa}), vars.redeemTokens);

654:                      return failOpaque(Error.MATH_ERROR, FailureInfo.REDEEM_EXCHANGE_TOKENS_CALCULATION_FAILED, uint(vars.mathErr));

659:                  (vars.mathErr, vars.redeemTokens) = divScalarByExpTruncate(redeemAmountIn, Exp({mantissa: vars.exchangeRateMantissa}));

661:                      return failOpaque(Error.MATH_ERROR, FailureInfo.REDEEM_EXCHANGE_AMOUNT_CALCULATION_FAILED, uint(vars.mathErr));

684:              return failOpaque(Error.MATH_ERROR, FailureInfo.REDEEM_NEW_TOTAL_SUPPLY_CALCULATION_FAILED, uint(vars.mathErr));

689:              return failOpaque(Error.MATH_ERROR, FailureInfo.REDEEM_NEW_ACCOUNT_BALANCE_CALCULATION_FAILED, uint(vars.mathErr));

776:              return failOpaque(Error.MATH_ERROR, FailureInfo.BORROW_ACCUMULATED_BALANCE_CALCULATION_FAILED, uint(vars.mathErr));

781:              return failOpaque(Error.MATH_ERROR, FailureInfo.BORROW_NEW_ACCOUNT_BORROW_BALANCE_CALCULATION_FAILED, uint(vars.mathErr));

786:              return failOpaque(Error.MATH_ERROR, FailureInfo.BORROW_NEW_TOTAL_BALANCE_CALCULATION_FAILED, uint(vars.mathErr));

819:       * @return (uint, uint) An error code (0=success, otherwise a failure, see ErrorReporter.sol), and the actual repayment amount.

835:       * @return (uint, uint) An error code (0=success, otherwise a failure, see ErrorReporter.sol), and the actual repayment amount.

863:       * @return (uint, uint) An error code (0=success, otherwise a failure, see ErrorReporter.sol), and the actual repayment amount.

869:              return (failOpaque(Error.COMPTROLLER_REJECTION, FailureInfo.REPAY_BORROW_COMPTROLLER_REJECTION, allowed), 0);

885:              return (failOpaque(Error.MATH_ERROR, FailureInfo.REPAY_BORROW_ACCUMULATED_BALANCE_CALCULATION_FAILED, uint(vars.mathErr)), 0);

940:       * @return (uint, uint) An error code (0=success, otherwise a failure, see ErrorReporter.sol), and the actual repayment amount.

942:      function liquidateBorrowInternal(address borrower, uint repayAmount, MTokenInterface mTokenCollateral) internal nonReentrant returns (uint, uint) {

945:              // accrueInterest emits logs on errors, but we still want to log the fact that an attempted liquidation failed

951:              // accrueInterest emits logs on errors, but we still want to log the fact that an attempted liquidation failed

966:       * @return (uint, uint) An error code (0=success, otherwise a failure, see ErrorReporter.sol), and the actual repayment amount.

968:      function liquidateBorrowFresh(address liquidator, address borrower, uint repayAmount, MTokenInterface mTokenCollateral) internal returns (uint, uint) {

970:          uint allowed = comptroller.liquidateBorrowAllowed(address(this), address(mTokenCollateral), liquidator, borrower, repayAmount);

1012:         (uint amountSeizeError, uint seizeTokens) = comptroller.liquidateCalculateSeizeTokens(address(this), address(mTokenCollateral), actualRepayAmount);

1034:         // comptroller.liquidateBorrowVerify(address(this), address(mTokenCollateral), liquidator, borrower, actualRepayAmount, seizeTokens);

1048:     function seize(address liquidator, address borrower, uint seizeTokens) override external nonReentrant returns (uint) {

1074:     function seizeInternal(address seizerToken, address liquidator, address borrower, uint seizeTokens) internal returns (uint) {

1095:             return failOpaque(Error.MATH_ERROR, FailureInfo.LIQUIDATE_SEIZE_BALANCE_DECREMENT_FAILED, uint(vars.mathErr));

1104:         vars.protocolSeizeAmount = mul_ScalarTruncate(Exp({mantissa: vars.exchangeRateMantissa}), vars.protocolSeizeTokens);

1111:             return failOpaque(Error.MATH_ERROR, FailureInfo.LIQUIDATE_SEIZE_BALANCE_INCREMENT_FAILED, uint(vars.mathErr));

1141:       * @dev Admin function to begin change of admin. The newPendingAdmin must call `_acceptAdmin` to finalize the transfer.

1222:             // accrueInterest emits logs on errors, but on top of that we want to log the fact that an attempted reserve factor change failed.

1266:             // accrueInterest emits logs on errors, but on top of that we want to log the fact that an attempted reduce reserves failed.

1279:      * @return (uint, uint) An error code (0=success, otherwise a failure (see ErrorReporter.sol for details)) and the actual amount added, net token fees

1329:             // accrueInterest emits logs on errors, but on top of that we want to log the fact that an attempted reduce reserves failed.

1394:             // accrueInterest emits logs on errors, but on top of that we want to log the fact that an attempted change of interest rate model failed

1446:             // accrueInterest emits logs on errors, but on top of that we want to log the fact that an attempted change of protocol seize share failed

1496:      * @dev Performs a transfer in, reverting upon failure. Returns the amount actually transferred to the protocol, in case of a fee.

1504:      *  If caller has checked protocol's balance, and verified it is >= amount, this should not revert in normal conditions.

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MToken.sol#L258

```solidity
File: src/core/MErc20.sol

139:      function liquidateBorrow(address borrower, uint repayAmount, MTokenInterface mTokenCollateral) override external returns (uint) {

145:       * @notice A public function to sweep accidental ERC-20 transfers to this contract. Tokens are sent to admin (timelock)

183:       *            See here: https://medium.com/coinmonks/missing-return-value-bug-at-least-130-tokens-affected-d67bf08521ca

216:       *      error code rather than reverting. If caller has not called checked protocol's balance, this may revert due to

217:       *      insufficient cash held in this contract. If caller has checked protocol's balance prior to this call, and verified

221:       *            See here: https://medium.com/coinmonks/missing-return-value-bug-at-least-130-tokens-affected-d67bf08521ca

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20.sol#L139

```solidity
File: src/core/MErc20Delegator.sol

39:           delegateTo(implementation_, abi.encodeWithSignature("initialize(address,address,address,uint256,string,string,uint8)",

61:       function _setImplementation(address implementation_, bool allowResign, bytes memory becomeImplementationData) override public {

129:          bytes memory data = delegateToImplementation(abi.encodeWithSignature("redeemUnderlying(uint256)", redeemAmount));

160:          bytes memory data = delegateToImplementation(abi.encodeWithSignature("repayBorrowBehalf(address,uint256)", borrower, repayAmount));

172:      function liquidateBorrow(address borrower, uint repayAmount, MTokenInterface mTokenCollateral) override external returns (uint) {

173:          bytes memory data = delegateToImplementation(abi.encodeWithSignature("liquidateBorrow(address,uint256,address)", borrower, repayAmount, mTokenCollateral));

196:          bytes memory data = delegateToImplementation(abi.encodeWithSignature("transferFrom(address,address,uint256)", src, dst, amount));

209:          bytes memory data = delegateToImplementation(abi.encodeWithSignature("approve(address,uint256)", spender, amount));

220:          bytes memory data = delegateToViewImplementation(abi.encodeWithSignature("allowance(address,address)", owner, spender));

252:          bytes memory data = delegateToViewImplementation(abi.encodeWithSignature("getAccountSnapshot(address)", account));

284:       * @notice Accrue interest to updated borrowIndex and then calculate account's borrow balance using the updated borrowIndex

299:          bytes memory data = delegateToViewImplementation(abi.encodeWithSignature("borrowBalanceStored(address)", account));

351:          bytes memory data = delegateToImplementation(abi.encodeWithSignature("seize(address,address,uint256)", liquidator, borrower, seizeTokens));

356:       * @notice A public function to sweep accidental ERC-20 transfers to this contract. Tokens are sent to admin (timelock)

368:        * @dev Admin function to begin change of admin. The newPendingAdmin must call `_acceptAdmin` to finalize the transfer.

373:          bytes memory data = delegateToImplementation(abi.encodeWithSignature("_setPendingAdmin(address)", newPendingAdmin));

383:          bytes memory data = delegateToImplementation(abi.encodeWithSignature("_setComptroller(address)", newComptroller));

393:          bytes memory data = delegateToImplementation(abi.encodeWithSignature("_setReserveFactor(uint256)", newReserveFactorMantissa));

434:          bytes memory data = delegateToImplementation(abi.encodeWithSignature("_setInterestRateModel(address)", newInterestRateModel));

444:          bytes memory data = delegateToImplementation(abi.encodeWithSignature("_setProtocolSeizeShare(uint256)", newProtocolSeizeShareMantissa));

483:          (bool success, bytes memory returnData) = address(this).staticcall(abi.encodeWithSignature("delegateToImplementation(bytes)", data));

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20Delegator.sol#L39

```solidity
File: src/core/IRModels/InterestRateModel.sol

29:       function getSupplyRate(uint cash, uint borrows, uint reserves, uint reserveFactorMantissa) virtual external view returns (uint);

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/IRModels/InterestRateModel.sol#L29

```solidity
File: src/core/IRModels/WhitePaperInterestRateModel.sol

80:       function getSupplyRate(uint cash, uint borrows, uint reserves, uint reserveFactorMantissa) override public view returns (uint) {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/IRModels/WhitePaperInterestRateModel.sol#L80

```solidity
File: src/core/IRModels/JumpRateModel.sol

15:       event NewInterestParams(uint baseRatePerTimestamp, uint multiplierPerTimestamp, uint jumpMultiplierPerTimestamp, uint kink);

104:      function getSupplyRate(uint cash, uint borrows, uint reserves, uint reserveFactorMantissa) override public view returns (uint) {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/IRModels/JumpRateModel.sol#L15

```solidity
File: src/core/Oracles/ChainlinkCompositeOracle.sol

145:          int256 scalingFactor = int256(10 ** uint256(expectedDecimals * 2)); /// calculate expected decimals for end quote

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkCompositeOracle.sol#L145

```solidity
File: src/core/Oracles/ChainlinkOracle.sol

62:           if (keccak256(abi.encodePacked(symbol)) == nativeToken) { /// @dev this branch should never get called as native tokens are not supported on this deployment

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkOracle.sol#L62

</details>

### [N&#x2011;11] Variable names that consist of all capital letters should be reserved for `constant`/`immutable` variables
If the variable needs to be different based on which class it comes from, a `view`/`pure` _function_ should be used instead (e.g. like [this](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/76eee35971c2541585e05cbf258510dda7b2fbc6/contracts/token/ERC20/extensions/draft-IERC20Permit.sol#L59)).

*There is one instance of this issue:*

```solidity
File: src/core/Governance/TemporalGovernor.sol

344:      function _executeProposal(bytes memory VAA, bool overrideDelay) private {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Governance/TemporalGovernor.sol#L344


### [N&#x2011;12] Non-library/interface files should use fixed compiler versions, not floating ones


*There are 2 instances of this issue:*

```solidity
File: src/core/Governance/TemporalGovernor.sol

2:    pragma solidity ^0.8.0;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Governance/TemporalGovernor.sol#L2

```solidity
File: src/core/Oracles/ChainlinkCompositeOracle.sol

2:    pragma solidity ^0.8.0;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkCompositeOracle.sol#L2


### [N&#x2011;13] Typos


*There are 6 instances of this issue:*

```solidity
File: src/core/Governance/TemporalGovernor.sol

/// @audit eash
73:           // Establish a list of trusted emitters from eash chain

/// @audit timstamp
244:          /// block.timstamp on a real chain will always be gt 0 and

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Governance/TemporalGovernor.sol#L73

```solidity
File: src/core/MToken.sol

/// @audit tather
1502:      * @dev Performs a transfer out, ideally returning an explanatory error code upon failure tather than reverting.

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MToken.sol#L1502

```solidity
File: src/core/MErc20Delegator.sol

/// @audit settor
48:           // New implementations always get set via the settor (post-initialize)

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20Delegator.sol#L48

```solidity
File: src/core/Oracles/ChainlinkCompositeOracle.sol

/// @audit compatability
27:       /// @dev this is also used for backwards compatability in the ChainlinkOracle.sol contract

/// @audit compatabililty
46:       /// interface for compatabililty with getChainlinkPrice function in ChainlinkOracle.sol

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkCompositeOracle.sol#L27


### [N&#x2011;14] File does not contain an SPDX Identifier


*There is one instance of this issue:*

```solidity
File: src/core/router/WETHRouter.sol

0:    pragma solidity 0.8.17;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/router/WETHRouter.sol#L0


### [N&#x2011;15] NatSpec `@param` is missing


*There are 35 instances of this issue:*

<details>
<summary>see instances</summary>


```solidity
File: src/core/MultiRewardDistributor/MultiRewardDistributor.sol

/// @audit Missing: '@param _mToken'
196       /// @notice A view to enumerate all configs for a given market, does not include index data
197       function getAllMarketConfigs(
198           MToken _mToken
199:      ) external view returns (MarketConfig[] memory) {

/// @audit Missing: '@param _mToken'
/// @audit Missing: '@param _emissionToken'
217       /// @notice A view to get a config for a specific market/emission token pair
218       function getConfigForMarket(
219           MToken _mToken,
220           address _emissionToken
221:      ) external view returns (MarketConfig memory) {

/// @audit Missing: '@param _user'
230       /// @notice A view to enumerate a user's rewards across all markets and all emission tokens
231       function getOutstandingRewardsForUser(
232           address _user
233:      ) external view returns (RewardWithMToken[] memory) {

/// @audit Missing: '@param _mToken'
/// @audit Missing: '@param _user'
255       /// @notice A view to enumerate a user's rewards across a specified market and all emission tokens for that market
256       function getOutstandingRewardsForUser(
257           MToken _mToken,
258           address _user
259:      ) public view returns (RewardInfo[] memory) {

/// @audit Missing: '@param _mToken'
/// @audit Missing: '@param _owner'
/// @audit Missing: '@param _emissionToken'
/// @audit Missing: '@param _supplyEmissionPerSec'
/// @audit Missing: '@param _borrowEmissionsPerSec'
/// @audit Missing: '@param _endTime'
378       /**
379        * @notice Add a new emission configuration for a specific market
380        * @dev Emission config must not already exist for the specified market (unique to the emission token)
381        */
382       function _addEmissionConfig(
383           MToken _mToken,
384           address _owner,
385           address _emissionToken,
386           uint256 _supplyEmissionPerSec,
387           uint256 _borrowEmissionsPerSec,
388           uint256 _endTime
389:      ) external onlyComptrollersAdmin {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L196-L199

```solidity
File: src/core/Comptroller.sol

/// @audit Missing: '@param account'
510       /**
511        * @notice Determine the current account liquidity wrt collateral requirements
512        * @return (possible error code (semi-opaque),
513                   account liquidity in excess of collateral requirements,
514        *          account shortfall below collateral requirements)
515        */
516:      function getAccountLiquidity(address account) public view returns (uint, uint, uint) {

/// @audit Missing: '@param account'
522       /**
523        * @notice Determine the current account liquidity wrt collateral requirements
524        * @return (possible error code,
525                   account liquidity in excess of collateral requirements,
526        *          account shortfall below collateral requirements)
527        */
528:      function getAccountLiquidityInternal(address account) internal view returns (Error, uint, uint) {

/// @audit Missing: '@param newOracle'
660       /**
661         * @notice Sets a new price oracle for the comptroller
662         * @dev Admin function to set a new price oracle
663         * @return uint 0=success, otherwise a failure (see ErrorReporter.sol for details)
664         */
665:      function _setPriceOracle(PriceOracle newOracle) public returns (uint) {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Comptroller.sol#L510-L516

```solidity
File: src/core/Unitroller.sol

/// @audit Missing: '@param newPendingImplementation'
38        /*** Admin Functions ***/
39:       function _setPendingImplementation(address newPendingImplementation) public returns (uint) {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Unitroller.sol#L38-L39

```solidity
File: src/core/Governance/TemporalGovernor.sol

/// @audit Missing: '@param VAA'
294       /// queue a proposal
295:      function _queueProposal(bytes memory VAA) private {

/// @audit Missing: '@param targets'
/// @audit Missing: '@param values'
/// @audit Missing: '@param calldatas'
411       /// @notice arity check for payload
412       function _sanityCheckPayload(
413           address[] memory targets,
414           uint256[] memory values,
415:          bytes[] memory calldatas

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Governance/TemporalGovernor.sol#L294-L295

```solidity
File: src/core/MToken.sol

/// @audit Missing: '@param borrower'
745       /**
746         * @notice Users borrow assets from the protocol to their own address
747         * @param borrowAmount The amount of the underlying asset to borrow
748         * @return uint 0=success, otherwise a failure (see ErrorReporter.sol for details)
749         */
750:      function borrowFresh(address payable borrower, uint borrowAmount) internal returns (uint) {

/// @audit Missing: '@param newComptroller'
1190      /**
1191        * @notice Sets a new comptroller for the market
1192        * @dev Admin function to set a new comptroller
1193        * @return uint 0=success, otherwise a failure (see ErrorReporter.sol for details)
1194        */
1195:     function _setComptroller(ComptrollerInterface newComptroller) override public returns (uint) {

/// @audit Missing: '@param newReserveFactorMantissa'
1214      /**
1215        * @notice accrues interest and sets a new reserve factor for the protocol using _setReserveFactorFresh
1216        * @dev Admin function to accrue interest and set a new reserve factor
1217        * @return uint 0=success, otherwise a failure (see ErrorReporter.sol for details)
1218        */
1219:     function _setReserveFactor(uint newReserveFactorMantissa) override external nonReentrant returns (uint) {

/// @audit Missing: '@param newReserveFactorMantissa'
1229      /**
1230        * @notice Sets a new reserve factor for the protocol (*requires fresh interest accrual)
1231        * @dev Admin function to set a new reserve factor
1232        * @return uint 0=success, otherwise a failure (see ErrorReporter.sol for details)
1233        */
1234:     function _setReserveFactorFresh(uint newReserveFactorMantissa) internal returns (uint) {

/// @audit Missing: '@param from'
/// @audit Missing: '@param amount'
1495      /**
1496       * @dev Performs a transfer in, reverting upon failure. Returns the amount actually transferred to the protocol, in case of a fee.
1497       *  This may revert due to insufficient balance or insufficient allowance.
1498       */
1499:     function doTransferIn(address from, uint amount) virtual internal returns (uint);

/// @audit Missing: '@param to'
/// @audit Missing: '@param amount'
1501      /**
1502       * @dev Performs a transfer out, ideally returning an explanatory error code upon failure tather than reverting.
1503       *  If caller has not called checked protocol's balance, may revert due to insufficient cash held in the contract.
1504       *  If caller has checked protocol's balance, and verified it is >= amount, this should not revert in normal conditions.
1505       */
1506:     function doTransferOut(address payable to, uint amount) virtual internal;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MToken.sol#L745-L750

```solidity
File: src/core/MErc20.sol

/// @audit Missing: '@param from'
/// @audit Missing: '@param amount'
176       /**
177        * @dev Similar to EIP20 transfer, except it handles a False result from `transferFrom` and reverts in that case.
178        *      This will revert due to insufficient balance or insufficient allowance.
179        *      This function returns the actual amount received,
180        *      which may be less than `amount` if there is a fee attached to the transfer.
181        *
182        *      Note: This wrapper safely handles non-standard ERC-20 tokens that do not return a value.
183        *            See here: https://medium.com/coinmonks/missing-return-value-bug-at-least-130-tokens-affected-d67bf08521ca
184        */
185:      function doTransferIn(address from, uint amount) virtual override internal returns (uint) {

/// @audit Missing: '@param to'
/// @audit Missing: '@param amount'
214       /**
215        * @dev Similar to EIP20 transfer, except it handles a False success from `transfer` and returns an explanatory
216        *      error code rather than reverting. If caller has not called checked protocol's balance, this may revert due to
217        *      insufficient cash held in this contract. If caller has checked protocol's balance prior to this call, and verified
218        *      it is >= amount, this should not revert in normal conditions.
219        *
220        *      Note: This wrapper safely handles non-standard ERC-20 tokens that do not return a value.
221        *            See here: https://medium.com/coinmonks/missing-return-value-bug-at-least-130-tokens-affected-d67bf08521ca
222        */
223:      function doTransferOut(address payable to, uint amount) virtual override internal {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20.sol#L176-L185

```solidity
File: src/core/MErc20Delegator.sol

/// @audit Missing: '@param newComptroller'
377       /**
378         * @notice Sets a new comptroller for the market
379         * @dev Admin function to set a new comptroller
380         * @return uint 0=success, otherwise a failure (see ErrorReporter.sol for details)
381         */
382:      function _setComptroller(ComptrollerInterface newComptroller) override public returns (uint) {

/// @audit Missing: '@param newReserveFactorMantissa'
387       /**
388         * @notice accrues interest and sets a new reserve factor for the protocol using _setReserveFactorFresh
389         * @dev Admin function to accrue interest and set a new reserve factor
390         * @return uint 0=success, otherwise a failure (see ErrorReporter.sol for details)
391         */
392:      function _setReserveFactor(uint newReserveFactorMantissa) override external returns (uint) {

/// @audit Missing: '@param newProtocolSeizeShareMantissa'
438       /**
439         * @notice accrues interest and sets a new protocol seize share for the protocol using _setProtocolSeizeShareFresh
440         * @dev Admin function to accrue interest and set a new protocol seize share
441         * @return uint 0=success, otherwise a failure (see ErrorReporter.sol for details)
442         */
443:      function _setProtocolSeizeShare(uint newProtocolSeizeShareMantissa) override external returns (uint) {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20Delegator.sol#L377-L382

</details>

### [N&#x2011;16] NatSpec `@return` argument is missing


*There are 28 instances of this issue:*

<details>
<summary>see instances</summary>


```solidity
File: src/core/MultiRewardDistributor/MultiRewardDistributor.sol

/// @audit Missing: '@return'
182        * @param _emissionToken The reward token address
183        */
184       function getCurrentOwner(
185           MToken _mToken,
186           address _emissionToken
187:      ) external view returns (address) {

/// @audit Missing: '@return'
196       /// @notice A view to enumerate all configs for a given market, does not include index data
197       function getAllMarketConfigs(
198           MToken _mToken
199:      ) external view returns (MarketConfig[] memory) {

/// @audit Missing: '@return'
217       /// @notice A view to get a config for a specific market/emission token pair
218       function getConfigForMarket(
219           MToken _mToken,
220           address _emissionToken
221:      ) external view returns (MarketConfig memory) {

/// @audit Missing: '@return'
230       /// @notice A view to enumerate a user's rewards across all markets and all emission tokens
231       function getOutstandingRewardsForUser(
232           address _user
233:      ) external view returns (RewardWithMToken[] memory) {

/// @audit Missing: '@return'
255       /// @notice A view to enumerate a user's rewards across a specified market and all emission tokens for that market
256       function getOutstandingRewardsForUser(
257           MToken _mToken,
258           address _user
259:      ) public view returns (RewardInfo[] memory) {

/// @audit Missing: '@return'
332       /// @notice A view to get the current emission caps
333:      function getCurrentEmissionCap() external view returns (uint256) {

/// @audit Missing: '@return'
338       /// @param mToken The market to get a config for
339       /// @param index The index of the config to get
340       function getGlobalSupplyIndex(
341           address mToken,
342           uint256 index
343:      ) public view returns (uint256) {

/// @audit Missing: '@return'
353       /// @param mToken The market to get a config for
354       /// @param index The index of the config to get
355       function getGlobalBorrowIndex(
356           address mToken,
357           uint256 index
358:      ) public view returns (uint256) {

/// @audit Missing: '@return'
825        * @param _supplier The address of the supplier
826        */
827       function calculateSupplyRewardsForUser(
828           MarketEmissionConfig storage _emissionConfig,
829           uint224 _globalSupplyIndex,
830           uint256 _supplierTokens,
831           address _supplier
832:      ) internal view returns (uint256) {

/// @audit Missing: '@return'
863        * @param _borrower The address of the supplier mToken balance and borrowed balance
864        */
865       function calculateBorrowRewardsForUser(
866           MarketEmissionConfig storage _emissionConfig,
867           uint224 _globalBorrowIndex,
868           Exp memory _marketBorrowIndex,
869           MTokenData memory _mTokenData,
870           address _borrower
871:      ) internal view returns (uint256) {

/// @audit Missing: '@return'
910        *        borrow side is (mToken.totalBorrows() / mToken.borrowIndex()).
911        */
912       function calculateNewIndex(
913           uint256 _emissionsPerSecond,
914           uint32 _currentTimestamp,
915           uint224 _currentIndex,
916           uint256 _rewardEndTime,
917           uint256 _denominator
918:      ) internal view returns (IndexUpdate memory) {

/// @audit Missing: '@return'
974        * @param _emissionToken The emission token to fetch a config for
975        */
976       function fetchConfigByEmissionToken(
977           MToken _mToken,
978           address _emissionToken
979:      ) internal view returns (MarketEmissionConfig storage) {

/// @audit Missing: '@return'
1212       * @param _rewardToken The reward token to send
1213       */
1214      function sendReward(
1215          address payable _user,
1216          uint256 _amount,
1217          address _rewardToken
1218:     ) internal nonReentrant returns (uint256) {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L182-L187

```solidity
File: src/core/Comptroller.sol

/// @audit Missing: '@return'
392        * @param repayAmount The amount of underlying being repaid
393        */
394       function liquidateBorrowAllowed(
395           address mTokenBorrowed,
396           address mTokenCollateral,
397           address liquidator,
398           address borrower,
399:          uint repayAmount) override external view returns (uint) {

/// @audit Missing: '@return'
432        * @param seizeTokens The number of collateral tokens to seize
433        */
434       function seizeAllowed(
435           address mTokenCollateral,
436           address mTokenBorrowed,
437           address liquidator,
438           address borrower,
439:          uint seizeTokens) override external returns (uint) {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Comptroller.sol#L392-L399

```solidity
File: src/core/Unitroller.sol

/// @audit Missing: '@return'
38        /*** Admin Functions ***/
39:       function _setPendingImplementation(address newPendingImplementation) public returns (uint) {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Unitroller.sol#L38-L39

```solidity
File: src/core/Governance/TemporalGovernor.sol

/// @audit Missing: '@return'
84        /// @param chainId The wormhole chain id to check
85        /// @param addr The address to check
86        function isTrustedSender(
87            uint16 chainId,
88            bytes32 addr
89:       ) public view returns (bool) {

/// @audit Missing: '@return'
94        /// @param chainId The wormhole chain id to check
95        /// @param addr The address to check
96        function isTrustedSender(
97            uint16 chainId,
98            address addr
99:       ) external view returns (bool) {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Governance/TemporalGovernor.sol#L84-L89

```solidity
File: src/core/MToken.sol

/// @audit Missing: '@return'
226        *  This exists mainly for inheriting test contracts to stub this result.
227        */
228:      function getBlockTimestamp() virtual internal view returns (uint) {

/// @audit Missing: '@return'
383        *   up to the current block and writes new checkpoint to storage.
384        */
385:      function accrueInterest() virtual override public returns (uint) {

/// @audit Missing: '@return'
1495      /**
1496       * @dev Performs a transfer in, reverting upon failure. Returns the amount actually transferred to the protocol, in case of a fee.
1497       *  This may revert due to insufficient balance or insufficient allowance.
1498       */
1499:     function doTransferIn(address from, uint amount) virtual internal returns (uint);

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MToken.sol#L226-L228

```solidity
File: src/core/MErc20.sol

/// @audit Missing: '@return'
176       /**
177        * @dev Similar to EIP20 transfer, except it handles a False result from `transferFrom` and reverts in that case.
178        *      This will revert due to insufficient balance or insufficient allowance.
179        *      This function returns the actual amount received,
180        *      which may be less than `amount` if there is a fee attached to the transfer.
181        *
182        *      Note: This wrapper safely handles non-standard ERC-20 tokens that do not return a value.
183        *            See here: https://medium.com/coinmonks/missing-return-value-bug-at-least-130-tokens-affected-d67bf08521ca
184        */
185:      function doTransferIn(address from, uint amount) virtual override internal returns (uint) {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20.sol#L176-L185

```solidity
File: src/core/MErc20Delegator.sol

/// @audit Missing: '@return'
334         *      up to the current block and writes new checkpoint to storage.
335         */
336:      function accrueInterest() override public returns (uint) {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20Delegator.sol#L334-L336

```solidity
File: src/core/Oracles/ChainlinkCompositeOracle.sol

/// @audit Missing: '@return'
45        /// @notice Get the latest price of a base/quote pair
46        /// interface for compatabililty with getChainlinkPrice function in ChainlinkOracle.sol
47        function latestRoundData()
48            external
49            view
50            returns (
51                uint80, /// roundId always 0, value unused in ChainlinkOracle.sol
52                int256, /// the composite price
53                uint256, /// startedAt always 0, value unused in ChainlinkOracle.sol
54                uint256, /// always block.timestamp
55:               uint80 /// answeredInRound always 0, value unused in ChainlinkOracle.sol

/// @audit Missing: '@return'
88        /// @param priceMultiplier The price of the quote token
89        /// @param scalingFactor The expected decimals of the derived price scaled up by 10 ** decimals
90        function calculatePrice(
91            int256 basePrice,
92            int256 priceMultiplier,
93            int256 scalingFactor
94:       ) public pure returns (uint256) {

/// @audit Missing: '@return'
101       /// @param expectedDecimals The expected decimals of the derived price
102       /// @dev always returns positive, otherwise reverts as comptroller only accepts positive oracle values
103       function getDerivedPrice(
104           address baseAddress,
105           address multiplierAddress,
106           uint8 expectedDecimals
107:      ) public view returns (uint256) {

/// @audit Missing: '@return'
164       /// @param oracleAddress The oracle address
165       /// @param expectedDecimals The amount of decimals the price should have
166       function getPriceAndScale(
167           address oracleAddress,
168           uint8 expectedDecimals
169:      ) public view returns (int256) {

/// @audit Missing: '@return'
178       /// returns the price and then the decimals of the given asset
179       /// reverts if price is 0 or if the oracle data is invalid
180       function getPriceAndDecimals(
181           address oracleAddress
182:      ) public view returns (int256, uint8) {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkCompositeOracle.sol#L45-L55

</details>

### [N&#x2011;17] Function definition modifier order does not follow Solidity style guide
See [this](https://docs.soliditylang.org/en/v0.8.19/style-guide.html#function-declaration) link for an explanation of the correct ordering

*There are 96 instances of this issue:*

<details>
<summary>see instances</summary>


```solidity
File: src/core/Comptroller.sol

/// @audit      function enterMarkets() override public returns () {          uint len = mTokens.length;            uint[] memory results = new uint[]();          for () {              MToken mToken = MToken();                results[i] = uint());          }            return results;      }  : override before visibility 
102:      function enterMarkets(address[] memory mTokens) override public returns (uint[] memory) {

/// @audit      function exitMarket() override external returns () {          MToken mToken = MToken();          /* Get sender tokensHeld and amountOwed underlying from the mToken */          () = mToken.getAccountSnapshot();          require(); // semi-opaque error code            /* Fail if the sender has a borrow balance */          if () {              return fail();          }            /* Fail if the sender is not permitted to redeem all of their tokens */          uint allowed = redeemAllowedInternal();          if () {              return failOpaque();          }            Market storage marketToExit = markets[address()];            /* Return true if the sender is not already ‘in’ the market */          if () {              return uint();          }            /* Set mToken account membership to false */          delete marketToExit.accountMembership[msg.sender];            /* Delete mToken from the account’s list of assets */          // load into memory for faster iteration          MToken[] memory userAssetList = accountAssets[msg.sender];          uint len = userAssetList.length;          uint assetIndex = len;          for () {              if () {                  assetIndex = i;                  break;              }          }            // We *must* have found the asset in the list or our redundant data structure is broken          assert();            // copy last item in list to location of item to be removed, reduce length by 1          MToken[] storage storedList = accountAssets[msg.sender];          storedList[assetIndex] = storedList[storedList.length - 1];          storedList.pop();            emit MarketExited();            return uint();      }  : override before visibility 
154:      function exitMarket(address mTokenAddress) override external returns (uint) {

/// @audit      function mintAllowed() override external returns () {          // Pausing is a very serious situation - we revert to sound the alarms          require();            // Shh - currently unused          mintAmount;            if () {              return uint();          }            uint supplyCap = supplyCaps[mToken];          // Supply cap of 0 corresponds to unlimited supplying          if () {              uint totalCash = MToken().getCash();              uint totalBorrows = MToken().totalBorrows();              uint totalReserves = MToken().totalReserves();              // totalSupplies = totalCash + totalBorrows - totalReserves              uint totalSupplies = sub_(), totalReserves);                uint nextTotalSupplies = add_();              require();          }            // Keep the flywheel moving          updateAndDistributeSupplierRewardsForToken();          return uint();      }  : override before visibility 
215:      function mintAllowed(address mToken, address minter, uint mintAmount) override external returns (uint) {

/// @audit      function redeemAllowed() override external returns () {          uint allowed = redeemAllowedInternal();          if ()) {              return allowed;          }            // Keep the flywheel moving          updateAndDistributeSupplierRewardsForToken();            return uint();      }  : override before visibility 
251:      function redeemAllowed(address mToken, address redeemer, uint redeemTokens) override external returns (uint) {

/// @audit      function redeemVerify() override pure external {          // Shh - currently unused          mToken;          redeemer;            // Require tokens is zero or amount is also zero          if () {              revert();          }      }  : override before visibility 
292:      function redeemVerify(address mToken, address redeemer, uint redeemAmount, uint redeemTokens) override pure external {

/// @audit      function borrowAllowed() override external returns () {          // Pausing is a very serious situation - we revert to sound the alarms          require();            if () {              return uint();          }            if () {              // only mTokens may call borrowAllowed if borrower not in market              require();                // attempt to add borrower to the market              Error addToMarketErr = addToMarketInternal(), borrower);              if () {                  return uint();              }                // it should be impossible to break the important invariant              assert();          }            if ()) == 0) {              return uint();          }            uint borrowCap = borrowCaps[mToken];          // Borrow cap of 0 corresponds to unlimited borrowing          if () {              uint totalBorrows = MToken().totalBorrows();              uint nextTotalBorrows = add_();              require();          }            () = getHypotheticalAccountLiquidityInternal(), 0, borrowAmount);          if () {              return uint();          }          if () {              return uint();          }            // Keep the flywheel moving          updateAndDistributeBorrowerRewardsForToken();            return uint();      }  : override before visibility 
310:      function borrowAllowed(address mToken, address borrower, uint borrowAmount) override external returns (uint) {

/// @audit      function repayBorrowAllowed() override external returns () {          // Shh - currently unused          payer;          borrower;          repayAmount;            if () {              return uint();          }            // Keep the flywheel moving          updateAndDistributeBorrowerRewardsForToken();            return uint();      }  : override before visibility 
366       function repayBorrowAllowed(
367           address mToken,
368           address payer,
369           address borrower,
370:          uint repayAmount) override external returns (uint) {

/// @audit      function liquidateBorrowAllowed() override external view returns () {          // Shh - currently unused          liquidator;            if () {              return uint();          }            /* The borrower must have shortfall in order to be liquidatable */          () = getAccountLiquidityInternal();          if () {              return uint();          }          if () {              return uint();          }            /* The liquidator may not repay more than what is allowed by the closeFactor */          uint borrowBalance = MToken().borrowBalanceStored();          uint maxClose = mul_ScalarTruncate(), borrowBalance);          if () {              return uint();          }            return uint();      }  : override before visibility 
394       function liquidateBorrowAllowed(
395           address mTokenBorrowed,
396           address mTokenCollateral,
397           address liquidator,
398           address borrower,
399:          uint repayAmount) override external view returns (uint) {

/// @audit      function seizeAllowed() override external returns () {          // Pausing is a very serious situation - we revert to sound the alarms          require();            // Shh - currently unused          seizeTokens;            if () {              return uint();          }            if ().comptroller() != MToken().comptroller()) {              return uint();          }            // Keep the flywheel moving          // Note: We don't update borrower indices here because as part of liquidations          //       repayBorrowFresh is called, which in turn calls `borrowAllow`, which updates          //       the liquidated borrower's indices.          updateAndDistributeSupplierRewardsForToken();          updateAndDistributeSupplierRewardsForToken();            return uint();      }  : override before visibility 
434       function seizeAllowed(
435           address mTokenCollateral,
436           address mTokenBorrowed,
437           address liquidator,
438           address borrower,
439:          uint seizeTokens) override external returns (uint) {

/// @audit      function transferAllowed() override external returns () {          // Pausing is a very serious situation - we revert to sound the alarms          require();            // Currently the only consideration is whether or not          //  the src is allowed to redeem this many tokens          uint allowed = redeemAllowedInternal();          if ()) {              return allowed;          }            // Keep the flywheel moving          updateAndDistributeSupplierRewardsForToken();          updateAndDistributeSupplierRewardsForToken();            return uint();      }  : override before visibility 
472:      function transferAllowed(address mToken, address src, address dst, uint transferTokens) override external returns (uint) {

/// @audit      function liquidateCalculateSeizeTokens() override external view returns () {          /* Read oracle prices for borrowed and collateral markets */          uint priceBorrowedMantissa = oracle.getUnderlyingPrice());          uint priceCollateralMantissa = oracle.getUnderlyingPrice());          if () {              return (), 0);          }            /*           * Get the exchange rate and calculate the number of collateral tokens to seize:           *  seizeAmount = actualRepayAmount * liquidationIncentive * priceBorrowed / priceCollateral           *  seizeTokens = seizeAmount / exchangeRate           *   = actualRepayAmount * () / ()           */          uint exchangeRateMantissa = MToken().exchangeRateStored(); // Note: reverts on error          uint seizeTokens;          Exp memory numerator;          Exp memory denominator;          Exp memory ratio;            numerator = mul_(), Exp());          denominator = mul_(), Exp());          ratio = div_();            seizeTokens = mul_ScalarTruncate();            return (), seizeTokens);      }  : override before visibility 
629:      function liquidateCalculateSeizeTokens(address mTokenBorrowed, address mTokenCollateral, uint actualRepayAmount) override external view returns (uint, uint) {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Comptroller.sol#L102

```solidity
File: src/core/MToken.sol

/// @audit      function transfer() override external nonReentrant returns () {          return transferTokens() == uint();      }  : override before visibility 
136:      function transfer(address dst, uint256 amount) override external nonReentrant returns (bool) {

/// @audit      function transferFrom() override external nonReentrant returns () {          return transferTokens() == uint();      }  : override before visibility 
147:      function transferFrom(address src, address dst, uint256 amount) override external nonReentrant returns (bool) {

/// @audit      function approve() override external returns () {          address src = msg.sender;          transferAllowances[src][spender] = amount;          emit Approval();          return true;      }  : override before visibility 
159:      function approve(address spender, uint256 amount) override external returns (bool) {

/// @audit      function allowance() override external view returns () {          return transferAllowances[owner][spender];      }  : override before visibility 
172:      function allowance(address owner, address spender) override external view returns (uint256) {

/// @audit      function balanceOf() override external view returns () {          return accountTokens[owner];      }  : override before visibility 
181:      function balanceOf(address owner) override external view returns (uint256) {

/// @audit      function balanceOfUnderlying() override external returns () {          Exp memory exchangeRate = Exp()});          () = mulScalarTruncate();          require();          return balance;      }  : override before visibility 
191:      function balanceOfUnderlying(address owner) override external returns (uint) {

/// @audit      function getAccountSnapshot() override external view returns () {          uint mTokenBalance = accountTokens[account];          uint borrowBalance;          uint exchangeRateMantissa;            MathError mErr;            () = borrowBalanceStoredInternal();          if () {              return (), 0, 0, 0);          }            () = exchangeRateStoredInternal();          if () {              return (), 0, 0, 0);          }            return (), mTokenBalance, borrowBalance, exchangeRateMantissa);      }  : override before visibility 
204:      function getAccountSnapshot(address account) override external view returns (uint, uint, uint, uint) {

/// @audit      function getBlockTimestamp() virtual internal view returns () {          return block.timestamp;      }  : virtual before visibility 
228:      function getBlockTimestamp() virtual internal view returns (uint) {

/// @audit      function borrowRatePerTimestamp() override external view returns () {          return interestRateModel.getBorrowRate(), totalBorrows, totalReserves);      }  : override before visibility 
236:      function borrowRatePerTimestamp() override external view returns (uint) {

/// @audit      function supplyRatePerTimestamp() override external view returns () {          return interestRateModel.getSupplyRate(), totalBorrows, totalReserves, reserveFactorMantissa);      }  : override before visibility 
244:      function supplyRatePerTimestamp() override external view returns (uint) {

/// @audit      function totalBorrowsCurrent() override external nonReentrant returns () {          require() == uint(), "accrue interest failed");          return totalBorrows;      }  : override before visibility 
252:      function totalBorrowsCurrent() override external nonReentrant returns (uint) {

/// @audit      function borrowBalanceCurrent() override external nonReentrant returns () {          require() == uint(), "accrue interest failed");          return borrowBalanceStored();      }  : override before visibility 
262:      function borrowBalanceCurrent(address account) override external nonReentrant returns (uint) {

/// @audit      function borrowBalanceStored() override public view returns () {          () = borrowBalanceStoredInternal();          require();          return result;      }  : override before visibility 
272:      function borrowBalanceStored(address account) override public view returns (uint) {

/// @audit      function exchangeRateCurrent() override public nonReentrant returns () {          require() == uint(), "accrue interest failed");          return exchangeRateStored();      }  : override before visibility 
319:      function exchangeRateCurrent() override public nonReentrant returns (uint) {

/// @audit      function exchangeRateStored() override public view returns () {          () = exchangeRateStoredInternal();          require();          return result;      }  : override before visibility 
329:      function exchangeRateStored() override public view returns (uint) {

/// @audit      function exchangeRateStoredInternal() virtual internal view returns () {          uint _totalSupply = totalSupply;          if () {              /*               * If there are no tokens minted:               *  exchangeRate = initialExchangeRate               */              return ();          } else {              /*               * Otherwise:               *  exchangeRate = () / totalSupply               */              uint totalCash = getCashPrior();              uint cashPlusBorrowsMinusReserves;              Exp memory exchangeRate;              MathError mathErr;                () = addThenSubUInt();              if () {                  return ();              }                () = getExp();              if () {                  return ();              }                return ();          }      }  : virtual before visibility 
340:      function exchangeRateStoredInternal() virtual internal view returns (MathError, uint) {

/// @audit      function getCash() override external view returns () {          return getCashPrior();      }  : override before visibility 
376:      function getCash() override external view returns (uint) {

/// @audit      function accrueInterest() virtual override public returns () {          /* Remember the initial block timestamp */          uint currentBlockTimestamp = getBlockTimestamp();          uint accrualBlockTimestampPrior = accrualBlockTimestamp;            /* Short-circuit accumulating 0 interest */          if () {              return uint();          }            /* Read the previous values out of storage */          uint cashPrior = getCashPrior();          uint borrowsPrior = totalBorrows;          uint reservesPrior = totalReserves;          uint borrowIndexPrior = borrowIndex;            /* Calculate the current borrow interest rate */          uint borrowRateMantissa = interestRateModel.getBorrowRate();          require();            /* Calculate the number of blocks elapsed since the last accrual */          () = subUInt();          require();            /*           * Calculate the interest accumulated into borrows and reserves and the new index:           *  simpleInterestFactor = borrowRate * blockDelta           *  interestAccumulated = simpleInterestFactor * totalBorrows           *  totalBorrowsNew = interestAccumulated + totalBorrows           *  totalReservesNew = interestAccumulated * reserveFactor + totalReserves           *  borrowIndexNew = simpleInterestFactor * borrowIndex + borrowIndex           */            Exp memory simpleInterestFactor;          uint interestAccumulated;          uint totalBorrowsNew;          uint totalReservesNew;          uint borrowIndexNew;            () = mulScalar(), blockDelta);          if () {              return failOpaque());          }            () = mulScalarTruncate();          if () {              return failOpaque());          }            () = addUInt();          if () {              return failOpaque());          }            () = mulScalarTruncateAddUInt(), interestAccumulated, reservesPrior);          if () {              return failOpaque());          }            () = mulScalarTruncateAddUInt();          if () {              return failOpaque());          }            /////////////////////////          // EFFECTS & INTERACTIONS          // ()            /* We write the previously calculated values into storage */          accrualBlockTimestamp = currentBlockTimestamp;          borrowIndex = borrowIndexNew;          totalBorrows = totalBorrowsNew;          totalReserves = totalReservesNew;            /* We emit an AccrueInterest event */          emit AccrueInterest();            return uint();      }  : virtual before visibility 
385:      function accrueInterest() virtual override public returns (uint) {

/// @audit      function seize() override external nonReentrant returns () {          return seizeInternal();      }  : override before visibility 
1048:     function seize(address liquidator, address borrower, uint seizeTokens) override external nonReentrant returns (uint) {

/// @audit      function _setPendingAdmin() override external returns () {          // Check caller = admin          if () {              return fail();          }            // Save current value, if any, for inclusion in log          address oldPendingAdmin = pendingAdmin;            // Store pendingAdmin with value newPendingAdmin          pendingAdmin = newPendingAdmin;            // Emit NewPendingAdmin()          emit NewPendingAdmin();            return uint();      }  : override before visibility 
1145:     function _setPendingAdmin(address payable newPendingAdmin) override external returns (uint) {

/// @audit      function _acceptAdmin() override external returns () {          // Check caller is pendingAdmin and pendingAdmin ≠ address()          if ()) {              return fail();          }            // Save current values for inclusion in log          address oldAdmin = admin;          address oldPendingAdmin = pendingAdmin;            // Store admin with value pendingAdmin          admin = pendingAdmin;            // Clear the pending value          pendingAdmin = payable());            emit NewAdmin();          emit NewPendingAdmin();            return uint();      }  : override before visibility 
1168:     function _acceptAdmin() override external returns (uint) {

/// @audit      function _setComptroller() override public returns () {          // Check caller is admin          if () {              return fail();          }            ComptrollerInterface oldComptroller = comptroller;          // Ensure invoke comptroller.isComptroller() returns true          require(), "marker method returned false");            // Set market's comptroller to newComptroller          comptroller = newComptroller;            // Emit NewComptroller()          emit NewComptroller();            return uint();      }  : override before visibility 
1195:     function _setComptroller(ComptrollerInterface newComptroller) override public returns (uint) {

/// @audit      function _setReserveFactor() override external nonReentrant returns () {          uint error = accrueInterest();          if ()) {              // accrueInterest emits logs on errors, but on top of that we want to log the fact that an attempted reserve factor change failed.              return fail(), FailureInfo.SET_RESERVE_FACTOR_ACCRUE_INTEREST_FAILED);          }          // _setReserveFactorFresh emits reserve-factor-specific logs on errors, so we don't need to.          return _setReserveFactorFresh();      }  : override before visibility 
1219:     function _setReserveFactor(uint newReserveFactorMantissa) override external nonReentrant returns (uint) {

/// @audit      function _reduceReserves() override external nonReentrant returns () {          uint error = accrueInterest();          if ()) {              // accrueInterest emits logs on errors, but on top of that we want to log the fact that an attempted reduce reserves failed.              return fail(), FailureInfo.REDUCE_RESERVES_ACCRUE_INTEREST_FAILED);          }          // _reduceReservesFresh emits reserve-reduction-specific logs on errors, so we don't need to.          return _reduceReservesFresh();      }  : override before visibility 
1326:     function _reduceReserves(uint reduceAmount) override external nonReentrant returns (uint) {

/// @audit      function _setInterestRateModel() override public returns () {          uint error = accrueInterest();          if ()) {              // accrueInterest emits logs on errors, but on top of that we want to log the fact that an attempted change of interest rate model failed              return fail(), FailureInfo.SET_INTEREST_RATE_MODEL_ACCRUE_INTEREST_FAILED);          }          // _setInterestRateModelFresh emits interest-rate-model-update-specific logs on errors, so we don't need to.          return _setInterestRateModelFresh();      }  : override before visibility 
1391:     function _setInterestRateModel(InterestRateModel newInterestRateModel) override public returns (uint) {

/// @audit      function _setProtocolSeizeShare() override external nonReentrant returns () {          uint error = accrueInterest();          if ()) {              // accrueInterest emits logs on errors, but on top of that we want to log the fact that an attempted change of protocol seize share failed              return fail(), FailureInfo.SET_PROTOCOL_SEIZE_SHARE_ACCRUE_INTEREST_FAILED);          }          // _setProtocolSeizeShareFresh emits protocol-seize-share-update-specific logs on errors, so we don't need to.          return _setProtocolSeizeShareFresh();      }  : override before visibility 
1443:     function _setProtocolSeizeShare(uint newProtocolSeizeShareMantissa) override external nonReentrant returns (uint) {

/// @audit      function getCashPrior() virtual internal view returns ();  : virtual before visibility 
1493:     function getCashPrior() virtual internal view returns (uint);

/// @audit      function doTransferIn() virtual internal returns ();  : virtual before visibility 
1499:     function doTransferIn(address from, uint amount) virtual internal returns (uint);

/// @audit      function doTransferOut() virtual internal;  : virtual before visibility 
1506:     function doTransferOut(address payable to, uint amount) virtual internal;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MToken.sol#L136

```solidity
File: src/core/MErc20.sol

/// @audit      function mint() override external returns () {          () = mintInternal();          return err;      }  : override before visibility 
46:       function mint(uint mintAmount) override external returns (uint) {

/// @audit      function mintWithPermit() override external returns () {          IERC20Permit token = IERC20Permit();            // Go submit our pre-approval signature data to the underlying token, but          // explicitly fail if there is an issue.          SafeERC20.safePermit(),              mintAmount, deadline,              v, r, s          );            () = mintInternal();          return err;      }  : override before visibility 
61        function mintWithPermit(
62            uint mintAmount,
63            uint deadline,
64            uint8 v, bytes32 r, bytes32 s
65:       ) override external returns (uint) {

/// @audit      function redeem() override external returns () {          return redeemInternal();      }  : override before visibility 
87:       function redeem(uint redeemTokens) override external returns (uint) {

/// @audit      function redeemUnderlying() override external returns () {          return redeemUnderlyingInternal();      }  : override before visibility 
97:       function redeemUnderlying(uint redeemAmount) override external returns (uint) {

/// @audit      function borrow() override external returns () {          return borrowInternal();      }  : override before visibility 
106:      function borrow(uint borrowAmount) override external returns (uint) {

/// @audit      function repayBorrow() override external returns () {          () = repayBorrowInternal();          return err;      }  : override before visibility 
115:      function repayBorrow(uint repayAmount) override external returns (uint) {

/// @audit      function repayBorrowBehalf() override external returns () {          () = repayBorrowBehalfInternal();          return err;      }  : override before visibility 
126:      function repayBorrowBehalf(address borrower, uint repayAmount) override external returns (uint) {

/// @audit      function liquidateBorrow() override external returns () {          () = liquidateBorrowInternal();          return err;      }  : override before visibility 
139:      function liquidateBorrow(address borrower, uint repayAmount, MTokenInterface mTokenCollateral) override external returns (uint) {

/// @audit      function sweepToken() override external {          require();          require() != underlying, "MErc20::sweepToken: can not sweep underlying token");      	uint256 balance = token.balanceOf());      	token.transfer();      }  : override before visibility 
148:      function sweepToken(EIP20NonStandardInterface token) override external {

/// @audit      function _addReserves() override external returns () {          return _addReservesInternal();      }  : override before visibility 
160:      function _addReserves(uint addAmount) override external returns (uint) {

/// @audit      function getCashPrior() virtual override internal view returns () {          EIP20Interface token = EIP20Interface();          return token.balanceOf());      }  : virtual before visibility 
171:      function getCashPrior() virtual override internal view returns (uint) {

/// @audit      function doTransferIn() virtual override internal returns () {          // Read from storage once          address underlying_ = underlying;          EIP20NonStandardInterface token = EIP20NonStandardInterface();          uint balanceBefore = EIP20Interface().balanceOf());          token.transferFrom(), amount);            bool success;          assembly {              switch returndatasize()                  case 0 {                       // This is a non-standard ERC-20                      success := not()          // set success to true                  }                  case 32 {                      // This is a compliant ERC-20                      returndatacopy()                      success := mload()        // Set `success = returndata` of external call                  }                  default {                      // This is an excessively non-compliant ERC-20, revert.                      revert()                  }          }          require();            // Calculate the amount that was *actually* transferred          uint balanceAfter = EIP20Interface().balanceOf());          require();          return balanceAfter - balanceBefore;   // underflow already checked above, just subtract      }  : virtual before visibility 
185:      function doTransferIn(address from, uint amount) virtual override internal returns (uint) {

/// @audit      function doTransferOut() virtual override internal {          EIP20NonStandardInterface token = EIP20NonStandardInterface();          token.transfer();            bool success;          assembly {              switch returndatasize()                  case 0 {                      // This is a non-standard ERC-20                      success := not()          // set success to true                  }                  case 32 {                     // This is a compliant ERC-20                      returndatacopy()                      success := mload()        // Set `success = returndata` of override external call                  }                  default {                     // This is an excessively non-compliant ERC-20, revert.                      revert()                  }          }          require();      }  : virtual before visibility 
223:      function doTransferOut(address payable to, uint amount) virtual override internal {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20.sol#L46

```solidity
File: src/core/MErc20Delegator.sol

/// @audit      function _setImplementation() override public {          require();            if () {              delegateToImplementation()"));          }            address oldImplementation = implementation;          implementation = implementation_;            delegateToImplementation()", becomeImplementationData));            emit NewImplementation();      }  : override before visibility 
61:       function _setImplementation(address implementation_, bool allowResign, bytes memory becomeImplementationData) override public {

/// @audit      function mint() override external returns () {          bytes memory data = delegateToImplementation()", mintAmount));          return abi.decode());      }  : override before visibility 
82:       function mint(uint mintAmount) override external returns (uint) {

/// @audit      function mintWithPermit() override external returns () {          bytes memory data = delegateToImplementation()",                  mintAmount, deadline, v, r, s              )          );          return abi.decode());      }  : override before visibility 
97        function mintWithPermit(
98            uint mintAmount,
99            uint deadline,
100           uint8 v, bytes32 r, bytes32 s
101:      ) override external returns (uint) {

/// @audit      function redeem() override external returns () {          bytes memory data = delegateToImplementation()", redeemTokens));          return abi.decode());      }  : override before visibility 
117:      function redeem(uint redeemTokens) override external returns (uint) {

/// @audit      function redeemUnderlying() override external returns () {          bytes memory data = delegateToImplementation()", redeemAmount));          return abi.decode());      }  : override before visibility 
128:      function redeemUnderlying(uint redeemAmount) override external returns (uint) {

/// @audit      function borrow() override external returns () {          bytes memory data = delegateToImplementation()", borrowAmount));          return abi.decode());      }  : override before visibility 
138:      function borrow(uint borrowAmount) override external returns (uint) {

/// @audit      function repayBorrow() override external returns () {          bytes memory data = delegateToImplementation()", repayAmount));          return abi.decode());      }  : override before visibility 
148:      function repayBorrow(uint repayAmount) override external returns (uint) {

/// @audit      function repayBorrowBehalf() override external returns () {          bytes memory data = delegateToImplementation()", borrower, repayAmount));          return abi.decode());      }  : override before visibility 
159:      function repayBorrowBehalf(address borrower, uint repayAmount) override external returns (uint) {

/// @audit      function liquidateBorrow() override external returns () {          bytes memory data = delegateToImplementation()", borrower, repayAmount, mTokenCollateral));          return abi.decode());      }  : override before visibility 
172:      function liquidateBorrow(address borrower, uint repayAmount, MTokenInterface mTokenCollateral) override external returns (uint) {

/// @audit      function transfer() override external returns () {          bytes memory data = delegateToImplementation()", dst, amount));          return abi.decode());      }  : override before visibility 
183:      function transfer(address dst, uint amount) override external returns (bool) {

/// @audit      function transferFrom() override external returns () {          bytes memory data = delegateToImplementation()", src, dst, amount));          return abi.decode());      }  : override before visibility 
195:      function transferFrom(address src, address dst, uint256 amount) override external returns (bool) {

/// @audit      function approve() override external returns () {          bytes memory data = delegateToImplementation()", spender, amount));          return abi.decode());      }  : override before visibility 
208:      function approve(address spender, uint256 amount) override external returns (bool) {

/// @audit      function allowance() override external view returns () {          bytes memory data = delegateToViewImplementation()", owner, spender));          return abi.decode());      }  : override before visibility 
219:      function allowance(address owner, address spender) override external view returns (uint) {

/// @audit      function balanceOf() override external view returns () {          bytes memory data = delegateToViewImplementation()", owner));          return abi.decode());      }  : override before visibility 
229:      function balanceOf(address owner) override external view returns (uint) {

/// @audit      function balanceOfUnderlying() override external returns () {          bytes memory data = delegateToImplementation()", owner));          return abi.decode());      }  : override before visibility 
240:      function balanceOfUnderlying(address owner) override external returns (uint) {

/// @audit      function getAccountSnapshot() override external view returns () {          bytes memory data = delegateToViewImplementation()", account));          return abi.decode());      }  : override before visibility 
251:      function getAccountSnapshot(address account) override external view returns (uint, uint, uint, uint) {

/// @audit      function borrowRatePerTimestamp() override external view returns () {          bytes memory data = delegateToViewImplementation()"));          return abi.decode());      }  : override before visibility 
260:      function borrowRatePerTimestamp() override external view returns (uint) {

/// @audit      function supplyRatePerTimestamp() override external view returns () {          bytes memory data = delegateToViewImplementation()"));          return abi.decode());      }  : override before visibility 
269:      function supplyRatePerTimestamp() override external view returns (uint) {

/// @audit      function totalBorrowsCurrent() override external returns () {          bytes memory data = delegateToImplementation()"));          return abi.decode());      }  : override before visibility 
278:      function totalBorrowsCurrent() override external returns (uint) {

/// @audit      function borrowBalanceCurrent() override external returns () {          bytes memory data = delegateToImplementation()", account));          return abi.decode());      }  : override before visibility 
288:      function borrowBalanceCurrent(address account) override external returns (uint) {

/// @audit      function borrowBalanceStored() override public view returns () {          bytes memory data = delegateToViewImplementation()", account));          return abi.decode());      }  : override before visibility 
298:      function borrowBalanceStored(address account) override public view returns (uint) {

/// @audit      function exchangeRateCurrent() override public returns () {          bytes memory data = delegateToImplementation()"));          return abi.decode());      }  : override before visibility 
307:      function exchangeRateCurrent() override public returns (uint) {

/// @audit      function exchangeRateStored() override public view returns () {          bytes memory data = delegateToViewImplementation()"));          return abi.decode());      }  : override before visibility 
317:      function exchangeRateStored() override public view returns (uint) {

/// @audit      function getCash() override external view returns () {          bytes memory data = delegateToViewImplementation()"));          return abi.decode());      }  : override before visibility 
326:      function getCash() override external view returns (uint) {

/// @audit      function accrueInterest() override public returns () {          bytes memory data = delegateToImplementation()"));          return abi.decode());      }  : override before visibility 
336:      function accrueInterest() override public returns (uint) {

/// @audit      function seize() override external returns () {          bytes memory data = delegateToImplementation()", liquidator, borrower, seizeTokens));          return abi.decode());      }  : override before visibility 
350:      function seize(address liquidator, address borrower, uint seizeTokens) override external returns (uint) {

/// @audit      function sweepToken() override external {          delegateToImplementation()", token));      }  : override before visibility 
359:      function sweepToken(EIP20NonStandardInterface token) override external {

/// @audit      function _setPendingAdmin() override external returns () {          bytes memory data = delegateToImplementation()", newPendingAdmin));          return abi.decode());      }  : override before visibility 
372:      function _setPendingAdmin(address payable newPendingAdmin) override external returns (uint) {

/// @audit      function _setComptroller() override public returns () {          bytes memory data = delegateToImplementation()", newComptroller));          return abi.decode());      }  : override before visibility 
382:      function _setComptroller(ComptrollerInterface newComptroller) override public returns (uint) {

/// @audit      function _setReserveFactor() override external returns () {          bytes memory data = delegateToImplementation()", newReserveFactorMantissa));          return abi.decode());      }  : override before visibility 
392:      function _setReserveFactor(uint newReserveFactorMantissa) override external returns (uint) {

/// @audit      function _acceptAdmin() override external returns () {          bytes memory data = delegateToImplementation()"));          return abi.decode());      }  : override before visibility 
402:      function _acceptAdmin() override external returns (uint) {

/// @audit      function _addReserves() override external returns () {          bytes memory data = delegateToImplementation()", addAmount));          return abi.decode());      }  : override before visibility 
412:      function _addReserves(uint addAmount) override external returns (uint) {

/// @audit      function _reduceReserves() override external returns () {          bytes memory data = delegateToImplementation()", reduceAmount));          return abi.decode());      }  : override before visibility 
422:      function _reduceReserves(uint reduceAmount) override external returns (uint) {

/// @audit      function _setInterestRateModel() override public returns () {          bytes memory data = delegateToImplementation()", newInterestRateModel));          return abi.decode());      }  : override before visibility 
433:      function _setInterestRateModel(InterestRateModel newInterestRateModel) override public returns (uint) {

/// @audit      function _setProtocolSeizeShare() override external returns () {          bytes memory data = delegateToImplementation()", newProtocolSeizeShareMantissa));          return abi.decode());      }  : override before visibility 
443:      function _setProtocolSeizeShare(uint newProtocolSeizeShareMantissa) override external returns (uint) {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20Delegator.sol#L61

```solidity
File: src/core/MErc20Delegate.sol

/// @audit      function _becomeImplementation() virtual override public {          // Shh -- currently unused          data;            // Shh -- we don't ever want this hook to be marked pure          if () {              implementation = address();          }            require();      }  : virtual before visibility 
21:       function _becomeImplementation(bytes memory data) virtual override public {

/// @audit      function _resignImplementation() virtual override public {          // Shh -- we don't ever want this hook to be marked pure          if () {              implementation = address();          }            require();      }  : virtual before visibility 
36        function _resignImplementation() virtual override public {
37            // Shh -- we don't ever want this hook to be marked pure
38:           if (false) {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20Delegate.sol#L21

```solidity
File: src/core/IRModels/InterestRateModel.sol

/// @audit      function getBorrowRate() virtual external view returns ();  : virtual before visibility 
19:       function getBorrowRate(uint cash, uint borrows, uint reserves) virtual external view returns (uint);

/// @audit      function getSupplyRate() virtual external view returns ();  : virtual before visibility 
29:       function getSupplyRate(uint cash, uint borrows, uint reserves, uint reserveFactorMantissa) virtual external view returns (uint);

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/IRModels/InterestRateModel.sol#L19

```solidity
File: src/core/IRModels/WhitePaperInterestRateModel.sol

/// @audit      function getBorrowRate() override public view returns () {          uint ur = utilizationRate();          return ur.mul().div().add();      }  : override before visibility 
67:       function getBorrowRate(uint cash, uint borrows, uint reserves) override public view returns (uint) {

/// @audit      function getSupplyRate() override public view returns () {          uint oneMinusReserveFactor = uint().sub();          uint borrowRate = getBorrowRate();          uint rateToPool = borrowRate.mul().div();          return utilizationRate().mul().div();      }  : override before visibility 
80:       function getSupplyRate(uint cash, uint borrows, uint reserves, uint reserveFactorMantissa) override public view returns (uint) {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/IRModels/WhitePaperInterestRateModel.sol#L67

```solidity
File: src/core/IRModels/JumpRateModel.sol

/// @audit      function getBorrowRate() override public view returns () {          uint util = utilizationRate();            if () {              return util.mul().div().add();          } else {              uint normalRate = kink.mul().div().add();              uint excessUtil = util.sub();              return excessUtil.mul().div().add();          }      }  : override before visibility 
84:       function getBorrowRate(uint cash, uint borrows, uint reserves) override public view returns (uint) {

/// @audit      function getSupplyRate() override public view returns () {          uint oneMinusReserveFactor = uint().sub();          uint borrowRate = getBorrowRate();          uint rateToPool = borrowRate.mul().div();          return utilizationRate().mul().div();      }  : override before visibility 
104:      function getSupplyRate(uint cash, uint borrows, uint reserves, uint reserveFactorMantissa) override public view returns (uint) {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/IRModels/JumpRateModel.sol#L84

</details>

### [N&#x2011;18] Large assembly blocks should have extensive comments
Assembly blocks are take a lot more time to audit than normal Solidity code, and often have gotchas and side-effects that the Solidity versions of the same code do not. Consider adding more comments explaining what is being done in every step of the assembly code

*There are 2 instances of this issue:*

```solidity
File: src/core/Unitroller.sol

140           assembly {
141                 let free_mem_ptr := mload(0x40)
142                 returndatacopy(free_mem_ptr, 0, returndatasize())
143   
144                 switch success
145                 case 0 { revert(free_mem_ptr, returndatasize()) }
146                 default { return(free_mem_ptr, returndatasize()) }
147:          }

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Unitroller.sol#L140-L147

```solidity
File: src/core/MErc20Delegator.sol

502           assembly {
503               let free_mem_ptr := mload(0x40)
504               returndatacopy(free_mem_ptr, 0, returndatasize())
505   
506               switch success
507               case 0 { revert(free_mem_ptr, returndatasize()) }
508               default { return(free_mem_ptr, returndatasize()) }
509:          }

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20Delegator.sol#L502-L509


### [N&#x2011;19] Function ordering does not follow the Solidity style guide
According to the [Solidity style guide](https://docs.soliditylang.org/en/v0.8.17/style-guide.html#order-of-functions), functions should be laid out in the following order :`constructor()`, `receive()`, `fallback()`, `external`, `public`, `internal`, `private`, but the cases below do not follow this pattern

*There are 35 instances of this issue:*

<details>
<summary>see instances</summary>


```solidity
File: src/core/MultiRewardDistributor/MultiRewardDistributor.sol

/// @audit getOutstandingRewardsForUser() came earlier
333:      function getCurrentEmissionCap() external view returns (uint256) {

/// @audit getGlobalBorrowIndex() came earlier
382       function _addEmissionConfig(
383           MToken _mToken,
384           address _owner,
385           address _emissionToken,
386           uint256 _supplyEmissionPerSec,
387           uint256 _borrowEmissionsPerSec,
388           uint256 _endTime
389:      ) external onlyComptrollersAdmin {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L333

```solidity
File: src/core/Comptroller.sol

/// @audit addToMarketInternal() came earlier
154:      function exitMarket(address mTokenAddress) override external returns (uint) {

/// @audit redeemAllowedInternal() came earlier
292:      function redeemVerify(address mToken, address redeemer, uint redeemAmount, uint redeemTokens) override pure external {

/// @audit getAccountLiquidityInternal() came earlier
542       function getHypotheticalAccountLiquidity(
543           address account,
544           address mTokenModify,
545           uint redeemTokens,
546:          uint borrowAmount) public view returns (uint, uint, uint) {

/// @audit getHypotheticalAccountLiquidityInternal() came earlier
629:      function liquidateCalculateSeizeTokens(address mTokenBorrowed, address mTokenCollateral, uint actualRepayAmount) override external view returns (uint, uint) {

/// @audit _setPriceOracle() came earlier
689:      function _setCloseFactor(uint newCloseFactorMantissa) external returns (uint) {

/// @audit _addMarketInternal() came earlier
807:      function _setMarketBorrowCaps(MToken[] calldata mTokens, uint[] calldata newBorrowCaps) external {

/// @audit _become() came earlier
959:      function _rescueFunds(address _tokenAddress, uint _amount) external {

/// @audit updateAndDistributeBorrowerRewardsForToken() came earlier
998       function claimReward() public {
999:          claimReward(msg.sender, allMarkets);

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Comptroller.sol#L154

```solidity
File: src/core/Unitroller.sol

/// @audit _acceptAdmin() came earlier
136       fallback() external {
137           // delegate all other functions to current implementation
138:          (bool success, ) = comptrollerImplementation.delegatecall(msg.data);

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Unitroller.sol#L136-L138

```solidity
File: src/core/Governance/TemporalGovernor.sol

/// @audit isTrustedSender() came earlier
96        function isTrustedSender(
97            uint16 chainId,
98            address addr
99:       ) external view returns (bool) {

/// @audit addressToBytes() came earlier
137       function setTrustedSenders(
138:          TrustedSender[] calldata _trustedSenders

/// @audit executeProposal() came earlier
242:      function permissionlessUnpause() external whenPaused {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Governance/TemporalGovernor.sol#L96-L99

```solidity
File: src/core/MToken.sol

/// @audit transferTokens() came earlier
136:      function transfer(address dst, uint256 amount) override external nonReentrant returns (bool) {

/// @audit getBlockTimestamp() came earlier
236:      function borrowRatePerTimestamp() override external view returns (uint) {

/// @audit borrowBalanceStoredInternal() came earlier
319:      function exchangeRateCurrent() override public nonReentrant returns (uint) {

/// @audit exchangeRateStoredInternal() came earlier
376:      function getCash() override external view returns (uint) {

/// @audit liquidateBorrowFresh() came earlier
1048:     function seize(address liquidator, address borrower, uint seizeTokens) override external nonReentrant returns (uint) {

/// @audit seizeInternal() came earlier
1145:     function _setPendingAdmin(address payable newPendingAdmin) override external returns (uint) {

/// @audit _setComptroller() came earlier
1219:     function _setReserveFactor(uint newReserveFactorMantissa) override external nonReentrant returns (uint) {

/// @audit _addReservesFresh() came earlier
1326:     function _reduceReserves(uint reduceAmount) override external nonReentrant returns (uint) {

/// @audit _reduceReservesFresh() came earlier
1391:     function _setInterestRateModel(InterestRateModel newInterestRateModel) override public returns (uint) {

/// @audit _setInterestRateModelFresh() came earlier
1443:     function _setProtocolSeizeShare(uint newProtocolSeizeShareMantissa) override external nonReentrant returns (uint) {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MToken.sol#L136

```solidity
File: src/core/MErc20.sol

/// @audit initialize() came earlier
46:       function mint(uint mintAmount) override external returns (uint) {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20.sol#L46

```solidity
File: src/core/MErc20Delegator.sol

/// @audit _setImplementation() came earlier
82:       function mint(uint mintAmount) override external returns (uint) {

/// @audit exchangeRateStored() came earlier
326:      function getCash() override external view returns (uint) {

/// @audit accrueInterest() came earlier
350:      function seize(address liquidator, address borrower, uint seizeTokens) override external returns (uint) {

/// @audit _setComptroller() came earlier
392:      function _setReserveFactor(uint newReserveFactorMantissa) override external returns (uint) {

/// @audit _setInterestRateModel() came earlier
443:      function _setProtocolSeizeShare(uint newProtocolSeizeShareMantissa) override external returns (uint) {

/// @audit delegateTo() came earlier
471:      function delegateToImplementation(bytes memory data) public returns (bytes memory) {

/// @audit delegateToViewImplementation() came earlier
496       fallback () external payable {
497:          require(msg.value == 0,"MErc20Delegator:fallback: cannot send value to fallback");

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20Delegator.sol#L82

```solidity
File: src/core/router/WETHRouter.sol

/// @audit redeem() came earlier
65:       receive() external payable {}

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/router/WETHRouter.sol#L65

```solidity
File: src/core/Oracles/ChainlinkOracle.sol

/// @audit getChainlinkPrice() came earlier
118       function setUnderlyingPrice(
119           MToken mToken,
120           uint256 underlyingPriceMantissa
121:      ) external onlyAdmin {

/// @audit getFeed() came earlier
167:      function assetPrices(address asset) external view returns (uint256) {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkOracle.sol#L118-L121

</details>

### [N&#x2011;20] Contract does not follow the Solidity style guide's suggested layout ordering
The [style guide](https://docs.soliditylang.org/en/v0.8.16/style-guide.html#order-of-layout) says that, within a contract, the ordering should be 1) Type declarations, 2) State variables, 3) Events, 4) Modifiers, and 5) Functions, but the contract(s) below do not follow this ordering

*There are 7 instances of this issue:*

```solidity
File: src/core/MultiRewardDistributor/MultiRewardDistributor.sol

/// @audit function initialize came earlier
124       modifier onlyComptrollersAdmin() {
125           require(
126               msg.sender == address(comptroller.admin()),
127               "Only the comptroller's administrator can do this!"
128           );
129           _;
130:      }

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L124-L130

```solidity
File: src/core/Comptroller.sol

/// @audit event NewRewardDistributor came earlier
62:       uint internal constant closeFactorMinMantissa = 0.05e18; // 0.05

/// @audit function getBlockTimestamp came earlier
1075      modifier nonReentrant() {
1076          // On the first call to nonReentrant, _notEntered will be true
1077          require(_locked != 1, "ReentrancyGuard: reentrant call");
1078  
1079          // Any calls to nonReentrant after this point will fail
1080          _locked = 1;
1081  
1082          _;
1083  
1084          // By storing the original value once again, a refund is triggered (see
1085          // https://eips.ethereum.org/EIPS/eip-2200)
1086          _locked = 0;
1087:     }

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Comptroller.sol#L62

```solidity
File: src/core/MToken.sol

/// @audit function doTransferOut came earlier
1514      modifier nonReentrant() {
1515          require(_notEntered, "re-entered");
1516          _notEntered = false;
1517          _;
1518          _notEntered = true; // get a gas-refund post-Istanbul
1519:     }

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MToken.sol#L1514-L1519

```solidity
File: src/core/IRModels/WhitePaperInterestRateModel.sol

/// @audit event NewInterestParams came earlier
20:       uint public constant timestampsPerYear = 31536000;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/IRModels/WhitePaperInterestRateModel.sol#L20

```solidity
File: src/core/IRModels/JumpRateModel.sol

/// @audit event NewInterestParams came earlier
20:       uint public constant timestampsPerYear = 60 * 60 * 24 * 365;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/IRModels/JumpRateModel.sol#L20

```solidity
File: src/core/Oracles/ChainlinkOracle.sol

/// @audit function constructor came earlier
50        modifier onlyAdmin() {
51            require(msg.sender == admin, "only admin may call");
52            _;
53:       }

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkOracle.sol#L50-L53


### [N&#x2011;21] Control structures do not follow the Solidity Style Guide
See the [control structures](https://docs.soliditylang.org/en/latest/style-guide.html#control-structures) section of the Solidity Style Guide

*There are 4 instances of this issue:*

```solidity
File: src/core/MultiRewardDistributor/MultiRewardDistributor.sol

836           if (
837:              userSupplyIndex == 0 && _globalSupplyIndex >= initialIndexConstant

875           if (
876:              userBorrowIndex == 0 && _globalBorrowIndex >= initialIndexConstant

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L836-L837

```solidity
File: src/core/Comptroller.sol

815:          for(uint i = 0; i < numMarkets; i++) {

852:          for(uint i = 0; i < numMarkets; i++) {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Comptroller.sol#L815


### [N&#x2011;22] Imports should use double quotes rather than single quotes


*There is one instance of this issue:*

```solidity
File: src/core/Governance/TemporalGovernor.sol

320:          // Very important to check to make sure that the VAA we're processing is specifically designed

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Governance/TemporalGovernor.sol#L320


### [N&#x2011;23] `mapping` definitions do not follow the Solidity Style Guide
See the [mappings](https://docs.soliditylang.org/en/latest/style-guide.html#mappings) section of the Solidity Style Guide

*There are 2 instances of this issue:*

```solidity
File: src/core/Oracles/ChainlinkOracle.sol

23:       mapping (address => uint256) internal prices;

27:       mapping (bytes32 => AggregatorV3Interface) internal feeds;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkOracle.sol#L23


### [N&#x2011;24] Numeric values having to do with time should use time units for readability
There are [units](https://docs.soliditylang.org/en/latest/units-and-global-variables.html#time-units) for seconds, minutes, hours, days, and weeks, and since they're defined, they should be used

*There are 3 instances of this issue:*

```solidity
File: src/core/IRModels/WhitePaperInterestRateModel.sol

/// @audit 31536000
17        /**
18         * @notice The approximate number of timestamps per year that is assumed by the interest rate model
19         */
20:       uint public constant timestampsPerYear = 31536000;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/IRModels/WhitePaperInterestRateModel.sol#L17-L20

```solidity
File: src/core/IRModels/JumpRateModel.sol

/// @audit 60
/// @audit 60
17        /**
18         * @notice The approximate number of timestamps per year that is assumed by the interest rate model
19         */
20:       uint public constant timestampsPerYear = 60 * 60 * 24 * 365;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/IRModels/JumpRateModel.sol#L17-L20


### [N&#x2011;25] Consider using `delete` rather than assigning zero/false to clear values
The `delete` keyword more closely matches the semantics of what is being done, and draws more attention to the changing of state, which may lead to a more thorough audit of its associated logic

*There is one instance of this issue:*

```solidity
File: src/core/Comptroller.sol

785:          newMarket.collateralFactorMantissa = 0;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Comptroller.sol#L785


### [N&#x2011;26] Contracts should have full test coverage
While 100% code coverage does not guarantee that there are no bugs, it often will catch easy-to-find bugs, and will ensure that there are fewer regressions when the code invariably has to be modified. Furthermore, in order to get full coverage, code authors will often have to re-organize their code so that it is more modular, so that each component can be tested separately, which reduces interdependencies between modules and layers, and makes for code that is easier to reason about and audit.

*There is one instance of this issue:*

```solidity
File: Various Files


```


### [N&#x2011;27] Large or complicated code bases should implement invariant tests
Large code bases, or code with lots of inline-assembly, complicated math, or complicated interactions between multiple contracts, should implement [invariant fuzzing tests](https://medium.com/coinmonks/smart-contract-fuzzing-d9b88e0b0a05). Invariant fuzzers such as Echidna require the test writer to come up with invariants which should not be violated under any circumstances, and the fuzzer tests various inputs and function calls to ensure that the invariants always hold. Even code with 100% code coverage can still have bugs due to the order of the operations a user performs, and invariant fuzzers, with properly and extensively-written invariants, can close this testing gap significantly.

*There is one instance of this issue:*

```solidity
File: Various Files


```


### [N&#x2011;28] Enable IR-based code generation
By using `--via-ir` or `{"viaIR": true}`, the compiler is able to use more advanced [multi-function optimizations](https://docs.soliditylang.org/en/v0.8.17/ir-breaking-changes.html#solidity-ir-based-codegen-changes), for extra gas savings.

*There is one instance of this issue:*

```solidity
File: Various Files


```


### [N&#x2011;29] Custom errors should be used rather than `revert()`/`require()`
Custom errors are available from solidity version 0.8.4. Custom errors are more easily processed in `try`-`catch` blocks, and are easier to re-use and maintain.

*There are 118 instances of this issue:*

<details>
<summary>see instances</summary>


```solidity
File: src/core/MultiRewardDistributor/MultiRewardDistributor.sol

93            require(
94                _comptroller != address(0),
95                "Comptroller can't be the 0 address!"
96:           );

97            require(
98                _pauseGuardian != address(0),
99                "Pause Guardian can't be the 0 address!"
100:          );

104           require(
105               comptroller.isComptroller(),
106               "Can't bind to something that's not a comptroller!"
107:          );

125           require(
126               msg.sender == address(comptroller.admin()),
127               "Only the comptroller's administrator can do this!"
128:          );

134           require(
135               msg.sender == address(comptroller) ||
136                   msg.sender == comptroller.admin(),
137               "Only the comptroller or comptroller admin can call this function"
138:          );

152           require(
153               msg.sender == emissionConfig.config.owner ||
154                   msg.sender == comptroller.admin(),
155               "Only the config owner or comptroller admin can call this function"
156:          );

162           require(
163               msg.sender == pauseGuardian || msg.sender == comptroller.admin(),
164               "Only the pause guardian or comptroller admin can call this function"
165:          );

393           require(
394               tokenIsListed,
395               "The market requested to be added is un-listed!"
396:          );

399           require(
400               _supplyEmissionPerSec < emissionCap,
401               "Cannot set a supply reward speed higher than the emission cap!"
402:          );

403           require(
404               _borrowEmissionsPerSec < emissionCap,
405               "Cannot set a borrow reward speed higher than the emission cap!"
406:          );

409           require(
410               _endTime > block.timestamp + 1,
411               "The _endTime parameter must be in the future!"
412:          );

421               require(
422                   mTokenConfig.config.emissionToken != _emissionToken,
423                   "Emission token already listed!"
424:              );

496           require(
497               _newPauseGuardian != address(0),
498               "Pause Guardian can't be the 0 address!"
499:          );

674           require(
675               _newSupplySpeed != currentSupplySpeed,
676               "Can't set new supply emissions to be equal to current!"
677:          );

678           require(
679               _newSupplySpeed < emissionCap,
680               "Cannot set a supply reward speed higher than the emission cap!"
681:          );

718           require(
719               _newBorrowSpeed != currentBorrowSpeed,
720               "Can't set new borrow emissions to be equal to current!"
721:          );

722           require(
723               _newBorrowSpeed < emissionCap,
724               "Cannot set a borrow reward speed higher than the emission cap!"
725:          );

789           require(
790               _newEndTime > currentEndTime,
791               "_newEndTime MUST be > currentEndTime"
792:          );

793           require(
794               _newEndTime > block.timestamp,
795               "_newEndTime MUST be > block.timestamp"
796:          );

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L93-L96

```solidity
File: src/core/Comptroller.sol

158:          require(oErr == 0, "exitMarket: getAccountSnapshot failed"); // semi-opaque error code

217:          require(!mintGuardianPaused[mToken], "mint is paused");

236:              require(nextTotalSupplies < supplyCap, "market supply cap reached");

312:          require(!borrowGuardianPaused[mToken], "borrow is paused");

320:              require(msg.sender == mToken, "sender must be mToken");

341:              require(nextTotalBorrows < borrowCap, "market borrow cap reached");

441:          require(!seizeGuardianPaused, "seize is paused");

474:          require(!transferGuardianPaused, "transfer is paused");

691:      	require(msg.sender == admin, "only admin can set close factor");

781:          require(mToken.isMToken(), "Must be an MToken"); // Sanity check to make sure its really a MToken

796:              require(allMarkets[i] != MToken(mToken), "market already added");

808:      	require(msg.sender == admin || msg.sender == borrowCapGuardian, "only admin or borrow cap guardian can set borrow caps");

813:          require(numMarkets != 0 && numMarkets == numBorrowCaps, "invalid input");

826:          require(msg.sender == admin, "only admin can set borrow cap guardian");

845:          require(msg.sender == admin || msg.sender == supplyCapGuardian, "only admin or supply cap guardian can set supply caps");

850:          require(numMarkets != 0 && numMarkets == numSupplyCaps, "invalid input");

863:          require(msg.sender == admin, "only admin can set supply cap guardian");

902:          require(msg.sender == admin, "Unauthorized");

912:          require(markets[address(mToken)].isListed, "cannot pause a market that is not listed");

913:          require(msg.sender == pauseGuardian || msg.sender == admin, "only pause guardian and admin can pause");

914:          require(msg.sender == admin || state == true, "only admin can unpause");

922:          require(markets[address(mToken)].isListed, "cannot pause a market that is not listed");

923:          require(msg.sender == pauseGuardian || msg.sender == admin, "only pause guardian and admin can pause");

924:          require(msg.sender == admin || state == true, "only admin can unpause");

932:          require(msg.sender == pauseGuardian || msg.sender == admin, "only pause guardian and admin can pause");

933:          require(msg.sender == admin || state == true, "only admin can unpause");

941:          require(msg.sender == pauseGuardian || msg.sender == admin, "only pause guardian and admin can pause");

942:          require(msg.sender == admin || state == true, "only admin can unpause");

950:          require(msg.sender == unitroller.admin(), "only unitroller admin can change brains");

951:          require(unitroller._acceptImplementation() == 0, "change not authorized");

960:          require(msg.sender == admin, "Unauthorized");

1029:         require(address(rewardDistributor) != address(0), "No reward distributor configured!");

1035:             require(markets[address(mToken)].isListed, "market must be listed");

1077:         require(_locked != 1, "ReentrancyGuard: reentrant call");

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Comptroller.sol#L158

```solidity
File: src/core/Governance/TemporalGovernor.sol

140           require(
141               msg.sender == address(this),
142               "TemporalGovernor: Only this contract can update trusted senders"
143:          );

167           require(
168               msg.sender == address(this),
169               "TemporalGovernor: Only this contract can update trusted senders"
170:          );

189           require(
190               msg.sender == address(this),
191               "TemporalGovernor: Only this contract can update grant guardian pause"
192:          );

207           require(
208               msg.sender == oldGuardian || msg.sender == address(this),
209               "TemporalGovernor: cannot revoke guardian"
210:          );

248           require(
249               lastPauseTime + permissionlessUnpauseTime <= block.timestamp,
250               "TemporalGovernor: not past pause window"
251:          );

278               require(
279                   guardianPauseAllowed,
280                   "TemporalGovernor: guardian pause not allowed"
281:              );

306:          require(valid, reason);

322:          require(intendedRecipient == address(this), "TemporalGovernor: Incorrect destination");

325           require(
326               trustedSenders[vm.emitterChainId].contains(vm.emitterAddress), /// allow multiple per chainid
327               "TemporalGovernor: Invalid Emitter Address"
328:          );

331           require(
332               queuedTransactions[vm.hash].queueTime == 0,
333               "TemporalGovernor: Message already queued"
334:          );

352:          require(valid, reason); /// ensure VAA parsing verification succeeded

355               require(
356                   queuedTransactions[vm.hash].queueTime != 0,
357                   "TemporalGovernor: tx not queued"
358:              );

359               require(
360                   queuedTransactions[vm.hash].queueTime + proposalDelay <=
361                       block.timestamp,
362                   "TemporalGovernor: timelock not finished"
363:              );

370           require(
371               trustedSenders[vm.emitterChainId].contains(vm.emitterAddress), /// allow multiple per chainid
372               "TemporalGovernor: Invalid Emitter Address"
373:          );

375           require(
376               !queuedTransactions[vm.hash].executed,
377               "TemporalGovernor: tx already executed"
378:          );

405:              require(success, string(returnData));

417:          require(targets.length != 0, "TemporalGovernor: Empty proposal");

418           require(
419               targets.length == values.length &&
420                   targets.length == calldatas.length,
421               "TemporalGovernor: Arity mismatch for payload"
422:          );

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Governance/TemporalGovernor.sol#L140-L143

```solidity
File: src/core/MToken.sol

32:           require(msg.sender == admin, "only admin may initialize the market");

33:           require(accrualBlockTimestamp == 0 && borrowIndex == 0, "market may only be initialized once");

37:           require(initialExchangeRateMantissa > 0, "initial exchange rate must be greater than zero.");

41:           require(err == uint(Error.NO_ERROR), "setting comptroller failed");

49:           require(err == uint(Error.NO_ERROR), "setting interest rate model failed");

194:          require(mErr == MathError.NO_ERROR, "balance could not be calculated");

253:          require(accrueInterest() == uint(Error.NO_ERROR), "accrue interest failed");

263:          require(accrueInterest() == uint(Error.NO_ERROR), "accrue interest failed");

274:          require(err == MathError.NO_ERROR, "borrowBalanceStored: borrowBalanceStoredInternal failed");

320:          require(accrueInterest() == uint(Error.NO_ERROR), "accrue interest failed");

331:          require(err == MathError.NO_ERROR, "exchangeRateStored: exchangeRateStoredInternal failed");

403:          require(borrowRateMantissa <= borrowRateMaxMantissa, "borrow rate is absurdly high");

407:          require(mathErr == MathError.NO_ERROR, "could not calculate block delta");

537:          require(vars.mathErr == MathError.NO_ERROR, "MINT_EXCHANGE_CALCULATION_FAILED");

545:          require(vars.mathErr == MathError.NO_ERROR, "MINT_NEW_TOTAL_SUPPLY_CALCULATION_FAILED");

548:          require(vars.mathErr == MathError.NO_ERROR, "MINT_NEW_ACCOUNT_BALANCE_CALCULATION_FAILED");

616:          require(redeemTokensIn == 0 || redeemAmountIn == 0, "one of redeemTokensIn or redeemAmountIn must be zero");

914:          require(vars.mathErr == MathError.NO_ERROR, "REPAY_BORROW_NEW_ACCOUNT_BORROW_BALANCE_CALCULATION_FAILED");

917:          require(vars.mathErr == MathError.NO_ERROR, "REPAY_BORROW_NEW_TOTAL_BALANCE_CALCULATION_FAILED");

1013:         require(amountSeizeError == uint(Error.NO_ERROR), "LIQUIDATE_COMPTROLLER_CALCULATE_AMOUNT_SEIZE_FAILED");

1016:         require(mTokenCollateral.balanceOf(borrower) >= seizeTokens, "LIQUIDATE_SEIZE_TOO_MUCH");

1027:         require(seizeError == uint(Error.NO_ERROR), "token seizure failed");

1102:         require(vars.mathErr == MathError.NO_ERROR, "exchange rate math error");

1203:         require(newComptroller.isComptroller(), "marker method returned false");

1308:         require(totalReservesNew >= totalReserves, "add reserves unexpected overflow");

1372:         require(totalReservesNew <= totalReserves, "reduce reserves unexpected underflow");

1426:         require(newInterestRateModel.isInterestRateModel(), "marker method returned false");

1515:         require(_notEntered, "re-entered");

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MToken.sol#L32

```solidity
File: src/core/MErc20.sol

149:          require(msg.sender == admin, "MErc20::sweepToken: only admin can sweep tokens");

150:          require(address(token) != underlying, "MErc20::sweepToken: can not sweep underlying token");

206:          require(success, "TOKEN_TRANSFER_IN_FAILED");

210:          require(balanceAfter >= balanceBefore, "TOKEN_TRANSFER_IN_OVERFLOW");

241:          require(success, "TOKEN_TRANSFER_OUT_FAILED");

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20.sol#L149

```solidity
File: src/core/MErc20Delegator.sol

62:           require(msg.sender == admin, "MErc20Delegator::_setImplementation: Caller must be admin");

497:          require(msg.value == 0,"MErc20Delegator:fallback: cannot send value to fallback");

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20Delegator.sol#L62

```solidity
File: src/core/MErc20Delegate.sol

30:           require(msg.sender == admin, "only the admin may call _becomeImplementation");

42:           require(msg.sender == admin, "only the admin may call _resignImplementation");

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20Delegate.sol#L30

```solidity
File: src/core/router/WETHRouter.sol

34:           require(mToken.mint(msg.value) == 0, "WETHRouter: mint failed");

52            require(
53                mToken.redeem(mTokenRedeemAmount) == 0,
54                "WETHRouter: redeem failed"
55:           );

62:           require(success, "WETHRouter: ETH transfer failed");

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/router/WETHRouter.sol#L34

```solidity
File: src/core/Oracles/ChainlinkCompositeOracle.sol

108           require(
109               expectedDecimals > uint8(0) && expectedDecimals <= uint8(18),
110               "CLCOracle: Invalid expected decimals"
111:          );

139           require(
140               expectedDecimals > uint8(0) && expectedDecimals <= uint8(18),
141               "CLCOracle: Invalid expected decimals"
142:          );

191:          require(valid, "CLCOracle: Oracle data is invalid");

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkCompositeOracle.sol#L108-L111

```solidity
File: src/core/Oracles/ChainlinkOracle.sol

51:           require(msg.sender == admin, "only admin may call");

102:          require(answer > 0, "Chainlink price cannot be lower than 0");

103:          require(updatedAt != 0, "Round is in incompleted state");

145           require(
146               feed != address(0) && feed != address(this),
147               "invalid feed address"
148:          );

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkOracle.sol#L51

</details>

### [N&#x2011;30] Array indicies should be referenced via `enum`s rather than via numeric literals


*There is one instance of this issue:*

```solidity
File: src/core/Comptroller.sol

1017:        holders[0] = holder;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Comptroller.sol#L1017-L1017


### [N&#x2011;31] Chainlink oracle `roundId` and `answeredIn` variables no longer contain useful information
The new Chainlink [OCR](https://docs.chain.link/architecture-overview/off-chain-reporting) methodology no longer relies on rounds, so adding checks related to the `roundId`/`answeredIn` return values is unnecessary, and may cause issues down the line.

*There is one instance of this issue:*

```solidity
File: src/core/Oracles/ChainlinkCompositeOracle.sol

183          (
184              uint80 roundId,
185              int256 price,
186              ,
187              ,
188              uint80 answeredInRound
189          ) = AggregatorV3Interface(oracleAddress).latestRoundData();
190          bool valid = price > 0 && answeredInRound == roundId;
191          require(valid, "CLCOracle: Oracle data is invalid");
192          uint8 oracleDecimals = AggregatorV3Interface(oracleAddress).decimals();
193  
194          return (price, oracleDecimals); /// price always gt 0 at this point
195:     }

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkCompositeOracle.sol#L183-L195


### [N&#x2011;32] Variable names for constants don't follow the Solidity style guide
For `constant` variable names, each word should use all capital letters, with underscores separating each word (CONSTANT_CASE)

*There are 8 instances of this issue:*

```solidity
File: src/core/Comptroller.sol

62:      uint internal constant closeFactorMinMantissa = 0.05e18; // 0.05

65:      uint internal constant closeFactorMaxMantissa = 0.9e18; // 0.9

68:      uint internal constant collateralFactorMaxMantissa = 0.9e18; // 0.9

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Comptroller.sol#L62-L62

```solidity
File: src/core/IRModels/InterestRateModel.sol

10:      bool public constant isInterestRateModel = true;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/IRModels/InterestRateModel.sol#L10-L10

```solidity
File: src/core/IRModels/JumpRateModel.sol

20:      uint public constant timestampsPerYear = 60 * 60 * 24 * 365;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/IRModels/JumpRateModel.sol#L20-L20

```solidity
File: src/core/IRModels/WhitePaperInterestRateModel.sol

20:      uint public constant timestampsPerYear = 31536000;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/IRModels/WhitePaperInterestRateModel.sol#L20-L20

```solidity
File: src/core/MultiRewardDistributor/MultiRewardDistributor.sol

63:      uint224 public constant initialIndexConstant = 1e36;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L63-L63

```solidity
File: src/core/Oracles/ChainlinkCompositeOracle.sol

29:      uint8 public constant decimals = 18;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkCompositeOracle.sol#L29-L29


### [N&#x2011;33] Use OpenZeppelin's `ReentrancyGuard`/`ReentrancyGuardUpgradeable` rather than writing your own
The OpenZeppelin version has been gas-optimized, and thoroughly test, so consider using it rather than writing your own

*There are 2 instances of this issue:*

```solidity
File: src/core/Comptroller.sol

1075     modifier nonReentrant() {
1076         // On the first call to nonReentrant, _notEntered will be true
1077         require(_locked != 1, "ReentrancyGuard: reentrant call");
1078 
1079         // Any calls to nonReentrant after this point will fail
1080         _locked = 1;
1081 
1082         _;
1083 
1084         // By storing the original value once again, a refund is triggered (see
1085         // https://eips.ethereum.org/EIPS/eip-2200)
1086         _locked = 0;
1087:    }

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Comptroller.sol#L1075-L1087

```solidity
File: src/core/MToken.sol

1514     modifier nonReentrant() {
1515         require(_notEntered, "re-entered");
1516         _notEntered = false;
1517         _;
1518         _notEntered = true; // get a gas-refund post-Istanbul
1519:    }

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MToken.sol#L1514-L1519


### [N&#x2011;34] Events are missing sender information
When an action is triggered based on a user's action, not being able to filter based on who triggered the action makes event processing a lot more cumbersome. Including the `msg.sender` the events of these types of action will make events much more useful to end users, especially when `msg.sender` is not `tx.origin`.

*There are 35 instances of this issue:*

```solidity
File: src/core/Comptroller.sol

678:         emit NewPriceOracle(oldOracle, newOracle);

695:         emit NewCloseFactor(oldCloseFactorMantissa, closeFactorMantissa);

737:         emit NewCollateralFactor(mToken, oldCollateralFactorMantissa, newCollateralFactorMantissa);

761:         emit NewLiquidationIncentive(oldLiquidationIncentiveMantissa, newLiquidationIncentiveMantissa);

789:         emit MarketListed(mToken);

817:             emit NewBorrowCap(mTokens[i], newBorrowCaps[i]);

835:         emit NewBorrowCapGuardian(oldBorrowCapGuardian, newBorrowCapGuardian);

854:             emit NewSupplyCap(mTokens[i], newSupplyCaps[i]);

872:         emit NewSupplyCapGuardian(oldSupplyCapGuardian, newSupplyCapGuardian);

892:         emit NewPauseGuardian(oldPauseGuardian, pauseGuardian);

908:         emit NewRewardDistributor(oldRewardDistributor, newRewardDistributor);

917:         emit ActionPaused(mToken, "Mint", state);

927:         emit ActionPaused(mToken, "Borrow", state);

936:         emit ActionPaused("Transfer", state);

945:         emit ActionPaused("Seize", state);

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Comptroller.sol#L678-L678

```solidity
File: src/core/Governance/TemporalGovernor.sol

197:         emit GuardianPauseGranted(block.timestamp);

220:         emit GuardianRevoked(oldGuardian);

341:         emit QueuedTransaction(intendedRecipient, targets, values, calldatas);

407:             emit ExecutedTransaction(target, value, data);

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Governance/TemporalGovernor.sol#L197-L197

```solidity
File: src/core/MErc20Delegator.sol

73:          emit NewImplementation(oldImplementation, implementation);

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20Delegator.sol#L73-L73

```solidity
File: src/core/MToken.sol

162:         emit Approval(src, spender, amount);

1158:        emit NewPendingAdmin(oldPendingAdmin, newPendingAdmin);

1184:        emit NewAdmin(oldAdmin, admin);

1185:        emit NewPendingAdmin(oldPendingAdmin, pendingAdmin);

1209:        emit NewComptroller(oldComptroller, newComptroller);

1253:        emit NewReserveFactor(oldReserveFactorMantissa, newReserveFactorMantissa);

1380:        emit ReservesReduced(admin, reduceAmount, totalReservesNew);

1432:        emit NewMarketInterestRateModel(oldInterestRateModel, newInterestRateModel);

1481:        emit NewProtocolSeizeShare(oldProtocolSeizeShareMantissa, newProtocolSeizeShareMantissa);

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MToken.sol#L162-L162

```solidity
File: src/core/Unitroller.sol

49:          emit NewPendingImplementation(oldPendingImplementation, pendingComptrollerImplementation);

73:          emit NewImplementation(oldImplementation, comptrollerImplementation);

74:          emit NewPendingImplementation(oldPendingImplementation, pendingComptrollerImplementation);

99:          emit NewPendingAdmin(oldPendingAdmin, newPendingAdmin);

125:         emit NewAdmin(oldAdmin, admin);

126:         emit NewPendingAdmin(oldPendingAdmin, pendingAdmin);

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Unitroller.sol#L49-L49


### [N&#x2011;35] Variables need not be initialized to zero
The default value for variables is zero, so initializing them to zero is superfluous.

*There are 48 instances of this issue:*

```solidity
File: src/core/Comptroller.sol

106:         for (uint i = 0; i < len; i++) {

186:         for (uint i = 0; i < len; i++) {

574:         for (uint i = 0; i < assets.length; i++) {

795:         for (uint i = 0; i < allMarkets.length; i ++) {

815:         for(uint i = 0; i < numMarkets; i++) {

852:         for(uint i = 0; i < numMarkets; i++) {

1040:                for (uint holderIndex = 0; holderIndex < holders.length; holderIndex++) {

1048:                for (uint holderIndex = 0; holderIndex < holders.length; holderIndex++) {

1031:        for (uint i = 0; i < mTokens.length; i++) {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Comptroller.sol#L106-L106

```solidity
File: src/core/Governance/TemporalGovernor.sol

74:          for (uint256 i = 0; i < _trustedSenders.length; i++) {

117:             for (uint256 i = 0; i < trustedSendersList.length; i++) {

146:             for (uint256 i = 0; i < _trustedSenders.length; i++) {

173:             for (uint256 i = 0; i < _trustedSenders.length; i++) {

394:         for (uint256 i = 0; i < targets.length; i++) {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Governance/TemporalGovernor.sol#L74-L74

```solidity
File: src/core/MToken.sol

81:          uint startingAllowance = 0;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MToken.sol#L81-L81

```solidity
File: src/core/MultiRewardDistributor/MultiRewardDistributor.sol

209:         for (uint256 index = 0; index < configs.length; index++) {

240:         for (uint256 index = 0; index < markets.length; index++) {

281:         for (uint256 index = 0; index < configs.length; index++) {

419:         for (uint256 index = 0; index < configs.length; index++) {

983:         for (uint256 index = 0; index < configs.length; index++) {

1009:        for (uint256 index = 0; index < configs.length; index++) {

1053:        for (uint256 index = 0; index < configs.length; index++) {

1112:        for (uint256 index = 0; index < configs.length; index++) {

1163:        for (uint256 index = 0; index < configs.length; index++) {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L209-L209

```solidity
File: src/core/Comptroller.sol

106:         for (uint i = 0; i < len; i++) {

186:         for (uint i = 0; i < len; i++) {

574:         for (uint i = 0; i < assets.length; i++) {

795:         for (uint i = 0; i < allMarkets.length; i ++) {

815:         for(uint i = 0; i < numMarkets; i++) {

852:         for(uint i = 0; i < numMarkets; i++) {

1040:                for (uint holderIndex = 0; holderIndex < holders.length; holderIndex++) {

1048:                for (uint holderIndex = 0; holderIndex < holders.length; holderIndex++) {

1031:        for (uint i = 0; i < mTokens.length; i++) {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Comptroller.sol#L106-L106

```solidity
File: src/core/Governance/TemporalGovernor.sol

74:          for (uint256 i = 0; i < _trustedSenders.length; i++) {

117:             for (uint256 i = 0; i < trustedSendersList.length; i++) {

146:             for (uint256 i = 0; i < _trustedSenders.length; i++) {

173:             for (uint256 i = 0; i < _trustedSenders.length; i++) {

394:         for (uint256 i = 0; i < targets.length; i++) {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Governance/TemporalGovernor.sol#L74-L74

```solidity
File: src/core/MToken.sol

81:          uint startingAllowance = 0;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MToken.sol#L81-L81

```solidity
File: src/core/MultiRewardDistributor/MultiRewardDistributor.sol

209:         for (uint256 index = 0; index < configs.length; index++) {

240:         for (uint256 index = 0; index < markets.length; index++) {

281:         for (uint256 index = 0; index < configs.length; index++) {

419:         for (uint256 index = 0; index < configs.length; index++) {

983:         for (uint256 index = 0; index < configs.length; index++) {

1009:        for (uint256 index = 0; index < configs.length; index++) {

1053:        for (uint256 index = 0; index < configs.length; index++) {

1112:        for (uint256 index = 0; index < configs.length; index++) {

1163:        for (uint256 index = 0; index < configs.length; index++) {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L209-L209


### [N&#x2011;36] Consider using named mappings
Consider moving to solidity version 0.8.18 or later, and using [named mappings](https://ethereum.stackexchange.com/a/145555) to make it easier to understand the purpose of each mapping

*There are 5 instances of this issue:*

```solidity
File: src/core/Governance/TemporalGovernor.sol

55:      mapping(uint16 => EnumerableSet.Bytes32Set) private trustedSenders;

59:      mapping(bytes32 => ProposalInfo) public queuedTransactions;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Governance/TemporalGovernor.sol#L55-L55

```solidity
File: src/core/MultiRewardDistributor/MultiRewardDistributor.sol

54:      mapping(address => MarketEmissionConfig[]) public marketConfigs;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L54-L54

```solidity
File: src/core/Oracles/ChainlinkOracle.sol

23:      mapping (address => uint256) internal prices;

27:      mapping (bytes32 => AggregatorV3Interface) internal feeds;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkOracle.sol#L23-L23


### [N&#x2011;37] Consider adding a block/deny-list
Doing so will significantly increase centralization, but will help to prevent hackers from using stolen tokens

*There are 8 instances of this issue:*

<details>
<summary>see instances</summary>


```solidity
File: src/core/Comptroller.sol

15   contract Comptroller is ComptrollerV2Storage, ComptrollerInterface, ComptrollerErrorReporter, ExponentialNoError {
16:      /// @notice Emitted when an admin supports a market

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Comptroller.sol#L15-L16

```solidity
File: src/core/MErc20.sol

12   contract MErc20 is MToken, MErc20Interface {
13       /**
14        * @notice Initialize the new money market
15        * @param underlying_ The address of the underlying asset
16        * @param comptroller_ The address of the Comptroller
17        * @param interestRateModel_ The address of the interest rate model
18        * @param initialExchangeRateMantissa_ The initial exchange rate, scaled by 1e18
19        * @param name_ ERC-20 name of this token
20        * @param symbol_ ERC-20 symbol of this token
21        * @param decimals_ ERC-20 decimal precision of this token
22:       */

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20.sol#L12-L22

```solidity
File: src/core/MErc20Delegate.sol

11   contract MErc20Delegate is MErc20, MDelegateInterface {
12       /**
13        * @notice Construct an empty delegate
14:       */

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20Delegate.sol#L11-L14

```solidity
File: src/core/MErc20Delegator.sol

11   contract MErc20Delegator is MTokenInterface, MErc20Interface, MDelegatorInterface {
12       /**
13        * @notice Construct a new money market
14        * @param underlying_ The address of the underlying asset
15        * @param comptroller_ The address of the Comptroller
16        * @param interestRateModel_ The address of the interest rate model
17        * @param initialExchangeRateMantissa_ The initial exchange rate, scaled by 1e18
18        * @param name_ ERC-20 name of this token
19        * @param symbol_ ERC-20 symbol of this token
20        * @param decimals_ ERC-20 decimal precision of this token
21        * @param admin_ Address of the administrator of this token
22        * @param implementation_ The address of the implementation the contract delegates to
23        * @param becomeImplementationData The encoded args for becomeImplementation
24:       */

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20Delegator.sol#L11-L24

```solidity
File: src/core/MultiRewardDistributor/MultiRewardDistributor.sol

43   contract MultiRewardDistributor is
44       Pausable,
45       ReentrancyGuard,
46       Initializable,
47       MultiRewardDistributorCommon,
48       ExponentialNoError
49:  {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L43-L49

```solidity
File: src/core/Oracles/ChainlinkOracle.sol

11   contract ChainlinkOracle is PriceOracle {
12       /// @dev redundant as used with solidity 0.8.0,
13:      /// but used to not make code changes around math

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkOracle.sol#L11-L13

```solidity
File: src/core/Unitroller.sol

11   contract Unitroller is UnitrollerAdminStorage, ComptrollerErrorReporter {
12   
13       /**
14         * @notice Emitted when pendingComptrollerImplementation is changed
15:        */

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Unitroller.sol#L11-L15

```solidity
File: src/core/router/WETHRouter.sol

11:  contract WETHRouter {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/router/WETHRouter.sol#L11-L11

</details>

### [N&#x2011;38] Non-`external`/`public` variable and function names should begin with an underscore
According to the Solidity Style Guide, non-`external`/`public` function names should begin with an [underscore](https://docs.soliditylang.org/en/latest/style-guide.html#underscore-prefix-for-non-external-functions-and-variables)

*There are 41 instances of this issue:*

```solidity
File: src/core/Comptroller.sol

121:     function addToMarketInternal(MToken mToken, address borrower) internal returns (Error) {

263:     function redeemAllowedInternal(address mToken, address redeemer, uint redeemTokens) internal view returns (uint) {

528:     function getAccountLiquidityInternal(address account) internal view returns (Error, uint, uint) {

563      function getHypotheticalAccountLiquidityInternal(
564          address account,
565          MToken mTokenModify,
566          uint redeemTokens,
567:         uint borrowAmount) internal view returns (Error, uint, uint) {

978:     function updateAndDistributeSupplierRewardsForToken(address mToken, address supplier) internal {

989:     function updateAndDistributeBorrowerRewardsForToken(address mToken, address borrower) internal {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Comptroller.sol#L121-L121

```solidity
File: src/core/MErc20.sol

171:     function getCashPrior() virtual override internal view returns (uint) {

185:     function doTransferIn(address from, uint amount) virtual override internal returns (uint) {

223:     function doTransferOut(address payable to, uint amount) virtual override internal {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20.sol#L171-L171

```solidity
File: src/core/MErc20Delegator.sol

455:     function delegateTo(address callee, bytes memory data) internal returns (bytes memory) {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20Delegator.sol#L455-L455

```solidity
File: src/core/MToken.sol

68:      function transferTokens(address spender, address src, address dst, uint tokens) internal returns (uint) {

228:     function getBlockTimestamp() virtual internal view returns (uint) {

283:     function borrowBalanceStoredInternal(address account) internal view returns (MathError, uint) {

340:     function exchangeRateStoredInternal() virtual internal view returns (MathError, uint) {

471:     function mintInternal(uint mintAmount) internal nonReentrant returns (uint, uint) {

498:     function mintFresh(address minter, uint mintAmount) internal returns (uint, uint) {

571:     function redeemInternal(uint redeemTokens) internal nonReentrant returns (uint) {

587:     function redeemUnderlyingInternal(uint redeemAmount) internal nonReentrant returns (uint) {

615:     function redeemFresh(address payable redeemer, uint redeemTokensIn, uint redeemAmountIn) internal returns (uint) {

728:     function borrowInternal(uint borrowAmount) internal nonReentrant returns (uint) {

750:     function borrowFresh(address payable borrower, uint borrowAmount) internal returns (uint) {

821:     function repayBorrowInternal(uint repayAmount) internal nonReentrant returns (uint, uint) {

837:     function repayBorrowBehalfInternal(address borrower, uint repayAmount) internal nonReentrant returns (uint, uint) {

865:     function repayBorrowFresh(address payer, address borrower, uint repayAmount) internal returns (uint, uint) {

942:     function liquidateBorrowInternal(address borrower, uint repayAmount, MTokenInterface mTokenCollateral) internal nonReentrant returns (uint, uint) {

968:     function liquidateBorrowFresh(address liquidator, address borrower, uint repayAmount, MTokenInterface mTokenCollateral) internal returns (uint, uint) {

1074:    function seizeInternal(address seizerToken, address liquidator, address borrower, uint seizeTokens) internal returns (uint) {

1493:    function getCashPrior() virtual internal view returns (uint);

1499:    function doTransferIn(address from, uint amount) virtual internal returns (uint);

1506:    function doTransferOut(address payable to, uint amount) virtual internal;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MToken.sol#L68-L68

```solidity
File: src/core/MultiRewardDistributor/MultiRewardDistributor.sol

827      function calculateSupplyRewardsForUser(
828          MarketEmissionConfig storage _emissionConfig,
829          uint224 _globalSupplyIndex,
830          uint256 _supplierTokens,
831          address _supplier
832:     ) internal view returns (uint256) {

865      function calculateBorrowRewardsForUser(
866          MarketEmissionConfig storage _emissionConfig,
867          uint224 _globalBorrowIndex,
868          Exp memory _marketBorrowIndex,
869          MTokenData memory _mTokenData,
870          address _borrower
871:     ) internal view returns (uint256) {

912      function calculateNewIndex(
913          uint256 _emissionsPerSecond,
914          uint32 _currentTimestamp,
915          uint224 _currentIndex,
916          uint256 _rewardEndTime,
917          uint256 _denominator
918:     ) internal view returns (IndexUpdate memory) {

976      function fetchConfigByEmissionToken(
977          MToken _mToken,
978          address _emissionToken
979:     ) internal view returns (MarketEmissionConfig storage) {

1001:    function updateMarketSupplyIndexInternal(MToken _mToken) internal {

1041     function disburseSupplierRewardsInternal(
1042         MToken _mToken,
1043         address _supplier,
1044         bool _sendTokens
1045:    ) internal {

1101:    function updateMarketBorrowIndexInternal(MToken _mToken) internal {

1147     function disburseBorrowerRewardsInternal(
1148         MToken _mToken,
1149         address _borrower,
1150         bool _sendTokens
1151:    ) internal {

1214     function sendReward(
1215         address payable _user,
1216         uint256 _amount,
1217         address _rewardToken
1218:    ) internal nonReentrant returns (uint256) {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L827-L832

```solidity
File: src/core/Oracles/ChainlinkOracle.sol

74:      function getPrice(MToken mToken) internal view returns (uint256 price) {

97       function getChainlinkPrice(
98           AggregatorV3Interface feed
99:      ) internal view returns (uint256) {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkOracle.sol#L74-L74


### [N&#x2011;39] Non-`external`/`public` variable names should begin with an underscore
According to the Solidity Style Guide, non-`external`/`public` variable names should begin with an [underscore](https://docs.soliditylang.org/en/latest/style-guide.html#underscore-prefix-for-non-external-functions-and-variables)

*There are 6 instances of this issue:*

```solidity
File: src/core/Comptroller.sol

62:      uint internal constant closeFactorMinMantissa = 0.05e18; // 0.05

65:      uint internal constant closeFactorMaxMantissa = 0.9e18; // 0.9

68:      uint internal constant collateralFactorMaxMantissa = 0.9e18; // 0.9

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Comptroller.sol#L62-L62

```solidity
File: src/core/Governance/TemporalGovernor.sol

55:      mapping(uint16 => EnumerableSet.Bytes32Set) private trustedSenders;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Governance/TemporalGovernor.sol#L55-L55

```solidity
File: src/core/Oracles/ChainlinkOracle.sol

23:      mapping (address => uint256) internal prices;

27:      mapping (bytes32 => AggregatorV3Interface) internal feeds;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkOracle.sol#L23-L23


### [N&#x2011;40] Use of `override` is unnecessary
Starting with Solidity version [0.8.8](https://docs.soliditylang.org/en/v0.8.20/contracts.html#function-overriding), using the `override` keyword when the function solely overrides an interface function, and the function doesn't exist in multiple base contracts, is unnecessary.

*There are 91 instances of this issue:*

<details>
<summary>see instances</summary>


```solidity
File: src/core/Comptroller.sol

102:     function enterMarkets(address[] memory mTokens) override public returns (uint[] memory) {

154:     function exitMarket(address mTokenAddress) override external returns (uint) {

215:     function mintAllowed(address mToken, address minter, uint mintAmount) override external returns (uint) {

251:     function redeemAllowed(address mToken, address redeemer, uint redeemTokens) override external returns (uint) {

292:     function redeemVerify(address mToken, address redeemer, uint redeemAmount, uint redeemTokens) override pure external {

310:     function borrowAllowed(address mToken, address borrower, uint borrowAmount) override external returns (uint) {

366      function repayBorrowAllowed(
367          address mToken,
368          address payer,
369          address borrower,
370:         uint repayAmount) override external returns (uint) {

394      function liquidateBorrowAllowed(
395          address mTokenBorrowed,
396          address mTokenCollateral,
397          address liquidator,
398          address borrower,
399:         uint repayAmount) override external view returns (uint) {

434      function seizeAllowed(
435          address mTokenCollateral,
436          address mTokenBorrowed,
437          address liquidator,
438          address borrower,
439:         uint seizeTokens) override external returns (uint) {

472:     function transferAllowed(address mToken, address src, address dst, uint transferTokens) override external returns (uint) {

629:     function liquidateCalculateSeizeTokens(address mTokenBorrowed, address mTokenCollateral, uint actualRepayAmount) override external view returns (uint, uint) {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Comptroller.sol#L102-L102

```solidity
File: src/core/Governance/TemporalGovernor.sol

106      function allTrustedSenders(uint16 chainId)
107          external
108          view
109          override
110          returns (bytes32[] memory)
111:     {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Governance/TemporalGovernor.sol#L106-L111

```solidity
File: src/core/IRModels/JumpRateModel.sol

84:      function getBorrowRate(uint cash, uint borrows, uint reserves) override public view returns (uint) {

104:     function getSupplyRate(uint cash, uint borrows, uint reserves, uint reserveFactorMantissa) override public view returns (uint) {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/IRModels/JumpRateModel.sol#L84-L84

```solidity
File: src/core/IRModels/WhitePaperInterestRateModel.sol

67:      function getBorrowRate(uint cash, uint borrows, uint reserves) override public view returns (uint) {

80:      function getSupplyRate(uint cash, uint borrows, uint reserves, uint reserveFactorMantissa) override public view returns (uint) {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/IRModels/WhitePaperInterestRateModel.sol#L67-L67

```solidity
File: src/core/MErc20.sol

46:      function mint(uint mintAmount) override external returns (uint) {

61       function mintWithPermit(
62           uint mintAmount,
63           uint deadline,
64           uint8 v, bytes32 r, bytes32 s
65:      ) override external returns (uint) {

87:      function redeem(uint redeemTokens) override external returns (uint) {

97:      function redeemUnderlying(uint redeemAmount) override external returns (uint) {

106:     function borrow(uint borrowAmount) override external returns (uint) {

115:     function repayBorrow(uint repayAmount) override external returns (uint) {

126:     function repayBorrowBehalf(address borrower, uint repayAmount) override external returns (uint) {

139:     function liquidateBorrow(address borrower, uint repayAmount, MTokenInterface mTokenCollateral) override external returns (uint) {

148:     function sweepToken(EIP20NonStandardInterface token) override external {

160:     function _addReserves(uint addAmount) override external returns (uint) {

171:     function getCashPrior() virtual override internal view returns (uint) {

185:     function doTransferIn(address from, uint amount) virtual override internal returns (uint) {

223:     function doTransferOut(address payable to, uint amount) virtual override internal {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20.sol#L46-L46

```solidity
File: src/core/MErc20Delegate.sol

21:      function _becomeImplementation(bytes memory data) virtual override public {

36:      function _resignImplementation() virtual override public {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20Delegate.sol#L21-L21

```solidity
File: src/core/MErc20Delegator.sol

61:      function _setImplementation(address implementation_, bool allowResign, bytes memory becomeImplementationData) override public {

82:      function mint(uint mintAmount) override external returns (uint) {

97       function mintWithPermit(
98           uint mintAmount,
99           uint deadline,
100          uint8 v, bytes32 r, bytes32 s
101:     ) override external returns (uint) {

117:     function redeem(uint redeemTokens) override external returns (uint) {

128:     function redeemUnderlying(uint redeemAmount) override external returns (uint) {

138:     function borrow(uint borrowAmount) override external returns (uint) {

148:     function repayBorrow(uint repayAmount) override external returns (uint) {

159:     function repayBorrowBehalf(address borrower, uint repayAmount) override external returns (uint) {

172:     function liquidateBorrow(address borrower, uint repayAmount, MTokenInterface mTokenCollateral) override external returns (uint) {

183:     function transfer(address dst, uint amount) override external returns (bool) {

195:     function transferFrom(address src, address dst, uint256 amount) override external returns (bool) {

208:     function approve(address spender, uint256 amount) override external returns (bool) {

219:     function allowance(address owner, address spender) override external view returns (uint) {

229:     function balanceOf(address owner) override external view returns (uint) {

240:     function balanceOfUnderlying(address owner) override external returns (uint) {

251:     function getAccountSnapshot(address account) override external view returns (uint, uint, uint, uint) {

260:     function borrowRatePerTimestamp() override external view returns (uint) {

269:     function supplyRatePerTimestamp() override external view returns (uint) {

278:     function totalBorrowsCurrent() override external returns (uint) {

288:     function borrowBalanceCurrent(address account) override external returns (uint) {

298:     function borrowBalanceStored(address account) override public view returns (uint) {

307:     function exchangeRateCurrent() override public returns (uint) {

317:     function exchangeRateStored() override public view returns (uint) {

326:     function getCash() override external view returns (uint) {

336:     function accrueInterest() override public returns (uint) {

350:     function seize(address liquidator, address borrower, uint seizeTokens) override external returns (uint) {

359:     function sweepToken(EIP20NonStandardInterface token) override external {

372:     function _setPendingAdmin(address payable newPendingAdmin) override external returns (uint) {

382:     function _setComptroller(ComptrollerInterface newComptroller) override public returns (uint) {

392:     function _setReserveFactor(uint newReserveFactorMantissa) override external returns (uint) {

402:     function _acceptAdmin() override external returns (uint) {

412:     function _addReserves(uint addAmount) override external returns (uint) {

422:     function _reduceReserves(uint reduceAmount) override external returns (uint) {

433:     function _setInterestRateModel(InterestRateModel newInterestRateModel) override public returns (uint) {

443:     function _setProtocolSeizeShare(uint newProtocolSeizeShareMantissa) override external returns (uint) {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20Delegator.sol#L61-L61

```solidity
File: src/core/MToken.sol

136:     function transfer(address dst, uint256 amount) override external nonReentrant returns (bool) {

147:     function transferFrom(address src, address dst, uint256 amount) override external nonReentrant returns (bool) {

159:     function approve(address spender, uint256 amount) override external returns (bool) {

172:     function allowance(address owner, address spender) override external view returns (uint256) {

181:     function balanceOf(address owner) override external view returns (uint256) {

191:     function balanceOfUnderlying(address owner) override external returns (uint) {

204:     function getAccountSnapshot(address account) override external view returns (uint, uint, uint, uint) {

236:     function borrowRatePerTimestamp() override external view returns (uint) {

244:     function supplyRatePerTimestamp() override external view returns (uint) {

252:     function totalBorrowsCurrent() override external nonReentrant returns (uint) {

262:     function borrowBalanceCurrent(address account) override external nonReentrant returns (uint) {

272:     function borrowBalanceStored(address account) override public view returns (uint) {

319:     function exchangeRateCurrent() override public nonReentrant returns (uint) {

329:     function exchangeRateStored() override public view returns (uint) {

376:     function getCash() override external view returns (uint) {

385:     function accrueInterest() virtual override public returns (uint) {

1048:    function seize(address liquidator, address borrower, uint seizeTokens) override external nonReentrant returns (uint) {

1145:    function _setPendingAdmin(address payable newPendingAdmin) override external returns (uint) {

1168:    function _acceptAdmin() override external returns (uint) {

1195:    function _setComptroller(ComptrollerInterface newComptroller) override public returns (uint) {

1219:    function _setReserveFactor(uint newReserveFactorMantissa) override external nonReentrant returns (uint) {

1326:    function _reduceReserves(uint reduceAmount) override external nonReentrant returns (uint) {

1391:    function _setInterestRateModel(InterestRateModel newInterestRateModel) override public returns (uint) {

1443:    function _setProtocolSeizeShare(uint newProtocolSeizeShareMantissa) override external nonReentrant returns (uint) {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MToken.sol#L136-L136

```solidity
File: src/core/Oracles/ChainlinkOracle.sol

58       function getUnderlyingPrice(
59           MToken mToken
60:      ) public view override returns (uint256) {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkOracle.sol#L58-L60

</details>

### [N&#x2011;41] Array is `push()`ed but not `pop()`ed
Array entries are added but are never removed. Consider whether this should be the case, or whether there should be a maximum, or whether old entries should be removed. Cases where there are specific potential problems will be flagged separately under a different issue.

*There are 3 instances of this issue:*

```solidity
File: src/core/Comptroller.sol

140:         accountAssets[borrower].push(mToken);

798:         allMarkets.push(MToken(mToken));

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Comptroller.sol#L140-L140

```solidity
File: src/core/MultiRewardDistributor/MultiRewardDistributor.sol

462:         MarketEmissionConfig storage newConfig = configs.push();

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L462-L462


### [N&#x2011;42] Consider using `SafeTransferLib.safeTransferETH()` or `Address.sendValue()` for clearer semantic meaning
These Functions indicate their purpose with their name more clearly than using low-level calls.

*There is one instance of this issue:*

```solidity
File: src/core/router/WETHRouter.sol

59:          (bool success, ) = payable(recipient).call{

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/router/WETHRouter.sol#L59-L59


### [N&#x2011;43] Complex casting
Consider whether the number of casts is really necessary, or whether using a different type would be more appropriate. Alternatively, add comments to explain in detail why the casts are necessary.

*There are 2 instances of this issue:*

```solidity
File: src/core/Oracles/ChainlinkOracle.sol

75           EIP20Interface token = EIP20Interface(
76:              MErc20(address(mToken)).underlying()

121      ) external onlyAdmin {
122:         address asset = address(MErc20(address(mToken)).underlying());

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkOracle.sol#L75-L76


### [N&#x2011;44] Large numeric literals should use underscores for readability


*There is one instance of this issue:*

```solidity
File: src/core/IRModels/WhitePaperInterestRateModel.sol

20:      uint public constant timestampsPerYear = 31536000;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/IRModels/WhitePaperInterestRateModel.sol#L20-L20


### [N&#x2011;45] Unused contract variables
Note that there may be cases where a variable appears to be used, but this is only because there are multiple definitions of the varible in different files. In such cases, the variable definition should be moved into a separate file. The instances below are the unused variables.

*There are 2 instances of this issue:*

```solidity
File: src/core/Comptroller.sol

62:      uint internal constant closeFactorMinMantissa = 0.05e18; // 0.05

65:      uint internal constant closeFactorMaxMantissa = 0.9e18; // 0.9

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Comptroller.sol#L62-L62


### [N&#x2011;46] Unused `modifer`s
Note that there may be cases where a modifier superficially appears to be used, but this is only because there are multiple definitions of the modifier in different files. In such cases, the modifier definition should be moved into a separate file. The instances below are the unused modifiers.

*There is one instance of this issue:*

```solidity
File: src/core/Comptroller.sol

1075     modifier nonReentrant() {
1076         // On the first call to nonReentrant, _notEntered will be true
1077         require(_locked != 1, "ReentrancyGuard: reentrant call");
1078 
1079         // Any calls to nonReentrant after this point will fail
1080         _locked = 1;
1081 
1082         _;
1083 
1084         // By storing the original value once again, a refund is triggered (see
1085         // https://eips.ethereum.org/EIPS/eip-2200)
1086         _locked = 0;
1087:    }

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Comptroller.sol#L1075-L1087


### [N&#x2011;47] Use `abi.encodeCall()` instead of `abi.encodeWithSignature()`/`abi.encodeWithSelector()`
`abi.encodeCall()` has compiler [type safety](https://github.com/OpenZeppelin/openzeppelin-contracts/issues/3693), whereas the other two functions do not

*There are 38 instances of this issue:*

```solidity
File: src/core/MErc20Delegator.sol

39:          delegateTo(implementation_, abi.encodeWithSignature("initialize(address,address,address,uint256,string,string,uint8)",

65:              delegateToImplementation(abi.encodeWithSignature("_resignImplementation()"));

71:          delegateToImplementation(abi.encodeWithSignature("_becomeImplementation(bytes)", becomeImplementationData));

83:          bytes memory data = delegateToImplementation(abi.encodeWithSignature("mint(uint256)", mintAmount));

103:             abi.encodeWithSignature(

118:         bytes memory data = delegateToImplementation(abi.encodeWithSignature("redeem(uint256)", redeemTokens));

129:         bytes memory data = delegateToImplementation(abi.encodeWithSignature("redeemUnderlying(uint256)", redeemAmount));

139:         bytes memory data = delegateToImplementation(abi.encodeWithSignature("borrow(uint256)", borrowAmount));

149:         bytes memory data = delegateToImplementation(abi.encodeWithSignature("repayBorrow(uint256)", repayAmount));

160:         bytes memory data = delegateToImplementation(abi.encodeWithSignature("repayBorrowBehalf(address,uint256)", borrower, repayAmount));

173:         bytes memory data = delegateToImplementation(abi.encodeWithSignature("liquidateBorrow(address,uint256,address)", borrower, repayAmount, mTokenCollateral));

184:         bytes memory data = delegateToImplementation(abi.encodeWithSignature("transfer(address,uint256)", dst, amount));

196:         bytes memory data = delegateToImplementation(abi.encodeWithSignature("transferFrom(address,address,uint256)", src, dst, amount));

209:         bytes memory data = delegateToImplementation(abi.encodeWithSignature("approve(address,uint256)", spender, amount));

220:         bytes memory data = delegateToViewImplementation(abi.encodeWithSignature("allowance(address,address)", owner, spender));

230:         bytes memory data = delegateToViewImplementation(abi.encodeWithSignature("balanceOf(address)", owner));

241:         bytes memory data = delegateToImplementation(abi.encodeWithSignature("balanceOfUnderlying(address)", owner));

252:         bytes memory data = delegateToViewImplementation(abi.encodeWithSignature("getAccountSnapshot(address)", account));

261:         bytes memory data = delegateToViewImplementation(abi.encodeWithSignature("borrowRatePerTimestamp()"));

270:         bytes memory data = delegateToViewImplementation(abi.encodeWithSignature("supplyRatePerTimestamp()"));

279:         bytes memory data = delegateToImplementation(abi.encodeWithSignature("totalBorrowsCurrent()"));

289:         bytes memory data = delegateToImplementation(abi.encodeWithSignature("borrowBalanceCurrent(address)", account));

299:         bytes memory data = delegateToViewImplementation(abi.encodeWithSignature("borrowBalanceStored(address)", account));

308:         bytes memory data = delegateToImplementation(abi.encodeWithSignature("exchangeRateCurrent()"));

318:         bytes memory data = delegateToViewImplementation(abi.encodeWithSignature("exchangeRateStored()"));

327:         bytes memory data = delegateToViewImplementation(abi.encodeWithSignature("getCash()"));

337:         bytes memory data = delegateToImplementation(abi.encodeWithSignature("accrueInterest()"));

351:         bytes memory data = delegateToImplementation(abi.encodeWithSignature("seize(address,address,uint256)", liquidator, borrower, seizeTokens));

360:         delegateToImplementation(abi.encodeWithSignature("sweepToken(address)", token));

373:         bytes memory data = delegateToImplementation(abi.encodeWithSignature("_setPendingAdmin(address)", newPendingAdmin));

383:         bytes memory data = delegateToImplementation(abi.encodeWithSignature("_setComptroller(address)", newComptroller));

393:         bytes memory data = delegateToImplementation(abi.encodeWithSignature("_setReserveFactor(uint256)", newReserveFactorMantissa));

403:         bytes memory data = delegateToImplementation(abi.encodeWithSignature("_acceptAdmin()"));

413:         bytes memory data = delegateToImplementation(abi.encodeWithSignature("_addReserves(uint256)", addAmount));

423:         bytes memory data = delegateToImplementation(abi.encodeWithSignature("_reduceReserves(uint256)", reduceAmount));

434:         bytes memory data = delegateToImplementation(abi.encodeWithSignature("_setInterestRateModel(address)", newInterestRateModel));

444:         bytes memory data = delegateToImplementation(abi.encodeWithSignature("_setProtocolSeizeShare(uint256)", newProtocolSeizeShareMantissa));

483:         (bool success, bytes memory returnData) = address(this).staticcall(abi.encodeWithSignature("delegateToImplementation(bytes)", data));

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20Delegator.sol#L39-L39


### [N&#x2011;48] `abi.encodeSignature()` does not work properly with aliases
In Solidity, `uint` is equivalent to `uint256`, and `int` to `int256`. However, when it comes to `abi.encodeSignature()`, only the '256' versions work properly. If only `uint` or `int` are used, the function will revert when called. Use `abi.encodeCall()` instead, which does not have this issue.

*There are 38 instances of this issue:*

```solidity
File: src/core/MErc20Delegator.sol

39:          delegateTo(implementation_, abi.encodeWithSignature("initialize(address,address,address,uint256,string,string,uint8)",

65:              delegateToImplementation(abi.encodeWithSignature("_resignImplementation()"));

71:          delegateToImplementation(abi.encodeWithSignature("_becomeImplementation(bytes)", becomeImplementationData));

83:          bytes memory data = delegateToImplementation(abi.encodeWithSignature("mint(uint256)", mintAmount));

103:             abi.encodeWithSignature(

118:         bytes memory data = delegateToImplementation(abi.encodeWithSignature("redeem(uint256)", redeemTokens));

129:         bytes memory data = delegateToImplementation(abi.encodeWithSignature("redeemUnderlying(uint256)", redeemAmount));

139:         bytes memory data = delegateToImplementation(abi.encodeWithSignature("borrow(uint256)", borrowAmount));

149:         bytes memory data = delegateToImplementation(abi.encodeWithSignature("repayBorrow(uint256)", repayAmount));

160:         bytes memory data = delegateToImplementation(abi.encodeWithSignature("repayBorrowBehalf(address,uint256)", borrower, repayAmount));

173:         bytes memory data = delegateToImplementation(abi.encodeWithSignature("liquidateBorrow(address,uint256,address)", borrower, repayAmount, mTokenCollateral));

184:         bytes memory data = delegateToImplementation(abi.encodeWithSignature("transfer(address,uint256)", dst, amount));

196:         bytes memory data = delegateToImplementation(abi.encodeWithSignature("transferFrom(address,address,uint256)", src, dst, amount));

209:         bytes memory data = delegateToImplementation(abi.encodeWithSignature("approve(address,uint256)", spender, amount));

220:         bytes memory data = delegateToViewImplementation(abi.encodeWithSignature("allowance(address,address)", owner, spender));

230:         bytes memory data = delegateToViewImplementation(abi.encodeWithSignature("balanceOf(address)", owner));

241:         bytes memory data = delegateToImplementation(abi.encodeWithSignature("balanceOfUnderlying(address)", owner));

252:         bytes memory data = delegateToViewImplementation(abi.encodeWithSignature("getAccountSnapshot(address)", account));

261:         bytes memory data = delegateToViewImplementation(abi.encodeWithSignature("borrowRatePerTimestamp()"));

270:         bytes memory data = delegateToViewImplementation(abi.encodeWithSignature("supplyRatePerTimestamp()"));

279:         bytes memory data = delegateToImplementation(abi.encodeWithSignature("totalBorrowsCurrent()"));

289:         bytes memory data = delegateToImplementation(abi.encodeWithSignature("borrowBalanceCurrent(address)", account));

299:         bytes memory data = delegateToViewImplementation(abi.encodeWithSignature("borrowBalanceStored(address)", account));

308:         bytes memory data = delegateToImplementation(abi.encodeWithSignature("exchangeRateCurrent()"));

318:         bytes memory data = delegateToViewImplementation(abi.encodeWithSignature("exchangeRateStored()"));

327:         bytes memory data = delegateToViewImplementation(abi.encodeWithSignature("getCash()"));

337:         bytes memory data = delegateToImplementation(abi.encodeWithSignature("accrueInterest()"));

351:         bytes memory data = delegateToImplementation(abi.encodeWithSignature("seize(address,address,uint256)", liquidator, borrower, seizeTokens));

360:         delegateToImplementation(abi.encodeWithSignature("sweepToken(address)", token));

373:         bytes memory data = delegateToImplementation(abi.encodeWithSignature("_setPendingAdmin(address)", newPendingAdmin));

383:         bytes memory data = delegateToImplementation(abi.encodeWithSignature("_setComptroller(address)", newComptroller));

393:         bytes memory data = delegateToImplementation(abi.encodeWithSignature("_setReserveFactor(uint256)", newReserveFactorMantissa));

403:         bytes memory data = delegateToImplementation(abi.encodeWithSignature("_acceptAdmin()"));

413:         bytes memory data = delegateToImplementation(abi.encodeWithSignature("_addReserves(uint256)", addAmount));

423:         bytes memory data = delegateToImplementation(abi.encodeWithSignature("_reduceReserves(uint256)", reduceAmount));

434:         bytes memory data = delegateToImplementation(abi.encodeWithSignature("_setInterestRateModel(address)", newInterestRateModel));

444:         bytes memory data = delegateToImplementation(abi.encodeWithSignature("_setProtocolSeizeShare(uint256)", newProtocolSeizeShareMantissa));

483:         (bool success, bytes memory returnData) = address(this).staticcall(abi.encodeWithSignature("delegateToImplementation(bytes)", data));

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20Delegator.sol#L39-L39


### [N&#x2011;49] Consider using descriptive `constant`s when passing zero as a function argument
Passing zero as a function argument can sometimes result in a security issue (e.g. passing zero as the slippage parameter). Consider using a `constant` variable with a descriptive name, so it's clear that the argument is intentionally being used, and for the right reasons.

*There are 6 instances of this issue:*

```solidity
File: src/core/Comptroller.sol

274:         (Error err, , uint shortfall) = getHypotheticalAccountLiquidityInternal(redeemer, MToken(mToken), redeemTokens, 0);

344:         (Error err, , uint shortfall) = getHypotheticalAccountLiquidityInternal(borrower, MToken(mToken), 0, borrowAmount);

517:         (Error err, uint liquidity, uint shortfall) = getHypotheticalAccountLiquidityInternal(account, MToken(address(0)), 0, 0);

529:         return getHypotheticalAccountLiquidityInternal(account, MToken(address(0)), 0, 0);

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Comptroller.sol#L274-L274

```solidity
File: src/core/MToken.sol

578:         return redeemFresh(payable(msg.sender), redeemTokens, 0);

594:         return redeemFresh(payable(msg.sender), 0, redeemAmount);

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MToken.sol#L578-L578


### [N&#x2011;50] Assembly block creates dirty bits
Writing data to the free memory pointer without later updating the free memory pointer will cause there to be dirty bits at that memory location. Not updating the free memory pointer will make it [harder](https://docs.soliditylang.org/en/latest/ir-breaking-changes.html#cleanup) for the optimizer to reason about whether the memory needs to be cleaned before use, which will lead to worse optimizations. Update the free memory pointer and annotate the block (`assembly (\"memory-safe\") { ... }`) to avoid this issue.

*There are 2 instances of this issue:*

```solidity
File: src/core/MErc20Delegator.sol

502          assembly {
503              let free_mem_ptr := mload(0x40)
504              returndatacopy(free_mem_ptr, 0, returndatasize())
505  
506              switch success
507              case 0 { revert(free_mem_ptr, returndatasize()) }
508              default { return(free_mem_ptr, returndatasize()) }
509:         }

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20Delegator.sol#L502-L509

```solidity
File: src/core/Unitroller.sol

140          assembly {
141                let free_mem_ptr := mload(0x40)
142                returndatacopy(free_mem_ptr, 0, returndatasize())
143  
144                switch success
145                case 0 { revert(free_mem_ptr, returndatasize()) }
146                default { return(free_mem_ptr, returndatasize()) }
147:         }

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Unitroller.sol#L140-L147


### [N&#x2011;51] Constants in comparisons should appear on the left side
Doing so will prevent [typo bugs](https://www.moserware.com/2008/01/constants-on-left-are-better-but-this.html)

*There are 55 instances of this issue:*

<details>
<summary>see instances</summary>


```solidity
File: src/core/Comptroller.sol

129:         if (marketToJoin.accountMembership[borrower] == true) {

158:         require(oErr == 0, "exitMarket: getAccountSnapshot failed"); // semi-opaque error code

161:         if (amountOwed != 0) {

167:         if (allowed != 0) {

228:         if (supplyCap != 0) {

298:         if (redeemTokens == 0 && redeemAmount > 0) {

332:         if (oracle.getUnderlyingPrice(MToken(mToken)) == 0) {

338:         if (borrowCap != 0) {

412:         if (shortfall == 0) {

579:             if (oErr != 0) { // semi-opaque error code, we assume NO_ERROR == 0 is invariant between upgrades

587:             if (vars.oraclePriceMantissa == 0) {

633:         if (priceBorrowedMantissa == 0 || priceCollateralMantissa == 0) {

728:         if (newCollateralFactorMantissa != 0 && oracle.getUnderlyingPrice(mToken) == 0) {

813:         require(numMarkets != 0 && numMarkets == numBorrowCaps, "invalid input");

850:         require(numMarkets != 0 && numMarkets == numSupplyCaps, "invalid input");

914:         require(msg.sender == admin || state == true, "only admin can unpause");

924:         require(msg.sender == admin || state == true, "only admin can unpause");

933:         require(msg.sender == admin || state == true, "only admin can unpause");

942:         require(msg.sender == admin || state == true, "only admin can unpause");

951:         require(unitroller._acceptImplementation() == 0, "change not authorized");

1038:            if (suppliers == true) {

1046:            if (borrowers == true) {

1077:        require(_locked != 1, "ReentrancyGuard: reentrant call");

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Comptroller.sol#L129-L129

```solidity
File: src/core/Governance/TemporalGovernor.sol

332:             queuedTransactions[vm.hash].queueTime == 0,

364:         } else if (queuedTransactions[vm.hash].queueTime == 0) {

356:                 queuedTransactions[vm.hash].queueTime != 0,

417:         require(targets.length != 0, "TemporalGovernor: Empty proposal");

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Governance/TemporalGovernor.sol#L332-L332

```solidity
File: src/core/IRModels/JumpRateModel.sol

70:          if (borrows == 0) {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/IRModels/JumpRateModel.sol#L70-L70

```solidity
File: src/core/IRModels/WhitePaperInterestRateModel.sol

53:          if (borrows == 0) {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/IRModels/WhitePaperInterestRateModel.sol#L53-L53

```solidity
File: src/core/MErc20Delegator.sol

497:         require(msg.value == 0,"MErc20Delegator:fallback: cannot send value to fallback");

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20Delegator.sol#L497-L497

```solidity
File: src/core/MToken.sol

33:          require(accrualBlockTimestamp == 0 && borrowIndex == 0, "market may only be initialized once");

71:          if (allowed != 0) {

295:         if (borrowSnapshot.principal == 0) {

342:         if (_totalSupply == 0) {

501:         if (allowed != 0) {

616:         require(redeemTokensIn == 0 || redeemAmountIn == 0, "one of redeemTokensIn or redeemAmountIn must be zero");

668:         if (allowed != 0) {

753:         if (allowed != 0) {

868:         if (allowed != 0) {

971:         if (allowed != 0) {

991:         if (repayAmount == 0) {

1077:        if (allowed != 0) {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MToken.sol#L33-L33

```solidity
File: src/core/MultiRewardDistributor/MultiRewardDistributor.sol

837:             userSupplyIndex == 0 && _globalSupplyIndex >= initialIndexConstant

876:             userBorrowIndex == 0 && _globalBorrowIndex >= initialIndexConstant

948:         if (deltaTimestamps == 0 || _emissionsPerSecond == 0) {

1220:        if (_amount == 0) {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L837-L837

```solidity
File: src/core/Oracles/ChainlinkOracle.sol

79:          if (prices[address(token)] != 0) {

103:         require(updatedAt != 0, "Round is in incompleted state");

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkOracle.sol#L79-L79

```solidity
File: src/core/router/WETHRouter.sol

34:          require(mToken.mint(msg.value) == 0, "WETHRouter: mint failed");

53:              mToken.redeem(mTokenRedeemAmount) == 0,

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/router/WETHRouter.sol#L34-L34

</details>

### [N&#x2011;52] Consider disabling `renounceOwnership()`
If the plan for your project does not include eventually giving up all ownership control, consider overwriting OpenZeppelin's `Ownable`'s `renounceOwnership()` function in order to disable it.

*There is one instance of this issue:*

```solidity
File: src/core/Governance/TemporalGovernor.sol

27:  contract TemporalGovernor is ITemporalGovernor, Ownable, Pausable {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Governance/TemporalGovernor.sol#L27-L27


### [N&#x2011;53] `else`-block not required
One level of nesting can be removed by not having an `else` block when the `if`-block returns, and `if (foo) { return 1; } else { return 2; }` becomes `if (foo) { return 1; } return 2;`

*There are 6 instances of this issue:*

```solidity
File: src/core/Comptroller.sol

614          if (vars.sumCollateral > vars.sumBorrowPlusEffects) {
615              return (Error.NO_ERROR, vars.sumCollateral - vars.sumBorrowPlusEffects, 0);
616          } else {
617              return (Error.NO_ERROR, 0, vars.sumBorrowPlusEffects - vars.sumCollateral);
618:         }

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Comptroller.sol#L614-L618

```solidity
File: src/core/IRModels/JumpRateModel.sol

87           if (util <= kink) {
88               return util.mul(multiplierPerTimestamp).div(1e18).add(baseRatePerTimestamp);
89           } else {
90               uint normalRate = kink.mul(multiplierPerTimestamp).div(1e18).add(baseRatePerTimestamp);
91               uint excessUtil = util.sub(kink);
92               return excessUtil.mul(jumpMultiplierPerTimestamp).div(1e18).add(normalRate);
93:          }

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/IRModels/JumpRateModel.sol#L87-L93

```solidity
File: src/core/MToken.sol

342          if (_totalSupply == 0) {
343              /*
344               * If there are no tokens minted:
345               *  exchangeRate = initialExchangeRate
346               */
347              return (MathError.NO_ERROR, initialExchangeRateMantissa);
348          } else {
349              /*
350               * Otherwise:
351               *  exchangeRate = (totalCash + totalBorrows - totalReserves) / totalSupply
352               */
353              uint totalCash = getCashPrior();
354              uint cashPlusBorrowsMinusReserves;
355              Exp memory exchangeRate;
356              MathError mathErr;
357  
358              (mathErr, cashPlusBorrowsMinusReserves) = addThenSubUInt(totalCash, totalBorrows, totalReserves);
359              if (mathErr != MathError.NO_ERROR) {
360                  return (mathErr, 0);
361              }
362  
363              (mathErr, exchangeRate) = getExp(cashPlusBorrowsMinusReserves, _totalSupply);
364              if (mathErr != MathError.NO_ERROR) {
365                  return (mathErr, 0);
366              }
367  
368              return (MathError.NO_ERROR, exchangeRate.mantissa);
369:         }

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MToken.sol#L342-L369

```solidity
File: src/core/Oracles/ChainlinkOracle.sol

62           if (keccak256(abi.encodePacked(symbol)) == nativeToken) { /// @dev this branch should never get called as native tokens are not supported on this deployment
63               return getChainlinkPrice(getFeed(symbol));
64           } else {
65               return getPrice(mToken);
66:          }

87           if (decimalDelta > 0) {
88               return price.mul(10 ** decimalDelta);
89           } else {
90               return price;
91:          }

108          if (decimalDelta > 0) {
109              return uint256(answer).mul(10 ** decimalDelta);
110          } else {
111              return uint256(answer);
112:         }

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkOracle.sol#L62-L66


### [N&#x2011;54] Consider adding emergency-stop functionality
Adding a way to quickly halt protocol functionality in an emergency, rather than having to pause individual contracts one-by-one, will make in-progress hack mitigation faster and much less stressful.

*There is one instance of this issue:*

```solidity
File: src/core/Governance/TemporalGovernor.sol

27:  contract TemporalGovernor is ITemporalGovernor, Ownable, Pausable {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Governance/TemporalGovernor.sol#L27-L27


### [N&#x2011;55] Cast to `bytes` or `bytes32` for clearer semantic meaning
Using a [cast](https://ethereum.stackexchange.com/questions/30912/how-to-compare-strings-in-solidity#answer-82739) on a single argument, rather than `abi.encodePacked()` makes the intended operation more clear, leading to less reviewer confusion.

*There are 4 instances of this issue:*

```solidity
File: src/core/Oracles/ChainlinkOracle.sol

46:          nativeToken = keccak256(abi.encodePacked(_nativeToken));

62:          if (keccak256(abi.encodePacked(symbol)) == nativeToken) { /// @dev this branch should never get called as native tokens are not supported on this deployment

150:         feeds[keccak256(abi.encodePacked(symbol))] = AggregatorV3Interface(

161:         return feeds[keccak256(abi.encodePacked(symbol))];

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkOracle.sol#L46-L46


### [N&#x2011;56] Use `string.concat()` on strings instead of `abi.encodePacked()` for clearer semantic meaning
Starting with version 0.8.12, Solidity has the `string.concat()` function, which allows one to concatenate a list of strings, without extra padding. Using this function rather than `abi.encodePacked()` makes the intended operation more clear, leading to less reviewer confusion.

*There are 4 instances of this issue:*

```solidity
File: src/core/Oracles/ChainlinkOracle.sol

46:          nativeToken = keccak256(abi.encodePacked(_nativeToken));

62:          if (keccak256(abi.encodePacked(symbol)) == nativeToken) { /// @dev this branch should never get called as native tokens are not supported on this deployment

150:         feeds[keccak256(abi.encodePacked(symbol))] = AggregatorV3Interface(

161:         return feeds[keccak256(abi.encodePacked(symbol))];

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkOracle.sol#L46-L46


### [N&#x2011;57] Events may be emitted out of order due to reentrancy
Ensure that events follow the best practice of check-effects-interaction, and are emitted before external calls

*There are 4 instances of this issue:*

```solidity
File: src/core/Comptroller.sol

/// @audit push() prior to emission of MarketEntered()
142:         emit MarketEntered(mToken, borrower);

/// @audit pop() prior to emission of MarketExited()
201:         emit MarketExited(mToken, msg.sender);

/// @audit getUnderlyingPrice() prior to emission of NewCollateralFactor()
737:         emit NewCollateralFactor(mToken, oldCollateralFactorMantissa, newCollateralFactorMantissa);

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Comptroller.sol#L142-L142

```solidity
File: src/core/MultiRewardDistributor/MultiRewardDistributor.sol

/// @audit safeTransfer() prior to emission of FundsRescued()
486:         emit FundsRescued(_tokenAddress, _amount);

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L486-L486


### [N&#x2011;58] Consider bounding input array length
The functions below take in an unbounded array, and make function calls for entries in the array. While the function will revert if it eventually runs out of gas, it may be a nicer user experience to `require()` that the length of the array is below some reasonable maximum, so that the user doesn't have to use up a full transaction's gas only to see that the transaction reverts.

*There are 9 instances of this issue:*

```solidity
File: src/core/Comptroller.sol

106          for (uint i = 0; i < len; i++) {
107              MToken mToken = MToken(mTokens[i]);
108  
109              results[i] = uint(addToMarketInternal(mToken, msg.sender));
110:         }

815          for(uint i = 0; i < numMarkets; i++) {
816              borrowCaps[address(mTokens[i])] = newBorrowCaps[i];
817              emit NewBorrowCap(mTokens[i], newBorrowCaps[i]);
818:         }

852          for(uint i = 0; i < numMarkets; i++) {
853              supplyCaps[address(mTokens[i])] = newSupplyCaps[i];
854              emit NewSupplyCap(mTokens[i], newSupplyCaps[i]);
855:         }

1031         for (uint i = 0; i < mTokens.length; i++) {
1032 
1033             // Safety check that the supplied mTokens are active/listed
1034             MToken mToken = mTokens[i];
1035             require(markets[address(mToken)].isListed, "market must be listed");
1036 
1037             // Disburse supply side
1038             if (suppliers == true) {
1039                 rewardDistributor.updateMarketSupplyIndex(mToken);
1040                 for (uint holderIndex = 0; holderIndex < holders.length; holderIndex++) {
1041                     rewardDistributor.disburseSupplierRewards(mToken, holders[holderIndex], true);
1042                 }
1043             }
1044 
1045             // Disburse borrow side
1046             if (borrowers == true) {
1047                 rewardDistributor.updateMarketBorrowIndex(mToken);
1048                 for (uint holderIndex = 0; holderIndex < holders.length; holderIndex++) {
1049                     rewardDistributor.disburseBorrowerRewards(mToken, holders[holderIndex], true);
1050                 }
1051             }
1052:        }

1040                 for (uint holderIndex = 0; holderIndex < holders.length; holderIndex++) {
1041                     rewardDistributor.disburseSupplierRewards(mToken, holders[holderIndex], true);
1042:                }

1048                 for (uint holderIndex = 0; holderIndex < holders.length; holderIndex++) {
1049                     rewardDistributor.disburseBorrowerRewards(mToken, holders[holderIndex], true);
1050:                }

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Comptroller.sol#L106-L110

```solidity
File: src/core/Governance/TemporalGovernor.sol

74           for (uint256 i = 0; i < _trustedSenders.length; i++) {
75               trustedSenders[_trustedSenders[i].chainId].add(
76                   addressToBytes(_trustedSenders[i].addr)
77               );
78:          }

146              for (uint256 i = 0; i < _trustedSenders.length; i++) {
147                  trustedSenders[_trustedSenders[i].chainId].add(
148                      addressToBytes(_trustedSenders[i].addr)
149                  );
150  
151                  emit TrustedSenderUpdated(
152                      _trustedSenders[i].chainId,
153                      _trustedSenders[i].addr,
154                      true /// added to list
155                  );
156:             }

173              for (uint256 i = 0; i < _trustedSenders.length; i++) {
174                  trustedSenders[_trustedSenders[i].chainId].remove(
175                      addressToBytes(_trustedSenders[i].addr)
176                  );
177  
178                  emit TrustedSenderUpdated(
179                      _trustedSenders[i].chainId,
180                      _trustedSenders[i].addr,
181                      false /// removed from list
182                  );
183:             }

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Governance/TemporalGovernor.sol#L74-L78


### [N&#x2011;59] `if`-statement can be converted to a ternary
The code can be made more compact while also increasing readability by converting the following `if`-statements to ternaries (e.g. `foo += (x > y) ? a : b`)

*There are 4 instances of this issue:*

```solidity
File: src/core/MToken.sol

82           if (spender == src) {
83               startingAllowance = type(uint).max;
84           } else {
85               startingAllowance = transferAllowances[src][spender];
86:          }

633              if (redeemTokensIn == type(uint).max) {
634                  vars.redeemTokens = accountTokens[redeemer];
635              } else {
636                  vars.redeemTokens = redeemTokensIn;
637:             }

889          if (repayAmount == type(uint).max) {
890              vars.repayAmount = vars.accountBorrows;
891          } else {
892              vars.repayAmount = repayAmount;
893:         }

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MToken.sol#L82-L86

```solidity
File: src/core/MultiRewardDistributor/MultiRewardDistributor.sol

937              if (_currentTimestamp < _rewardEndTime) {
938                  deltaTimestamps = sub_(_rewardEndTime, _currentTimestamp);
939              } else {
940                  // Otherwise just set deltaTimestamps to 0 to ensure that we short circuit
941                  // in the next step
942                  deltaTimestamps = 0;
943:             }

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L937-L943


### [N&#x2011;60] Consider moving `msg.sender` checks to a common authorization `modifier`


*There are 15 instances of this issue:*

```solidity
File: src/core/Comptroller.sol

320:             require(msg.sender == mToken, "sender must be mToken");

691:     	require(msg.sender == admin, "only admin can set close factor");

826:         require(msg.sender == admin, "only admin can set borrow cap guardian");

863:         require(msg.sender == admin, "only admin can set supply cap guardian");

902:         require(msg.sender == admin, "Unauthorized");

950:         require(msg.sender == unitroller.admin(), "only unitroller admin can change brains");

960:         require(msg.sender == admin, "Unauthorized");

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Comptroller.sol#L320-L320

```solidity
File: src/core/Governance/TemporalGovernor.sol

141:             msg.sender == address(this),

168:             msg.sender == address(this),

190:             msg.sender == address(this),

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Governance/TemporalGovernor.sol#L141-L141

```solidity
File: src/core/MErc20.sol

149:         require(msg.sender == admin, "MErc20::sweepToken: only admin can sweep tokens");

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20.sol#L149-L149

```solidity
File: src/core/MErc20Delegate.sol

30:          require(msg.sender == admin, "only the admin may call _becomeImplementation");

42:          require(msg.sender == admin, "only the admin may call _resignImplementation");

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20Delegate.sol#L30-L30

```solidity
File: src/core/MErc20Delegator.sol

62:          require(msg.sender == admin, "MErc20Delegator::_setImplementation: Caller must be admin");

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20Delegator.sol#L62-L62

```solidity
File: src/core/MToken.sol

32:          require(msg.sender == admin, "only admin may initialize the market");

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MToken.sol#L32-L32


### [N&#x2011;61] Setters should prevent re-setting of the same value
This especially problematic when the setter also emits the same value, which may be confusing to offline parsers

*There are 2 instances of this issue:*

```solidity
File: src/core/MultiRewardDistributor/MultiRewardDistributor.sol

512      function _setEmissionCap(
513          uint256 _newEmissionCap
514      ) external onlyComptrollersAdmin {
515          uint256 oldEmissionCap = emissionCap;
516  
517          emissionCap = _newEmissionCap;
518  
519          emit NewEmissionCap(oldEmissionCap, _newEmissionCap);
520:     }

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L512-L520

```solidity
File: src/core/Oracles/ChainlinkOracle.sol

173      function setAdmin(address newAdmin) external onlyAdmin {
174          address oldAdmin = admin;
175          admin = newAdmin;
176  
177          emit NewAdmin(oldAdmin, newAdmin);
178:     }

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkOracle.sol#L173-L178


### [N&#x2011;62] Imports could be organized more systematically
The contract's interface should be imported first, followed by each of the interfaces it uses, followed by all other files. The examples below do not follow this layout.

*There are 2 instances of this issue:*

```solidity
File: src/core/MultiRewardDistributor/MultiRewardDistributor.sol

8:   import {IERC20} from "@openzeppelin/contracts/token/ERC20/IERC20.sol";

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L8-L8

```solidity
File: src/core/router/WETHRouter.sol

4:   import {IERC20} from "@openzeppelin/contracts/token/ERC20/IERC20.sol";

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/router/WETHRouter.sol#L4-L4


### [N&#x2011;63] Polymorphic functions make security audits more time-consuming and error-prone
The instances below point to one of two functions with the same name. Consider naming each function differently, in order to make code navigation and analysis easier.

*There are 5 instances of this issue:*

```solidity
File: src/core/Comptroller.sol

1006:    function claimReward(address holder) public {

1015:    function claimReward(address holder, MToken[] memory mTokens) public {

1028:    function claimReward(address[] memory holders, MToken[] memory mTokens, bool borrowers, bool suppliers) public {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Comptroller.sol#L1006-L1006

```solidity
File: src/core/Governance/TemporalGovernor.sol

96       function isTrustedSender(
97           uint16 chainId,
98           address addr
99:      ) external view returns (bool) {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Governance/TemporalGovernor.sol#L96-L99

```solidity
File: src/core/MultiRewardDistributor/MultiRewardDistributor.sol

256      function getOutstandingRewardsForUser(
257          MToken _mToken,
258          address _user
259:     ) public view returns (RewardInfo[] memory) {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L256-L259


### [N&#x2011;64] Long functions should be refactored into multiple, smaller, functions


*There are 2 instances of this issue:*

```solidity
File: src/core/MToken.sol

385:     function accrueInterest() virtual override public returns (uint) {

615:     function redeemFresh(address payable redeemer, uint redeemTokensIn, uint redeemAmountIn) internal returns (uint) {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MToken.sol#L385-L385


### [N&#x2011;65] Mixed usage of `int`/`uint` with `int256`/`uint256`
`int256`/`uint256` are the preferred type names (they're what are used for function signatures), so they should be used consistently

*There are 578 instances of this issue:*

<details>
<summary>see instances</summary>


```solidity
File: src/core/Comptroller.sol

26:      event NewCloseFactor(uint oldCloseFactorMantissa, uint newCloseFactorMantissa);

29:      event NewCollateralFactor(MToken mToken, uint oldCollateralFactorMantissa, uint newCollateralFactorMantissa);

32:      event NewLiquidationIncentive(uint oldLiquidationIncentiveMantissa, uint newLiquidationIncentiveMantissa);

47:      event NewBorrowCap(MToken indexed mToken, uint newBorrowCap);

53:      event NewSupplyCap(MToken indexed mToken, uint newSupplyCap);

62:      uint internal constant closeFactorMinMantissa = 0.05e18; // 0.05

65:      uint internal constant closeFactorMaxMantissa = 0.9e18; // 0.9

68:      uint internal constant collateralFactorMaxMantissa = 0.9e18; // 0.9

103:         uint len = mTokens.length;

109:             results[i] = uint(addToMarketInternal(mToken, msg.sender));

106:         for (uint i = 0; i < len; i++) {

157:         (uint oErr, uint tokensHeld, uint amountOwed, ) = mToken.getAccountSnapshot(msg.sender);

166:         uint allowed = redeemAllowedInternal(mTokenAddress, msg.sender, tokensHeld);

175:             return uint(Error.NO_ERROR);

184:         uint len = userAssetList.length;

185:         uint assetIndex = len;

186:         for (uint i = 0; i < len; i++) {

203:         return uint(Error.NO_ERROR);

154:     function exitMarket(address mTokenAddress) override external returns (uint) {

223:             return uint(Error.MARKET_NOT_LISTED);

226:         uint supplyCap = supplyCaps[mToken];

229:             uint totalCash = MToken(mToken).getCash();

230:             uint totalBorrows = MToken(mToken).totalBorrows();

231:             uint totalReserves = MToken(mToken).totalReserves();

233:             uint totalSupplies = sub_(add_(totalCash, totalBorrows), totalReserves);

235:             uint nextTotalSupplies = add_(totalSupplies, mintAmount);

241:         return uint(Error.NO_ERROR);

215:     function mintAllowed(address mToken, address minter, uint mintAmount) override external returns (uint) {

252:         uint allowed = redeemAllowedInternal(mToken, redeemer, redeemTokens);

253:         if (allowed != uint(Error.NO_ERROR)) {

260:         return uint(Error.NO_ERROR);

251:     function redeemAllowed(address mToken, address redeemer, uint redeemTokens) override external returns (uint) {

265:             return uint(Error.MARKET_NOT_LISTED);

270:             return uint(Error.NO_ERROR);

274:         (Error err, , uint shortfall) = getHypotheticalAccountLiquidityInternal(redeemer, MToken(mToken), redeemTokens, 0);

276:             return uint(err);

279:             return uint(Error.INSUFFICIENT_LIQUIDITY);

282:         return uint(Error.NO_ERROR);

263:     function redeemAllowedInternal(address mToken, address redeemer, uint redeemTokens) internal view returns (uint) {

292:     function redeemVerify(address mToken, address redeemer, uint redeemAmount, uint redeemTokens) override pure external {

315:             return uint(Error.MARKET_NOT_LISTED);

325:                 return uint(addToMarketErr);

333:             return uint(Error.PRICE_ERROR);

336:         uint borrowCap = borrowCaps[mToken];

339:             uint totalBorrows = MToken(mToken).totalBorrows();

340:             uint nextTotalBorrows = add_(totalBorrows, borrowAmount);

344:         (Error err, , uint shortfall) = getHypotheticalAccountLiquidityInternal(borrower, MToken(mToken), 0, borrowAmount);

346:             return uint(err);

349:             return uint(Error.INSUFFICIENT_LIQUIDITY);

355:         return uint(Error.NO_ERROR);

310:     function borrowAllowed(address mToken, address borrower, uint borrowAmount) override external returns (uint) {

377:             return uint(Error.MARKET_NOT_LISTED);

383:         return uint(Error.NO_ERROR);

370:         uint repayAmount) override external returns (uint) {

404:             return uint(Error.MARKET_NOT_LISTED);

408:         (Error err, , uint shortfall) = getAccountLiquidityInternal(borrower);

410:             return uint(err);

413:             return uint(Error.INSUFFICIENT_SHORTFALL);

417:         uint borrowBalance = MToken(mTokenBorrowed).borrowBalanceStored(borrower);

418:         uint maxClose = mul_ScalarTruncate(Exp({mantissa: closeFactorMantissa}), borrowBalance);

420:             return uint(Error.TOO_MUCH_REPAY);

423:         return uint(Error.NO_ERROR);

399:         uint repayAmount) override external view returns (uint) {

447:             return uint(Error.MARKET_NOT_LISTED);

451:             return uint(Error.COMPTROLLER_MISMATCH);

461:         return uint(Error.NO_ERROR);

439:         uint seizeTokens) override external returns (uint) {

478:         uint allowed = redeemAllowedInternal(mToken, src, transferTokens);

479:         if (allowed != uint(Error.NO_ERROR)) {

487:         return uint(Error.NO_ERROR);

472:     function transferAllowed(address mToken, address src, address dst, uint transferTokens) override external returns (uint) {

498:         uint sumCollateral;

499:         uint sumBorrowPlusEffects;

500:         uint mTokenBalance;

501:         uint borrowBalance;

502:         uint exchangeRateMantissa;

503:         uint oraclePriceMantissa;

517:         (Error err, uint liquidity, uint shortfall) = getHypotheticalAccountLiquidityInternal(account, MToken(address(0)), 0, 0);

519:         return (uint(err), liquidity, shortfall);

516:     function getAccountLiquidity(address account) public view returns (uint, uint, uint) {

528:     function getAccountLiquidityInternal(address account) internal view returns (Error, uint, uint) {

547:         (Error err, uint liquidity, uint shortfall) = getHypotheticalAccountLiquidityInternal(account, MToken(mTokenModify), redeemTokens, borrowAmount);

548:         return (uint(err), liquidity, shortfall);

545:         uint redeemTokens,

546:         uint borrowAmount) public view returns (uint, uint, uint) {

570:         uint oErr;

574:         for (uint i = 0; i < assets.length; i++) {

566:         uint redeemTokens,

567:         uint borrowAmount) internal view returns (Error, uint, uint) {

631:         uint priceBorrowedMantissa = oracle.getUnderlyingPrice(MToken(mTokenBorrowed));

632:         uint priceCollateralMantissa = oracle.getUnderlyingPrice(MToken(mTokenCollateral));

634:             return (uint(Error.PRICE_ERROR), 0);

643:         uint exchangeRateMantissa = MToken(mTokenCollateral).exchangeRateStored(); // Note: reverts on error

644:         uint seizeTokens;

655:         return (uint(Error.NO_ERROR), seizeTokens);

629:     function liquidateCalculateSeizeTokens(address mTokenBorrowed, address mTokenCollateral, uint actualRepayAmount) override external view returns (uint, uint) {

680:         return uint(Error.NO_ERROR);

665:     function _setPriceOracle(PriceOracle newOracle) public returns (uint) {

693:         uint oldCloseFactorMantissa = closeFactorMantissa;

697:         return uint(Error.NO_ERROR);

689:     function _setCloseFactor(uint newCloseFactorMantissa) external returns (uint) {

733:         uint oldCollateralFactorMantissa = market.collateralFactorMantissa;

739:         return uint(Error.NO_ERROR);

707:     function _setCollateralFactor(MToken mToken, uint newCollateralFactorMantissa) external returns (uint) {

755:         uint oldLiquidationIncentiveMantissa = liquidationIncentiveMantissa;

763:         return uint(Error.NO_ERROR);

748:     function _setLiquidationIncentive(uint newLiquidationIncentiveMantissa) external returns (uint) {

791:         return uint(Error.NO_ERROR);

772:     function _supportMarket(MToken mToken) external returns (uint) {

795:         for (uint i = 0; i < allMarkets.length; i ++) {

810:         uint numMarkets = mTokens.length;

811:         uint numBorrowCaps = newBorrowCaps.length;

815:         for(uint i = 0; i < numMarkets; i++) {

847:         uint numMarkets = mTokens.length;

848:         uint numSupplyCaps = newSupplyCaps.length;

852:         for(uint i = 0; i < numMarkets; i++) {

894:         return uint(Error.NO_ERROR);

880:     function _setPauseGuardian(address newPauseGuardian) public returns (uint) {

964:         if (_amount == type(uint).max) {

959:     function _rescueFunds(address _tokenAddress, uint _amount) external {

1040:                for (uint holderIndex = 0; holderIndex < holders.length; holderIndex++) {

1048:                for (uint holderIndex = 0; holderIndex < holders.length; holderIndex++) {

1031:        for (uint i = 0; i < mTokens.length; i++) {

1064:    function getBlockTimestamp() public view returns (uint) {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Comptroller.sol#L26-L26

```solidity
File: src/core/IRModels/InterestRateModel.sol

19:      function getBorrowRate(uint cash, uint borrows, uint reserves) virtual external view returns (uint);

29:      function getSupplyRate(uint cash, uint borrows, uint reserves, uint reserveFactorMantissa) virtual external view returns (uint);

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/IRModels/InterestRateModel.sol#L19-L19

```solidity
File: src/core/IRModels/JumpRateModel.sol

13:      using SafeMath for uint;

15:      event NewInterestParams(uint baseRatePerTimestamp, uint multiplierPerTimestamp, uint jumpMultiplierPerTimestamp, uint kink);

20:      uint public constant timestampsPerYear = 60 * 60 * 24 * 365;

25:      uint public multiplierPerTimestamp;

30:      uint public baseRatePerTimestamp;

35:      uint public jumpMultiplierPerTimestamp;

40:      uint public kink;

52:      constructor(uint baseRatePerYear, uint multiplierPerYear, uint jumpMultiplierPerYear, uint kink_) {

68:      function utilizationRate(uint cash, uint borrows, uint reserves) public pure returns (uint) {

85:          uint util = utilizationRate(cash, borrows, reserves);

90:              uint normalRate = kink.mul(multiplierPerTimestamp).div(1e18).add(baseRatePerTimestamp);

91:              uint excessUtil = util.sub(kink);

84:      function getBorrowRate(uint cash, uint borrows, uint reserves) override public view returns (uint) {

105:         uint oneMinusReserveFactor = uint(1e18).sub(reserveFactorMantissa);

106:         uint borrowRate = getBorrowRate(cash, borrows, reserves);

107:         uint rateToPool = borrowRate.mul(oneMinusReserveFactor).div(1e18);

104:     function getSupplyRate(uint cash, uint borrows, uint reserves, uint reserveFactorMantissa) override public view returns (uint) {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/IRModels/JumpRateModel.sol#L13-L13

```solidity
File: src/core/IRModels/WhitePaperInterestRateModel.sol

13:      using SafeMath for uint;

15:      event NewInterestParams(uint baseRatePerTimestamp, uint multiplierPerTimestamp);

20:      uint public constant timestampsPerYear = 31536000;

25:      uint public multiplierPerTimestamp;

30:      uint public baseRatePerTimestamp;

37:      constructor(uint baseRatePerYear, uint multiplierPerYear) {

51:      function utilizationRate(uint cash, uint borrows, uint reserves) public pure returns (uint) {

68:          uint ur = utilizationRate(cash, borrows, reserves);

67:      function getBorrowRate(uint cash, uint borrows, uint reserves) override public view returns (uint) {

81:          uint oneMinusReserveFactor = uint(1e18).sub(reserveFactorMantissa);

82:          uint borrowRate = getBorrowRate(cash, borrows, reserves);

83:          uint rateToPool = borrowRate.mul(oneMinusReserveFactor).div(1e18);

80:      function getSupplyRate(uint cash, uint borrows, uint reserves, uint reserveFactorMantissa) override public view returns (uint) {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/IRModels/WhitePaperInterestRateModel.sol#L13-L13

```solidity
File: src/core/MErc20.sol

26:                          uint initialExchangeRateMantissa_,

47:          (uint err,) = mintInternal(mintAmount);

46:      function mint(uint mintAmount) override external returns (uint) {

77:          (uint err,) = mintInternal(mintAmount);

62:          uint mintAmount,

63:          uint deadline,

65:      ) override external returns (uint) {

87:      function redeem(uint redeemTokens) override external returns (uint) {

97:      function redeemUnderlying(uint redeemAmount) override external returns (uint) {

106:     function borrow(uint borrowAmount) override external returns (uint) {

116:         (uint err,) = repayBorrowInternal(repayAmount);

115:     function repayBorrow(uint repayAmount) override external returns (uint) {

127:         (uint err,) = repayBorrowBehalfInternal(borrower, repayAmount);

126:     function repayBorrowBehalf(address borrower, uint repayAmount) override external returns (uint) {

140:         (uint err,) = liquidateBorrowInternal(borrower, repayAmount, mTokenCollateral);

139:     function liquidateBorrow(address borrower, uint repayAmount, MTokenInterface mTokenCollateral) override external returns (uint) {

160:     function _addReserves(uint addAmount) override external returns (uint) {

171:     function getCashPrior() virtual override internal view returns (uint) {

189:         uint balanceBefore = EIP20Interface(underlying_).balanceOf(address(this));

209:         uint balanceAfter = EIP20Interface(underlying_).balanceOf(address(this));

185:     function doTransferIn(address from, uint amount) virtual override internal returns (uint) {

223:     function doTransferOut(address payable to, uint amount) virtual override internal {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20.sol#L26-L26

```solidity
File: src/core/MErc20Delegator.sol

28:                  uint initialExchangeRateMantissa_,

84:          return abi.decode(data, (uint));

82:      function mint(uint mintAmount) override external returns (uint) {

108:         return abi.decode(data, (uint));

98:          uint mintAmount,

99:          uint deadline,

101:     ) override external returns (uint) {

119:         return abi.decode(data, (uint));

117:     function redeem(uint redeemTokens) override external returns (uint) {

130:         return abi.decode(data, (uint));

128:     function redeemUnderlying(uint redeemAmount) override external returns (uint) {

140:         return abi.decode(data, (uint));

138:     function borrow(uint borrowAmount) override external returns (uint) {

150:         return abi.decode(data, (uint));

148:     function repayBorrow(uint repayAmount) override external returns (uint) {

161:         return abi.decode(data, (uint));

159:     function repayBorrowBehalf(address borrower, uint repayAmount) override external returns (uint) {

174:         return abi.decode(data, (uint));

172:     function liquidateBorrow(address borrower, uint repayAmount, MTokenInterface mTokenCollateral) override external returns (uint) {

183:     function transfer(address dst, uint amount) override external returns (bool) {

221:         return abi.decode(data, (uint));

219:     function allowance(address owner, address spender) override external view returns (uint) {

231:         return abi.decode(data, (uint));

229:     function balanceOf(address owner) override external view returns (uint) {

242:         return abi.decode(data, (uint));

240:     function balanceOfUnderlying(address owner) override external returns (uint) {

253:         return abi.decode(data, (uint, uint, uint, uint));

251:     function getAccountSnapshot(address account) override external view returns (uint, uint, uint, uint) {

262:         return abi.decode(data, (uint));

260:     function borrowRatePerTimestamp() override external view returns (uint) {

271:         return abi.decode(data, (uint));

269:     function supplyRatePerTimestamp() override external view returns (uint) {

280:         return abi.decode(data, (uint));

278:     function totalBorrowsCurrent() override external returns (uint) {

290:         return abi.decode(data, (uint));

288:     function borrowBalanceCurrent(address account) override external returns (uint) {

300:         return abi.decode(data, (uint));

298:     function borrowBalanceStored(address account) override public view returns (uint) {

309:         return abi.decode(data, (uint));

307:     function exchangeRateCurrent() override public returns (uint) {

319:         return abi.decode(data, (uint));

317:     function exchangeRateStored() override public view returns (uint) {

328:         return abi.decode(data, (uint));

326:     function getCash() override external view returns (uint) {

338:         return abi.decode(data, (uint));

336:     function accrueInterest() override public returns (uint) {

352:         return abi.decode(data, (uint));

350:     function seize(address liquidator, address borrower, uint seizeTokens) override external returns (uint) {

374:         return abi.decode(data, (uint));

372:     function _setPendingAdmin(address payable newPendingAdmin) override external returns (uint) {

384:         return abi.decode(data, (uint));

382:     function _setComptroller(ComptrollerInterface newComptroller) override public returns (uint) {

394:         return abi.decode(data, (uint));

392:     function _setReserveFactor(uint newReserveFactorMantissa) override external returns (uint) {

404:         return abi.decode(data, (uint));

402:     function _acceptAdmin() override external returns (uint) {

414:         return abi.decode(data, (uint));

412:     function _addReserves(uint addAmount) override external returns (uint) {

424:         return abi.decode(data, (uint));

422:     function _reduceReserves(uint reduceAmount) override external returns (uint) {

435:         return abi.decode(data, (uint));

433:     function _setInterestRateModel(InterestRateModel newInterestRateModel) override public returns (uint) {

445:         return abi.decode(data, (uint));

443:     function _setProtocolSeizeShare(uint newProtocolSeizeShareMantissa) override external returns (uint) {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20Delegator.sol#L28-L28

```solidity
File: src/core/MToken.sol

40:          uint err = _setComptroller(comptroller_);

41:          require(err == uint(Error.NO_ERROR), "setting comptroller failed");

49:          require(err == uint(Error.NO_ERROR), "setting interest rate model failed");

28:                          uint initialExchangeRateMantissa_,

70:          uint allowed = comptroller.transferAllowed(address(this), src, dst, tokens);

81:          uint startingAllowance = 0;

83:              startingAllowance = type(uint).max;

90:          uint allowanceNew;

91:          uint srcTokensNew;

92:          uint dstTokensNew;

117:         if (startingAllowance != type(uint).max) {

127:         return uint(Error.NO_ERROR);

68:      function transferTokens(address spender, address src, address dst, uint tokens) internal returns (uint) {

137:         return transferTokens(msg.sender, msg.sender, dst, amount) == uint(Error.NO_ERROR);

148:         return transferTokens(msg.sender, src, dst, amount) == uint(Error.NO_ERROR);

193:         (MathError mErr, uint balance) = mulScalarTruncate(exchangeRate, accountTokens[owner]);

191:     function balanceOfUnderlying(address owner) override external returns (uint) {

205:         uint mTokenBalance = accountTokens[account];

206:         uint borrowBalance;

207:         uint exchangeRateMantissa;

213:             return (uint(Error.MATH_ERROR), 0, 0, 0);

218:             return (uint(Error.MATH_ERROR), 0, 0, 0);

221:         return (uint(Error.NO_ERROR), mTokenBalance, borrowBalance, exchangeRateMantissa);

204:     function getAccountSnapshot(address account) override external view returns (uint, uint, uint, uint) {

228:     function getBlockTimestamp() virtual internal view returns (uint) {

236:     function borrowRatePerTimestamp() override external view returns (uint) {

244:     function supplyRatePerTimestamp() override external view returns (uint) {

253:         require(accrueInterest() == uint(Error.NO_ERROR), "accrue interest failed");

252:     function totalBorrowsCurrent() override external nonReentrant returns (uint) {

263:         require(accrueInterest() == uint(Error.NO_ERROR), "accrue interest failed");

262:     function borrowBalanceCurrent(address account) override external nonReentrant returns (uint) {

273:         (MathError err, uint result) = borrowBalanceStoredInternal(account);

272:     function borrowBalanceStored(address account) override public view returns (uint) {

286:         uint principalTimesIndex;

287:         uint result;

283:     function borrowBalanceStoredInternal(address account) internal view returns (MathError, uint) {

320:         require(accrueInterest() == uint(Error.NO_ERROR), "accrue interest failed");

319:     function exchangeRateCurrent() override public nonReentrant returns (uint) {

330:         (MathError err, uint result) = exchangeRateStoredInternal();

329:     function exchangeRateStored() override public view returns (uint) {

341:         uint _totalSupply = totalSupply;

353:             uint totalCash = getCashPrior();

354:             uint cashPlusBorrowsMinusReserves;

340:     function exchangeRateStoredInternal() virtual internal view returns (MathError, uint) {

376:     function getCash() override external view returns (uint) {

387:         uint currentBlockTimestamp = getBlockTimestamp();

388:         uint accrualBlockTimestampPrior = accrualBlockTimestamp;

392:             return uint(Error.NO_ERROR);

396:         uint cashPrior = getCashPrior();

397:         uint borrowsPrior = totalBorrows;

398:         uint reservesPrior = totalReserves;

399:         uint borrowIndexPrior = borrowIndex;

402:         uint borrowRateMantissa = interestRateModel.getBorrowRate(cashPrior, borrowsPrior, reservesPrior);

406:         (MathError mathErr, uint blockDelta) = subUInt(currentBlockTimestamp, accrualBlockTimestampPrior);

419:         uint interestAccumulated;

420:         uint totalBorrowsNew;

421:         uint totalReservesNew;

422:         uint borrowIndexNew;

426:             return failOpaque(Error.MATH_ERROR, FailureInfo.ACCRUE_INTEREST_SIMPLE_INTEREST_FACTOR_CALCULATION_FAILED, uint(mathErr));

431:             return failOpaque(Error.MATH_ERROR, FailureInfo.ACCRUE_INTEREST_ACCUMULATED_INTEREST_CALCULATION_FAILED, uint(mathErr));

436:             return failOpaque(Error.MATH_ERROR, FailureInfo.ACCRUE_INTEREST_NEW_TOTAL_BORROWS_CALCULATION_FAILED, uint(mathErr));

441:             return failOpaque(Error.MATH_ERROR, FailureInfo.ACCRUE_INTEREST_NEW_TOTAL_RESERVES_CALCULATION_FAILED, uint(mathErr));

446:             return failOpaque(Error.MATH_ERROR, FailureInfo.ACCRUE_INTEREST_NEW_BORROW_INDEX_CALCULATION_FAILED, uint(mathErr));

462:         return uint(Error.NO_ERROR);

385:     function accrueInterest() virtual override public returns (uint) {

472:         uint error = accrueInterest();

473:         if (error != uint(Error.NO_ERROR)) {

471:     function mintInternal(uint mintAmount) internal nonReentrant returns (uint, uint) {

484:         uint exchangeRateMantissa;

485:         uint mintTokens;

486:         uint totalSupplyNew;

487:         uint accountTokensNew;

488:         uint actualMintAmount;

500:         uint allowed = comptroller.mintAllowed(address(this), minter, mintAmount);

514:             return (failOpaque(Error.MATH_ERROR, FailureInfo.MINT_EXCHANGE_RATE_READ_FAILED, uint(vars.mathErr)), 0);

562:         return (uint(Error.NO_ERROR), vars.actualMintAmount);

498:     function mintFresh(address minter, uint mintAmount) internal returns (uint, uint) {

572:         uint error = accrueInterest();

573:         if (error != uint(Error.NO_ERROR)) {

571:     function redeemInternal(uint redeemTokens) internal nonReentrant returns (uint) {

588:         uint error = accrueInterest();

589:         if (error != uint(Error.NO_ERROR)) {

587:     function redeemUnderlyingInternal(uint redeemAmount) internal nonReentrant returns (uint) {

600:         uint exchangeRateMantissa;

601:         uint redeemTokens;

602:         uint redeemAmount;

603:         uint totalSupplyNew;

604:         uint accountTokensNew;

623:             return failOpaque(Error.MATH_ERROR, FailureInfo.REDEEM_EXCHANGE_RATE_READ_FAILED, uint(vars.mathErr));

649:             if (redeemAmountIn == type(uint).max) {

661:                     return failOpaque(Error.MATH_ERROR, FailureInfo.REDEEM_EXCHANGE_AMOUNT_CALCULATION_FAILED, uint(vars.mathErr));

654:                     return failOpaque(Error.MATH_ERROR, FailureInfo.REDEEM_EXCHANGE_TOKENS_CALCULATION_FAILED, uint(vars.mathErr));

633:             if (redeemTokensIn == type(uint).max) {

641:                 return failOpaque(Error.MATH_ERROR, FailureInfo.REDEEM_EXCHANGE_TOKENS_CALCULATION_FAILED, uint(vars.mathErr));

667:         uint allowed = comptroller.redeemAllowed(address(this), redeemer, vars.redeemTokens);

684:             return failOpaque(Error.MATH_ERROR, FailureInfo.REDEEM_NEW_TOTAL_SUPPLY_CALCULATION_FAILED, uint(vars.mathErr));

689:             return failOpaque(Error.MATH_ERROR, FailureInfo.REDEEM_NEW_ACCOUNT_BALANCE_CALCULATION_FAILED, uint(vars.mathErr));

720:         return uint(Error.NO_ERROR);

615:     function redeemFresh(address payable redeemer, uint redeemTokensIn, uint redeemAmountIn) internal returns (uint) {

729:         uint error = accrueInterest();

730:         if (error != uint(Error.NO_ERROR)) {

728:     function borrowInternal(uint borrowAmount) internal nonReentrant returns (uint) {

740:         uint accountBorrows;

741:         uint accountBorrowsNew;

742:         uint totalBorrowsNew;

752:         uint allowed = comptroller.borrowAllowed(address(this), borrower, borrowAmount);

776:             return failOpaque(Error.MATH_ERROR, FailureInfo.BORROW_ACCUMULATED_BALANCE_CALCULATION_FAILED, uint(vars.mathErr));

781:             return failOpaque(Error.MATH_ERROR, FailureInfo.BORROW_NEW_ACCOUNT_BORROW_BALANCE_CALCULATION_FAILED, uint(vars.mathErr));

786:             return failOpaque(Error.MATH_ERROR, FailureInfo.BORROW_NEW_TOTAL_BALANCE_CALCULATION_FAILED, uint(vars.mathErr));

813:         return uint(Error.NO_ERROR);

750:     function borrowFresh(address payable borrower, uint borrowAmount) internal returns (uint) {

822:         uint error = accrueInterest();

823:         if (error != uint(Error.NO_ERROR)) {

821:     function repayBorrowInternal(uint repayAmount) internal nonReentrant returns (uint, uint) {

838:         uint error = accrueInterest();

839:         if (error != uint(Error.NO_ERROR)) {

837:     function repayBorrowBehalfInternal(address borrower, uint repayAmount) internal nonReentrant returns (uint, uint) {

850:         uint repayAmount;

851:         uint borrowerIndex;

852:         uint accountBorrows;

853:         uint accountBorrowsNew;

854:         uint totalBorrowsNew;

855:         uint actualRepayAmount;

867:         uint allowed = comptroller.repayBorrowAllowed(address(this), payer, borrower, repayAmount);

885:             return (failOpaque(Error.MATH_ERROR, FailureInfo.REPAY_BORROW_ACCUMULATED_BALANCE_CALCULATION_FAILED, uint(vars.mathErr)), 0);

889:         if (repayAmount == type(uint).max) {

931:         return (uint(Error.NO_ERROR), vars.actualRepayAmount);

865:     function repayBorrowFresh(address payer, address borrower, uint repayAmount) internal returns (uint, uint) {

943:         uint error = accrueInterest();

944:         if (error != uint(Error.NO_ERROR)) {

950:         if (error != uint(Error.NO_ERROR)) {

942:     function liquidateBorrowInternal(address borrower, uint repayAmount, MTokenInterface mTokenCollateral) internal nonReentrant returns (uint, uint) {

970:         uint allowed = comptroller.liquidateBorrowAllowed(address(this), address(mTokenCollateral), liquidator, borrower, repayAmount);

996:         if (repayAmount == type(uint).max) {

1002:        (uint repayBorrowError, uint actualRepayAmount) = repayBorrowFresh(liquidator, borrower, repayAmount);

1003:        if (repayBorrowError != uint(Error.NO_ERROR)) {

1012:        (uint amountSeizeError, uint seizeTokens) = comptroller.liquidateCalculateSeizeTokens(address(this), address(mTokenCollateral), actualRepayAmount);

1013:        require(amountSeizeError == uint(Error.NO_ERROR), "LIQUIDATE_COMPTROLLER_CALCULATE_AMOUNT_SEIZE_FAILED");

1019:        uint seizeError;

1027:        require(seizeError == uint(Error.NO_ERROR), "token seizure failed");

1036:        return (uint(Error.NO_ERROR), actualRepayAmount);

968:     function liquidateBorrowFresh(address liquidator, address borrower, uint repayAmount, MTokenInterface mTokenCollateral) internal returns (uint, uint) {

1048:    function seize(address liquidator, address borrower, uint seizeTokens) override external nonReentrant returns (uint) {

1054:        uint borrowerTokensNew;

1055:        uint liquidatorTokensNew;

1056:        uint liquidatorSeizeTokens;

1057:        uint protocolSeizeTokens;

1058:        uint protocolSeizeAmount;

1059:        uint exchangeRateMantissa;

1060:        uint totalReservesNew;

1061:        uint totalSupplyNew;

1076:        uint allowed = comptroller.seizeAllowed(address(this), seizerToken, liquidator, borrower, seizeTokens);

1095:            return failOpaque(Error.MATH_ERROR, FailureInfo.LIQUIDATE_SEIZE_BALANCE_DECREMENT_FAILED, uint(vars.mathErr));

1111:            return failOpaque(Error.MATH_ERROR, FailureInfo.LIQUIDATE_SEIZE_BALANCE_INCREMENT_FAILED, uint(vars.mathErr));

1133:        return uint(Error.NO_ERROR);

1074:    function seizeInternal(address seizerToken, address liquidator, address borrower, uint seizeTokens) internal returns (uint) {

1160:        return uint(Error.NO_ERROR);

1145:    function _setPendingAdmin(address payable newPendingAdmin) override external returns (uint) {

1187:        return uint(Error.NO_ERROR);

1168:    function _acceptAdmin() override external returns (uint) {

1211:        return uint(Error.NO_ERROR);

1195:    function _setComptroller(ComptrollerInterface newComptroller) override public returns (uint) {

1220:        uint error = accrueInterest();

1221:        if (error != uint(Error.NO_ERROR)) {

1219:    function _setReserveFactor(uint newReserveFactorMantissa) override external nonReentrant returns (uint) {

1250:        uint oldReserveFactorMantissa = reserveFactorMantissa;

1255:        return uint(Error.NO_ERROR);

1234:    function _setReserveFactorFresh(uint newReserveFactorMantissa) internal returns (uint) {

1264:        uint error = accrueInterest();

1265:        if (error != uint(Error.NO_ERROR)) {

1263:    function _addReservesInternal(uint addAmount) internal nonReentrant returns (uint) {

1283:        uint totalReservesNew;

1284:        uint actualAddAmount;

1317:        return (uint(Error.NO_ERROR), actualAddAmount);

1281:    function _addReservesFresh(uint addAmount) internal returns (uint, uint) {

1327:        uint error = accrueInterest();

1328:        if (error != uint(Error.NO_ERROR)) {

1326:    function _reduceReserves(uint reduceAmount) override external nonReentrant returns (uint) {

1344:        uint totalReservesNew;

1382:        return uint(Error.NO_ERROR);

1342:    function _reduceReservesFresh(uint reduceAmount) internal returns (uint) {

1392:        uint error = accrueInterest();

1393:        if (error != uint(Error.NO_ERROR)) {

1391:    function _setInterestRateModel(InterestRateModel newInterestRateModel) override public returns (uint) {

1434:        return uint(Error.NO_ERROR);

1407:    function _setInterestRateModelFresh(InterestRateModel newInterestRateModel) internal returns (uint) {

1444:        uint error = accrueInterest();

1445:        if (error != uint(Error.NO_ERROR)) {

1443:    function _setProtocolSeizeShare(uint newProtocolSeizeShareMantissa) override external nonReentrant returns (uint) {

1462:        uint oldProtocolSeizeShareMantissa;

1483:        return uint(Error.NO_ERROR);

1459:    function _setProtocolSeizeShareFresh(uint newProtocolSeizeShareMantissa) internal returns (uint) {

1493:    function getCashPrior() virtual internal view returns (uint);

1499:    function doTransferIn(address from, uint amount) virtual internal returns (uint);

1506:    function doTransferOut(address payable to, uint amount) virtual internal;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MToken.sol#L40-L40

```solidity
File: src/core/MultiRewardDistributor/MultiRewardDistributor.sol

886:         uint borrowerAmount = div_(

892:         uint borrowerDelta = mul_(borrowerAmount, deltaIndex);

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L886-L886

```solidity
File: src/core/Unitroller.sol

51:          return uint(Error.NO_ERROR);

39:      function _setPendingImplementation(address newPendingImplementation) public returns (uint) {

76:          return uint(Error.NO_ERROR);

59:      function _acceptImplementation() public returns (uint) {

101:         return uint(Error.NO_ERROR);

86:      function _setPendingAdmin(address newPendingAdmin) public returns (uint) {

128:         return uint(Error.NO_ERROR);

109:     function _acceptAdmin() public returns (uint) {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Unitroller.sol#L51-L51

</details>

### [N&#x2011;66] Use the latest solidity (prior to 0.8.20 if on L2s) for deployment
```
When deploying contracts, you should use the latest released version of Solidity. Apart from exceptional cases, only the latest version receives security fixes.
```
https://docs.soliditylang.org/en/v0.8.20/

*There are 14 instances of this issue:*

<details>
<summary>see instances</summary>


```solidity
File: src/core/Comptroller.sol

2:   pragma solidity 0.8.17;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Comptroller.sol#L2-L2

```solidity
File: src/core/Governance/TemporalGovernor.sol

2:   pragma solidity ^0.8.0;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Governance/TemporalGovernor.sol#L2-L2

```solidity
File: src/core/IRModels/InterestRateModel.sol

2:   pragma solidity 0.8.17;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/IRModels/InterestRateModel.sol#L2-L2

```solidity
File: src/core/IRModels/JumpRateModel.sol

2:   pragma solidity 0.8.17;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/IRModels/JumpRateModel.sol#L2-L2

```solidity
File: src/core/IRModels/WhitePaperInterestRateModel.sol

2:   pragma solidity 0.8.17;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/IRModels/WhitePaperInterestRateModel.sol#L2-L2

```solidity
File: src/core/MErc20.sol

2:   pragma solidity 0.8.17;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20.sol#L2-L2

```solidity
File: src/core/MErc20Delegate.sol

2:   pragma solidity 0.8.17;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20Delegate.sol#L2-L2

```solidity
File: src/core/MErc20Delegator.sol

2:   pragma solidity 0.8.17;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20Delegator.sol#L2-L2

```solidity
File: src/core/MToken.sol

2:   pragma solidity 0.8.17;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MToken.sol#L2-L2

```solidity
File: src/core/MultiRewardDistributor/MultiRewardDistributor.sol

2:   pragma solidity 0.8.17;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L2-L2

```solidity
File: src/core/Oracles/ChainlinkCompositeOracle.sol

2:   pragma solidity ^0.8.0;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkCompositeOracle.sol#L2-L2

```solidity
File: src/core/Oracles/ChainlinkOracle.sol

2:   pragma solidity 0.8.17;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkOracle.sol#L2-L2

```solidity
File: src/core/Unitroller.sol

2:   pragma solidity 0.8.17;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Unitroller.sol#L2-L2

```solidity
File: src/core/router/WETHRouter.sol

1:   pragma solidity 0.8.17;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/router/WETHRouter.sol#L1-L1

</details>



## Disputed Issues

The issues below may be reported by other bots/wardens, but can be penalized/ignored since either the rule or the specified instances are invalid


### [D&#x2011;01] ~~`mint()`/`burn()` missing access control~~
The general rule is valid, but the instances below are invalid

*There are 3 instances of this issue:*

```solidity
File: src/core/MErc20.sol

46       function mint(uint mintAmount) override external returns (uint) {
47           (uint err,) = mintInternal(mintAmount);
48           return err;
49:      }

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20.sol#L46-L49

```solidity
File: src/core/MErc20Delegator.sol

82       function mint(uint mintAmount) override external returns (uint) {
83           bytes memory data = delegateToImplementation(abi.encodeWithSignature("mint(uint256)", mintAmount));
84           return abi.decode(data, (uint));
85:      }

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20Delegator.sol#L82-L85

```solidity
File: src/core/router/WETHRouter.sol

31       function mint(address recipient) external payable {
32           weth.deposit{value: msg.value}();
33   
34           require(mToken.mint(msg.value) == 0, "WETHRouter: mint failed");
35   
36           IERC20(address(mToken)).safeTransfer(
37               recipient,
38               mToken.balanceOf(address(this))
39           );
40:      }

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/router/WETHRouter.sol#L31-L40


### [D&#x2011;02] ~~Return values of `transfer()`/`transferFrom()` not checked~~
Not all `IERC20` implementations `revert()` when there's a failure in `transfer()`/`transferFrom()`. The function signature has a `boolean` return value and they indicate errors that way instead. By not checking the return value, operations that should have marked as failed, may potentially go through without actually making a payment

*There are 2 instances of this issue:*

```solidity
File: src/core/Comptroller.sol

965:              token.transfer(admin, token.balanceOf(address(this)));

967:              token.transfer(admin, _amount);

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Comptroller.sol#L965


### [D&#x2011;03] ~~Some tokens may revert when zero value transfers are made~~
The general rule is valid, but the instances below are invalid

*There are 8 instances of this issue:*

```solidity
File: src/core/MErc20.sol

149          require(msg.sender == admin, "MErc20::sweepToken: only admin can sweep tokens");
150          require(address(token) != underlying, "MErc20::sweepToken: can not sweep underlying token");
151      	uint256 balance = token.balanceOf(address(this));
152:     	token.transfer(admin, balance);

187          address underlying_ = underlying;
188          EIP20NonStandardInterface token = EIP20NonStandardInterface(underlying_);
189          uint balanceBefore = EIP20Interface(underlying_).balanceOf(address(this));
190:         token.transferFrom(from, address(this), amount);

223      function doTransferOut(address payable to, uint amount) virtual override internal {
224          EIP20NonStandardInterface token = EIP20NonStandardInterface(underlying);
225:         token.transfer(to, amount);

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20.sol#L149-L152

```solidity
File: src/core/MultiRewardDistributor/MultiRewardDistributor.sol

480                  token.balanceOf(address(this))
481              );
482          } else {
483:             token.safeTransfer(comptroller.admin(), _amount);

475          IERC20 token = IERC20(_tokenAddress);
476          // Similar to mTokens, if this is uint256.max that means "transfer everything"
477          if (_amount == type(uint256).max) {
478              token.safeTransfer(
479                  comptroller.admin(),
480                  token.balanceOf(address(this))
481:             );

1234         // Only transfer out if we have enough of a balance to cover it (otherwise just accrue without sending)
1235         if (_amount > 0 && _amount <= currentTokenHoldings) {
1236             // Ensure we use SafeERC20 to revert even if the reward token isn't ERC20 compliant
1237:            token.safeTransfer(_user, _amount);

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L480-L483

```solidity
File: src/core/router/WETHRouter.sol

33   
34           require(mToken.mint(msg.value) == 0, "WETHRouter: mint failed");
35   
36           IERC20(address(mToken)).safeTransfer(
37               recipient,
38               mToken.balanceOf(address(this))
39:          );

45       function redeem(uint256 mTokenRedeemAmount, address recipient) external {
46           IERC20(address(mToken)).safeTransferFrom(
47               msg.sender,
48               address(this),
49               mTokenRedeemAmount
50:          );

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/router/WETHRouter.sol#L33-L39


### [D&#x2011;04] ~~Unsafe downcast~~
When a type is downcast to a smaller type, the higher order bits are truncated, effectively applying a modulo to the original value. Without any other checks, this wrapping will lead to unexpected behavior and bugs

*There are 3 instances of this issue:*

```solidity
File: src/core/Oracles/ChainlinkCompositeOracle.sol

/// @audit uint8
109:              expectedDecimals > uint8(0) && expectedDecimals <= uint8(18),

/// @audit int256
113:          int256 scalingFactor = int256(10 ** uint256(expectedDecimals)); /// calculate expected decimals for end quote

/// @audit uint8
140:              expectedDecimals > uint8(0) && expectedDecimals <= uint8(18),

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkCompositeOracle.sol#L109


### [D&#x2011;05] ~~`require()` should be used instead of `assert()`~~
Prior to solidity version 0.8.0, hitting an assert consumes the **remainder of the transaction's available gas** rather than returning it, as `require()`/`revert()` do. `assert()` should be avoided even past solidity version 0.8.0 as its [documentation](https://docs.soliditylang.org/en/v0.8.14/control-structures.html#panic-via-assert-and-error-via-require) states that "The assert function creates an error of type Panic(uint256). ... Properly functioning code should never create a Panic, not even on invalid external input. If this happens, then there is a bug in your contract which you should fix".

*There are 4 instances of this issue:*

```solidity
File: src/core/Comptroller.sol

194:          assert(assetIndex < len);

329:              assert(markets[mToken].accountMembership[borrower]);

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Comptroller.sol#L194

```solidity
File: src/core/Governance/TemporalGovernor.sol

256:          assert(!guardianPauseAllowed); /// this should never revert, statement for SMT solving

289:          assert(!guardianPauseAllowed); /// this should be an unreachable state

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Governance/TemporalGovernor.sol#L256


### [D&#x2011;06] ~~Missing contract-existence checks before low-level calls~~
The general rule is valid, but the instances below are invalid

*There is one instance of this issue:*

```solidity
File: src/core/router/WETHRouter.sol

59           (bool success, ) = payable(recipient).call{
60               value: address(this).balance
61           }("");
62           require(success, "WETHRouter: ETH transfer failed");
63:      }

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/router/WETHRouter.sol#L59-L63


### [D&#x2011;07] ~~External call recipient may consume all transaction gas~~
The general rule is valid, but the instances below are invalid

*There is one instance of this issue:*

```solidity
File: src/core/router/WETHRouter.sol

/// @audit `redeem()`
59:          (bool success, ) = payable(recipient).call{

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/router/WETHRouter.sol#L59-L59


### [D&#x2011;08] ~~Division by zero not prevented~~
The general rule is valid, but the instances below are invalid

*There is one instance of this issue:*

```solidity
File: src/core/Oracles/ChainlinkCompositeOracle.sol

92           int256 priceMultiplier,
93           int256 scalingFactor
94       ) public pure returns (uint256) {
95:          return ((basePrice * priceMultiplier) / scalingFactor).toUint256();

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkCompositeOracle.sol#L92-L95


### [D&#x2011;09] ~~Solidity version 0.8.20 may not work on other chains due to `PUSH0`~~
The general rule is valid, but the instances below are invalid

*There are 2 instances of this issue:*

```solidity
File: src/core/Governance/TemporalGovernor.sol

2:   pragma solidity ^0.8.0;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Governance/TemporalGovernor.sol#L2-L2

```solidity
File: src/core/Oracles/ChainlinkCompositeOracle.sol

2:   pragma solidity ^0.8.0;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkCompositeOracle.sol#L2-L2


### [D&#x2011;10] ~~Error messages should descriptive, rather that cryptic~~
The general rule is valid, but the instances below are invalid

*There are 13 instances of this issue:*

```solidity
File: src/core/Comptroller.sol

902:         require(msg.sender == admin, "Unauthorized");

960:         require(msg.sender == admin, "Unauthorized");

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Comptroller.sol#L902-L902

```solidity
File: src/core/MErc20.sol

206:         require(success, "TOKEN_TRANSFER_IN_FAILED");

210:         require(balanceAfter >= balanceBefore, "TOKEN_TRANSFER_IN_OVERFLOW");

241:         require(success, "TOKEN_TRANSFER_OUT_FAILED");

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20.sol#L206-L206

```solidity
File: src/core/MToken.sol

537:         require(vars.mathErr == MathError.NO_ERROR, "MINT_EXCHANGE_CALCULATION_FAILED");

545:         require(vars.mathErr == MathError.NO_ERROR, "MINT_NEW_TOTAL_SUPPLY_CALCULATION_FAILED");

548:         require(vars.mathErr == MathError.NO_ERROR, "MINT_NEW_ACCOUNT_BALANCE_CALCULATION_FAILED");

914:         require(vars.mathErr == MathError.NO_ERROR, "REPAY_BORROW_NEW_ACCOUNT_BORROW_BALANCE_CALCULATION_FAILED");

917:         require(vars.mathErr == MathError.NO_ERROR, "REPAY_BORROW_NEW_TOTAL_BALANCE_CALCULATION_FAILED");

1013:        require(amountSeizeError == uint(Error.NO_ERROR), "LIQUIDATE_COMPTROLLER_CALCULATE_AMOUNT_SEIZE_FAILED");

1016:        require(mTokenCollateral.balanceOf(borrower) >= seizeTokens, "LIQUIDATE_SEIZE_TOO_MUCH");

1515:        require(_notEntered, "re-entered");

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MToken.sol#L537-L537


### [D&#x2011;11] ~~Shorten the array rather than copying to a new one~~
None of these examples are of filtering out entries from an array.

*There are 5 instances of this issue:*

```solidity
File: src/core/Comptroller.sol

105          uint[] memory results = new uint[](len);
106          for (uint i = 0; i < len; i++) {
107              MToken mToken = MToken(mTokens[i]);
108: 

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Comptroller.sol#L105-L108

```solidity
File: src/core/Governance/TemporalGovernor.sol

112          bytes32[] memory trustedSendersList = new bytes32[](
113              trustedSenders[chainId].length()
114          );
115  
116          unchecked {
117:             for (uint256 i = 0; i < trustedSendersList.length; i++) {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Governance/TemporalGovernor.sol#L112-L117

```solidity
File: src/core/MultiRewardDistributor/MultiRewardDistributor.sol

204          MarketConfig[] memory outputMarketConfigs = new MarketConfig[](
205              configs.length
206          );
207  
208          // Pop out the MarketConfigs to return them
209:         for (uint256 index = 0; index < configs.length; index++) {

236          RewardWithMToken[] memory outputData = new RewardWithMToken[](
237              markets.length
238          );
239  
240          for (uint256 index = 0; index < markets.length; index++) {
241:             RewardInfo[] memory rewardInfo = getOutstandingRewardsForUser(

266          RewardInfo[] memory outputRewardData = new RewardInfo[](configs.length);
267  
268          // Code golf to avoid too many local vars :rolling-eyes:
269:         CalculatedData memory calcData = CalculatedData({

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L204-L209


### [D&#x2011;12] ~~Storage Write Removal Bug On Conditional Early Termination~~
In solidity versions 0.8.13 through 0.8.16, there is a [bug](https://blog.soliditylang.org/2022/09/08/storage-write-removal-before-conditional-termination/) involving the use of the Yul functions `return()` and `stop()`. If those functions aren't called, or if the Solidity version doesn't match, the finding is invalid.

*There are 4 instances of this issue:*

```solidity
File: src/core/MErc20.sol

193          assembly {
194              switch returndatasize()
195                  case 0 {                       // This is a non-standard ERC-20
196                      success := not(0)          // set success to true
197                  }
198                  case 32 {                      // This is a compliant ERC-20
199                      returndatacopy(0, 0, 32)
200                      success := mload(0)        // Set `success = returndata` of external call
201                  }
202                  default {                      // This is an excessively non-compliant ERC-20, revert.
203                      revert(0, 0)
204                  }
205:         }

228          assembly {
229              switch returndatasize()
230                  case 0 {                      // This is a non-standard ERC-20
231                      success := not(0)          // set success to true
232                  }
233                  case 32 {                     // This is a compliant ERC-20
234                      returndatacopy(0, 0, 32)
235                      success := mload(0)        // Set `success = returndata` of override external call
236                  }
237                  default {                     // This is an excessively non-compliant ERC-20, revert.
238                      revert(0, 0)
239                  }
240:         }

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20.sol#L193-L205

```solidity
File: src/core/MErc20Delegator.sol

457          assembly {
458              if eq(success, 0) {
459                  revert(add(returnData, 0x20), returndatasize())
460              }
461:         }

484          assembly {
485              if eq(success, 0) {
486                  revert(add(returnData, 0x20), returndatasize())
487              }
488:         }

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20Delegator.sol#L457-L461


### [D&#x2011;13] ~~Bad bot rules~~
The titles below correspond to issues submitted by various bots, where the submitting bot solely submitted invalid findings (i.e. the submitter didn't filter the results of the rule), so they should be given extra scrutiny:
- **Max allowance is not compatible with all tokens** - internal approval for the contract's own balance, so the rule is pointing to the support **for** max allowance
- **increase/decrease allowance should be used instead of approve** - this is an internal approval function
- **Must approve or increase allowance first** - the rule is flagging all transferFrom() calls, without approval logic
- **Contract existence is not checked before low level call** - reading calldata, not making an external call
- **Empty function blocks** - the bot's removed the extensive comment documentation in the 'code blocks' it shows for these virtual functions used to allow child contracts to implement functionality, or are constructors
- **Utility contracts can be made into libraries** - all provided examples are invalid
- **Address values should be used through variables rather than used as literals** - none of the examples are of addresses
- **Employ Explicit Casting to Bytes or Bytes32 for Enhanced Code Clarity and Meaning** - the large majority of the examples are of multiple arguments, not just one
- **Some if-statement can be converted to a ternary** - you can't use a ternary when only one of the branches is a `return`
- **Addresses shouldn't be hard-coded** - none of these are addresses
- **State variables used within a function more than once should be cached to save gas** - none of these are state variables
- **Use storage instead of memory for structs/arrays** - these all are array call arguments, not arrays copied from storage
- **Use bitmap to save gas** - none of these are examples where bitmaps can be used
- **Consider merging sequential for loops** - the examples cannot be merged
- **Emitting storage values instead of the memory one.** - this is a gas finding, not a Low one
- **`selfbalance()` is cheaper than `address(this).balance`** - some bots submit the issue twice (under the heading `Use assembly when getting a contractundefineds balance of ETH`)
- **Imports could be organized more systematically** - a lot of bots are blindly checking for interfaces not coming first. That is not the only way of organizing imports, and most projects are doing it in a systematic, valid, way
- **Unused * definition** - some bots are reporting false positives for these rules. Check that it isn't used, or that if it's used, that there are two definitions, with one being unused
- **`internal` functions not called by the contract should be removed** - some bots are reporting false positives when the function is called by a child contract, rather than the defining contract
- **Change `public` to `external` for functions that are not called internally** - some bots are reporting false positives when the function is called by a child contract, rather than the defining contract


Some of these have been raised as invalid in multiple contests, and the bot owners have not fixed them. Without penalties, they're unlikely to make any changes

*There is one instance of this issue:*

```solidity
File: src/core/router/WETHRouter.sol

1:   pragma solidity 0.8.17;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/router/WETHRouter.sol#L1-L1


### [D&#x2011;14] ~~Missing contract-existence checks before low-level calls~~
The contract exists in this case

*There is one instance of this issue:*

```solidity
File: src/core/MErc20Delegator.sol

482      function delegateToViewImplementation(bytes memory data) public view returns (bytes memory) {
483          (bool success, bytes memory returnData) = address(this).staticcall(abi.encodeWithSignature("delegateToImplementation(bytes)", data));
484          assembly {
485              if eq(success, 0) {
486:                 revert(add(returnData, 0x20), returndatasize())

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20Delegator.sol#L482-L486


### [D&#x2011;15] ~~Function result should be cached~~
Transfers are not something that can be 'cached'

*There are 2 instances of this issue:*

```solidity
File: src/core/Comptroller.sol

967:             token.transfer(admin, _amount);

965:             token.transfer(admin, token.balanceOf(address(this)));

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Comptroller.sol#L967-L967


### [D&#x2011;16] ~~Use delete instead of setting mapping/state variable to zero, to save gas~~
Using delete instead of assigning zero to state variables does not save any extra gas with the optimizer [on](https://gist.github.com/IllIllI000/ef8ec3a70aede7f12433fe63dc418515#with-the-optimizer-set-at-200-runs) (saves 5-8 gas with optimizer completely off), so this finding is invalid, especially since if they were interested in gas savings, they'd have the optimizer enabled.

*There are 5 instances of this issue:*

```solidity
File: src/core/Comptroller.sol

785:         newMarket.collateralFactorMantissa = 0;

1086:        _locked = 0;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Comptroller.sol#L785-L785

```solidity
File: src/core/Governance/TemporalGovernor.sol

195:         lastPauseTime = 0;

214:         lastPauseTime = 0;

253:         lastPauseTime = 0;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Governance/TemporalGovernor.sol#L195-L195


### [D&#x2011;17] ~~Event names should use CamelCase~~
The instances below are already CamelCase (events are supposed to use CamelCase, not lowerCamelCase)

*There are 24 instances of this issue:*

```solidity
File: src/core/Comptroller.sol

17:      event MarketListed(MToken mToken);

20:      event MarketEntered(MToken mToken, address account);

23:      event MarketExited(MToken mToken, address account);

26:      event NewCloseFactor(uint oldCloseFactorMantissa, uint newCloseFactorMantissa);

29:      event NewCollateralFactor(MToken mToken, uint oldCollateralFactorMantissa, uint newCollateralFactorMantissa);

32:      event NewLiquidationIncentive(uint oldLiquidationIncentiveMantissa, uint newLiquidationIncentiveMantissa);

35:      event NewPriceOracle(PriceOracle oldPriceOracle, PriceOracle newPriceOracle);

38:      event NewPauseGuardian(address oldPauseGuardian, address newPauseGuardian);

41:      event ActionPaused(string action, bool pauseState);

44:      event ActionPaused(MToken mToken, string action, bool pauseState);

47:      event NewBorrowCap(MToken indexed mToken, uint newBorrowCap);

50:      event NewBorrowCapGuardian(address oldBorrowCapGuardian, address newBorrowCapGuardian);

53:      event NewSupplyCap(MToken indexed mToken, uint newSupplyCap);

56:      event NewSupplyCapGuardian(address oldSupplyCapGuardian, address newSupplyCapGuardian);

59:      event NewRewardDistributor(MultiRewardDistributor oldRewardDistributor, MultiRewardDistributor newRewardDistributor);

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Comptroller.sol#L17-L17

```solidity
File: src/core/IRModels/JumpRateModel.sol

15:      event NewInterestParams(uint baseRatePerTimestamp, uint multiplierPerTimestamp, uint jumpMultiplierPerTimestamp, uint kink);

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/IRModels/JumpRateModel.sol#L15-L15

```solidity
File: src/core/IRModels/WhitePaperInterestRateModel.sol

15:      event NewInterestParams(uint baseRatePerTimestamp, uint multiplierPerTimestamp);

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/IRModels/WhitePaperInterestRateModel.sol#L15-L15

```solidity
File: src/core/Oracles/ChainlinkOracle.sol

30       event PricePosted(
31           address asset,
32           uint256 previousPriceMantissa,
33           uint256 requestedPriceMantissa,
34           uint256 newPriceMantissa
35:      );

38:      event NewAdmin(address oldAdmin, address newAdmin);

41:      event FeedSet(address feed, string symbol);

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkOracle.sol#L30-L35

```solidity
File: src/core/Unitroller.sol

16:      event NewPendingImplementation(address oldPendingImplementation, address newPendingImplementation);

21:      event NewImplementation(address oldImplementation, address newImplementation);

26:      event NewPendingAdmin(address oldPendingAdmin, address newPendingAdmin);

31:      event NewAdmin(address oldAdmin, address newAdmin);

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Unitroller.sol#L16-L16


### [D&#x2011;18] ~~Contracts do not work with fee-on-transfer tokens~~
An ERC20 token being used, in and of itself, is not evidence of a fee-on-transfer issue; there must be other evidence that the balance accounting gets broken, and these lines do not contain such evidence.

*There are 4 instances of this issue:*

```solidity
File: src/core/MErc20.sol

66:          IERC20Permit token = IERC20Permit(underlying);

70:          SafeERC20.safePermit(

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20.sol#L66-L66

```solidity
File: src/core/Oracles/ChainlinkOracle.sol

76:              MErc20(address(mToken)).underlying()

122:         address asset = address(MErc20(address(mToken)).underlying());

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkOracle.sol#L76-L76


### [D&#x2011;19] ~~Multiplications of powers of can be replaced by a left shift operation to save gas~~
This is not safe to do in all cases, because there is no overflow protection with left bit shifts

*There is one instance of this issue:*

```solidity
File: src/core/Oracles/ChainlinkCompositeOracle.sol

145:         int256 scalingFactor = int256(10 ** uint256(expectedDecimals * 2)); /// calculate expected decimals for end quote

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkCompositeOracle.sol#L145-L145


### [D&#x2011;20] ~~Contracts are not using their OZ Upgradeable counterparts~~
The rule is true only when the contract being defined is upgradeable, which isn't the case for these invalid examples

*There are 10 instances of this issue:*

```solidity
File: src/core/Governance/TemporalGovernor.sol

/// @audit TemporalGovernor is a non-upgradeable contract
4:   import {EnumerableSet} from "@openzeppelin/contracts/utils/structs/EnumerableSet.sol";

/// @audit TemporalGovernor is a non-upgradeable contract
5:   import {SafeCast} from "@openzeppelin/contracts/utils/math/SafeCast.sol";

/// @audit TemporalGovernor is a non-upgradeable contract
6:   import {Pausable} from "@openzeppelin/contracts/security/Pausable.sol";

/// @audit TemporalGovernor is a non-upgradeable contract
7:   import {Ownable} from "@openzeppelin/contracts/access/Ownable.sol";

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Governance/TemporalGovernor.sol#L4-L4

```solidity
File: src/core/MErc20.sol

/// @audit MErc20 is a non-upgradeable contract
4:   import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20.sol#L4-L4

```solidity
File: src/core/MultiRewardDistributor/MultiRewardDistributor.sol

/// @audit MultiRewardDistributor is a non-upgradeable contract
4:   import {ReentrancyGuard} from "@openzeppelin/contracts/security/ReentrancyGuard.sol";

/// @audit MultiRewardDistributor is a non-upgradeable contract
5:   import {Initializable} from "@openzeppelin/contracts/proxy/utils/Initializable.sol";

/// @audit MultiRewardDistributor is a non-upgradeable contract
6:   import {SafeERC20} from "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

/// @audit MultiRewardDistributor is a non-upgradeable contract
7:   import {Pausable} from "@openzeppelin/contracts/security/Pausable.sol";

/// @audit MultiRewardDistributor is a non-upgradeable contract
8:   import {IERC20} from "@openzeppelin/contracts/token/ERC20/IERC20.sol";

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L4-L4


### [D&#x2011;21] ~~`decimals()` is not a part of the ERC-20 standard~~


*There are 3 instances of this issue:*

```solidity
File: src/core/Oracles/ChainlinkCompositeOracle.sol

192:         uint8 oracleDecimals = AggregatorV3Interface(oracleAddress).decimals();

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkCompositeOracle.sol#L192-L192

```solidity
File: src/core/Oracles/ChainlinkOracle.sol

85:          uint256 decimalDelta = uint256(18).sub(uint256(token.decimals()));

106:         uint256 decimalDelta = uint256(18).sub(feed.decimals());

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkOracle.sol#L85-L85


### [D&#x2011;22] ~~`symbol()` is not a part of the ERC-20 standard~~


*There are 2 instances of this issue:*

```solidity
File: src/core/Oracles/ChainlinkOracle.sol

61:          string memory symbol = mToken.symbol();

82:              price = getChainlinkPrice(getFeed(token.symbol()));

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkOracle.sol#L61-L61


### [D&#x2011;23] ~~Change `public` function visibility to `external` to save gas~~
Both `public` and `external` functions use the same amount of gas (both deployment and runtime gas), so this finding is invalid

*There are 23 instances of this issue:*

```solidity
File: src/core/Comptroller.sol

516:     function getAccountLiquidity(address account) public view returns (uint, uint, uint) {

542      function getHypotheticalAccountLiquidity(
543          address account,
544          address mTokenModify,
545          uint redeemTokens,
546:         uint borrowAmount) public view returns (uint, uint, uint) {

665:     function _setPriceOracle(PriceOracle newOracle) public returns (uint) {

880:     function _setPauseGuardian(address newPauseGuardian) public returns (uint) {

901:     function _setRewardDistributor(MultiRewardDistributor newRewardDistributor) public {

911:     function _setMintPaused(MToken mToken, bool state) public returns (bool) {

921:     function _setBorrowPaused(MToken mToken, bool state) public returns (bool) {

931:     function _setTransferPaused(bool state) public returns (bool) {

940:     function _setSeizePaused(bool state) public returns (bool) {

949:     function _become(Unitroller unitroller) public {

998:     function claimReward() public {

1006:    function claimReward(address holder) public {

1060:    function getAllMarkets() public view returns (MToken[] memory) {

1064:    function getBlockTimestamp() public view returns (uint) {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Comptroller.sol#L516-L516

```solidity
File: src/core/Governance/TemporalGovernor.sol

237:     function executeProposal(bytes memory VAA) public whenNotPaused {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Governance/TemporalGovernor.sol#L237-L237

```solidity
File: src/core/MErc20.sol

23       function initialize(address underlying_,
24                           ComptrollerInterface comptroller_,
25                           InterestRateModel interestRateModel_,
26                           uint initialExchangeRateMantissa_,
27                           string memory name_,
28                           string memory symbol_,
29:                          uint8 decimals_) public {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20.sol#L23-L29

```solidity
File: src/core/MToken.sol

26       function initialize(ComptrollerInterface comptroller_,
27                           InterestRateModel interestRateModel_,
28                           uint initialExchangeRateMantissa_,
29                           string memory name_,
30                           string memory symbol_,
31:                          uint8 decimals_) public {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MToken.sol#L26-L31

```solidity
File: src/core/MultiRewardDistributor/MultiRewardDistributor.sol

340      function getGlobalSupplyIndex(
341          address mToken,
342          uint256 index
343:     ) public view returns (uint256) {

355      function getGlobalBorrowIndex(
356          address mToken,
357          uint256 index
358:     ) public view returns (uint256) {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L340-L343

```solidity
File: src/core/Unitroller.sol

39:      function _setPendingImplementation(address newPendingImplementation) public returns (uint) {

59:      function _acceptImplementation() public returns (uint) {

86:      function _setPendingAdmin(address newPendingAdmin) public returns (uint) {

109:     function _acceptAdmin() public returns (uint) {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Unitroller.sol#L39-L39


### [D&#x2011;24] ~~Use replace and pop instead of the delete keyword to removing an item from an array~~
The examples below are mappings, not arrays

*There is one instance of this issue:*

```solidity
File: src/core/Comptroller.sol

179:         delete marketToExit.accountMembership[msg.sender];

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Comptroller.sol#L179-L179


### [D&#x2011;25] ~~Do not use underscore at the end of variable name~~
A common convention is to add a trailing underscore to a variable name in order to prevent the shadowing of [another variable](https://github.com/ethereum/solidity/issues/11764#issuecomment-895813808) or function name, as is the case with the examples below.

*There are 12 instances of this issue:*

```solidity
File: src/core/IRModels/JumpRateModel.sol

52:      constructor(uint baseRatePerYear, uint multiplierPerYear, uint jumpMultiplierPerYear, uint kink_) {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/IRModels/JumpRateModel.sol#L52-L52

```solidity
File: src/core/MErc20.sol

23:      function initialize(address underlying_,

187:         address underlying_ = underlying;

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20.sol#L23-L23

```solidity
File: src/core/MErc20Delegator.sol

32:                  address payable admin_,

33:                  address implementation_,

61:      function _setImplementation(address implementation_, bool allowResign, bytes memory becomeImplementationData) override public {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20Delegator.sol#L32-L32

```solidity
File: src/core/MToken.sol

26:      function initialize(ComptrollerInterface comptroller_,

27:                          InterestRateModel interestRateModel_,

28:                          uint initialExchangeRateMantissa_,

29:                          string memory name_,

30:                          string memory symbol_,

31:                          uint8 decimals_) public {

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MToken.sol#L26-L26


### [D&#x2011;26] ~~Save gas with the use of specific import statements~~
Importing whole files rather than specific identifiers [does not waste gas](https://ethereum.stackexchange.com/questions/138876/does-solidity-optimizer-eliminate-unused-internal-functions-of-libraries), so this finding is invalid

*There are 27 instances of this issue:*

<details>
<summary>see instances</summary>


```solidity
File: src/core/Comptroller.sol

4:   import "./MToken.sol";

5:   import "./ErrorReporter.sol";

6:   import "./Oracles/PriceOracle.sol";

7:   import "./ComptrollerInterface.sol";

8:   import "./ComptrollerStorage.sol";

9:   import "./Unitroller.sol";

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Comptroller.sol#L4-L4

```solidity
File: src/core/IRModels/JumpRateModel.sol

4:   import "./InterestRateModel.sol";

5:   import "../SafeMath.sol";

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/IRModels/JumpRateModel.sol#L4-L4

```solidity
File: src/core/IRModels/WhitePaperInterestRateModel.sol

4:   import "./InterestRateModel.sol";

5:   import "../SafeMath.sol";

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/IRModels/WhitePaperInterestRateModel.sol#L4-L4

```solidity
File: src/core/MErc20.sol

4:   import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

5:   import "./MToken.sol";

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20.sol#L4-L4

```solidity
File: src/core/MErc20Delegate.sol

4:   import "./MErc20.sol";

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20Delegate.sol#L4-L4

```solidity
File: src/core/MErc20Delegator.sol

4:   import "./MTokenInterfaces.sol";

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20Delegator.sol#L4-L4

```solidity
File: src/core/MToken.sol

4:   import "./ComptrollerInterface.sol";

5:   import "./MTokenInterfaces.sol";

6:   import "./ErrorReporter.sol";

7:   import "./Exponential.sol";

8:   import "./EIP20Interface.sol";

9:   import "./IRModels/InterestRateModel.sol";

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MToken.sol#L4-L4

```solidity
File: src/core/Oracles/ChainlinkOracle.sol

4:   import "./PriceOracle.sol";

5:   import "../MErc20.sol";

6:   import "../EIP20Interface.sol";

7:   import  "../SafeMath.sol";

8:   import "./AggregatorV3Interface.sol";

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkOracle.sol#L4-L4

```solidity
File: src/core/Unitroller.sol

4:   import "./ErrorReporter.sol";

5:   import "./ComptrollerStorage.sol";

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Unitroller.sol#L4-L4

</details>



See [this](https://illilli000.github.io/races/2023-07-lens/scorer.html) link for how to use this rubric:
```json
{"salt":"fad623","hashes":["cfebcefc60","015ffa8d88","677636433d","062dadb582","e942782ebe","ef1e313c5d","12aa56faa5","f101887384","ba240f491a","9a8f7ce079","01f3940d87","685175c41a","126bdbc3f8","5b2e73c7aa","cc549efadc","2b922fc11d","efa34d8040","b442ac7c4b","ed020d72cc","b82f846751","99d30dbd0a","70a680e413","853bceab33","2da9272cbc","38b8f73fea","92713a0a06","93edee1e06","c7643c10a5","14d3449b1a","554bddeb2f","5dd7d77af6","85ee2d1886","a2291dd2a4","6c6ac5638c","ca29d27fb5","91ec3e87b4","3dd9674981","e9ae40689e","48204a3cbd","3926a0550b","bf32783d02","284c5aa39e","9047572b3a","e7dadbbf95","ecf41cf54a","ff87ad6d8a","67d16ed132","efcb8dbda9","e3dee14410","5897a5259d","349f239c6c","52faccf619","ed4594ca09","ea04a12559","dfb9745dfd","a26e0c62e4","6efc28140a","f4ce696fa3","a266bc605b","0d3b009903","887d75eb86","fad217eb43","65581424c4","0206d2434c","443fd0fc0c","c49ef32331","cfebc40219","9e61dab724","48f5e96b0f","00fea9db4d","de85601ee6","be7e7580b6","9cfccc389e","8f07bcadb1","02d0250e3a","d2db95d94a","6651cbf662","bcef0c6694","59f3bf773e","183eb96f6b","589ccd3610","fab4c7d433","36676d7799","6abcfa7a85","2f89933b53","590ff6adfc","54e734f1c4","a32f0cfa62","2e2f63166f","5cea1fad30","75756ae9a1","5897a5259d","349f239c6c","52faccf619","ed4594ca09","ea04a12559","dfb9745dfd","a26e0c62e4","a3d27e2e73","35573057f6","f67e02a245","ddc74820c9","1b7038c4be","99db7577dc","fc10e9950d","639bf17f6d","5e05c3a692","64a1126ef7","fb0da4e88e","0b6361781c","a753217495","665f7ae889","afab91453a","137d6d721d","4d7c629475","a3f7628a45","3bb0b691be","addfb06d23","fff0353173","a3a6417354","3e07183a09","df0210a36b","ceb9411352","80dc0220d8","57f7e19c07","9b7a45fe94","20594406a5","cdd2a2350b","1dcbb2d7b8","77f8734f6c","ee064e6548","d7a24b9bd6","bcfe2e9759","5a87b20590","c510bd129a","5f353818ee","938f3a630d","cfad89cee4","925334cf79","0a08d9f7ce","57cb7a84de","4a6f91fbcd","6140fb5a24","e64536dfee","2548249d55","f254f7e659","f290428a60","2f18debe05","1ca7ef8f4f","5ae85969da","f49f9c4925","ebc4ccf4ef","ff7c09a12e","2efd0147a7","2c554076a8","129b02f983","7465408daa","63004baac4","11f7fa2fd3","d23964d7f6","65c64f7b8e","d1deeeb693","3138f2de9a","bdb9e0e865","9316d69796","b9400ee61a","e71a378fc2","c1cd1f97e2","e2bb68de07","e36271b924","5b6ed67e75","8e52629d6d","f09ebac3dd","7f94278e0a","f722e1ef72","2ea58dc4fb","cdd1924e4e","2e42271272","72dd2e83c7","e46e39faf5","a4cbe6ff34","fd98ce0630","c63c2e0e6a","82caacd27a","46a53c05dd","9865000963","2c4efb206f","78cfe5fcf9","09b35f711d","f25fa190f7","ce6ec0b80f","095a4b6bdf","b8de89d419","2ef9a2bda4","596034ccd2","498281ece0","a2afe73e8e","40ecac819e","154b69b3ce","8f9858513d","4bd7a836af","b155dd20bf","b285883bcb","7c1c82758c","d2eedf18e3","43d82c3a66","4cb967eb0d","0bc04b5895","92cf96e5d8","d4683b5778","a6d085c414","2a1a554e7c","ec48a8bd7f","ee8700b5f2","2289087621","3ca9ec285a","08d7509ff8","f642f2945b","b9d024f5f5","6dd63c736f","6a64a49e96","b7a2c23136","71b3a6d8f5","6173056967","c5def33452","fd343051f9","e9c1d541cf","540b5ed48d","ea76eead64","70309910ad","dfb9054c28","627119f2d4","77b9e67d0b","54ec19721a","26db92903f","f9826df49b","db262c90e9","ff9ea91aab","bf337b2d06","f0e638c351","2c171b8260","6c847571f5","9ad4ea9f1e","1b0bb4d034","6cc744323a","cbcb0d6f65","0338ddec14","472104b145","e1f4924dd0","2d59059b7f","1e0939c764","4676dac09b","d3b3e96e37","f906045bff","68cc6f774c","e0c5dbb9f0","ed52b4ea32","f6a8b9eeeb","81869f0822","5d6f710044","669df31647","f5a3f46189","afe3a48974","4335402cdf","fe424b429b","4be214f832","218d2ba03c","a5451eeb0a","daab1185d2","28b4ab6596","17e18192b3","96da5d4063","ea56abffed","c2d1854ce2","5a4c145614","3418642293","0f28ce9c90","76e465ee4f","92ac81b394","af5ab3a68f","e625334441","4b00e56f8d","93d3372e51","2a0f4a35c3","ac2a21d059","759ee9553a","288a3e8642","09661c421c","b7c3c94281","b966b04366","4ad9c42c50","720d8323b7","44fcd147a7","bc233eb0fc","48f0274ff0","c25f48cddb","dac79e94dd","ba76831497","3b99578ec7","af4771c6e5","e2f026c3f2","308f901bb0","25c0df7ebc","ca9e905a3b","8eb009be5e","2325d09691","3626a134ca","e293c38ca1","09661c421c","b7c3c94281","b966b04366","4ad9c42c50","720d8323b7","44fcd147a7","bc233eb0fc","b413b39ecd","6e310ba94f","98d5218e6a","f70d5bcd4f","ce5892a5d0","428db7897b","88d757e750","4eaa1fe820","764592d7c9","4592d05c17","4a91a6f444","8255087eeb","6ca44de78f","543e229ba9","4accef3dc5","8c3194b6fd","0e6541851f","889a208fe8","b3f52a5c52","04913b86c8","3d349719af","1ee38d554c","040dd60b46","c7fea8710f","a755199f0a","712388e54c","ebb58639c9","f45840fdfc","30505233cd","bcd65cc823","a60020c758","151acf36fb","8a78234666","f7fe806a3d","704533fab1","e7e3aa44ec","443fa0c5c0","9597cd7d92","c71ea07caf","5aac44251a","4be5e59bb4","87caef6430","05cdd72318","4646abe8e7","4c2f38f4a8","652c8a7f21","4034836f96","19972dd930","597972e6a2","123e07ff18","e80cc3bafb","e805b742bd","ab2c1bc8cb","1117cefd42","337c30a398","16d93c8ab9","b44b5c0434","ef0b65d18c","3fae9d2901","634b0e96e4","e2c6cc18bf","4d04f1018e","fad09794ea","e6a6dfc12d","4b0ad19d44","7c64868785","4baed6fe00","67203887ed","2bcf30438d","a600efd2c5","4b90ddcd1a","9fe58b0714","a92b732ce1","0924dcb7d0","8a24237e4f","34ddceebf0","0a5ee89b03","2ee66edb8a","419f910387","6229839960","0b02a43cf3","ec14d024da","4b95b0df65","c1ffda3b95","242ccf8e5e","f21a0661c8","4db1aaa07f","0a1a30164f","ec1c040ee0","08be90db83","6c8637b65a","3ff0f127c9","a6ab0e2e02","6d8febf04c","2e55d54f61","c53a9df4ca","e294a102d4","f2cc72e7e9","450ad594b4","5eaba2f44e","634a068c23","a61dd2a943","98c0a68765","3f6132282d","b7170f4da3","4bf6c53a90","87d8927c19","01d6d2bfb2","3ad538eaa7","690a917d60","4e9989b75d","ab8412ff11","34384d611a","24ffb4467a","0bd6165679","715982acf7","e4e6cb88b7","3aadc68e49","bc437d6195","4b90ddcd1a","9fe58b0714","a92b732ce1","0924dcb7d0","8a24237e4f","34ddceebf0","0a5ee89b03","e6cfd675bb","80004de05b","84f00d3537","843687f150","449f55f90b","a82658ecf6","9c67054c07","bc3e525a34","ddd20a6f25","4a20cc48bf","586e68191e","9cab0edb1f","907f6159b3","a0a7f71bc2","67f006156e","4dea51133c","0f5c91a5ca","e1d5d356d2","87f9c172a0","2e19740c76","1fda757a37","c4e37379eb","6b17db5633","e12105abe1","252da763ba","9f9d3b5073","093ee41a5f","5f8854dbf7","230206d699","4e7bf88e68","f88063f858","6365d9feef","fca57e8313","9451605127","02a96af687","27ffd02c28","82148b1019","ffba6febef","9ff488fdef","29187e5d27","b32918b0d2","c4edac18d0","30b33d33fb","6e5979e221","485f21d0ae","4759ebc23f","85aa0e4924","49c7be95a8","ecebd0f541","230206d699","4e7bf88e68","f88063f858","6365d9feef","fca57e8313","9451605127","02a96af687","9a674a6700","91acd32667","c4a5c1194a","b564068b94","3ebf9021cd","67f1baef4e","fd1909a547","acac9b4a2e","9da1c8453b","65bd54a5e7","680a50012b","2bd1a72ad5","580b0b4c10","d6def257a9","8df07ebfe8","3b271c241b","df0b85b714","e95ca5ab87","a3f7c28bd1","5f3f375ce8","d1725b473d","2eed053c9b","1cc1d6ef8c","3da6ef288c","c8a6ee92ca","d5919a8cca","24e15dc053","e3ef871011","cf315c1ee2","c790b6fe2d","cb73ef0f97","b72988249d","cc5b96d88d","029b01ff38","7e1042448f","ab78bcc52c","5ab3a7a11d","cc112e2447","45fc998eb7","b29d2e4294","d403c6023a","d87b2dab2b","e75f5af5d0","6b5c74b3eb","e17ded5005","71d13360fb","69fd788820","4be73f71c3","c809a017c0","49709cc5ff","40ab93b222","71e3a07870","0bd45c8245","1c895de8fb","be2593a8b2","beab40fdfa","ab1302f42c","aa1552533f","60ce40c650","216abd059e","6bcc2478b6","60d2aa7291","5076fedb6f","cea423ee8f","232fab6c2a","331d5ff894","920d68b454","98845a09a1","2fee787b98","63467a275f","5fbe2611be","cf083eed58","e74ee9d134","ae3fdbe081","22ce9b3397","4a7c68f03b","609abe6d08","69874b538f","0c8b53134a","a70043eddc","c78ca19b0b","9afa963a44","a9f7633f18","c4e94204c3","96c3f173dc","df3d9d8630","e3ccaada76","636f592373","c53ea3f679","63f79484e5","da4c03fd1d","4f070356dc","c0438ab962","c25f9566f3","4d5699a35a","9f04006b2c","4a64bf48c9","8cec2eb2c8","21f1c8a600","d70195b9a1","e2f2296d30","d3a9749275","64c5917aac","8bc5e523aa","e0a43e868b","237a89e384","75161edd40","b09a577d76","707706363b","c63eee1159","83aa4d1099","ade6bfe1fb","0223add2ae","d02b2012e3","07f31ffffb","d6e193c8f4","0209c22af4","b978f6d187","b106fa43f1","4fe07798a9","1fe57c0d1e","48910727da","626f199b27","069ec32a34","9fbe3ca856","ae73abd13a","84503c73ab","85513e5284","5d1f67634c","da3a41179e","2386297abf","ba44db26ad","053f54ed11","f0ea0d4e38","b6dcfc0465","179776b9c2","29ee05813c","7c1cee61a3","07ac5ce70e","5c441035bb","a2927a1d23","711eaeb148","f213b48fff","b4f8869077","0e7af9cb83","3d6098b586","f27f108327","0584769297","461d9d252e","dcd412a745","46e6c380d9","430d3a5d22","e665b43eec","3cbfa45739","2c0718f100","b537bfb44b","7188142122","e203256d58","d562b75cdc","72a294d082","a0011cb01c","a25972748b","d41c478c69","87365c603a","c5e10dda9d","003e2fc254","5e53320360","49813993d1","2c0718f100","b537bfb44b","7188142122","e203256d58","d562b75cdc","72a294d082","a0011cb01c","0584769297","461d9d252e","dcd412a745","46e6c380d9","430d3a5d22","e665b43eec","3cbfa45739","024f7f8fc3","2a046d6645","c9dc3e16b1","80d9a61c84","8b32810407","b8e0ded774","ad3a1e1077","bebfd7e851","5b42b5385d","47f17d6742","d85ca0c12b","212f023fad","89d448e7ed","4b490a5292","80138f5666","910da09645","8769e3c0d5","7c884c802a","ceb7946345","d0c1602613","630b9a382f","7d63d8757b","b53d0c7707","feadbee282","9023d66def","73297b2b25","c843311490","97231cfe88","5c5a0f67d3","965a853feb","3683615c84","ec08dc2c2c","ce5e09f5fd","23e6dfeaf2","01d3af213a","386d5875ec","91c9e4c341","7a9235d66e","e9cc92a66f","549296f60f","2205693b43","4dc85b476c","aa7bc996cb","49f50d104b","fc53755a30","01854da535","1a7bfea9ce","1eefebda48","1e79e1f561","351d94637e","852df871b0","ed196aa86e","bd5732c60d","dd801661cb","a58fcc90e0","8e0fcc6e0d","8d73ba9f52","e6696b9b76","dd5b5e96e2","2c5577484f","b2a31c2469","7c3957c0db","63208be2f5","993965c4ba","da52eac25c","61ac0e8aff","33c5feae49","54fd770cc8","217bffbb24","d26f3e012f","26b59d7976","35c83066f7","a1b322f64e","4bd5d8f66f","048781d534","014d08a5aa","b82e6aef83","f85c2da4f9","26695c1497","cc233152d4","f81c251a07","04d75b08f1","4019168efb","450cb93b39","b8d298108e","1212b49e75","d993743b5c","249d3b521f","fe451e59e4","517acf0779","dc6466e244","d8953afee5","1620539d68","2f5cfea5fe","23b65012f7","7f60208fbb","05a77810c4","dbfb7f27de","130c47fffa","fa86c51842","b72725dae8","1674688ce9","7c87fd6bb3","4099053e06","ef553a9205","1ff2b414f8","d22495cc93","3cd605e3ce","b960696a72","66a982c918","05f7c6306e","b03e8b412a","40210050f5","84e45c09bc","4d70852e6f","614012c18e","a98c9ef1a6","30cdfdd33e","6fc8adb6d7","846a378782","f4ca6410a0","36b535d828","c5301c122d","768ff97d19","6989bdbc31","2cc8af7ed4","43ce3cbd83","bde5b1e9a5","f32861ae79","ce0341beb0","d5d5670725","17396087c3","b374a915c0","ce0de27489","f9053a1982","7d1ac6c42c","d0121e04d1","f103787d18","5a8ec09c93","f092c6ac71","91228dcab6","1d71df6356","1af4896dc4","e84ca5ba26","401a979d34","d22f0ea792","3589a8713d","40a69f6960","dcf2c0dd9d","eb0cb97925","9803141cc7","7a97bcd3d2","6c621c9e59","36b1460039","656d91ecdc","72391f4584","80ace4cd4e","5044155695","72deedc6c7","91934b09ad","b58c441f55","2a1127c077","125af2d32e","d0a9957ca4","35e8724219","c11626d506","05efb27c2d","7b5363c5c5","639f931623","06438dd977","13ecd0bf73","7b3c335d46","88214059a5","a168ce73c1","dde25bd556","acdeaf969d","01f2f2fa8a","8823c057f5","b79de91c84","9efb6106a4","6bc7693e89","6738c05b02","4b3fcce969","f9beecf7e7","48a30710e7","ed7a6cd7d8","c161bf830f","2d20022b4f","af21f8fb6f","ae1971f57d","8bbe5a7b83","d009c1004c","82d27864aa","9ce80b6b16","55e4e818bb","341d7ba670","de402d48ec","3d744f2cb2","8e37a3eb79","80302f2df7","885d9c5efc","2233b26534","6de4ef742f","f64a5c17a4","6576bed23d","68aae63176","a041f06e57","8f169558f4","cb702b1295","2df8a3a63c","c8d590b326","4dbe5d6826","220215a8cb","0eb5852afe","58552e3b3a","5a92e900e6","541601a94c","ce05e37705","d14b7ff633","465c801b38","b725e6c9e5","67ed310080","c8e70dd423","1dceb4842b","da0b838929","38b2b8c2a4","05755057f9","8656d74115","f41b9534c1","1236463852","a4d0d4fcaa","da802f0420","2244bb7bf8","7fa66b0f33","cca302a258","ddcf38d5dc","9fcf156256","21aff07d18","93603d1af9","aa0b4d4ec5","3cfeae2b2e","dc73b94b78","ad56ff2990","549075181a","8325197771","4dbe414bd4","b3b863cbc1","269b054f34","991418b523","f61a33a61c","6755ad4ffc","e716ba54b9","66f84b5a81","21f0c04fd4","a29236c737","c4b1f0677e","6ac2cf9d31","e8a7069bfe","4ad72160d9","6e25fcc9e9","33a6431098","73dc7c8cc1","ebf0df6ed9","62bce91906","00cf142b86","4cd6aaeb5a","d829aece5b","4564c5ae08","e178c94a0d","64c22fd74c","c80a7b2c94","2610603a9d","46cb6353a8","faa652efdc","cc3f02e1a8","6ff717329d","1649886146","c6225dd2ff","4618282835","41d1cf7ce8","1bed8413ee","7174b7bab2","8742e853bc","2902cdb089","8a84cb0dae","a44eb3fee4","2404f386a9","02434f3308","29ae74f232","6e1d196f5f","772183f26e","9da41b8792","d959cdfa65","a1c770a8e9","24b3886738","be209b4d99","d6188c211d","1b9860a914","886f4b7014","ab03ba5690","cf2476c2f6","7ffab43d53","224c299524","6be5f83ee3","f48e502878","cbfa996899","53e91f5ca6","e65becf4e5","8b98911455","455d98d266","89e7729a2a","d8c7dc8f7a","fbebc0809a","2b9d70e3f1","8e33bb8947","f3838dfcb5","45e999c7a3","6398fb4e13","cf7962f9ff","80ca492c94","5a9230f9fc","d6fa4c63e0","b22e34578f","ec0c5d63d7","e206a56b44","ea49ad3a5b","6169936760","2198069457","5b8ca4ae03","86ecd95e53","8d0483896a","41233192f4","ca8435613a","10000f7685","274c8e2223","079071cfb6","a8129b57b9","a084058f7f","13d6c946ab","3287aa4c01","58b1e4a51c","51359c7c7c","0d4cd465b6","256b6b6313","b992873405","953ed1496a","69bd9a25fc","02012cb3c9","7bd74b780b","5444e40bb9","5c48b1ec85","c385ae7817","23b89cd4e4","f6b61eeb3b","c43d12361f","fce09aa133","b83d2a48d0","668a70a933","37f6a07ccf","c3436b5c4c","071990c456","b43d28f66a","8130a4431f","e6bc429d4a","542a3db871","bfcaff7eaf","0799ca5aa2","b9b28c7f20","c99efa1a70","31bf6af21f","5195e56e98","de204e898a","15c21b85a0","ba038ddef9","53736e10d9","76aa6c0339","2b9a92c405","8acad42e48","0fa2115735","5cab1a9bf5","5cb06bbeb1","6ca6fa4634","bbb3bebf64","66d8b138a3","3bd2a42e9b","2d42d1b717","00178d53ef","5529ac015e","7bbb2e09eb","17e435249a","66e0aaa0aa","41cb8f66cb","b57629d9c3","f16d07209c","27629f9a52","97202e4a7a","232730b485","0c1a5c2d32","482f43e24b","033bab39df","37a8fb4ec2","59caa38017","3a80f2a2af","6a7d1d0a13","251c46c7e0","697be49567","033bab39df","37a8fb4ec2","59caa38017","3a80f2a2af","6a7d1d0a13","251c46c7e0","697be49567","097cf3cc80","3d9f991fe3","a4cd0533b7","6fc876dd73","a68ceef797","7ae8f0b298","e15c3fd471","407abcb93f","cc4c73a7f2","e0fffdadb2","06b49c52f7","3dd3154083","fb91455b60","c3a1ed5838","13add12d3d","2435438667","4c9501d57a","dbb0cf0ea0","c76952e21e","53413042c8","6bdf705f72","c0c52f98ed","50240cd536","e0c647967d","fe73961be8","5c0eed1a08","a37ee642a2","7c978147ce","0f34d31bac","ceb60a31b5","aa273a9dfd","0d5ad61f50","5bcb5fd881","f40e80ceb5","2c5188d42d","96ece61fb3","2bf37d561d","d95f871715","0be3339001","40e451d760","5ce7652c28","a71744a843","b376cf0bd6","75fc802a72","7f66a85235","2c3e063f53","43db9d91cb","615fd8f524","c101ad62c4","057b5e8e3c","29f224c78c","9426ebc330","7ff285e136","3f6ea2ed6e","8169f7fb70","ad9608b9fe","0461b13fb8","e23ebd397d","3a4ed86f41","abebad313d","52f5c3cf8d","17b508348e","61530526c3","b961a2bf37","722b3309c3","e9dae252f9","96b76f8f42","1dc9c09293","964d51b0cb","5446d2b1c3","a3a39006d8","3b14dba430","8b29e3b3f8","5ab8321ba9","83da7f6dfb","2c2fdfb9d2","9e44f89456","85033fe9ad","eae0a6b1af","b30ad9e325","b4f0a6b342","cb5183331e","43b9f9cfe6","fa18078e36","46e3f18944","05cc0dfd7f","0e7ab7ac7e","a0c8583cda","e0e3aa8abc","982ccaaa3c","7d9099b242","e1c244d42e","1865696e94","d8a383a6ee","f01cfdd643","e9ed7b20cd","88188bccc2","60f730c399","11c6828030","fe9a900e54","d3f963a1ba","989b9184b0","70634f3ec9","d13515bd80","787733fc7e","1772d9d778","c0e30cbb89","e9526fe7f5","c7881751c1","7ed33eb751","fb465095ad","23667cd039","53efeeac00","986df36997","7023ddeede","4343e93375","b357190618","655c0c39fa","e326f10b1f","e88c389190","9031890a18","5215e1026c","57c726fbf4","48a128ac5a","2f4cd10dae","60dc853c35","71e8a9e611","bcaf497c2b","5c8b9d9412","a2681d0c15","ba028f7b24","03a04c8669","1972369168","6866029458","e2f5c93136","97471d9cfb","a736b77210","a3ccf10070","5eeb1e0b1d","8788c0905e","13c9b8e75f","e77f58d52a","cb67ff7bfb","4b7e9a6896","4ca7189617","a96580cb0f","1c33aadbf7","13c9b8e75f","e77f58d52a","cb67ff7bfb","4b7e9a6896","4ca7189617","a96580cb0f","1c33aadbf7","38b9c602bf","758acaeee3","7e41f69cb3","d81428a3be","a193bc1a7f","240ee30cb4","a03e4505d9","14ab4bb5f8","96890355fa","e9f0fd934f","5674f4400a","85f8e860f8","8d5301f0a6","c4ac441379","9eb614f5fe","b06b3e290a","1cba719d03","a87e302e06","c90beed6d7","c6662d332b","fd5bfcbea9","4bf1198bf9","f6f3aa8ffe","1ab99061c1","2aa5b3b823","629485361a","7e6201d6b4","3b34b419bb","dd95871ce9","d967b3d59a","565d8dcbb7","94448e7e31","d99d8daf6b","5a45f9f1bb","cf96503ea6","955d2d0b0a","ce6764d59c","5fe15e444d","14b15e9737","d9998c0de7","4f87d2a5b0","07f7ddc600","34001c4072","592d4287b0","58ae3d4b04","a3e9cccc89","314596775e","71f39bdc48","f16565dddb","34001c4072","592d4287b0","58ae3d4b04","a3e9cccc89","314596775e","71f39bdc48","f16565dddb","955d2d0b0a","ce6764d59c","5fe15e444d","14b15e9737","d9998c0de7","4f87d2a5b0","07f7ddc600","224440f726","0f20e74b66","9d622a44b6","c9ebe34ba5","e9064dca18","4a8cd2d120","63a8f5efee","f12a6188cc","b3bb5f5607","d02cedb55b","25a784e72f","0f5f9f741b","165a4e817b","3d92ef1ea6","e9714e43e0","1503b3af29","06ad173065","a6d4bc5426","9972062934","b5939cf98b","e228d1e8c7","c84c0a0d83","ab7230af65","4ea2695414","6d8555eb4f","6e37b0d813","9c3b17a4f5","bc761ebf38","14da39c2e6","10e3421aba","4cb5c30d2d","5ed2fc612b","41d5da34a6","514ad6d441","b3630aaefd","e9714e43e0","1503b3af29","06ad173065","a6d4bc5426","9972062934","b5939cf98b","e228d1e8c7","98b0ed1e32","02eacbb518","94d7d5ef86","b400dd0510","32c3073faa","8ce77af7f5","96c79e1c0a","96b808de84","d2d868894b","f60835a331","bfb3ff1de9","2e765fc44c","072af933f9","2e6f04dac0","bdcb4e01ad","b08f9d9136","dad50b003c","0567e83ba5","1d78acef55","933dc66552","b4e5b92581","345446f0c0","a2fe5af874","a53f7ebdb7","d4f8b3ef53","12502e259d","68471bca28","8cd4fc4b46","105dd3b2f6","8f8b75b47b","cc5ae62d75","79e365a92a","64fd084b38","3fb4856655","f73d5b2ca5","62a5c8f35c","3f8eb58c27","5e9e48c1c1","c88e23c4d0","f5371bd58a","e3624d2138","26397561f9","eff88dec37","8c80b5d1ab","10fed771dc","1e36521793","9a16fafb16","41bd6c0675","7528789eb1","4f2a90b466","9ee6010492","e77efd8255","75e2777b4e","f58b4ae05d","0b15431989","82fd2c8f15","fb54f352ae","8dc3e478e8","6ea32904df","3d28db88b5","335fe3f904","8a0b540394","0be21f4021","27e0c7f7d8","3c7cac22bd","55f1217f76","6a6010cd05","cbb21b3abd","5c5a108f5a","77dc0907ad","55b0379f26","a454eef59d","a45b036398","bcde8b8f15","3950b11f4e","6a0621082f","15edb60952","badb3ee256","8cfe3fe05e","a95519e332","4246d5fd0b","3fb234f908","ec3325becb","ea769ef610","f86b0001f1","13029a962c","337195342e","03531b1ca3","39e7305b1c","b80a5b79bf","795d768498","22d181e0b8","4f4a350c2d","03020ab0b1","075e577d1d","b8a87f110c","42a5f9341d","15507d47c3","4b77690457","3263238cb2","6c065598fa","388db51124","bd3874ab66","ee881215d1","4bf9ff4a6c","1abf2a9dfe","4db47b98c1","8bd77f1762","0b712a0a91","b34c71806f","3efce76855","4e9c1e05d4","c8d87a02a8","761c026ade","0bc299e7af","dda363e991","1941936bb5","b344336f4f","fd73decef0","669270ccd8","2c8591494c","dd87e9ecfb","fd41582a93","acc3250214","c9577f6b33","516e6ed392","64c33cb040","af4ec208ec","f3a5f17b68","78274678f0","c51861cc00","66f02de614","c03eca0acd","4f95aeedfe","075a6a6e20","159e30ddca","d23c11316e","8cbc9fea30","8536188c89","4dbc4c472a","ce2d2e83f6","a5c9cf2549","1c1970ca71","196d54e904","48d0d4c395","7ff2b59c61","68c7ee9f2e","708a9e4812","f4eeb7ad14","732721b2c0","a620895a2d","297d7d932d","dd8c6f15a2","1b4311b7ec","9bc61918b3","2d84ef570f","e369afaa22","16b5861bfe","debb07c69b","844d90e517","f8932dc09e","fa7c1143f3","a7f7072944","89d45e2c8e","08e3de4123","9296497dfc","33ac1eb52f","764d0945dd","708a9e4812","f4eeb7ad14","732721b2c0","a620895a2d","297d7d932d","dd8c6f15a2","1b4311b7ec","4f95aeedfe","075a6a6e20","159e30ddca","d23c11316e","8cbc9fea30","8536188c89","4dbc4c472a","ab4b7e9264","4fe78636ba","255346d2c6","3939e71779","73423ed7a8","4363d38da1","cf14d945e2","3344dd8f45","b927735619","e021bf0d89","e51e3cdfc2","63bc88b76d","84f0c6a680","9924d51d95","e7147ab94b","1e6b0b7e29","6665d5579c","ac73f0b6b0","fd1eee2617","7c4dba62fc","6657e52e7a","6a33e4a2ce","1efaa35f02","231b91e2e7","17b00cc4c3","c9a0527638","37ffc697ce","0349ad82b3","6069e75f9a","28e29fcf23","3f3b38ebf0","8637e2ff40","1045489ed9","bc6779cd72","9222eac970","b50c24de52","778ba94fd2","475a4d0d49","a01b6e7a67","f3fa1ca046","cebaf63274","3ef9d632ca","a4c95acf35","5dc17e52de","96ef26e674","655c6e0200","5e395efe40","ea4ed1043f","d693d40566","94bbb2d0a1","c60ef2b841","2c951fa8f5","f909469369","38f3e10ac3","1680ef3eb8","bcf9a1d21f","17ae3fbb7b","3015705a1a","94717316b8","84cec73def","5b6aacb93e","87911077d2","9e03041310","5213c61587","e8a8196450","f36be2c3a0","45b57c0ba0","76dc537062","00b962de57","125ad23e4b","1583e5505e","0258800aab","f5c008a165","86638a0828","cb7b950612","cf7236eaeb","5a4fddf719","42b0785ea6","7f86aca4a0","659de95ffd","39e3dc2aca","09e707251a","2604c04036","60d87826ca","3dfc4f8b63","5708167640","0e7f572b49","bd120fba4a","0ded40af33","30087136e1","60d827003d","240626ff54","911c18a5b6","79199ef7ae","9e76dba820","cc9c37a0f2","2f9c8fb2d6","9cf7795b34","0cab8368af","a9d521cbfc","ff33db77cb","e574dca35c","c845377026","5cb51f64ea","55da975aa3","c860f751bb","5621cd74f9","656c345758","cba4a20217","70eed4c23b","60cf2df0f0","c44487fd49","a6bc9f7193","f736f1ea90","e7866f3e27","7d5393a9ea","7670ae461b","6c03f47c3c","b9c548af4e","feb680ac1c","3b959d546a","1b92804219","19eae5ca87","ad8a86ad53","8a9ac18b65","493028b60c","179328d5c2","31cda1040a","1c32fc40ee","26edeb1658","9e21d4e8e5","b51632ae71","9e4f710860","787656601a","5a5127ae26","5e5e345c7a","8abb7ef668","7ffef6efcf","98b321fa8e","9963c532a0","335f1fcf85","1ce3aa7e68","385974baf2","f089db7fe2","5dd7acf70c","81b0ce1204","055e2c14cc","4966405715","3acd8510eb","310cfb148f","83465b0c57","2f82308a73","76ec1f0f75","be64b443a2","73a73273b6","c6fdb1bd2e","e9779ce342","c90977878e","2dd4ed9e07","63692c82db","53cf97ce09","9472bfeb92","cd062282e1","b1a3c07975","e8442441f9","8f242b2a05","7c8f6e9808","fcf6f7c002","376116e170","99807f9d5f","0e6f56470b","535d1bdb5e","b2e80cbbae","e6fbc3f92a","7ea2c0bb60","4966405715","3acd8510eb","310cfb148f","83465b0c57","2f82308a73","76ec1f0f75","be64b443a2","c4f98ea359","23db0e9b45","4b6dc5560b","6be8795d8f","520b154e0c","51597a1565","715c74305a","d9fd8756e7","de7aad2d4d","341b2be0d9","ede74c74e7","90566ed1b2","785efa9b8d","d3603ac57f","6c80acfe93","e603bf3c72","35664e7170","2f121e4d1c","cf85d008ab","b419d5f4b3","d81cbbc10b","53ef1da700","a13d2b2a33","bbb2d709d6","a7e0543bae","938a71b325","8db7a10a83","b2e6ded07b","774169cb10","4999a91150","98e6b3a3b8","d0d3e447dc","dc1d00650a","71d89d9352","02e63a8167","36f371626d","7f1e8905a6","49d2b03b73","f16bc4db40","71710c4f26","6090c07ce1","69b1f97cb0","825949e1b4","4eeb614813","b06cedd073","37721b2b8d","0485909b07","5c8cb946c3","a78c621673","7d2a05847b","bf9e774c94","d06f73aa1f","cc97247ad7","5e481e8d9a","60343904bb","46a48c6243","0d74b37828","9fef9b761d","8a25166ce1","a476c4da8a","46e3c94d70","2d66ea9b55","e05bed5cf0","0e09cda03e","d25db0f07e","961eed6c56","ea023724f0","3e9deb0f90","6856da56fb","db2c9df880","554906c5ce","971d02e03e","81baff3c98","37c6639f00","c7d14f1024","7009ab2a5e","1f3fbf331b","04fa59af06","14a731963b","d05592f677","e925aa3e4d","e0d3858b1e","ea6cd6d6e5","ef0d99cdc3","c1d3ebb3f0","1d1fd18ce1","1dfe639be2","ff825ca4af","025ab115ce","4156192ec6","4cd11a3c0c","3b1882ef1b","fa11a682c2","10c9435fea","d698090221","8ce7dced55","b5e8241f04","4d284e0583","917166ec45","b123e374d1","adaafad397","4391a0f5d3","e45c37b2c7","c99a6ada60","94a50b829a","a95a50cc08","95921d9e83","e526180d37","6f65af8eba","afe1472479","14f4df3916","a3135dce5b","1c9dfa9536","64e6196921","0a998fa6ca","cf0f5c3c94","8a45b24233","44c4df2402","2a9dd324c4","7a4ecd7c68","4c81bc1bef","627f4c77d7","cb52b93a45","16fdb5ebb5","1222bc4457","54cf5a6cee","79d68dc017","5c46bc5e64","3c59bac1b7","87d9cded58","32d0ac942c","fec1f8213a","115ec363fd","8bee18d18e","360b1d1182","4a41af7763","6996c63425","f039c34154","bfa92f3e64","82060f1c06","9a1eb5788d","c5e4f04996","97d868426f","f303dccdd5","e428a41979","86cbe5a441","8667b21ef4","90aa28be97","93c9f6fb61","6ec0d87b50","402f4f1192","6ac9d32d9d","364dcb33e9","619eaa8316","c6de462a3f","bd68642b00","25dfec991a","92b28ac7a5","c6b4e23bf6","fb726a045d","512a33ec31","e9b5d78e8c","5181f04e87","2ade5f29fa","ef226f9ae2","e7c4747857","3b94f9d8b4","6000faa6f2","a044990da2","6fa0856092","886ccba21b","236097e39b","8d8ae00c93","dc14f08623","19b53b73a8","4b8fcfbd70","5fd0917bd8","07c19c603d","9bed17619f","e3d81c0134","02134d3d7e","d5b4708417","4215973120","92c2dbe2f7","b6b4b1fd0c","7cf0dd9ce2","037c11a375","19a8ffec09","1bbc3adfcc","4f3af3e42a","d0c24ca0d5","bef25ed077","86e5af6b21","8b38740a7b","b1b031bacc","c3f8df2f1a","bfbe77e80e","6e078af493","0690206190","d37663a421","d7346ca21f","5649e6e395","7325be70a8","c1550d35c4","ac99115d6f","e1127f7bde","b1c0db447e","b7e04338cb","7f24ca78a0","5b1c55ff91","0b306eb72b","0ddf971ff4","d57402746d","cc3a04dc06","1219f719d4","ff666c9371","fea5f169cd","9064c01bd5","e2097dfdd7","d3f992f386","76e77d704f","898a74812c","03d1f9e373","f6a090c791","88f36bae39","c242369bac","7ad8aafb81","52fbe56a91","d1203aa1e3","d7ee81cdb1","dc3b8181ad","704be10c8e","12288dacc9","993ad1c89c","f7428e06c3","82eab06f4e","8abc32aac6","27af8b76e2","cffe7e6977","da9aff521e","4e88900ec4","1bba992a99","fff0517f50","b550bf53d8","b8ce6b3682","7538ef9951","94d6f80dc7","306cd6e6b1","19db2a6403","419aaaf0da","3181efaec7","6484236040","24802d93a8","8965097cbc","55f4d2e4a1","05292baca5","954d26ffde","5a32b9603e","0435dae6db","83519154fd","6ae68cb5f0","b43695092a","bcb46b58ad","9e8e86c40a","fad902358c","b80f8d6b29","112f3ba61b","c14abff499","2c7f2d4ef6","cbb1b8db11","b38ca35266","30a3d18884","9d7bfd2e81","ec831c9eec","5676d9fd46","71de76518e","6ef43612c0","f358a152d3","7960d60cc0","d2c764b060","d2ccaf255d","b9baf56823","893f5992d3","7b482ca4d9","9416719838","3bd397f742","919262837a","e535295954","0aacb61766","e6a955cfac","c393a8a356","0265164cc3","2cf920f1e8","834e75ce94","1a8e8eac3e","0743db378d","6b03984905","4953771cb4","7143003cc3","c84ff62d4e","49947c39ae","796a3d0607","be0c7c0034","3c2ac123e2","7160a6ae92","5a85602d24","0ea3067ea4","a2233cfb79","e807fd548a","6f83271ba3","031df04553","20f670a33d","6e6c099c5b","d3e62ed69e","ec97c37830","d2ccaf255d","b9baf56823","893f5992d3","7b482ca4d9","9416719838","3bd397f742","919262837a","be0742b869","851a090584","307dd004c0","d4c742de61","9b18acf30f","7745080365","2c2fc284f5","5040b3ded2","fd6aa9a418","7d83037812","b157b5da43","271c12c561","a5a822fb26","aabf3e7133","f641166af3","24119cd6fc","d4132a5afd","ed1be84cdb","277a8df45d","291a41e607","238b091283","d3e87d4b2b","f707f25992","0f24471783","ee0b2d9648","67a0e6895f","a0f3c9feea","f68755c5d6","8de224c064","eb7d783f30","4a746535b3","51cbf96037","be787aff45","538d9702ca","f4ddc05c94","a57063aed0","aeef3a64dc","d20113ae02","8e4bc88fa0","71d502e59c","56eb31b952","16ad3b4c45","7e89c2617a","12cf400946","03022e188d","8c16956d61","fdbb7d7392","1760c3ed7b","7ab8fcc328","2de8429613","d2b6519ed4","a66a5aba9e","59c36fe26f","e2d862bccb","09989e3be3","e7e304bf5c","f2977f8bd2","38794330e8","bf56dd4fbb","2170f98004","e76ab33327","255f5d3e58","4415649e94","a57063aed0","aeef3a64dc","d20113ae02","8e4bc88fa0","71d502e59c","56eb31b952","16ad3b4c45","b82078e516","e1f01112d2","ec24952b9b","95d6299465","17b3766de5","cb0b828a84","64f49dd836","1697d531fd","92abb5336f","dc84886111","b974a31afb","fc34123c07","3cd40ddd02","342d6347b8","2763826a2f","3a3650a178","13f751d076","364e5b9ce4","d8c3c352ff","91e69167ff","a8c9003fd8","9d8341b69b","99ea530f44","6fd419f0b2","832f99f37a","8c2d7324ab","c8cdd68e58","1c433a31dd","3deca0eb51","26d22a7e31","3ba1e217d6","03ac13a11f","ecea887dd3","ea82fc29ec","a84bfc017a","f27de5c7d9","2d32db08b5","c6aa1c3f78","92d7e3cad4","8efb36eb25","95dd23ca53","f4e8f5afd8","3deca0eb51","26d22a7e31","3ba1e217d6","03ac13a11f","ecea887dd3","ea82fc29ec","a84bfc017a","c47de4a160","3b356e7d54","b333ec91db","3192dfc3a0","9077b9ed05","977f6ac2b2","6c7ce6f113","e97699884a","a125ddbf59","310186f007","47ea3b167c","967036cfcc","25da299c14","c677582060","d9d7f40f3a","4ecc2180d3","fb9ace01da","465d62664f","a8acf96c5a","ed71e6d212","28e98b1b50","d4608d00f9","93acf058df","297b34ba77","c232e0198d","97a6fa5a3d","98ff318e42","7ecf45d39b","224bc10869","eae676d6e9","ea44ecc157","1e9218b814","20935d12ce","2e1e4b329a","a36a9a5c27","c3b30ca585","edce7ebd61","db50d9b65c","ac71602ca1","4555d53c3f","6e348f2888","e3dbf98986","392b54832d","72b9f33692","481af02ded","aed0c4cb79","baebed3807","fef69ab4ff","782626021a","6e35e8cb5c","ccf00448f1","fc1003f8ad","49f94fcbbe","09b1ccce86","e356e4c29f","b275778e10","78688ad772","ec40918f2a","584bde38a4","b7035352d0","101fac80ee","16220eb55f","9e2c571429","413d5dc2be","309d6b8345","2c6d87edb5","8f6e0fd5b7","147c79a6f8","a587afdac3","bce44a0626","743df466ee","310cd86fb7","257eacc593","e40f4843da","e1f0883498","d3f049b117","e9f448efdb","5cfd60ab3e","78560faa2b","537c8cf838","cd0b78fa69","6c3ddf7e2e","3fd282bbe6","a2d28373af","e83bee96e5","0c069e3350","7e31120b49","ba1874438d","4691bc6bc2","cdb999945a","8c76932cee","8dfc66092c","f6a32dbeb5","db18644412","8e31828b7f","7c40fe383a","f85bc7828c","6cac2e8aef","b0b8335552","c77a560905","a637642b41","78002cac52","00828e1ce2","78962b8ab6","3c43a0f594","f170c44ab3","f849e47feb","f3afda09f6","be106841fc","17ac450131","f766cc89fb","73772eed82","d6282f6e50","4d273fa828","41297fe5f3","c4c8eb41bb","0e87f1eefe","e6200db8f2","fd1519fbbf","e1302def21","11c028555e","5fe9439494","2f74a332ff","fe23097411","07762b2e29","a86628c5aa","481f7d748b","ee79dcc591","aea87a087a","811457c85b","4b68b0ccd9","cd04c0cbbc","4d332b91a8","92eeeac07f","91081516e2","ebaea595c4","73d166e188","7babe9ba9b","22a584a966","1429ce183a","5fa68c97b8","205a0c790d","8702be133e","beb935e2f9","f280a03230","51040eb182","d244e007b4","b4a9a881ca","3172113560","72af7f6526","f27e1ba7d2","dfbff98f59","f959a95216","368cf49eb5","8e2e9355d1","673df7c9e4","ba23805e43","b0f532d1c9","0f1cf6b326","77a739348a","462b9d16ec","d46d612107","0f30c5325f","ad94eae217","1277acf2e0","414f96b550","4c14f821cf","7a8974f5f7","62de71e875","a68e312ba8","7c3a25a3c6","a7ae0f4250","a36b017384","8389e5177a","1147eb7012","e2998f52f9","23635d58c0","a2f411e2a2","57b4490310","31dab3472f","3d1f0241cc","1806a12d79","3e8a4fe93c","e703260f29","a88c35f946","bbed97d435","aee1fa8463","c7bee216fa","a140cfa0fb","2550e10a16","ab2ae24223","37ef5ac022","1ec05286a9","84cae16196","5a78f6b686","4ec848ee83","47b12e547d","49aae1c35d","5d64992c9e","2313fbc6fe","5fac451f62","2126109e45","b47b7ea908","b362beff17","8b732b4ba2","cfba2230b4","e02b62b85a","3ae506afcf","8c06b9304f","18efcddd28","16f888926b","0fe07e044f","71b4e99fe7","ab97410edf","2a461b667f","55f7a27a0a","d3bce92dcc","9673b5598e","a78d836c6a","eb8cf2af74","132288e98a","8962866f26","5cc4b10bf3","3fdc1a9464","46c29d7f55","c380f691fb","294bdfd299","d0b00c553d","ffac55ea51","1baa9d634b","d613de0274","9aa9501c4d","88dcef6dfd","a279627dfc","6b1a31704d","01e28afe69","332aa5e543","adf16e52d5","03d094432e","1bd522bc0a","6bda43c79c","cb3f92c201","ede115500f","e69fdb0079","534162acba","8613fb6652","3d986e2c85","de12dded88","637a99c6b3","ae10d799f6","3e656b9495","8b5d8a95a6","88153ae7d1","34364d2c2a","d93e70effd","b6b613fc24","44f08665b9","b58bd9a97e","bc82606c9b","ec924d4294","49fd0ff3e9","3a69be5bfc","b966fa20aa","d018fb3590","26fdd81436","8f25de7559","1963ee3fb9","5064557cd3","ccc4031e15","3998cf701a","b4d6b3bffa","5a66398458","62a3913e4a","3ef326281a","ff022b01d8","bc547f2c57","d5d6dd7a3a","fba9df206a","f89c5252e7","fdd6704735","972be548d2","6e6baaf008","2a9f3b937a","9a2918fcd6","40240f32c0","6f3f1e7ac6","3411ef2a90","da6dc2886b","4ee303a55b","ff065e6a5b","a6f92a1251","19f8895c31","155372dd06","c23dbb9c46","2d0364fa8a","3caba85d21","d6cc5154fc","83ec493579","15dfad6b3f","0fe3a002ca","e34976375b","8f0ef1f6bc","2703b428c1","21b073ae18","a1f64b0d84","e6c57828cd","67afd2aa19","dbeb35efb5","b56db892c3","45b95d2e5d","b7466b4d6e","8c744f6a59","6ec66f92f2","3161d3bd4f","ac9975c149","1a4c226724","66b15aa4d9","6866a95954","34d0800b8c","627f85a098","0685443171","ff82fb7e80","aed493355a","041162861c","4b2ce08ec5","5c68031010","f3448d1199","8c5cb15ca3","8aeff43a82","4eb5b998b1","26e753a230","f20def6d1f","e83c6009cc","087081a2fe","05b6ab61b9","c90bfb3f32","6f5a4f87af","798b963710","f8f17681d4","acc74ea300","dd79eaace9","681c87cb9c","c99da4e96b","44b2f4ecba","62fef7f7a6","334b5823bc","769a4acf5c","7bad8104fe","8b2d9a373d","ca1b91b1bd","9444305c92","e3a04601c4","0fc3c6a181","629405933b","7ce13d358f","2164c258a0","ed1d36c40c","177e60f1a4","67122b7be4","f7198e5626","ed29ae71b9","ed12cdd2ae","b03335d95b","1828a87cfb","b0f05e69d8","3d594b905a","6254e4b43f","3e96ccf1b7","3e58dd9240","214e3f5ca1","e076fb62aa","8927b0f1f8","bcc8291f22","9326ce3b6c","6df15ab6d2","d88de5d64e","c55d078f3a","5fa2830226","3f2e03f4ae","c77b07968b","c069b46a7b","8bf2e29b41","5d0e0430fd","80ed351829","e76ff3e7f5","21af33ca6b","f914a6ba6f","6a141be0cd","e7ce1c3361","5721354e71","6b2bbf1b46","83df6f199d","7acac362d9","4e06e842a2","d31ac0ef9c","9d87c705ad","eed50a3fc5","30416686be","2562ca07dc","7f173480b3","6015f0bba2","98c760f3f7","b21b56bb2d","ac5e3f436a","82916e4a42","0836a2f334","cfe7a894cd","3b9f55d344","d6ecd927b5","1444a886c9","0ce3f16384","7920661a3d","695da34f3f","f644de658e","7c4f5e2018","a5a0c6ce69","47441d9e09","7aa6e56194","82255e6438","478ee41e5a","a9bbff117c","863c1936d7","43516c7274","7b52505e23","2aa575e385","c591798814","6a5b1e0fe8","673cb3822b","9a798a01d5","e8c380ccb3","ffc62d89ed","0f276e5ec3","82a5d3cec5","0404664e83","c1eb5151c7","6a6c22a6d2","a7c470fe2c","78436f89ba","472807cf3b","537c247715","93e16f6cd9","e0e0766695","ec3da9f01e","1e4062a3b0","9e6058cf72","299656e6c9","98ecc405ce","6910ff843b","cf2768f127","1f96642404","b179f166ec","520721bb4f","2bb02d7816","0f9143cc24","e22f8beaa7","6aa5f9fcdc","b97a104353","b61730a185","107eb94736","8cf38ad6a7","0e138c8187","6c2e04e962","3741864f03","48b654e18e","28ed309d71","d98e339678","df29ead446","3467210e61","3a85bdc5f3","9af2845d54","12698c2a44","50adbfabea","e92e35aad3","47be3bf473","17e8ea7d3d","2a90e6ae34","e508ebc11d","e67453e2eb","692ad9902e","4aecd6eb76","bcbbdb6f8f","97a0c9d554","e456930a00","6efb433f6d","8ad0284ca5","2f87a0779f","11b7187921","6c3eff6a04","59023751e0","881341cd52","be5a382fb2","769d6ed5b7","628e0c9989","cc4736a487","a8b6fd3905","61ac2e25e9","d3ffedfd00","279a2943b3","1a20d2a46f","4a366a0ddb","3c86dc2a27","fd4f4f1829","8b8bae7f52","81723544a0","d60093a663","811441e903","a02353278b","3ecd3bdd29","29344a1371","2cdb2116b4","27956814d9","837488a28c","47c9a89dbb","480ce7d9e0","4b2b440640","0399d1a473","b0e545cdbe","fbc663f82d","5a2ba82fa6","d04de42027","43753a28b7","53112c30ed","980d38b1cb","0cb7bf5377","a63fa0bf19","652d729138","b3e2ae643a","bb47bbd61c","7fe312e6bd","f24973b3a9","5b5c8b0389","30281b2bf3","b08801a39c","d1919fed52","309b0a2dac","0f19dc5ca2","1d59678c4d","bb7f6f589f","2c1bc7ad79","0e48433cb4","e7e24c2c69","708ac5a0f4","18d81778d0","2ad3ad6b32","749d0ff416","f6732160d8","34eea55578","7067278a65","37c85fe34f","b600922061","72ddd1a90c","3b331d8b78","e95cd4792d","0450ad94bc","73056960e9","ff15558a73","6b14eb8fc6","a5f84f6454","fa32594da1","af0201af9e","71057c758d","9262af9af5","e776a55ad9","55a580cf87","f9441c68db","d77a8b84db","18e31d8ddb","932e0e07f3","115cd973f7","4cba054a4c","2ec74f7671","4d371c2036","c7ba9f9667","d33a5f2ca7","35ea89b1a1","5fa621a89a","2220599170","0e8b3d4662","574c207423","649a90cb50","080930f99a","cfb9470ca4","75b1ae2801","35dc09f0b4","c8128b6625","06b2c854b2","af48bdd2db","a8b0d47451","f0b7cdbe4f","8d4b3a711f","5989e772cf","1262a509c2","df5afd6c15","07213e29f4","d5db25a015","3abf2e9659","ecfd45a536","612a0060dd","3fc1e4a2b3","02074d1c9a","5878487b09","4471aef8f5","e7442cef3f","b1ac380909","54dd6bbf95","204c19131b","63222d5855","efb3b7fa23","2c7b219820","32d89b9954","ff81edf37f","1c452d0ee9","9d7bcf4be8","37b268b190","ecc59a4e17","6f260904d9","4251c5ede3","e644cfd844","09e91f3ef2","599c3c83e4","2e5c120e39","bbcf150257","9238e2a797","4fc6297aec","48cd059af4","f06cbf95fa","29e727a7f9","71d6304673","07106ff0ea","bffc788dea","9babbe2a6f","085851556b","f0b20b0520","7d765ebca1","2f3f33894a","571fdf3f5d","1b0c63c013","537486b795","80b3493ebd","863b052fd3","6b8dd19db8","5bc62e1897","e38051804f","6c84aab718","9774b9749f","cf2fbc51d5","efa0ac8365","2082845bbe","6d268fe38a","8c437d17b6","e5d6390062","599e88ac33","2f4fe0428b","e48ad8a3cc","734f1c58e0","c48252040c","8f7d793688","ac2d9cd6bf","5520496db9","c8544a1665","5d8bb2a96e","d33e2868d0","0f9856a5d6","692c0f65f2","9401613602","5520496db9","c8544a1665","5d8bb2a96e","d33e2868d0","0f9856a5d6","692c0f65f2","9401613602","b9d3181b89","c925588307","2e65a5f9f6","215ea67302","d63fb4e5ca","0d37735cc5","c948202b4e","33eaee052e","82685492f8","b06ec78a12","4612145500","f39b4b5b6d","2888723465","efa6e3f46b","5ea52e8d20","4cbc2c0605","3a90f257e2","b246707b39","140f506677","eb076d40e4","13a21f377e","8352bccb00","17f67e0c0a","65056659d0","9d89a60d87","245220a001","7d8236d3bd","83fc454be3","a5688d36c0","5dcbdb4482","75c43acd39","3233dbaa76","3ebc201c17","6594e4d38b","b254ec32a5","d1ddafd979","3021f567e8","a421515411","fb8e10abfd","79bb0a31fd","e8934f3d0e","9e3389b4ab","a9526de57a","78b1d9848d","32d9b907e6","044f8c90d3","ad6acb3e9d","0674b64eaf","af4dd575e4","e9c64139dd","a79e6df134","85a5589ac8","4d7c23fb1e","59e07d6069","d923267382","d74047f184","72398bc3bf","b637b0bac9","ed4753b2bf","866911a37d","e9c0cd71bc","666eae6c6a","8d820fc80c","f04f01c903","6b4cd9c2f0","6881b0a1d1","541f2abb4f","c55df2a2e8","c46432ad55","5282111eb6","3f0dfcd5b4","452ccbf1f2","9ffb8a3959","feb911e28e","8906d9a49a","1e0b5bdbf4","4648c5f784","f8437d5d31","71aec1e624","999d6b9944","0472035535","f2b1aa055e","cbefca1942","2c99132925","f8437d5d31","71aec1e624","999d6b9944","0472035535","f2b1aa055e","cbefca1942","2c99132925","ecd886360a","7485a83ea5","381d0bcacc","347d249fc8","1bb7332184","57779bfc52","b85bafa5fe","5e9690213a","6e8e813565","1c6424c57b","045ee34343","287057183f","c6ee33ba36","b04e417920","d452b2a6be","bed5907418","c7c6352e9a","6907a5de39","504a21fea7","29ea9c90f8","037454e5d4","ed76c72ef9","832cf41237","b076c83cb4","10c72813e5","95799c4e5d","549553381a","c86ce44da5","2cf49780a8","9151a57ba8","666b81cd67","c90993edb7","53c3d62a9b","1135129922","8a9727f709","a1b1c48706","04bfd3f317","93b21a1b19","1635e7cb8b","5fc6ee52f9","a5658d8dd2","ff8d79a2a1","72e0c255c7","2f4923940f","21d69d7760","568ecbeb7a","633df87756","e7e6b3b33b","3c2bc57fa8","9fec7c6ff8","f13fe2d273","9daa382ac2","266570a5a0","013c267ec4","9c65aaf425","2288a1a7a0","72ee8d8cc3","de2b8edf27","59d5531b81","2d17b3d565","bfc36da5e0","c76440d933","0f36c7759c","c82e414a2b","47dd315d9a","d66931f8a5","47409cc615","41e53ca8b0","698a8f29c9","9531ee572e","044939a6fb","f96fd670e4","87b2bc6a5a","0736b6795f","9ddcba3c05","a09dbdf512","df168b4f98","16d1379cbb","ad0e7bcd14","c1b4fa2f28","960575f629","44a64d8fa4","de4ecd533f","70b75f2280","4a49d27583","b9191ed4d0","e1b88bb36d","c6e31c0643","d16dda51b8","10b02a40cb","b353f81b6b","d454f4f188","7eccefd397","1c5c0ea59e","2162c27f1d","c99b0762f5","ef92035226","4d8676f74d","590249f68f","d1173bd4e3","22b721256d","7d9bfe62d0","7b6924e2be","fda113efd6","c406f441a3","c30d15f9f1","4ad354931a","7190a9fd7e","c1d8af84c2","f694b05dd4","6939bbe0d6","d0aaa16dea","6c5ac94532","b171abd019","b37d927eca","b4e691fb1e","56fd5ded69","f72a502d38","a0442b845e","17333ffbb4","65deab31f1","3198622944","0c36a01d2a","6547b4985f","a2002c2032","00a116dd36","72eabe3761","7e6f4cfebf","224752d705","52758aa324","d7d5966e0e","820c5b2cfe","2511ddad80","bebab053f2","70d31afa0c","df0f0927b6","662bbe88e7","89e5ba36ec","38be7b0fdc","3523dfdd88","e6a002667d","5f30e0e8ea","4758227597","1f0e2e8fc5","840df5a97b","094b27273c","cf8b7f7137","bebab053f2","70d31afa0c","df0f0927b6","662bbe88e7","89e5ba36ec","38be7b0fdc","3523dfdd88","91406d7394","5169cc3c66","79a7947385","443f9c0dbd","959d040cb2","b86be0bdb4","4fa8af778b","707ee9bf4d","41dc4fd8fc","c59661d0a4","9f02285460","022306e12a","8da13cc03d","7361ba48d9","376c141287","aa9f897f07","ca7a101565","9350a18009","d66726c858","bf605b27c2","c85418908c","6c5fd3f1a3","83edd3464a","ecd31b57ee","12a1de6373","8ba5d70c76","edd0d77b67","f1302e7cc5","29eb1e950a","80a6711000","1a352f6e80","e2984d120b","064a405225","b416951d81","6d3cc3d199","24156f58da","b43668bcff","3dc68a6680","7d379c1ba1","185ddc72b4","bbe122310c","eb14f26241","faffe880f9","7aab91ed68","0bcadf1402","241edffc36","a1afca8542","68eb07347d","fefd913b78","66eb05e80d","cacfa10c81","09b61c421d","87bb36900a","5bb595b9e4","4b41bca784","6b8c2e1b77","4664ef71de","b997bf0dfb","2d22a93af9","13576842e4","171fa392c4","34ccedbf27","e1bfa5a592","aa09257f45","f79686dc08","c6aa171072","2f33d897d5","577bc611cc","13e3388309","4f741a1c67","2462e13a85","555e968d7e","9a1095e79a","a37cdae19a","f9dc67797a","fbcd96fe04","0c12dd53e2"]}
```

---
## Exclusive Findings
---
### Non Critical Issues

| No. | Issue |
| --- | --- |
| 1 | [NatSpec comments should be increased in contracts](#NC-01-natspec-comments-should-be-increased-in-contracts)|
| 2 | [Tokens accidentally sent to the contract cannot be recovered](#NC-02-tokens-accidentally-sent-to-the-contract-cannot-be-recovered)|
| 3 | [Use a more recent version of Solidity](#NC-03-use-a-more-recent-version-of-solidity)|
| 4 | [Use underscores for number literals](#NC-04-use-underscores-for-number-literals)|
| 5 | [Omissions in Events](#NC-05-omissions-in-events)|
| 6 | [Empty blocks should be removed or Emit something](#NC-06-empty-blocks-should-be-removed-or-emit-something)|
| 7 | [Follow Solidity naming conventions for internal or private functions](#NC-07-follow-solidity-naming-conventions-for-internal-or-private-functions)|
---
### [NC-01] NatSpec comments should be increased in contracts

---
**Description:**

It is recommended that Solidity contracts are fully annotated using NatSpec for all public interfaces (everything in the ABI). It is clearly stated in the Solidity official documentation. In complex projects such as Defi, the interpretation of all functions and their arguments and returns is important for code readability and auditability. https://docs.soliditylang.org/en/v0.8.21/natspec-format.html

**Lines of Code:** 

- [Comptroller.sol](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Comptroller.sol)
- [TemporalGovernor.sol](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Governance/TemporalGovernor.sol)
- [InterestRateModel.sol](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/IRModels/InterestRateModel.sol)
- [JumpRateModel.sol](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/IRModels/JumpRateModel.sol)
- [WhitePaperInterestRateModel.sol](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/IRModels/WhitePaperInterestRateModel.sol)
- [MErc20.sol](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MErc20.sol)
- [MErc20Delegate.sol](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MErc20Delegate.sol)
- [MErc20Delegator.sol](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MErc20Delegator.sol)
- [MToken.sol](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MToken.sol)
- [MultiRewardDistributor.sol](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol)
- [ChainlinkCompositeOracle.sol](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Oracles/ChainlinkCompositeOracle.sol)
- [ChainlinkOracle.sol](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Oracles/ChainlinkOracle.sol)
- [Unitroller.sol](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Unitroller.sol)
- [WETHRouter.sol](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/router/WETHRouter.sol)

---
### [NC-02] Tokens accidentally sent to the contract cannot be recovered

---
**Description:**

It can't be recovered if the tokens accidentally arrive at the contract address, which has happened to many popular projects, so I recommend adding a recovery code to your critical contracts.

**Recommendation:**

Add this code:
```solidity
/**
	* @notice Sends ERC20 tokens trapped in contract to external address
	* @dev Onlyowner is allowed to make this function call
	* @param account is the receiving address
	* @param externalToken is the token being sent
	* @param amount is the quantity being sent
	* @return boolean value indicating whether the operation succeeded.
	*
*/
	function rescueERC20(address account, address externalToken, uint256 amount) public onlyOwner returns (bool) {
		IERC20(externalToken).transfer(account, amount);
		return true;
	}
}
```


---
### [NC-03] Use a more recent version of Solidity

---
**Description:**

For security, it is best practice to use the latest Solidity version. For the security fix list in the versions: https://github.com/ethereum/solidity/blob/develop/Changelog.md

**Recommendation:**

Old version of Solidity is used , newer version can be used (0.8.21)

**Lines of Code:** 

- [Comptroller.sol](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Comptroller.sol)
- [TemporalGovernor.sol](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Governance/TemporalGovernor.sol)
- [InterestRateModel.sol](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/IRModels/InterestRateModel.sol)
- [JumpRateModel.sol](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/IRModels/JumpRateModel.sol)
- [WhitePaperInterestRateModel.sol](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/IRModels/WhitePaperInterestRateModel.sol)
- [MErc20.sol](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MErc20.sol)
- [MErc20Delegate.sol](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MErc20Delegate.sol)
- [MErc20Delegator.sol](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MErc20Delegator.sol)
- [MToken.sol](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MToken.sol)
- [MultiRewardDistributor.sol](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol)
- [ChainlinkCompositeOracle.sol](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Oracles/ChainlinkCompositeOracle.sol)
- [ChainlinkOracle.sol](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Oracles/ChainlinkOracle.sol)
- [Unitroller.sol](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Unitroller.sol)
- [WETHRouter.sol](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/router/WETHRouter.sol)

---
### [NC-04] Use underscores for number literals

---
**Description:**

For example, `1000` should be written as `1_000`

**Lines of Code:** 

- [WhitePaperInterestRateModel.sol#L20](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/IRModels/WhitePaperInterestRateModel.sol#L20)

---
### [NC-05] Omissions in Events

---
**Description:**

Throughout the codebase, events are generally emitted when sensitive changes are made to the contracts. However, some events are missing important parameters

**Lines of Code:** 

- [Comptroller.sol#L26](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Comptroller.sol#L26)
- [Comptroller.sol#L29](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Comptroller.sol#L29)
- [Comptroller.sol#L32](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Comptroller.sol#L32)
- [Comptroller.sol#L35](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Comptroller.sol#L35)
- [Comptroller.sol#L38](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Comptroller.sol#L38)
- [Comptroller.sol#L47](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Comptroller.sol#L47)
- [Comptroller.sol#L50](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Comptroller.sol#L50)
- [Comptroller.sol#L53](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Comptroller.sol#L53)
- [Comptroller.sol#L56](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Comptroller.sol#L56)
- [Comptroller.sol#L59](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Comptroller.sol#L59)
- [JumpRateModel.sol#L15](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/IRModels/JumpRateModel.sol#L15)
- [WhitePaperInterestRateModel.sol#L15](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/IRModels/WhitePaperInterestRateModel.sol#L15)
- [ChainlinkOracle.sol#L30-L35](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Oracles/ChainlinkOracle.sol#L30-L35)
- [ChainlinkOracle.sol#L38](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Oracles/ChainlinkOracle.sol#L38)
- [Unitroller.sol#L16](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Unitroller.sol#L16)
- [Unitroller.sol#L21](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Unitroller.sol#L21)
- [Unitroller.sol#L26](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Unitroller.sol#L26)
- [Unitroller.sol#L31](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Unitroller.sol#L31)

---
### [NC-06] Empty blocks should be removed or Emit something

---
**Description:**

Code contains empty block

**Recommendation:**

The code should be refactored such that they no longer exist, or the block should do something useful, such as emitting an event or reverting.

**Lines of Code:** 

- [MErc20Delegate.sol#L15-L21](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MErc20Delegate.sol#L15-L21)

---
### [NC-07] Follow Solidity naming conventions for internal or private functions

---
**Description:**

The mixedCase format starting with an underscore (_mixedCase starting with an underscore) is used for internal and private functions and state variables. The mixedCase format without an underscore (mixedCase without an underscore) is used for external functions and events.

**Recommendation:**

Add an underscore to the prefix of the function name.

**Lines of Code:** 

- [MultiRewardDistributor.sol#L827-L832](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L827-L832)
- [MultiRewardDistributor.sol#L865-L871](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L865-L871)
- [MultiRewardDistributor.sol#L912-L918](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L912-L918)
- [MultiRewardDistributor.sol#L976-L979](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L976-L979)
- [MultiRewardDistributor.sol#L1001](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L1001)
- [MultiRewardDistributor.sol#L1041-L1045](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L1041-L1045)
- [MultiRewardDistributor.sol#L1101](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L1101)
- [MultiRewardDistributor.sol#L1147-L1151](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L1147-L1151)
- [MultiRewardDistributor.sol#L1214-L1218](https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L1214-L1218)
