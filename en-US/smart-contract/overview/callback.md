---
description: The entry point for protocol callbacks to reenter an Agent in a transaction.
---

# Callback

Protocolink blocks reentrancy in the middle of a transaction for security reasons. However, blocking reentrancy stops Protocolink from supporting flash loan protocols. To support them without sacrificing security, Protocolink employs the Callback which is shown below.

<figure><img src="../../.gitbook/assets/callbacks (2).png" alt=""><figcaption><p>Callback Call Flow</p></figcaption></figure>

The Callback plays the receiver role to flash loan services. When users try to borrow a flash loan within Protocolink, they must provide the corresponding callback address in the [callback field](https://github.com/dinngo/protocolink-contract/blob/4b765ea9da53fc02b4bce890676cf080206fd00e/src/libraries/DataType.sol#L26).

Currently, the supported callbacks are:

* [AaveV2FlashLoanCallback](https://github.com/dinngo/protocolink-contract/blob/master/src/callbacks/AaveV2FlashLoanCallback.sol)
* [AaveV3FlashLoanCallback](https://github.com/dinngo/protocolink-contract/blob/master/src/callbacks/AaveV3FlashLoanCallback.sol)
* [BalancerV2FlashLoanCallback](https://github.com/dinngo/protocolink-contract/blob/master/src/callbacks/BalancerV2FlashLoanCallback.sol)
* [RadiantV2FlashLoanCallback](https://github.com/dinngo/protocolink-contract/blob/master/src/callbacks/RadiantV2FlashLoanCallback.sol)

Check how the AaveV2 callback is used in this [example](https://github.com/dinngo/protocolink-contract/blob/cba23729be7acb7fd41e72d22d6652e03f34bdc2/test/integration/AaveV2.t.sol#L126).
