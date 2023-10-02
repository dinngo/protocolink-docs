---
description: >-
  The execution unit of user transactions. The Agent securely holds token
  approvals for its exclusive user, and it is non-upgradable.
---

# Agent

#### Create a New Agent

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

#### Grant Agent Permissions

In certain scenarios, users must grant their Agent certain permissions, such as setting token allowances in Permit2 before sending transactions.

#### Calculate Agent Address

To grant permissions to the right `Agent`, users can use the **`calcAgent(address)`** function to obtain the `Agent` address before executing a transaction.

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

This function utilizes the [CREATE2](https://eips.ethereum.org/EIPS/eip-1014) opcode. It requires a user address that is used to query the corresponding `Agent` address.
