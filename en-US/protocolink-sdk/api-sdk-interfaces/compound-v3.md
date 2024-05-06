# Compound V3

In this section, we will introduce the Compound V3 SDK interfaces, which provide developers with a convenient and efficient way to interact with the Compound V3 protocol. These interfaces cover various aspects of the protocol, including supply, withdraw, borrow, repay, and claim. They are designed to be used easily and flexibly.

Before we dive into the SDK interfaces, we would like to provide an overview of the features of Compound V3. If you are already familiar with Compound V3, feel free to skip this section. For those who are new to Compound V3, here's a brief explanation:

Each market in Compound V3 is independent, meaning that any action performed on the protocol must specify which market it's interacting with. For example, Ethereum has markets for USDC and ETH. Polygon has a USDC market.

Once you have identified the market you want to interact with, you can choose to either be a Lender or a Borrower.&#x20;

* If you want to be a Lender, you need to supply the base token, such as USDC in the Ethereum USDC market. In return, the market will mint cUSDCv3 tokens for you.
* If you want to be a Borrower, you must deposit non-base tokens as collateral and borrow base token. For instance, in the Ethereum USDC market, you can deposit ETH or WBTC as collateral, but the market won't mint any cTokens for you. Instead, it will update the collateral value in the contract, and you can then borrow USDC.

Please note that Compound V3 is subject to change and improvements, and we will update our SDK accordingly.

The following section will introduce the interfaces related to the Compound V3 protocol, which can be accessed through the `api.protocols.compoundv3.` prefix.

## SupplyBase

The following code defines interfaces and functions related to the Compound V3 supply base logic:

### Types

* **SupplyBaseParams**: A type that represents the input parameters for the Compound V3 supply base logic

```typescript
interface SupplyBaseParams {
  marketId: string;
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
}
```

* **SupplyBaseFields**: A type that represents the fields required for the Compound V3 supply base logic.

```typescript
interface SupplyBaseFields {
  marketId: string;
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
}
```

* **SupplyBaseLogic**: An interface that extends the `Logic` interface and represents the Compound V3 supply base logic. It includes the `rid`, and `fields` properties.

```typescript
interface SupplyBaseLogic {
  rid: string;
  fields: SupplyBaseFields;
}
```

### Functions

* **getSupplyBaseTokenList(chainId: number)**: An asynchronous function that retrieves the list of tokens supported by the Compound V3 supply base logic on the specified `chainId`.
* **getSupplyBaseQuotation(chainId: number, params: SupplyBaseParams)**: An asynchronous function that retrieves a quotation for supplying base token on the Compound V3 protocol with the specified `params` object on the specified `chainId`.
* **newSupplyBaseLogic(fields: SupplyBaseFields)**: A function that creates the Compound V3 supply base logic data with the given `fields` object.

### Example Code

```typescript
import * as api from '@protocolink/api';
import * as logics from '@protocolink/logics';

const chainId = 1;
const marketId = logics.compoundv3.MarketId.USDC;

const tokenList = await api.protocols.compoundv3.getSupplyBaseTokenList(chainId);
const baseToken = tokenList[marketId][0][0];
const cToken = tokenList[marketId][0][1];

const supplyBaseQuotation = await api.protocols.compoundv3.getSupplyBaseQuotation(chainId, {
  marketId,
  input: {
    token: baseToken,
    amount: '10',
  },
  tokenOut: cToken,
});

const supplyBaseLogic = await api.protocols.compoundv3.newSupplyBaseLogic(supplyBaseQuotation);
```

## SupplyCollateral

The following code defines interfaces and functions related to the Compound V3 supply collateral logic:

### Types

* **SupplyCollateralFields**: A type that represents the fields required for the Compound V3 supply collateral logic.

```typescript
interface SupplyCollateralFields {
  marketId: string;
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

* **SupplyCollateralLogic**: An interface that extends the `Logic` interface and represents the Compound V3 supply collateral logic. It includes the `rid`, and `fields` properties.

```typescript
interface SupplyCollateralLogic {
  rid: string;
  fields: SupplyCollateralFields;
}
```

### Functions

* **getSupplyCollateralTokenList(chainId: number)**: An asynchronous function that retrieves the list of tokens supported by the Compound V3 supply collateral logic on the specified `chainId`.
* **newSupplyCollateralLogic(fields: SupplyCollateralFields)**: A function that creates the Compound V3 supply collateral logic data with the given `fields` object.

### Example Code

```typescript
import * as api from '@protocolink/api';
import * as logics from '@protocolink/logics';

const chainId = 1;
const marketId = logics.compoundv3.MarketId.USDC;

