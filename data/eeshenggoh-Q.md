## Summary
Security Risk in MErc20::sweepToken Function without using OpenZeppelin's onlyAdmin Modifier

During the security analysis of the MErc20::sweepToken function, a potential issue was identified. The function has its own condition for checking admin privileges, instead of utilizing OpenZeppelin's onlyAdmin modifier. Also -in some scenarios - consume most gast than necessary
## Vulnerability Details

`require(msg.sender == admin, "MErc20::sweepToken: only admin can sweep tokens");
`

https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20.sol#L148-L149

The primary difference between transfer and safeTransfer (or its equivalents safeTransferFrom in the context of ERC721 tokens) is in how they handle interactions with smart contracts. The transfer function simply transfers tokens from one address to another without any checks. However, if the recipient is a smart contract, it might not be prepared to receive tokens and could potentially cause them to be locked forever docs.openzeppelin.com.

On the other hand, safeTransfer performs checks to ensure that if the recipient is a smart contract, it is prepared to handle the tokens. Specifically, the recipient contract must implement the IERC721Receiver.onERC721Received function, which is called upon a safe transfer. If this function is not implemented, the transfer will fail, preventing tokens from being locked in the contract

This code is responsible for ensuring the function can only be executed by the admin. However, using this pattern could increase the chance of a mistake, particularly if there are many places in the contract where similar checks are necessary.
## Impact

If this pattern is not implemented correctly in every instance, it could allow unauthorized users to execute administrative functions. This could lead to potential loss of funds or other changes to the state of the smart contract.

## Recommended Mitigation Steps

The recommended course of action is to replace the custom admin checks with OpenZeppelin's onlyAdmin modifier.

```
import "@openzeppelin/contracts/access/AccessControl.sol";

contract MErc20 is AccessControl {
    bytes32 public constant ADMIN_ROLE = keccak256("ADMIN_ROLE");

    constructor () {
        _setupRole(ADMIN_ROLE, msg.sender);
    }

    function sweepToken(EIP20NonStandardInterface token) override external onlyRole(ADMIN_ROLE) {
        // ...
    }
}
```

Implementing OpenZeppelin's Access Control model in this way adds the onlyRole modifier, replacing the need to check msg.sender against the admin address.

OpenZeppelin Advantages:
1) Audited Code: The OpenZeppelin library is thoroughly audited, reducing the possibility of security vulnerabilities.
2) Standardized: OpenZeppelin follows standardized best practices in Solidity, increasing code quality and readability.
3) Gas Efficiency: Custom modifiers and conditions may – in some scenarios – consume more gas than necessary. By using commonly used, optimized libraries like OpenZeppelin, you reduce this risk.

--------------
## Summary

Not using Openzepplin safeTransfer function which could lead to potential security risk

## Vulnerability Details

`token. Transfer(admin, balance);`

https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20.sol#L152

## Impact

The impact of not using safeTransfer can be significant. If tokens are transferred to a contract that is not prepared to handle them, they can be locked in the contract forever. This can lead to financial loss for the user who intended to transfer the tokens. Additionally, it can lead to a loss of trust in the platform, which can affect its reputation and user base

## Proof of Concept

Let's consider a scenario where a user attempts to transfer tokens to a smart contract that does not implement the IERC721Receiver.onERC721Received function. If the transfer function is used, the tokens will be transferred without any checks:
`token.transfer(contractAddress, amount);
`
However, if the safeTransfer function is used, the transfer will fail, preventing the tokens from being locked.


Describe the tools used throughout your testing and analysis process.

## Recommended Mitigation Steps

`token.safeTransfer(contractAddress, amount);
`
To mitigate this risk, developers should use the safeTransfer function instead of transfer when interacting with tokens in smart contracts. This ensures that if the recipient is a smart contract, it is prepared to handle the tokens
