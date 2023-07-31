# Findings Summary

| ID     | Title                                                                | Severity     |
| ------ | -------------------------------------------------------------------- | ------------ |
| [L-01] | The setDirectPrice function is not recommended                       | Low          |
| [L-02] | Collateral token's decimal greater than 18 is not supported          | Low          |
| [L-03] | Cross-chain message execution doesn't check the target chain         | Low          |
| [L-04] | The oracle check is incomplete                                       | Low          |
| [N-01] | expectedDecimals can be zero                                         | Non-Critical |
| [N-02] | ChainlinkCompositeOracle can only multiply and cannot reverse prices | Non-Critical |

# Detailed Findings

# [L-01] The setDirectPrice function is not recommended

## Description

```solidity
    function setDirectPrice(address asset, uint256 price) external onlyAdmin {
        emit PricePosted(asset, prices[asset], price, price);
        prices[asset] = price;
    }

    function getPrice(MToken mToken) internal view returns (uint256 price) {
        EIP20Interface token = EIP20Interface(
            MErc20(address(mToken)).underlying()
        );

        if (prices[address(token)] != 0) {
            price = prices[address(token)];
        } else {
            price = getChainlinkPrice(getFeed(token.symbol()));
        }

        uint256 decimalDelta = uint256(18).sub(uint256(token.decimals()));
        // Ensure that we don't multiply the result by 0
        if (decimalDelta > 0) {
            return price.mul(10 ** decimalDelta);
        } else {
            return price;
        }
    }
```

The protocol allows admin to set collateral prices directly, which can lead to two problems:
1. Centralized risk, admin can change collateral price and rug pull
2. When the collateral price fluctuates sharply, admin cannot update the price in time(single point risk, such as offline), and the stale price may be used for arbitrage

Even stablecoins can encounter issues such as UST death spiral, USDC decoupling, etc.

## Recommendations

Use only oracle prices, add backup oracles

# [L-02] Collateral token's decimal greater than 18 is not supported

## Description

```solidity
    function getPrice(MToken mToken) internal view returns (uint256 price) {
        EIP20Interface token = EIP20Interface(
            MErc20(address(mToken)).underlying()
        );

        if (prices[address(token)] != 0) {
            price = prices[address(token)];
        } else {
            price = getChainlinkPrice(getFeed(token.symbol()));
        }

        uint256 decimalDelta = uint256(18).sub(uint256(token.decimals()));
        // Ensure that we don't multiply the result by 0
        if (decimalDelta > 0) {
            return price.mul(10 ** decimalDelta);
        } else {
            return price;
        }
    }
```

When obtain the oracle price, it is calculated `18 - token.decimals()`. This is not compatible with a decimal greater than 18 for collateral tokens, such as NEAR.

## Recommendations

Use 36 scale instead of 18

# [L-03] Cross-chain message execution doesn't check the target chain

## Description

```solidity
        address intendedRecipient;
        address[] memory targets; /// contracts to call
        uint256[] memory values; /// native token amount to send
        bytes[] memory calldatas; /// calldata to send

        (intendedRecipient, targets, values, calldatas) = abi.decode(
            vm.payload,
            (address, address[], uint256[], bytes[])
        );

        _sanityCheckPayload(targets, values, calldatas);

        // Very important to check to make sure that the VAA we're processing is specifically designed
        // to be sent to this contract
        require(intendedRecipient == address(this), "TemporalGovernor: Incorrect destination");

        // Ensure the emitterAddress of this VAA is a trusted address
        require(
            trustedSenders[vm.emitterChainId].contains(vm.emitterAddress), /// allow multiple per chainid
            "TemporalGovernor: Invalid Emitter Address"
        );
```

During cross-chain message verification, intendedRecipient, the source chain, and the source contract address are checked, but whether the current chain is the target chain is not checked.
Sponsor indicates Moonwell is currently deployed on Moonbeam and Moonriver, if the upgraded contract is deployed on Moonbeam and Base, may occur double-spend attack.

## Recommendations

Check target chainId.

# [L-04] The oracle check is incomplete

## Description

```solidity
    function getChainlinkPrice(
        AggregatorV3Interface feed
    ) internal view returns (uint256) {
        (, int256 answer, , uint256 updatedAt, ) = AggregatorV3Interface(feed)
            .latestRoundData();
        require(answer > 0, "Chainlink price cannot be lower than 0");
        require(updatedAt != 0, "Round is in incompleted state");

        // Chainlink USD-denominated feeds store answers at 8 decimals
        uint256 decimalDelta = uint256(18).sub(feed.decimals());
        // Ensure that we don't multiply the result by 0
        if (decimalDelta > 0) {
            return uint256(answer).mul(10 ** decimalDelta);
        } else {
            return uint256(answer);
        }
    }
```

The oracle check condition is not complete, although it is scanned out by the automated bot, but the modification proposal proposed by the bot is also incomplete, and the oracle price still has a circuit breaker.
When the oracle price drops significantly below the circuit breaker built into the aggregator, the aggregator returns the built-in minimum price instead of the real price, which leads to a malicious user who can take advantage of the inflated price arbitrage.

## Recommendations

Added backup oracle price.

# [N-01] expectedDecimals can be zero

## Description

```solidity
    function getDerivedPrice(
        address baseAddress,
        address multiplierAddress,
        uint8 expectedDecimals
    ) public view returns (uint256) {
        require(
            expectedDecimals > uint8(0) && expectedDecimals <= uint8(18),
            "CLCOracle: Invalid expected decimals"
        );

        int256 scalingFactor = int256(10 ** uint256(expectedDecimals)); /// calculate expected decimals for end quote

        int256 basePrice = getPriceAndScale(baseAddress, expectedDecimals);
        int256 quotePrice = getPriceAndScale(
            multiplierAddress,
            expectedDecimals
        );

        /// both quote and base price should be scaled up to 18 decimals by now if expectedDecimals is 18
        return calculatePrice(basePrice, quotePrice, scalingFactor);
    }
```

ChainlinkCompositeOracle getDerivedPrice function doesn't allow expectedDecimals to be equal to 0.    
When expectedDecimals are 0, `10 ** expectedDecimals = 1`, it doesn't seem to matter allow expectedDecimals to be 0, that is a price number with no decimal.

## Recommendations

Allow expectedDecimals = 0


# [N-02] ChainlinkCompositeOracle can only multiply and cannot reverse prices

## Description

```solidity
    function getDerivedPrice(
        address baseAddress,
        address multiplierAddress,
        uint8 expectedDecimals
    ) public view returns (uint256) {
        require(
            expectedDecimals > uint8(0) && expectedDecimals <= uint8(18),
            "CLCOracle: Invalid expected decimals"
        );

        int256 scalingFactor = int256(10 ** uint256(expectedDecimals)); /// calculate expected decimals for end quote

        int256 basePrice = getPriceAndScale(baseAddress, expectedDecimals);
        int256 quotePrice = getPriceAndScale(
            multiplierAddress,
            expectedDecimals
        );

        /// both quote and base price should be scaled up to 18 decimals by now if expectedDecimals is 18
        return calculatePrice(basePrice, quotePrice, scalingFactor);
    }
```

ChainlinkCompositeOracle can only calculate the composite price of multiplication, such as A/B * B/C = A/C, not support inversion prices, such as A/B / C/B = A/C.
In chainlink, many tokens are quoted in USD or ETH, and few are multiplied to calculate the composite price. For example, A/USD and B/USD can only calculate A/B by division, not multiplication.

## Recommendations

Add support for inversion prices.