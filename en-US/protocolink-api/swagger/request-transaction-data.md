# Request Transaction Data



{% swagger src="https://api.swaggerhub.com/apis/dinngodev/Protocolink/1.0.0/swagger.json" path="/v1/transactions/build" method="post" %}
[https://api.swaggerhub.com/apis/dinngodev/Protocolink/1.0.0/swagger.json](https://api.swaggerhub.com/apis/dinngodev/Protocolink/1.0.0/swagger.json)
{% endswagger %}

Provides the transaction request that needs to be sent, comprising the Router contract `address` (to), transaction `data` (data), and the amount of ETH to be carried in the transaction (value).

```javascript
const getRouterTransactionRequest = async (routerData) => {
  const result = await client.post("/v1/transactions/build", { body: routerData });
  return result.data;
};

const routerData = {
  chainId: 1,
  account: USER_ADDRESS,
  logics: [swapLogic, supplyLogic],
  // If the estimate result returns permitData, the user should sign the permitData,
  // and return the signed permitSig along with it.
  permitData: permitData,
  permitSig: permitSig,
  // If there is only one referral address, you can use the 'referral' property.
  // If there are multiple referral addresses, use 'referrals' and specify the rates accordingly.
  referral: collector,
  referrals: [
    { collector: collector, rate: 5000 },
    ...
  ]
};

const transactionRequest = await getRouterTransactionRequest(routerData);
```

The result contains a list of chains that looks the following:

```json
{
  "to": "0x67e4d4Af097787Aa5a7daE7f9b147Bf32243F030",
  "data": "0x1c81999100000000000000000000000000000000000000000000000000000000000000600000000000000000000000000000000000000000000000000000000000000a00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000040000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000032000000000000000000000000000000000000000000000000000000000000004c00000000000000000000000000000000000000000000000000000000000000780000000000000000000000000000000000022d473030f116ddee9f6b43ac78ba300000000000000000000000000000000000000000000000000000000000000c0000000000000000000000000000000000000000000000000000000000000028000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000001842b67b570000000000000000000000000aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa000000000000000000000000a0b86991c6218b36c1d19d4a2e9eb0ce3606eb48000000000000000000000000ffffffffffffffffffffffffffffffffffffffff00000000000000000000000000000000000000000000000000000000645faecd00000000000000000000000000000000000000000000000000000000000000000000000000000000000000009e2a2394c69874545c013492d52d1dc0c607b20800000000000000000000000000000000000000000000000000000000643828d500000000000000000000000000000000000000000000000000000000000001000000000000000000000000000000000000000000000000000000000000000041bb8d0cf3e494c2ed4dc1057ee31c90cab5387b8a606019cc32a6d12f714303df183b1b0cd7a1114bd952a4c533ac18606056dda61f922e030967df0836cf76f91c00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000022d473030f116ddee9f6b43ac78ba300000000000000000000000000000000000000000000000000000000000000c00000000000000000000000000000000000000000000000000000000000000180000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000008436c78516000000000000000000000000aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa0000000000000000000000009e2a2394c69874545c013492d52d1dc0c607b208000000000000000000000000000000000000000000000000000000003b9aca00000000000000000000000000a0b86991c6218b36c1d19d4a2e9eb0ce3606eb48000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000e592427a0aece92de3edee1f18e0157c0586156400000000000000000000000000000000000000000000000000000000000000c000000000000000000000000000000000000000000000000000000000000002400000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000144c04b8d59000000000000000000000000000000000000000000000000000000000000002000000000000000000000000000000000000000000000000000000000000000a00000000000000000000000009e2a2394c69874545c013492d52d1dc0c607b20800000000000000000000000000000000000000000000000000000000643828d6000000000000000000000000000000000000000000000000000000003b9aca0000000000000000000000000000000000000000000000000000000000003612330000000000000000000000000000000000000000000000000000000000000042a0b86991c6218b36c1d19d4a2e9eb0ce3606eb480001f4c02aaa39b223fe8d0a0e5c4f27ead9083c756cc20001f42260fac5e5542a773aa44fbcfedf7c193bc2c599000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000001000000000000000000000000a0b86991c6218b36c1d19d4a2e9eb0ce3606eb48ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff000000000000000000000000000000000000000000000000000000003b9aca0000000000000000000000000087870bca3f3fd6335c3f4ce8392d69350b4fa4e200000000000000000000000000000000000000000000000000000000000000c000000000000000000000000000000000000000000000000000000000000001800000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000084617ba0370000000000000000000000002260fac5e5542a773aa44fbcfedf7c193bc2c5990000000000000000000000000000000000000000000000000000000000369e050000000000000000000000009e2a2394c69874545c013492d52d1dc0c607b20800000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000010000000000000000000000002260fac5e5542a773aa44fbcfedf7c193bc2c599ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff0000000000000000000000000000000000000000000000000000000000369e0500000000000000000000000000000000000000000000000000000000000000030000000000000000000000002260fac5e5542a773aa44fbcfedf7c193bc2c5990000000000000000000000005ee5bf7ae06d1be5997a1a72006fe6c607ec6de8000000000000000000000000a0b86991c6218b36c1d19d4a2e9eb0ce3606eb48",
  "value": "0"
}
```

For more information on the **routerData** object and its properties, please refer to the [**Router Data Documentation**](../../protocolink-sdk/api-sdk-interfaces/global-types.md#routerdata).