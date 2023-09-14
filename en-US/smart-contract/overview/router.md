---
description: >-
  This is the  single entry point for users to interact with when executing
  transactions. The Router forwards user transactions to respective Agents.
---

# Router

The **Router** provides users with a single entry point for conducting transaction operations in an intuitive and concise manner. Within the **Router**, users can execute transactions by calling the `execute()` function of the Router contract.

{% code title="src/Router.sol" %}
```solidity
function execute(
    IParam.Logic[] calldata logics,
    address[] calldata tokensReturn,
    uint256 referralCode
) external payable {}
```
{% endcode %}

#### **execute() Function**

The `execute()` function serves as the core interface for users to execute transactions and requires three parameters:

* **`IParam.Logic[] logics`**: The `logics` parameter provided by the user should be an array of type `IParam.Logic` containing all the logic operations to be executed (such as transaction-related information like `to` and `data`). Detailed explanations regarding `IParam.Logic` will be provided in subsequent sections.
* **`address[] tokensReturn`**: Users must provide an array of type `address` as the `tokensReturn` parameter to indicate all the tokens expected to be returned to the user's account.
* **`uint256 referralCode`**: Currently, this parameter has no actual function and is only reserved. Users can pass in **`0`** directly.

At the same time, users can also transfer ether to the Router through the `msg.value` parameter to complete the corresponding operations.

### IParam.Logic

{% code title="src/interfaces/IParam.sol" %}
```solidity
struct Logic {
    address to;
    bytes data;
    Input[] inputs;
    WrapMode wrapMode;
    address approveTo;
    address callback;
}

struct Input {
    address token;
    uint256 balanceBps;
    uint256 amountOrOffset;
}

enum WrapMode {
    NONE,
    WRAP_BEFORE, 
    UNWRAP_AFTER
}
```
{% endcode %}

#### Logic Data Structure:

* **`address to`**: The address of the contract with which to interact.
* **`bytes data`**: The transaction data to execute.
* **`Input[] inputs`**: The token addresses and amounts expected to be used in this transaction.
* **`WrapMode wrapMode`**: Specifies whether the native token needs to be wrapped (e.g., ETH to WETH). If wrapping is necessary, set it to `WRAP_BEFORE`. If unwrapping is necessary after the action is executed, set it to `UNWRAP_AFTER`. If wrapping is not needed, fill in `NONE`.
* **`address approveTo`**: Authorize the contract address that is not the `to` contract, such as [Paraswap TokenTransferProxy](https://etherscan.io/address/0x216b4b4ba9f3e719726886d34a177484278bfcae#code). If the approved contract address is the same as the `to` contract address, fill in the `approveTo` field with `address(0)`.
* **`address callback`**: For flash loan cases, a callback function is required to be provided for the FlashLoan contract to call and execute subsequent operations.

#### Input Data Structure:

* **`address token`**: The address of the input token.
* **`uint256 balanceBps`**: `7_000` represents 70% of the token balance as the replacement amount. If you want to skip the bps calculation, fill `type(uint256).max`, and the **`amountOrOffset`** will be used as the amount directly.
*   **`uint256 amountOrOffset`**: If `amountBps` is omitted, the `amountOrOffset` field represents the actual amount of the trade. However, if `amountBps` is specified, the `amountOrOffset` field represents the byte offset of the amount value in `Logic.data` for replacement. If replacement of the amount value is not required, set `amountOrOffset` to `type(uint256).max` to skip it.

    \
