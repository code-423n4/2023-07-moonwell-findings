## Risk of frontrunning when token allowance is altered

The contract `MToken` implements an `approve` function, but no `decreaseAllowance` and `increaseAllowance` functions.

As a result, when an allowance is increased or decreased, the approved person could take advantage of both the old and the new allowance, as described [here](https://docs.google.com/document/d/1YLPtQxZu1UAvO9cZ1O2RPXBbT0mooh4DYKjA_jp-RLM/edit).

https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MToken.sol

## Mismatch between code and comments

The comments state that

```solidity
 // If our user's index isn't set yet, set to the current global supply index
```

but the code behaves differently. The `userSupplyIndex` is actually set to `initialIndexConstant` and stays 0 if the `_globalSupplyIndex` is less than the `initialIndexConstant`.

```solidity
if (
    userSupplyIndex == 0 && _globalSupplyIndex >= initialIndexConstant
) {
    userSupplyIndex = initialIndexConstant;
}
```

https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L835-L840

## Fallback function in MErc20Delegator.sol does not need to be payable

As the function doesn't accept ether, the `payable` keyword could be left out, making the check for `msg.value` being `0` unnecessary.

```solidity
 require(msg.value == 0,"MErc20Delegator:fallback: cannot send value to fallback");
```

https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20Delegator.sol#L496-L497

## Critical functions don't revert on failure

According to the EIP-20 spec, `The [transfer] function SHOULD throw if the message caller’s account balance does not have enough tokens to spend` and `The [transferFrom] function SHOULD throw unless the _from account has deliberately authorized the sender of the message via some mechanism.` ([source](https://eips.ethereum.org/EIPS/eip-20))

Contrary to that, the `transfer` and `transferFrom` functions in `MToken.sol` only return false, but don't revert on failure.  
This could easily lead to errors for protocols that wish to integrate with Moonwell, if the return value remains unchecked.

https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MToken.sol

## Transfer event logging `address(this)` instead of `address(0)` when minting new tokens

According to the EIP-20 spec, `A token contract which creates new tokens SHOULD trigger a Transfer event with the _from address set to 0x0 when tokens are created` ([source](https://eips.ethereum.org/EIPS/eip-20)), but here the `_from` address of the event is set to the token contract itself.

```solidity
emit Transfer(address(this), minter, vars.mintTokens);
```

https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MToken.sol#556

Similarly, when tokens are burned (redeemFresh function), the Transfer event logs `address(this)` and not the 0 address as the `_to` address.

```solidity
 emit Transfer(redeemer, address(this), vars.redeemTokens);
```

https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MToken.sol#776
