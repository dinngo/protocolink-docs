# Web3Toolkit

The `Web3Toolkit` class is used to interact with RPC in order to simplify the development process. It has the following properties and methods.

* Properties:
  * `chainId`: A readonly property that returns the current chain ID.
  * `network`: A readonly property that returns the `Network` object for the current chain.
  * `provider`: A readonly property that returns the web3 provider.
  * `nativeToken`: A readonly property that returns the `Token` class instance for the native token of the current chain.
  * `wrappedNativeToken`: A readonly property that returns the `Token` class instance for the wrapped native token of the current chain.
* Constructor:
  * `constructor(chainId, provider?)`: A constructor that initializes the `Web3Toolkit` object with a specified `chainId` and optional `provider`.
* Instance methods:
  * `getToken`: A method that returns a `Token` class instance for the specified token or token address.
  * `getTokens`: A method that returns an array of `Token` class instances for the specified array of token addresses.
  * `getBalance`: A method that returns the balance of the specified account for the specified token or token address.
  * `getAllowance`: A method that returns the allowance of the specified account for the specified token or token address and spender.
  * `getAllowances`: A method that returns an array of allowances for the specified account, array of token addresses, and spender.
