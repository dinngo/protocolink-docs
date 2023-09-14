---
description: This can be called by the Agent to perform additional actions.
---

# Utility

In order to handle special operations that cannot be directly performed by the Agent, such as creating a `CDP` on `Maker`, it is necessary to complete these operations before proceeding with subsequent steps on this `CDP`.

&#x20;Therefore we have established a **utility** contract specifically designed to handle such special operations to assist users in using Protocolink with more flexibility. Users can interact directly with the **MakerUtility** contract via Agent to complete the `openLockETHAndDraw()` and `openLockGemAndDraw()` operations.

<figure><img src="../../.gitbook/assets/MakerUtility.png" alt=""><figcaption></figcaption></figure>
