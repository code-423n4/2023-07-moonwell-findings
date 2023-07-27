## Advanced Analysis Report for [Moonwell](https://github.com/code-423n4/2023-07-moonwell) by K42
### Overview 
- [Moonwell](https://github.com/code-423n4/2023-07-moonwell) is a decentralized finance protocol that has recently undergone significant changes in its [V2](https://github.com/code-423n4/2023-07-moonwell) update. The changes include updates to the [Comptroller](https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Comptroller.sol) contract, the introduction of a [Multi-Reward Distributor](https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol), and various other updates to the Compound v2 codebase.

### Understanding the Ecosystem:
- [Moonwell](https://github.com/code-423n4/2023-07-moonwell) operates within the DeFi ecosystem, interacting with various other protocols and assets. The protocol allows users to earn interest on their assets and borrow against them. The recent updates have introduced the ability to earn multiple rewards, enhancing the earning potential for users.

### Codebase Quality Analysis: 
- The [Moonwell](https://github.com/code-423n4/2023-07-moonwell) codebase appears to be well-structured and organized. The contracts are clearly named and their functions are well-documented. The codebase has been updated to Solidity 0.8.17, which brings several improvements and security enhancements. I also noticed, even though out of scope, the use of a new contract, [ExponentialNoError](https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/ExponentialNoError.sol), to handle potential underflow and overflow errors, which is a good practice.

### Architecture Recommendations: 
- The architecture of [Moonwell](https://github.com/code-423n4/2023-07-moonwell) is well-designed, with clear separation of concerns among the contracts. The introduction of the [Multi-Reward Distributor](https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol) is a significant improvement, as it allows the protocol to distribute multiple rewards efficiently. However, it would be beneficial to have more detailed comments in the code explaining the logic behind the calculations, especially in complex functions like [updateMarketSupplyIndex](https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L536C5-L540C6).

### Centralization Risks: 
- The protocol has a Pause Guardian, which can pause the distribution of rewards. This introduces a degree of centralization, as the Pause Guardian has significant control over the protocol. It's important to ensure that the Pause Guardian cannot be easily manipulated or exploited.

### Mechanism Review: 
- The [Multi-Reward Distributor](https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol) is a significant upgrade, allowing users to earn rewards from multiple sources. This mechanism enhances the protocol's value proposition and could attract more users to the platform.
- The changes to the [Comptroller](https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Comptroller.sol), including the addition of new APIs and the implementation of a 'time lock' mechanism, improve the system's functionality and user experience. 

### Systemic Risks: 
- The protocol interacts with various other protocols and assets in the DeFi ecosystem, which introduces systemic risk. If there's a vulnerability in one of these protocols or assets, it could potentially impact [Moonwell](https://github.com/code-423n4/2023-07-moonwell). It's crucial to regularly monitor and assess the protocols and assets that [Moonwell](https://github.com/code-423n4/2023-07-moonwell) interacts with.

### Areas of Concern

- The Pause Guardian introduces a degree of centralization.
- The complexity of some functions could make them difficult to understand and maintain.
- The systemic risk introduced by interactions with other protocols and assets.

### Codebase Analysis
- The codebase is well-structured and organized, with clear separation of concerns among the contracts. The contracts are clearly named and their functions are well-documented. The codebase has been updated to Solidity 0.8.17, which brings several improvements and security enhancements.

### Recommendations
- Add more detailed comments in the code explaining the logic behind the calculations.
- Regularly monitor and assess the protocols and assets that [Moonwell](https://github.com/code-423n4/2023-07-moonwell) interacts with to mitigate systemic risk.
- Consider implementing a decentralized governance system to reduce the centralization risk associated with the Pause Guardian.

### Contract Details
- The main contracts in the [Moonwell](https://github.com/code-423n4/2023-07-moonwell) protocol are the [Comptroller](https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Comptroller.sol) and the [Multi-Reward Distributor](https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol). The [Comptroller](https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Comptroller.sol) contract handles the core logic of the protocol, including accruing interest and updating supply and borrow indices. The [Multi-Reward Distributor](https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol) contract handles the distribution of multiple rewards.

### My understanding of V2 Contract Changes

[Comptroller](https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Comptroller.sol) Changes:

- The [Comptroller](https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Comptroller.sol) has been updated to version 0.8.17 of Solidity, which brings several improvements and changes.
- The [Comptroller](https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Comptroller.sol) now has a [Multi-Reward Distributor](https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Comptroller.sol#L901C1-L909C6), which is a new contract that handles the distribution of multiple rewards.
- The [Comptroller](https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Comptroller.sol) now has a [Pause Guardian](https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Comptroller.sol#L880C1-L895C6), which can pause the distribution of rewards.

Other Changes to the Compound v2 codebase:

- The codebase now has a new contract, [Unitroller](https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Unitroller.sol), which is a version of the [Comptroller](https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Comptroller.sol) contract that doesn't have any storage.

[Multi-Reward Distributor](https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol):

- The [Multi-Reward Distributor](https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol) is a new contract that handles the distribution of multiple rewards.
- The [Multi-Reward Distributor](https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol) has a [Pause Guardian](https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L109C5-L109C40), which can pause the distribution of rewards.
- The [Multi-Reward Distributor](https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol) has a new function, [updateMarketSupplyIndex](https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L536C3-L540C6), which updates the supply index for a market.
- The [Multi-Reward Distributor](https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol) has a new function, [disperseSupplierRewards](https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L549C2-L555C6), which disperses supply rewards to a supplier.
- The [Multi-Reward Distributor](https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol) has a new function, [updateMarketBorrowIndex](https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L577C3-L582C1), which updates the borrow index for a market.
- The [Multi-Reward Distributor](https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol) has a new function, [disperseBorrowerRewards](https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L590C2-L596C6), which disperses borrow rewards to a borrower.
- The [Multi-Reward Distributor](https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol) has a new function, [sendReward](https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L1214C2-L1247C6), which sends a reward to a user.

### Conclusion
- [Moonwell](https://github.com/code-423n4/2023-07-moonwell)'s V2 update brings significant improvements to the protocol, including the ability to earn multiple rewards and more efficient interest accrual. However, there are areas of concern, including the centralization risk associated with the Pause Guardian and the complexity of some functions. With careful management and ongoing improvements, [Moonwell](https://github.com/code-423n4/2023-07-moonwell) has the potential to be a leading protocol in the DeFi space.

### Time spent:
16 hours