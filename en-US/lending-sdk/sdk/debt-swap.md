# Debt swap

Continuing from [#id-4.-select-an-use-case](./#id-4.-select-an-use-case "mention").

Debt swap enables users to replace a borrowed asset with another in a single step by using a flash loan.

* Source token: the debt token to be swapped from.
* Destination token: the debt token to be swapped for.

## 5. Preview the estimated post-debt-swap portfolio

By specifying the source token, the source token amount, and the destination token, the function will return the destination token amount, the updated user portfolio, and the logics to be executed.

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

The logic should include:

1. Borrow a flash loan of the destination token
2. Swap the destination token to the source token
3. Repay the debt with the source token
4. Borrow the dest token
5. Repay the flash loan with the destination token

## 6. Obtain the required approval permission and send the router transaction

To perform the logics, certain approvals need to be processed. You may refer to [estimate-router-data.md](../../protocolink-sdk/estimate-router-data.md "mention") and [send-router-transaction.md](../../protocolink-sdk/send-router-transaction.md "mention") for more details.

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
