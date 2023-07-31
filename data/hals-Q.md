# Findings Summary

| ID              | Title                                                                                 | Severity     |
| --------------- | ------------------------------------------------------------------------------------- | ------------ |
| [L-01](#l-01)   | `fastTrackProposalExecution` function is missing `whenPaused` modifier                | Low          |
| [L-02](#l-02)   | Revoking guardian role is dangerous to the governor contract                          | Low          |
| [L-03](#l-03)   | `getChainlinkPrice` will revert if the asset decimals is greater than 18              | Low          |
| [L-04](#l-04)   | Missing min & max value check when updating `closeFactorMantissa`                     | Low          |
| [L-05](#l-05)   | Tokens with same symbols will collide in `ChainlinkOracle`                            | Low          |
| [L-06](#l-06)   | Market functions will always revert once `block.timestamp` reaches `type(uint32).max` | Low          |
| [L-07](#l-07)   | Liquidations can be done without incentivizing the liquidator                         | Low          |
| [NC-01](#nc-01) | There's no check on the min & max `proposalDelay` when setting it                     | Non Critical |

# Low

## [L-01] `fastTrackProposalExecution` function is missing `whenPaused` modifier <a id="l-01" ></a>

## Details

- In `TemporalGovernor` contract: as per the comments of the `fastTrackProposalExecution` function; this function is meant to be called by the owner of the contract when it's **paused** and in cases of emergencies.
- So in cases of emergencies: the owner (which is the guardian as well) can pause the contract so that proposals can't be executed by anyone (proposals can be executed if it has been queued and passed the time delay).
- And the intention of `fastTrackProposalExecution` function is to enable the owner to execute any urgent proposal during these times.
- But the `fastTrackProposalExecution` function is missing the `whenPaused` modifier; so it can be executed by the owner anytime outside emergency cases.

## Impact

- So the owner can execute any proposal without needing to queue it anytime when there's no emergency cases.
- **note:** I contacted the sponsor regarding this issue, and he confirmed it.

## Proof of Concept

- [Line 261-268](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Governance/TemporalGovernor.sol#L261-L268)

  ```solidity
  File: src/core/Governance/TemporalGovernor.sol
  Line 261-268:
     /// @notice Allow the guardian to process a VAA when the
    /// Temporal Governor is paused this is only for use during
    /// periods of emergency when the governance on moonbeam is
    /// compromised and we need to stop additional proposals from going through.
    /// @param VAA The signed Verified Action Approval to process
    function fastTrackProposalExecution(bytes memory VAA) external onlyOwner {
        _executeProposal(VAA, true); /// override timestamp checks and execute
    }
  ```

## Tools Used

Manual Testing.

## Recommended Mitigation Steps

Add `whenPaused` modifier to the `fastTrackProposalExecution` function

## [L-02] Revoking guardian role is dangerous to the governor contract <a id="l-02" ></a>

## Details

- In `TemporalGovernor` contract: the guardian role is meant to execute proposals in a fast track without queuing them in cases of emergencies (when the governance on moonbeam is compromised) and to pause/unpause the contract with a governonce proposal.
- But this role can be revoked (intended by design) either by the guardian himself (which is the owner of the contract as well) or by a governance proposal.
- As it seems that this role is very important in cases of emergencies;but there's no function to reset this role again if revoked!

## Impact

- So the `fastTrackProposalExecution` function will be disabled permanently & **the contract can never be paused** in cases of emergencies.

## Proof of Concept

- [Line 266-268](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Governance/TemporalGovernor.sol#L266-L268)

  ```solidity
  File: src/core/Governance/TemporalGovernor.sol
  Line 266-268:
    function fastTrackProposalExecution(bytes memory VAA) external onlyOwner {
        _executeProposal(VAA, true); /// override timestamp checks and execute
    }
  ```

- [Line 205-221](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Governance/TemporalGovernor.sol#L205-L221)

  ```solidity
  File: src/core/Governance/TemporalGovernor.sol
  Line 205-221:
    function revokeGuardian() external {
        address oldGuardian = owner();
        require(
            msg.sender == oldGuardian || msg.sender == address(this),
            "TemporalGovernor: cannot revoke guardian"
        );

        _transferOwnership(address(0));
        guardianPauseAllowed = false;
        lastPauseTime = 0;

        if (paused()) {
            _unpause();
        }

        emit GuardianRevoked(oldGuardian);
    }
  ```

## Tools Used

Manual Testing.

## Recommended Mitigation Steps

- In `revokeGuardian()`: make sure to transfer ownership to the `TemporalGovernor` contract address so that the owner/guardian role can't be lost.
- Add a setter function to reset the owner (guardian) by a governonance proposal.

## [L-03] `getChainlinkPrice` will revert if the asset decimals is greater than 18 <a id="l-03" ></a>

## Details

In `ChainlinkOracle` contract: `getChainlinkPrice` will revert if the asset decimals is greater than 18.

## Impact

This will prevent markets from using any underlying assets with decimals >18, as their price feed can't be normalized.

## Proof of Concept

- [Line 85](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Oracles/ChainlinkOracle.sol#L85)

```solidity
File:src/core/Oracles/ChainlinkOracle.sol
Line 85: uint256 decimalDelta = uint256(18).sub(uint256(token.decimals())); //!@audit : will revert if token.decimals() >18
```

## Tools Used

Manual Testing.

## Recommended Mitigation Steps

Update `getChainlinkPrice` function to account for tokens with decimals > 18.

## [L-04] Missing min & max value check when updating `closeFactorMantissa` <a id="l-04" ></a>

## Details

- `closeFactorMantissa` is a multiplier used to calculate the maximum repayAmount when liquidating a borrow (the liquidator may not repay more than what is allowed by the closeFactor).
- In `Comptroller` contract : the admin can change the `closeFactorMantissa` without checking if it complies with the `closeFactorMinMantissa` (which is 0.05e18) & `closeFactorMaxMantissa` (which is 0.9e18).

## Impact

- If this value is set accidentally to a value much lower than the `closeFactorMinMantissa` (say 0); then liquidators will not be able to liquidate unhealthy positions (temporarily until the admin resets it again):

  ```solidity
        uint borrowBalance = MToken(mTokenBorrowed).borrowBalanceStored(borrower);
        uint maxClose = mul_ScalarTruncate(Exp({mantissa: closeFactorMantissa}), borrowBalance);//@audit which will equal to zero if closeFactorMantissa=0
        if (repayAmount > maxClose) { //@audit : this statement will always be true in this case
            return uint(Error.TOO_MUCH_REPAY);
        }
  ```

- And if this value is set to a value much higher than the `closeFactorMaxMantissa` (say 10e18); then liquidators can repay the full borrow;and this will be dangerous to the market stability (temporarily until the admin resets it again):

  ```solidity
        uint borrowBalance = MToken(mTokenBorrowed).borrowBalanceStored(borrower);
        uint maxClose = mul_ScalarTruncate(Exp({mantissa: closeFactorMantissa}), borrowBalance);//@audit which will equal to a very large value if closeFactorMantissa is set to 10e18
        if (repayAmount > maxClose) { //@audit : so this condition can be bypassed
            return uint(Error.TOO_MUCH_REPAY);
        }
  ```

## Proof of Concept

- [\_setCloseFactor function](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Comptroller.sol#L689-L698)

  ```solidity
  File: src/core/Comptroller.sol
  Line 689-698:
    function _setCloseFactor(uint newCloseFactorMantissa) external returns (uint) {
        // Check caller is admin
    	require(msg.sender == admin, "only admin can set close factor");

        uint oldCloseFactorMantissa = closeFactorMantissa;
        closeFactorMantissa = newCloseFactorMantissa;
        emit NewCloseFactor(oldCloseFactorMantissa, closeFactorMantissa);

        return uint(Error.NO_ERROR);
    }
  ```

- [In liquidateBorrowAllowed function](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Comptroller.sol#L417-L421)

  ```solidity
  File: src/core/Comptroller.sol
  Line 417-421:
        uint borrowBalance = MToken(mTokenBorrowed).borrowBalanceStored(borrower);
        uint maxClose = mul_ScalarTruncate(Exp({mantissa: closeFactorMantissa}), borrowBalance);
        if (repayAmount > maxClose) {
            return uint(Error.TOO_MUCH_REPAY);
        }
  ```

## Tools Used

Manual Testing.

## Recommended Mitigation Steps

In `_setCloseFactor` function: check if `newCloseFactorMantissa` is within the max & min values before assigng it to the `closeFactorMantissa`.

## [L-05] Tokens with same symbols will collide in `ChainlinkOracle` <a id="l-05" ></a>

## Details

- In `ChainlinkOracle` contract : asset tokens are saved in feeds mapping as `feeds[keccak256(abi.encodePacked(symbol))]=AggregatorV3Interface`.
- So if there's two tokens with the same symbol (or two mTokens with the same symbol); only one of them can be saved and used.

## Impact

- This will limit the number of asset tokens supported by the markets.
- Also, this can lead to returning the same price for the two tokens sharing the same symbol when `getUnderlyingPrice` or `getFeed` is called.

## Proof of Concept

- [setFeed function](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Oracles/ChainlinkOracle.sol#L144-L153)

  ```solidity
  File:src/core/Oracles/ChainlinkOracle.sol
  Line 144-153:
    function setFeed(string calldata symbol, address feed) external onlyAdmin {
        require(
            feed != address(0) && feed != address(this),
            "invalid feed address"
        );
        emit FeedSet(feed, symbol);
        feeds[keccak256(abi.encodePacked(symbol))] = AggregatorV3Interface(
            feed
        );
    }
  ```

- [Line 82](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Oracles/ChainlinkOracle.sol#L82)

  ```solidity
  File:src/core/Oracles/ChainlinkOracle.sol
  Line 82: price = getChainlinkPrice(getFeed(token.symbol()));
  ```

- [getFeed function](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Oracles/ChainlinkOracle.sol#L158-L162)

  ```solidity
  File:src/core/Oracles/ChainlinkOracle.sol
  Line 158-162:
    function getFeed(
        string memory symbol
    ) public view returns (AggregatorV3Interface) {
        return feeds[keccak256(abi.encodePacked(symbol))];
    }
  ```

## Tools Used

Manual Testing.

## Recommended Mitigation Steps

Modify feeds mapping to save token address insted of token symbol; this will require updating functions that uses token symbols to use addresses instead accordingly.

## [L-06] Market functions will always revert once `block.timestamp` reaches `type(uint32).max`<a id="l-06" ></a>

## Details

The cause of the problem is caused by `calculateNewIndex` function:

- In `MultiRewardDistributor` contract : `calculateNewIndex` function is used to calculate the global reward indicies for markets for the sake of rewards distribution management.
- The function calculates the `tokenAccrued` based on the `deltaTimestamps` and the market emissionsPerSecond of rewards tokens:
  where `deltaTimestamps` is the difference between `supplyGlobalTimestamp` and the current `block.timestamp`.

  ```solidity
        uint32 blockTimestamp = safe32(
            block.timestamp,
            "block timestamp exceeds 32 bits"
        );
        uint256 deltaTimestamps = sub_(
            blockTimestamp,
            uint256(_currentTimestamp)
        );
  ```

  as can be seen; the current `block.timestamp` is downcasted to unit32 by using `safe32` function:

  ```solidity
    function safe32(
        uint n,
        string memory errorMessage
    ) internal pure returns (uint32) {
        require(n < 2 ** 32, errorMessage);
        return uint32(n);
    }
  ```

  then subtracted from the `emissionConfig.config.borrowGlobalTimestamp` which is `_currentTimestamp` input.

- The problem arises when the current `block.timestamp` reaches `type(uint32).max` as the calculation of `blockTimestamp` will always revert .

- Even if this underflow is to happen in the very far future;but risks must be avoided.

## Impact

- `calculateNewIndex` function will always revert once `block.timestamp` hits `type(uint32).max`; so this will cause the other functions using it to revert as well.
- And as can be noticed; almost most of the functions in the `Comptroller` and `MultiRewardDistributor` are using this function indirectly to update accrued rewards, so the market will stop being functional and users funds will be stuck (can't redeem/borrow/transfer/repay/...).

## Proof of Concept

In `src/core/MultiRewardDistributor/MultiRewardDistributor.sol`:

- [calculateNewIndex function](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L912-L968)

```solidity
  function calculateNewIndex(
        uint256 _emissionsPerSecond,
        uint32 _currentTimestamp,
        uint224 _currentIndex,
        uint256 _rewardEndTime,
        uint256 _denominator
    ) internal view returns (IndexUpdate memory) {
        uint32 blockTimestamp = safe32(
            block.timestamp,
            "block timestamp exceeds 32 bits"
        );
        uint256 deltaTimestamps = sub_(
            blockTimestamp,
            uint256(_currentTimestamp)
        );

        // If our current block timestamp is newer than our emission end time, we need to halt
        // reward emissions by stinting the growth of the global index, but importantly not
        // the global timestamp. Should not be gte because the equivalent case makes a
        // 0 deltaTimestamp which doesn't accrue the last bit of rewards properly.
        if (blockTimestamp > _rewardEndTime) {
            // If our current index timestamp is less than our end time it means this
            // is the first time the endTime threshold has been breached, and we have
            // some left over rewards to accrue, so clamp deltaTimestamps to the whatever
            // window of rewards still remains.
            if (_currentTimestamp < _rewardEndTime) {
                deltaTimestamps = sub_(_rewardEndTime, _currentTimestamp);
            } else {
                // Otherwise just set deltaTimestamps to 0 to ensure that we short circuit
                // in the next step
                deltaTimestamps = 0;
            }
        }

        // Short circuit to update the timestamp but *not* the index if there's nothing
        // to calculate
        if (deltaTimestamps == 0 || _emissionsPerSecond == 0) {
            return
                IndexUpdate({
                    newIndex: _currentIndex,
                    newTimestamp: blockTimestamp
                });
        }

        // At this point we know we have to calculate a new index, so do so
        uint256 tokenAccrued = mul_(deltaTimestamps, _emissionsPerSecond);
        Double memory ratio = _denominator > 0
            ? fraction(tokenAccrued, _denominator)
            : Double({mantissa: 0});

        uint224 newIndex = safe224(
            add_(Double({mantissa: _currentIndex}), ratio).mantissa,
            "new index exceeds 224 bits"
        );

        return IndexUpdate({newIndex: newIndex, newTimestamp: blockTimestamp});
    }
```

- [Line 285](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L285)

  ```solidity
  IndexUpdate memory supplyUpdate = calculateNewIndex(
  ```

- [Line 294](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L294)

  ```solidity
  IndexUpdate memory borrowUpdate = calculateNewIndex(
  ```

- [Line 1013](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L1013)

  ```solidity
  IndexUpdate memory supplyUpdate = calculateNewIndex(
  ```

- [Line 1116](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L1116)

  ```solidity
  IndexUpdate memory borrowIndexUpdate = calculateNewIndex(
  ```

## Tools Used

Manual Testing.

## Recommended Mitigation Steps

Avoid downcasting `block.timestamp` to `uint32`.

## [L-07] Liquidations can be done without incentivizing liquidators <a id="l-07" ></a>

## Details

- Liquidators are playing a vital role in preserving the health of the protocol markets by seizing/liquidating unhealthy positions (when the user collateral value / borrowed underlying asset value ratio drops below the collateral factor).
- And liquidators are incentivized by giving them a discount on the collateral asset in return of the liqudated borrows amount (repayAmount);calculated based on `liquidationIncentiveMantissa`.
- But in `Comptroller` contract: there's no initial value set for the `liquidationIncentiveMantissa`.
- The admin can set the `liquidationIncentiveMantissa` by `_setLiquidationIncentive`; but there's no check that liquidations has begun before setting a value for this factor.

## Impact

- So liquidators can liquidate unhealthy positions without being paid with mTokens in return,and borrowers mTokens balances will not be reduced as well.
- Borrowers with unhealthy positions will be the winners in this deal as they will preserve their mTokens,while liquidators will be losing their underlying tokens without getting mTokens in return as incentives.

## Proof of Concept

- [liquidateCalculateSeizeTokens function/Line 649-653](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Comptroller.sol#L649-L653)

```solidity
File:src/core/Comptroller.sol
Line 649-653 :
        numerator = mul_(Exp({mantissa: liquidationIncentiveMantissa}), Exp({mantissa: priceBorrowedMantissa}));
        denominator = mul_(Exp({mantissa: priceCollateralMantissa}), Exp({mantissa: exchangeRateMantissa}));
        ratio = div_(numerator, denominator);

        seizeTokens = mul_ScalarTruncate(ratio, actualRepayAmount);

```

## Tools Used

Manual Testing.

## Recommended Mitigation Steps

In `Comptroller` contract: initialize `liquidationIncentiveMantissa` in the constructor:

```diff
-  constructor() {
+  constructor(uint _newLiquidationIncentiveMantissa) {
        admin = msg.sender;
+       liquidationIncentiveMantissa=_newLiquidationIncentiveMantissa;
    }
```

#

# Non Critical

## [NC-01] There's no check on the min & max `proposalDelay` when setting it <a id="nc-01" ></a>

## Details

- In `TemporalGovernor` contract: there's no check on the value of `proposalDelay` (which is the amount of time a proposal must wait before being processed) when setting it.
- As there's no function to update this value once set in the consructor; setting it to a wrong (very low or very high) value can harm the governor by affecting the proposal execution time.

## Proof of Concept

[Line 68](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Governance/TemporalGovernor.sol#L68)

```solidity
File:src/core/Governance/TemporalGovernor.sol
Line 68: proposalDelay = _proposalDelay;
```

## Tools Used

Manual Testing.

## Recommended Mitigation Steps

Bound `proposalDelay` with a minimum and maximum values and check for it in the constructor before assignment.
