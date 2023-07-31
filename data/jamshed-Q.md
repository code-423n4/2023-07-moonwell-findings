## [N-01] Pragma version^0.8.17 version too recent to be trusted.

`0.8.17 (2022-09-08)`
>Unexpected bugs can be reported in recent versions;
Risks related to recent releases
Risks of complex code generation changes
Risks of new language features
Risks of known bugs

>Use a non-legacy and more battle-tested version
Use 0.8.10


## [N-02] Showing the actual values of numbers in NatSpec comments makes checking and reading code easier
`uint224 public constant initialIndexConstant = 1e36;`
https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L63

## [N-03] Use constants for numbers
==In several locations in the code, numbers like 1e36, 100e18, 1e27 are used...==
https://github.com/code-423n4/2021-05-yield-findings/issues/3#issuecomment-852039791

## [N-04] Use SMTChecker
The highest tier of smart contract behavior assurance is formal mathematical verification. All assertions that are made are guaranteed to be true across all inputs → The quality of your asserts is the quality of your verification.

https://twitter.com/0xOwenThurm/status/1614359896350425088?t=dbG9gHFigBX85Rv29lOjIQ&s=19

## [N-05] For modern and more readable code; update import usages

Solidity code is also cleaner in another way that might not be noticeable: the struct Point. We were importing it previously with global import but not using it. The Point struct polluted the source code with an unnecessary object we were not using because we did not need it.
This was breaking the rule of modularity and modular programming: only import what you need Specific imports with curly braces allow us to apply this rule better.
*Recommendation*
`import {contract1 , contract2} from "filename.sol";`
https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Comptroller.sol#L4-L9

https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Unitroller.sol#L4C1-L5C35

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MErc20Delegate.sol#L4

https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/IRModels/WhitePaperInterestRateModel.sol#L4-L5

## [N-06] Assembly Codes Specific – Should Have Comments
Since this is a low level language that is more difficult to parse by readers, include extensive documentation, comments on the rationale behind its use, clearly explaining what each assembly instruction does.

This will make it easier for users to trust the code, for reviewers to validate the code, and for developers to build on or update the code.

Note that using Assembly removes several important security features of 
Solidity, which can make the code more insecure and more error-prone.

https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Unitroller.sol#L140C7-L147C10

https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MErc20Delegator.sol#L501-L509C10

 ##  [N-07] Inconsistent Solidity Versions
 `pragma solidity 0.8.17;`
 https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L2
 
 `pragma solidity ^0.8.0;`
 https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Governance/TemporalGovernor.sol#L2
 
 ## [N-08] Floating pragma
Description
Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.
https://swcregistry.io/docs/SWC-103
```
// SPDX-License-Identifier: BSD-3-Clause
pragma solidity 0.8.17;
```
https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L1-L2C24

```
//SPDX-License-Identifier: GPL-3.0-or-later
pragma solidity ^0.8.0;
```
https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/test/proposals/mips/mip00.sol#L1C1-L2C24