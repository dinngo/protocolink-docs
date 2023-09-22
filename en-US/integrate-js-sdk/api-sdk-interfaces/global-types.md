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
  referral?: string;
  referrals?: { collector: string; rate: number }[];
}
```

The `RouterData` interface represents the data required to execute a router transaction. It includes the following properties:

* **chainId**: The chain ID of the blockchain network.
* **account**: The address of the user's wallet account.
* **logics**: The Logics data.
* **permitData(optional)**: The permit data required for the transaction.
* **permitSig(optional)**: The signature of the permit data.
* **referral(optional)**: This property represents the address to which fees are sent when conducting on-chain fee-sharing.
*   **referrals(optional)**: This property is used when you want to distribute fees to multiple addresses. It is an array of objects, where each object has two attributes:

    * `collector`: This is the address where a portion of the proceeds will be sent.
    * `rate`: The `rate` parameter represents the percentage allocation of fees to a particular `collector`. It is an integer value ranging from 1 to 10,000, where 1 represents 0.01%, and 10,000 represents 100%. The sum of all `rate` values within the `referrals` array should equal 10,000 to ensure that the entire fee amount is correctly distributed among the specified collectors.

    \
