# Request Protocols

{% swagger src="https://api.swaggerhub.com/apis/dinngodev/Protocolink/1.0.0/swagger.json" path="/v1/protocols" method="get" %}
[https://api.swaggerhub.com/apis/dinngodev/Protocolink/1.0.0/swagger.json](https://api.swaggerhub.com/apis/dinngodev/Protocolink/1.0.0/swagger.json)
{% endswagger %}

Provides a list of Protocols. See [networks-and-protocols.md](../../networks-and-protocols.md "mention").

```javascript
const client = axios.create({ baseURL: 'https://api.protocolink.com' });
const getProtocols = async () => {
    const result = await client.get(`/v1/protocols`);
    return result.data;
}
const tokens = await getProtocols();
```

The result contains a list of protocols and logics that looks the following:

```json
{
  "protocols": [
    {
      "id": "uniswap-v3",
      "logics": [
        {
          "id": "swap-token",
          "supportedChainIds": [
            1
          ]
        }
      ]
    }
  ]
}
```
