---
description: The structures and enumerations used in Protocolink contracts.
---

# Data Type

#### [WrapMode](https://github.com/dinngo/protocolink-contract/blob/5a243c64a27b4cbae3a2ff5b8595cfcc146c6a14/src/libraries/DataType.sol#L6)

```solidity
enum WrapMode {
    NONE,
    WRAP_BEFORE,
    UNWRAP_AFTER
}
```

* **`NONE`**: Do not wrap the native token when Protocolink executes the protocol operation.
* **`WRAP_BEFORE`**: Wrap the native token before Protocolink executes the protocol operation.
* **`UNWRAP_AFTER`**: Unwrap the token after Protocolink executes the protocol operation.

#### [Input](https://github.com/dinngo/protocolink-contract/blob/5a243c64a27b4cbae3a2ff5b8595cfcc146c6a14/src/libraries/DataType.sol#L13)

```solidity
struct Input {
    address token;
    uint256 balanceBps;
    uint256 amountOrOffset;
}
```

* **`token`**: The address of the input token.
* **`balanceBps`**: The value represents how much percentage of the token balance will be used. The base is `10_000` which means `7_000` represents 70% of the token balance. If you want to use a fixed amount instead, set this value to `0`.
* **`amountOrOffset`**: The value represents the fixed token amount in use when `balanceBps` is set to `0`. Otherwise, it represents the byte offset for replacing the amount value in the `Logic.data`. If the amount value does not need to be replaced, set `amountOrOffset` to [`1<<255`](https://github.com/dinngo/protocolink-contract/blob/5a243c64a27b4cbae3a2ff5b8595cfcc146c6a14/src/AgentImplementation.sol#L44).

#### [Logic](https://github.com/dinngo/protocolink-contract/blob/4b765ea9da53fc02b4bce890676cf080206fd00e/src/libraries/DataType.sol#L20)

{% code fullWidth="false" %}
```solidity
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

* **`to`**: The contract address that Protocolink interacts with.
* **`data`**: The transaction data to execute.
* **`inputs`**: The token addresses and amounts expected to be used in the transaction.
* **`wrapMode`**: Specifies whether the native tokens need to be wrapped (e.g., ETH to WETH). If the native tokens need to be wrapped, set it to `WRAP_BEFORE`. If the tokens need to be unwrapped after the transaction data is executed, set it to `UNWRAP_AFTER`. Otherwise, set it to `NONE`.
* **`approveTo`**: Authorize a contract address, such as [Paraswap TokenTransferProxy](https://etherscan.io/address/0x216b4b4ba9f3e719726886d34a177484278bfcae#code), that does not equal the `to` address. If the `approveTo` address equals the `to` address, fill in the `approveTo` address with `address(0)`.
* **`callback`**: The callback address is provided to a flash loan contract to execute subsequent operations.

#### [ExecutionDetails](https://github.com/dinngo/protocolink-contract/blob/c8743edc492bf7a25bbc8a0f55befb148e687a38/src/libraries/DataType.sol#L30)

```solidity
struct ExecutionDetails {
    bytes[] permit2Datas;
    Logic[] logics;
    address[] tokensReturn;
    uint256 nonce;
    uint256 deadline;
}
```

* **`permit2Datas`**: The Permit2 data for pulling tokens from the user address.
* **`logics`**: The operations for Protocolink to interact with other protocols.
* **`tokensReturn`**: The specified tokens should be sent back to the user address at the end of the transaction.
* **`nonce`**: A one-time used value to prevent signature replay attack.
* **`deadline`**: The signature expiration time.

#### [Fee](https://github.com/dinngo/protocolink-contract/blob/c8743edc492bf7a25bbc8a0f55befb148e687a38/src/libraries/DataType.sol#L39)

```solidity
struct Fee {
    address token;
    uint256 amount;
    bytes32 metadata;
}
```

* **`token`**: The fee token address.
* **`amount`**: The fee amount.
* **`metadata`**: The fee source that is used only for events.

#### [LogicBatch](https://github.com/dinngo/protocolink-contract/blob/5a243c64a27b4cbae3a2ff5b8595cfcc146c6a14/src/libraries/DataType.sol#L46C12-L46C22)

```solidity
struct LogicBatch {
    Logic[] logics;
    Fee[] fees;
    bytes32[] referrals;
    uint256 deadline;
}
```

* **`logics`**: The operations for Protocolink to interact with other protocols.
* **`fees`**: The fees that are charged by Protocolink.
* **`referrals`**: The receivers of the fees and the fee rates.
* **`deadline`**: The signature expiration time.

#### [ExecutionBatchDetails](https://github.com/dinngo/protocolink-contract/blob/5a243c64a27b4cbae3a2ff5b8595cfcc146c6a14/src/libraries/DataType.sol#L54)

```solidity
struct ExecutionBatchDetails {
    bytes[] permit2Datas;
    LogicBatch logicBatch;
    address[] tokensReturn;
    uint256 nonce;
    uint256 deadline;
}
```

* **`permit2Datas`**: The Permit2 data for pulling tokens from the user address.
* **`logicBatch`**: The operations for Protocolink to interact with other protocols.
* **`tokensReturn`**: The specified tokens should be sent back to the user address at the end of the transaction.
* **`nonce`**: A one-time used value to prevent signature replay attack.
* **`deadline`**: The signature expiration time.

#### [DelegationDetails](https://github.com/dinngo/protocolink-contract/blob/c8743edc492bf7a25bbc8a0f55befb148e687a38/src/libraries/DataType.sol#L63)

```solidity
struct DelegationDetails {
    address delegatee;
    uint128 expiry;
    uint128 nonce;
    uint256 deadline;
}
```

* **`delegatee`**: The delegatee address.
* **`expiry`**: The delegation expiration time.
* **`nonce`**: A one-time used value to prevent signature replay attack.
* **`deadline`**: The signature expiration time.

#### [PackedDelegation](https://github.com/dinngo/protocolink-contract/blob/c8743edc492bf7a25bbc8a0f55befb148e687a38/src/libraries/DataType.sol#L71C21-L71C21)

```solidity
struct PackedDelegation {
    uint128 expiry;
    uint128 nonce;
}
```

* **`expiry`**: The delegation expiration time.
* **`nonce`**: A one-time used value to prevent signature replay attack.
