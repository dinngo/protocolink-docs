# Collateral Swap

Continuing from [#4.-select-an-intent](./#4.-select-an-intent "mention")

## 5. Preview the Estimated Post-Collateral-Swap Position & Approval  Permissions

Specify the `srcToken`, `srcAmount` and `destToken` that the user wants to collateral swap. Then It returns a `quotation` used for getting the estimated position. `approvals` need to be signed and submitted on-chain. `logics` is the detailed steps used for building transaction. Logics workflow is as follows:

1. Initiates a flash loan of the withdrawal token.
2. Swap the withdrawal token to the target token.
3. Supply the target token for the user.
4. Withdraw the withdrawal token.
5. Repay the flash loan.

<pre class="language-typescript"><code class="lang-typescript">import * as compoundkit from '@protocolink/compound-kit';
import * as common from '@protocolink/common';
import * as logics from '@protocolink/logics';

const chainId = common.ChainId.polygon;
const marketId = compoundkit.MarketId.USDC;
const params = {
  account: '0xaAaAaAaaAaAaAaaAaAAAAAAAAaaaAaAaAaaAaaAa',
  srcToken: logics.compoundv3.polygonTokens.WETH,
  srcAmount: '1',
  destToken: logics.compoundv3.polygonTokens.MATIC,
  slippage: 100,
};
<strong>const collateralSwapQuotation = await compoundkit.getCollateralSwapQuotation(chainId, marketId, params);
</strong>// {
//   "quotation": {
//     "destAmount": "2898.720763549476022827",
//     "currentPosition": {
//       "utilization": "0.6525",
//       "healthRate": "1.64",
//       "liquidationThreshold": "0.7774",
//       "supplyUSD": "0",
//       "borrowUSD": "76855.87",
//       "collateralUSD": "161922",
//       "netAPR": "-0.0434"
//     },
//     "targetPosition": {
//       "utilization": "0.6525",
//       "healthRate": "1.6",
//       "liquidationThreshold": "0.7605",
//       "supplyUSD": "0",
//       "borrowUSD": "76855.87",
//       "collateralUSD": "161921.75",
//       "netAPR": "-0.0434"
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
//         "amount": "1.449862459546925566"
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
//         "id": "0fa4d68e-3206-4ebf-9245-6858edc77b26",
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
//             "amount": "1"
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
//           "amount": "1"
//         },
//         "output": {
//           "token": {
//             "chainId": 137,
//             "address": "0x0d500B1d8E8eF31E21C99d1Db9A6444d3ADf1270",
//             "decimals": 18,
//             "symbol": "WMATIC",
//             "name": "Wrapped Matic Token"
//           },
//           "amount": "2898.720763549476022827"
//         },
//         "slippage": 100
//       }
//     },
//     {
//       "rid": "compound-v3:supply-collateral",
//       "fields": {
//         "marketId": "USDC",
//         "input": {
//           "token": {
//             "chainId": 137,
//             "address": "0x0d500B1d8E8eF31E21C99d1Db9A6444d3ADf1270",
//             "decimals": 18,
//             "symbol": "WMATIC",
//             "name": "Wrapped Matic Token"
//           },
//           "amount": "2898.720763549476022827"
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
//           "amount": "1"
//         }
//       }
//     },
//     {
//       "rid": "utility:flash-loan-aggregator",
//       "fields": {
//         "id": "0fa4d68e-3206-4ebf-9245-6858edc77b26",
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
//             "amount": "1"
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

```typescript
import * as compoundkit from '@protocolink/compound-kit';
import * as common from '@protocolink/common';
import * as apisdk from '@protocolink/api';

const routerData: apisdk.RouterData = {
  chainId: common.ChainId.polygon,
  account: '0xaAaAaAaaAaAaAaaAaAAAAAAAAaaaAaAaAaaAaaAa',
  logics: collateralSwapQuotation.logics
};
const transactionRequest = await compoundkit.buildCollateralSwapTransactionRequest(routerData);
// {
//   "to": "0xf4dEf6B4389eAb49dF2a7D67890810e5249B5E70",
//   "data": "0x...",
//   "value": "1450016165953125548"
// }
```

