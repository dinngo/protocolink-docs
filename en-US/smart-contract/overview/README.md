# 🔮 Overview

The following is the contract architecture diagram for Protocolink. Each component of the router will be explained in detail below.&#x20;

<div align="left">

<figure><img src="https://lh5.googleusercontent.com/YaDNu0MTtbyVqmhXZO3vqnTHhEXkgzvP64RlOTbrtVKWX5A9o0p9QZeqAP2UFNzMFRTbhWaq5tOrLXCuFV6tJIpxacQm-wAkYSnnh_I512syfQq1WeWCPUBRei8axTNV7UI9ddXXXXJSFDm7fVA7AkA" alt=""><figcaption></figcaption></figure>

</div>

* [**Router**](router.md): This component serves as the single entry point for users to interact with when executing transactions. The Router forwards user transactions to the respective Agents, which execute the transactions on behalf of the users.
* [**Agent**](agent.md): This component is the execution unit of user transactions. Each Agent is exclusive to one user, and all token approvals are securely held by the Agent. Through the Agent, users can execute various transactions, including: token transfers, liquidity provision, and yield farming. Proxy-Implementation architecture is used in the Agent contract to reduce gas costs for deployment.
* [**Callback**](callback.md): This component is used to enable reentry into the Agent contract. During contract execution, a one-time address is generated and set as the callback address. This address can be used to re-enter the Agent contract later.
* [**Utility**](utility.md): This component provides additional functionality and tools that can be called by the Agent to perform various actions. Utilities include functionalities such as: interacting with different protocols, calculating token prices, and managing user data.

In the following chapters, we will provide a detailed explanation of each component and their interactions in Protocolink.&#x20;
