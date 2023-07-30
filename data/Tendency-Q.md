# [L-01] Comptroller.sol#allMarkets: an unbounded loop on array can lead to DoS

Comptroller::allMarkets, is an array where there are just pushes. No upper bound, no pop.

As this array can grow quite large, the transaction’s gas cost could exceed the block gas limit and make it impossible to call this function at all here:

```solidity
    function _addMarketInternal(address mToken) internal {
        for (uint i = 0; i < allMarkets.length; i ++) {
            require(allMarkets[i] != MToken(mToken), "market already added");
        }
        allMarkets.push(MToken(mToken));
    }
```

Consider introducing a reasonable upper limit based on block gas limits

# [L-02] Allow borrowCap to be filled fully
Here the condition should be ’<=’, not ’<’ to allow filling the cap fully

In Comptroller::borrowAllowed :
```solidity
             #########

        if (borrowCap != 0) {
            uint totalBorrows = MToken(mToken).totalBorrows();
            uint nextTotalBorrows = add_(totalBorrows, borrowAmount);
            require(nextTotalBorrows < borrowCap, "market borrow cap reached"); // <--@audit Here
        }
           
                  #########

```

should be:
```solidity
require(nextTotalBorrows <= borrowCap, "market borrow cap reached");
```

# [L-03] Requirements Violation

It is possible to set close factor mantissa less than closeFactorMinMantissa and greater than closeFactorMaxMantissa as the method Comptroller::setCloseFactor does not limit the input parameters.

I recommend adding a check for the newCloseFactorMantissa parameter, restrict the input to be within the set limits

# [L-04] Possible Race Condition

Contract MToken implements a standard approve function for the managing of the allowance. Changing an allowance with this method brings the risk that someone may use both the old and the new allowance via unfortunate transaction ordering. It can lead to user fund loss.

I recommend Implementing decreaseAllowance and increaseAllowance functions to mitigate the risk.


# [N-01] Reliance on the fact that NO_ERROR = 0
In several occasions it’s relied upon that the error value NO_ERROR is equivalent to 0.

No problems detected based on this yet, but however in most locations there is an explicit check for NO_ERROR and comparing with 0 allows for possible future mistakes (especially if the enums would change).

Recommend replacing 0 with the appropriate version of …NO_ERROR

