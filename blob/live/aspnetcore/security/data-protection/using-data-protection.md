---
title: "开始使用数据保护 Api"
author: rick-anderson
description: "本文档说明如何使用 ASP.NET Core 数据保护 Api 用于保护和取消保护应用中的数据。"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/using-data-protection
ms.openlocfilehash: 54976a7f2ac13fe445eb2eea204f4f781813030f
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/19/2018
---
# <a name="getting-started-with-the-data-protection-apis"></a><span data-ttu-id="6bfab-103">开始使用数据保护 Api</span><span class="sxs-lookup"><span data-stu-id="6bfab-103">Getting Started with the Data Protection APIs</span></span>

<a name="security-data-protection-getting-started"></a>

<span data-ttu-id="6bfab-104">在其最简单、 保护数据包括以下步骤：</span><span class="sxs-lookup"><span data-stu-id="6bfab-104">At its simplest, protecting data consists of the following steps:</span></span>

1. <span data-ttu-id="6bfab-105">从数据保护提供程序创建一个数据保护程序。</span><span class="sxs-lookup"><span data-stu-id="6bfab-105">Create a data protector from a data protection provider.</span></span>

2. <span data-ttu-id="6bfab-106">调用`Protect`与你想要保护的数据的方法。</span><span class="sxs-lookup"><span data-stu-id="6bfab-106">Call the `Protect` method with the data you want to protect.</span></span>

3. <span data-ttu-id="6bfab-107">调用`Unprotect`想要将返回转换为纯文本的数据的方法。</span><span class="sxs-lookup"><span data-stu-id="6bfab-107">Call the `Unprotect` method with the data you want to turn back into plain text.</span></span>

<span data-ttu-id="6bfab-108">大多数框架和应用模型，如 ASP.NET 或 SignalR，已配置数据保护系统，并将其添加到通过依赖关系注入访问的服务容器。</span><span class="sxs-lookup"><span data-stu-id="6bfab-108">Most frameworks and app models, such as ASP.NET or SignalR, already configure the data protection system and add it to a service container you access via dependency injection.</span></span> <span data-ttu-id="6bfab-109">下面的示例演示配置依赖关系注入的服务容器和注册数据保护堆栈、 接收通过 DI 数据保护提供程序、 创建保护程序和保护然后正在取消保护数据</span><span class="sxs-lookup"><span data-stu-id="6bfab-109">The following sample demonstrates configuring a service container for dependency injection and registering the data protection stack, receiving the data protection provider via DI, creating a protector and protecting then unprotecting data</span></span>

[!code-csharp[Main](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

<span data-ttu-id="6bfab-110">创建一个保护程序时必须提供一个或多个[目的字符串](consumer-apis/purpose-strings.md)。</span><span class="sxs-lookup"><span data-stu-id="6bfab-110">When you create a protector you must provide one or more [Purpose Strings](consumer-apis/purpose-strings.md).</span></span> <span data-ttu-id="6bfab-111">目的字符串都提供了使用者之间的隔离。</span><span class="sxs-lookup"><span data-stu-id="6bfab-111">A purpose string provides isolation between consumers.</span></span> <span data-ttu-id="6bfab-112">例如，使用"green"的目的字符串创建一个保护程序将无法取消保护数据的目的为"紫色"提供的保护程序。</span><span class="sxs-lookup"><span data-stu-id="6bfab-112">For example, a protector created with a purpose string of "green" would not be able to unprotect data provided by a protector with a purpose of "purple".</span></span>

>[!TIP]
> <span data-ttu-id="6bfab-113">实例`IDataProtectionProvider`和`IDataProtector`是线程安全的多个调用方。</span><span class="sxs-lookup"><span data-stu-id="6bfab-113">Instances of `IDataProtectionProvider` and `IDataProtector` are thread-safe for multiple callers.</span></span> <span data-ttu-id="6bfab-114">它是预期，后一个组件获取的引用`IDataProtector`通过调用`CreateProtector`，它将使用该引用，以便多个调用`Protect`和`Unprotect`。</span><span class="sxs-lookup"><span data-stu-id="6bfab-114">It is intended that once a component gets a reference to an `IDataProtector` via a call to `CreateProtector`, it will use that reference for multiple calls to `Protect` and `Unprotect`.</span></span>
>
><span data-ttu-id="6bfab-115">调用`Unprotect`将引发 CryptographicException，如果无法验证或中译解出来的受保护的负载。</span><span class="sxs-lookup"><span data-stu-id="6bfab-115">A call to `Unprotect` will throw CryptographicException if the protected payload cannot be verified or deciphered.</span></span> <span data-ttu-id="6bfab-116">某些组件可能想要忽略错误期间取消保护操作;组件它读取身份验证 cookie 可能处理此错误和请求则将视为根本具有任何 cookie，而不使迫切地请求失败。</span><span class="sxs-lookup"><span data-stu-id="6bfab-116">Some components may wish to ignore errors during unprotect operations; a component which reads authentication cookies might handle this error and treat the request as if it had no cookie at all rather than fail the request outright.</span></span> <span data-ttu-id="6bfab-117">需要此行为的组件应专门捕获 CryptographicException，而不是忽略所有异常。</span><span class="sxs-lookup"><span data-stu-id="6bfab-117">Components which want this behavior should specifically catch CryptographicException instead of swallowing all exceptions.</span></span>
