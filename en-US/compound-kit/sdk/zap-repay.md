# Zap Repay

Continuing from[#id-4.-select-an-use-case](./#id-4.-select-an-use-case "mention").

## 5. Preview the Estimated Post-Zap-Repay Position & Approval  Permissions

Specify the `srcToken`, `srcAmount` that the user has, intended for repaying with base token. Then It returns a `quotation` used for getting the estimated position. `approvals` need to be signed and submitted on-chain. `logics` is the detailed steps used for building transaction. Logics workflow is as follows:

1. Swap the source token to the base token.
2. Repay with the base token.

<pre class="language-typescript"><code class="lang-typescript">import * as compoundkit from '@protocolink/compound-kit';
import * as common from '@protocolink/common';
import * as logics from '@protocolink/logics';

const chainId = common.ChainId.polygon;
const marketId = compoundkit.MarketId.USDC;
const params = {
  account: '0xaAaAaAaaAaAaAaaAaAAAAAAAAaaaAaAaAaaAaaAa',
  srcToken: logics.compoundv3.polygonTokens.MATIC,
  srcAmount: '10000',
  slippage: 100,
};
<strong>const zapRepayQuotation = await compoundkit.getZapRepayQuotation(chainId, marketId, params);
</strong>// {
//   "quotation": {
//     "destAmount": "5610.402066",
//     "currentPosition": {
//       "utilization": "0.7166",
//       "healthRate": "1.49",
//       "liquidationThreshold": "0.7829",
//       "supplyUSD": "0",
//       "borrowUSD": "82542.9",
//       "collateralUSD": "157176.66",
//       "netAPR": "-0.0472"
//     },
//     "targetPosition": {
//       "utilization": "0.6679",
//       "healthRate": "1.6",
//       "liquidationThreshold": "0.7829",
//       "supplyUSD": "0",
//       "borrowUSD": "76932.11",
//       "collateralUSD": "157176.66",
//       "netAPR": "-0.0409"
//     }
//   },
//   "fees": [
//     {
//       "rid": "native-token",
//       "feeAmount": {
//         "token": {
//           "chainId": 137,
//           "address": "0x0000000000000000000000000000000000001010",
//           "decimals": 18,
//           "symbol": "MATIC",
//           "name": "Matic Token"
//         },
//         "amount": "20"
//       }
//     }
//   ],
//   "approvals": [],
//   "logics": [
//     {
//       "rid": "paraswap-v5:swap-token",
//       "fields": {
//         "input": {
//           "token": {
//             "chainId": 137,
//             "address": "0x0000000000000000000000000000000000001010",
//             "decimals": 18,
//             "symbol": "MATIC",
//             "name": "Matic Token"
//           },
//           "amount": "10000"
//         },
//         "output": {
//           "token": {
//             "chainId": 137,
//             "address": "0x2791Bca1f2de4661ED88A30C99A7a9449Aa84174",
//             "decimals": 6,
//             "symbol": "USDC",
//             "name": "USD Coin (PoS)"
//           },
//           "amount": "5610.402066"
//         },
//         "slippage": 100
//       }
//     },
//     {
//       "rid": "compound-v3:repay",
//       "fields": {
//         "marketId": "USDC",
//         "borrower": "0x0FBeABcaFCf817d47E10a7bCFC15ba194dbD4EEF",
//         "input": {
//           "token": {
//             "chainId": 137,
//             "address": "0x2791Bca1f2de4661ED88A30C99A7a9449Aa84174",
//             "decimals": 6,
//             "symbol": "USDC",
//             "name": "USD Coin (PoS)"
//           },
//           "amount": "5610.402066"
//         },
//         "balanceBps": 10000
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
  logics: zapRepayQuotation.logics,
  referral: '0xbBbBBBBbbBBBbbbBbbBbbbbBBbBbbbbBbBbbBBbB',
};
const transactionRequest = await compoundkit.buildZapRepayTransactionRequest(routerData);
// {
//   "to": "0xf4dEf6B4389eAb49dF2a7D67890810e5249B5E70",
//   "data": "0x...",
//   "value": "10020000000000000000"
// }
```

