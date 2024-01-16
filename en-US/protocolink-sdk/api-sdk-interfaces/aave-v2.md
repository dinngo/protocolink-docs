# Aave V2

In this section, we will introduce the Aave V2 SDK interfaces, which provide developers with a convenient and efficient way to interact with the Aave V2 protocol. These interfaces cover various aspects of the protocol, including deposit, withdraw, borrow, repay, and flash loan. They are designed to be used easily and flexibly.

The following section will introduce the interfaces related to the Aave V2 protocol, which can be accessed through the `api.protocols.aavev2.` prefix.

## Deposit

The following code defines interfaces and functions related to the Aave V2 deposit logic:

### Types

* **DepositParams**: A type that represents the input parameters for the Aave V2 deposit logic

<pre class="language-typescript"><code class="lang-typescript"><strong>interface DepositParams {
</strong>  input: {
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
</code></pre>

* **DepositFields**: A type that represents the fields required for the Aave V2 deposit logic.

```typescript
interface DepositFields {
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

* **DepositLogic**: An interface that extends the `Logic` interface and represents the Aave V2 deposit logic. It includes the `rid`, and `fields` properties.

```typescript
interface DepositLogic {
  rid: string;
  fields: DepositFields;
}
```

### Functions

* **getDepositTokenList(chainId: number)**: An asynchronous function that retrieves the list of tokens supported by the Aave V2 deposit logic on the specified `chainId`.
* **getDepositQuotation(chainId: number, params: DepositParams)**: An asynchronous function that retrieves a quotation for depositing assets on the Aave V2 protocol with the specified `params` object on the specified `chainId`.
* **newDepositLogic(fields: DepositFields)**: A function that creates the Aave V2 deposit logic data with the given `fields` object.

### Example Code

```typescript
import * as api from '@protocolink/api';

const chainId = 1;

const tokenList = await api.protocols.aavev2.getDepositTokenList(chainId);
const underlyingToken = tokenList[0][0];
const aToken = tokenList[0][1];

const depositQuotation = await api.protocols.aavev2.getDepositQuotation(chainId, {
  input: {
    token: underlyingToken,
    amount: '10',
  },
  tokenOut: aToken,
});

const depositLogic = await api.protocols.aavev2.newDepositLogic(depositQuotation);
```

## Withdraw

The following code defines interfaces and functions related to the Aave V2 withdraw logic:

### Types

* **WithdrawParams**: A type that represents the input parameters for the Aave V2 withdraw logic

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

* **WithdrawFields**: A type that represents the fields required for the Aave V2 withdraw logic.

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

* **WithdrawLogic**: An interface that extends the `Logic` interface and represents the Aave V2 withdraw logic. It includes the `rid`, and `fields` properties.

```typescript
interface WithdrawLogic {
  rid: string;
  fields: WithdrawFields;
}
```

### Functions

* **getWithdrawTokenList(chainId: number)**: An asynchronous function that retrieves the list of tokens supported by the Aave V2 withdraw logic on the specified `chainId`.
* **getWithdrawQuotation(chainId: number, params: WithdrawParams)**: An asynchronous function that retrieves a quotation for withdrawing assets from the Aave V2 protocol with the specified `params` object on the specified `chainId`.
* **newWithdrawLogic(fields: WithdrawFields)**: A function that creates the Aave V2 withdraw logic data with the given `fields` object.

### Example Code

```typescript
import * as api from '@protocolink/api';

const chainId = 1;

const tokenList = await api.protocols.aavev2.getWithdrawTokenList(chainId);
const aToken = tokenList[0][0];
const underlyingToken = tokenList[0][1];

const withdrawQuotation = await api.protocols.aavev2.getWithdrawQuotation(chainId, {
  input: {
    token: aToken,
    amount: '10',
  },
  tokenOut: underlyingToken,
});

const withdrawLogic = await api.protocols.aavev2.newWithdrawLogic(withdrawQuotation);
```

## Borrow

The following code defines interfaces and functions related to the Aave V2 borrow logic:

### Types

* **BorrowFields**: A type that represents the fields required for the Aave V2 borrow logic.

```typescript
import * as logics from '@protocolink/logics';

interface BorrowFields {
  interestRateMode: logics.aavev2.InterestRateMode;
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

* **BorrowLogic**: An interface that extends the Logic interface and represents the Aave V2 borrow logic. It includes the `rid`, and `fields` properties.

```typescript
interface BorrowLogic {
  rid: string;
  fields: BorrowFields;
}
```

### Functions

* **getBorrowTokenList(chainId: number)**: An asynchronous function that retrieves the list of tokens supported by the Aave V2 borrow logic on the specified chainId.
* **newBorrowLogic(fields: BorrowFields)**: A function that creates the Aave V2 borrow logic data with the given fields object.

### Example Code

```typescript
import * as api from '@protocolink/api';
import * as logics from '@protocolink/logics';

const chainId = 1;

const tokenList = await api.protocols.aavev2.getBorrowTokenList(chainId);
const underlyingToken = tokenList[0];

const borrowLogic = await api.protocols.aavev2.newBorrowLogic({
  interestRateMode: logics.aavev2.InterestRateMode.variable,
  output: {
    token: underlyingToken,
    amount: '10',
  },
});
```

## Repay

The following code defines interfaces and functions related to the Aave V2 repay logic:

### Types

* **RepayParams**: A type that represents the input parameters for the Aave V2 repay logic

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
  interestRateMode: logics.aavev2.InterestRateMode;
}
```

* **RepayFields**: A type that represents the fields required for the Aave V2 repay logic.

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
  interestRateMode: logics.aavev2.InterestRateMode;
}
```

* **RepayLogic**: An interface that extends the `Logic` interface and represents the Aave V2 repay logic. It includes the `rid`, and `fields` properties.

```typescript
interface RepayLogic {
  rid: string;
  fields: RepayFields;
}
```

### Functions

* **getRepayTokenList(chainId: number)**: An asynchronous function that retrieves the list of tokens supported by the Aave V2 repay logic on the specified `chainId`.
* **getRepayQuotation(chainId: number, params: RepayParams)**: A function that retrieves a quotation for repaying a loan using the specified parameters and Aave V2 protocol on the specified chain.
* **newRepayLogic(fields: RepayFields)**: A function that creates the Aave V2 repay logic data with the given `fields` object.

### Example Code

```typescript
import * as api from '@protocolink/api';
import * as logics from '@protocolink/logics';

const chainId = 1;
const account = '0xaAaAaAaaAaAaAaaAaAAAAAAAAaaaAaAaAaaAaaAa';

const tokenList = await api.protocols.aavev2.getRepayTokenList(chainId);
const underlyingToken = tokenList[0];

const repayQuotation = await api.protocols.aavev2.getRepayQuotation(chainId, {
  borrower: account,
  tokenIn: underlyingToken,
  interestRateMode: logics.aavev2.InterestRateMode.variable,
});

const repayLogic = await api.protocols.aavev2.newRepayLogic(repayQuotation);
```

## FlashLoan

The following code defines functions related to the Aave V2 flash loan logic:

### Types

Please refer to the [FlashLoan Logic](flashloan-logic.md) section for more information.

### Functions

* **getFlashLoanTokenList(chainId: number)**: An asynchronous function that retrieves the list of tokens supported by the Aave V2 flash loan logic on the specified `chainId`.
* **getFlashLoanQuotation(chainId: number, params: FlashLoanParams)**: An asynchronous function that retrieves a quotation for flash loaning assets on the Aave V2 protocol with the specified `params` object on the specified `chainId`.
* **newFlashLoanLogic(fields: FlashLoanFields)**: A function that creates the Aave V2 flash loan logic data with the given `fields` object.
* **newFlashLoanLogicPair(loans: FlashLoanFields\['loans'])**: A function that creates the Aave V2 flash loan logic data pair with the given `loans` object.

### Example Code

```typescript
import * as api from '@protocolink/api';

const chainId = 1;

const tokenList = await api.protocols.aavev2.getFlashLoanTokenList(chainId);
const underlyingToken = tokenList[0];

const loans = [
  {
    token: underlyingToken,
    amount: '10000',
  },
];

const flashLoanQuotation = await api.protocols.aavev2.getFlashLoanQuotation(chainId, {
  loans,
});

const [flashLoanLoanLogic, flashLoanRepayLogic] = api.protocols.aavev2.newFlashLoanLogicPair(loans);
const logics = [flashLoanLoanLogic];
// logics.push(swapLogic)
// logics.push(supplyLogic)
// logics.push(...)
logics.push(flashLoanRepayLogic);
```
