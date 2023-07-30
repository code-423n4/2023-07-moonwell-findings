# Table of Contents
| Number | Issues Details                                                                                         | Count |
| :----- | :----------------------------------------------------------------------------------------------------- | :---- |
| [L-1] | Potential Rounding Issues | 1 |
| [L-2] | Extending the Timestamp Limit to Avoid Future Reverting | 1 |
| [L-3] | Handle Unused `receive()` Function to Prevent Ether Lock | 1 |
| [N-1] | Eliminating Redundant Return Statements for Named Return Variables | 1 |
| [N-2] | Prefer `require()` Over `assert()` | 4 |
| [N-3] | Omitting Initialization of `uint` Variables to Zero | 1 |
| [N-4] | Eliminating Redundant Casts | 2 |
| [N-5] | Emitting Events for Significant Parameter Changes | 2 |
| [N-6] | Removing `forge-std` Import | 1 |
| [N-7] | Resolving Open TODOs | 1 |
| [N-8] | Redundancy of Timestamp Input Given Use of block.timestamp in Event Emission | 2 |
| [N-9] | Redundant Boolean Checks like == true or == false are Unnecessary | 1 |
| [N-10] | Utilizing Time Units for Readability of Numeric Values | 2 |
| [N-11] | Redundant `require()`/`revert()` Verifications Should Be Streamlined Into A Modifier Or Function | 2 |
| [N-12] | When it's impossible for them to overflow, such as in for- and while-loops, `++i` and `i++` should be written as `unchecked{++i}` and `unchecked{i++}` respectively for better efficiency | 3 |
| [N-13] | Reduce strings exceeding 32 bytes in `require()` or `revert()` statements result for better efficiency and readability | 29 |
| [N-14] | Employ the `delete` statement instead of `= 0`  | 5 |
| [N-15] | Storing a reference to a storage variable instead of repeatedly retrieving it is more efficient | 7 |

## [L-1]</a><a name="L-1"> Potential Rounding Issues

<details>
<summary><i>There are 1 instances of this issue:</i></summary>

```solidity
File: ChainlinkCompositeOracle.sol
return ((basePrice * priceMultiplier) / scalingFactor).toUint256();
```
</details>



## [L-2]</a><a name="L-2"> Extending the Timestamp Limit to Avoid Future Reverting

It has been observed that limiting the timestamp variable to fit within a `uint32` may lead to a call reverting in the year 2106. To prevent this issue and ensure the continued functionality of the contract beyond that timeframe, it is recommended to extend the timestamp limit.

<details>
<summary><i>There are 1 instances of this issue:</i></summary>

```solidity
File: MultiRewardDistributor.sol
uint32 blockTimestamp = safe32(
```
</details>

## [L-3]</a><a name="L-3"> Handle Unused `receive()` Function to Prevent Ether Lock

<details>
<summary><i>There are 1 instances of this issue:</i></summary>

```solidity
File: WETHRouter.sol
receive() external payable {}
```
</details>



## [N-2]</a><a name="N-2"> Prefer `require()` Over `assert()`

<details>
<summary><i>There are 4 instances of this issue:</i></summary>

```solidity
File: TemporalGovernor.sol
assert(!guardianPauseAllowed);
```

```solidity
File: Comptroller.sol
assert(markets[mToken].accountMembership[borrower]);
```

```solidity
File: TemporalGovernor.sol
assert(!guardianPauseAllowed);
```

```solidity
File: Comptroller.sol
assert(assetIndex < len);
```
</details>



## [N-3]</a><a name="N-3"> Omitting Initialization of `uint` Variables to Zero

Initializing `uint` variables to zero is unnecessary since their default value is already `0`.

<details>
<summary><i>There are 1 instances of this issue:</i></summary>

```solidity
File: MToken.sol
uint startingAllowance = 0;
```
</details>



## [N-4]</a><a name="N-4"> Eliminating Redundant Casts

