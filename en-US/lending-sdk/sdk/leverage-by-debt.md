# Leverage By Debt

Continuing from [#id-4.-select-an-use-case](./#id-4.-select-an-use-case "mention").

Leverage By Debt enables users to achieve the desired debt exposure in a single step by using a flash loan.

* Source token: the debt token to be leveraged.
* Destination token: the collateral token to be leveraged against.

## 5. Preview the estimated post-leverage portfolio

Specify the source token, source token amount, and the destination token that the user wants to leverage. The function will return the deposited destination token amount, the updated user portfolio, and the logics to be executed.

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

The logics should include:

1. Borrow a flash loan of the source token
2. Swap source token for destination token
3. Deposit the destination token and get the protocol destination token (aToken)
4. Return the protocol destination token (aToken) to the user
5. Borrow the source token
6. Repay the flash loan with the source token

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
