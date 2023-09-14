# FlashLoan Logic

In this section, we will introduce four main types that are essential for implementing Flash Loan via our SDK, which can be accessed using the `api.` prefix:

* **FlashLoanLogicParams**: This type represents the params used to configure the parameters required for making Flash Loan quote requests. By using the `loans` or `repays` parameter, you can specify the desired loan or repayment amounts.

```typescript
import * as common from '@protocolink/common';

type FlashLoanLoanParams<T = object> = {
  loans: common.TokenAmounts;
};

type FlashLoanRepayParams<T = object> = {
  repays: common.TokenAmounts;
};

type FlashLoanLogicParams = FlashLoanLoanParams | FlashLoanRepayParams;
```

* **FlashLoanLogicFields**: This type represents the fields required for FlashLoanLogic, including the unique `id`, `loans` and a boolean `isLoan` field.

```typescript
import * as common from '@protocolink/common';

interface FlashLoanLogicFields {
  id: string;
  loans: common.TokenAmounts;
  isLoan: boolean;
}
```

* **FlashLoanFields**: A type that represents the fields required for the flash loan logic.

```typescript
interface FlashLoanFields {
  id: string;
  loans: {
    token: {
      chainId: number;
      address: string;
      decimals: number;
      symbol: string;
      name: string;
    };
    amount: string;
  }[];
  isLoan: boolean;
}
```

* **FlashLoanLogic**: An interface that extends the `Logic` interface and represents the flash loan logic. It includes the `rid`, and `fields` properties.

```typescript
interface FlashLoanLogic {
  rid: string;
  fields: FlashLoanFields;
}
```

For any FlashLoan-like logic, you will need to create two FlashLoan logics data **with a unique ID (such as a UUID):**

* one with **`isLoan = true`**, representing the loan taken out from the protocol
* one with **`isLoan = false`**, representing the loan repayment

Any additional logics that need to be executed during the FlashLoan process should be wrapped within these two FlashLoan logics.

Remember, the FlashLoan logic with **`isLoan = true`** should be the first one, and the FlashLoan logic with **`isLoan = false`** should be the last one.
