# Aave V3

In this section, we will introduce the Aave V3 SDK interfaces, which provide developers with a convenient and efficient way to interact with the Aave V3 protocol. These interfaces cover various aspects of the protocol, including supply, withdraw, borrow, repay, and flash loan, and are designed to be easy to use and flexible.

In the following section, we will introduce the interfaces related to the Aave V3 protocol, which can be accessed through the `api.protocols.aavev3.` prefix.

## Supply

The following code defines interfaces and functions related to the Aave V3 supply logic:

### Types

* **SupplyParams**: A type that represents the input parameters for the Aave V3 supply logic

```typescript
interface SupplyParams {
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

* **SupplyFields**: A type that represents the fields required for the Aave V3 supply logic.

```typescript
interface SupplyFields {
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

* **SupplyLogic**: An interface that extends the `Logic` interface and represents the Aave V3 supply logic. It includes the `rid`, and `fields` properties.

```typescript
interface SupplyLogic {
  rid: string;
  fields: SupplyFields;
}
```

### Functions

* **getSupplyTokenList(chainId: number)**: An asynchronous function that retrieves the list of tokens supported by the Aave V3 supply logic on the specified `chainId`.
* **getSupplyQuotation(chainId: number, params: SupplyParams)**: An asynchronous function that retrieves a quotation for supplying assets on the Aave V3 protocol with the specified `params` object on the specified `chainId`.
* **newSupplyLogic(fields: SupplyFields)**: A function that creates the Aave V3 supply logic data with the given `fields` object.

### Example Code

```typescript
import * as api from '@protocolink/api';

const chainId = 1;

const tokenList = await api.protocols.aavev3.getSupplyTokenList(chainId);
const underlyingToken = tokenList[0][0];
const aToken = tokenList[0][1];

const supplyQuotation = await api.protocols.aavev3.getSupplyQuotation(chainId, {
  input: {
    token: underlyingToken,
    amount: '10',
  },
  tokenOut: aToken,
});

const supplyLogic = await api.protocols.aavev3.newSupplyLogic(supplyQuotation);
```

## Withdraw

The following code defines interfaces and functions related to the Aave V3 withdraw logic:

### Types

* **WithdrawParams**: A type that represents the input parameters for the Aave V3 withdraw logic

```typescript
interface WithdrawParams {
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

* **WithdrawFields**: A type that represents the fields required for the Aave V3 withdraw logic.

```typescript
interface WithdrawFields {
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

* **WithdrawLogic**: An interface that extends the `Logic` interface and represents the Aave V3 withdraw logic. It includes the `rid`, and `fields` properties.

```typescript
interface WithdrawLogic {
  rid: string;
  fields: WithdrawFields;
}
```

### Functions

* **getWithdrawTokenList(chainId: number)**: An asynchronous function that retrieves the list of tokens supported by the Aave V3 withdraw logic on the specified `chainId`.
* **getWithdrawQuotation(chainId: number, params: WithdrawParams)**: An asynchronous function that retrieves a quotation for withdrawing assets from the Aave V3 protocol with the specified `params` object on the specified `chainId`.
* **newWithdrawLogic(fields: WithdrawFields)**: A function that creates the Aave V3 withdraw logic data with the given `fields` object.

### Example Code

```typescript
import * as api from '@protocolink/api';

const chainId = 1;

const tokenList = await api.protocols.aavev3.getWithdrawTokenList(chainId);
const aToken = tokenList[0][0];
const underlyingToken = tokenList[0][1];

const withdrawQuotation = await api.protocols.aavev3.getWithdrawQuotation(chainId, {
  input: {
    token: aToken,
    amount: '10',
  },
  tokenOut: underlyingToken,
});

const withdrawLogic = await api.protocols.aavev3.newWithdrawLogic(withdrawQuotation);
```

## Borrow

The following code defines interfaces and functions related to the Aave V3 borrow logic:

### Types

* **BorrowFields**: A type that represents the fields required for the Aave V3 borrow logic.

```typescript
import * as logics from '@protocolink/logics';

interface BorrowFields {
  interestRateMode: logics.aavev3.InterestRateMode;
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

* **BorrowLogic**: An interface that extends the Logic interface and represents the Aave V3 borrow logic. It includes the `rid`, and `fields` properties.

```typescript
interface BorrowLogic {
  rid: string;
  fields: BorrowFields;
}
```

### Functions

* **getBorrowTokenList(chainId: number)**: An asynchronous function that retrieves the list of tokens supported by the Aave V3 borrow logic on the specified chainId.
* **newBorrowLogic(fields: BorrowFields)**: A function that creates the Aave V3 borrow logic data with the given fields object.

### Example Code

```typescript
import * as api from '@protocolink/api';
import * as logics from '@protocolink/logics';

const chainId = 1;

const tokenList = await api.protocols.aavev3.getBorrowTokenList(chainId);
const underlyingToken = tokenList[0];

const borrowLogic = await api.protocols.aavev3.newBorrowLogic({
  interestRateMode: logics.aavev3.InterestRateMode.variable,
  output: {
    token: underlyingToken,
    amount: '10',
  },
});
```

## Repay

The following code defines interfaces and functions related to the Aave V3 repay logic:

### Types

* **RepayParams**: A type that represents the input parameters for the Aave V3 repay logic

```typescript
import * as logics from '@protocolink/logics';

interface RepayParams {
  tokenIn: {
    chainId: number;
    address: string;
    decimals: number;
    symbol: string;
    name: string;
  };
  borrower: string;
  interestRateMode: logics.aavev3.InterestRateMode;
}
```

* **RepayFields**: A type that represents the fields required for the Aave V3 repay logic.

```typescript
import * as logics from '@protocolink/logics';

interface RepayFields {
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
  interestRateMode: logics.aavev3.InterestRateMode;
}
```

* **RepayLogic**: An interface that extends the `Logic` interface and represents the Aave V3 repay logic. It includes the `rid`, and `fields` properties.

```typescript
interface RepayLogic {
  rid: string;
  fields: RepayFields;
}
```

### Functions

* **getRepayTokenList(chainId: number)**: An asynchronous function that retrieves the list of tokens supported by the Aave V3 repay logic on the specified `chainId`.
* **getRepayQuotation(chainId: number, params: RepayParams)**: A function that retrieves a quotation for repaying a loan using the specified parameters and Aave V3 protocol on the specified chain.
* **newRepayLogic(fields: RepayFields)**: A function that creates the Aave V3 repay logic data with the given `fields` object.

### Example Code

```typescript
import * as api from '@protocolink/api';
import * as logics from '@protocolink/logics';

const chainId = 1;
const account = '0xaAaAaAaaAaAaAaaAaAAAAAAAAaaaAaAaAaaAaaAa';

const tokenList = await api.protocols.aavev3.getRepayTokenList(chainId);
const underlyingToken = tokenList[0];

const repayQuotation = await api.protocols.aavev3.getRepayQuotation(chainId, {
  borrower: account,
  tokenIn: underlyingToken,
  interestRateMode: logics.aavev3.InterestRateMode.variable,
});

const repayLogic = await api.protocols.aavev3.newRepayLogic(repayQuotation);
```

## FlashLoan

The following code defines functions related to the Aave V3 flash loan logic:

### Types

Please refer to the [FlashLoan Logic](flashloan-logic.md) section for more information.

### Functions

* **getFlashLoanTokenList(chainId: number)**: An asynchronous function that retrieves the list of tokens supported by the Aave V3 flash loan logic on the specified `chainId`.
* **getFlashLoanQuotation(chainId: number, params: FlashLoanParams)**: An asynchronous function that retrieves a quotation for flash loaning assets on the Aave V3 protocol with the specified `params` object on the specified `chainId`.
* **newFlashLoanLogic(fields: FlashLoanFields)**: A function that creates the Aave V3 flash loan logic data with the given `fields` object.
* **newFlashLoanLogicPair(loans: FlashLoanFields\['loans'])**: A function that creates the Aave V3 flash loan logic data pair with the given `loans` object.

### Example Code

```typescript
import * as api from '@protocolink/api';

const chainId = 1;

const tokenList = await api.protocols.aavev3.getFlashLoanTokenList(chainId);
const underlyingToken = tokenList[0];

const loans = [
  {
    token: underlyingToken,
    amount: '10000',
  },
];

const flashLoanQuotation = await api.protocols.aavev3.getFlashLoanQuotation(chainId, {
  loans,
});

const [flashLoanLoanLogic, flashLoanRepayLogic] = api.protocols.aavev3.newFlashLoanLogicPair(loans);
const logics = [flashLoanLoanLogic];
// logics.push(swapLogic)
// logics.push(supplyLogic)
// logics.push(...)
logics.push(flashLoanRepayLogic);
```
