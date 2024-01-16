# Morphoblue

In this section, we will introduce the Morphoblue SDK interfaces, which provide developers with a convenient and efficient way to interact with the Morphoblue protocol. These interfaces cover various aspects of the protocol, including supply, supply-collateral, withdraw, withdraw-collateral, borrow, repay, and flash-loan. They are designed to be used easily and flexibly.

Before we dive into the SDK interfaces, we would like to provide an overview of the features of Morphoblue. If you are already familiar with Morphoblue, feel free to skip this section. For those who are new to Morphoblue, here's a brief explanation:

Morphoblue is a singleton contract that manages user balances without minting any tokens.

Each market in Morphoblue is independent, meaning that any action performed on the protocol must specify which market it's interacting with.

Once you have identified the market you want to interact with, you can choose to either be a Lender or a Borrower. Let's take the WETH(USDC, 90%, Chainlink, AdaptiveCurveIRM) market as an example:

* If you want to be a Lender, you need to supply WETH as the loan token.
* If you want to be a Borrower, you must deposit USDC as the collateral token and borrow WETH as the loan token.

Please note that Morphoblue is subject to change and improvements, and we will update our SDK accordingly.

The following section will introduce the interfaces related to the Morphoblue protocol, which can be accessed through the `api.protocols.morphoblue.` prefix.

## Supply

The following code defines interfaces and functions related to the Morphoblue supply logic:

### Types

* **SupplyFields**: A type that represents the fields required for the Morphoblue supply logic.

```javascript
interface SupplyFields {
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

**SupplyLogic**: An interface that extends the `Logic` interface and represents the Morphoblue supply logic. It includes the `rid`, and `fields` properties.

```javascript
interface SupplyLogic {
  rid: string;
  fields: SupplyFields;
}
```

### Functions

* **getSupplyTokenList(chainId: number)**: An asynchronous function that retrieves the list of tokens supported by the Morphoblue supply logic on the specified `chainId`.
* **newSupplyLogic(fields: SupplyFields)**: A function that creates the Morphoblue supply logic data with the given `fields` object.

### Example Code

```javascript
import * as api from '@protocolink/api';

const chainId = 5;
const marketId = '0x3098a46de09dd8d9a8c6fa1ab7b3f943b6f13e5ea72a4e475d9e48f222bfd5a0';

const tokenList = await api.protocols.morphoblue.getSupplyTokenList(chainId);
const asset = tokenList[marketId][0];

const supplyLogic = await api.protocols.morphoblue.newSupplyLogic({
    marketId,
    input: {
      token: asset,
      amount: '10',
    },
});
```

## SupplyCollateral

The following code defines interfaces and functions related to the Morphoblue supply collateral logic:

### Types

* **SupplyCollateralFields**: A type that represents the fields required for the Morphoblue supply collateral logic.

```javascript
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

* **SupplyCollateralLogic**: An interface that extends the `Logic` interface and represents the Morphoblue supply collateral logic. It includes the `rid`, and `fields` properties.

```javascript
interface SupplyCollateralLogic {
  rid: string;
  fields: SupplyCollateralFields;
}
```

### Functions

* **getSupplyCollateralTokenList(chainId: number)**: An asynchronous function that retrieves the list of tokens supported by the Morphoblue supply collateral logic on the specified `chainId`.
* **newSupplyCollateralLogic(fields: SupplyCollateralFields)**: A function that creates the Morphoblue supply collateral logic data with the given `fields` object.

### Example Code

```javascript
import * as api from '@protocolink/api';

const chainId = 5;
const marketId = '0x3098a46de09dd8d9a8c6fa1ab7b3f943b6f13e5ea72a4e475d9e48f222bfd5a0';

const tokenList = await api.protocols.morphoblue.getSupplyCollateralTokenList(chainId);
const asset = tokenList[marketId][0];

const supplyCollateralLogic = await api.protocols.morphoblue.newSupplyCollateralLogic({
  marketId,
  input: {
    token: asset,
    amount: '10',
  },
});
```

## Withdraw

The following code defines interfaces and functions related to the Morphoblue withdraw logic:

### Types

* **WithdrawFields**: A type that represents the fields required for the Morphoblue withdraw logic.

```javascript
interface WithdrawFields {
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

* **WithdrawLogic**: An interface that extends the `Logic` interface and represents the Morphoblue withdraw logic. It includes the `rid`, and `fields` properties.

```javascript
interface WithdrawLogic {
  rid: string;
  fields: WithdrawFields;
}
```

### Functions

* **getWithdrawTokenList(chainId: number)**: An asynchronous function that retrieves the list of tokens supported by the Morphoblue withdraw logic on the specified `chainId`.
* **newWithdrawLogic(fields: WithdrawFields)**: A function that creates the Morphoblue withdraw  logic data with the given `fields` object.

### Example Code

```javascript
import * as api from '@protocolink/api';

const chainId = 5;
const marketId = '0x3098a46de09dd8d9a8c6fa1ab7b3f943b6f13e5ea72a4e475d9e48f222bfd5a0';

const tokenList = await api.protocols.morphoblue.getWithdrawTokenList(chainId);
const asset = tokenList[marketId][0];

