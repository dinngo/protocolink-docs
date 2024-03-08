# Zap borrow

Continuing from [#id-4.-select-an-use-case](./#id-4.-select-an-use-case "mention").

Zap borrow enables users to borrow an asset and swap it for any token in one transaction.

* Source token: the token to be borrowed from a lending protocol.
* Destination token: the token that the user receives.

## 5. Preview the estimated post-zap-borrow portfolio

By specifying the source token, the source token amount, and the destination token, the function will return the destination token amount, the updated user portfolio, and the logics to be executed.

```typescript
// User obtains a quotation for zap borrow
const srcToken = mainnetTokens.USDC;
const srcAmount = '1';
const destToken = mainnetTokens.USDT;
const zapBorrowInfo = await adapter.zapBorrow({
  account,
  portfolio,
  srcToken,
  srcAmount,
  destToken
});
```

The logic should be include:

1. Borrow the source token
2. Swap the source token to the destination token

## 6. Obtain the required approval permission and send the router transaction

To perform the logics, certain approvals need to be processed. You may refer to [estimate-router-data.md](../../protocolink-sdk/estimate-router-data.md "mention") and [send-router-transaction.md](../../protocolink-sdk/send-router-transaction.md "mention") for more details.

```typescript
// User needs to permit the Protocolink user agent to borrow for the user
const estimateResult = await apisdk.estimateRouterData(
  { chainId, account, logics: zapBorrowInfo.logics },
  permit2Type
);

// User obtains a zap borrow transaction request
const routerData: apisdk.RouterData = {
  chainId,
  account,
  logics: zapBorrowInfo.logics
};
const transactionRequest = await apisdk.buildRouterTransactionRequest(routerData);
```
