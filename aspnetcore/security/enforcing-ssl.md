---
title: "在 ASP.NET Core 应用程序实施 SSL"
author: rick-anderson
description: "演示如何要求在 ASP.NET Core SSL web 应用"
keywords: "ASP.NET 核心、 SSL、 HTTPS、 RequireHttpsAttribute、 IIS Express"
ms.author: riande
manager: wpickett
ms.date: 07/19/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/enforcing-ssl
ms.openlocfilehash: e8e7d4a69fd681534fb313ff113805bfd6a6d44e
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2017
---
# <a name="enforcing-ssl-in-an-aspnet-core-app"></a><span data-ttu-id="09b42-104">在 ASP.NET Core 应用程序实施 SSL</span><span class="sxs-lookup"><span data-stu-id="09b42-104">Enforcing SSL in an ASP.NET Core app</span></span>

<span data-ttu-id="09b42-105">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="09b42-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="09b42-106">本文档说明如何：</span><span class="sxs-lookup"><span data-stu-id="09b42-106">This document shows how to:</span></span>

- <span data-ttu-id="09b42-107">要求将 SSL 用于所有请求 （仅适用于 HTTPS 请求）。</span><span class="sxs-lookup"><span data-stu-id="09b42-107">Require SSL for all requests (HTTPS requests only).</span></span>
- <span data-ttu-id="09b42-108">所有 HTTP 请求重都定向到 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="09b42-108">Redirect all HTTP requests to HTTPS.</span></span>

## <a name="require-ssl"></a><span data-ttu-id="09b42-109">要求 SSL</span><span class="sxs-lookup"><span data-stu-id="09b42-109">Require SSL</span></span>

<span data-ttu-id="09b42-110">[RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute)用于要求 SSL。</span><span class="sxs-lookup"><span data-stu-id="09b42-110">The [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require SSL.</span></span> <span data-ttu-id="09b42-111">你可以修饰控制器或具有此特性的方法，或可以将其应用全局如下所示：</span><span class="sxs-lookup"><span data-stu-id="09b42-111">You can decorate controllers or methods with this attribute or you can apply it globally as shown below:</span></span>

<span data-ttu-id="09b42-112">以下代码添加到`ConfigureServices`中`Startup`:</span><span class="sxs-lookup"><span data-stu-id="09b42-112">Add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-)]

<span data-ttu-id="09b42-113">上面的突出显示的代码需要所有请求都使用`HTTPS`，因此 HTTP 请求将被忽略。</span><span class="sxs-lookup"><span data-stu-id="09b42-113">The highlighted code above requires all requests use `HTTPS`, therefore HTTP requests are ignored.</span></span> <span data-ttu-id="09b42-114">以下突出显示的代码将所有 HTTP 请求重都定向到 HTTPS:</span><span class="sxs-lookup"><span data-stu-id="09b42-114">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-)]

<span data-ttu-id="09b42-115">请参阅[URL 重写中间件](xref:fundamentals/url-rewriting)有关详细信息。</span><span class="sxs-lookup"><span data-stu-id="09b42-115">See [URL Rewriting Middleware](xref:fundamentals/url-rewriting) for more information.</span></span>

<span data-ttu-id="09b42-116">全局需要 HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) 是最佳安全方案。</span><span class="sxs-lookup"><span data-stu-id="09b42-116">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="09b42-117">应用`[RequireHttps]`特性应用到所有控制器不被视为尽可能安全全局需要 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="09b42-117">Applying the `[RequireHttps]` attribute to all controller is not considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="09b42-118">你不能保证新控制器添加到你的应用程序将记住应用`[RequireHttps]`属性。</span><span class="sxs-lookup"><span data-stu-id="09b42-118">You can't guarantee new controllers added to your app will remember to apply the `[RequireHttps]` attribute.</span></span>

## <a name="set-up-iis-express-for-sslhttps"></a><span data-ttu-id="09b42-119">为 SSL/HTTPS 设置了 IIS Express</span><span class="sxs-lookup"><span data-stu-id="09b42-119">Set up IIS Express for SSL/HTTPS</span></span>

<span data-ttu-id="09b42-120">请参阅[ASP.NET Core 中开发的设置 HTTPS](xref:security/https#iisxpress)。</span><span class="sxs-lookup"><span data-stu-id="09b42-120">See [Setting up HTTPS for development in ASP.NET Core](xref:security/https#iisxpress).</span></span>
