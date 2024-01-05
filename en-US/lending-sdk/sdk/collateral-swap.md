# Collateral swap

Continuing from [#4.-select-an-intent](./#4.-select-an-intent "mention")

Collateral swap enables user to replace one collateral asset with another in a single step using a flash loan.

* Source token: the collateral to be swapped.
* Destination token: the collateral to be swapped to.

## 5. Preview the estimated post-collateral-swap portfolio

Specify the source token, source token amount, and the destination token that the user wants to swap. The function will return the destination token amount, the portfolio after swapping, and the logics to be executed.

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

The logic should be composed by steps including:

1. Flashloan source token
2. Swap source token to destination token
3. Deposit destination token
4. Withdraw destination token (return collateralized destination token and get collateralized source token from user if needed)
5. Repay flashloan token

## 6. Obtain the required approval permission and send router transaction

To perform the logics, certain approvals need to be processed. You may refer to [estimate-router-data.md](../../integrate-js-sdk/how-to-use/estimate-router-data.md "mention") and [send-router-transaction.md](../../integrate-js-sdk/how-to-use/send-router-transaction.md "mention") for more details.

```typescript
// User needs to permit the Protocolink user agent to borrow for the user
const estimateResult = await apisdk.estimateRouterData(
  { chainId, account, logics: collateralSwapInfo.logics },
  permit2Type
);

// User obtains a collateral swap transaction request
const routerData: apisdk.RouterData = {
  chainId,
  account,
  logics: collateralSwapInfo.logics
};
const transactionRequest = await apisdk.buildRouterTransactionRequest(routerData);
```
