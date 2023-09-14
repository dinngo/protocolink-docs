# 2⃣ Build Logics

## **Trading strategy**

Before using the SDK, it's important to have a clear understanding of your trading **strategy**. This will help you make informed decisions when using the SDK to send transactions. Below, we provide an example scenario to help illustrate how to define a trading strategy. You can use this as a guide to customize your own strategy.

**Example:** Suppose a user has 1000 USDC in Ethereum and wants to swap it for WBTC to supply the WBTC pool on the Aave V3 lending platform and earn interest. The user's address is `0xaAaAaAaaAaAaAaaAaAAAAAAAAaaaAaAaAaaAaaAa`.&#x20;

Typically, the user needs to perform two actions:&#x20;

1. Exchange **1000 USDC** to **WBTC** by **Uniswap V3**&#x20;
2. Supply **WBTC** to get **aWBTC** by **Aave V3**

Each of the actions mentioned above represents a **`Logic`** in Protocolink.&#x20;

Let's start building these logics. Here you can find [**example code**](https://github.com/dinngo/protocolink-js-sdk/blob/master/packages/api/examples/uniswap-v3-swap-and-aave-v3-supply.ts)**.**

## Logic 1: Swap

The user can swap 1000 USDC for WBTC using the SwapTokenLogic of Uniswap V3.

1. Use `api.protocols.uniswapv3.getSwapTokenQuotation` to get a quotation. Additionally, slippage is optional. The type should be a number, and the value should be in Basis Points, where 1 Basis Point equals 0.01%.
2. Use `api.protocols.uniswapv3.newSwapTokenLogic` to build the swap Logic data.

Below is the code snippet:

```typescript
import * as api from '@protocolink/api';
import * as common from '@protocolink/common';

const chainId = common.ChainId.mainnet;

const USDC = {
  chainId: 1,
  address: '0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48',
  decimals: 6,
  symbol: 'USDC',
  name: 'USD Coin',
};
const WBTC = {
  chainId: 1,
  address: '0x2260FAC5E5542a773Aa44fBCfeDf7C193bc2C599',
  decimals: 8,
  symbol: 'WBTC',
  name: 'Wrapped BTC',
};

const swapQuotation = await api.protocols.uniswapv3.getSwapTokenQuotation(chainId, {
  input: { token: USDC, amount: '1000' },
  tokenOut: WBTC,
  slippage: 100, // 1%
});

const swapLogic = api.protocols.uniswapv3.newSwapTokenLogic(swapQuotation)
```

## Logic 2: Supply

Then you can take the output of the previous `swapQuotation` and use it as an input to get the supply quotation.

1. Use `api.protocols.aavev3.getSupplyQuotation` to get a quote for supplying WBTC, which will provide a 1:1 aEthWBTC token.
2. Use `api.protocols.aavev3.newSupplyLogic` to build the supply Logic data.

```typescript
import * as api from '@protocolink/api';

const supplyQuotation = await api.protocols.aavev3.getSupplyQuotation(chainId, {
  input: swapQuotation.output,
  tokenOut: aEthWBTC,
});

const supplyLogic = api.protocols.aavev3.newSupplyLogic(supplyQuotation);
```
