# Permit2

In this section, we will introduce the Permit2 SDK interface, which provides developers with a convenient and efficient way to interact with the Permit2 protocol. These interfaces are related to pull token and are designed to be used easily and flexibly.

The following section will introduce the interfaces related to the Permit2 protocol, which can be accessed through the `api.protocols.permit2.` prefix.

## PullToken

The following code defines interfaces and functions related to the Permit2 pull token logic:

### Types

* **PullTokenFields**: A type that represents the fields required for the Permit2 pull token logic.

```typescript
interface PullTokenFields {
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
}
```

* **PullTokenLogic**: An interface that extends the `Logic` interface and represents the Permit2 pull token logic. It includes the `rid`, and `fields` properties.

```typescript
interface PullTokenLogic {
  rid: string;
  fields: PullTokenFields;
}
```

### Functions

* **getPullTokenTokenList(chainId: number)**: An asynchronous function that retrieves the list of tokens supported by the Permit2 pull token logic on the specified `chainId`.
* **newPullTokenLogic(fields: PullTokenFields)**: A function that creates the Permit2 pull token logic data with the given `fields` object.

### Example Code

```typescript
import * as api from '@protocolink/api';
import * as logics from '@protocolink/logics';

const chainId = 1;

const tokenList = await api.protocols.permit2.getPullTokenTokenList(chainId);
const asset = tokenList[0];

const pullTokenLogic = await api.protocols.permit2.newPullTokenLogic({
  input: {
    token: asset,
    amount: '10',
  },
});
```
