# Zap supply

Continuing from [#id-4.-select-an-use-case](./#id-4.-select-an-use-case "mention").

Zap supply enables users to swap any token and supply the swapped out token in one transaction.

* Source token: the user-provided token.
* Destination token: the token to be supplied to a lending protocol.

Note that the supplied token can be either liquidity or collateral, depending on the design of the lending protocol.

## 5. Preview the estimated post-zap-supply portfolio

By specifying the source token, the source token amount, and the destination token, the function will return the destination token amount, the updated user portfolio, and the logics to be executed.

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

The logic should include:

1. Swap the source token to the destination token
2. Supply the destination token

## 6. Obtain the required approval permission and send the router transaction

To perform the logics, certain approvals need to be processed. You may refer to [estimate-router-data.md](../../protocolink-sdk/estimate-router-data.md "mention") and [send-router-transaction.md](../../protocolink-sdk/send-router-transaction.md "mention") for more details.

```typescript
// User needs to permit the Protocolink user agent to borrow for the user
const estimateResult = await apisdk.estimateRouterData(
  { chainId, account, logics: zapSupplyInfo.logics }
);

// User obtains a zap supply transaction request
const routerData: apisdk.RouterData = {
  chainId,
  account,
  logics: zapSupplyInfo.logics
};
const transactionRequest = await apisdk.buildRouterTransactionRequest(routerData);
```
