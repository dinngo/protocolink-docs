---
description: >-
  The single entry point for users to interact with. Router forwards the
  calldata to an Agent when executing a transaction.
---

# Router

**Router** provides users with a single entry point for conducting transaction operations in an intuitive and concise manner. Within the **Router**, users can execute transactions by calling the [`execute()`](https://github.com/dinngo/protocolink-contract/blob/5a243c64a27b4cbae3a2ff5b8595cfcc146c6a14/src/Router.sol#L234) function of the Router contract.

```solidity
function execute(
    bytes[] calldata permit2Datas,
    DataType.Logic[] calldata logics,
    address[] calldata tokensReturn,
) external payable {}
```

### **Execute Transactions**&#x20;

The `execute()` function serves as the core interface for users to execute transactions. It requires three parameters:

* **`bytes[] calldata permit2Datas`**: The Permit2 calldata for pulling tokens from a user.
* **`DataType.Logic[] logics`**: The operations for Protocolink to interact with other protocols. More details can be found at [`DataType.Logic`](router.md#datatype.logic)
* **`address[] tokensReturn`**: The expected tokens back to a user at the end of a transaction.

Users are able to operate their native tokens through the Router by using `msg.value` as well.

#### DataType.Logic

{% code fullWidth="false" %}
```solidity
enum WrapMode {
    NONE,
    WRAP_BEFORE, 
    UNWRAP_AFTER
}

struct Logic {
    address to;
    bytes data;
    Input[] inputs;
    WrapMode wrapMode;
    address approveTo;
    address callback;
}
```
{% endcode %}

#### Logic Data Structure:

* **`address to`**: The address of the contract with which to interact.
* **`bytes data`**: The transaction data to execute.
* **`Input[] inputs`**: The token addresses and amounts expected to be used in this transaction.
* **`WrapMode wrapMode`**: Specifies whether the native tokens need to be wrapped (e.g., ETH to WETH). If the native tokens need to be wrapped, set it to `WRAP_BEFORE`. If the tokens need to be unwrapped after the transaction data is executed, set it to `UNWRAP_AFTER`. Otherwise, set it to `NONE`.
* **`address approveTo`**: Authorize a contract address, such as [Paraswap TokenTransferProxy](https://etherscan.io/address/0x216b4b4ba9f3e719726886d34a177484278bfcae#code), that is not equal to the `to` address. If the approveTo address equals to the `to` address, fill in the `approveTo` address with `address(0)`.
* **`address callback`**: The callback address is provided to a flash loan contract to execute subsequent operations.

```solidity
struct Input {
    address token;
    uint256 balanceBps;
    uint256 amountOrOffset;
}
```

#### Input Data Structure:

* **`address token`**: The address of the input token.
* **`uint256 balanceBps`**: The value represents how much percentage of the token balance will be used. The base is `10_000` which means `7_000` represents 70% of the token balance. If you want to use a fixed amount instead, set this value to `0`.
*   **`uint256 amountOrOffset`**: The value represents the fixed token amount in use when `balanceBps` is set to `0`. Otherwise, it represents the byte offset for replacing the amount value in the `Logic.data`. If the amount value does not need to be replaced, set `amountOrOffset` to [`1<<255`](https://github.com/dinngo/protocolink-contract/blob/5a243c64a27b4cbae3a2ff5b8595cfcc146c6a14/src/AgentImplementation.sol#L44).



### Delegation

Users (aka. delegators) are allowed to grant permissions to other users called delegatees. A delegatee is able to send transactions on a delegator's behalf.

To delegate transactions, users need to [allow](https://github.com/dinngo/protocolink-contract/blob/5a243c64a27b4cbae3a2ff5b8595cfcc146c6a14/src/Router.sol#L413C4-L416C6) their delegatees with an expiry value. The expiry value ensures the delegation is limited within a period of time. If a user wants to revoke the permission from a delegatee before the expiry, the user needs to call the [disallow](https://github.com/dinngo/protocolink-contract/blob/5a243c64a27b4cbae3a2ff5b8595cfcc146c6a14/src/Router.sol#L442) function.\


If users do not require a standalone delegator, they can provide a signature instead. [Signature](https://github.com/dinngo/protocolink-contract/blob/5a243c64a27b4cbae3a2ff5b8595cfcc146c6a14/src/libraries/DataType.sol#L30) includes a `nonce` and a `deadline` to ensure it can only be used once in a period of time. Now, anyone can call [`executeBySig()`](https://github.com/dinngo/protocolink-contract/blob/5a243c64a27b4cbae3a2ff5b8595cfcc146c6a14/src/Router.sol#L263) to send transactions on behalf of the signer.
