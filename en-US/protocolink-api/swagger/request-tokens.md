# Request Tokens

{% swagger src="https://api.swaggerhub.com/apis/dinngodev/Protocolink/1.0.0/swagger.json" path="/v1/protocols/{chainId}/{protocolId}/{logicId}/tokens" method="get" %}
[https://api.swaggerhub.com/apis/dinngodev/Protocolink/1.0.0/swagger.json](https://api.swaggerhub.com/apis/dinngodev/Protocolink/1.0.0/swagger.json)
{% endswagger %}

Provides a list of tokens supported by a Logic, depending on the chain and protocol. See [networks-and-protocols.md](../../networks-and-protocols.md "mention").

```javascript
const client = axios.create({ baseURL: 'https://api.protocolink.com' });
const getTokens = async (chainId, protocol, logicId) => {
    const result = await client.get(`/v1/protocols/${chainId}/${protocol}/${logicId}/tokens`);
    return result.data;
}

const chainId = 1;
const protocol = "uniswap-v3";
const logicId = "swap-token";

const tokens = await getTokens(chainId, protocol, logicId);
```

The result contains a list of chains that looks the following:

```json
{
   "tokens":[
      {
         "chainId":1,
         "address":"0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48",
         "decimals":6,
         "symbol":"USDC",
         "name":"USD Coin"
      },
      ... // other tokens will be present in the list
   ]
}
```

Presented according to different protocols or the token has been paired:

```json
：{
   "tokens":[
      [
         {
            "chainId":1,
            "address":"0x0000000000000000000000000000000000000000",
            "decimals":18,
            "symbol":"ETH",
            "name":"Ethereum"
         },
         {
            "chainId":1,
            "address":"0x4d5F47FA6A74757f35C14fD3a6Ef8E3C9BC514E8",
            "decimals":18,
            "symbol":"aEthWETH",
            "name":"Aave Ethereum WETH"
         }
      ],
      ... // other token pair will be present in the list
   ]
}j
```
