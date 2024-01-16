# Token

The Token section provides a set of functions and interfaces for interacting with tokens. It includes the following:

* The `ELASTIC_ADDRESS` constant string representing the substitute address for native token.

```typescript
const ELASTIC_ADDRESS = "0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE";
```

* The `TokenObject` interface representing the structure of a token object. It has the following properties:
  * `chainId`: The ID of the blockchain the token is on.
  * `address`: The address of the token.
  * `decimals`: The number of decimal places the token has.
  * `symbol`: The symbol of the token.
  * `name`: The name of the token.

```typescript
interface TokenObject {
  chainId: number;
  address: string;
  decimals: number;
  symbol: string;
  name: string;
}
```

* The `Token` class is designed to address the issues encountered in the interaction and conversion of tokens. It has the following properties and methods.
  * Properties:
    * `chainId`: The ID of the blockchain the token is on.
    * `address`: The address of the token.
    * `decimals`: The number of decimal places the token has.
    * `symbol`: The symbol of the token.
    * `name`: The name of the token.
  * Constructors:
    * `constructor(chainId, address, decimals, symbol, name)`: Creates a new `Token` instance.
    * `constructor(tokenObject)`: Creates a new `Token` instance from a `TokenObject`.
  * Static methods:
    * The static `isNative()` function returns true if the token is the native token.
    * The static `isWrapped()` function returns true if the token is the wrapped native token.
  * Instance methods:
    * `isNative`: Returns true if the token is the native token.
    * `isWrapped`: Returns true if the token is the wrapped native token.
    * `wrapped`: Returns the wrapped native token if the token is the native token, or returns the token itself.
    * `unwrapped`: Returns the native token if the token is the wrapped native token, or returns the token itself.
    * `elasticAddress`: Returns the `ELASTIC_ADDRESS` if the token is the native token, or return the token address itself.
    * `sortsBefore`: Returns true if the token sorts before the given token, based on the comparison of their addresses in lowercase.
    * `toObject`: Returns a `TokenObject` representing the token class instannce.

```typescript
class Token {
  readonly chainId: number;
  readonly address: string;
  readonly decimals: number;
  readonly symbol: string;
  readonly name: string;
  constructor(chainId: number, address: string, decimals: number, symbol: string, name: string);
  constructor(tokenObject: TokenObject);
  static isNative(chainId: number, address: string): boolean;
  static isNative(token: TokenTypes): boolean;
  static isWrapped(chainId: number, address: string): boolean;
  static isWrapped(token: TokenTypes): boolean;
  is(token: TokenTypes): boolean;
  get isNative(): boolean;
  get isWrapped(): boolean;
  get wrapped(): Token;
  get unwrapped(): Token;
  get elasticAddress(): string;
  sortsBefore(token: TokenTypes): boolean;
  toObject(): TokenObject;
}
```

* The `TokenTypes` type represents either a token object or an instance of the `Token` class.

```typescript
type TokenTypes = TokenObject | Token;
```

* The `TokenAmountObject` interface representing the structure of a token amount object. It has the following properties:
  * The `token` property represents a token object or `Token` class instance
  * The `amount` property is a string that represents the amount of the token.

```typescript
interface TokenAmountObject {
    token: TokenTypes;
    amount: string;
}
```

* The `TokenAmountPair` type representing an array with two elements. The first element is a a token object or `Token` class instance and the second element is a string representing the amount of token.

```typescript
type TokenAmountPair = [TokenTypes, string];
```

* The `TokenAmount` class is designed to address the issues encountered in the interaction and conversion of token amounts. It has the following properties and methods.
  * Properties:
    * `token`: A `Token` class instance.
    * `amount`: A string representing the amount of token.
  * Constructors:
    * `constructor(token, amount?)`: Creates a new `TokenAmount` instance.
    * `constructor(tokenAmountObject)`: Creates a new `TokenAmount` instance from a `TokenAmountObject`.
    * `constructor(tokenAmountPair)`: Creates a new `TokenAmount` instance from a `TokenAmountPair`.
    * `constructor(tokenAmount)`: Creates a new `TokenAmount` instance from an existing `TokenAmount` instance.
  * Instance methods:
    * `amountWei`: Returns the amount of tokens in wei.
    * `set`: Sets the amount property.
    * `setWei`: Sets the amount property in wei.
    * `add`: Adds amount.
    * `addWei`: Adds amount in wei.
    * `sub`: Subtracts amount.
    * `subWei`: Subtracts amount in wei.
    * `isZero`: Returns true if the amount property is zero.
    * `eq`: Returns true if the amount property is equal to the given token amount.
    * `gt`: Returns true if the amount property is greater than the given token amount.
    * `gte`: Returns true if the amount property is greater than or equal to the given token amount.
    * `lt`: Returns true if the amount property is less than the given token amount.
    * `lte`: Returns true if the amount property is less than or equal to the given token amount.
    * `toObject`: Returns a `TokenAmountObject` representing the token amount class instance.
    * `clone`: Returns a new `TokenAmount` instance with the same token and amount as the current instance.
* `getNativeToken`: A function that takes in a `chainId` and returns a Token class instance representing the native token of the corresponding blockchain network.
* `getWrappedNativeToken`: A function that takes in a `chainId` and returns a Token class instance representing the wrapped native token of the corresponding blockchain network.
* `sortByAddress`: A function that takes in an array of `Token`, `TokenObject`, `TokenAmount`, or `TokenAmountObject` objects, and sorts them in ascending order based on their respective addresses. The sorted array is then returned.
