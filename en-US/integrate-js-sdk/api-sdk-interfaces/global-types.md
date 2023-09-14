# Global Types

## Logic

```typescript
interface Logic<TFields = any> {
  rid: string;
  fields: TFields;
}
```

The `Logic<TFields = any>` interface includes the following properties:

* **rid**: An identity composed of the protocol and logicId.
* **fields**: Information about the fields required for this Logic.

Note that `TFields` is a generic type parameter that can be replaced with any type.

## RouterData

```typescript
interface RouterData {
  chainId: number;
  account: string;
  logics: Logic[];
  permitData?: PermitSingleData | PermitBatchData;
  permitSig?: string;
  referralCode?: number;
}
```

The `RouterData` interface represents the data required to execute a router transaction. It includes the following properties:

* **chainId**: The chain ID of the blockchain network.
* **account**: The address of the user's wallet account.
* **logics**: The Logics data.
* **permitData(optional)**: The permit data required for the transaction.
* **permitSig(optional)**: The signature of the permit data.
* **referralCode(optional)**: The code provided when applying to be a partner.