const tokenList = await api.protocols.compoundv3.getSupplyCollateralTokenList(chainId);
const asset = tokenList[marketId][0];

const supplyCollateralLogic = await api.protocols.compoundv3.newSupplyCollateralLogic({
  marketId,
  input: {
    token: asset,
    amount: '10',
  },
});
```

## WithdrawBase

The following code defines interfaces and functions related to the Compound V3 withdraw base logic:

### Types

* **WithdrawBaseParams**: A type that represents the input parameters for the Compound V3 withdraw base logic

```typescript
interface WithdrawBaseParams {
  marketId: string;
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
}
```

* **WithdrawBaseFields**: A type that represents the fields required for the Compound V3 withdraw base logic.

```typescript
interface WithdrawBaseFields {
  marketId: string;
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
}
```

* **WithdrawBaseLogic**: An interface that extends the `Logic` interface and represents the Compound V3 withdraw base logic. It includes the `rid`, and `fields` properties.

```typescript
interface WithdrawBaseLogic {
  rid: string;
  fields: WithdrawBaseFields;
}
```

### Functions

* **getWithdrawBaseTokenList(chainId: number)**: An asynchronous function that retrieves the list of tokens supported by the Compound V3 withdraw base logic on the specified `chainId`.
* **getWithdrawBaseQuotation(chainId: number, params: WithdrawBaseParams)**: An asynchronous function that retrieves a quotation for withdrawing base token from the Compound V3 protocol with the specified `params` object on the specified `chainId`.
* **newWithdrawBaseLogic(fields: WithdrawBaseFields)**: A function that creates the Compound V3 withdraw base logic data with the given `fields` object.

### Example Code

```typescript
import * as api from '@protocolink/api';
import * as logics from '@protocolink/logics';

const chainId = 1;
const marketId = logics.compoundv3.MarketId.USDC;

const tokenList = await api.protocols.compoundv3.getWithdrawBaseTokenList(chainId);
const cToken = tokenList[marketId][0][0];
const baseToken = tokenList[marketId][0][1];

const withdrawBaseQuotation = await api.protocols.compoundv3.getWithdrawBaseQuotation(chainId, {
  marketId,
  input: {
    token: cToken,
    amount: '10',
  },
  tokenOut: baseToken,
});

const withdrawBaseLogic = await api.protocols.compoundv3.newWithdrawBaseLogic(withdrawBaseQuotation);
```

## WithdrawCollateral

The following code defines interfaces and functions related to the Compound V3 withdraw collateral logic:

### Types

* **WithdrawCollateralFields**: A type that represents the fields required for the Compound V3 withdraw collateral logic.

<pre class="language-typescript"><code class="lang-typescript">interface WithdrawCollateralFields {
  marketId: string;
  output: {
    token: {
      chainId: number;
      address: string;
      decimals: number;
      symbol: string;
      name: string;
    };
<strong>    amount: string;
</strong>  };
}
</code></pre>

* **WithdrawCollateralLogic**: An interface that extends the `Logic` interface and represents the Compound V3 withdraw collateral logic. It includes the `rid`, and `fields` properties.

```typescript
interface WithdrawCollateralLogic {
  rid: string;
  fields: WithdrawCollateralFields;
}
```

### Functions

* **getWithdrawCollateralTokenList(chainId: number)**: An asynchronous function that retrieves the list of tokens supported by the Compound V3 withdraw collateral logic on the specified `chainId`.
* **newWithdrawCollateralLogic(fields: WithdrawCollateralFields)**: A function that creates the Compound V3 withdraw collateral logic data with the given `fields` object.

### Example Code

```typescript
import * as api from '@protocolink/api';
import * as logics from '@protocolink/logics';

const chainId = 1;
const marketId = logics.compoundv3.MarketId.USDC;

const tokenList = await api.protocols.compoundv3.getWithdrawCollateralTokenList(chainId);
const asset = tokenList[marketId][0];

const withdrawCollateralLogic = await api.protocols.compoundv3.newWithdrawCollateralLogic({
  marketId,
  output: {
    token: asset,
    amount: '10',
  },
});
```

## Borrow

The following code defines interfaces and functions related to the Compound V3 borrow logic:

### Types

* **BorrowFields**: A type that represents the fields required for the Compound V3 borrow logic.

```typescript
interface BorrowFields {
  marketId: string;
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
}
```

* **BorrowLogic**: An interface that extends the `Logic` interface and represents the Compound V3 borrow logic. It includes the `rid`, and `fields` properties.

```typescript
interface BorrowLogic {
  rid: string;
  fields: BorrowFields;
}
```

### Functions

* **getBorrowTokenList(chainId: number)**: An asynchronous function that retrieves the list of tokens supported by the Compound V3 borrow logic on the specified `chainId`.
* **newBorrowLogic(fields: BorrowFields)**: A function that creates the Compound V3 borrow logic data with the given `fields` object.

### Example Code

```typescript
import * as api from '@protocolink/api';
import * as logics from '@protocolink/logics';

