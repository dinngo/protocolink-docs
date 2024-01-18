# Zap withdraw

Continuing from [#id-4.-select-an-use-case](./#id-4.-select-an-use-case "mention").

Zap withdraw enables user to withdraw then swap to any token in one transaction.

* Source token: the token to be withdrawn from lending platform.
* Destination token: the token that user receives.

## 5. Preview the estimated post-zap-withdraw portfolio

Specify the source token, source token amount, and the destination token that the user wants to withdraw. The function will return the destination token amount, the portfolio after withdraw, and the logics to be executed.

```typescript
// User obtains a quotation for zap withdraw
const srcToken = mainnetTokens.WBTC;
const srcAmount = '1';
const destToken = mainnetTokens.USDC;
const zapWithdrawInfo = await adapter.zapWithdraw({
  account,
  portfolio,
  srcToken,
  srcAmount,
  destToken
});
```

The logic should be composed by steps including:

1. Withdraw source token
2. Swap source token to destination token

## 6. Obtain the required approval permission and send router transaction

To perform the logics, certain approvals need to be processed. You may refer to [estimate-router-data.md](../../protocolink-sdk/estimate-router-data.md "mention") and [send-router-transaction.md](../../protocolink-sdk/send-router-transaction.md "mention") for more details.

```typescript
// User needs to permit the Protocolink user agent to borrow for the user
const estimateResult = await apisdk.estimateRouterData(
  { chainId, account, logics: zapWithdrawInfo.logics },
  permit2Type
);

// User obtains a zap withdraw transaction request
const routerData: apisdk.RouterData = {
  chainId,
  account,
  logics: zapWithdrawInfo.logics
};
const transactionRequest = await apisdk.buildRouterTransactionRequest(routerData);
```
