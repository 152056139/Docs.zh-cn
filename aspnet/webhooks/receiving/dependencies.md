---
uid: webhooks/receiving/dependencies
title: ASP.NET Webhook 接收方依赖项 |Microsoft Docs
author: rick-anderson
description: 接收方依赖项和依赖关系注入在 ASP.NET Webhook。
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.openlocfilehash: c44cfe3ed310aa728a989b108c410e8786e4f514
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834934"
---
# <a name="aspnet-webhooks-receiver-dependencies"></a><span data-ttu-id="5a2dd-103">ASP.NET Webhook 接收方依赖项</span><span class="sxs-lookup"><span data-stu-id="5a2dd-103">ASP.NET WebHooks receiver dependencies</span></span>

<span data-ttu-id="5a2dd-104">Microsoft ASP.NET Webhook 旨在通过记住的依赖关系注入。</span><span class="sxs-lookup"><span data-stu-id="5a2dd-104">Microsoft ASP.NET WebHooks is designed with dependency injection in mind.</span></span> <span data-ttu-id="5a2dd-105">可以使用替代实现使用依赖项注入引擎替换系统中的大多数依赖项。</span><span class="sxs-lookup"><span data-stu-id="5a2dd-105">Most dependencies in the system can be replaced with alternative implementations using a dependency injection engine.</span></span>

<span data-ttu-id="5a2dd-106">请参阅[DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs)有关接收方依赖项的列表。</span><span class="sxs-lookup"><span data-stu-id="5a2dd-106">Please see [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) for a list of receiver dependencies.</span></span> <span data-ttu-id="5a2dd-107">如果尚未注册任何依赖项，则使用默认实现。</span><span class="sxs-lookup"><span data-stu-id="5a2dd-107">If no dependency has been registered, a default implementation is used.</span></span> <span data-ttu-id="5a2dd-108">请参阅[ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs)有关默认实现的列表。</span><span class="sxs-lookup"><span data-stu-id="5a2dd-108">Please see [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) for a list of default implementations.</span></span>
