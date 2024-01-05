# Zap supply

Continuing from [#4.-select-an-intent](./#4.-select-an-intent "mention")

Zap supply enables user to swap any token to supply token in one transaction.

* Source token: the token to be provided by user.
* Destination token: the token to be supplied to lending protocol.

Note that the token being supplied can be liquidity or collateral, depending on the design of lending protocol.

## 5. Preview the estimated post-zap-supply portfolio

Specify the source token, source token amount, and the destination token that the user wants to supply. The function will return the destination token amount, the portfolio after supplying, and the logics to be executed.

```typescript
// User obtains a quotation for zap supply
const srcToken = mainnetTokens.USDC;
const srcAmount = '100';
const destToken = mainnetTokens.WBTC;
const zapSupplyInfo = await adapter.zapSupply({
  account,
  portfolio,
  srcToken,
  srcAmount,
  destToken
});
```

The logic should be composed by steps including:

1. Swap source token to destination token
2. Supply destination token

## 6. Obtain the required approval permission and send router transaction

To perform the logics, certain approvals need to be processed. You may refer to [estimate-router-data.md](../../integrate-js-sdk/how-to-use/estimate-router-data.md "mention") and [send-router-transaction.md](../../integrate-js-sdk/how-to-use/send-router-transaction.md "mention") for more details.

```typescript
// User needs to permit the Protocolink user agent to borrow for the user
const estimateResult = await apisdk.estimateRouterData(
  { chainId, account, logics: zapSupplyInfo.logics },
  permit2Type
);

// User obtains a zap supply transaction request
const routerData: apisdk.RouterData = {
  chainId,
  account,
  logics: zapSupplyInfo.logics
};
const transactionRequest = await apisdk.buildRouterTransactionRequest(routerData);
```
