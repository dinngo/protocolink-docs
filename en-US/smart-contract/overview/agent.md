---
description: >-
  This is execution unit of user transactions. The token approvals are securely
  held since one Agent is exclusive to one user only.
---

# Agent

In Protocolink, each user has their own dedicated `Agent` to execute transactions. This design significantly reduces the risk of being attacked, as the corresponding user can only use each `Agent`. When a user conducts a transaction with the `Router` contract for the first time, the system checks whether the corresponding `Agent` exists. If not, a new `Agent` is automatically created during the user's first transaction, eliminating the need for the user to initiate an additional transaction to set up the `Agent`.

Even though each user has their own dedicated Agent contract in Protocolink, they still need to interact with the system through the Router contract. The Router serves as the only entry point for users to execute transactions and interact with Protocolink.

In some cases, it may be necessary to grant the `Agent` certain permissions before executing a transaction through the `Router`, such as: transferring tokens using the Uniswap Permit2 or borrowing tokens from AAVE through Protocolink.

#### Calculate Agent Address

To facilitate this process, we provide the **`calcAgent(address)`** function, which enables users to obtain the corresponding Agent contract address in advance. This function utilizes the `create2` opcode to generate the Agent contract address based on the user's address. By knowing the Agent contract address ahead of time, users can grant the necessary permissions and prepare for transaction execution through the Router.

{% code title="src/Router.sol" %}
```solidity
function calcAgent(address owner_) external view returns (address) {}
```
{% endcode %}

This function has the following parameter:

* **`owner_`**: an `address` type, used to query the Agent address associated with the `owner_`.
