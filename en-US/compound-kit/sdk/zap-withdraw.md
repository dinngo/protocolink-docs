# Zap Withdraw

Continuing from[#id-4.-select-an-use-case](./#id-4.-select-an-use-case "mention").

## 5. Preview the Estimated Post-Zap-Withdraw Position & Approval  Permissions

Specify the `srcToken`, `srcAmount` that represents the base or collateral token the user wants to withdraw, along with the `destToken` which the user wants. Then It returns a `quotation` used for getting the estimated position. `approvals` need to be signed and submitted on-chain. `logics` is the detailed steps used for building transaction. Logics workflow is as follows:

1. Withdraw the base or collateral token from the market.
2. Swap the base or collateral token to the destination token.

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
<strong>const zapWithdrawQuotation = await compoundkit.getZapWithdrawQuotation(chainId, marketId, params);
</strong>// {
//   "quotation": {
//     "destAmount": "2965.808167230875928853",
//     "currentPosition": {
//       "utilization": "0.7093",
//       "healthRate": "1.51",
//       "liquidationThreshold": "0.7829",
//       "supplyUSD": "0",
//       "borrowUSD": "82535.89",
//       "collateralUSD": "158778.09",
//       "netAPR": "-0.0462"
//     },
//     "targetPosition": {
//       "utilization": "-0.0008",
//       "healthRate": "1.49",
//       "liquidationThreshold": "0.7824",
//       "supplyUSD": "0",
//       "borrowUSD": "82535.89",
//       "collateralUSD": "157074.4",
//       "netAPR": "-0.0462"
//     }
//   },
//   "fees": [],
//   "approvals": [
//     {
//       "to": "0xF25212E676D1F7F89Cd72fFEe66158f541246445",
//       "data": "0x110496e5000000000000000000000000e7c086090b361c955950956105df200b20f66d700000000000000000000000000000000000000000000000000000000000000001"
//     }
//   ],
//   "logics": [
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
//             "address": "0x0000000000000000000000000000000000001010",
//             "decimals": 18,
//             "symbol": "MATIC",
//             "name": "Matic Token"
//           },
//           "amount": "2965.808167230875928853"
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
  logics: zapWithdrawQuotation.logics,
  referral: '0xbBbBBBBbbBBBbbbBbbBbbbbBBbBbbbbBbBbbBBbB',
};
const transactionRequest = await compoundkit.buildZapWithdrawTransactionRequest(routerData);
// {
//   "to": "0xf4dEf6B4389eAb49dF2a7D67890810e5249B5E70",
//   "data": "0x...",
//   "value": "10020000000000000000"
// }
```

