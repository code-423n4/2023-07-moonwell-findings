https://github.com/code-423n4/2023-07-moonwell/blob/8694244ebf607a4ed33c0b74f422019fe8eb8d3e/src/core/MTokenInterfaces.sol#L81

```solidty
uint public protocolSeizeShareMantissa;

   function _setProtocolSeizeShareFresh(uint newProtocolSeizeShareMantissa) internal returns (uint) {

        // Used to store old share for use in the event that is emitted on success
        uint oldProtocolSeizeShareMantissa;

        // Check caller is admin
        if (msg.sender != admin) {
            return fail(Error.UNAUTHORIZED, FailureInfo.SET_PROTOCOL_SEIZE_SHARE_OWNER_CHECK);
        }

        // We fail gracefully unless market's block timestamp equals current block timestamp
        if (accrualBlockTimestamp != getBlockTimestamp()) {
            return fail(Error.MARKET_NOT_FRESH, FailureInfo.SET_PROTOCOL_SEIZE_SHARE_FRESH_CHECK);
        }

        // Track the market's current protocol seize share
        oldProtocolSeizeShareMantissa = protocolSeizeShareMantissa;

        // Set the protocol seize share to newProtocolSeizeShareMantissa
        protocolSeizeShareMantissa = newProtocolSeizeShareMantissa;

        // Emit NewProtocolSeizeShareMantissa(oldProtocolSeizeShareMantissa, newProtocolSeizeShareMantissa)
        emit NewProtocolSeizeShare(oldProtocolSeizeShareMantissa, newProtocolSeizeShareMantissa);

        return uint(Error.NO_ERROR);
    }

```
it seems that  protocolSeizeShareMantissa don't  have default value ,if  admin forget to set protocolSeizeShareMantissa value ,liquidation calculation may have a wrong result