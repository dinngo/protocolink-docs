# Request Quote

{% swagger src="https://api.swaggerhub.com/apis/dinngodev/Protocolink/0.3.0/swagger.yaml" path="/v1/protocols/{chainId}/{protocolId}/{logicId}/quote" method="post" %}
[https://api.swaggerhub.com/apis/dinngodev/Protocolink/0.3.0/swagger.yaml](https://api.swaggerhub.com/apis/dinngodev/Protocolink/0.3.0/swagger.yaml)
{% endswagger %}

Provides quote of a Logic, depending on the chain and protocol. See [networks-and-protocols.md](../../networks-and-protocols.md "mention").

<pre class="language-javascript"><code class="lang-javascript"><strong>const getQuote = async (chainId, protocol, logicId, logicParams) => {
</strong>    const result = await client.post(`/v1/protocols/${chainId}/${protocol}/${logicId}/quote`, {
        body: logicParams,
    });
    return result.data;
}

const chainId = 1;
const protocol = "uniswap-v3";
const logicId = "swap-token";
const logicParams = {
    input: {
        token: USDC,
        amount: '1000'
    },
    tokenOut: WBTC,
    slippage: 100, // 1%
};

const swapQuote = await getQuote(chainId, protocol, logicId, logicParams);
</code></pre>

Quote is used in fields of Logic data. It can be used after building Logic according to the following format:

```javascript
const swapLogic = {
    rid: `${protocol}:${logicId}`,
    fields: swapQuote,
}
```

### Logic Params (default)

```javascript
// aave-v2: deposit, withdraw
// aave-v3: supply, withdraw
// uniswap-v3: swap-token (exact in)
// utility: wrapped-native-token
{
    input: {
        token: inputToken,
        amount: inputAmount
    },
    tokenOut: tokenOut,
    slippage: 100, // 1%
}

// uniswap-v3: swap-token (exact out)
{
    tokenIn: tokenIn,
    output: {
        token: inputToken,
        amount: inputAmount
    },
    slippage: 100, // 1%
}
```

The result contains a list of chains that looks the following:

```json
{
    "tradeType": "exactOut",
    "input": {
        "token": {
            "chainId": 1,
            "address": "0x2260FAC5E5542a773Aa44fBCfeDf7C193bc2C599",
            "decimals": 8,
            "symbol": "WBTC",
            "name": "Wrapped BTC"
        },
        "amount": "0.03618858"
    },
    "output": {
        "token": {
            "chainId": 1,
            "address": "0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48",
            "decimals": 6,
            "symbol": "USDC",
            "name": "USD Coin"
        },
        "amount": "1000"
    },
    "path": "0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb480001f46b175474e89094c44da98b954eedeac495271d0f000bb82260fac5e5542a773aa44fbcfedf7c193bc2c599",
    "slippage": 100
}
```

### Compound Logics Params

```javascript
// compound-v3: supply-base, withdraw-base
{
    marketId: marketId
    input: {
        token: inputToken,
        amount: inputAmount
    },
    tokenOut: tokenOut,
}

// compound-v3: repay
{
    marketId: marketId
    borrower: USER_ADDRESS,
    tokenIn: tokenIn,
}

// compound-v3: claim
{
    marketId: marketId
    borrower: USER_ADDRESS,
    tokenIn: tokenIn,
}
```

The result contains a list of chains that looks the following:

```json
{
    "marketId": "USDC",
    "input": {
        "token": {
            "chainId": 1,
            "address": "0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48",
            "decimals": 6,
            "symbol": "USDC",
            "name": "USD Coin"
        },
        "amount": "1000"
    },
    "output": {
        "token": {
            "chainId": 1,
            "address": "0xc3d688B66703497DAA19211EEdff47f25384cdc3",
            "decimals": 6,
            "symbol": "cUSDCv3",
            "name": "Compound USDC"
        },
        "amount": "1000"
    }
}
```
