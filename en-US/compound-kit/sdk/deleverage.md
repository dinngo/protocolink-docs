# Deleverage

Continuing from [#4.-select-an-intent](./#4.-select-an-intent "mention")

## 5. Preview the Estimated Post-Deleverage Position & Approval  Permissions

Specify the `collateralToken` and `baseAmount` that the user wants to deleverage. Then It returns a `quotation` used for getting the estimated position. `approvals` need to be signed and submitted on-chain. `logics` is the detailed steps used for building transaction. Logics workflow is as follows:

1. Initiates a flash loan of the deleverage token.
2. Swap the deleverage token to the base token.
3. Repay the base token.
4. Withdraw the deleverage token for the user.
5. Repay the flash loan.

<pre class="language-typescript"><code class="lang-typescript">import * as compoundkit from '@protocolink/compound-kit';
import * as common from '@protocolink/common';
import * as logics from '@protocolink/logics';

const chainId = common.ChainId.polygon;
const marketId = compoundkit.MarketId.USDC;
const params = {
  account: '0xaAaAaAaaAaAaAaaAaAAAAAAAAaaaAaAaAaaAaaAa',
  collateralToken: logics.compoundv3.polygonTokens.WETH,
  baseAmount: '1000', // It's base token amount
  slippage: 100,
};
<strong>const deleverageQuotation = await compoundkit.getDeleverageQuotation(chainId, marketId, params);
</strong>// {
//   "quotation": {
//     "currentPosition": {
//       "utilization": "0.653",
//       "healthRate": "1.64",
//       "liquidationThreshold": "0.7774",
//       "supplyUSD": "0",
//       "borrowUSD": "76853.77",
//       "collateralUSD": "161808.21",
//       "netAPR": "-0.0434"
//     },
//     "targetPosition": {
//       "utilization": "0.6487",
//       "healthRate": "1.65",
//       "liquidationThreshold": "0.7771",
//       "supplyUSD": "0",
//       "borrowUSD": "75853.82",
//       "collateralUSD": "160808.26",
//       "netAPR": "-0.0429"
//     }
//   },
//   "fees": [
//     {
//       "rid": "utility:flash-loan-aggregator",
//       "feeAmount": {
//         "token": {
//           "chainId": 137,
//           "address": "0x0000000000000000000000000000000000001010",
//           "decimals": 18,
//           "symbol": "MATIC",
//           "name": "Matic Token"
//         },
//         "amount": "0.811706124847048818"
//       }
//     }
//   ],
//   "approvals": [
//     {
//       "to": "0xF25212E676D1F7F89Cd72fFEe66158f541246445",
//       "data": "0x110496e50000000000000000000000000e7c086090b361c955950956105df200b20f66d70000000000000000000000000000000000000000000000000000000000000001"
//     }
//   ],
//   "logics": [
//     {
//       "rid": "utility:flash-loan-aggregator",
//       "fields": {
//         "id": "82c3253b-3203-4f3b-bd2a-bbc7b308646b",
//         "protocolId": "balancer-v2",
//         "loans": [
//           {
//             "token": {
//               "chainId": 137,
//               "address": "0x7ceB23fD6bC0adD59E62ac25578270cFf1b9f619",
//               "decimals": 18,
//               "symbol": "WETH",
//               "name": "Wrapped Ether"
//             },
//             "amount": "0.558401658980619882"
//           }
//         ],
//         "isLoan": true
//       }
//     },
//     {
//       "rid": "paraswap-v5:swap-token",
//       "fields": {
//         "input": {
//           "token": {
//             "chainId": 137,
//             "address": "0x7ceB23fD6bC0adD59E62ac25578270cFf1b9f619",
//             "decimals": 18,
//             "symbol": "WETH",
//             "name": "Wrapped Ether"
//           },
//           "amount": "0.558401658980619882"
//         },
//         "output": {
//           "token": {
//             "chainId": 137,
//             "address": "0x2791Bca1f2de4661ED88A30C99A7a9449Aa84174",
//             "decimals": 6,
//             "symbol": "USDC",
//             "name": "USD Coin (PoS)"
//           },
//           "amount": "1000"
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
//           "amount": "1000"
//         },
//         "balanceBps": 10000
//       }
//     },
//     {
//       "rid": "compound-v3:withdraw-collateral",
//       "fields": {
//         "marketId": "USDC",
//         "output": {
//           "token": {
//             "chainId": 137,
//             "address": "0x7ceB23fD6bC0adD59E62ac25578270cFf1b9f619",
//             "decimals": 18,
//             "symbol": "WETH",
//             "name": "Wrapped Ether"
//           },
//           "amount": "0.558401658980619882"
//         }
//       }
//     },
//     {
//       "rid": "utility:flash-loan-aggregator",
//       "fields": {
//         "id": "82c3253b-3203-4f3b-bd2a-bbc7b308646b",
//         "protocolId": "balancer-v2",
//         "loans": [
//           {
//             "token": {
//               "chainId": 137,
//               "address": "0x7ceB23fD6bC0adD59E62ac25578270cFf1b9f619",
//               "decimals": 18,
//               "symbol": "WETH",
//               "name": "Wrapped Ether"
//             },
//             "amount": "0.558401658980619882"
//           }
//         ],
//         "isLoan": false
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
  logics: deleverageQuotation.logics,
  referral: '0xbBbBBBBbbBBBbbbBbbBbbbbBBbBbbbbBbBbbBBbB',
};
const transactionRequest = await compoundkit.buildDeleverageTransactionRequest(routerData);
// {
//   "to": "0xf4dEf6B4389eAb49dF2a7D67890810e5249B5E70",
//   "data": "0x...",
//   "value": "811819097629533926"
// }
```

