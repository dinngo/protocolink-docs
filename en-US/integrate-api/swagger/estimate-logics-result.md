# Estimate Logics Result

{% swagger src="https://api.swaggerhub.com/apis/dinngodev/Protocolink/0.3.0/swagger.yaml" path="/v1/transactions/estimate" method="post" %}
[https://api.swaggerhub.com/apis/dinngodev/Protocolink/0.3.0/swagger.yaml](https://api.swaggerhub.com/apis/dinngodev/Protocolink/0.3.0/swagger.yaml)
{% endswagger %}

Estimates of how much funds will be spent (**funds**) and how many balances will be obtained (**balances**) from this transaction. It will also identify any approvals that the user needs to execute (**approvals**) before the transaction and whether there is any Permit2 data that the user needs to sign before proceeding (**permitData**).

```javascript
const getEstimateResult = async (chainId, account, logics) => {
    const result = await client.post('/v1/transactions?isEstimate=true', {
        body: {
            chainId: chainId,
            account: account,
            logics: logics,
        }
    });
    return result.data;
}

const chainId = 1;
const account = USER_ADDRESS;
const logics = [swapLogic, supplyLogic];

const estimateResult = await getEstimateResult(chainId, account, logics);
```

The result contains a list of chains that looks the following:

```json
{
  "funds": [
    {
      "token": {
        "chainId": 1,
        "address": "0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48",
        "decimals": 6,
        "symbol": "USDC",
        "name": "USD Coin"
      },
      "amount": "1000"
    },
    ... // all other tokens will be spent
  ],
  "balances": [
    {
      "token": {
        "chainId": 1,
        "address": "0x5Ee5bf7ae06D1Be5997A1A72006FE6C607eC6DE8",
        "decimals": 8,
        "symbol": "aEthWBTC",
        "name": "Aave Ethereum WBTC"
      },
      "amount": "0.03579397"
    },
    ... // all other tokens will be obtained
  ],
  "fees": [
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
    },
    ... // all other fees will be charged
  ],
  "approvals": [
    {
      "to": "0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48",
      "data": "0x095ea7b3000000000000000000000000000000000022d473030f116ddee9f6b43ac78ba3ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff"
    },
    ... // all other needs to execute (approvals) before the transaction
  ],
  "permitData": {
    "domain": {
      "name": "Permit2",
      "chainId": 1,
      "verifyingContract": "0x000000000022D473030F116dDEE9F6B43aC78BA3"
    },
    "types": {
      "PermitSingle": [
        {
          "name": "details",
          "type": "PermitDetails"
        },
        {
          "name": "spender",
          "type": "address"
        },
        {
          "name": "sigDeadline",
          "type": "uint256"
        }
      ],
      "PermitDetails": [
        {
          "name": "token",
          "type": "address"
        },
        {
          "name": "amount",
          "type": "uint160"
        },
        {
          "name": "expiration",
          "type": "uint48"
        },
        {
          "name": "nonce",
          "type": "uint48"
        }
      ]
    },
    "values": {
      "details": {
        "token": "0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48",
        "amount": "0xffffffffffffffffffffffffffffffffffffffff",
        "expiration": 1683990224,
        "nonce": 0
      },
      "spender": "0x9E2A2394C69874545C013492d52d1DC0c607b208",
      "sigDeadline": 1681400024
    }
  }
}
```

### Funds (Initial Funds)

This denotes the amount of initial funds required in the logic combination. For instance, if the subsequent logic generates tokens from the previous logic, then the user need not transfer them again, and thus they are not counted in the total.

### Balances (You will receive)

This is a projection of the amount of money that will be refunded to your wallet upon completion of the transaction, which can be conveniently displayed to the user.

### Fees

Protocolink will calculate fees based on the logics included in the transaction, which can be conveniently displayed to the user. You can refer to our [Fees](../../fees.md) section for a list of logics that may incur fees.

### Approvals

After the current estimation, there are transactions that necessitate user authorization, such as when a user uses Permit2 or borrows delegation for the first time.

### PermitData

The first time the token is approved for Protocolink by Permit2, the user's signature is required. The default validity period of the signature is `30 minutes`, and the expiration time of the Token permit is `30 days`.

