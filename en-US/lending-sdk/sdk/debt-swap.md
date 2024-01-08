# Debt swap

Continuing from [#4.-select-an-intent](./#4.-select-an-intent "mention")

Debt swap enables user to replace one loan asset with another in a single step using a flash loan.

* Source token: the debt to be swapped.
* Destination token: the debt to be swapped to.

## 5. Preview the estimated post-debt-swap portfolio

Specify the source token, source token amount, and the destination token that the user wants to swap. The function will return the destination token amount, the portfolio after swapping, and the logics to be executed.

```typescript
// User obtains a quotation for swapping the debt from src token to dest token
const srcToken = mainnetTokens.USDC;
const srcAmount = '1000';
const destToken = mainnetTokens.DAI;
const debtSwapInfo = await adapter.debtSwap({
  account,
  portfolio,
  srcToken,
  srcAmount,
  destToken
});
```

The logic should be composed by steps including:

1. Flashloan destination token
2. Swap destination token to source token
3. Repay source token
4. Borrow dest token
5. Repay flashloan token

## 6. Obtain the required approval permission and send router transaction

To perform the logics, certain approvals need to be processed. You may refer to [estimate-router-data.md](../../integrate-js-sdk/how-to-use/estimate-router-data.md "mention") and [send-router-transaction.md](../../integrate-js-sdk/how-to-use/send-router-transaction.md "mention") for more details.

```typescript
// User needs to permit the Protocolink user agent to borrow for the user
const estimateResult = await apisdk.estimateRouterData(
  { chainId, account, logics: debtSwapInfo.logics },
  permit2Type
);

// User obtains a debt swap transaction request
const routerData: apisdk.RouterData = {
  chainId,
  account,
  logics: debtSwapInfo.logics
};
const transactionRequest = await apisdk.buildRouterTransactionRequest(routerData);
```