const withdrawLogic = await api.protocols.morphoblue.newWithdrawLogic({
  marketId,
  output: {
    token: asset,
    amount: '10',
  },
});
```

## WithdrawCollateral

The following code defines interfaces and functions related to the Morphoblue withdraw collateral logic:

### Types

* **WithdrawCollateralFields**: A type that represents the fields required for the Morphoblue withdraw collateral logic.

```javascript
interface WithdrawCollateralFields {
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

* **WithdrawCollateralLogic**: An interface that extends the `Logic` interface and represents the Morphoblue withdraw collateral logic. It includes the `rid`, and `fields` properties.

```javascript
interface WithdrawCollateralLogic {
  rid: string;
  fields: WithdrawCollateralFields;
}
```

### Functions

* **getWithdrawCollateralTokenList(chainId: number)**: An asynchronous function that retrieves the list of tokens supported by the Morphoblue withdraw collateral logic on the specified `chainId`.
* **newWithdrawCollateralLogic(fields: WithdrawCollateralFields)**: A function that creates the Morphoblue withdraw collateral logic data with the given `fields` object.

### Example Code

```javascript
import * as api from '@protocolink/api';

const chainId = 5;
const marketId = '0x3098a46de09dd8d9a8c6fa1ab7b3f943b6f13e5ea72a4e475d9e48f222bfd5a0';

const tokenList = await api.protocols.morphoblue.getWithdrawCollateralTokenList(chainId);
const asset = tokenList[marketId][0];

const withdrawCollateralLogic = await api.protocols.morphoblue.newWithdrawCollateralLogic({
  marketId,
  output: {
    token: asset,
    amount: '10',
  },
});
```

## Borrow

The following code defines interfaces and functions related to the Morphoblue borrow logic:

### Types

* **BorrowFields**: A type that represents the fields required for the Morphoblue borrow logic.

```javascript
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

* **BorrowLogic**: An interface that extends the `Logic` interface and represents the Morphoblue borrow logic. It includes the `rid`, and `fields` properties.

```javascript
interface BorrowLogic {
  rid: string;
  fields: BorrowFields;
}
```

### Functions

* **getBorrowTokenList(chainId: number)**: An asynchronous function that retrieves the list of tokens supported by the Morphoblue borrow logic on the specified `chainId`.
* **newBorrowLogic(fields: BorrowFields)**: A function that creates the Morphoblue borrow logic data with the given `fields` object.

### Example Code

```javascript
import * as api from '@protocolink/api';

const chainId = 5;
const marketId = '0x3098a46de09dd8d9a8c6fa1ab7b3f943b6f13e5ea72a4e475d9e48f222bfd5a0';

const tokenList = await api.protocols.morphoblue.getBorrowTokenList(chainId);
const loanToken = tokenList[marketId][0];

const borrowLogic = await api.protocols.morphoblue.newBorrowLogic({
  marketId,
  output: {
    token: loanToken,
    amount: '10',
  },
});
```

## Repay

The following code defines interfaces and functions related to the Morphoblue repay logic:

### Types

* **RepayParams**: A type that represents the input parameters for the Morphoblue repay logic

```javascript
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

* **RepayFields**: A type that represents the fields required for the Morphoblue repay logic.

```javascript
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

* **RepayLogic**: An interface that extends the `Logic` interface and represents the Morphoblue repay logic. It includes the `rid`, and `fields` properties.

```javascript
interface RepayLogic {
  rid: string;
  fields: RepayFields;
}
```

### Functions

* **getRepayTokenList(chainId: number)**: An asynchronous function that retrieves the list of tokens supported by the Morphoblue repay logic on the specified `chainId`.
* **getRepayQuotation(chainId: number, params: RepayParams)**: An asynchronous function that retrieves a quotation for repaying a loan on the Morphoblue protocol with the specified `params` object on the specified `chainId`.
* **newRepayLogic(fields: RepayFields)**: A function that creates the Morphoblue repay logic data with the given `fields` object.

### Example Code

```javascript
import * as api from '@protocolink/api';
import * as logics from '@protocolink/logics';

const chainId = 5;
const marketId = '0x3098a46de09dd8d9a8c6fa1ab7b3f943b6f13e5ea72a4e475d9e48f222bfd5a0';
const account = '0xaAaAaAaaAaAaAaaAaAAAAAAAAaaaAaAaAaaAaaAa';

const tokenList = await api.protocols.morphoblue.getRepayTokenList(chainId);
const loanToken = tokenList[marketId][0];

const repayQuotation = await api.protocols.morphoblue.getRepayQuotation(chainId, {
  marketId,
  tokenIn: loanToken,
  borrower: account,
});

const repayLogic = await api.protocols.morphoblue.newRepayLogic(repayQuotation);
```

## FlashLoan

The following code defines functions related to the Morphoblue flash loan logic:

### Types

Please refer to the [FlashLoan Logic](flashloan-logic.md) section for more information.

### Functions

* **getFlashLoanTokenList(chainId: number)**: An asynchronous function that retrieves the list of tokens supported by the Morphoblue flash loan logic on the specified `chainId`.
* **getFlashLoanQuotation(chainId: number, params: FlashLoanParams)**: An asynchronous function that retrieves a quotation for flash loaning assets on the Morphoblue protocol with the specified `params` object on the specified `chainId`.
* **newFlashLoanLogic(fields: FlashLoanFields)**: A function that creates the Morphoblue flash loan logic data with the given `fields` object.
* **newFlashLoanLogicPair(loans: FlashLoanFields\['loans'])**: A function that creates the Morphoblue flash loan logic data pair with the given `loans` object.

### Example Code

```javascript
import * as api from '@protocolink/api';

const chainId = 5;

const tokenList = await api.protocols.morphoblue.getFlashLoanTokenList(chainId);
const underlyingToken = tokenList[0];

const loans = [
  {
    token: underlyingToken,
    amount: '10000',
  },
];

const flashLoanQuotation = await api.protocols.morphoblue.getFlashLoanQuotation(chainId, {
  loans,
});

const [flashLoanLoanLogic, flashLoanRepayLogic] = api.protocols.morphoblue.newFlashLoanLogicPair(loans);
const logics = [flashLoanLoanLogic];
// logics.push(swapLogic)
// logics.push(supplyLogic)
// logics.push(...)
logics.push(flashLoanRepayLogic);
```
