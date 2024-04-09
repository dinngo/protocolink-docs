# 3️⃣ Estimate Router Data

To properly estimate the logics of the router results, closely follow the steps below.

### Step 1: Build Router Data

First, you need to prepare the Router Data, which will include the `chainId`, `account`, and `logics` data.

```typescript
import * as api from '@protocolink/api';

const routerData: api.RouterData = {
  chainId,
  account: '0xaAaAaAaaAaAaAaaAaAAAAAAAAaaaAaAaAaaAaaAa',
  logics: [swapLogic, supplyLogic],
};
```

### Step 2: Estimate Router Data Result

Next, use `api.estimateRouterData` to estimate how much funds will be spent (funds) and how many balances will be obtained (balances) from this transaction. It will also identify any approvals that the user needs to execute (approvals) before the transaction.

In the current implementation of Permit2, users have two options for authorizing [`Agent`](../smart-contract/overview/agent.md) to spend their assets: `permit` and `approve`. These methods provide distinct approaches to token authorization, allowing users to choose the one that best suits their needs. If not specified, `permit` is the default behavior.

```typescript
type Permit2Type = 'permit' | 'approve';
```

The `permit` method involves obtaining authorization data (`permitData`) for the ERC20 tokens being spent in the router data. Users are required to sign this data. The `permitData` and the associated signature are then sent to the Protocolink API as part of the transaction.

```typescript
import * as api from '@protocolink/api';

const estimateResult = await api.estimateRouterData(routerData);
// or
const estimateResult = await api.estimateRouterData(routerData, { permit2Type: 'permit'});
```

The `approve` method provides users with the necessary transactions to approve the expenditure of ERC20 tokens in the router data. Users must complete these approval transactions before sending the router transaction.

```typescript
import * as api from '@protocolink/api';

const estimateResult = await api.estimateRouterData(routerData, { permit2Type: 'approve'});
```

The structure of the `estimateResult` obtained is roughly as follows:

```json
{
  "funds": [         // How much initial funds are needed
    {
      "token": ...,  // USDC
      "amount": "1000"
    }
  ],
  "balances": [      // How many tokens you will receive
    {
      "token": ...,  // aEthWBTC
      "amount": "0.03579397"
    }
  ],
  "fees": [          // How many fees you will be charged
    {
      "rid": "permit2:pull-token",
      "feeAmount": {
        "token": {
          "chainId": 1,
          "address": "0x0000000000000000000000000000000000000000",
          "decimals": 18,
          "symbol": "ETH",
          "name": "Ethereum"
        },
        "amount": "0.001240475451939898"
      }
    }
  ],
  "approvals": [     // User approval before transaction if need
    {
      "to": "0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48",
      "data": "0x095ea7b3000000000000000000000000000000000022d473030f116ddee9f6b43ac78ba3ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff"
    }
  ],
  "permitData": {    // Token permit before transaction if need
    "domain": ...,
    "types": ...,
    "values": ...
  }
}

```

**Funds:** For this transaction, `1000 USDC` will be spent, which can be displayed on the interface.

**Balances:** For this transaction, `0.03579397 aEthWBTC` will be obtained, which can be displayed on the interface.

**Fees:** For this transaction, `0.001240475451939898 ETH` will be charged, which can be displayed on the interface.

### Step 3: Approvals (optional)

For this transaction, the user may need to perform one or more of the following approvals before proceeding:

* Approve the Permit2 contract to spend their assets.
* Approve the `Agent` to spend their assets.
* Authorize the `Agent` to execute protocol specific operations that require authorization.

If using `ethers` package, you can refer to the following code snippet to help the user send approval transactions:

```typescript
const signer = provider.getSigner(account);
for (const approval of estimateResult.approvals) {
  const tx = await signer.sendTransaction(approval);
}
```

### Step 4: PermitData (optional)

For this transaction, the user needs to sign to permit Protocolink contract to spend his USDC fist. If using `ethers` package, you can refer to the following code snippet to help the user sign the \`permitData\`:

```typescript
const signer = provider.getSigner(account);
const permitSig = await signer._signTypedData(permitData.domain, permitData.types, permitData.values);
```

After signing, it is necessary to include `permitData` and `permitSig` into `routerData`. Without this step, the transaction will fail.

```typescript
routerData.permitData = estimateResult.permitData;
routerData.permitSig = permitSig;
```
