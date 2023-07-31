# Vulnerability 1 (low) : Absence of sanity check of amount of tokens transferred in `transferAllowed()` method

## Description and Impact
The smart contract `Comptroller.sol` is vulnerable to a potential logic flaw. The vulnerability lies in the absence of a code logic check on the `transferTokens > 0` variable call in the [`transferAllowed()` function](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Comptroller.sol#L464-L488).

An attacker can use this loophole to invoke the `transferAllowed` method with `transferTokens=0`. This results in triggering an unexpected transfer mechanism that moves no tokens but manipulates the logic of the transfer itself.

## Proof of concept
Assuming an instance of the `Comptroller` contract is deployed at address `ComptrollerAddress`:
```solidity
Comptroller comptroller = Comptroller(ComptrollerAddress);
comptroller.transferAllowed(address(mTokenInstance), address(attacker), address(recipient), 0);
```
The above codepiece shows the `transferAllowed` method being called with `transferTokens=0`. This deceptive action could lead to an unexpected transfer process being triggered where no tokens are actually moved, resulting in erroneous behavior of the contract.

The vulnerability is present in this part of the code:
```solidity
function transferAllowed(address mToken, address src, address dst, uint transferTokens) override external returns (uint) {
    require(!transferGuardianPaused, "transfer is paused");
    // remainder of the method
}
```
In case a validator is not in place to check whether `transferTokens > 0`, it leaves the door wide open for potential exploitation through zero-token transfer mechanism malfunctions. 

## Recommended Mitigation Steps
To resolve this issue, a `require` statement ensuring `transferTokens > 0` should be added at the start of the `transferAllowed` function:
```solidity
function transferAllowed(address mToken, address src, address dst, uint transferTokens) override external returns (uint) {
    require(transferTokens > 0, "Transfer tokens must be greater than zero");
    require(!transferGuardianPaused, "transfer is paused");
    // remainder of the method
}
```
By enforcing this new condition, we can prevent the zero-token transfer issue, thus securing the transfer logic from potential exploitation and making the contract more robust.


# Vulnerability 2 (low) : Absence of validation check of tokens in function `liquidateBorrowAllowed()`

## Description and Impact

In the smart contract `Comptroller.sol`, the function [`liquidateBorrowAllowed()`](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Comptroller.sol#L386-L424) does not validate if the `mTokenCollateral` and `mTokenBorrowed` are references to different tokens. This could potentially result in a logic flaw and produce undesired behaviors when called with the same token for `mTokenCollateral` and `mTokenBorrowed`.

Specifically, an attacker could exploit this vulnerability to manipulate the system's logic in a way that could lead to unexpected and severe consequences, such as compromising the system's financial mechanisms, which could in turn impact the contract's users if these mechanisms are used for pricing or exchange rate references.

## Proof of Concept

Here's a simplistic view on how this could be exploited:

1. The attacker calls `liquidateBorrowAllowed()` and intentionally provides the same token for `mTokenCollateral` and `mTokenBorrowed`. 
2. The system performs checks and computations, assuming that `mTokenCollateral` and `mTokenBorrowed` are different tokens.
3. As these computations are based on flawed assumptions, the outcome could be radically different than what is to be expected in a correct use case. 


## Recommended Mitigation Steps

In order to prevent a potential exploit of this vulnerability, a sanity check should be added inside the `liquidateBorrowAllowed()` function to ensure that `mTokenCollateral` and `mTokenBorrowed` are not referencing the same token. 

This can simply be done by adding a guard clause at the start of the function, as shown below:

```solidity
function liquidateBorrowAllowed(
    address mTokenBorrowed,
    address mTokenCollateral,
    address liquidator,
    address borrower,
    uint repayAmount) override external view returns (uint) {
    
    require(mTokenBorrowed != mTokenCollateral, "Collateral token and Borrowed token cannot be the same");
   
    // Rest of the function implementation
}
```

By adding this check, the function will revert and throw an error if `mTokenCollateral` and `mTokenBorrowed` are the same, thus preventing the system from progressing on a flawed logic.