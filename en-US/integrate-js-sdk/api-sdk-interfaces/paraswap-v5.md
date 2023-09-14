# ParaSwap V5

In this section, we will introduce the ParaSwap V5 SDK interfaces, which provide developers with a convenient and efficient way to interact with the ParaSwap V5 protocol. These interfaces cover various aspects of the protocol, including swap token and are designed to be easy to use and flexible.

In the following section, we will introduce the interfaces related to the ParaSwap V5 protocol, which can be accessed through the `api.protocols.paraswapv5.` prefix.

## SwapToken

The following code defines interfaces and functions related to the ParaSwap V5 swap token logic:

### Types

* **SwapTokenParams**: A type that represents the input parameters for the ParaSwap V5 swap token logic

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
      excludeDEXS?: string[]
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
      excludeDEXS?: string[]
    };
```

* **SwapTokenFields**: A type that represents the fields required for the ParaSwap V5 swap token logic.

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
  excludeDEXS?: string[]
}
```

* **SwapTokenLogic**: An interface that extends the `Logic` interface and represents the ParaSwap V5 swap token logic. It includes the `rid`, and `fields` properties.

```typescript
interface SwapTokenLogic {
  rid: string;
  fields: SwapTokenFields;
}
```

### Functions

* **getSwapTokenTokenList(chainId: number)**: An asynchronous function that retrieves the list of tokens supported by the ParaSwap V5 swap token logic on the specified `chainId`.
* **getSwapTokenQuotation(chainId: number, params: SwapTokenParams)**: An asynchronous function that retrieves a quotation for swaping assets on the ParaSwap V5 protocol with the specified `params` object on the specified `chainId`.
* **newSwapTokenLogic(fields: SwapTokenFields)**: A function that creates the ParaSwap V5 swap token logic data with the given `fields` object.

### Example Code

```typescript
import * as api from '@protocolink/api';

const chainId = 1;

const tokenList = await api.protocols.paraswapv5.getSwapTokenTokenList(chainId);
const tokenIn = tokenList[0];
const tokenOut = tokenList[2];

const swapTokenQuotation = await api.protocols.paraswapv5.getSwapTokenQuotation(chainId, {
  input: {
    token: tokenIn,
    amount: '10',
  },
  tokenOut,
  slippage: 100, // 1%
});

const swapTokenLogic = await api.protocols.paraswapv5.newSwapTokenLogic(swapTokenQuotation);
```
