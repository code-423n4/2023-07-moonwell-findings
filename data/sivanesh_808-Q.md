# 1) Floating Pragma

## Description:
Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.

Lock the pragma version and also consider known bugs ([https://github.com/ethereum/solidity/releases](https://github.com/ethereum/solidity/releases)) for the compiler version that is chosen.

## Remediation:
Pragma statements can be allowed to float when a contract is intended for consumption by other developers, as in the case with contracts in a library or EthPM package. Otherwise, the developer would need to manually update the pragma in order to compile locally.

Refer : [https://swcregistry.io/docs/SWC-103/](https://swcregistry.io/docs/SWC-103/)

**1) test/proposals/mips/mip00.sol**

[https://github.com/code-423n4/2023-07-moonwell/blob/main/test/proposals/mips/mip00.sol#L2](https://github.com/code-423n4/2023-07-moonwell/blob/main/test/proposals/mips/mip00.sol#L2)