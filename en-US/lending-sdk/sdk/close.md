# Close

Continuing from [#id-4.-select-an-use-case](./#id-4.-select-an-use-case "mention").

Close enables users to empty all positions in a single step by using a flash loan to repay borrowed assets and withdraw deposited assets.

* Collateral token: the collateral token to be withdrawn.
* Debt token: the debt token to be repaid.
* Withdrawal token: the user-specified token swapped from the collateral token.

## 5. Preview the estimated post-close portfolio

By specifying the withdrawal token that the user wants to receive, the function will return the remaining withdrawn token amount, the updated user portfolio, and the logics to be executed.

```typescript
// User obtains a quotation for closing positions
const withdrawalToken = mainnetTokens.WBTC;
const deleverageInfo = await adapter.close({
  account,
  portfolio,
  withdrawalToken,
});
```

The logics should include:

1. Borrow a flash loan of the withdrawal token
2. Swap the withdrawal token for the debt tokens
3. Repay the debt with the debt tokens
4. Get the protocol tokens (aToken) from the user
5. Withdraw the collateral tokens
6. Swap the collateral tokens for the withdrawal token
7. Repay the flash loan with the withdrawal token
8. Return the remained withdrawal token to the user

## 6. Obtain the required approval permission and send the router transaction

To perform the logics, certain approvals need to be processed. You may refer to [estimate-router-data.md](../../protocolink-sdk/estimate-router-data.md "mention") and [send-router-transaction.md](../../protocolink-sdk/send-router-transaction.md "mention") for more details.

```typescript
// User needs to permit the Protocolink user agent to borrow for the user
const estimateResult = await apisdk.estimateRouterData(
  { chainId, account, logics: closeInfo.logics }
);

// User obtains a close transaction request
const routerData: apisdk.RouterData = {
  chainId,
  account,
  logics: closeInfo.logics
};
const transactionRequest = await apisdk.buildRouterTransactionRequest(routerData);
```
