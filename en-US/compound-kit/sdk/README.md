# ✳ SDK

## GitHub

[https://github.com/dinngo/compound-kit-js-sdk](https://github.com/dinngo/compound-kit-js-sdk)

## 1. Install

```bash
yarn add @protocolink/compound-kit
```

## 2. Markets List

List all Compound markets available across multiple chains. This function will return a list of market groups, each containing the chain ID and the markets available on that chain.

<pre class="language-typescript"><code class="lang-typescript">import * as compoundkit from '@protocolink/compound-kit';

<strong>const marketGroups = await compoundkit.getMarketGroups();
</strong>// {
//   "marketGroups": [
//     {
//       "chainId": 1,
//       "markets": [
//         {
//           "id": "USDC",
//           "label": "USDC"
//         },
//         {
//           "id": "ETH",
//           "label": "ETH"
//         }
//       ]
//     },
//     {
//       "chainId": 137,
//       "markets": [
//         {
//           "id": "USDC",
//           "label": "USDC"
//         }
//       ]
//     },
//     {
//       "chainId": 42161,
//       "markets": [
//         {
//           "id": "USDC",
//           "label": "USDC.e"
//         }
//       ]
//     }
//   ]
// }
</code></pre>

## 3. Market Information & Positions for Accounts

Gather market information and positions for a specific account. The account parameter is optional. If an account address is provided, this function will also return the position details for that account in that particular market. This organized information can be presented to users to assist them in making decisions.

```typescript
import * as compoundkit from '@protocolink/compound-kit';
import * as common from '@protocolink/common';

const chainId = common.ChainId.polygon;
const marketId = compoundkit.MarketId.USDC;
const account = '0xaAaAaAaaAaAaAaaAaAAAAAAAAaaaAaAaAaaAaaAa';
const marketInfo = await compoundkit.getMarketInfo(chainId, marketId, account);
// {
//   "baseToken": {
//     "chainId": 137,
//     "address": "0x2791Bca1f2de4661ED88A30C99A7a9449Aa84174",
//     "decimals": 6,
//     "symbol": "USDC",
//     "name": "USD Coin (PoS)"
//   },
//   "baseTokenPrice": "0.999956",
//   "baseBorrowMin": "100",
//   "supplyAPR": "0.034",
//   "supplyBalance": "0",
//   "supplyUSD": "0",
//   "borrowAPR": "0.048",
//   "borrowBalance": "76857.439242",
//   "borrowUSD": "76854.06",
//   "collateralUSD": "161888.83",
//   "borrowCapacity": "117764.28294",
//   "borrowCapacityUSD": "117759.1",
//   "availableToBorrow": "40906.843698",
//   "availableToBorrowUSD": "40905.04",
//   "liquidationLimit": "125853.54",
//   "liquidationThreshold": "0.7774",
//   "liquidationRisk": "0.61",
//   "liquidationPoint": "98756.531272",
//   "liquidationPointUSD": "98752.19",
//   "utilization": "0.6526",
//   "healthRate": "1.64",
//   "netAPR": "-0.0434",
//   "collaterals": [
//     {
//       "asset": {
//         "chainId": 137,
//         "address": "0x7ceB23fD6bC0adD59E62ac25578270cFf1b9f619",
//         "decimals": 18,
//         "symbol": "WETH",
//         "name": "Wrapped Ether"
//       },
//       "assetPrice": "1792.148",
//       "borrowCollateralFactor": "0.775",
//       "liquidateCollateralFactor": "0.825",
//       "collateralBalance": "42.561192291871282298",
//       "collateralUSD": "76275.96",
//       "borrowCapacity": "59116.466748",
//       "borrowCapacityUSD": "59113.87"
//     },
//     {
//       "asset": {
//         "chainId": 137,
//         "address": "0x1BFD67037B42Cf73acF2047067bd4F2C47D9BfD6",
//         "decimals": 8,
//         "symbol": "WBTC",
//         "name": "(PoS) Wrapped BTC"
//       },
//       "assetPrice": "28536.334695",
//       "borrowCollateralFactor": "0.7",
//       "liquidateCollateralFactor": "0.75",
//       "collateralBalance": "2.10038726",
//       "collateralUSD": "59937.35",
//       "borrowCapacity": "41957.99384",
//       "borrowCapacityUSD": "41956.15"
//     },
//     {
//       "asset": {
//         "chainId": 137,
//         "address": "0x0000000000000000000000000000000000001010",
//         "decimals": 18,
//         "symbol": "MATIC",
//         "name": "Matic Token"
//       },
//       "assetPrice": "0.6172",
//       "borrowCollateralFactor": "0.65",
//       "liquidateCollateralFactor": "0.7",
//       "collateralBalance": "41600",
//       "collateralUSD": "25675.52",
//       "borrowCapacity": "16689.822352",
//       "borrowCapacityUSD": "16689.09"
//     },
//     {
//       "asset": {
//         "chainId": 137,
//         "address": "0xfa68FB4628DFF1028CFEc22b4162FCcd0d45efb6",
//         "decimals": 18,
//         "symbol": "MaticX",
//         "name": "Liquid Staking Matic (PoS)"
//       },
//       "assetPrice": "0.66438874",
//       "borrowCollateralFactor": "0.55",
//       "liquidateCollateralFactor": "0.6",
//       "collateralBalance": "0",
//       "collateralUSD": "0",
//       "borrowCapacity": "0",
//       "borrowCapacityUSD": "0"
//     }
//   ]
// }
```

## 4. Select an use case

* [Leverage](leverage.md): achieve the desired collateral exposure in a single step using a flash loan.
* [Deleverage](deleverage.md): reduce the collateral exposure in a single step using a flash loan to repay the borrowed asset.
* [Collateral swap](collateral-swap.md): replace one collateral asset with another in a single step using a flash loan.
* [Zap Supply](zap-supply.md): swap any token to add as base or collateral token in one transaction.
* [Zap Withdraw](zap-withdraw.md): withdraw base or collateral token, then swap to any token in one transaction.
* [Zap Repay](zap-repay.md): swap any token to repay as base token in one transaction.
* [Zap Borrow](zap-borrow.md): borrow the base token, then swap to any token in one transaction.

Depending on the selected use cases, users will need to input different parameters, and the SDK will generate the expected outcome. If users are satisfied with the expected outcome and the fees, they can sign and submit the approval and transaction data. Typically, the approval is a one-time requirement for allowing Protocolink to act as a manager or a Permit2 token approval (which poses no risk as each user has their own agent within Protocolink - for further details, please visit [Broken link](broken-reference "mention")).
