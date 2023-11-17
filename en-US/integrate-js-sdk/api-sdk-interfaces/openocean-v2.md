# OpenOcean V2

In this section, we will introduce the OpenOcean V2 SDK interfaces, which provide developers with a convenient and efficient way to interact with the OpenOcean V2 protocol. These interfaces cover various aspects of the protocol, including swap token and are designed to be easy to use and flexible.

In the following section, we will introduce the interfaces related to the OpenOcean V2 protocol, which can be accessed through the `api.protocols.openoceanv2.` prefix.

## SwapToken

The following code defines interfaces and functions related to the OpenOcean V2 swap token logic:

### Types

* **SwapTokenParams**: A type that represents the input parameters for the OpenOcean V2 swap token logic

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
      disableDexIds?: string;
    }
```

* **SwapTokenFields**: A type that represents the fields required for the OpenOcean V2 swap token logic.

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
  disableDexIds?: string;
}
```

* **SwapTokenLogic**: An interface that extends the `Logic` interface and represents the OpenOcean V2 swap token logic. It includes the `rid`, and `fields` properties.

```typescript
interface SwapTokenLogic {
  rid: string;
  fields: SwapTokenFields;
}
```

### Functions

* **getSwapTokenTokenList(chainId: number)**: An asynchronous function that retrieves the list of tokens supported by the OpenOcean V2 swap token logic on the specified `chainId`.
* **getSwapTokenQuotation(chainId: number, params: SwapTokenParams)**: An asynchronous function that retrieves a quotation for swapping assets on the OpenOcean V2 protocol with the specified `params` object on the specified `chainId`.
* **newSwapTokenLogic(fields: SwapTokenFields)**: A function that creates the OpenOcean V2 swap token logic data with the given `fields` object.

```typescript
import * as api from '@protocolink/api';

const chainId = 1088;

const tokenList = await api.protocols.openoceanv2.getSwapTokenTokenList(chainId);
const tokenIn = tokenList[0];
const tokenOut = tokenList[2];

const swapTokenQuotation = await api.protocols.openoceanv2.getSwapTokenQuotation(chainId, {
  input: {
    token: tokenIn,
    amount: '10',
  },
  tokenOut,
  slippage: 100, // 1%
});

const swapTokenLogic = await api.protocols.openoceanv2.newSwapTokenLogic(swapTokenQuotation);
```
