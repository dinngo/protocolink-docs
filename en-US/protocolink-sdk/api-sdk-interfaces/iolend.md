# Iolend

In this section, we will introduce the Iolend SDK interfaces, which provide developers with a convenient and efficient way to interact with the Iolend protocol. These interfaces cover various aspects of the protocol, including deposit, withdraw, borrow, and repay. They are designed to be used easily and flexibly.

The following section will introduce the interfaces related to the Iolend protocol, which can be accessed through the `api.protocols.iolend.` prefix.

## Deposit <a href="#deposit" id="deposit"></a>

The following code defines interfaces and functions related to the Iolend deposit logic:

## Types <a href="#types" id="types"></a>

* **DepositParams**: A type that represents the input parameters for the Iolend deposit logic

```typescript
interface DepositParams {
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

* **DepositFields**: A type that represents the fields required for the Iolend deposit logic.

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

* **DepositLogic**: An interface that extends the `Logic` interface and represents the Iolend deposit logic. It includes the `rid`, and `fields` properties.

```typescript
interface DepositLogic {
  rid: string;
  fields: DepositFields;
}
```

## Functions

* **getDepositTokenList(chainId: number)**: An asynchronous function that retrieves the list of tokens supported by the Iolend deposit logic on the specified `chainId`.
* **getDepositQuotation(chainId: number, params: DepositParams)**: An asynchronous function that retrieves a quotation for depositing assets on the Iolend protocol with the specified `params` object on the specified `chainId`.
* **newDepositLogic(fields: DepositFields)**: A function that creates the Iolend deposit logic data with the given `fields` object.

## Example Code <a href="#example-code" id="example-code"></a>

```typescript
import * as api from '@protocolink/api';

const chainId = 8822;

const tokenList = await api.protocols.iolend.getDepositTokenList(chainId);
const underlyingToken = tokenList[0][0];
const aToken = tokenList[0][1];

const depositQuotation = await api.protocols.iolend.getDepositQuotation(chainId, {
  input: {
    token: underlyingToken,
    amount: '10',
  },
  tokenOut: aToken,
});

const depositLogic = await api.protocols.iolend.newDepositLogic(depositQuotation);
```

## Withdraw <a href="#withdraw" id="withdraw"></a>

The following code defines interfaces and functions related to the Iolend withdraw logic:

## Types <a href="#types-1" id="types-1"></a>

* **WithdrawParams**: A type that represents the input parameters for the Iolend withdraw logic

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

* **WithdrawFields**: A type that represents the fields required for the Iolend withdraw logic.

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

* **WithdrawLogic**: An interface that extends the `Logic` interface and represents the Iolend withdraw logic. It includes the `rid`, and `fields` properties.

```typescript
interface WithdrawLogic {
  rid: string;
  fields: WithdrawFields;
}
```

## Functions <a href="#functions-1" id="functions-1"></a>

* **getWithdrawTokenList(chainId: number)**: An asynchronous function that retrieves the list of tokens supported by the Iolend withdraw logic on the specified `chainId`.
* **getWithdrawQuotation(chainId: number, params: WithdrawParams)**: An asynchronous function that retrieves a quotation for withdrawing assets from the Iolend protocol with the specified `params` object on the specified `chainId`.
* **newWithdrawLogic(fields: WithdrawFields)**: A function that creates Iolend withdraw logic data with the given `fields` object.

## Example Code <a href="#example-code-1" id="example-code-1"></a>

```typescript
import * as api from '@protocolink/api';

const chainId = 8822;

const tokenList = await api.protocols.iolend.getWithdrawTokenList(chainId);
const aToken = tokenList[0][0];
const underlyingToken = tokenList[0][1];

const withdrawQuotation = await api.protocols.iolend.getWithdrawQuotation(chainId, {
  input: {
    token: aToken,
    amount: '10',
  },
  tokenOut: underlyingToken,
});

const withdrawLogic = await api.protocols.iolend.newWithdrawLogic(withdrawQuotation);
```

## Borrow

The following code defines interfaces and functions related to the Iolend borrow logic:

## Types <a href="#types-2" id="types-2"></a>

* **BorrowFields**: A type that represents the fields required for the Iolend borrow logic.

```typescript
import * as logics from '@protocolink/logics';

interface BorrowFields {
  interestRateMode: logics.iolend.InterestRateMode;
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

* **BorrowLogic**: An interface that extends the Logic interface and represents the Iolend borrow logic. It includes the `rid`, and `fields` properties.

```typescript
interface BorrowLogic {
  rid: string;
  fields: BorrowFields;
}
```

## Functions <a href="#functions-2" id="functions-2"></a>

* **getBorrowTokenList(chainId: number)**: An asynchronous function that retrieves the list of tokens supported by the Iolend borrow logic on the specified chainId.
* **newBorrowLogic(fields: BorrowFields)**: A function that creates the Iolend borrow logic data with the given fields object.

## Example Code <a href="#example-code-2" id="example-code-2"></a>

```typescript
import * as api from '@protocolink/api';
import * as logics from '@protocolink/logics';

const chainId = 8822;

const tokenList = await api.protocols.iolend.getBorrowTokenList(chainId);
const underlyingToken = tokenList[0];

const borrowLogic = await api.protocols.iolend.newBorrowLogic({
  interestRateMode: logics.iolend.InterestRateMode.variable,
  output: {
    token: underlyingToken,
    amount: '10',
  },
});
```

## Repay <a href="#repay" id="repay"></a>

The following code defines interfaces and functions related to the Iolend repay logic:

## Types <a href="#types-3" id="types-3"></a>

* **RepayParams**: A type that represents the input parameters for the Iolend repay logic

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
  interestRateMode: logics.iolend.InterestRateMode;
}
```

* **RepayFields**: A type that represents the fields required for the Iolend repay logic.

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
  interestRateMode: logics.iolend.InterestRateMode;
}
```

* **RepayLogic**: An interface that extends the `Logic` interface and represents the Iolend repay logic. It includes the `rid`, and `fields` properties.

```typescript
interface RepayLogic {
  rid: string;
  fields: RepayFields;
}
```

## Functions <a href="#functions-3" id="functions-3"></a>

* **getRepayTokenList(chainId: number)**: An asynchronous function that retrieves the list of tokens supported by the Iolend repay logic on the specified `chainId`.
* **getRepayQuotation(chainId: number, params: RepayParams)**: A function that retrieves a quotation for repaying a loan using the specified parameters and Iolend protocol on the specified chain.
* **newRepayLogic(fields: RepayFields)**: A function that creates the Iolend repay logic data with the given `fields` object.

## Example Code <a href="#example-code-3" id="example-code-3"></a>

```typescript
import * as api from '@protocolink/api';
import * as logics from '@protocolink/logics';

const chainId = 8822;
const account = '0xaAaAaAaaAaAaAaaAaAAAAAAAAaaaAaAaAaaAaaAa';

const tokenList = await api.protocols.iolend.getRepayTokenList(chainId);
const underlyingToken = tokenList[0];

const repayQuotation = await api.protocols.iolend.getRepayQuotation(chainId, {
  borrower: account,
  tokenIn: underlyingToken,
  interestRateMode: logics.iolend.InterestRateMode.variable,
});

const repayLogic = await api.protocols.iolend.newRepayLogic(repayQuotation);
```
