
# `borrowRatePerTimestamp` is not fresh and does not include interests accrued
https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MToken.sol#L236
`borrowRatePerTimestamp` returns the borrowing rate, but the interests accrued are not included.
```solidity
function borrowRatePerTimestamp() override external view returns (uint) {
        return interestRateModel.getBorrowRate(getCashPrior(), totalBorrows, totalReserves);
    }
```
Impact
Any smart contract relying on the value of borrowRatePerTimestamp() to perform some logic - such as decide whether to borrow from VToken based on the returned value - will be impacted.

For instance, imagine a protocol XYZ having a function in their contract that calls borrowRatePerTimestamp() and have a conditional call to mToken.borrow() if the borrowing rate is less than N. The call to borrowRatePerTimestamp() may return a value M < N, but the actual borrowing rate processed in borrow() can be P > N due to interest accrual.

## Recommended Mitigation Steps
Include some interest accrual logic in borrowRatePerTimestamp() - note that it cannot be a simple call to accrueInterest as borrowRatePerBlock() is a view function.


# Check the timestamp of the latest answer
[chainlink](https://docs.chain.link/data-feeds#check-the-timestamp-of-the-latest-answer) states that:
> During periods of low volatility, the heartbeat triggers updates to the latest answer. Some heartbeats are configured to last several hours, so your application should check the timestamp and verify that the latest answer is recent enough for your application.

Both [`ChainlinkCompositeOracle.getPriceAndDecimals`](https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Oracles/ChainlinkCompositeOracle.sol#L180) and [`ChainlinkOracle.getChainlinkPrice`](https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Oracles/ChainlinkOracle.sol#L97) should check that the reported answer is updated within the heartbeat or within time limits deemed acceptable:
## Recommendation:
The following check could be added:
```solidity
require(block.timestamp - updatedAt < PRICE_ORACLE_STALE_THRESHOLD, "STALE_THRESHOLD: Exceeded");
```