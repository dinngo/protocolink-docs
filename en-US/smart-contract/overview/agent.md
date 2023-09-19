---
description: >-
  The execution unit of user transactions. Agent securely holds token approvals
  for its exclusive user, and Agent is non-upgradable.
---

# Agent

In Protocolink, an `Agent` is dedicated to a user. When a user conducts a transaction for the first time, the `Router` checks whether the corresponding `Agent` exists. If not, a new `Agent` is created in the user's first transaction, eliminating the need for the user to initiate another transaction to set up the `Agent`. This design reduces the risk of placing token approvals on a shared `Agent`.

#### Grant Agent Permissions

In certain scenarios, users need to grant their `Agent` certain permissions, such as setting token allowances in [Permit2](https://github.com/Uniswap/permit2), before they send transactions.

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

This function utilizes the [CREATE2](https://eips.ethereum.org/EIPS/eip-1014) opcode. The parameter is explained below:

* user: An address that is used to query the corresponding `Agent` address.
