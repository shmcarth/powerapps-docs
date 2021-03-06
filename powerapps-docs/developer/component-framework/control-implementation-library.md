---
title: Component implementation library | Microsoft Docs
description: Create custom components using JavaScript or TypeScript
keywords:
ms.author: nabuthuk
manager: kvivek
ms.date: 04/23/2019
ms.service: "powerapps"
ms.suite: ""
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 5d100dc3-bd82-4b45-964c-d90eaebc0735
---

Implementing the component library is one of the key step when you are developing custom components using PowerApps component framework. Developers can implement component library using TypeScript. Each custom component must have a library that includes the definition of a function which returns an object that implements the methods described in the custom component interface. 

The object implements the following methods:

- [init](reference/control/init.md) (Required)
- [updateView](reference/control/updateview.md) (Required)
- [getOutputs](reference/control/getoutputs.md) (Optional)
- [destroy](reference/control/destroy.md) (Required)

These methods controls the lifecycle of the custom component.

