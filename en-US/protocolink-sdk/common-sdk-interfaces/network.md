# Network

The Network section provides a set of functions and interfaces for interacting with different blockchain networks. It includes the following:

* The `Network` interface defines the properties of a blockchain network, such as its name, chain ID, RPC URL, native token, wrapped native token, and explorer URL.

```typescript
interface Network {
  id: string;
  chainId: number;
  name: string;
  explorerUrl: string;
  rpcUrl: string;
  nativeToken: {
    chainId: number;
    address: string;
    decimals: number;
    symbol: string;
    name: string;
  };
  wrappedNativeToken: {
    chainId: number;
    address: string;
    decimals: number;
    symbol: string;
    name: string;
  };
  multicall2Address: string;
  multicall3Address: string;
}
```

* The `getNetwork()` function returns the Network object for a given chain ID.

```typescript
function getNetwork(chainId: number): Network;
```

* The `getNetworkId()` function returns the network ID (e.g. "mainnet", "polygon", etc.) for a given chain ID.

```typescript
function getNetworkId(chainId: number): string;
```

* The `ChainId` enum defines constants for the chain IDs of the supported blockchain networks.

```typescript
enum ChainId {
  mainnet = 1,
  polygon = 137,
  arbitrum = 42161,
  zksync = 324,
}
```

* The `NetworkId` enum defines constants for the network IDs of the supported blockchain networks.

```typescript
export enum NetworkId {
  mainnet = 'mainnet',
  polygon = 'polygon',
  arbitrum = 'arbitrum',
  zksync = 'zksync',
}
```

* The `isSupportedChainId()` function returns a boolean indicating whether a given chain ID is supported.

```typescript
function isSupportedChainId(chainId: number): boolean;
```

* The `isSupportedNetworkId()` function returns a boolean indicating whether a given network ID is supported.

```typescript
function isSupportedNetworkId(networkId: string): boolean;
```

* The `newExplorerUrl()` function returns a formatted explorer URL for a given chain ID, explorer type, and data (e.g. transaction hash, address, token address).

```typescript
function newExplorerUrl(chainId: number, type: 'tx' | 'address' | 'token', data: string): string;
```

These functions and interfaces provide a convenient and standardized way of interacting with different blockchain networks, and can be used across multiple libraries and applications.
