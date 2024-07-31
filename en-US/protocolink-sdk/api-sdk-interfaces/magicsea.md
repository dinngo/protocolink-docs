# Magicsea

In this section, we will introduce the Magicsea SDK interfaces, which provide developers with a convenient and efficient way to interact with the Magicsea protocol. These interfaces are related to swap token and are designed to be used easily and flexibly.

The following section will introduce the interfaces related to the Magicsea protocol, which can be accessed through the `api.protocols.magicsea.` prefix.

## SwapToken

The following code defines interfaces and functions related to the Magicsea swap token logic:

### Types

**SwapTokenParams**: A type that represents the input parameters for the Magicsea swap token logic

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

**SwapTokenFields**: A type that represents the fields required for the Magicsea swap token logic.

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
  path: string[];
  slippage?: number;
}
```

**SwapTokenLogic**: An interface that extends the `Logic` interface and represents the Magicsea swap token logic. It includes the `rid`, and `fields` properties.

```typescript
interface SwapTokenLogic {
  rid: string;
  fields: SwapTokenFields;
}
```

### Functions

* **getSwapTokenTokenList(chainId: number)**: An asynchronous function that retrieves the list of tokens supported by the Magicsea swap token logic on the specified `chainId`.
* **getSwapTokenQuotation(chainId: number, params: SwapTokenParams)**: An asynchronous function that retrieves a quotation for swapping assets on the Magicsea protocol with the specified `params` object on the specified `chainId`.
* **newSwapTokenLogic(fields: SwapTokenFields)**: A function that creates the Magicsea swap token logic data with the given `fields` object.

### Example Code

```typescript
import * as api from '@protocolink/api';

const chainId = 8822;

const tokenList = await api.protocols.magicsea.getSwapTokenTokenList(chainId);
const tokenIn = tokenList[0];
const tokenOut = tokenList[2];

const swapTokenQuotation = await api.protocols.magicsea.getSwapTokenQuotation(chainId, {
  input: {
    token: tokenIn,
    amount: '10',
  },
  tokenOut,
  slippage: 100, // 1%
});

const swapTokenLogic = await api.protocols.magicsea.newSwapTokenLogic(swapTokenQuotation);
```
