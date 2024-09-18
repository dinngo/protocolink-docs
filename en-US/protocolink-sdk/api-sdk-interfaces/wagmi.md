# Wagmi

In this section, we will introduce the Wagmi SDK interfaces, which provide developers with a convenient and efficient way to interact with the Wagmi protocol. These interfaces are related to swap token and are designed to be used easily and flexibly.

The following section will introduce the interfaces related to the Wagmi protocol, which can be accessed through the `api.protocols.wagmi.` prefix.

## SwapToken <a href="#swaptoken" id="swaptoken"></a>

The following code defines interfaces and functions related to the Wagmi swap token logic:

## Types <a href="#types" id="types"></a>

* **SwapTokenParams**: A type that represents the input parameters for the Wagmi swap token logic

```typescript
type SwapTokenParams =
  | {
      input: {
        token: {
          chainId: number;
          address: string;
          decimals: number;
          symbol: string;
          name: string;
        };
        amount: string;
      };
      tokenOut: {
        chainId: number;
        address: string;
        decimals: number;
        symbol: string;
        name: string;
      };
      slippage?: number;
    }
  | {
      tokenIn: {
        chainId: number;
        address: string;
        decimals: number;
        symbol: string;
        name: string;
      };
      output: {
        token: {
          chainId: number;
          address: string;
          decimals: number;
          symbol: string;
          name: string;
        };
        amount: string;
      };
      slippage?: number;
    };
```

* **SwapTokenFields**: A type that represents the fields required for the Wagmi swap token logic.

```typescript
import * as core from '@protocolink/core';

interface SwapTokenFields {
  tradeType: core.TradeType;
  input: {
    token: {
      chainId: number;
      address: string;
      decimals: number;
      symbol: string;
      name: string;
    };
    amount: string;
  };
  output: {
    token: {
      chainId: number;
      address: string;
      decimals: number;
      symbol: string;
      name: string;
    };
    amount: string;
  };
  fee?: number;
  path?: string;
  slippage?: number;
}
```

* **SwapTokenLogic**: An interface that extends the `Logic` interface and represents the Iolend swap token logic. It includes the `rid`, and `fields` properties.

```typescript
interface SwapTokenLogic {
  rid: string;
  fields: SwapTokenFields;
}
```

## Functions <a href="#types" id="types"></a>

* **getSwapTokenTokenList(chainId: number)**: An asynchronous function that retrieves the list of tokens supported by the Wagmi swap token logic on the specified `chainId`.
* **getSwapTokenQuotation(chainId: number, params: SwapTokenParams)**: An asynchronous function that retrieves a quotation for swapping assets on the Wagmi protocol with the specified `params` object on the specified `chainId`.
* **newSwapTokenLogic(fields: SwapTokenFields)**: A function that creates the Wagmi swap token logic data with the given `fields` object.

## Example Code <a href="#types" id="types"></a>

```typescript
import * as api from '@protocolink/api';

const chainId = 8822;

const tokenList = await api.protocols.wagmi.getSwapTokenTokenList(chainId);
const tokenIn = tokenList[0];
const tokenOut = tokenList[2];

const swapTokenQuotation = await api.protocols.wagmi.getSwapTokenQuotation(chainId, {
  input: {
    token: tokenIn,
    amount: '10',
  },
  tokenOut,
  slippage: 100, // 1%
});

const swapTokenLogic = await api.protocols.wagmi.newSwapTokenLogic(swapTokenQuotation);
```
