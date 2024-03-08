# Deleverage

Continuing from [#id-4.-select-an-use-case](./#id-4.-select-an-use-case "mention").

Deleverage enables users to reduce collateral exposure in a single step by using a flash loan to repay the borrowed asset.

* Source token: the debt token to be repaid.
* Destination token: the collateral token to be withdrawn.

## 5. Preview the estimated post-deleverage portfolio

By specifying the source token, the source token amount, and the destination token that the user wants to deleverage, the function will return the withdrawn destination token amount, the updated user portfolio, and the logics to be executed.

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

1. Borrow a flash loan of the destination token
2. Swap the destination token for the source token
3. Repay the debt with the source token
4. Get the protocol token (aToken) from the user
5. Withdraw the destination token
6. Repay the flash loan with the destination token

## 6. Obtain the required approval permission and send router transaction

To perform the logics, certain approvals need to be processed. You may refer to [estimate-router-data.md](../../protocolink-sdk/estimate-router-data.md "mention") and [send-router-transaction.md](../../protocolink-sdk/send-router-transaction.md "mention") for more details.

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
