## L-01
### Reference
https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/IRModels/JumpRateModel.sol#L53
https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/IRModels/JumpRateModel.sol#L54
https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/IRModels/JumpRateModel.sol#L55

### description
(a * multiplier / (b * multiplier) is simply a/b and will still return a decimal/fraction which can cause loss of some data.</description>

### POC
```solidity
    /**
     * @notice Construct an interest rate model
     * @param baseRatePerYear The approximate target base APR, as a mantissa (scaled by 1e18)
     * @param multiplierPerYear The rate of increase in interest rate wrt utilization (scaled by 1e18)
     * @param jumpMultiplierPerYear The multiplierPerTimestamp after hitting a specified utilization point
     * @param kink_ The utilization point at which the jump multiplier is applied
     */
     // @audit-issue L-01 a*multiplier / (b*multiplier) is simply a/b and will still return a decimal if it is not maintained and can cause loss of some data. 
     // @recommendation Use buffer multiplier to give option for decimals, like adding 1e3 to allow for integers up to 3 dp.
    constructor(uint baseRatePerYear, uint multiplierPerYear, uint jumpMultiplierPerYear, uint kink_) {
        baseRatePerTimestamp = baseRatePerYear.mul(1e18).div(timestampsPerYear).div(1e18);
        multiplierPerTimestamp = multiplierPerYear.mul(1e18).div(timestampsPerYear).div(1e18);
        jumpMultiplierPerTimestamp = jumpMultiplierPerYear.mul(1e18).div(timestampsPerYear).div(1e18);
        kink = kink_;

        emit NewInterestParams(baseRatePerTimestamp, multiplierPerTimestamp, jumpMultiplierPerTimestamp, kink);
    }
```

### Recommendations
 - Use buffer multiplier to give option for decimals, like adding 1e6 to allow for numbers up to 6 dp.

## L-02
### Reference
https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/router/WETHRouter.sol#L65

### description
Unused receive function which allows the contract to receive native tokens but with no functionality could cause unexpected results

### POC
```solidity
        // These funds could come from transferred native tokens allowed by the receive function or withdrawn weth or both
        // @recommendation Specify the exact amount needed to be transferred
        (bool success, ) = payable(recipient).call{
            value: address(this).balance
        }("");
        require(success, "WETHRouter: ETH transfer failed");
    }

    // @audit-issue l-02 Unused receive function which allow contract to receive native tokens but with no functionality
    // @recommendation It is advised to add some functionality or remove the function to reject token transfers to improve trust in your token contracts.
    receive() external payable {}
}

```
### Recommendations
  - It is advised to add some functionality or remove the function to reject token transfers to improve trust in your contracts.

## L-03
### Reference
https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Comptroller.sol#L1040
https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Comptroller.sol#L1048

### description
Unbounded for-loop could cause DOS

### POC
```solidity
        for (uint i = 0; i < mTokens.length; i++) {

            // Safety check that the supplied mTokens are active/listed
            MToken mToken = mTokens[i];
            require(markets[address(mToken)].isListed, "market must be listed");

            // Disburse supply side
            if (suppliers == true) {
                rewardDistributor.updateMarketSupplyIndex(mToken);
                // @audit-issue L-03 unbounded loop could cause DOS 
                // @recommendation limit the length of the array to avoid gas griefing and DOS
                for (uint holderIndex = 0; holderIndex < holders.length; holderIndex++) {
                    rewardDistributor.disburseSupplierRewards(mToken, holders[holderIndex], true);
                }
            }

            // Disburse borrow side
            if (borrowers == true) {
                rewardDistributor.updateMarketBorrowIndex(mToken);
                // @recommendation limit the length of the array to avoid gas griefing and DOS
                for (uint holderIndex = 0; holderIndex < holders.length; holderIndex++) {
```
### Recommendations
  - Limit the number of iterations in the for-loops 
  - Limit the number of iterations with functions making external calls


##L-04
### Reference
https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MErc20Delegator.sol#L69
https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Comptroller.sol#L675
https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Comptroller.sol#L832
https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Comptroller.sol#L869
https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Comptroller.sol#L1017
https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/router/WETHRouter.sol#L24
https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/router/WETHRouter.sol#L25
https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Oracles/ChainlinkCompositeOracle.sol#L40
https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Oracles/ChainlinkCompositeOracle.sol#L41
https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Oracles/ChainlinkCompositeOracle.sol#L42

### description
No check for zero input address (address(0) or address(0x0)) especially in functions input and constructors

### POC
```solidity
    /**
     * @notice Construct an interest rate model
     * @param baseRatePerYear The approximate target base APR, as a mantissa (scaled by 1e18)
     * @param multiplierPerYear The rate of increase in interest rate wrt utilization (scaled by 1e18)
     * @param jumpMultiplierPerYear The multiplierPerTimestamp after hitting a specified utilization point
     * @param kink_ The utilization point at which the jump multiplier is applied
     */
     // @audit-issue L-01 a*multiplier / (b*multiplier) is simply a/b and will still return a decimal if it is not maintained and can cause loss of some data. 
     // @recommendation Use buffer multiplier to give option for decimals, like adding 1e3 to allow for integers up to 3 dp.
    constructor(uint baseRatePerYear, uint multiplierPerYear, uint jumpMultiplierPerYear, uint kink_) {
        baseRatePerTimestamp = baseRatePerYear.mul(1e18).div(timestampsPerYear).div(1e18);
        multiplierPerTimestamp = multiplierPerYear.mul(1e18).div(timestampsPerYear).div(1e18);
        jumpMultiplierPerTimestamp = jumpMultiplierPerYear.mul(1e18).div(timestampsPerYear).div(1e18);
        kink = kink_;

        emit NewInterestParams(baseRatePerTimestamp, multiplierPerTimestamp, jumpMultiplierPerTimestamp, kink);
    }

    /**
```

### Recommendations
 - Add some checks for these instances


##L-05
### Reference
https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Oracles/ChainlinkCompositeOracle.sol#L209
https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Oracles/ChainlinkCompositeOracle.sol#L212

### description
Division by numbers may result in the result being zer or loss of data in decimal so, due to solidity not supporting decimals.

### POC
```solidity
    function scalePrice(
        int256 price,
        uint8 priceDecimals,
        uint8 expectedDecimals
    ) public pure returns (int256) {
        if (priceDecimals < expectedDecimals) {
            return
                price * (10 ** uint256(expectedDecimals - priceDecimals)).toInt256();
        } else if (priceDecimals > expectedDecimals) {
            // @audit-issue L-05 Division by  numbers may result in the result being zer or loss of data in decimalso, due to solidity not supporting decimals. 
            // @recommendation Consider requiring a minimum amount for the numerator or add buffer multiplier to save data in decimal places
            return
                price / (10 ** uint256(priceDecimals - expectedDecimals)).toInt256();
        }

        /// if priceDecimals == expectedDecimals, return price without any changes

        return price;
    }
```

### Recommendations
 - Consider requiring a minimum amount for the numerator to avoid loss of decimals
 - add buffer multiplier to avoid loss data in decimal places, use minimum of 1e3 to keep numeric data up to 3 dp