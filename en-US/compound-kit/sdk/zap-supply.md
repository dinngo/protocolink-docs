# Zap Supply

Continuing from [#4.-select-an-intent](./#4.-select-an-intent "mention")

## 5. Preview the Estimated Post-Zap-Supply Position & Approval  Permissions

Specify the `srcToken`, `srcAmount` that the user has, along with the `destToken` which represents the base or collateral tokens the user wants to supply. Then It returns a `quotation` used for getting the estimated position. `approvals` need to be signed and submitted on-chain. `logics` is the detailed steps used for building transaction. Logics workflow is as follows:

1. Swap the source token to the base or collateral token.
2. Supply the base or collateral to the market.

<pre class="language-typescript"><code class="lang-typescript">import * as compoundkit from '@protocolink/compound-kit';
import * as common from '@protocolink/common';
import * as logics from '@protocolink/logics';

const chainId = common.ChainId.polygon;
const marketId = compoundkit.MarketId.USDC;
const params = {
  account: '0xaAaAaAaaAaAaAaaAaAAAAAAAAaaaAaAaAaaAaaAa',
  srcToken: logics.compoundv3.polygonTokens.MATIC,
  srcAmount: '10',
  destToken: logics.compoundv3.polygonTokens.WETH,
  slippage: 100,
};
<strong>const zapSupplyQuotation = await compoundkit.getZapSupplyQuotation(chainId, marketId, params);
</strong>// {
//   "quotation": {
//     "destAmount": "0.003414475482347128",
//     "currentPosition": {
//       "utilization": "0.6649",
//       "healthRate": "1.61",
//       "liquidationThreshold": "0.7828",
//       "supplyUSD": "0",
//       "borrowUSD": "77538.38",
//       "collateralUSD": "159157.49",
//       "netAPR": "-0.0407"
//     },
//     "targetPosition": {
//       "utilization": "0.6648",
//       "healthRate": "1.61",
//       "liquidationThreshold": "0.7828",
//       "supplyUSD": "0",
//       "borrowUSD": "77538.38",
//       "collateralUSD": "159163.31",
//       "netAPR": "-0.0407"
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
//         "amount": "0.02"
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
//           "amount": "10"
//         },
//         "output": {
//           "token": {
//             "chainId": 137,
//             "address": "0x7ceB23fD6bC0adD59E62ac25578270cFf1b9f619",
//             "decimals": 18,
//             "symbol": "WETH",
//             "name": "Wrapped Ether"
//           },
//           "amount": "0.003414475482347128"
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
//             "address": "0x7ceB23fD6bC0adD59E62ac25578270cFf1b9f619",
//             "decimals": 18,
//             "symbol": "WETH",
//             "name": "Wrapped Ether"
//           },
//           "amount": "0.003414475482347128"
//         },
//         "balanceBps": 10000
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
  logics: zapSupplyQuotation.logics
};
const transactionRequest = await compoundkit.buildZapSupplyTransactionRequest(routerData);
// {
//   "to": "0xf4dEf6B4389eAb49dF2a7D67890810e5249B5E70",
//   "data": "0x...",
//   "value": "10020000000000000000"
// }
```

