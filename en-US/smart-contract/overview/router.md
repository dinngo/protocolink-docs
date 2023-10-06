---
description: >-
  The single entry point for users to interact with. The Router forwards the
  data to an Agent when executing a transaction.
---

# Router

### **Execute Transactions**

The Router provides a single entry point for users to operate protocols in an intuitive and concise manner. Within the Router, users can execute operations by calling the `execute()` function.

```solidity
function execute(
    bytes[] calldata permit2Datas,
    DataType.Logic[] calldata logics,
    address[] calldata tokensReturn
) external payable {}
```

The [`execute()`](https://github.com/dinngo/protocolink-contract/blob/c8743edc492bf7a25bbc8a0f55befb148e687a38/src/AgentImplementation.sol#L77) function serves as the core interface for users to execute transactions. It requires three parameters:

* **`permit2Datas`**: The Permit2 data for pulling tokens from the user address.
* **`logics`**: The operations for Protocolink to interact with other protocols.
* **`tokensReturn`**: The specified tokens should be sent back to the user address at the end of the transaction.

{% hint style="info" %}
Native tokens should be sent by users to the Router in `msg.value.`

Protocolink charges fees in the execute() function. Check more details of the [fees.md](fees.md "mention")
{% endhint %}

### **Execute Transactions with API Data**

Though Protocolink provides the [#execute-transactions](router.md#execute-transactions "mention") for users to operate protocols in a single transaction, it would take users a bunch of time to build the parameters. To solve this issue, Protocolink provides the `executeWithSignerFee()` function.

```solidity
function executeWithSignerFee(
    bytes[] calldata permit2Datas,
    DataType.LogicBatch calldata logicBatch,
    address signer,
    bytes calldata signature,
    address[] calldata tokensReturn
) external payable {}
```

The [`executeWithSignerFee()`](https://github.com/dinngo/protocolink-contract/blob/c8743edc492bf7a25bbc8a0f55befb148e687a38/src/AgentImplementation.sol#L96) function serves as the core interface for users to execute transactions with Protocolink API data. It requires five parameters:

* **`permit2Datas`**: The Permit2 data for pulling tokens from the user address.
* **`logicBatch`**: The operations for Protocolink to interact with other protocols.
* **`signer`**: The address that signed the `logicBatch`.
* **`signature`**: The hash value of the signed `logicBatch`.
* **`tokensReturn`**: The specified tokens should be sent back to the user address at the end of the transaction.

{% hint style="info" %}
The [#logicbatch](data-type.md#logicbatch "mention") also includes other information like fees. Check more details of the [fees.md](fees.md "mention").
{% endhint %}

### Execute Transactions with Delegations

Users (aka. delegators) can delegate their Agent to other users (aka. delegatee). When an Agent is delegated, the delegatee is allowed to send transactions on the delegator's behalf by calling the `executeFor()` function.

```solidity
function executeFor(
    address user,
    bytes[] calldata permit2Datas,
    DataType.Logic[] calldata logics,
    address[] calldata tokensReturn
) external payable {}
```

The [`executeFor()`](https://github.com/dinngo/protocolink-contract/blob/4b765ea9da53fc02b4bce890676cf080206fd00e/src/Router.sol#L248) function serves as the core interface for delegatees to execute transactions. It requires four parameters:

* **`user`**: The delegator address.
* **`permit2Datas`**: The Permit2 data for pulling tokens from the user address.
* **`logics`**: The operations for Protocolink to interact with other protocols.
* **`tokensReturn`**: The specified tokens should be sent back to the user address at the end of the transaction.

To delegate their Agent, delegators need to [allow](https://github.com/dinngo/protocolink-contract/blob/5a243c64a27b4cbae3a2ff5b8595cfcc146c6a14/src/Router.sol#L413C4-L416C6) the delegatee with an expiry value. The expiry value ensures that the Agent is delegated within a period.

```solidity
function allow(address delegatee, uint128 expiry) public {
    delegations[msg.sender][delegatee].expiry = expiry;
    emit Delegated(msg.sender, delegatee, expiry);
}
```

If users want to revoke the delegation from a delegatee before the expiry, users need to provide the delegatee address to the [disallow()](https://github.com/dinngo/protocolink-contract/blob/5a243c64a27b4cbae3a2ff5b8595cfcc146c6a14/src/Router.sol#L442) function.

```solidity
function disallow(address delegatee) external {
    allow(delegatee, 0);
}
```

#### With Delegations and API Data

Same as [#execute-transactions-with-api-data](router.md#execute-transactions-with-api-data "mention"), delegatees can call the Router contract with API data by using the `executeForWithSignerFee()` function. &#x20;

```solidity
function executeForWithSignerFee(
    address user,
    bytes[] calldata permit2Datas,
    DataType.LogicBatch calldata logicBatch,
    address signer,
    bytes calldata signature,
    address[] calldata tokensReturn
) external payable {}
```

The [`executeForWithSignerFee()`](https://github.com/dinngo/protocolink-contract/blob/4b765ea9da53fc02b4bce890676cf080206fd00e/src/Router.sol#L310) function serves as the core interface for delegatees to execute transactions with API data. It requires six parameters:

* **`user`**: The delegator address.
* **`permit2Datas`**: The Permit2 data for pulling tokens from the user address.
* **`logicBatch`**: The operations for Protocolink to interact with other protocols.
* **`signer`**: The address that signed the `logicBatch`.
* **`signature`**: The hash value of the signed `logicBatch`.
* **`tokensReturn`**: The specified tokens should be sent back to the user address at the end of the transaction.

{% hint style="info" %}
The [#logicbatch](data-type.md#logicbatch "mention") also includes other information like `fees`. Check more details of the [fees.md](fees.md "mention").
{% endhint %}

### Execute Transactions with Signatures

If users do not require a standalone delegatee, they can share their signature instead. A signature is signed by a delegator and it represents the hash value of the `details`. The `details` includes a `nonce` value to ensure it can only be used once before the `deadline`. Now, anyone with a signature can call the `executeBySig()` function to send a transaction on behalf of the delegator.

```solidity
function executeBySig(
    DataType.ExecutionDetails calldata details,
    address user,
    bytes calldata signature
) external payable {}
```

The [`executeBySig()`](https://github.com/dinngo/protocolink-contract/blob/5a243c64a27b4cbae3a2ff5b8595cfcc146c6a14/src/Router.sol#L263) function serves as the core interface for anyone to execute transactions with signatures. It requires three parameters:

* **`details`**: The operations for Protocolink to interact with other protocols.
* **`user`**: The delegator address.
* **`signature`**: The hash value of the signed `logicBatch`.

{% hint style="info" %}
The [#executiondetails](data-type.md#executiondetails "mention") also includes other information like `permit2Datas`.
{% endhint %}

#### With Signatures and API Data

Same as [#execute-transactions-with-api-data](router.md#execute-transactions-with-api-data "mention"), anyone can call the Router contract with a signature and API data by using the `executeBySigWithSignerFee()` function.

```solidity
function executeBySigWithSignerFee(
    DataType.ExecutionBatchDetails calldata details,
    address user,
    bytes calldata userSignature,
    address signer,
    bytes calldata signerSignature
) external payable {}
```

The [`executeBySigWithSignerFee()`](https://github.com/dinngo/protocolink-contract/blob/4b765ea9da53fc02b4bce890676cf080206fd00e/src/Router.sol#L331) function serves as the core interface for anyone to execute a transaction with the signatures and API data. It requires five parameters:

* **`details`**: The operations for Protocolink to interact with other protocols.
* **`user`**: The delegator address.
* **`userSignature`**: The hash value of the signed `details`.
* **`signer`**: The address that signed the `logicBatch` from the `details`.
* **`signerSignature`**: The hash value of the signed `logicBatch`.

{% hint style="info" %}
The [#executionbatchdetails](data-type.md#executionbatchdetails "mention") also includes other information like `permit2Datas`.
{% endhint %}
