## QA Summary<a name="QA Summary">

### Low Risk Issues
| |Issue|Contexts|
|-|:-|:-:|
| [LOW&#x2011;1](#low1-contract-will-stop-functioning-in-the-year-2106) | Contract will stop functioning in the year 2106 | 1 |
| [LOW&#x2011;2](#low2-large-transfers-may-not-work-with-some-erc20-tokens) | Large transfers may not work with some `ERC20` tokens | 9 |
| [LOW&#x2011;3](#low3-minting-tokens-to-the-zero-address-should-be-avoided) | Minting tokens to the zero address should be avoided | 1 |
| [LOW&#x2011;4](#low13-use-safecast-to-safely-downcast-variables) | Use SafeCast to safely downcast variables | 5 |
| [LOW&#x2011;5](#low15-wrong-statement-msgsender--0x0-address) | Wrong statement `msg.sender == 0x0 address` | 5 |

Total: 21 contexts over 5 issues

### Non-critical Issues
| |Issue|Contexts|
|-|:-|:-:|
| [NC&#x2011;1](#nc1-avoid-variable-names-that-can-shade) | Avoid variable names that can shade | 1 |
| [NC&#x2011;2](#nc2-no-need-for--true-or--false-checks) | No need for `== true` or `== false` checks | 7 |
| [NC&#x2011;3](#nc3-blocktimestamp-is-already-used-when-emitting-events-no-need-to-input-timestamp) | `block.timestamp` is already used when emitting events, no need to input timestamp | 2 |
| [NC&#x2011;4](#nc4-empty-bytes-check-is-missing) | Empty `bytes` check is missing | 13 |
| [NC&#x2011;5](#nc12-function-names-should-differ-to-make-the-code-more-readable) | Function names should differ to make the code more readable | 8 |
| [NC&#x2011;6](#nc13-function-must-not-be-longer-than-50-lines) | Function must not be longer than 50 lines | 15 |
| [NC&#x2011;7](#nc14-some-functions-contain-the-same-exact-logic) | Some functions contain the same exact logic | 2 |
| [NC&#x2011;8](#nc17-generate-perfect-code-headers-for-better-solidity-code-layout-and-readability) | Generate perfect code headers for better solidity code layout and readability | 1 |
| [NC&#x2011;9](#nc25-internal-functions-not-called-by-the-contract-should-be-removed) | `internal` functions not called by the contract should be removed | 1 |
| [NC&#x2011;10](#nc10-redundant-cast) | Redundant Cast | 2 |
| [NC&#x2011;11](#nc11-use-named-function-calls) | Use named function calls | 1 |
| [NC&#x2011;12](#nc12-use-smtchecker) | Use SMTChecker | 1 |
| [NC&#x2011;13](#nc13-use-time-units-directly) | Use time units directly | 1 |
| [NC&#x2011;14](#nc14-zero-as-a-function-argument-should-have-a-descriptive-meaning) | Zero as a function argument should have a descriptive meaning | 67 |

Total: 122 contexts over 14 issues

## Low Risk Issues

### <a href="#qa-summary">[LOW&#x2011;1]</a><a name="LOW&#x2011;1"> Contract will stop functioning in the year 2106

Limiting the timestamp variable to fit in a `uint32` will cause the call to start reverting in year 2106 for the following calls.

#### <ins>Proof Of Concept</ins>


```solidity
File: MultiRewardDistributor.sol

919: uint32 blockTimestamp = safe32(

```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L919



### <a href="#qa-summary">[LOW&#x2011;2]</a><a name="LOW&#x2011;2"> Large transfers may not work with some `ERC20` tokens

Some `IERC20` implementations (e.g `UNI`, `COMP`) may fail if the valued transferred is larger than `uint96`. [Source](https://github.com/d-xo/weird-erc20#revert-on-large-approvals--transfers)

#### <ins>Proof Of Concept</ins>

<details>

```solidity
File: Comptroller.sol

965: token.transfer(admin, token.balanceOf(address(this)));
967: token.transfer(admin, _amount);

```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Comptroller.sol#L965

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Comptroller.sol#L967



```solidity
File: MErc20.sol

152: token.transfer(admin, balance);

```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MErc20.sol#L152

```solidity
File: MultiRewardDistributor.sol

478: token.safeTransfer(
483: token.safeTransfer(
1237: token.safeTransfer(

```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L478

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L483

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L1237



```solidity
File: MultiRewardDistributor.sol

1237: token.safeTransfer(_user, _amount);

```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L1237

```solidity
File: WETHRouter.sol

36: IERC20(address(mToken)).safeTransfer(

```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/router/WETHRouter.sol#L36

```solidity
File: WETHRouter.sol

46: IERC20(address(mToken)).safeTransferFrom(

```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/router/WETHRouter.sol#L46



</details>



### <a href="#qa-summary">[LOW&#x2011;3]</a><a name="LOW&#x2011;3"> Minting tokens to the zero address should be avoided

The core function `mint` is used by users to mint an option position by providing token1 as collateral and borrowing the max amount of liquidity. `Address(0)` check is missing in this function, which is triggered to mint the tokens to the `recipient` address. Consider applying a check in the function to ensure tokens aren't minted to the zero address.

#### <ins>Proof Of Concept</ins>


<details>

```solidity
File: WETHRouter.sol


function mint(address recipient) external payable {
        weth.deposit{value: msg.value}();

        require(mToken.mint(msg.value) == 0, "WETHRouter: mint failed");

        IERC20(address(mToken)).safeTransfer(
            recipient,
            mToken.balanceOf(address(this))
        );
    }

```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/router/WETHRouter.sol#L31



</details>


### <a href="#qa-summary">[LOW&#x2011;4]</a><a name="LOW&#x2011;4"> Use SafeCast to safely downcast variables

Downcasting `int`/`uint`s in Solidity can be unsafe due to the potential for data loss and unintended behavior. When downcasting a larger integer type to a smaller one (e.g., `uint256` to `uint128`), the value may exceed the range of the target type, leading to truncation and loss of significant digits. This data loss can result in unexpected state changes, incorrect calculations, or other contract vulnerabilities, ultimately compromising the contracts functionality and reliability. To prevent these risks, developers should carefully consider the range of values their variables may hold and ensure that proper checks are in place to prevent out-of-range values before performing downcasting. Also consider using OZ SafeCast functionality.

#### <ins>Proof Of Concept</ins>

<details>

```solidity
File: TemporalGovernor.sol

284: lastPauseTime = block.timestamp.toUint248();

```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Governance/TemporalGovernor.sol#L284

```solidity
File: TemporalGovernor.sol

339: queuedTransactions[vm.hash].queueTime = block.timestamp.toUint248();
366: queuedTransactions[vm.hash].queueTime = block.timestamp.toUint248();

```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Governance/TemporalGovernor.sol#L339

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Governance/TemporalGovernor.sol#L366

```solidity
File: ChainlinkCompositeOracle.sol

109: expectedDecimals > uint8(0) && expectedDecimals <= uint8(18),
140: expectedDecimals > uint8(0) && expectedDecimals <= uint8(18),

```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Oracles/ChainlinkCompositeOracle.sol#L109

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Oracles/ChainlinkCompositeOracle.sol#L140

</details>



### <a href="#qa-summary">[LOW&#x2011;5]</a><a name="LOW&#x2011;5"> Wrong statement `msg.sender == 0x0 address`

The following statement is wrongly written. As how it is now, when the statement is triggered it checks if `msg.sender` is address(0) which can never happen.

#### <ins>Proof Of Concept</ins>


<details>

```solidity
File: MToken.sol

1170: if (msg.sender != pendingAdmin || msg.sender == address(0)) {

```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MToken.sol#L1170

```solidity
File: Unitroller.sol

111: if (msg.sender != pendingAdmin || msg.sender == address(0)) {

```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Unitroller.sol#L111

</details>




## Non Critical Issues

### <a href="#qa-summary">[NC&#x2011;1]</a><a name="NC&#x2011;1"> Avoid variable names that can shade

With global variable names in the form of `call{value: value }` , argument name similarities can shade and negatively affect code readability.

#### <ins>Proof Of Concept</ins>


```solidity
File: TemporalGovernor.sol

400: (bool success, bytes memory returnData) = target.call{value: value}(
```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Governance/TemporalGovernor.sol#L400






### <a href="#qa-summary">[NC&#x2011;2]</a><a name="NC&#x2011;2"> No need for `== true` or `== false` checks

There is no need to verify that `== true` or `== false` when the variable checked upon is a boolean as well.

#### <ins>Proof Of Concept</ins>


<details>

```solidity
File: Comptroller.sol

129: if (marketToJoin.accountMembership[borrower] == true) {

```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Comptroller.sol#L129

```solidity
File: Comptroller.sol

914: require(msg.sender == admin || state == true, "only admin can unpause");

```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Comptroller.sol#L914

```solidity
File: Comptroller.sol

924: require(msg.sender == admin || state == true, "only admin can unpause");

```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Comptroller.sol#L924

```solidity
File: Comptroller.sol

933: require(msg.sender == admin || state == true, "only admin can unpause");

```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Comptroller.sol#L933

```solidity
File: Comptroller.sol

942: require(msg.sender == admin || state == true, "only admin can unpause");

```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Comptroller.sol#L942

```solidity
File: Comptroller.sol

1038: if (suppliers == true) {
1046: if (borrowers == true) {

```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Comptroller.sol#L1038

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Comptroller.sol#L1046





</details>


#### <ins>Recommended Mitigation Steps</ins>

Instead simply check for `variable` or `!variable`


### <a href="#qa-summary">[NC&#x2011;3]</a><a name="NC&#x2011;3"> block.timestamp is already used when emitting events, no need to input timestamp

#### <ins>Proof Of Concept</ins>

```solidity
File: TemporalGovernor.sol

197: emit GuardianPauseGranted(block.timestamp);

```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Governance/TemporalGovernor.sol#L197

```solidity
File: TemporalGovernor.sol

258: emit PermissionlessUnpaused(block.timestamp);

```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Governance/TemporalGovernor.sol#L258



### <a href="#qa-summary">[NC&#x2011;4]</a><a name="NC&#x2011;4"> Empty `bytes` check is missing

To avoid mistakenly accepting empty `bytes` as a valid value, consider to check for empty `bytes`. This helps ensure that empty `bytes` are not treated as valid inputs, preventing potential issues.

#### <ins>Proof Of Concept</ins>


<details>

```solidity
File: MErc20.sol

62: function mintWithPermit(
        uint mintAmount,
        uint deadline,
        uint8 v, bytes32 r, bytes32 s
    ) override external returns (uint) {
```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MErc20.sol#L62

```solidity
File: MErc20Delegate.sol

21: function _becomeImplementation(bytes memory data) virtual override public {
```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MErc20Delegate.sol#L21

```solidity
File: MErc20Delegator.sol

61: function _setImplementation(address implementation_, bool allowResign, bytes memory becomeImplementationData) override public {
```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MErc20Delegator.sol#L61

```solidity
File: MErc20Delegator.sol

98: function mintWithPermit(
        uint mintAmount,
        uint deadline,
        uint8 v, bytes32 r, bytes32 s
    ) override external returns (uint) {
```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MErc20Delegator.sol#L98

```solidity
File: MErc20Delegator.sol

455: function delegateTo(address callee, bytes memory data) internal returns (bytes memory) {
```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MErc20Delegator.sol#L455

```solidity
File: MErc20Delegator.sol

471: function delegateToImplementation(bytes memory data) public returns (bytes memory) {
```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MErc20Delegator.sol#L471

```solidity
File: MErc20Delegator.sol

482: function delegateToViewImplementation(bytes memory data) public view returns (bytes memory) {
```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MErc20Delegator.sol#L482

```solidity
File: TemporalGovernor.sol

87: function isTrustedSender(
        uint16 chainId,
        bytes32 addr
    ) public view returns (bool) {
```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Governance/TemporalGovernor.sol#L87

```solidity
File: TemporalGovernor.sol

229: function queueProposal(bytes memory VAA) external whenNotPaused {
```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Governance/TemporalGovernor.sol#L229

```solidity
File: TemporalGovernor.sol

237: function executeProposal(bytes memory VAA) public whenNotPaused {
```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Governance/TemporalGovernor.sol#L237

```solidity
File: TemporalGovernor.sol

266: function fastTrackProposalExecution(bytes memory VAA) external onlyOwner {
```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Governance/TemporalGovernor.sol#L266

```solidity
File: TemporalGovernor.sol

295: function _queueProposal(bytes memory VAA) private {
```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Governance/TemporalGovernor.sol#L295

```solidity
File: TemporalGovernor.sol

344: function _executeProposal(bytes memory VAA, bool overrideDelay) private {
```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Governance/TemporalGovernor.sol#L344



</details>


### <a href="#qa-summary">[NC&#x2011;5]</a><a name="NC&#x2011;5"> Function names should differ to make the code more readable

In Solidity, while function overriding allows for functions with the same name to coexist, it is advisable to avoid this practice to enhance code readability and maintainability. Having multiple functions with the same name, even with different parameters or in inherited contracts, can cause confusion and increase the likelihood of errors during development, testing, and debugging. Using distinct and descriptive function names not only clarifies the purpose and behavior of each function, but also helps prevent unintended function calls or incorrect overriding. By adopting a clear and consistent naming convention, developers can create more comprehensible and maintainable smart contracts.

#### <ins>Proof Of Concept</ins>


```solidity
File: Comptroller.sol

998: function claimReward
```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Comptroller.sol#L998

```solidity
File: Comptroller.sol

1006: function claimReward
```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Comptroller.sol#L1006

```solidity
File: Comptroller.sol

1015: function claimReward
```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Comptroller.sol#L1015

```solidity
File: Comptroller.sol

1028: function claimReward
```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Comptroller.sol#L1028

```solidity
File: TemporalGovernor.sol

86: function isTrustedSender
```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Governance/TemporalGovernor.sol#L86

```solidity
File: TemporalGovernor.sol

96: function isTrustedSender
```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Governance/TemporalGovernor.sol#L96

```solidity
File: MultiRewardDistributor.sol

231: function getOutstandingRewardsForUser
```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L231

```solidity
File: MultiRewardDistributor.sol

256: function getOutstandingRewardsForUser
```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L256






### <a href="#qa-summary">[NC&#x2011;6]</a><a name="NC&#x2011;6"> Function must not be longer than 50 lines

#### <ins>Proof Of Concept</ins>


<details>

```solidity
File: Comptroller.sol

563: function getHypotheticalAccountLiquidityInternal(

```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Comptroller.sol#L563

```solidity
File: MToken.sol

68: function transferTokens(

```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MToken.sol#L68

```solidity
File: MToken.sol

385: function accrueInterest(

```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MToken.sol#L385

```solidity
File: MToken.sol

615: function redeemFresh(

```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MToken.sol#L615

```solidity
File: MToken.sol

750: function borrowFresh(

```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MToken.sol#L750

```solidity
File: MToken.sol

865: function repayBorrowFresh(

```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MToken.sol#L865

```solidity
File: MToken.sol

968: function liquidateBorrowFresh(

```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MToken.sol#L968

```solidity
File: MToken.sol

1074: function seizeInternal(

```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MToken.sol#L1074

```solidity
File: TemporalGovernor.sol

344: function _executeProposal(

```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Governance/TemporalGovernor.sol#L344

```solidity
File: MultiRewardDistributor.sol

231: function getOutstandingRewardsForUser(
256: function getOutstandingRewardsForUser(

```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L231

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L256



```solidity
File: MultiRewardDistributor.sol

382: function _addEmissionConfig(

```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L382

```solidity
File: MultiRewardDistributor.sol

912: function calculateNewIndex(

```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L912

```solidity
File: MultiRewardDistributor.sol

1041: function disburseSupplierRewardsInternal(

```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L1041

```solidity
File: MultiRewardDistributor.sol

1147: function disburseBorrowerRewardsInternal(

```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L1147



</details>




### <a href="#qa-summary">[NC&#x2011;7]</a><a name="NC&#x2011;7"> Some functions contain the same exact logic

These functions might be a problem if the logic changes before the contract is deployed, as the developer must remember to syncronize the logic between all the function instances.

Consider using a single function instead of duplicating the code, for example by using a `library`, or through inheritance.

#### <ins>Proof Of Concept</ins>


```solidity
File: JumpRateModel.sol

104: function getSupplyRate(uint cash, uint borrows, uint reserves, uint reserveFactorMantissa) override public view returns (uint) {
        uint oneMinusReserveFactor = uint(1e18).sub(reserveFactorMantissa);
        uint borrowRate = getBorrowRate(cash, borrows, reserves);
        uint rateToPool = borrowRate.mul(oneMinusReserveFactor).div(1e18);
        return utilizationRate(cash, borrows, reserves).mul(rateToPool).div(1e18);
    }
}

```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/IRModels/JumpRateModel.sol#L104

```solidity
File: WhitePaperInterestRateModel.sol

80: function getSupplyRate(uint cash, uint borrows, uint reserves, uint reserveFactorMantissa) override public view returns (uint) {
        uint oneMinusReserveFactor = uint(1e18).sub(reserveFactorMantissa);
        uint borrowRate = getBorrowRate(cash, borrows, reserves);
        uint rateToPool = borrowRate.mul(oneMinusReserveFactor).div(1e18);
        return utilizationRate(cash, borrows, reserves).mul(rateToPool).div(1e18);
    }
}

```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/IRModels/WhitePaperInterestRateModel.sol#L80



### <a href="#qa-summary">[NC&#x2011;8]</a><a name="NC&#x2011;8"> Generate perfect code headers for better solidity code layout and readability

It is recommended to use pre-made headers for Solidity code layout and readability:
https://github.com/transmissions11/headers

```solidity
/*//////////////////////////////////////////////////////////////
                           TESTING 123
//////////////////////////////////////////////////////////////*/
```


#### <ins>Proof Of Concept</ins>


```solidity
File: TemporalGovernor.sol

11: TemporalGovernor.sol

```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Governance/TemporalGovernor.sol#L11



### <a href="#qa-summary">[NC&#x2011;9]</a><a name="NC&#x2011;9"> `internal` functions not called by the contract should be removed

#### <ins>Proof Of Concept</ins>


```solidity
File: MErc20.sol

function getCashPrior() virtual override internal view returns (uint) {
```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MErc20.sol#L171



### <a href="#qa-summary">[NC&#x2011;10]</a><a name="NC&#x2011;10"> Redundant Cast

The type of the variable is the same as the type to which the variable is being cast

#### <ins>Proof Of Concept</ins>


```solidity
File: MultiRewardDistributor.sol

1107: MToken(_mToken)
```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L1107

```solidity
File: ChainlinkOracle.sol

100: AggregatorV3Interface(feed)
```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Oracles/ChainlinkOracle.sol#L100


### <a href="#qa-summary">[NC&#x2011;11]</a><a name="NC&#x2011;11"> Use named function calls

Code base has an extensive use of named function calls, but it somehow missed one instance where this would be appropriate.

It should use named function calls on function call, as such:
```solidity
    library.exampleFunction{value: _data.amount.value}({
        _id: _data.id,
        _amount: _data.amount.value,
        _token: _data.token,
        _example: "",
        _metadata: _data.metadata
    });
```

#### <ins>Proof Of Concept</ins>


```solidity
File: TemporalGovernor.sol

400: (bool success, bytes memory returnData) = target.call{value: value}(
                data
            );

```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Governance/TemporalGovernor.sol#L400



### <a href="#qa-summary">[NC&#x2011;12]</a><a name="NC&#x2011;12"> Use SMTChecker

The highest tier of smart contract behavior assurance is formal mathematical verification. All assertions that are made are guaranteed to be true across all inputs → The quality of your asserts is the quality of your verification

https://twitter.com/0xOwenThurm/status/1614359896350425088?t=dbG9gHFigBX85Rv29lOjIQ&s=19




### <a href="#qa-summary">[NC&#x2011;13]</a><a name="NC&#x2011;13"> Use time units directly

The values `hours`,`days`,`weeks` etc, can be used directly instead of using the calculated value as it is not needed.

#### <ins>Proof Of Concept</ins>

```solidity
File: JumpRateModel.sol

20: uint public constant timestampsPerYear = 60 * 60 * 24 * 365;
```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/IRModels/JumpRateModel.sol#L20



### <a href="#qa-summary">[NC&#x2011;14]</a><a name="NC&#x2011;14"> Zero as a function argument should have a descriptive meaning

Consider using descriptive constants or an enum instead of passing zero directly on function calls, as that might be error-prone, to fully describe the caller's intention.

#### <ins>Proof Of Concept</ins>


<details>

```solidity
File: Comptroller.sol

344: (Error err, , uint shortfall) = getHypotheticalAccountLiquidityInternal(borrower, MToken(mToken), 0, borrowAmount);

```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Comptroller.sol#L344

```solidity
File: Comptroller.sol

517: (Error err, uint liquidity, uint shortfall) = getHypotheticalAccountLiquidityInternal(account, MToken(address(0)), 0, 0);

```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Comptroller.sol#L517

```solidity
File: Comptroller.sol

529: return getHypotheticalAccountLiquidityInternal(account, MToken(address(0)), 0, 0);

```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Comptroller.sol#L529

```solidity
File: Comptroller.sol

580: return (Error.SNAPSHOT_ERROR, 0, 0);
588: return (Error.PRICE_ERROR, 0, 0);
617: return (Error.NO_ERROR, 0, vars.sumBorrowPlusEffects - vars.sumCollateral);

```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Comptroller.sol#L580

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Comptroller.sol#L588

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Comptroller.sol#L617


```solidity
File: MErc20.sol

199: returndatacopy(0, 0, 32)
234: returndatacopy(0, 0, 32)
203: revert(0, 0)
238: revert(0, 0)

```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MErc20.sol#L199

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MErc20.sol#L234

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MErc20.sol#L203

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MErc20.sol#L238

```solidity
File: MToken.sol

213: return (uint(Error.MATH_ERROR), 0, 0, 0);
218: return (uint(Error.MATH_ERROR), 0, 0, 0);
```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MToken.sol#L213

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MToken.sol#L218


```solidity
File: MToken.sol

594: return redeemFresh(payable(msg.sender), 0, redeemAmount);

```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/MToken.sol#L594


```solidity
File: Unitroller.sol

142: returndatacopy(free_mem_ptr, 0, returndatasize())

```

https://github.com/code-423n4/2023-07-moonwell/tree/main/src/core/Unitroller.sol#L142


</details>




