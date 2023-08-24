# Two step transfer

ChainlinkOracle contract contains a transfer admin which doesn't have two step transfer ownership. When this `setAdmin` function is called with a wrong input, it will break the contract. It is recomended to use two step transfer ownership pattern, like store it in some kind of pendingAdmin variable, and confirm the new admin after the delegated new admin calling claim or accept the admin delegation. This can make sure the admin is valid.

```js
File: ChainlinkOracle.sol
173:     function setAdmin(address newAdmin) external onlyAdmin {
174:         address oldAdmin = admin;
175:         admin = newAdmin;
176:
177:         emit NewAdmin(oldAdmin, newAdmin);
178:     }
```

# Redundant check of msg.sender != address(0)

The `msg.sender == address(0)` check is wrong, it supposed to be `pendingAdmin == address(0)`. The `msg.sender`` will never be address(0)

```js
File: Unitroller.sol
109:     function _acceptAdmin() public returns (uint) {
110:         // Check caller is pendingAdmin and pendingAdmin ≠ address(0)
111:         if (msg.sender != pendingAdmin || msg.sender == address(0)) {
112:             return fail(Error.UNAUTHORIZED, FailureInfo.ACCEPT_ADMIN_PENDING_ADMIN_CHECK);
113:         }

File: MToken.sol
1168:     function _acceptAdmin() override external returns (uint) {
1169:         // Check caller is pendingAdmin and pendingAdmin ≠ address(0)
1170:         if (msg.sender != pendingAdmin || msg.sender == address(0)) {
1171:             return fail(Error.UNAUTHORIZED, FailureInfo.ACCEPT_ADMIN_PENDING_ADMIN_CHECK);
1172:         }
```
# Remove the code not effecting
The `_amount > 0` will always true because on line 1220 it already check if `_amount == 0` then return the `_amount`.
Therefore, the line 1235: `if (_amount > 0 && _amount <= currentTokenHoldings)` we can remove the check if `_amount > 0`
```js
File: MultiRewardDistributor.sol
1214:     function sendReward(
1215:         address payable _user,
1216:         uint256 _amount,
1217:         address _rewardToken
1218:     ) internal nonReentrant returns (uint256) {
1219:         // Short circuit if we don't have anything to send out
1220:         if (_amount == 0) {
1221:             return _amount;
1222:         }
1223:
1224:         // If pause guardian is active, bypass all token transfers, but still accrue to local tally
1225:         if (paused()) {
1226:             return _amount;
1227:         }
1228:
1229:         IERC20 token = IERC20(_rewardToken);
1230:
1231:         // Get the distributor's current balance
1232:         uint256 currentTokenHoldings = token.balanceOf(address(this));
1233:
1234:         // Only transfer out if we have enough of a balance to cover it (otherwise just accrue without sending)
1235:         if (_amount > 0 && _amount <= currentTokenHoldings) {
1236:             // Ensure we use SafeERC20 to revert even if the reward token isn't ERC20 compliant
1237:             token.safeTransfer(_user, _amount);
1238:             return 0;
1239:         } else {
1240:             // If we've hit here it means we weren't able to emit the reward and we should emit an event
1241:             // instead of failing.
1242:             emit InsufficientTokensToEmit(_user, _rewardToken, _amount);
1243:
1244:             // By default, return the same amount as what's left over to send, we accrue reward but don't send them out
1245:             return _amount;
1246:         }
1247:     }
```
# Inconsistent authorization check pattern
Since Comptroller use this common admin check in alot of function, it's wise to create a modifier rather than creating this `if` and `require` check mechanism.
```js
File: Comptroller.sol
881:         if (msg.sender != admin) {
882:             return fail(Error.UNAUTHORIZED, FailureInfo.SET_PAUSE_GUARDIAN_OWNER_CHECK);
883:         }

File: Comptroller.sol
863:         require(msg.sender == admin, "only admin can set supply cap guardian");
```
