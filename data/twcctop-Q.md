https://github.com/code-423n4/2023-07-moonwell/blob/8694244ebf607a4ed33c0b74f422019fe8eb8d3e/src/core/MToken.sol#L1305-L1308

```solidity
 totalReservesNew = totalReserves + actualAddAmount;

        /* Revert on overflow */
        require(totalReservesNew >= totalReserves, "add reserves unexpected overflow");

```

since actualAddAmount is uint , `totalReservesNew >= totalReserves ` is always satisfied , which seems require unnecessary