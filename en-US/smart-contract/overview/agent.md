---
description: >-
  The execution unit of user transactions. The Agent securely holds token
  approvals for its exclusive user, and it is non-upgradable.
---

# Agent

### Execute Transactions

When an Agent is called by the Router, it receives the protocol operations specified in the `logics`. Since the token amount used in each operation has to be set before the execution, how do users know the precise value (say operation B consumes the return token of operation A) considering the fluctuating blockchain states? To solve this problem, Protocolink offers a mechanism called **BalanceLink**.

```solidity
function _executeLogics(
    DataType.Logic[] calldata logics,
    bool chargeOnCallback
) internal {}
```

#### BalanceLink

BalanceLink allows users to set **will-be-replaced** token amounts in the `data`. To deal with the will-be-replaced token amount, the Agent reads the `balanceBps` and the `amountOrOffset` from the [#input](data-type.md#input "mention") included in the `logics`:

* The `balanceBps` is for the Agent to calculate the real amount with its token balance.
* The `amountOrOffset` is used as the offset by the Agent to replace the will-be-replaced token amount from the `data`.

After replacing the token amount, the Agent is ready to execute the operation without knowing how many tokens it got from the prior operation.

{% hint style="info" %}
If the token amount is sent via `msg.value`, the `amountOrOffset` needs to be specified as [`1<<255`](https://github.com/dinngo/protocolink-contract/blob/5a243c64a27b4cbae3a2ff5b8595cfcc146c6a14/src/AgentImplementation.sol#L44) to skip the replacement.
{% endhint %}

BalanceLink also allows users to set fixed token amounts in the `data` directly. To help the Agent know the token amounts in the `data` do not need to be replaced, users need to set the `balanceBps` to 0 and the `amountOrOffset` to the corresponding fixed token amount. When the Agent finds the `balanceBps` value as zero, it executes the operation with the `data` directly.

### Create a New Agent

In Protocolink, one Agent is dedicated to one user. When a user conducts the very first transaction, the Router checks whether the corresponding Agent exists. If the Agent does not exist, the Router creates a new one in the transaction, eliminating the need to create it in another transaction.

```solidity
function _execute(
    address user,
    bytes[] calldata permit2Datas,
    DataType.Logic[] calldata logics,
    address[] calldata tokensReturn
) internal {
    IAgent agent = agents[user];

    if (address(agent) == address(0)) {
        agent = IAgent(_newAgent(user));
    }

    emit Executed(user, address(agent));
    agent.execute{value: msg.value}(permit2Datas, logics, tokensReturn);
}

```

When creating an Agent, the Router requires the user address as salt and saves the relation between the user and the Agent in the map. By having a dedicated Agent, users no longer need to worry about the risk of placing token approvals on a shared one.

```solidity
function _newAgent(address user) internal returns (address) {
    IAgent agent = IAgent(address(new Agent{salt: bytes32(bytes20(user))}(agentImplementation)));
    agents[user] = agent;
    emit AgentCreated(address(agent), user);
    return address(agent);
}
```

### Grant Agent Permissions

In certain scenarios, users must grant their Agent certain permissions, such as setting token allowances in Permit2 before sending transactions.

### Calculate Agent Address

To grant permissions to the right Agent, users can use the **`calcAgent(address)`** function to obtain the Agent address before executing a transaction.

```solidity
function calcAgent(address user) external view returns (address) {
    address result = address(
        uint160(
            uint256(
                keccak256(
                    abi.encodePacked(
                        bytes1(0xff),
                        address(this),
                        bytes32(bytes20(user)),
                        keccak256(abi.encodePacked(type(Agent).creationCode, abi.encode(agentImplementation)))
                    )
                )
            )
        )
    );
    return result;
}
```

This function utilizes the [CREATE2](https://eips.ethereum.org/EIPS/eip-1014) opcode. It requires a user address that is used to query the corresponding Agent address.
