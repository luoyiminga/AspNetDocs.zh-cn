---
uid: webhooks/receiving/dependencies
title: ASP.NET Webhook 接收方依赖项 |Microsoft Docs
author: rick-anderson
description: 接收方依赖项和依赖关系注入在 ASP.NET Webhook。
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.openlocfilehash: c44cfe3ed310aa728a989b108c410e8786e4f514
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/01/2019
ms.locfileid: "57048724"
---
# <a name="aspnet-webhooks-receiver-dependencies"></a>ASP.NET Webhook 接收方依赖项

Microsoft ASP.NET Webhook 旨在通过记住的依赖关系注入。 可以使用替代实现使用依赖项注入引擎替换系统中的大多数依赖项。

请参阅[DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs)有关接收方依赖项的列表。 如果尚未注册任何依赖项，则使用默认实现。 请参阅[ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs)有关默认实现的列表。
