# Stargate V2

In this section, we will introduce the Stargate V2 SDK interfaces, which provide developers with a convenient and efficient way to interact with the Stargate protocol. These interfaces are related to swap token and are designed to be used easily and flexibly.

## SwapToken

The following code defines interfaces and functions related to the Stargate V2 swap token logic:

### Types

* **SwapTokenParams**: A type that represents the input parameters for the Stargate swap token logic

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
  receiver: string;
  slippage?: number;
}
```

**SwapTokenFields**: A type that represents the fields required for the Stargate swap token logic.

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
  receiver: string;
  fee: string;
  oftFee: string;
  slippage?: number;
}
```

**SwapTokenLogic**: An interface that extends the `Logic` interface and represents the Stargate V2 swap token logic. It includes the `rid`, and `fields` properties.

```typescript
interface SwapTokenLogic {
  rid: string;
  fields: SwapTokenFields;
}
```

### Functions

* **getSwapTokenTokenList(chainId: number)**: An asynchronous function that retrieves the list of tokens supported by the Stargate V2 swap token logic on the specified `chainId`.
* **getSwapTokenQuotation(chainId: number, params: SwapTokenParams)**: An asynchronous function that retrieves a quotation for swapping assets on the Stargate V2 protocol with the specified `params` object on the specified `chainId`.
* **newSwapTokenLogic(fields: SwapTokenFields)**: A function that creates the Stargate V2 swap token logic data with the given `fields` object.

### Example Code

```typescript
import * as api from '@protocolink/api';

const chainId = 56;
const receiver = '0xaAaAaAaaAaAaAaaAaAAAAAAAAaaaAaAaAaaAaaAa';

const tokenList = await api.protocols.stargatev2.getSwapTokenTokenList(chainId);
const tokenIn = tokenList[0].srcToken;
const tokenOut = tokenList[0].destTokens[0];

const swapTokenQuotation = await api.protocols.stargatev2.getSwapTokenQuotation(chainId, {
  input: {
    token: tokenIn,
    amount: '10',
  },
  tokenOut,
  receiver,
  slippage: 100, // 1%
});

const swapTokenLogic = await api.protocols.stargatev2.newSwapTokenLogic(swapTokenQuotation);
```
