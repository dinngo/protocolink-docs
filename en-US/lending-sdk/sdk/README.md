# ✳️ SDK

Lending SDK provides tools and build logics for lending purpose, which works as a part of [Protocolink JS SDK](../../protocolink-sdk/overview.md). Please refer to [Broken link](broken-reference "mention") for complete setup and transaction construction.&#x20;

## GitHub

[https://github.com/dinngo/protocolink-js-sdk/tree/master/packages/lending](https://github.com/dinngo/protocolink-js-sdk/tree/master/packages/lending)

## 1. Install

```bash
yarn add @protocolink/lending
```

## 2. Adapter

The `Adapter` is the primary role in Lending SDK. The `Adapter` enables seamless retrieval of user information and interaction with a range of supported protocols, simplifying the process of integrating blockchain functionalities into various applications.&#x20;

### 2.1 Configuration

Lending SDK coordinates the interaction between lending protocols and token swappers. Currently Protocolink supports the following protocols:

* Lending protocol
  * Aave V2
  * Aave V3
  * Radiant V2
  * Spark
  * Compound V3
  * Morpho Blue
* Swapper
  * Paraswap V5
  * Openocean V2

&#x20;User may register the protocols they want to use in the service.

```typescript
import * as lending from '@protocolink/lending';

lending.Adapter.registerProtocol(lending.protocols.radiantv2.LendingProtocol);
lending.Adapter.registerSwapper(lending.swappers.paraswapv5.LendingSwapper);
```

### 2.2 Initialization

To initialize the `Adapter`, users must specify a `chainId`, with the option to include a `provider` for enhanced flexibility. Once initialized, the `Adapter` enables seamless retrieval of user information and interaction with a range of supported protocols, simplifying the process of integrating blockchain functionalities into various applications.

```typescript
import * as common from '@protocolink/common';

const chainId = common.ChainId.arbitrum;
const adapter = await lending.Adapter.createAdapter(chainId/*, provider*/);
// Note: Adapter initialization is async starting from version 2.0.0
```

## 3. Get the portfolio

Gather the information of a user portfolio. The portfolio is determined by user account, protocol, and market. The portfolio contains information to assist user to identify their position to make further decisions.

```typescript
const account = '0xBf891E7eFCC98A8239385D3172bA10AD593c7886';
const protocolId = 'radiant-v2';
const marketId = 'arbitrum';
const portfolio = await adapter.getPortfolio(account, protocolId, marketId);

// {
//   "chainId": 42161,
//   "protocolId": "radiant-v2",
//   "marketId": "arbitrum",
//   "utilization": "0.67652248088493198001",
//   "healthRate": "1.56460505284881241768",
//   "netAPY": "-0.53758244345432822726",
//   "totalSupplyUSD": "1384373.85885496154690423014032762",
//   "totalBorrowUSD": "683244.82625914760441",
//   "supplies": [
//     {
//       "token": {
//         "chainId": 42161,
//         "address": "0x2f2a2543B76A4166549F7aaB2e75Bef0aefC5B0f",
//         "decimals": 8,
//         "symbol": "WBTC",
//         "name": "Wrapped BTC"
//       },
//       "price": "71465.657",
//       "balance": "13.60248086",
//       "apy": "0.0035962942519172305",
//       "lstApy": "0",
//       "grossApy": "0.0035962942519172305",
//       "usageAsCollateralEnabled": true,
//       "ltv": "0.7",
//       "liquidationThreshold": "0.75",
//       "isNotCollateral": false,
//       "supplyCap": "0",
//       "totalSupply": "936.79215855"
//     },
//     {
//       "token": {
//         "chainId": 42161,
//         "address": "0xFd086bC7CD5C481DCC9C85ebE478A1C0b69FCbb9",
//         "decimals": 6,
//         "symbol": "USDT",
//         "name": "Tether USD"
//       },
//       "price": "0.99966",
//       "balance": "501.934966",
//       "apy": "0.12921063613317743755",
//       "lstApy": "0",
//       "grossApy": "0.12921063613317743755",
//       "usageAsCollateralEnabled": true,
//       "ltv": "0.8",
//       "liquidationThreshold": "0.85",
//       "isNotCollateral": false,
//       "supplyCap": "0",
//       "totalSupply": "6009014.331071"
//     },
//     {
//       "token": {
//         "chainId": 42161,
//         "address": "0xFF970A61A04b1cA14834A43f5dE4533eBDDB5CC8",
//         "decimals": 6,
//         "symbol": "USDC.e",
//         "name": "USD Coin (Arb1)"
//       },
//       "price": "1.00005167",
//       "balance": "722.233224",
//       "apy": "0.05509963326726775296",
//       "lstApy": "0",
//       "grossApy": "0.05509963326726775296",
//       "usageAsCollateralEnabled": true,
//       "ltv": "0.8",
//       "liquidationThreshold": "0.85",
//       "isNotCollateral": false,
//       "supplyCap": "0",
//       "totalSupply": "7402674.944346"
//     },
//     {
//       "token": {
//         "chainId": 42161,
//         "address": "0xaf88d065e77c8cC2239327C5EDb3A432268e5831",
//         "decimals": 6,
//         "symbol": "USDC",
//         "name": "USD Coin"
//       },
//       "price": "1.00005167",
//       "balance": "963.911118",
//       "apy": "0.1754250895281379224",
//       "lstApy": "0",
//       "grossApy": "0.1754250895281379224",
//       "usageAsCollateralEnabled": true,
//       "ltv": "0.8",
//       "liquidationThreshold": "0.85",
//       "isNotCollateral": false,
//       "supplyCap": "0",
//       "totalSupply": "15761329.248605"
//     },
//     {
//       "token": {
//         "chainId": 42161,
//         "address": "0xDA10009cBd5D07dd0CeCc66161FC93D7c9000da1",
//         "decimals": 18,
//         "symbol": "DAI",
//         "name": "Dai Stablecoin"
//       },
//       "price": "1.00008749",
//       "balance": "380.992966426220398966",
//       "apy": "0.08880017844722081458",
//       "lstApy": "0",
//       "grossApy": "0.08880017844722081458",
//       "usageAsCollateralEnabled": true,
//       "ltv": "0.75",
//       "liquidationThreshold": "0.85",
//       "isNotCollateral": false,
//       "supplyCap": "0",
//       "totalSupply": "339906.064990617229487942"
//     },
//     {
//       "token": {
//         "chainId": 42161,
//         "address": "0x0000000000000000000000000000000000000000",
//         "decimals": 18,
//         "symbol": "ETH",
//         "name": "Ethereum"
//       },
//       "price": "3693.9254",
//       "balance": "110.634917518888222245",
//       "apy": "0.01602458439533162576",
//       "lstApy": "0",
//       "grossApy": "0.01602458439533162576",
//       "usageAsCollateralEnabled": true,
//       "ltv": "0.8",
//       "liquidationThreshold": "0.825",
//       "isNotCollateral": false,
//       "supplyCap": "0",
//       "totalSupply": "20050.991957797928381536"
//     },
//     {
//       "token": {
//         "chainId": 42161,
//         "address": "0x5979D7b546E38E414F7E9822514be443A4800529",
//         "decimals": 18,
//         "symbol": "wstETH",
//         "name": "Wrapped liquid staked Ether 2.0"
//       },
//       "price": "4290.67538678",
//       "balance": "0.057950930053571226",
//       "apy": "0.00600761428859582296",
//       "lstApy": "0.0333",
//       "grossApy": "0.03930761428859582296",
//       "usageAsCollateralEnabled": true,
//       "ltv": "0.7",
//       "liquidationThreshold": "0.8",
//       "isNotCollateral": false,
//       "supplyCap": "0",
//       "totalSupply": "10512.143583095959491247"
//     },
//     {
//       "token": {
//         "chainId": 42161,
//         "address": "0x912CE59144191C1204E64559FE8253a0e49E6548",
//         "decimals": 18,
//         "symbol": "ARB",
//         "name": "Arbitrum"
//       },
//       "price": "1.5594",
//       "balance": "493.025980055092585195",
//       "apy": "0.00367554381258376507",
//       "lstApy": "0",
//       "grossApy": "0.00367554381258376507",
//       "usageAsCollateralEnabled": true,
//       "ltv": "0.4",
//       "liquidationThreshold": "0.5",
//       "isNotCollateral": false,
//       "supplyCap": "0",
//       "totalSupply": "9425057.51767783669294782"
//     }
//   ],
//   "borrows": [
//     {
//       "token": {
//         "chainId": 42161,
//         "address": "0x2f2a2543B76A4166549F7aaB2e75Bef0aefC5B0f",
//         "decimals": 8,
//         "symbol": "WBTC",
//         "name": "Wrapped BTC"
//       },
//       "price": "71465.657",
//       "balance": "0",
//       "apy": "0.05086599469915566102",
//       "lstApy": "0",
//       "grossApy": "0.05086599469915566102",
//       "borrowMin": "0",
//       "borrowCap": "0",
//       "totalBorrow": "271.12489857"
//     },
//     {
//       "token": {
//         "chainId": 42161,
//         "address": "0xFd086bC7CD5C481DCC9C85ebE478A1C0b69FCbb9",
//         "decimals": 6,
//         "symbol": "USDT",
//         "name": "Tether USD"
//       },
//       "price": "0.99966",
//       "balance": "0",
//       "apy": "0.70234151476701983941",
//       "lstApy": "0",
//       "grossApy": "0.70234151476701983941",
//       "borrowMin": "0",
//       "borrowCap": "0",
//       "totalBorrow": "5490249.656378"
//     },
//     {
//       "token": {
//         "chainId": 42161,
//         "address": "0xFF970A61A04b1cA14834A43f5dE4533eBDDB5CC8",
//         "decimals": 6,
//         "symbol": "USDC.e",
//         "name": "USD Coin (Arb1)"
//       },
//       "price": "1.00005167",
//       "balance": "0",
//       "apy": "0.28102251213477240658",
//       "lstApy": "0",
//       "grossApy": "0.28102251213477240658",
//       "borrowMin": "0",
//       "borrowCap": "0",
//       "totalBorrow": "6412773.614383"
//     },
//     {
//       "token": {
//         "chainId": 42161,
//         "address": "0xaf88d065e77c8cC2239327C5EDb3A432268e5831",
//         "decimals": 6,
//         "symbol": "USDC",
//         "name": "USD Coin"
//       },
//       "price": "1.00005167",
//       "balance": "683209.524823",
//       "apy": "0.56682419999364271168",
//       "lstApy": "0",
//       "grossApy": "0.56682419999364271168",
//       "borrowMin": "0",
//       "borrowCap": "0",
//       "totalBorrow": "14182714.92319"
//     },
//     {
//       "token": {
//         "chainId": 42161,
//         "address": "0xDA10009cBd5D07dd0CeCc66161FC93D7c9000da1",
//         "decimals": 18,
//         "symbol": "DAI",
//         "name": "Dai Stablecoin"
//       },
//       "price": "1.00008749",
//       "balance": "0",
//       "apy": "0.46649664229510928389",
//       "lstApy": "0",
//       "grossApy": "0.46649664229510928389",
//       "borrowMin": "0",
//       "borrowCap": "0",
//       "totalBorrow": "302115.82003522250341245"
//     },
//     {
//       "token": {
//         "chainId": 42161,
//         "address": "0x0000000000000000000000000000000000000000",
//         "decimals": 18,
//         "symbol": "ETH",
//         "name": "Ethereum"
//       },
//       "price": "3693.9254",
//       "balance": "0",
//       "apy": "0.09645232539347118479",
//       "lstApy": "0",
//       "grossApy": "0.09645232539347118479",
//       "borrowMin": "0",
//       "borrowCap": "0",
//       "totalBorrow": "13847.188505152656025417"
//     },
//     {
//       "token": {
//         "chainId": 42161,
//         "address": "0x5979D7b546E38E414F7E9822514be443A4800529",
//         "decimals": 18,
//         "symbol": "wstETH",
//         "name": "Wrapped liquid staked Ether 2.0"
//       },
//       "price": "4290.67538678",
//       "balance": "0",
//       "apy": "0.08847706308451713951",
//       "lstApy": "0.0333",
//       "grossApy": "0.05517706308451713951",
//       "borrowMin": "0",
//       "borrowCap": "0",
//       "totalBorrow": "2970.716041389107318538"
//     },
//     {
//       "token": {
//         "chainId": 42161,
//         "address": "0x912CE59144191C1204E64559FE8253a0e49E6548",
//         "decimals": 18,
//         "symbol": "ARB",
//         "name": "Arbitrum"
//       },
//       "price": "1.5594",
//       "balance": "0",
//       "apy": "0.06860262760727258274",
//       "lstApy": "0",
//       "grossApy": "0.06860262760727258274",
//       "borrowMin": "0",
//       "borrowCap": "0",
//       "totalBorrow": "2084566.740882688263324304"
//     }
//   ]
// }
```

You may also get all the existing portfolios of a user across protocols. &#x20;

```typescript
const portfolios = await adapter.getPortfolios(account);
```

## 4. Select a use case

Lending SDK supports all the popular operations of position management and lending-related operations across different lending platforms. Every operation includes

* User account&#x20;
* User's portfolio on the protocol
* Token information includes source token, source token amount, and destination token. The role of the source token and the destination token depends on the operation. Details will be explained in the following section.
* The acceptable slippage for token swap in the operation.

The supported operations are&#x20;

* [Open by collateral](open-by-collateral.md): swap any token to supply collateral, and achieve the desired collateral exposure in a single step by using a flash loan.
* [Open by debt](open-by-debt.md): swap any token to supply collateral, and achieve the desired debt exposure in a single step by using a flash loan.
* [Close](close.md): empty all positions in a single step by using a flash loan to repay borrowed assets and withdraw deposited assets.
* [Leverage by collateral](leverage-by-collateral.md): achieve the desired collateral exposure in a single step by using a flash loan.
* [Leverage by debt](leverage-by-debt.md): achieve the desired debt exposure in a single step by using a flash loan.
* [Deleverage](deleverage.md): reduce the collateral exposure in a single step by using a flash loan to repay the borrowed asset.
* [Collateral swap](collateral-swap.md): replace one collateral asset with another in a single step by using a flash loan.
* [Debt swap](debt-swap.md): replace one loan asset with another in a single step by using a flash loan.
* [Zap supply](../../compound-kit/sdk/zap-supply.md): swap any token to supply token in one transaction.
* [Zap withdraw](../../compound-kit/sdk/zap-withdraw.md): withdraw then swap to any token in one transaction.
* [Zap repay](../../compound-kit/sdk/zap-repay.md): swap any token to repay the debt in one transaction.
* [Zap borrow](../../compound-kit/sdk/zap-borrow.md): borrow then swap to any token in one transaction.

Depending on the selected use case, users will need to input different parameters, and the SDK will generate the expected outcome, including

* Destination token amount.
* User portfolio after the operation is executed.
* Logics involved in the operation.

If users are satisfied with the expected outcome and the fees, they can sign and submit the approval and transaction data. Typically, the approval is a one-time requirement for allowing Protocolink to act as a manager or a Permit2 token approval (which poses no risk as each user has their agent within Protocolink - for further details, please visit [Broken link](broken-reference "mention")).
