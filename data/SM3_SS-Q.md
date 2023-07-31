
# Low Risk Issues

| Number | Issue | Instances |
|--------|-------|-----------|
|[L-01]| Signature use at deadlines should be allowed | 2 |
|[L-02]| Use of abi.encodePacked with dynamic types inside keccak256 | 3 |
|[L-03]|  Missing checks for address(0x0) when assigning values to address state variables | 1 |
|[L-04]| Consider using OpenZeppelin's SafeCast library to prevent unexpected overflows when casting from various type int/uint values | 2 |
|[L-05]| Missing zero address check in constructor | 4 |
|[L-06]| Unbounded loop | 2 |
|[L-07]| Using zero as a parameter | 12 |
|[L-08]| Array lengths not checked | 1 |
|[L-09]| approve()/safeApprove() may revert if the current approval is not zero | 1 |
|[L-10]| Unsafe ERC20 operation(s) | 8 |
|[L-11]| Multiplication on the result of a division | 1 |


## [L‑01] Signature use at deadlines should be allowed

According to EIP-2612, signatures used on exactly the deadline timestamp are supposed to be allowed. While the signature may or may not be used for the exact EIP-2612 use case (transfer approvals), for consistency's sake, all deadlines should follow this semantic. If the timestamp is an expiration rather than a deadline, consider whether it makes more sense to include the expiration timestamp as a valid timestamp, as is done for deadlines.

```solidity
file:    src/core/MultiRewardDistributor/MultiRewardDistributor.sol
793            require(
            _newEndTime > block.timestamp,
            "_newEndTime MUST be > block.timestamp"
        ); 

409            require(
            _endTime > block.timestamp + 1,
            "The _endTime parameter must be in the future!"
        );
```
https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L793-L796


## [L-02] Use of abi.encodePacked with dynamic types inside keccak256

abi.encodePacked should not be used with dynamic types when passing the result to a hash function such as keccak256. Use abi.encode instead, which will pad items to 32 bytes, to prevent any hash collisions

```solidity
File: src/core/Oracles/ChainlinkOracle.sol
46         nativeToken = keccak256(abi.encodePacked(_nativeToken));

150        feeds[keccak256(abi.encodePacked(symbol))] = AggregatorV3Interface(

161        return feeds[keccak256(abi.encodePacked(symbol))];
```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkOracle.sol#L46




## [L-03] Missing checks for address(0x0) when assigning values to address state variables

```solidity
File: src/core/MErc20Delegator.sol
52        admin = admin_;
```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20Delegator.sol#L52



## [L-04] Consider using OpenZeppelin's SafeCast library to prevent unexpected overflows when casting from various type int/uint values

```solidity
File: src/core/Oracles/ChainlinkCompositeOracle.sol
113        int256 scalingFactor = int256(10 ** uint256(expectedDecimals)); /// calculate expected decimals for end quote

145        int256 scalingFactor = int256(10 ** uint256(expectedDecimals * 2)); /// calculate expected decimals for end quote

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkCompositeOracle.sol#L113


## [L-05] Missing zero address check in constructor

```solidity
file:  src/core/Governance/TemporalGovernor.sol

67     wormholeBridge = IWormhole(wormholeCore);

```
https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Governance/TemporalGovernor.sol#L67

```solidity
file:    src/core/Oracles/ChainlinkCompositeOracle.sol

40           base = baseAddress;

41          multiplier = multiplierAddress;

42          secondMultiplier = secondMultiplierAddress;

```
https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Oracles/ChainlinkCompositeOracle.sol#L40

## [L-06] Unbounded loop

New items are pushed into the following arrays but there is no option to pop them out. Currently, the array can grow indefinitely. E.g. there’s no maximum limit and there’s no functionality to remove array values.

If the array grows too large, calling relevant functions might run out of gas and revert. Calling these functions could result in a DOS condition.

```solidity
file:  src/core/Comptroller.sol

140    accountAssets[borrower].push(mToken);

798     allMarkets.push(MToken(mToken));

```
https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Comptroller.sol#L140


## [L-07] Using zero as a parameter

Taking 0 as a valid argument in Solidity without checks can lead to severe security issues. A historical example is the infamous 0x0 address bug where numerous tokens were lost. This happens because 0 can be interpreted as an uninitialized address, leading to transfers to the 0x0 address, effectively burning tokens. Moreover, 0 as a denominator in division operations would cause a runtime exception. It's also often indicative of a logical error in the caller's code. It's important to always validate input and handle edge cases like 0 appropriately. Use require() statements to enforce conditions and provide clear error messages to facilitate debugging and safer code.

```solidity
file:   src/core/MToken.sol

