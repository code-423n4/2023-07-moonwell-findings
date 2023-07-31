# 1. Learnings

From reviewing the Moonwell codebase,I was able to achieve the following:

1. Learning more about open lending and borrowing protocols,and the logic behind flywheel rewards distribution.
2. Learning more about security measures in emergency cases.
3. Delved deep in using foundry for codebase analysis and testing the what-if scenarios.

# 2. Approach Taken

1. first, I gained a high-level understanding of the protocol's objectives and architecture from the provided documentation.
2. then conducted a detailed manual review of the code, trying to identify potential vulnerabilities.
3. then tried to match-pattern with known vulnerabilities reported in previous defi projects,mainly referred to solodit website for past related vulnerabilities found in similar protocols.
4. I referred to the tests,changing the test parameters to analyse the what-if scenarios.
5. finally, I documented my findings with PoCs.

# 3. Codebase Analysis

The codebase can be divided into three groups:

#### 1. Governance and proposals execution:

- `TemporalGovernor` contract
  contains all the governance logic as setting trusted senders between chains by a governance proposal,queuing and executing proposals after being verified.

#### 2. Markets & user operations:

- `IRModels` contracts:
  where two interset rate models are defined to be adopted (one of them) by the markets: jump & whitpaper models.
- `Oracles` contracts:
  where the price feed of markets underlying assets are retrieved either directly or aggregated by the `ChainlinkCompositeOracle` if there's no direct feed for a specific aseet.
- `WETHRouter.sol` :
  where users can enter any market with native ethers if they don't have the underlying asset of the market; the router will handle the asset conversion from eth to weth and the deposit (mint), also, it enables users to redeem their mTokens and getting ethers in return.
- `MErc20.sol` :
  represents the gateway for users to interact with the market; it holds the basic functions for mTokens.
- `MToken.sol` :
  this contract is the brain of the mToken, where all the user operations are executed.
- `Comptroller.sol` :
  it is the main contract that handles all markets configurations (set by the admin), and checks if the user operations (minting,borrowing,repaying,liquidations,rewards claiming,...) are allowed or not based on the market (mToken) specifications and user state.

- `MultiRewardDistributor.sol` :
  manages calculating rewards indices with time & rewards distribution to market participant, where every user is incentivized by rewards token accrued with time.

#### 3. Upgradeability

- `MErc20Delegate.sol` & `MErc20Delegator.sol` :
  where MErc20Delegator is responsible of upgrading the MToken implementation by setting a new implementation of it & MErc20Delegate receives delegated calls from the MErc20Delegator.

- `Unitroller.sol` :
  it follow the UUP upgrade pattern, where the comptroller implementation address can be changed, pointing to a new comptroller with an updatable logic.

# 4. Architecture Feedback

1. Governance: in `TemporalGovernor`

   - the contract has a pausable functionality where it can be paused by the guardian or a governance proposal in cases of emergencies, also in emergency cases when the contract is paused; the guardian can directly execute any proposal without queuing it.
   - in normal cases: VAA proposals can be executed by any relayer as long as the proposals passed the delay period.
   - trusted senders (trusted emitters for each chain) can be set & unset only by a governance proposal , which makes it less centralization risk prone compared when being set by the owner only.

2. User flow: aside from MErc20 delagtion; users have two options to interact with the market at first: either by depositing underlying market tokens by directly interacting with `mint` function in `MErc20` contract , or by depositing native tokens (ethers) in the `WETHRouter` contract to enter a specific market without having it's underlying asset as the router will handle the whole operation for the user:
   this is a simple abstract for the user flow when interacting with the market,the flow is similar for redeem,but for borrow,repay,liquidate & rewards claim; the user must directly interact with `MErc20` as the `WETHRouter` can only handle depositing(mint) & redeem:
   ![User Flow](https://drive.google.com/uc?id=1J_Fa_qDBMbK2U0egZe3oPXcDHH0oQK4H)

# 5. Centralization Risks

#### `MErc20` admin privilages:

1. Sweeping accidental ERC-20 transfers to this contract: no risks as the admin can't sweep the underlying tokens.
2. Setting a new admin for the market.
3. Setting a new comptroller: a risk if the newly setted comptroller is malicious or contains a flawed logic,as this will wreck the market since this contract is responsible of almost verifying each user operation.
4. Reduce reserves: there is a slight risk as there is no criteria or check to when the admin is allowed to reduce the reserves of te market,any malicious admin can exploit it to rug-pull the market.

#### `Comptroller` admin privilages:

1. Setting price oracles,setting vital market variables as closeFactor,collateralFactor & liquidation incentive.
2. Adding markets,setting markets borrow & market supply caps.
3. Setting a new rewards distributor: a risk if the newly setted implementation is malicious or contains a flawed logic,as this will wreck the rewards calculation logic and distribution.
4. Pause/unpause the contract in cases of emergencies
5. Rescue funds: here lies a big risk as there is no criteria or check to when the admin is allowed to transfer all the contract tokens balances,any malicious admin can exploit it to rug-pull the market.
6. The admin of the comptroller is the same admin of the `MultiRewardsDistributor` contract: privilaged to set rewards emission configuration and caps,updating user and markets indicies for rewards calculation & rescue funds which has the same risk explained in point#5 above.

# 6. Systemic Risks

- The codebase is robust and follows the security best practices and implementations in terms of aggressively checking of almost every input parameter and market configuration set values.
- Some low vulnerabilites were detected regarding chainlink oracle contract design.
- Some vulnerabilities to the users were detected when the market admin changes some vital market parameters without giving a grace period to the users to enhance their positions (as changing the collateral factor).
- A vulnerability in the `TemporalGovernor` contract was detected because it's un-payable contract, so proposals with values to send can't be executed.

# 7. Other Recommendations

1. In `TemporalGovernor`: revoking the guardian role is dangerous as the contract will never be pausable again in the cases of emergencies; so it's recommended to preserve this role instead of revoking it permanently.

2. In `Comptroller`: Changing a vital markets configuration parameters as collateral factor without implementing a grace period can harm the user s positions and expose them to liquidations; so considering implementing a grace period when setting such a critical configurations.
   Considering Moonwell's commitment to security, regular third-party security audits are advisable to identify and remediate any hidden vulnerabilities. Additionally, providing detailed and extensive documentation for the smart contracts, user interactions, and governance processes will promote community understanding and foster a robust user base.

# 8. Time Spent

Approximately 45 hours ; divided between manually reviewing the codebase, reading documentation, foundry testing, and documenting my findings.


### Time spent:
45 hours