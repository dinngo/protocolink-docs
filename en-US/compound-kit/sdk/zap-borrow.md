# Zap Borrow

Continuing from [#4.-select-an-intent](./#4.-select-an-intent "mention")

## 5. Preview the Estimated Post-Zap-Borrow Position & Approval  Permissions

Specify the `srcAmount` of base token that the user wants to borrow, along with the `destToken` which represents the token which user wants. Then It returns a `quotation` used for getting the estimated position. `approvals` need to be signed and submitted on-chain. `logics` is the detailed steps used for building transaction. Logics workflow is as follows:

1. Borrow the base token.
2. Swap the base token to the destination token.

<pre class="language-typescript"><code class="lang-typescript">import * as compoundkit from '@protocolink/compound-kit';
import * as common from '@protocolink/common';
import * as logics from '@protocolink/logics';

const chainId = common.ChainId.polygon;
const marketId = compoundkit.MarketId.USDC;
const params = {
  account: '0xaAaAaAaaAaAaAaaAaAAAAAAAAaaaAaAaAaaAaaAa',
  srcAmount: '1000',
  destToken: logics.compoundv3.polygonTokens.MATIC,
  slippage: 100,
};
<strong>const zapBorrowQuotation = await compoundkit.getZapBorrowQuotation(chainId, marketId, params);
</strong>// {
//   "quotation": {
//     "destAmount": "17659.570770299088050127",
//     "currentPosition": {
//       "utilization": "0.7166",
//       "healthRate": "1.49",
//       "liquidationThreshold": "0.7829",
//       "supplyUSD": "0",
//       "borrowUSD": "82544.66",
//       "collateralUSD": "157169.13",
//       "netAPR": "-0.0471"
//     },
//     "targetPosition": {
//       "utilization": "0.8034",
//       "healthRate": "1.33",
//       "liquidationThreshold": "0.7829",
//       "supplyUSD": "0",
//       "borrowUSD": "92545.56",
//       "collateralUSD": "157169.13",
//       "netAPR": "-0.061"
//     }
//   },
//   "fees": [
//     {
//       "rid": "compound-v3:borrow",
//       "feeAmount": {
//         "token": {
//           "chainId": 137,
//           "address": "0x0000000000000000000000000000000000001010",
//           "decimals": 18,
//           "symbol": "MATIC",
//           "name": "Matic Token"
//         },
//         "amount": "35.314145434915390767"
//       }
//     }
//   ],
//   "approvals": [
//     {
//       "to": "0xF25212E676D1F7F89Cd72fFEe66158f541246445",
//       "data": "0x110496e5000000000000000000000000e7c086090b361c955950956105df200b20f66d700000000000000000000000000000000000000000000000000000000000000001"
//     }
//   ],
//   "logics": [
//     {
//       "rid": "compound-v3:borrow",
//       "fields": {
//         "marketId": "USDC",
//         "output": {
//           "token": {
//             "chainId": 137,
//             "address": "0x2791Bca1f2de4661ED88A30C99A7a9449Aa84174",
//             "decimals": 6,
//             "symbol": "USDC",
//             "name": "USD Coin (PoS)"
//           },
//           "amount": "10000"
//         }
//       }
//     },
//     {
//       "rid": "paraswap-v5:swap-token",
//       "fields": {
//         "input": {
//           "token": {
//             "chainId": 137,
//             "address": "0x2791Bca1f2de4661ED88A30C99A7a9449Aa84174",
//             "decimals": 6,
//             "symbol": "USDC",
//             "name": "USD Coin (PoS)"
//           },
//           "amount": "10000"
//         },
//         "output": {
//           "token": {
//             "chainId": 137,
//             "address": "0x0000000000000000000000000000000000001010",
//             "decimals": 18,
//             "symbol": "MATIC",
//             "name": "Matic Token"
//           },
//           "amount": "17659.570770299088050127"
//         },
//         "slippage": 100
//       }
//     }
//   ]
// }
</code></pre>

## 6. Obtain Transaction Data for Execution

Provides transaction data that is needed to execute this operation. Armed with the `logics` from the previous step, it generates the `to`, `data` and `value` including fees then signs and submits to finalize the operation.

If you wish to include a referral for fee sharing, you can append the `referral` property to the `routerData` object. For detailed information on using `routerData`, please refer to the [Router Data Documentation](../../protocolink-sdk/api-sdk-interfaces/global-types.md#routerdata).

```typescript
import * as compoundkit from '@protocolink/compound-kit';
import * as common from '@protocolink/common';
import * as apisdk from '@protocolink/api';

const routerData: apisdk.RouterData = {
  chainId: common.ChainId.polygon,
  account: '0xaAaAaAaaAaAaAaaAaAAAAAAAAaaaAaAaAaaAaaAa',
  logics: zapBorrowQuotation.logics,
  referral: '0xbBbBBBBbbBBBbbbBbbBbbbbBBbBbbbbBbBbbBBbB',
};
const transactionRequest = await compoundkit.buildZapBorrowTransactionRequest(routerData);
// {
//   "to": "0xf4dEf6B4389eAb49dF2a7D67890810e5249B5E70",
//   "data": "0x...",
//   "value": "10020000000000000000"
// }
```

