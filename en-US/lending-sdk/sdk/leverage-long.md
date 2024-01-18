# Leverage long

Continuing from [#id-4.-select-an-use-case](./#id-4.-select-an-use-case "mention").

Leverage long enables user to achieve the desired collateral exposure in a single step using a flash loan.

* Source token: the token to be longed.
* Destination token: the token to be leveraged against.

## 5. Preview the estimated post-leverage portfolio

Specify the source token, source token amount, and the destination token that the user wants to leverage. The function will return the destination token amount, the portfolio after leveraging, and the logics to be executed.

```typescript
// User obtains a quotation for leveraging long src token
const srcToken = mainnetTokens.USDC;
const srcAmount = '1';
const destToken = mainnetTokens.WETH;
const leverageLongInfo = await adapter.leverageLong({
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
3. Deposit source token and get collateral token
4. Return collateral token to user
5. Borrow destination token
6. Repay flashloan token

## 6. Obtain the required approval permission and send router transaction

To perform the logics, certain approvals need to be processed. You may refer to [estimate-router-data.md](../../protocolink-sdk/estimate-router-data.md "mention") and [send-router-transaction.md](../../protocolink-sdk/send-router-transaction.md "mention") for more details.

```typescript
// User needs to permit the Protocolink user agent to borrow for the user
const estimateResult = await apisdk.estimateRouterData(
  { chainId, account, logics: leverageLongInfo.logics },
  permit2Type
);

// User obtains a leverage long transaction request
const routerData: apisdk.RouterData = {
  chainId,
  account,
  logics: leverageLongInfo.logics
};
const transactionRequest = await apisdk.buildRouterTransactionRequest(routerData);
```
