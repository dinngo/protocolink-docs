# 🔮 Overview

{% hint style="info" %}
The Compound Kit is currently in testing phase. We welcome your feedback, and if you have any questions or concerns, please feel free to reach out to us.
{% endhint %}

## Introduction to Compound Kit

The Compound Kit is an open source SDK/API which empowers developers to rapidly build intent-centric applications and enhance the user experience with the Compound protocol.

Building an application on top of the Compound Protocol can be a challenging and time-consuming process. This is due to the inherent risks associated with working with smart contracts, as well as the complex calculations and encoding required for various intents, such as leverage, deleverage, collateral swap, and zaps.

The Compound Kit reduces the need for developers to spend time and resources on building and maintaining their own smart contracts. The Compound Kit leverages [Protocolink](../why-protocolink.md), which provides the ability to add more intents in a highly modular way without deploying new smart contracts, and significantly reducing user risk.

## Architecture Diagram

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption><p><strong>Logic:</strong> the above illustrates the actions performed during execution, such as: approval, swap, supply, borrow, flash loans, and so on.</p></figcaption></figure>

## Developers

Developers can adapt the Compound Kit SDK/API into their frontend or backend services. With the intents supported by the Compound Kit, they can easily generate transaction data, or utilize it for complex transaction scenarios. This significantly lowers the barrier for developers, allowing them to focus on providing a better user experience or infrastructure.

## Compound Kit SDK & API

The Compound Kit API and its TypeScript SDK generate and return transaction data based on the specified intents. It provides estimated execution results, calculates the optimal path for each intent, and forwards protocol logics to [Protocolink](../why-protocolink.md).

### [TypeScript SDK](sdk/)

Interacts with the Compound Kit API without having to handle the low-level details of the API requests and responses.

### [API](api.md)

Provides information about Compound markets, encodes a variety of intents, and includes comprehensive Swagger documentation for ease of use.

## Tenderly Simulation Examples

Employ Balancer flash loan (0% fee) on Ethereum ETH market

* [Leverage 1 wstETH collateral](https://dashboard.tenderly.co/shared/fork/simulation/bac1babd-74ca-492c-a9d6-9b08cddb320c)
* [Deleverage \~10 ETH base by wstETH collateral](https://dashboard.tenderly.co/shared/fork/simulation/14d750bb-215c-4d86-a31a-55535bce18f5)
* [Collateral-Swap 10 wstETH to cbETH](https://dashboard.tenderly.co/shared/fork/simulation/64a3a5f4-a7d4-403d-a396-945b911da42c)
* [Zap-Supply 0.1 ETH to cbETH collateral](https://dashboard.tenderly.co/shared/fork/simulation/9a041e5d-a2bf-4ba7-a5e7-dbed6a7d4475)
* [Zap-Supply 100 USDC to cWETHv3](https://dashboard.tenderly.co/shared/fork/simulation/8aa83672-a36c-46c5-ae9d-7dfa64e5c79f)
* [Zap-Withdraw 1 wstETH to ETH](https://dashboard.tenderly.co/shared/fork/simulation/c1232449-f1b4-4128-aa96-d8d56595c9bf)
* [Zap-Withdraw 10 cWETHv3 to cbETH](https://dashboard.tenderly.co/shared/fork/simulation/7dcaf50a-fcf3-40b1-9f97-bcbb00ed8262)
* [Zap-Repay by 1 WBTC](https://dashboard.tenderly.co/shared/fork/simulation/2a283752-4f42-4564-8a4a-4d281f88c978)
* [Zap-Borrow 1 ETH to cbETH](https://dashboard.tenderly.co/shared/fork/simulation/9bd8bb16-f864-4cde-8f41-073284e0561a)

Employ Aave v3 flash loan (0.05% fee) on Ethereum USDC market

* [Deleverage \~2800 USDC by UNI collateral](https://dashboard.tenderly.co/shared/fork/simulation/1a53ecaa-8a29-40c9-bb93-bac74a445f2c)
* [Collateral-Swap all UNI to ETH](https://dashboard.tenderly.co/shared/fork/simulation/13c010d1-b8bf-4ca1-b505-262a3d9d47af)

