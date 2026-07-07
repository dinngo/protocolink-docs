---
description: >-
  Current Chain 138 adoption status for Protocolink, including the live fallback
  deployment and the remaining canonical deployment step.
---

# DeFi Oracle Meta Mainnet Adoption Status

DeFi Oracle Meta Mainnet (Chain ID `138`) is prepared for Protocolink SDK, logics, and documentation integration. The current deployment is split into a canonical path and a fallback path.

## Current status

- Network: DeFi Oracle Meta Mainnet
- Chain ID: `138`
- Explorer: `https://blockscout.defi-oracle.io/`
- RPC: `https://rpc.d-bis.org`
- Status: in progress until the canonical Router is deployed

## Live fallback deployment

The fallback deployment is live for Chain 138 integration testing:

- Router: [`0xE7f51632381d0791eC5c05F5585e7b1bFf1de5F5`](https://blockscout.defi-oracle.io/address/0xE7f51632381d0791eC5c05F5585e7b1bFf1de5F5)
- CREATE3Factory: [`0x486B2E145F486eFA0190a60259B5BB464BD6b22b`](https://blockscout.defi-oracle.io/address/0x486B2E145F486eFA0190a60259B5BB464BD6b22b)
- Permit2: [`0x000000000022D473030F116dDEE9F6B43aC78BA3`](https://blockscout.defi-oracle.io/address/0x000000000022D473030F116dDEE9F6B43aC78BA3)

The fallback Router is not the canonical cross-chain Protocolink Router address. It should be replaced after the canonical CREATE3Factory is available on Chain 138.

## Canonical deployment blocker

The canonical Router address is expected to remain:

- Router: `0xDec80E988F4baF43be69c13711453013c212feA8`
- CREATE3Factory prerequisite: `0xFa3e9a110E6975ec868E9ed72ac6034eE4255B64`

Canonical Router deployment is blocked until the production CREATE3Factory is deployed at `0xFa3e9a110E6975ec868E9ed72ac6034eE4255B64`.

## Follow-up

After the canonical CREATE3Factory is deployed, update SDK and docs references from the fallback Router to the canonical Router and move Chain 138 from in-progress to supported.
