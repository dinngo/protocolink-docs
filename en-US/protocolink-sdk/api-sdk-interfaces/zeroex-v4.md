# ZeroEx V4

In this section, we will introduce the ZeroEx V4 SDK interfaces, which provide developers with a convenient and efficient way to interact with the ZeroEx V4 protocol. These interfaces are related to swap token and are designed to be used easily and flexibly.

The following section will introduce the interfaces related to the ZeroEx V4 protocol, which can be accessed through the `api.protocols.zeroexv4.` prefix.

## SwapToken

The following code defines interfaces and functions related to the ZeroEx V4 swap token logic:

### Types

* **SwapTokenParams**: A type that represents the input parameters for the ZeroEx V4 swap token logic

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
      apiKey: string;
    }
```

**SwapTokenFields**: A type that represents the fields required for the ZeroEx V4 swap token logic.

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
  slippage?: number;
  apiKey: string;
}
```

**SwapTokenLogic**: An interface that extends the `Logic` interface and represents the ZeroEx V4 swap token logic. It includes the `rid`, and `fields` properties.

```typescript
interface SwapTokenLogic {
  rid: string;
  fields: SwapTokenFields;
}
```

### Functions

* **getSwapTokenTokenList(chainId: number)**: An asynchronous function that retrieves the list of tokens supported by the ZeroEx V4 swap token logic on the specified `chainId`.
* **getSwapTokenQuotation(chainId: number, params: SwapTokenParams)**: An asynchronous function that retrieves a quotation for swapping assets on the ZeroEx V4 protocol with the specified `params` object on the specified `chainId`.
* **newSwapTokenLogic(fields: SwapTokenFields)**: A function that creates the ZeroEx V4 swap token logic data with the given `fields` object.

```typescript
import * as api from '@protocolink/api';

const chainId = 1;
const apiKey = process.env.ZEROEX_API_KEY as string;

const tokenList = await api.protocols.zeroexv4.getSwapTokenTokenList(chainId);
const tokenIn = tokenList[0];
const tokenOut = tokenList[2];

const swapTokenQuotation = await api.protocols.zeroexv4.getSwapTokenQuotation(chainId, {
  input: {
    token: tokenIn,
    amount: '10',
  },
  tokenOut,
  slippage: 100, // 1%
  apiKey,
});

const swapTokenLogic = await api.protocols.zeroexv4.newSwapTokenLogic(swapTokenQuotation);
```



