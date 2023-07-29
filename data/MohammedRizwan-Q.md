## Summary

### Low Risk Issues
|Number|Issue|Instances| |
|-|:-|:-:|:-:|
| [L&#x2011;01] | Incorrect timestampsPerYear constant in JUMPRATEMODEL documentation | 1 |
| [L&#x2011;02] | chainId should use uint64/uint256 instead of uint16 | 1 |
| [L&#x2011;03] | Calls inside loops that may address DoS | 1 |
| [L&#x2011;04] | Avoid using safeMath for solidity version 0.8.0 and above | 1 |
| [L&#x2011;05] | set limit for emissionCap in _setEmissionCap() | 1 |
| [L&#x2011;06] | delegatecall should check comptrollerImplementation address first | 1 |
| [L&#x2011;07] | block.timestamp can be directly used instead of making function | 13 |
| [L&#x2011;08] | A very Misleading comment and function parameter in MToken.sol | 3 |
| [L&#x2011;09] | require() check validations should be at top of functions, not at last | 2 |
| [L&#x2011;10] | Incomplete interaction summary and writeup for mint and borrow in documentation | 2 |
| [L&#x2011;11] | Use Inline assembly in contracts, if only neccessary | 7 |
| [L&#x2011;12] | Use of block.timestamp instead of block.number | All contracts |
| [L&#x2011;13] | Missing payment amount check in Comptroller.sol | 3 |
| [L&#x2011;14] | WETH9 interface function visibility does not match with L2 WETH9 interface | 1 |
| [L&#x2011;15] | Pre-EIP-155 is not supported by default | 1 |
| [L&#x2011;16] | Misleading comment in MIP00 documentation | 3 |

### [L&#x2011;01]  Incorrect timestampsPerYear constant in JUMPRATEMODEL documentation
timestampsPerYear contant is incorrectly mentioned in documentation as compared to current implementation in JumpRateModel.sol contract. The documentation states,

```timestampsPerYear: The number of timestamps per year (606024*365).```

This is not correct and must be corrected per recommendation.

### Recommended Mitigation Steps
```timestampsPerYear: The number of timestamps per year (60 * 60 * 24 * 365).```

### [L&#x2011;02]  chainId should use uint64/uint256 instead of uint16
Per the ethereum paper, the chainID is defined in uint256 but later a proposal is submitted to make uint256 chainID to uint64. 
Reference:- https://github.com/ethereum/evmc/pull/420

But the current the implementation has used uint16 for chainID which will be insufficient with growing chainID as uint16 will not support large numbers. Some currently supported EVM chains with chainID can not support. For example, ``` Aurora Mainnet  ChainID- 1313161554 (0x4e454152)```

Different EVM supported chainID can be seen from below link:
https://chainlist.org/chain/1313161554

### Recommended Mitigation steps
Use uint64 for chainId instead of uint16. If design allows, it is also recommended to use uint256 for chainId per Ethereum paper.

### [L&#x2011;03]  Calls inside loops that may address DoS
In TemporalGovernor.sol contract _executeProposal() function,
Calls to external contracts inside a loop are dangerous because it could lead to DoS if one of the calls reverts or execution runs out of gas. Such issue also introduces chance of problems with the gas limits.

**Per SWC-113:**
External calls can fail accidentally or deliberately, which can cause a DoS condition in the contract. To minimize the damage caused by such failures, it is better to isolate each external call into its own transaction that can be initiated by the recipient of the call.

Reference link- https://swcregistry.io/docs/SWC-113

**Code Reference:**
```Solidity
File: contracts/ArcadeTreasury.sol

344    function _executeProposal(bytes memory VAA, bool overrideDelay) private {


       // Some code


394        for (uint256 i = 0; i < targets.length; i++) {
395            address target = targets[i];
396            uint256 value = values[i];
397            bytes memory data = calldatas[i];
398
399            // Go make our call, and if it is not successful revert with the error bubbling up
400            (bool success, bytes memory returnData) = target.call{value: value}(
401                data
402            );


       // Some code


409    }
```
### Recommended Mitigation Steps
1) Avoid combining multiple calls in a single transaction, especially when calls are executed as part of a loop
2) Always assume that external calls can fail
3) Implement the contract logic to handle failed calls

### [L&#x2011;04]  Avoid using safeMath for solidity version 0.8.0 and above
Contracts like WhitePaperInterestRateModel.sol and JumpRateModel.sol have used safeMath for calculations.Solidity ^0.8.0 handles overflow and underflow by default. There is no need of safeMath here, A direct arithmatic caculations can be performed.

There are 2 instances of this issue:
1) [WhitePaperInterestRateModel.sol](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/IRModels/WhitePaperInterestRateModel.sol#L2) and 
2) [JumpRateModel.sol](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/IRModels/JumpRateModel.sol#L2)

### Recommended Mitigations steps
Remove safeMath from these contracts.

### [L&#x2011;05]  set limit for emissionCap in _setEmissionCap()
In MultiRewardDistributor.sol, _setEmissionCap() is used to set emission cap to any value and this can be accessed by comptroller admin howver emmision limit must be set otherwise 0 as well as any higher value can be passed accidentally or intentionally.

There is 1 instance of this issue:

```Solidity
File: src/core/MultiRewardDistributor/MultiRewardDistributor.sol

    function _setEmissionCap(
        uint256 _newEmissionCap
    ) external onlyComptrollersAdmin {
        uint256 oldEmissionCap = emissionCap;

        emissionCap = _newEmissionCap;

        emit NewEmissionCap(oldEmissionCap, _newEmissionCap);
    }
```

### Recommended Mitigation steps
Add a limit check to validate the emissionCap.

### [L&#x2011;06]  delegatecall should check comptrollerImplementation address first
Per solidity documentation, EXTCODE is not checked for delegatecall as it is low level call function. It is an exception by solidity compiler but this can result on functionality. If the comptrollerImplementation address is not existed, the delegatecall will return success. It means it always return success for non-existent address, Therefore comptrollerImplementation code existence check is very much required.

```Solidity
File: src/core/Unitroller.sol

    fallback() external {
        // delegate all other functions to current implementation
        (bool success, ) = comptrollerImplementation.delegatecall(msg.data);

        assembly {
              let free_mem_ptr := mload(0x40)
              returndatacopy(free_mem_ptr, 0, returndatasize())

              switch success
              case 0 { revert(free_mem_ptr, returndatasize()) }
              default { return(free_mem_ptr, returndatasize()) }
        }
    }
```

### Recommended Mitigation steps
```Solidity
File: src/core/Unitroller.sol

    fallback() external {
+      uint codeSize;
+      assembly {
+            codeSize := extcodesize(comptrollerImplementation)
+      }
+      require(codeSize > 0);

        // delegate all other functions to current implementation
        (bool success, ) = comptrollerImplementation.delegatecall(msg.data);

        assembly {
              let free_mem_ptr := mload(0x40)
              returndatacopy(free_mem_ptr, 0, returndatasize())

              switch success
              case 0 { revert(free_mem_ptr, returndatasize()) }
              default { return(free_mem_ptr, returndatasize()) }
        }
    }
```

### [L&#x2011;07]  block.timestamp can be directly used instead of making function
In MToken.sol, block.timestamp can be directly used instead of making function.

There is 13 instance of this issue:

```Solidity
File: src/core/MToken.sol

    function getBlockTimestamp() virtual internal view returns (uint) {
        return block.timestamp;
    }
```
### Recommended Mitigation steps
Use block.timestamp instead of wrapping it under function.

### [L&#x2011;08]  A very Misleading comment in MToken.sol
In MToken.sol, in accrueInterest() function, there is misleading comment and function parameter which must be corrected. Here the calculations are performed in terms of blocktimestamp howevever comments and function parameters states it as in block terms which is incorrect.

There is 3 instances of this issue:

### Recommended Mitigation Steps

```Solidity
File: src/core/MToken.sol

-       /* Calculate the number of blocks elapsed since the last accrual */
+       /* Calculate the total timestamp elapsed since the last accrual */

-       (MathError mathErr, uint blockDelta) = subUInt(currentBlockTimestamp, accrualBlockTimestampPrior);
+       (MathError mathErr, uint timestampDelta) = subUInt(currentBlockTimestamp, accrualBlockTimestampPrior);

        require(mathErr == MathError.NO_ERROR, "could not calculate block delta");

        /*
         * Calculate the interest accumulated into borrows and reserves and the new index:
-        *  simpleInterestFactor = borrowRate * blockDelta
+        *  simpleInterestFactor = borrowRate * timestampDelta
         *  interestAccumulated = simpleInterestFactor * totalBorrows
         *  totalBorrowsNew = interestAccumulated + totalBorrows
         *  totalReservesNew = interestAccumulated * reserveFactor + totalReserves
         *  borrowIndexNew = simpleInterestFactor * borrowIndex + borrowIndex
         */

        Exp memory simpleInterestFactor;
        uint interestAccumulated;
        uint totalBorrowsNew;
        uint totalReservesNew;
        uint borrowIndexNew;

-       (mathErr, simpleInterestFactor) = mulScalar(Exp({mantissa: borrowRateMantissa}), blockDelta);
+       (mathErr, simpleInterestFactor) = mulScalar(Exp({mantissa: borrowRateMantissa}), timestampDelta);
```

### [L&#x2011;09]  require() check validations should be at top of functions, not at last
In MErc20Delegate.sol, require() checks should always be at top checking the access control to function instead putting at lost. This avoid gas wastage till the code coming to require check however, the current implementation is not recommended considering checks, Effects, Interactions pattern too. However there is no external interactions here.

There are 2 instances of this issue:

### Recommended Mitigation steps

```Solidity
File: src/core/MErc20Delegate.sol

    function _becomeImplementation(bytes memory data) virtual override public {
+       require(msg.sender == admin, "only the admin may call _becomeImplementation");
        // Shh -- currently unused
        data;

        // Shh -- we don't ever want this hook to be marked pure
        if (false) {
            implementation = address(0);
        }

-       require(msg.sender == admin, "only the admin may call _becomeImplementation");
    }

    /**
     * @notice Called by the delegator on a delegate to forfeit its responsibility
     */
    function _resignImplementation() virtual override public {
+       require(msg.sender == admin, "only the admin may call _resignImplementation");
        // Shh -- we don't ever want this hook to be marked pure
        if (false) {
            implementation = address(0);
        }

-       require(msg.sender == admin, "only the admin may call _resignImplementation");
    }
```

### [L&#x2011;10]  Incomplete interaction summary and writeup for mint and borrow in documentation
In CROSSCONTRACTINTERACTION.md, cross chain interaction with mint and borrow is given however as the document says ```in depth look``` which is missing. The interaction summary is incomplete given with direct path wothout mentioning the sub functions or internal functions for which new user will confuse and mislead them. Therefore the interaction summary for both mint and borrow needs to add more detail. I recommend the interaction summary for both, however the description should be modified for same by developers.

There are 2 instances of this issue:
Document Link: [click here](https://github.com/code-423n4/2023-07-moonwell/blob/main/CROSSCONTRACTINTERACTION.md)

### Recommmended Mitigation Steps

```Solidity
File: main/CROSSCONTRACTINTERACTION.md

**Mint:**
- Summarizing the interactions that occur when a user calls mint: MToken -> Comptroller -> MultiRewardDistributor -> MToken
+ Summarizing the interactions that occur when a user calls mint: MErc20 -> MToken -> mintInternal -> mintFresh -> Comptroller -> mintAllowed -> MultiRewardDistributor -> MToken -> MErc20 

**Borrow:**
- Summarizing the interactions that occur when a user calls borrow: MToken -> Comptroller -> MultiRewardDistributor -> MToken
+ Summarizing the interactions that occur when a user calls borrow: MErc20 -> MToken -> borrowInternal -> borrowFresh -> Comptroller -> -> borrowAllowed -> MultiRewardDistributor -> MToken -> MErc20 
```

**Note:** Add more depth to description per above.

### [L&#x2011;11]  Use Inline assembly in contracts, if only neccessary
Inline assembly is a way to access the Virtual Machine at a low level. This discards several important safety features in Solidity. It is recommended only when inline assembly is very much needed in contracts.

There are 7 instances of this issue:

In MErc20.sol at L-[193](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MErc20.sol#L193-L205)

In MErc20.sol at L-[228](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MErc20.sol#L228-L240)

In MErc20Delegator.sol at L-[457](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MErc20Delegator.sol#L457-L461)

In MErc20Delegator.sol at L-[457](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MErc20Delegator.sol#L457-L461)

In MErc20Delegator.sol at L-[484](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MErc20Delegator.sol#L484-L487)

In MErc20Delegator.sol at L-[457](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MErc20Delegator.sol#L457-L460)

In MErc20Delegator.sol at L-[140](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Unitroller.sol#L140-L147)

### Recommended Mitigation steps
The contracts should avoid using inline assembly because it interacts with the blockchain Virtual Machine at a low level. An attacker could bypass many essential safety features of Solidity.

### [L&#x2011;12]  Use of block.timestamp instead of block.number
The contracts JumpRateModel, WhitePaperInterestRateModel, Comptroller.sol and other contracts has used block.timestamp. The global variable block.timestamp does not necessarily hold the current time, and may not be accurate. Miners can influence the value of block.timestamp to perform Maximal Extractable Value (MEV) attacks. There is no guarantee that the value is correct, only that it is higher than the previous block’s timestamp.
However, it is not as reliable as block.number, as it can be manipulated by miners. This makes it a less reliable choice for time-dependent computations.

Block.number is a more reliable source of truth than block.timestamp, as it is guaranteed by the protocol to increase by one compared to the previous block.

Instances are present in all contracts.

### Recommended Mitigation steps
Use block.number instead of block.timestamp. However if it intended design with well accepted risk then it should be okay.

### [L&#x2011;13]  Missing payment amount check in Comptroller.sol
The protocol includes some financial functions Comptroller.sol such as borrowAllowed(), liquidateBorrowAllowed(), transferAllowed(), etc. Attackers could use these functions with large amounts in Flash-Loan or similar financial attacks. Other DeFi platforms like The DAO, Spartan, and others were hacked, and attackers stole a significant amount of funds.The issue here is there are no restrictions on large transfers. Prevention is better than cure. The already happended hacks suggest to have a check on large amounts and it should be considered in functions. Basically, users are not limited for transactions in these functions.

This could be a Medium severity finding but it could also be intended design so considered it as low severity with that assumption. However judge decision is respected.

There are 3 instances of this issue:

In [Comptroller.sol, borrowAllowed() function](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Comptroller.sol#L310-L356)

In [Comptroller.sol, liquidateBorrowAllowed() function](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Comptroller.sol#L394-L424)

In [Comptroller.sol, transferAllowed() function](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Comptroller.sol#L472-L488)

### Recommended Mitigation Steps
It is recommended to implement a mechanism which controls payment amount.

### [L&#x2011;14]  WETH9 interface function visibility does not match with L2 WETH9 interface
The WETH9 interface used in contract does not match the function visibility of L2 deployed WETH9 interface. Per the discussion with sponsor, The contracts will be deployed only on BASE. BASE is built on the Bedrock release of the OP Stack. Therefore on [OP mainnet](https://community.optimism.io/docs/developers/build/differences/#contract-addresses), WETH9 contract is installed on address ```0x4200000000000000000000000000000000000006```. Check the contract address and see the functions visibility which differs in current implementation.

Code reference: [Click here](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/router/WETHRouter.sol#L6)

WETH9 used interface: [Click here](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/router/IWETH.sol#L22-L39)

### Recommended Mitigation steps
Use WETH9 interface per OP WETH9 contract address ```0x4200000000000000000000000000000000000006```

### [L&#x2011;15]  Pre-EIP-155 is not supported by default
Per discussion with sponsor, the contracts will be deployed on BASE L2 blockchain. BASE is built on the Bedrock release of the OP Stack. 

OP stack documentation states,
"Pre-EIP-155 transactions do not have a chain ID, which means a transaction on one Ethereum blockchain can be replayed on others. This is a security risk, so pre-EIP-155 transactions are not supported on OP Stack by default."
Reference link: [Click here](https://stack.optimism.io/docs/releases/bedrock/differences/#pre-eip-155-support)

### Recommended Mitigation steps
An extra care should be taken for this EIP-155 default issue. Ensure it is documented in contracts as well as MoonWell documentation.

### [L&#x2011;16]  Misleading comment in MIP00 documentation
In MIP00.md documentation, It is referred as cToken instead of mToken which is not correct as the contracts has used MToken and no used compound Token. 

Documentation reference [link](https://github.com/code-423n4/2023-07-moonwell/blob/main/docs/MIP00.md#build-cross-chain-gov-proposal-steps)

There are 3 instances of this issue:

```Solidity

- Set Collateral Factor on CToken: // Some text
+ Set Collateral Factor on MToken: // Some text

- Set Liquidation Incentive on CToken: // Some text
+ Set Liquidation Incentive on MToken: // Some text

- Set Close Factor on CToken: // Some text
+ Set Close Factor on MToken: // Some text
```