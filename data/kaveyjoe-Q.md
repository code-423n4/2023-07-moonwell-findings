##  1 .Target : https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Governance/TemporalGovernor.sol

- internal functions (e.g., _queueProposal, _executeProposal, _sanityCheckPayload) do not have any visibility modifiers explicitly defined. By default, functions in Solidity are internal, but it is recommended to explicitly specify the visibility to enhance code readability and make the contract more secure.

- The contract lacks input validation in various places, the addressToBytes function doesn't check whether the given address is valid or not. Proper input validation is crucial to prevent unexpected behavior and potential vulnerabilities.

-  In the permissionlessUnpause function, there is a calculation that could potentially lead to an arithmetic overflow. The expression lastPauseTime + permissionlessUnpauseTime should be checked to avoid this scenario.

- The mechanism for granting and revoking the guardian's ability to pause the contract seems to have issues. In the togglePause function, it sets guardianPauseAllowed to false after unpausing the contract, which seems contradictory. Additionally, there might be cases where guardianPauseAllowed is not set back to true after revoking the guardian's ability.

- The functions setTrustedSenders and unSetTrustedSenders should have more robust permission checks. Currently, they only check if the caller is the contract itself, which might not be sufficient to prevent unauthorized updates.

- The contract uses target.call{value: value}(data) to execute external calls, but this method is susceptible to reentrancy attacks. It's recommended to use target.call{value: value, gas: gasAmount}(...) with a specified gas amount and handle the return values accordingly.


##  2. TARGET : https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MErc20.sol

- the mintWithPermit function allows users to supply assets without a 2-step approval process by using the EIP-2612 permit mechanism. However, the function does not validate the permit signature or check if the user has the necessary underlying assets.

Here's the relevant part of the code:

function mintWithPermit(
    uint mintAmount,
    uint deadline,
    uint8 v, bytes32 r, bytes32 s
) override external returns (uint) {
    IERC20Permit token = IERC20Permit(underlying);

    // Go submit our pre-approval signature data to the underlying token, but
    // explicitly fail if there is an issue.
    SafeERC20.safePermit(
        token,
        msg.sender, address(this),
        mintAmount, deadline,
        v, r, s
    );

    (uint err,) = mintInternal(mintAmount);
    return err;
}

In this function, the safePermit function from the OpenZeppelin library is used to execute the permit function of the underlying ERC-20 token contract. This allows the user to provide a permit signature that approves the contract (MErc20) to spend their tokens on their behalf.

However, there are no checks in the mintWithPermit function to ensure that the permit signature is valid or that the user has the required balance of the underlying asset. It simply proceeds with the minting process by calling the mintInternal function.

If this contract is used in a financial application, it could pose security risks. Users may unintentionally or maliciously approve the contract to spend tokens it shouldn't have access to, leading to potential loss of funds or disruptions in the system's functioning.

- To make the contract safer, additional checks and validations should be added in the mintWithPermit function. 

 ## 3 . TARGET : https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MErc20Delegator.sol

- The function delegateToViewImplementation is supposed to delegate execution to an implementation contract and return the result. However, it incorrectly calls the delegateToImplementation function in its static call, resulting in a mismatch in function signatures.

function delegateToViewImplementation(bytes memory data) public view returns (bytes memory) {
    (bool success, bytes memory returnData) = address(this).staticcall(abi.encodeWithSignature("delegateToImplementation(bytes)", data));
    assembly {
        if eq(success, 0) {
            revert(add(returnData, 0x20), returndatasize())
        }
    }
    return abi.decode(returnData, (bytes));
}
 

 The delegateToViewImplementation function is using the staticcall assembly opcode to delegate execution to the implementation contract, which is correct for a view function as it prevents state changes. However, 
the issue lies in the following line:

(bool success, bytes memory returnData) = address(this).staticcall(abi.encodeWithSignature("delegateToImplementation(bytes)", data));


The function signature passed to abi.encodeWithSignature is "delegateToImplementation(bytes)", which is the function signature of delegateToImplementation, not delegateTo. This causes the static call to fail as it tries to execute the wrong function.

- solution :

- To fix the bug, update the function signature in the delegateToViewImplementation function to "delegateTo(bytes)", which matches the correct function signature.

function delegateToViewImplementation(bytes memory data) public view returns (bytes memory) {
    (bool success, bytes memory returnData) = address(this).staticcall(abi.encodeWithSignature("delegateTo(bytes)", data));
    assembly {
        if eq(success, 0) {
            revert(add(returnData, 0x20), returndatasize())
        }
    }
    return returnData;
}

With this change, the delegateToViewImplementation function will correctly delegate execution to the implementation contract and return the expected result for view functions.

 ## 4 . TARGET : https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MErc20Delegate.sol


- In the MErc20Delegate contract, the _becomeImplementation and _resignImplementation functions are meant to be accessible only by the admin. 
The issue is with the following lines of code in both functions:

// Shh -- we don't ever want this hook to be marked pure
if (false) {
    implementation = address(0);
}


In both functions, there's a useless if (false) statement that sets the implementation variable to address(0). However, since the condition is false, this code is never executed, and the implementation variable is never changed.

This means that anyone can call the _becomeImplementation and _resignImplementation functions without any proper access control checks since the intended check on the msg.sender == admin is not enforced. This effectively means that the admin control is bypassed, allowing any account to take over the contract as the implementation, or resign the implementation, potentially compromising the security and functionality of the contract.

- To fix this issue, we should remove the unnecessary if (false) statements from both functions.

 The corrected MErc20Delegate would look like this:

