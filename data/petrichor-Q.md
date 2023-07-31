# low

| no  | Issue   | Instance |
|------|---------|----------|
|[L-01]|Missing Event for initialize|3|
|[L-02]| Gas griefing/theft is possible on unsafe external call|1|
|[L-03]|Use of abi.encodePacked with dynamic types inside keccak256|3|
|[L-04]|Consider using OpenZeppelin's SafeCast library to prevent unexpected overflows when casting from various type int/uint values|2|
|[L-05]| Missing checks for address(0x0) when assigning values to address state variables|1|
|[L-06]|Missing checks for approve()’s return status|1|
|[L-07]| Signature use at deadlines should be allowed|1|



## [L-01] Missing Event for initialize

```solidity
88    function initialize(
```    
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L88


```solidity
23  function initialize(address underlying_,
```    
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20.sol#L23

```solidity
26  function initialize(ComptrollerInterface comptroller_,
```    
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MToken.sol#L26



## [L-02] Gas griefing/theft is possible on unsafe external call

return data (bool success,) has to be stored due to EVM architecture, if in a usage like below, 'out' and 'outsize' values are given (0,0) . Thus, this storage disappears and may come from external contracts a possible Gas griefing/theft problem is avoided

```solidity
59        (bool success, ) = payable(recipient).call{
```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/router/WETHRouter.sol#L59



## [L-03] Use of abi.encodePacked with dynamic types inside keccak256

abi.encodePacked should not be used with dynamic types when passing the result to a hash function such as keccak256. Use abi.encode instead, which will pad items to 32 bytes, to prevent any hash collisions

```solidity
46         nativeToken = keccak256(abi.encodePacked(_nativeToken));

150        feeds[keccak256(abi.encodePacked(symbol))] = AggregatorV3Interface(

161        return feeds[keccak256(abi.encodePacked(symbol))];
```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkOracle.sol#L46


https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkOracle.sol#L150

https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkOracle.sol#L161


## [L-04] Consider using OpenZeppelin's SafeCast library to prevent unexpected overflows when casting from various type int/uint values


```solidity
113        int256 scalingFactor = int256(10 ** uint256(expectedDecimals)); /// calculate expected decimals for end quote

145        int256 scalingFactor = int256(10 ** uint256(expectedDecimals * 2)); /// calculate expected decimals for end quote

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkCompositeOracle.sol#L113


https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkCompositeOracle.sol#L145



## [L-05] Missing checks for address(0x0) when assigning values to address state variables

```solidity
52        admin = admin_;
```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20Delegator.sol#L52

## [L-06] Missing checks for approve()’s return status

Some tokens, such as Tether (USDT) return false rather than reverting if the approval fails. Use OpenZeppelin’s safeApprove(), which reverts if there’s a failure, instead

```solidity
26        _weth.approve(address(_mToken), type(uint256).max);
```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20Delegator.sol#L26



## [L‑07] Signature use at deadlines should be allowed

According to EIP-2612, signatures used on exactly the deadline timestamp are supposed to be allowed. While the signature may or may not be used for the exact EIP-2612 use case (transfer approvals), for consistency's sake, all deadlines should follow this semantic. If the timestamp is an expiration rather than a deadline, consider whether it makes more sense to include the expiration timestamp as a valid timestamp, as is done for deadlines.


```solidity
409  require(
            _endTime > block.timestamp + 1,
            "The _endTime parameter must be in the future!"
        );
```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L409-412
