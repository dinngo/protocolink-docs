# Open By Debt

Continuing from [#id-4.-select-an-use-case](./#id-4.-select-an-use-case "mention").

Open By Debt enables users to achieve the desired debt exposure in a single step by using a flash loan.

* Zap token: the user-provided token to be swapped to the collateral token.
* Collateral token: the collateral token to be leveraged.
* Debt token: the debt token to be leveraged against.

## 5. Preview the estimated post-open portfolio

By specifying the zap token, the zap token amount, the collateral token, the debt token, and the debt token amount, the function will return the deposited collateral token amount, the updated user portfolio, and the logics to be executed.

```typescript
// User obtains a quotation for opening by debt
const zapToken = mainnetTokens.USDC;
const zapAmount = '1000';
const collateralToken = mainnetTokens.WETH;
const debtToken = mainnetTokens.USDC;
const debtAmount = '1';
const openByCollateralInfo = await adapter.openByDebt({
  account,
  portfolio,
  zapToken,
  zapAmount,
  collateralToken,
  debtToken,
  debtAmount
});
```

The logics should include:

1. Swap the zap token for the collateral token
2. Borrow a flash loan of the debt token
3. Swap the debt token for the collateral token
4. Deposit the collateral token and get the protocol token (aToken)
5. Return the protocol token (aToken) to the user
6. Borrow the debt token
7. Repay the flash loan with the debt token

## 6. Obtain the required approval permission and send the router transaction

To perform the logics, certain approvals need to be processed. You may refer to [estimate-router-data.md](../../protocolink-sdk/estimate-router-data.md "mention") and [send-router-transaction.md](../../protocolink-sdk/send-router-transaction.md "mention") for more details.

```typescript
// User needs to permit the Protocolink user agent to borrow for the user
const estimateResult = await apisdk.estimateRouterData(
  { chainId, account, logics: openByDebtInfo.logics },
  permit2Type
);

// User obtains an open-by-debt transaction request
const routerData: apisdk.RouterData = {
  chainId,
  account,
  logics: openByDebtInfo.logics
};
const transactionRequest = await apisdk.buildRouterTransactionRequest(routerData);
```