contract MErc20Delegate is MErc20, MDelegateInterface {
    constructor() {}

    function _becomeImplementation(bytes memory data) virtual override public {
        // Shh -- currently unused
        data;

        require(msg.sender == admin, "only the admin may call _becomeImplementation");
    }

    function _resignImplementation() virtual override public {
        require(msg.sender == admin, "only the admin may call _resignImplementation");
    }
}


With this modification, the admin access control is correctly enforced, and only the admin will be able to call these functions, preventing potential security issues. 

## 5 . TARGET : https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/IRModels/WhitePaperInterestRateModel.sol

There is a potential issue with the utilization rate calculation, which could lead to incorrect interest rate calculations. The bug is related to how the utilization rate is calculated when cash, borrows, or reserves are zero.

- In the utilizationRate function, when borrows is equal to zero, the utilization rate is incorrectly set to zero. This would imply that there are no borrows, and the function would not handle the scenario where borrows are zero, but there is non-zero cash and reserves.

function utilizationRate(uint cash, uint borrows, uint reserves) public pure returns (uint) {
    if (borrows == 0) {
        return 0; // Incorrect handling of borrows being zero
    }

    return borrows.mul(1e18).div(cash.add(borrows).sub(reserves));
}


- Impact:
This bug could lead to incorrect interest rate calculations in the getBorrowRate and getSupplyRate functions when the borrows amount is zero, even if there are non-zero cash and reserves. This may result in borrowers or lenders being charged or rewarded with incorrect interest rates, affecting the overall functioning of the financial protocol.

Suggested Fix:
To fix the bug, the utilization rate calculation should be modified to handle the scenario where borrows are zero. Instead of returning zero, the utilization rate should be set to the maximum value 1e18 to represent full utilization.

function utilizationRate(uint cash, uint borrows, uint reserves) public pure returns (uint) {
    // Utilization rate is 0 when there are no borrows
    if (borrows == 0) {
        return 1e18; // Set utilization rate to maximum value to represent full utilization
    }

    return borrows.mul(1e18).div(cash.add(borrows).sub(reserves));
}


## 6 . TARGET : https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/IRModels/JumpRateModel.sol

- In the getBorrowRate function, the borrow rate is calculated based on the utilization rate (util) and the specified parameters (kink, baseRatePerTimestamp, multiplierPerTimestamp, and jumpMultiplierPerTimestamp).

When the utilization rate is less than or equal to the kink, the correct formula is used:

return util.mul(multiplierPerTimestamp).div(1e18).add(baseRatePerTimestamp);


However, when the utilization rate exceeds the kink, the calculation is as follows:

uint normalRate = kink.mul(multiplierPerTimestamp).div(1e18).add(baseRatePerTimestamp);
uint excessUtil = util.sub(kink);
return excessUtil.mul(jumpMultiplierPerTimestamp).div(1e18).add(normalRate);


The issue lies in the calculation of normalRate. Since the kink value is used directly as a multiplier for multiplierPerTimestamp, the result may not accurately represent the interest rate at the kink point. This could lead to incorrect interest rates being applied for borrowing.

- Recommendation:

To fix , we need to ensure that the normalRate calculation is correct when the utilization rate exceeds the kink. One possible fix is to adjust the formula as follows:


uint normalRate = kink.mul(jumpMultiplierPerTimestamp).div(1e18).add(baseRatePerTimestamp);
uint excessUtil = util.sub(kink);
return excessUtil.mul(jumpMultiplierPerTimestamp).div(1e18).add(normalRate);
By using jumpMultiplierPerTimestamp instead of multiplierPerTimestamp for the normalRate calculation when the utilization rate exceeds the kink, we can achieve the correct behavior.

## 7 . TARGET : https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Oracles/ChainlinkCompositeOracle.sol

In the getDerivedPriceThreeOracles function, there is a potential overflow issue. The function calculates the scalingFactor as int256(10 ** uint256(expectedDecimals * 2)), where expectedDecimals is provided by the user as an argument. If the expectedDecimals is greater than 9, the scalingFactor value can exceed the maximum value representable by int256, leading to an overflow. This can result in incorrect derived prices and unpredictable behavior of the contract.

- solution :

To avoid potential overflow, you should perform a check on the expectedDecimals value before calculating the scalingFactor. If expectedDecimals is greater than 9, revert the function and provide an appropriate error message.
 

## 8 . TARGET : https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Oracles/ChainlinkOracle.sol

- The getUnderlyingPrice function in the ChainlinkOracle contract has a branch intended to handle native tokens. However, the condition used to determine whether a token is a native token is incorrect. As a result, the contract may execute code that should never be reached, leading to unexpected behavior or potential vulnerabilities.


Affected Function :

function getUnderlyingPrice(
    MToken mToken
) public view override returns (uint256) {
    string memory symbol = mToken.symbol();
    if (keccak256(abi.encodePacked(symbol)) == nativeToken) {
        return getChainlinkPrice(getFeed(symbol));
    } else {
        return getPrice(mToken);
    }
}



The condition keccak256(abi.encodePacked(symbol)) == nativeToken is intended to check whether the symbol of the mToken is the same as the nativeToken, indicating that the token is a native token. However, the nativeToken is assigned the value of keccak256(abi.encodePacked(_nativeToken)) in the constructor, which is a one-time initialization.

This means that the condition will always evaluate to false after the contract deployment, making the code block for handling native tokens effectively unreachable. As a result, this part of the code is redundant and should be removed.

- Solution : 
To fix this issue, we can remove the redundant condition and directly call the getPrice(mToken) function for all cases. The modified code will look like this:

function getUnderlyingPrice(
    MToken mToken
) public view override returns (uint256) {
    return getPrice(mToken);
}


By making this change, the getPrice function will handle the logic for obtaining the price, either from the overridden prices set by the admin or from the Chainlink feed.