213     return (uint(Error.MATH_ERROR), 0, 0, 0);

218     return (uint(Error.MATH_ERROR), 0, 0, 0);

594    return redeemFresh(payable(msg.sender), 0, redeemAmount);

```
https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MToken.sol#L213 

```solidity
file:   src/core/Unitroller.sol

142    returndatacopy(free_mem_ptr, 0, returndatasize())

```
https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Unitroller.sol#L142

```solidity
file:  src/core/MErc20Delegator.sol

504   returndatacopy(free_mem_ptr, 0, returndatasize())

```
https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MErc20Delegator.sol#L504 

```solidity
file:   src/core/MErc20.sol

119    returndatacopy(0, 0, 32)

234    returndatacopy(0, 0, 32)

```
https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MErc20.sol#L119

```solidity
file:   src/core/Comptroller.sol
344   (Error err, , uint shortfall) = getHypotheticalAccountLiquidityInternal(borrower, MToken(mToken), 0, borrowAmount);

517    (Error err, uint liquidity, uint shortfall) = getHypotheticalAccountLiquidityInternal(account, MToken(address(0)), 0, 0);

529     return getHypotheticalAccountLiquidityInternal(account, MToken(address(0)), 0, 0);

580     return (Error.SNAPSHOT_ERROR, 0, 0);

588     return (Error.PRICE_ERROR, 0, 0);

617      return (Error.NO_ERROR, 0, vars.sumBorrowPlusEffects - vars.sumCollateral);
```

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Comptroller.sol#L344

## [L‑08] Array lengths not checked

If the length of the arrays are not required to be of the same length, user operations may not be fully executed due to a mismatch in the number of items iterated over, versus the number of items provided in the second array

```solidity
file:   src/core/Governance/TemporalGovernor.sol

164         function unSetTrustedSenders(
        TrustedSender[] calldata _trustedSenders
    ) external 


```
https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Governance/TemporalGovernor.sol#L164-L166


## [L‑09] approve()/safeApprove() may revert if the current approval is not zero

Calling approve() without first calling approve(0) if the current approval is non-zero will revert with some tokens, such as Tether (USDT). While Tether is known to do this, it applies to other tokens as well, which are trying to protect against this attack vector. safeApprove() itself also implements this protection. Always reset the approval to zero before changing it to a new value, or use safeIncreaseAllowance()/safeDecreaseAllowance()

```solidity
file:  src/core/router/WETHRouter.sol

26    _weth.approve(address(_mToken), type(uint256).max);

```
https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/router/WETHRouter.sol#L26


## [L-10] Unsafe ERC20 operation(s)

```solidity
file:    src/core/router/WETHRouter.sol

36            IERC20(address(mToken)).safeTransfer(
            recipient,
            mToken.balanceOf(address(this))
        );

46            IERC20(address(mToken)).safeTransferFrom(
            msg.sender,
            address(this),
            mTokenRedeemAmount
        );
```
https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/router/WETHRouter.sol#L36-L39

```solidity
file:   src/core/Comptroller.sol

965     token.transfer(admin, token.balanceOf(address(this)));  

967      token.transfer(admin, _amount);

```
https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Comptroller.sol#L965
```solidity
file:   src/core/MErc20.sol
 
152    token.transfer(admin, balance);

190     token.transferFrom(from, address(this), amount);

225     token.transfer(to, amount); 

```
https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MErc20.sol#L152

```solidity
file:   src/core/MToken.sol

137    return transferTokens(msg.sender, msg.sender, dst, amount) == uint(Error.NO_ERROR);

```
https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MToken.sol#L137

## [L‑11] Multiplication on the result of a division

Dividing an integer by another integer will often result in loss of precision. When the result is multiplied by another number, the loss of precision is magnified, often to material magnitudes. (X / Z) * Y should be re-written as (X * Y) / Z

```solidity
file:  src/core/Oracles/ChainlinkCompositeOracle.sol

212    price / (10 ** uint256(priceDecimals - expectedDecimals)).toInt256();


```
https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Oracles/ChainlinkCompositeOracle.sol#L212
