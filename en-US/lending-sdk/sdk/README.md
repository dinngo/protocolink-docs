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

lending.Adapter.registerProtocol(lending.protocols.aavev2.LendingProtocol);
lending.Adapter.registerSwapper(lending.swappers.paraswapv5.LendingSwapper);
```

### 2.2 Initialization

To initialize the `Adapter`, users must specify a `chainId`, with the option to include a `provider` for enhanced flexibility. Once initialized, the `Adapter` enables seamless retrieval of user information and interaction with a range of supported protocols, simplifying the process of integrating blockchain functionalities into various applications.

```typescript
import * as common from '@protocolink/common';

const chainId = common.ChainId.arbitrum;
const adapter = new lending.Adapter(chainId/*, provider*/);
```

## 3. Get the portfolio

Gather the information of a user portfolio. The portfolio is determined by user account, protocol, and market. The portfolio contains information to assist user to identify their position to make further decisions.

```typescript
const account = '0xBf891E7eFCC98A8239385D3172bA10AD593c7886';
const protocolId = 'radiant-v2';
const marketId = 'arbitrum';
const portfolio = await adapter.getPortfolio(account, protocolId, marketId);

// {
//   chainId: 42161,
//   protocolId: 'radiant-v2',
//   marketId: 'arbitrum',
//   utilization: '0.99482640662952741054',
//   healthRate: '1.04721703882927822941',
//   netAPY: '-0.29111736722085137435',
//   totalSupplyUSD: '28931071.94611821305332002113083932',
//   totalBorrowUSD: '22455171.44491027878871862817614778',
//   supplies: [
//     {
//       token: {
//         chainId: 42161,
//         address: '0x2f2a2543B76A4166549F7aaB2e75Bef0aefC5B0f',
//         decimals: 8,
//         symbol: 'WBTC',
//         name: 'Wrapped BTC',
//       },
//       price: '42230.4416495',
//       balance: '99.14372699',
//       apy: '0.00919880889510915271',
//       usageAsCollateralEnabled: true,
//       ltv: '0.7',
//       liquidationThreshold: '0.75',
//       isNotCollateral: false,
//     },
//     {
//       token: {
//         chainId: 42161,
//         address: '0xFd086bC7CD5C481DCC9C85ebE478A1C0b69FCbb9',
//         decimals: 6,
//         symbol: 'USDT',
//         name: 'Tether USD',
//       },
//       price: '0.99993522',
//       balance: '0',
//       apy: '0.04577020387917572561',
//       usageAsCollateralEnabled: true,
//       ltv: '0.8',
//       liquidationThreshold: '0.85',
//       isNotCollateral: false,
//     },
//     {
//       token: {
//         chainId: 42161,
//         address: '0xFF970A61A04b1cA14834A43f5dE4533eBDDB5CC8',
//         decimals: 6,
//         symbol: 'USDC',
//         name: 'USD Coin (Arb1)',
//       },
//       price: '1.00023692',
//       balance: '0',
//       apy: '0.0601275814876277881',
//       usageAsCollateralEnabled: true,
//       ltv: '0.8',
//       liquidationThreshold: '0.85',
//       isNotCollateral: false,
//     },
//     {
//       token: {
//         chainId: 42161,
//         address: '0xDA10009cBd5D07dd0CeCc66161FC93D7c9000da1',
//         decimals: 18,
//         symbol: 'DAI',
//         name: 'Dai Stablecoin',
//       },
//       price: '1.00005605',
//       balance: '356.025961434974232342',
//       apy: '0.04044080289699676233',
//       usageAsCollateralEnabled: true,
//       ltv: '0.75',
//       liquidationThreshold: '0.85',
//       isNotCollateral: false,
//     },
//     {
//       token: {
//         chainId: 42161,
//         address: '0x0000000000000000000000000000000000000000',
//         decimals: 18,
//         symbol: 'ETH',
//         name: 'Ethereum',
//       },
//       price: '2244.01978972',
//       balance: '10340.220231199711325636',
//       apy: '0.01435478420580403125',
//       usageAsCollateralEnabled: true,
//       ltv: '0.8',
//       liquidationThreshold: '0.825',
//       isNotCollateral: false,
//     },
//     {
//       token: {
//         chainId: 42161,
//         address: '0x5979D7b546E38E414F7E9822514be443A4800529',
//         decimals: 18,
//         symbol: 'wstETH',
//         name: 'Wrapped liquid staked Ether 2.0',
//       },
//       price: '2575.13844791',
//       balance: '597.854708759269774618',
//       apy: '0.00680150448981446132',
//       usageAsCollateralEnabled: true,
//       ltv: '0.7',
//       liquidationThreshold: '0.8',
//       isNotCollateral: false,
//     },
//     {
//       token: {
//         chainId: 42161,
//         address: '0x912CE59144191C1204E64559FE8253a0e49E6548',
//         decimals: 18,
//         symbol: 'ARB',
//         name: 'Arbitrum',
//       },
//       price: '1.12526332',
//       balance: '546.580484888309644006',
//       apy: '0.00698937805550445597',
//       usageAsCollateralEnabled: true,
//       ltv: '0.4',
//       liquidationThreshold: '0.5',
//       isNotCollateral: false,
//     },
//   ],
//   borrows: [
//     {
//       token: {
//         chainId: 42161,
//         address: '0x2f2a2543B76A4166549F7aaB2e75Bef0aefC5B0f',
//         decimals: 8,
//         symbol: 'WBTC',
//         name: 'Wrapped BTC',
//       },
//       price: '42230.4416495',
//       balances: ['67.14350092'],
//       apys: ['0.09288334354334835704'],
//     },
//     {
//       token: {
//         chainId: 42161,
//         address: '0xFd086bC7CD5C481DCC9C85ebE478A1C0b69FCbb9',
//         decimals: 6,
//         symbol: 'USDT',
//         name: 'Tether USD',
//       },
//       price: '0.99993522',
//       balances: ['0'],
//       apys: ['0.24556445414442673749'],
//     },
//     {
//       token: {
//         chainId: 42161,
//         address: '0xFF970A61A04b1cA14834A43f5dE4533eBDDB5CC8',
//         decimals: 6,
//         symbol: 'USDC',
//         name: 'USD Coin (Arb1)',
//       },
//       price: '1.00023692',
//       balances: ['239995.369223'],
//       apys: ['0.33344635059000808296'],
//     },
//     {
//       token: {
//         chainId: 42161,
//         address: '0xDA10009cBd5D07dd0CeCc66161FC93D7c9000da1',
//         decimals: 18,
//         symbol: 'DAI',
//         name: 'Dai Stablecoin',
//       },
//       price: '1.00005605',
//       balances: ['0'],
//       apys: ['0.22273058517580570194'],
//     },
//     {
//       token: {
//         chainId: 42161,
//         address: '0x0000000000000000000000000000000000000000',
//         decimals: 18,
//         symbol: 'ETH',
//         name: 'Ethereum',
//       },
//       price: '2244.01978972',
//       balances: ['8636.117912147309336459'],
//       apys: ['0.09927541344639658325'],
//     },
//     {
//       token: {
//         chainId: 42161,
//         address: '0x5979D7b546E38E414F7E9822514be443A4800529',
//         decimals: 18,
//         symbol: 'wstETH',
//         name: 'Wrapped liquid staked Ether 2.0',
//       },
//       price: '2575.13844791',
//       balances: ['0.00000661139690993'],
//       apys: ['0.09438182165452173365'],
//     },
//     {
//       token: {
//         chainId: 42161,
//         address: '0x912CE59144191C1204E64559FE8253a0e49E6548',
//         decimals: 18,
//         symbol: 'ARB',
//         name: 'Arbitrum',
//       },
//       price: '1.12526332',
//       balances: ['0'],
//       apys: ['0.09573188509401746726'],
//     },
//   ],
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
