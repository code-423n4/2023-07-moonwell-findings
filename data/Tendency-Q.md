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

# [L-06] TemporalGovernor::setTrustedSenders emits a TrustedSenderUpdated event even when the sender wasn't set

TemporalGovernor::setTrustedSenders emits a TrustedSenderUpdated event even when the sender wasn't set

In the for loop, the add method used checks if the trusted sender has previously been set at the chain id, if it has, it returns false.
In such scenarios, the function still emits a TrustedSenderUpdated event

Since the add method returns true when the value is added and false when it is not, I will recommend checking the returns value, and emitting an event only when the returned value is true

https://github.com/OpenZeppelin/openzeppelin-contracts/blob/d6b63a48ba440ad8d551383697db6e5b0ef84137/contracts/utils/structs/EnumerableSet.sol#L65

# [L-07] TemporalGovernor::unSetTrustedSenders emits a TrustedSenderUpdated event even when a trusted sender isn't removed

The remove method used here from OZ EnumerableSet::remove function, checks the index, the value is stored at, if the index equals 0, meaning the value hasn't been previously added to the array, the boolean false is returned.

In such a scenario the function still emits a TrustedSenderUpdated removal event
```solidity
    function unSetTrustedSenders(
        TrustedSender[] calldata _trustedSenders
    ) external {
        require(
            msg.sender == address(this),
            "TemporalGovernor: Only this contract can update trusted senders"
        );

        unchecked {
            for (uint256 i = 0; i < _trustedSenders.length; i++) {
                trustedSenders[_trustedSenders[i].chainId].remove(
                    addressToBytes(_trustedSenders[i].addr)
                );

                emit TrustedSenderUpdated(
                    _trustedSenders[i].chainId,
                    _trustedSenders[i].addr,
                    false /// removed from list
                );
            }
        }
    }
```

I will recommend checking the return value the removal operation and then emitting the event only if the return value equals true


# [N-01] The comment in TemporalGovernor::togglePause is misleading

```solidity
    /// @notice Allow the guardian to pause the contract
    /// removes the guardians ability to call pause again until governance reaaproves them
    /// starts the timer for the permissionless unpause
    /// cannot call this function if guardian is revoked
    function togglePause() external onlyOwner {
        if (paused()) {
            _unpause();
        } else {
            require(
                guardianPauseAllowed,
                "TemporalGovernor: guardian pause not allowed"
            );

            guardianPauseAllowed = false;
            lastPauseTime = block.timestamp.toUint248();
            _pause();
        }

        /// statement for SMT solver
        assert(!guardianPauseAllowed); /// this should be an unreachable state
    }

```

The comments says the function allows the guardian to pause the contract, but this isn't just the case. When the owner interacts with the togglePause function, if the contract is paused, the function unpauses the contract, it only pauses the contract if the above check is false