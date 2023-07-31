# Introduction

The Moonwell Protocol is a fork of Benqi, which is a fork of Compound v2 with features like borrow caps and multi-token emissions. 
The Moonwell Protocol is a major system upgrade to use solidity 0.8.17, add supply caps, and a number
of improvements for user experience (things like mintWithPermit and claimAllRewards).    

## Observations

## Codebase quality analysis

The codebase forked Benqi and Compound, both of which have undergone multiple audits and have high code security.    
The fork is based on Benqi and carries a lot of compatible code for lower versions of solidity, such as overflow detection, which results in lower readability.

## Centralization risks

Trusted owners can change key parameters such as setting the seer price, pause, etc.

## Mechanism review

### CDP

CDP is basically based on Compound, with many audits, the quality is reliable.

#### Collaterals

- [x] If a user's CDP is closed (e.g., when the user has paid off all the debt), what happens to the vault entry in storage? Is the storage variable erased? Are there checks in the code sections where the variable is used to ensure its existence?
Yes, these stored variables will be deleted normally, such as exitMarket.
- [x] Is it possible to have a condition when a user cannot repay their loan?
When the ratio is lower than the healthy rate, users will be liquidated in time to avoid further losses
- [x] How is the pool value calculated? How is the fee paid by borrowers for the loan distributed? If a portion goes to the lender (depositor) and another portion goes to the pool, is the calculation of the pool value correctly implemented?
The pool value is mainly divided into three variables: supply, borrow and reserve, and supply + borrow - reserve is the totalSupply of the token. A portion of the borrowing fee goes into a pool of funds, known as a reserve.

#### Liquidations

- [x] Can the user liquidate their own position with a profit? How can the health ratio change, apart from typical cases such as withdrawal of collateral, taking on additional debt, or a significant drop in the price of collateral?
Yes, users can liquidate themselves with no restrictions. When the collateral drops significantly, the aggregator may return the built-in minimum price, potentially affecting liquidation.
- [x] When is the interest rate calculated? Before or after liquidation of the vault?
Interest is calculated before any operation is performed.
- [x] Is there a risk of re-entrancy in the ERC721 withdrawal method before checking for a normal health ratio? If the health ratio is checked after calling safeTransferFrom() with the implementation of onERC721Received(), is there any way to bypass the health ratio check in the callback?
No, ERC721 is not supported

### Oracle

When reading the chainlink oracle price, there is a problem with the freshness check, which may read the stale price.     
In addition to reading from chainlink, the oracle also allows the owner to set the price directly, which leads to two problems:
1. Centralized risk, admin can change collateral price and rug pull. Of course, the owner is the governor, so the risk is low
2. When the collateral price fluctuates sharply, admin cannot update the price in time(single point risk, such as offline), and the stale price may be used for arbitrage

## Systemic risks

### Isolate

Comptroller does not isolate collateral, such as when a collateral price fails to obtain, it will affect the operation of the entire protocol.  

### Wormhole

The TemporalGovernor administration is based on wormhole, where validation does not detect whether the target chain is the current chain. Currently the contract is only deployed in the Base, which has no impact, but if extended to other chains in the future, it may lead to double spending issues.

### L1 Difference

The contract will be deployed in Base and there are some differences compared to L1:
1. The sequencer should be checked whether is offline before reading the oracle price
2. Base does not support the push0 opcode at the moment, so solidity 0.20.0 should not be used
3. Block production intervals are different, so block cannot be used like Compound, and this contract addresses compatibility issues by using timestamps


### Time spent:
12 hours