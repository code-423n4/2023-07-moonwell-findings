1] Missing zero address check can send rewards to zero address.

MultiRewardDistributor.sol is missing a zero address check for   payable _user address, which could maybe allow rewards sent to be mistakenly tranfered to 0 address.

proof of concept

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L1215

Recommended Mitigation Steps

Add a require() check for zero address for the  payable _user address 


2] User can withdraw  more than their reward tokens in MultiRewardDistributor.sendreward

Here is the issue,'sendreward' function does not check the total rewards they owe users, therefore user can internally call the 'sendreward' function and if the distributer has enough balance,user can withdraw more than their calculated rewards.
https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L1237

The function 'sendrewards' first checks if there is anything to give out else it returns amount. It then continues to check if  pause guardian is active, then  gets the address  of reward tokens. It gets the distributor's current balance and  only transfer out if their is  enough balance to cover it otherwise it just accrues without sending. Meaning when there is enough balance it will just send the given amount.
.

 proof of concept
https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L1237
 The function does not check the amount they owe an address.
 User can internally call the function with their address with more amount of rewards than their calculated rewards.

Recommended Mitigation Steps
Add check to confirm the amount the distributer owes a user.


 3]Loss of rewards when user exits market with pending rewards in comptroller.sol 

Here is the issue,it has no check to confirm if the user has some accrued rewards before exiting market therefore the user will lose their rewards if they exit the market with accrued rewards. They wont be allowed to withdraw since their markets will read 'not listed or inactive' in 'https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Comptroller.sol#L1035'.

The 'exitmarket' function checks if user has a borrow balance then also fails if the sender is not permitted to redeem all of their tokens.It does checks to confirm if sender is in market then deletes mtoken account membership.
https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Comptroller.sol#L154

proof of concept
For a user to get their rewards in claimreward.sol,it checks if the market is active but the user may have some accrued rewards but user will have already exited the market so the user's market will read 'not listed' therefore their rewards will be locked.
https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Comptroller.sol#L1028

Recommended Mitigation Steps
 Provide a check to confirm there is no accrued rewards pending before a user exits market.


4] When reward speed is non zero value, user can 'entermarket', get their rewards then immediately withdraw their asset, therefore their assets wont bring any value to the markets liquidity.

 Here is the issue,the function 'exitmarket' has no timestamp of how long a user has to be in market to exit or receive rewards therefore users can entermarket to receive rewards then leave. It only checks the tokenheld and the borrow balance. It will be a loss for the 
 reward disbributer since the users market will not have provided liquidity but will receive rewards when reward speed is non zero.

In 'comptroller.sol',users can 'entermarket' which they get their market listed,therefore they become eligible for a supplier reward .claimrewards disburse rewards as long as the market is listed. User can enter market then get the rewards and 'exitmarket' when reward speed in non zero value hence benefiting the market with no liquidity but receiving the emission tokens as a supplier when there is enough balance in the reward disributer.
 https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Comptroller.sol#L1041
 
 proof of concept
https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Comptroller.sol#L1041

User can get their rewards disbursed being simply getting their markets listed,get rewards then leave.

Recommended Mitigation Steps
Add timestamp to when a user can collect their rewards according to the amount of time they have been in the market.

