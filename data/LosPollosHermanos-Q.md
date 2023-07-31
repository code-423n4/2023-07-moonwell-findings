## Low 01 - Inconsistent safety checks for MToken's Comptroller in `Comptroller.liquidateBorrowAllowed()`

Comptroller's `siezeAllowed()` makes a check that if `MToken(mTokenCollateral).comptroller() != MToken(mTokenBorrowed).comptroller()` then the function returns an error code. However, `liquidateBorrowAllowed()` which also deals with two mTokens does not make such a check.

Consider adding the code below to `Comptroller.liquidateBorrowAllowed()`:
```solidity
if (MToken(mTokenCollateral).comptroller() != MToken(mTokenBorrowed).comptroller()) {
            return uint(Error.COMPTROLLER_MISMATCH);
        }
```

https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Comptroller.sol#L403-L405

## Low 02 - `Comptroller.siezeAllowed()` checks that `comptroller()` of MTokens match but not that they are the correct address

In `Comptroller.siezeAllowed()`, the collateral and borrow mToken are checked so that they share the same comptroller but there is no guarantee that the given addresss is the same as the deployed comptroller's. This will cause the contract to revert instead of gracefully returning an error.

Consider adding this code to `Comptroller.siezeAllowed()`:
```solidity
if (MToken(mTokenCollateral).comptroller() == address(this)) {
    return uint(Error.COMPTROLLER_MISMATCH);
}
```

https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Comptroller.sol#L450-L452

## Low 03 - `Comptroller._setMarketBorrowCaps()` and `Comptroller._setMarketSupplyCaps()` do not check that mTokens have been added to a listed market

As explained in the title, there is no check that markets aren't listed for given mTokens which allows caps to be set for arbitrary addresses. Although this does not negatively affect the protocol, any such data will be obselete.

Consider adding the following check in the two above functions:
```solidity
require(markets[address(mTokens[i])].isListed, "Market is not listed");
```

https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Comptroller.sol#L807
https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Comptroller.sol#L844

## Low 04 - `Comptroller._rescueFunds()` and `MultiRewardDistributor._rescueFunds()` will revert if `amount` specified is too large

In an emergency, if an admin tries to recover funds and specifies `amount` such that `amount > token.balanceOf(address(this))` and `amount != type(uint256).max` then the function may revert even though the contract has a non-zero token balance.

Consider modifying:
```solidity
if (_amount == type(uint256).max) {
   token.safeTransfer(
       comptroller.admin(),
       token.balanceOf(address(this))
   );
} else {
  token.safeTransfer(comptroller.admin(), _amount);
}
```

to:
```solidity
if (_amount == type(uint256).max || amount > token.balanceOf(address(this))) {
   token.safeTransfer(
       comptroller.admin(),
       token.balanceOf(address(this))
   );
} else {
  token.safeTransfer(comptroller.admin(), _amount);
}
```

https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L477

https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Comptroller.sol#L962

## Low 05 - Decreasing `emissionCap` in `MultiRewardDistributor` may cause certain markets to break invariant

In `MultiRewardDistrubitor._setEmissionCap()`, `emissionCap` can be changed to any arbitrary value. Naturally, if this value is set too low, then it may be that certain markets will have higher emissions than the new cap. This will break the invariant that `marketEmission <= emissionCap` which may be problematic as the emissionCap may have been reduced due to overflows in calculations.

Consider modifying the following code in `MultiRewardDistributor.calculateNewIndex()` from:
```solidity
// Short circuit to update the timestamp but *not* the index if there's nothing
// to calculate
if (deltaTimestamps == 0 || _emissionsPerSecond == 0) {
    return
        IndexUpdate({
            newIndex: _currentIndex,
            newTimestamp: blockTimestamp
        });
}
```
to:
```solidity
if (deltaTimestamps == 0 || _emissionsPerSecond == 0 || _emissionPerSecond > emissionCap) {
    return
        IndexUpdate({
            newIndex: _currentIndex,
            newTimestamp: blockTimestamp
        });
}
```

This will prevent rewards from being accrued until emissionPerSecond has been updated in `MultiRewardDistributor._updateSupplySpeed()` and `MultiRewardDistributor._updateBorrowSpeed()`. However, the above fix may allow the guardian to abuse their power and prevent reward accruals by setting the emissionCap to `0`. In that case, consider adding a minimum `emissionCap`.

https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L512-L520