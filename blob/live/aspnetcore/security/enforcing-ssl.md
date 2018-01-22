---
title: "在 ASP.NET Core 应用程序实施 SSL"
author: rick-anderson
description: "演示如何要求在 ASP.NET Core SSL web 应用"
ms.author: riande
manager: wpickett
ms.date: 07/19/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/enforcing-ssl
ms.openlocfilehash: 42d8767fda2d3f3545876f8ca18e0f6fbe6741b8
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/19/2018
---
# <a name="enforcing-ssl-in-an-aspnet-core-app"></a><span data-ttu-id="187ac-103">在 ASP.NET Core 应用程序实施 SSL</span><span class="sxs-lookup"><span data-stu-id="187ac-103">Enforcing SSL in an ASP.NET Core app</span></span>

<span data-ttu-id="187ac-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="187ac-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="187ac-105">本文档说明如何：</span><span class="sxs-lookup"><span data-stu-id="187ac-105">This document shows how to:</span></span>

- <span data-ttu-id="187ac-106">要求将 SSL 用于所有请求 （仅适用于 HTTPS 请求）。</span><span class="sxs-lookup"><span data-stu-id="187ac-106">Require SSL for all requests (HTTPS requests only).</span></span>
- <span data-ttu-id="187ac-107">所有 HTTP 请求重都定向到 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="187ac-107">Redirect all HTTP requests to HTTPS.</span></span>

## <a name="require-ssl"></a><span data-ttu-id="187ac-108">要求 SSL</span><span class="sxs-lookup"><span data-stu-id="187ac-108">Require SSL</span></span>

<span data-ttu-id="187ac-109">[RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute)用于要求 SSL。</span><span class="sxs-lookup"><span data-stu-id="187ac-109">The [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require SSL.</span></span> <span data-ttu-id="187ac-110">你可以修饰控制器或具有此特性的方法，或可以将其应用全局如下所示：</span><span class="sxs-lookup"><span data-stu-id="187ac-110">You can decorate controllers or methods with this attribute or you can apply it globally as shown below:</span></span>

<span data-ttu-id="187ac-111">以下代码添加到`ConfigureServices`中`Startup`:</span><span class="sxs-lookup"><span data-stu-id="187ac-111">Add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-)]

<span data-ttu-id="187ac-112">上面的突出显示的代码需要所有请求都使用`HTTPS`，因此 HTTP 请求将被忽略。</span><span class="sxs-lookup"><span data-stu-id="187ac-112">The highlighted code above requires all requests use `HTTPS`, therefore HTTP requests are ignored.</span></span> <span data-ttu-id="187ac-113">以下突出显示的代码将所有 HTTP 请求重都定向到 HTTPS:</span><span class="sxs-lookup"><span data-stu-id="187ac-113">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-)]

<span data-ttu-id="187ac-114">请参阅[URL 重写中间件](xref:fundamentals/url-rewriting)有关详细信息。</span><span class="sxs-lookup"><span data-stu-id="187ac-114">See [URL Rewriting Middleware](xref:fundamentals/url-rewriting) for more information.</span></span>

<span data-ttu-id="187ac-115">全局需要 HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) 是最佳安全方案。</span><span class="sxs-lookup"><span data-stu-id="187ac-115">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="187ac-116">应用`[RequireHttps]`特性应用到所有控制器不被视为尽可能安全全局需要 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="187ac-116">Applying the `[RequireHttps]` attribute to all controller is not considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="187ac-117">你不能保证新控制器添加到你的应用程序将记住应用`[RequireHttps]`属性。</span><span class="sxs-lookup"><span data-stu-id="187ac-117">You can't guarantee new controllers added to your app will remember to apply the `[RequireHttps]` attribute.</span></span>
