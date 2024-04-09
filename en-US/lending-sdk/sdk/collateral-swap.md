# Collateral swap

Continuing from [#id-4.-select-an-use-case](./#id-4.-select-an-use-case "mention").

Collateral swap enables users to replace one collateral asset with another in a single step by using a flash loan.

* Source token: the collateral token to be swapped from.
* Destination token: the collateral token to be swapped for.

## 5. Preview the estimated post-collateral-swap portfolio

By specifying the source token, the source token amount, and the destination token, the function will return the destination token amount, the updated user portfolio, and the logics to be executed.

```typescript
// User obtains a quotation for swapping the collateral from src token to dest token
const srcToken = mainnetTokens.WBTC;
const srcAmount = '1000';
const destToken = mainnetTokens.WETH;
const collateralSwapInfo = await adapter.collateralSwap({
  account,
  portfolio,
  srcToken,
  srcAmount,
  destToken
});
```

The logic should include:

1. Borrow a flash loan of the source token
2. Swap the source token for the destination token
3. Deposit the destination token
4. Return the destination protocol token (aToken) to the user
5. Get the source protocol token (aToken) from the user
6. Withdraw the source token
7. Repay the flash loan token with the source token

## 6. Obtain the required approval permission and send the router transaction

To perform the logics, certain approvals need to be processed. You may refer to [estimate-router-data.md](../../protocolink-sdk/estimate-router-data.md "mention") and [send-router-transaction.md](../../protocolink-sdk/send-router-transaction.md "mention") for more details.

```typescript
// User needs to permit the Protocolink user agent to borrow for the user
const estimateResult = await apisdk.estimateRouterData(
  { chainId, account, logics: collateralSwapInfo.logics }
);

// User obtains a collateral swap transaction request
const routerData: apisdk.RouterData = {
  chainId,
  account,
  logics: collateralSwapInfo.logics
};
const transactionRequest = await apisdk.buildRouterTransactionRequest(routerData);
```