It appears that there is a redundant cast in the code. The variable is already of the same type as the one it is being cast to, making the cast unnecessary. Removing this redundant cast enhances code readability and eliminates unnecessary operations.

<details>
<summary><i>There are 2 instances of this issue:</i></summary>

```solidity
File: ChainlinkOracle.sol
(, int256 answer, , uint256 updatedAt, ) = AggregatorV3Interface(feed)
            .latestRoundData();
```

```solidity
File: MultiRewardDistributor.sol
uint256 totalBorrows = MToken(_mToken).totalBorrows();
```
</details>



## [N-5]</a><a name="N-5"> Emitting Events for Significant Parameter Changes

It is important to emit events when modifying state variables to enable effective monitoring using off-chain monitoring tools. Emitting events facilitates the tracking of activities related to critical parameter changes.

<details>
<summary><i>There are 2 instances of this issue:</i></summary>

```solidity
File: ChainlinkOracle.sol
function setFeed(string calldata symbol, address feed) external onlyAdmin {
```

```solidity
File: ChainlinkOracle.sol
function setAdmin(address newAdmin) external onlyAdmin {
```
</details>



## [N-6]</a><a name="N-6"> Removing `forge-std` Import

The `forge-std` import is primarily used for logging and debugging purposes during development. However, it should be removed from the codebase when it is not actively utilized for development purposes. Removing the `forge-std` import helps reduce unnecessary dependencies and ensures a cleaner and more concise codebase.

<details>
<summary><i>There are 1 instances of this issue:</i></summary>

```solidity
File: mip00.sol
import "@forge-std/Test.sol";
```
</details>



## [N-7]</a><a name="N-7"> Resolving Open TODOs

An open TODO has been identified in the codebase. It is advisable to address and resolve open TODOs as they can signify programming errors or incomplete tasks that require attention. By eliminating open TODOs, the codebase can be maintained in a more reliable and organized manner, ensuring a higher quality of the software.

<details>
<summary><i>There are 1 instances of this issue:</i></summary>

```solidity
File: mip00.sol
addresses.getAddress("GUARDIAN") /// TODO figure out what the pause guardian is on Base, then replace it
```

```solidity
File: mip00.sol
/// TODO calculate initial exchange rate
```
</details>



## [N-8]</a><a name="N-8"> Redundancy of Timestamp Input Given Use of block.timestamp in Event Emission

<details>
<summary><i>There are 2 instances of this issue:</i></summary>

```solidity
File: TemporalGovernor.sol
PermissionlessUnpaused(block.timestamp);
```

```solidity
File: TemporalGovernor.sol
GuardianPauseGranted(block.timestamp);
```
</details>



## [N-9]</a><a name="N-9"> Redundant Boolean Checks like == true or == false are Unnecessary

When evaluating a variable that is also a boolean, it is redundant to validate it with == true or == false.

<details>
<summary><i>There are 1 instances of this issue:</i></summary>

```solidity
File: Comptroller.sol
require(msg.sender == admin || state == true, "only admin can unpause");
```
</details>



## [N-10]</a><a name="N-10"> Utilizing Time Units for Readability of Numeric Values

