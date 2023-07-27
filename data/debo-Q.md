## [L-01] SWC-123 Requirement violation.

## Description
```txt
The inheritance values are the wrong way around.
```

```url
https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Unitroller.sol#L11
```

## POC
Vulnerable Code
```sol
contract Unitroller is UnitrollerAdminStorage, ComptrollerErrorReporter {
```

## Remediation
```sol
//swap the inherritance around.
change: 
contract Unitroller is UnitrollerAdminStorage, ComptrollerErrorReporter {
to:
contract Unitroller is ComptrollerErrorReporter, UnitrollerAdminStorage {
```