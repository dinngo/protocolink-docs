# Zap repay

Continuing from [#id-4.-select-an-use-case](./#id-4.-select-an-use-case "mention").

Zap repay enables user to swap any token to repay the debt in one transaction.

* Source token: the debt token to be repaid.
* Destination token: the token to be provided by user.

## 5. Preview the estimated post-zap-repay portfolio

Specify the source token, source token amount, and the destination token that the user wants to repay. The function will return the destination token amount, the portfolio after repay, and the logics to be executed.

```typescript
// User obtains a quotation for zap repay
const srcToken = mainnetTokens.USDC;
const srcAmount = '1000';
const destToken = mainnetTokens.USDT;
const zapRepayInfo = await adapter.zapRepay({
  account,
  portfolio,
  srcToken,
  srcAmount,
  destToken
});
```

The logic should be composed by steps including:

1. Swap destination token to source token
2. Repay source token

## 6. Obtain the required approval permission and send router transaction

To perform the logics, certain approvals need to be processed. You may refer to [estimate-router-data.md](../../protocolink-sdk/estimate-router-data.md "mention") and [send-router-transaction.md](../../protocolink-sdk/send-router-transaction.md "mention") for more details.

```typescript
// User needs to permit the Protocolink user agent to borrow for the user
const estimateResult = await apisdk.estimateRouterData(
  { chainId, account, logics: zapRepayInfo.logics },
  permit2Type
);

// User obtains a zap repay transaction request
const routerData: apisdk.RouterData = {
  chainId,
  account,
  logics: zapRepayInfo.logics
};
const transactionRequest = await apisdk.buildRouterTransactionRequest(routerData);
```
