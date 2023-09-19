# 🧑💻 Security Review Details

## Scope

* GitHub Repository: [https://github.com/dinngo/protocolink-contract](https://github.com/dinngo/protocolink-contract)
* `src/` (except for `src/interfaces/`)

## Contract Details

**Router**

* Users can use only their respective Agent without others permission and an Agent is dedicated to one user.
* The Router has two privileged roles, `owner` and `pauser`.
* The **Execution-related** functions in the Router
  * are the only ways to reach the Agent.
  * should not be re-entered.

**Agent**

* The `execute` function and the `executeWithSignerFee()` function can only be called by the Router.
* The `initialize()` function is required upon creation and can only be called once.
* The callback is allowed to re-enter the Agent via the `executeByCallback()` function once only if the callback address is provided in the `logic`.
* Users receive refunds only if their token balances on Agent exceed the `DUST` threshold. The `DUST` threshold does not consider token decimals.
* If users fail to provide the correct `tokensReturn`, tokens stay in their Agent. To withdraw the tokens,  users need to send another transaction.

**Callback**

* Callback can be deployed by anyone.
* If a callback re-enters an Agent with malicious `logics`, the Agent's token approvals would be abused.

**Utility**

* Utility can be deployed by anyone.
* Utility is used for scenarios that the Agent cannot handle directly. For example, the Agent transfers the ID back to the user through Utility after minting Maker CDPs.

## Security Assumptions

* The addresses provided to the Router should be trustworthy, including `to`, `callback`, and `token` within the `logics`. If malicious contracts are included in `to`, `callback`, or `token`, only the user initiating the transaction should be affected. Even though the `to` (e.g., a Uniswap Router address) in `logics` is trustworthy, it is important to note that the execution would trigger other malicious code (e.g., a malicious `token.transfer()` in a swap process). Measures should be in place to block attacks such as transferring assets temporarily held by the Agent or abusing token approvals on the Agent.
* Owner should be trustworthy since the owner is able to call privilege functions like `setFeeRate()`.&#x20;
* Delegatee should be trustworthy since the delegatee is able to send transactions on behalf of its users.

## Areas of Concern

The team's concerns with the contracts are around

**Router**

* Would the Router go into an unexpected state and act unexpectedly?
* Would reentrancy be possible when executing a transaction?
* Would the user be directed to an unexpected Agent?
* Could an attacker execute transactions on a user’s Agent without permission?
* Could the signature be forged?

**Agent**

* Would tokens be stuck in the Agent forever?
* Would the Agent execute any `logics` that is not provided by a user?
* Would a callback re-enters the Agent more than once?