const chainId = 1;
const marketId = logics.compoundv3.MarketId.USDC;

const tokenList = await api.protocols.compoundv3.getBorrowTokenList(chainId);
const baseToken = tokenList[marketId][0];

const borrowLogic = await api.protocols.compoundv3.newBorrowLogic({
  marketId,
  output: {
    token: baseToken,
    amount: '10',
  },
});
```

## Repay

The following code defines interfaces and functions related to the Compound V3 repay logic:

### Types

* **RepayParams**: A type that represents the input parameters for the Compound V3 repay logic

```typescript
interface RepayParams {
  marketId: string;
  tokenIn: {
    chainId: number;
    address: string;
    decimals: number;
    symbol: string;
    name: string;
  };
  borrower: string;
}
```

* **RepayFields**: A type that represents the fields required for the Compound V3 repay logic.

```typescript
interface RepayFields {
  marketId: string;
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
  borrower: string;
}
```

* **RepayLogic**: An interface that extends the `Logic` interface and represents the Compound V3 repay logic. It includes the `rid`, and `fields` properties.

```typescript
interface RepayLogic {
  rid: string;
  fields: RepayFields;
}
```

### Functions

* **getRepayTokenList(chainId: number)**: An asynchronous function that retrieves the list of tokens supported by the Compound V3 repay logic on the specified `chainId`.
* **getRepayQuotation(chainId: number, params: RepayParams)**: An asynchronous function that retrieves a quotation for repaying a loan on the Compound V3 protocol with the specified `params` object on the specified `chainId`.
* **newRepayLogic(fields: RepayFields)**: A function that creates the Compound V3 repay logic data with the given `fields` object.

### Example Code

```typescript
import * as api from '@protocolink/api';
import * as logics from '@protocolink/logics';

const chainId = 1;
const marketId = logics.compoundv3.MarketId.USDC;
const account = '0xaAaAaAaaAaAaAaaAaAAAAAAAAaaaAaAaAaaAaaAa';

const tokenList = await api.protocols.compoundv3.getRepayTokenList(chainId);
const baseToken = tokenList[marketId][0];

const repayQuotation = await api.protocols.compoundv3.getRepayQuotation(chainId, {
  marketId,
  tokenIn: baseToken,
  borrower: account,
});

const repayLogic = await api.protocols.compoundv3.newRepayLogic(repayQuotation);
```

## Claim

The following code defines interfaces and functions related to the Compound V3 claim logic:

### Types

* **ClaimParams**: A type that represents the input parameters for the Compound V3 claim logic

```typescript
interface ClaimParams {
  marketId: string;
  owner: string;
}
```

* **ClaimFields**: A type that represents the fields required for the Compound V3 claim logic.

```typescript
interface ClaimFields {
  marketId: string;
  owner: string;
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
}
```

* **ClaimLogic**: An interface that extends the `Logic` interface and represents the Compound V3 claim logic. It includes the `rid`, and `fields` properties.

```typescript
interface ClaimLogic {
  rid: string;
  fields: ClaimFields;
}
```

### Functions

* **getClaimTokenList(chainId: number)**: An asynchronous function that retrieves the list of tokens supported by the Compound V3 withdraw base logic on the specified `chainId`.
* **getClaimQuotation(chainId: number, params: ClaimParams)**: An asynchronous function that retrieves a quotation for claiming COMP on the Compound V3 protocol with the specified `params` object on the specified `chainId`.
* **newClaimLogic(fields: ClaimFields)**: A function that creates the Compound V3 claim logic data with the given `fields` object.

### Example Code

```typescript
import * as api from '@protocolink/api';
import * as logics from '@protocolink/logics';

const chainId = 1;
const marketId = logics.compoundv3.MarketId.USDC;
const account = '0xaAaAaAaaAaAaAaaAaAAAAAAAAaaaAaAaAaaAaaAa';

const tokenList = await api.protocols.compoundv3.getClaimTokenList(chainId);
const COMP = tokenList[0];

const claimQuotation = await api.protocols.compoundv3.getClaimQuotation(chainId, {
  marketId,
  owner: account,
});

const claimLogic = await api.protocols.compoundv3.newClaimLogic(claimQuotation);
```
