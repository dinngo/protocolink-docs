# Deleverage

Continuing from [#4.-select-an-intent](./#4.-select-an-intent "mention")

Leverage long enables user to reduce the collateral exposure in a single step using a flash loan to repay the borrowed asset.

* Source token: the debt token.
* Destination token: the collateral token.

## 5. Preview the estimated post-deleverage portfolio

Specify the source token, source token amount, and the destination token that the user wants to deleverage, decreasing source token debt and destination token collateral. The function will return the destination token amount, the portfolio after deleveraging, and the logics to be executed.

```typescript
// User obtains a quotation for deleveraging dest token
const srcToken = mainnetTokens.USDC;
const srcAmount = '1000';
const destToken = mainnetTokens.WBTC;
const deleverageInfo = await adapter.deleverage({
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
4. Withdraw destination token (get collateral token from user if needed)
5. Repay flashloan token

## 6. Obtain the required approval permission and send router transaction

To perform the logics, certain approvals need to be processed. You may refer to [estimate-router-data.md](../../integrate-js-sdk/how-to-use/estimate-router-data.md "mention") and [send-router-transaction.md](../../integrate-js-sdk/how-to-use/send-router-transaction.md "mention") for more details.

```typescript
// User needs to permit the Protocolink user agent to borrow for the user
const estimateResult = await apisdk.estimateRouterData(
  { chainId, account, logics: deleverageInfo.logics },
  permit2Type
);

// User obtains a deleverage transaction request
const routerData: apisdk.RouterData = {
  chainId,
  account,
  logics: deleverageInfo.logics
};
const transactionRequest = await apisdk.buildRouterTransactionRequest(routerData);
```