When dealing with numeric values associated with time, it is recommended to use time units for improved code readability. Solidity provides predefined units for seconds, minutes, hours, days, and weeks, which should be utilized to enhance clarity and maintain consistency with time-related calculations. For more information on time units, refer to the [Solidity documentation on units and global variables](https://docs.soliditylang.org/en/latest/units-and-global-variables.html#time-units).

<details>
<summary><i>There are 2 instances of this issue:</i></summary>

```solidity
File: JumpRateModel.sol
uint public constant timestampsPerYear = 60 * 60 * 24 * 365;
```
</details>



## [N-11]</a><a name="N-11"> Redundant `require()`/`revert()` Verifications Should Be Streamlined Into A Modifier Or Function

<details>
<summary><i>There are 2 instances of this issue:</i></summary>

```solidity
File: TemporalGovernor.sol
require(valid, reason);
require(valid, reason);
```

```solidity
File: MToken.sol
require(accrueInterest() == uint(Error.NO_ERROR), "accrue interest failed");
require(accrueInterest() == uint(Error.NO_ERROR), "accrue interest failed");
require(accrueInterest() == uint(Error.NO_ERROR), "accrue interest failed");
```
</details>



## [N-12]</a><a name="N-12"> When it's impossible for them to overflow, such as in for- and while-loops, `++i` and `i++` should be written as `unchecked{++i}` and `unchecked{i++}` respectively for better efficiency

<details>
<summary><i>There are 8 instances of this issue:</i></summary>

```solidity
File: MultiRewardDistributor.sol
for (uint256 index = 0; index < configs.length; index++) {
```

```solidity
File: Comptroller.sol
for (uint i = 0; i < len; i++) {
```

```solidity
File: Comptroller.sol
for(uint i = 0; i < numMarkets; i++) {
```
</details>



## [N-13]</a><a name="N-13"> Reduce strings exceeding 32 bytes in `require()` or `revert()` statements result for better efficiency and readability

<details>
<summary><i>There are 29 instances of this issue:</i></summary>

```solidity
File: MToken.sol
require(totalReservesNew <= totalReserves, "reduce reserves unexpected underflow");
```

```solidity
File: MToken.sol
require(vars.mathErr == MathError.NO_ERROR, "MINT_EXCHANGE_CALCULATION_FAILED");
```

```solidity
File: MToken.sol
require(vars.mathErr == MathError.NO_ERROR, "MINT_NEW_TOTAL_SUPPLY_CALCULATION_FAILED");
```

```solidity
File: MToken.sol
require(vars.mathErr == MathError.NO_ERROR, "MINT_NEW_ACCOUNT_BALANCE_CALCULATION_FAILED");
```

```solidity
File: MToken.sol
require(newInterestRateModel.isInterestRateModel(), "marker method returned false");
```

```solidity
File: Comptroller.sol
require(msg.sender == unitroller.admin(), "only unitroller admin can change brains");
```

```solidity
File: ChainlinkCompositeOracle.sol
require(valid, "CLCOracle: Oracle data is invalid");
```

```solidity
File: Comptroller.sol
require(markets[address(mToken)].isListed, "cannot pause a market that is not listed");
```

```solidity
File: Comptroller.sol
require(msg.sender == pauseGuardian || msg.sender == admin, "only pause guardian and admin can pause");
```

```solidity
File: MErc20Delegate.sol
require(msg.sender == admin, "only the admin may call _becomeImplementation");
```

```solidity
File: MToken.sol
require(vars.mathErr == MathError.NO_ERROR, "REPAY_BORROW_NEW_ACCOUNT_BORROW_BALANCE_CALCULATION_FAILED");
```

```solidity
File: MToken.sol
require(vars.mathErr == MathError.NO_ERROR, "REPAY_BORROW_NEW_TOTAL_BALANCE_CALCULATION_FAILED");
```

```solidity
File: Comptroller.sol
require(msg.sender == admin || msg.sender == borrowCapGuardian, "only admin or borrow cap guardian can set borrow caps");
```

```solidity
File: Comptroller.sol
require(msg.sender == pauseGuardian || msg.sender == admin, "only pause guardian and admin can pause");
```

```solidity
File: Comptroller.sol
require(address(rewardDistributor) != address(0), "No reward distributor configured!");
```

```solidity
File: MErc20Delegate.sol
require(msg.sender == admin, "only the admin may call _resignImplementation");
```

```solidity
File: MToken.sol
require(err == MathError.NO_ERROR, "exchangeRateStored: exchangeRateStoredInternal failed");
```

```solidity
File: Comptroller.sol
require(msg.sender == pauseGuardian || msg.sender == admin, "only pause guardian and admin can pause");
```

```solidity
File: MToken.sol
require(totalReservesNew >= totalReserves, "add reserves unexpected overflow");
```

```solidity
File: MToken.sol
require(mErr == MathError.NO_ERROR, "balance could not be calculated");
```

```solidity
File: MToken.sol
require(err == MathError.NO_ERROR, "borrowBalanceStored: borrowBalanceStoredInternal failed");
```

```solidity
File: MToken.sol
require(redeemTokensIn == 0 || redeemAmountIn == 0, "one of redeemTokensIn or redeemAmountIn must be zero");
```

```solidity
File: MToken.sol
require(newComptroller.isComptroller(), "marker method returned false");
```

```solidity
File: Comptroller.sol
require(oErr == 0, "exitMarket: getAccountSnapshot failed");
```

```solidity
File: TemporalGovernor.sol
require(targets.length != 0, "TemporalGovernor: Empty proposal");
```

```solidity
File: Comptroller.sol
require(markets[address(mToken)].isListed, "cannot pause a market that is not listed");
```

```solidity
File: Comptroller.sol
require(msg.sender == pauseGuardian || msg.sender == admin, "only pause guardian and admin can pause");
```

```solidity
File: TemporalGovernor.sol
require(intendedRecipient == address(this), "TemporalGovernor: Incorrect destination");
```

```solidity
File: ChainlinkOracle.sol
require(answer > 0, "Chainlink price cannot be lower than 0");
```

```solidity
File: ChainlinkOracle.sol
require(updatedAt != 0, "Round is in incompleted state");
```

```solidity
File: MErc20Delegator.sol
require(msg.value == 0,"MErc20Delegator:fallback: cannot send value to fallback");
```

```solidity
File: Comptroller.sol
require(msg.sender == admin, "only admin can set close factor");
```

```solidity
File: MToken.sol
require(msg.sender == admin, "only admin may initialize the market");
```

```solidity
File: MToken.sol
require(accrualBlockTimestamp == 0 && borrowIndex == 0, "market may only be initialized once");
```

```solidity
File: MToken.sol
require(initialExchangeRateMantissa > 0, "initial exchange rate must be greater than zero.");
```

```solidity
File: MToken.sol
require(err == uint(Error.NO_ERROR), "setting interest rate model failed");
```

```solidity
File: MErc20.sol
require(msg.sender == admin, "MErc20::sweepToken: only admin can sweep tokens");
```

```solidity
File: MErc20.sol
require(address(token) != underlying, "MErc20::sweepToken: can not sweep underlying token");
```

```solidity
File: WETHRouter.sol
require(success, "WETHRouter: ETH transfer failed");
```
</details>



## [N-14]</a><a name="N-14"> Employ the `delete` statement instead of `= 0` 

<details>
<summary><i>There are 6 instances of this issue:</i></summary>

```solidity
File: TemporalGovernor.sol
lastPauseTime = 0;
```

```solidity
File: Comptroller.sol
newMarket.collateralFactorMantissa = 0;
```

```solidity
File: MultiRewardDistributor.sol
deltaTimestamps = 0;
```

```solidity
File: MToken.sol
uint startingAllowance = 0;
```

```solidity
File: TemporalGovernor.sol
lastPauseTime = 0;
```
</details>



## [N-15]</a><a name="N-15"> Storing a reference to a storage variable instead of repeatedly retrieving it is more efficient

<details>
<summary><i>There are 9 instances of this issue:</i></summary>

```solidity
File: MultiRewardDistributor.sol
MarketEmissionConfig storage emissionConfig = configs[index];
```

```solidity
File: MultiRewardDistributor.sol
markets[index],
```

```solidity
File: MultiRewardDistributor.sol
address(markets[index]),
```

```solidity
File: Comptroller.sol
MToken[] memory assets = accountAssets[account];
```

```solidity
File: TemporalGovernor.sol
trustedSenders[chainId].length()
```

```solidity
File: TemporalGovernor.sol
trustedSendersList[i] = trustedSenders[chainId].at(i);
```

```solidity
File: Comptroller.sol
rewardDistributor.disburseSupplierRewards(mToken, holders[holderIndex], true);
```
</details>



