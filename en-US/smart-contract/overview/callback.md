---
description: The entry point for protocol callbacks to reenter an Agent in a transaction.
---

# Callback

Protocolink blocks reentrancy while executing transactions. However, this stops Protocolink from supporting flash loan protocols. To support flash loan protocols without sacrificing security, Protocolink employs the `Callback` which is explained below.

<figure><img src="../../.gitbook/assets/callbacks (2).png" alt=""><figcaption><p>Callback Flow</p></figcaption></figure>

To borrow a flash loan, borrowers need to provide a receiver address to the flash loan service. For Protocolink users, we provide stateless and secured callbacks specifically for this purpose. Users need to provide the callback address in the `callback` field of the [DataType.Logic](router.md#datatype.logic) when executing a transaction.

Currently, Protocolink provides the following callbacks to support flash loan services:

* [FlashLoanCallbackAaveV2](https://github.com/dinngo/protocolink-contract/blob/master/src/callbacks/AaveV2FlashLoanCallback.sol)
* [FlashLoanCallbackAaveV3](https://github.com/dinngo/protocolink-contract/blob/master/src/callbacks/AaveV3FlashLoanCallback.sol)
* [FlashLoanCallbackBalancerV2](https://github.com/dinngo/protocolink-contract/blob/master/src/callbacks/BalancerV2FlashLoanCallback.sol)

You can check how to use the AaveV2 callback in this [example](https://github.com/dinngo/protocolink-contract/blob/cba23729be7acb7fd41e72d22d6652e03f34bdc2/test/integration/AaveV2.t.sol#L126).
