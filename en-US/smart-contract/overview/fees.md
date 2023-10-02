---
description: Protocolink charges fees based on the token amount and transaction type
---

# Fees

This page explains the fee mechanism in the Protocolink contracts. The overview fee structure can be found [here](../../fees.md).

Protocolink calculates the fees on-chain or off-chain depending on the user called function. When users call the Protocolink contract directly (e.g., [the `execute()` function](router.md#execute-transactions)), Protocolink calculates the fees on-chain. When users call the Protocolink contract with API data (e.g., [the `executeWithSignerFee` function](router.md#execute-transactions-with-api-data)), Protocolink charges the off-chain fee.

### On-Chain Fee

If users call the Router contract without the API data, Protocolink will calculate and charge the on-chain fee in [the `execute()` function](router.md#execute-transactions). The on-chain fee is charged from the token amounts transferred in the transaction. For ERC-20 tokens, the on-chain fee is charged in the [`_doPermit2()`](https://github.com/dinngo/protocolink-contract/blob/4b765ea9da53fc02b4bce890676cf080206fd00e/src/AgentImplementation.sol#L138) function, and for native tokens, the on-chain fee is charged in the [`_chargeByMsgValue()`](https://github.com/dinngo/protocolink-contract/blob/4b765ea9da53fc02b4bce890676cf080206fd00e/src/AgentImplementation.sol#L293) function. Protocolink charges the fee from the Agent.

```solidity
function execute(
    bytes[] calldata permit2Datas,
    DataType.Logic[] calldata logics,
    address[] calldata tokensReturn
) external payable {
    if (msg.sender != router) revert NotRouter();
    _doPermit2(permit2Datas, true);
    _chargeByMsgValue();
    _executeLogics(logics, true);
    _returnTokens(tokensReturn);
}
```

Protocolink also charges the flash loan fee before returning the control flow back to flash loan services. You can find out more details in the [AaveV3FlashloanCallback](https://github.com/dinngo/protocolink-contract/blob/c8743edc492bf7a25bbc8a0f55befb148e687a38/src/callbacks/AaveV3FlashLoanCallback.sol#L73C23-L73C23).

### Off-Chain Fee

If users call the Router contract with API data, Protocolink will charge the off-chain fee in [the `executeWithSignerFee()` function](router.md#execute-transactions-with-api-data). The fees are calculated in Protocolink's API server off-chain and are charged by token types. If the token is an ERC-20 token, the fee is charged from the user address. If the token is a native token, the fee is charged from the Agent address.

```solidity
function executeWithSignerFee(
    bytes[] calldata permit2Datas,
    DataType.Logic[] calldata logics,
    DataType.Fee[] calldata fees,
    bytes32[] calldata referrals,
    address[] calldata tokensReturn
) external payable {
    if (msg.sender != router) revert NotRouter();
    _doPermit2(permit2Datas, false);
    for (uint256 i; i < referrals.length; ) {
        _charge(fees, referrals[i], false);
        unchecked {
            ++i;
        }
    }
    _executeLogics(logics, false);
    _returnTokens(tokensReturn);
}
```
