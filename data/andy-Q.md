### `MultiRewardDistributor::sendReward` can be optmised

Condition [_amount > 0](https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L1235) should be optimised as `_amount == 0` short-circuits it. The type of `_amount` is uint256, hence, `_amount > 0` is not necessary.

```solidity
function sendReward(
address payable _user,
uint256 _amount,
address _rewardToken
) internal nonReentrant returns (uint256) {
// Short circuit if we don't have anything to send out
if (_amount == 0) {
    return _amount;
}

// If pause guardian is active, bypass all token transfers, but still accrue to local tally
if (paused()) {
    return _amount;
}

IERC20 token = IERC20(_rewardToken);

// Get the distributor's current balance
uint256 currentTokenHoldings = token.balanceOf(address(this));

// ANDY
// Only transfer out if we have enough of a balance to cover it (otherwise just accrue without sending)
if (_amount > 0 && _amount <= currentTokenHoldings) {
    // Ensure we use SafeERC20 to revert even if the reward token isn't ERC20 compliant
    token.safeTransfer(_user, _amount);
    return 0;
}
```
