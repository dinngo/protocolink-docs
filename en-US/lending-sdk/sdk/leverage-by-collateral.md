# Leverage By Collateral

Continuing from [#id-4.-select-an-use-case](./#id-4.-select-an-use-case "mention").

Leverage By Collateral enables users to achieve the desired collateral exposure in a single step by using a flash loan.

* Source token: the collateral token to be leveraged.
* Destination token: the debt token to be leveraged against.

## 5. Preview the estimated post-leverage portfolio

By specifying the source token, the source token amount, and the destination token that the user wants to leverage, the function will return the destination token amount, the updated user portfolio, and the logics to be executed.

```typescript
// User obtains a quotation for leveraging by collateral: src token
const srcToken = mainnetTokens.USDC;
const srcAmount = '1';
const destToken = mainnetTokens.WETH;
const leverageByCollateralInfo = await adapter.leverageByCollateral({
  account,
  portfolio,
  srcToken,
  srcAmount,
  destToken
});
```

The logics should include:

1. Borrow a flash loan of the destination token
2. Swap the destination token for the source token
3. Deposit the source token and get the protocol source token (aToken)
4. Return the protocol source token (aToken) to the user
5. Borrow the destination token
6. Repay the flash loan with the destination token

## 6. Obtain the required approval permission and send the router transaction

To perform the logics, certain approvals need to be processed. You may refer to [estimate-router-data.md](../../protocolink-sdk/estimate-router-data.md "mention") and [send-router-transaction.md](../../protocolink-sdk/send-router-transaction.md "mention") for more details.

```typescript
// User needs to permit the Protocolink user agent to borrow for the user
const estimateResult = await apisdk.estimateRouterData(
  { chainId, account, logics: leverageByCollateralInfo.logics },
  permit2Type
);

// User obtains a leverage by collateral transaction request
const routerData: apisdk.RouterData = {
  chainId,
  account,
  logics: leverageByCollateralInfo.logics
};
const transactionRequest = await apisdk.buildRouterTransactionRequest(routerData);
```
