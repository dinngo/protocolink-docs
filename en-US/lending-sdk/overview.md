# 🔮 Overview

As [build-logics.md](../protocolink-sdk/build-logics.md "mention") describes the general way to build logics by [Protocolink JS SDK ](broken-reference)from scratch, Lending SDK aims to provide a user friendly tool to build around lending functions.&#x20;

## Introduction to Lending SDK

Lending SDK is open source tool which empowers developers to rapidly build applications and enhance the user experience with the lending protocols.

Building an application on top of the lending protocols can be a challenging and time-consuming process. This is due to the inherent risks associated with working with smart contracts, as well as the complex calculations and encoding required for various use cases, such as leverage, deleverage, collateral swap, and zaps.

Lending SDK reduces the need for developers to spend time and resources on building and maintaining their own smart contracts. The Lending SDK leverages [Protocolink](../why-protocolink.md), which provides the ability to add more use cases in a highly modular way without deploying new smart contracts, and significantly reducing user risk.

## Architecture Diagram

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption><p><strong>Logic:</strong> the above illustrates the actions performed during execution, such as: approval, swap, supply, borrow, flash loans, and so on.</p></figcaption></figure>

## Developers

Developers can adapt the Lending SDK into their frontend or backend services. With the use cases supported by the Lending SDK, they can easily generate transaction data, or utilize it for complex transaction scenarios. This significantly lowers the barrier for developers, allowing them to focus on providing a better user experience or infrastructure.

## TypeScript SDK

The Lending SDK generates and return transaction data based on the specified use cases. It provides estimated execution results, calculates the optimal path for each use case, and forwards protocol logics to [Protocolink](../why-protocolink.md).

## Examples

The Lending SDK stands as an efficient and user-friendly tool, specifically designed to assist developers in creating their own position management tools. This SDK streamlines the development process, making it more accessible for developers to build sophisticated and effective applications in the lending space.&#x20;

An example of the SDK's application is [Furucombo lending dashboard](https://furucombo.app/lending). This dashboard effectively utilizes the Lending SDK to offer a seamless and intuitive interface. It enables users to effortlessly manage their positions across various lending platforms. This integration not only showcases the SDK's versatility but also its capability to enhance user experience through simplified position management in a complex financial ecosystem.

Here are some transactions generated through Lending SDK:

[Aave V3 Deleverage on Polygon](https://www.tdly.co/tx/137/0x3f28c81cdb2a058b77c7ebcfafc487ab74666147e3b0fb60bb767d833a567ed4)

[Radiant V2 Debt swap on Arbitrum](https://www.tdly.co/tx/42161/0x77f2872312d8cc126bfbd555fb51f402638d5d7444021d0d5aa18fb9f04c3e4b)

[Radiant V2 Leverage on Arbitrum](https://www.tdly.co/tx/42161/0x4af3d1b4df4fe8c594763a3266cc125778aba4c9226b34773659cf43931eafa3)

[Radiant V2 Deleverage on Arbitrum](https://www.tdly.co/tx/42161/0x93f70429f3c906c5e88eab2ac80e91771fe8b2b8ecdc665c9d48eb7a76ed6af1)

[Compound V3 Deleverage on Arbitrum](https://www.tdly.co/tx/42161/0x11bb3296c6d44800d38e9c4f7e46c9a80247808c571e30ce1dfbeccb7578b49b)

[Aave V3 Collateral Swap on Arbitrum](https://www.tdly.co/tx/42161/0x88b7c18f0d184e5fa7cb3b0e34e765d72a09eea64c84b38ec48a7834d0aa3a6d)







