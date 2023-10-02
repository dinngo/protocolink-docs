---
description: >-
  A router system which consolidates protocol interactions within a secure
  Router/Agent architecture in a single transaction.
---

# 🔮 Overview

Protocolink is protocol-agnostic. All protocol-related code is defined in the [protocolink-logics](https://github.com/dinngo/protocolink-logics) repository instead of in the contracts. Protocolink also offers an [API](https://docs.protocolink.com/integrate-api/overview) and an [SDK](https://docs.protocolink.com/integrate-js-sdk/overview) for developers to create transactions.

<div align="left">

<figure><img src="https://lh5.googleusercontent.com/YaDNu0MTtbyVqmhXZO3vqnTHhEXkgzvP64RlOTbrtVKWX5A9o0p9QZeqAP2UFNzMFRTbhWaq5tOrLXCuFV6tJIpxacQm-wAkYSnnh_I512syfQq1WeWCPUBRei8axTNV7UI9ddXXXXJSFDm7fVA7AkA" alt=""><figcaption></figcaption></figure>

</div>

**When a user tries to execute a transaction**

* ERC-20 tokens are transferred through the [Permit2](https://github.com/Uniswap/permit2).
* The data is passed to an **exclusive Agent** through the Router.
* The Agent transfers tokens from the user and executes the data.
* After the data is executed, the Agent returns tokens back to the user.

**Protocolink contracts consist of**

* [**Router**](router.md): The single entry point for users to interact with. The Router forwards the data to an Agent when executing a transaction.
* [**Agent**](agent.md): The execution unit of user transactions. The Agent executes the data like token transfer, liquidity provision, and yield farming.
* [**Callback**](callback.md): The entry point for protocol callbacks to re-enter the Agent in a transaction.
* [**Utility**](utility.md): The extensions for the Agent to perform extra actions like interacting with specific protocols, calculating token prices, and managing user data.

You can find the details of each component on the following pages.
