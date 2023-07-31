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

# [L-04] ERC20 double spend race condition

Due to MToken's inheritance of ERC20’s approve function, it is vulnerable to the ERC20 approve and double spend front running attack. Briefly, an authorized spender could spend both allowances by front running an allowance-changing transaction. 

Consider implementing OpenZeppelin’s decreaseAllowance and increaseAllowance functions to help mitigate this.

# [L-05] Admin Must Receive Reserves

The mToken  administrator can call the _reduceReserves function to withdraw some of the reserves. However, the function enforces the receiver to be the admin
```solidity

        // doTransferOut reverts if anything goes wrong, since we can't be sure if side effects occurred.
        doTransferOut(admin, reduceAmount);
```
 This merges roles that should probably be distinct, particularly when the administrator is replaced with a 
decentralized governance process.

Consider allowing the administrator to choose the recipient in the function call.

# [L-06] 



# [N-01] 