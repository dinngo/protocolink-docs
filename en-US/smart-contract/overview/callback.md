---
description: >-
  The one-time address is used for re-entering the Agent. This can only be set
  during contract execution.
---

# Callback

During FlashLoan operations, a callback contract must be provided for the FlashLoan service to call upon, which facilitates the completion of fund management and repayment. In Protocolink, we have designed state-less, one-time callback contracts specifically for these purposes. if you intend to use a callback contract, you must specify it in the `callback` field of the [`IParam.Logic`](router.md#iparam.logic) struct in advance.

Currently, we support the following:

* [FlashLoanCallbackAaveV2](https://github.com/dinngo/protocolink-contract/blob/master/src/callbacks/AaveV2FlashLoanCallback.sol)
* [FlashLoanCallbackAaveV3](https://github.com/dinngo/protocolink-contract/blob/master/src/callbacks/AaveV3FlashLoanCallback.sol)
* [FlashLoanCallbackBalancerV2](https://github.com/dinngo/protocolink-contract/blob/master/src/callbacks/BalancerV2FlashLoanCallback.sol)

For detailed sample code, please refer to [this link](https://github.com/dinngo/protocolink-contract/blob/master/test/integration/AaveV2.t.sol#L119)

<figure><img src="../../.gitbook/assets/callbacks (2).png" alt=""><figcaption></figcaption></figure>



###



