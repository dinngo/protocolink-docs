# Leverage By Debt

Continuing from [#id-4.-select-an-use-case](./#id-4.-select-an-use-case "mention").

Leverage By Debt enables user to achieve the desired debt exposure in a single step using a flash loan.

* Source token: the debt token to be leveraged.
* Destination token: the collateral token to be leveraged against.

## 5. Preview the estimated post-leverage portfolio

Specify the source token, source token amount, and the destination token that the user wants to leverage. The function will return the destination token amount, the portfolio after leveraging, and the logics to be executed.

```typescript
// User obtains a quotation for leveraging by debt: src token
const srcToken = mainnetTokens.USDC;
const srcAmount = '1';
const destToken = mainnetTokens.WETH;
const leverageByDebtInfo = await adapter.leverageByDebt({
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
3. Deposit destination token and get protocol destination token (aToken)
4. Return protocol destination token (aToken) to user
5. Borrow source token
6. Repay flashloan token

## 6. Obtain the required approval permission and send router transaction

To perform the logics, certain approvals need to be processed. You may refer to [estimate-router-data.md](../../protocolink-sdk/estimate-router-data.md "mention") and [send-router-transaction.md](../../protocolink-sdk/send-router-transaction.md "mention") for more details.

```typescript
// User needs to permit the Protocolink user agent to borrow for the user
const estimateResult = await apisdk.estimateRouterData(
  { chainId, account, logics: leverageByDebtInfo.logics },
  permit2Type
);

// User obtains a leverage by debt transaction request
const routerData: apisdk.RouterData = {
  chainId,
  account,
  logics: leverageByDebtInfo.logics
};
const transactionRequest = await apisdk.buildRouterTransactionRequest(routerData);
```
