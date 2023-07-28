# Description

Calls inside a loop might lead to a denial-of-service attack. In function "getHypotheticalAccountLiquidityInternal" discovered there is a for loop on variable "i" that iterates up to the assets.length. If this integer is
evaluated at large numbers, this can cause a DoS.

## Code location

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Comptroller.sol#L563

## Recommendation
It is recommended to set the max length to which a for loop can iterate,
and avoid usage of nested loops. If possible, use pull over push strategy
for external calls.

