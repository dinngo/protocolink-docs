# SyncSwap

In this section, we will introduce the SyncSwap SDK interfaces, which provide developers with a convenient and efficient way to interact with the SyncSwap protocol. These interfaces cover various aspects of the protocol, including swap token and are designed to be easy to use and flexible.

In the following section, we will introduce the interfaces related to the SyncSwap protocol, which can be accessed through the `api.protocols.syncswap.` prefix.

## SwapToken

The following code defines interfaces and functions related to the SyncSwap swap token logic:

### Types

* **SwapTokenParams**: A type that represents the input parameters for the SyncSwap swap token logic

```typescript
interface SwapTokenParams {
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

```

* **SwapTokenFields**: A type that represents the fields required for the SyncSwap swap token logic.

```typescript
interface SwapTokenFields {
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
  paths?: {
    steps: {
      pool: string;
      tokenIn: string;
    }[];
    tokenIn: string;
    amountIn: string;
  }[];
  slippage?: number;
}
```

* **SwapTokenLogic**: An interface that extends the `Logic` interface and represents the SyncSwap swap token logic. It includes the `rid`, and `fields` properties.

```typescript
interface SwapTokenLogic {
  rid: string;
  fields: SwapTokenFields;
}
```

### Functions

* **getSwapTokenTokenList(chainId: number)**: An asynchronous function that retrieves the list of tokens supported by the SyncSwap swap token logic on the specified `chainId`.
* **getSwapTokenQuotation(chainId: number, params: SwapTokenParams)**: An asynchronous function that retrieves a quotation for swapping assets on the SyncSwap protocol with the specified `params` object on the specified `chainId`.
* **newSwapTokenLogic(fields: SwapTokenFields)**: A function that creates the SyncSwap swap token logic data with the given `fields` object.

### Example Code

```typescript
import * as api from '@protocolink/api';

const chainId = 324;

const tokenList = await api.protocols.syncswap.getSwapTokenTokenList(chainId);
const tokenIn = tokenList[0];
const tokenOut = tokenList[2];

const swapTokenQuotation = await api.protocols.syncswap.getSwapTokenQuotation(chainId, {
  input: {
    token: tokenIn,
    amount: '10',
  },
  tokenOut,
  slippage: 100, // 1%
});

const swapTokenLogic = await api.protocols.syncswap.newSwapTokenLogic(swapTokenQuotation);
```
