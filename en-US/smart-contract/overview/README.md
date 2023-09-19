# 🔮 Overview

<div align="left">

<figure><img src="https://lh5.googleusercontent.com/YaDNu0MTtbyVqmhXZO3vqnTHhEXkgzvP64RlOTbrtVKWX5A9o0p9QZeqAP2UFNzMFRTbhWaq5tOrLXCuFV6tJIpxacQm-wAkYSnnh_I512syfQq1WeWCPUBRei8axTNV7UI9ddXXXXJSFDm7fVA7AkA" alt=""><figcaption></figcaption></figure>

</div>

**When a user tries to execute a transaction**

* Token allowances **MUST** be set directly or through [Permit2](https://github.com/Uniswap/permit2) in advance.
* The `calldata` is passed to an **exclusive Agent** through the Router.
* The Agent transfers tokens from the user and executes the `calldata`.
* After the `calldata` is executed, the Agent returns tokens back to the user.

**Protocolink contracts consist of**

* [**Router**](router.md): The single entry point for users to interact with. Router forwards `calldata` to an Agent when executing a transaction.
* [**Agent**](agent.md): The execution unit of user transactions. Agent employs the proxy pattern to execute `calldata` like token transfer, liquidity provision, and yield farming.
* [**Callback**](callback.md): The entry point for protocol callbacks to re-enter an Agent in a transaction.
* [**Utility**](utility.md): The extensions for Agents to perform extra actions like interacting with specific protocols, calculating token prices, and managing user data.

You can find the details of each component on the following pages.
