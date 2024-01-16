# Swap & Supply (Example)

In the following section, we will guide you step by step on how to swap tokens using Uniswap and supply tokens to the Aave protocol.

### Step 1: Prepare Strategy Using Tokens

To earn interest by supplying to the Aave Protocol, users holding 1000 USDC need to exchange it for WBTC. This scenario involves two separate steps of logic:

1. Exchange **USDC** to **WBTC** by **Uniswap V3**&#x20;
2. Supply WBTC to get **aWBTC** by **Aave V3**

Retrieve the necessary token information by utilizing the Request Tokens API.

```javascript
const client = axios.create({ baseURL: 'https://api.protocolink.com' });
const getSwapTokens = async () => {
    const result = await client.get('/v1/protocols/1/uniswap-v3/swap-token/tokens');
    return result.data;
}
```

### Step 2: Build the Logic

The initial logic quote request is to exchange 1000 USDC to WBTC using Uniswap V3. The request is as follows:

```javascript
const getSwapQuote = async (inputToken, inputAmount, tokenOut) => {
    const result = await client.post('/v1/protocols/1/uniswap-v3/swap-token/quote', {
        input: {
            token: inputToken,
            amount: inputAmount
        },
        tokenOut: tokenOut,
    });
    return result.data;
}

const inputToken = USDC;
const inputAmount = '1000';
const tokenOut = WBTC;

const swapQuote = await getSwapQuote(inputToken, inputAmount, tokenOut);
const swapLogic = {
    rid: 'uniswap-v3:swap-token',
    fields: swapQuote,
}
```

The second logic involves supplying the output token (WBTC) and its corresponding amount to Aave V3 in exchange for aWBTC. The request is as follows:

```javascript
const getSupplyQuote = async (inputToken, inputAmount, tokenOut) => {
    const result = await client.post('/v1/protocols/1/aave-v3/supply/quote', {
        input: {
            token: inputToken,
            amount: inputAmount
        },
        tokenOut: tokenOut,
    });
    return result.data;
}

const inputToken = swapQuote.output.token;
const inputAmount = swapQuote.output.amount;
const tokenOut = aWBTC;

const supplyQuote = await getSupplyQuote(inputToken, inputAmount, tokenOut);
const supplyLogic = {
    rid: 'aave-v3:supply',
    fields: supplyQuote,
}
```

### Step 3: Estimate Router Data

This endpoint offers an estimation of the amount of funds to be spent (**funds**) and the number of balances to be obtained (**balances**) from the transaction. Additionally, it will identify any necessary approvals that the user must execute (**approvals**) before proceeding with the transaction and whether there is any Permit2 data that the user needs to sign (**permitData**) beforehand.

```javascript
const getEstimateResult = async (chainId, account, logics) => {
    const result = await client.post('/v1/transactions/estimate', {
        chainId: chainId,
        account: account,
        logics: logics,
    });
    return result.data;
}

const chainId = 1;
const account = USER_ADDRESS;
const logics = [swapLogic, supplyLogic];

const estimateResult = await getEstimateResult(chainId, account, logics);
```

To ensure a smoother experience in the future, it is recommended to use Permit2, which enables users to avoid sending multiple transactions.

### Step 4: Build Router Transaction

Once the logic estimate request is confirmed, the user in this scenario will permit USDC for the first time.

```javascript
const signer = provider.getSigner(account);
const permitData = estimateResult.permitData;
const permitSig = await signer._signTypedData(permitData.domain, permitData.types, permitData.values);
```

You can then use the request router transaction data method to get the transaction request.

```javascript
const getRouterTransactionRequest = async (chainId, account, logics) => {
    const result = await client.post('/v1/transactions/build', {
        chainId: chainId,
        account: account,
        logics: logics,
        permitData: permitData,
        permitSig: permitSig,
    });
    return result.data;
}

const chainId = 1;
const account = USER_ADDRESS;
const logics = [swapLogic, supplyLogic];

const transactionRequest = await getRouterTransactionRequest(chainId, account, logics);
```

### Step 5: Sending the Transaction

The transaction to Router is what needs needs to be sent next. If you are using the `ethers` package, you can refer to the code example provided below to send the transaction:

```javascript
const signer = provider.getSigner(account);
const tx = await signer.sendTransaction(transactionRequest);
```
