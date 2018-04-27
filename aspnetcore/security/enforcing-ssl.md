---
title: 强制实施 HTTPS 在 ASP.NET 核心
author: rick-anderson
description: 演示如何要求在 ASP.NET Core HTTPS/TLS web 应用。
manager: wpickett
ms.author: riande
ms.date: 2/9/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: 0509bebe430c6ba213031a2cb7cb91bb7a39566d
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/18/2018
---
# <a name="enforce-https-in-an-aspnet-core"></a><span data-ttu-id="fa5ce-103">强制实施 HTTPS 在 ASP.NET 核心</span><span class="sxs-lookup"><span data-stu-id="fa5ce-103">Enforce HTTPS in an ASP.NET Core</span></span>

<span data-ttu-id="fa5ce-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fa5ce-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="fa5ce-105">本文档说明如何：</span><span class="sxs-lookup"><span data-stu-id="fa5ce-105">This document shows how to:</span></span>

- <span data-ttu-id="fa5ce-106">所有请求需要 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="fa5ce-106">Require HTTPS for all requests.</span></span>
- <span data-ttu-id="fa5ce-107">所有 HTTP 请求重都定向到 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="fa5ce-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="fa5ce-108">执行**不**使用`RequireHttpsAttribute`接收敏感信息的 Web api。</span><span class="sxs-lookup"><span data-stu-id="fa5ce-108">Do **not** use `RequireHttpsAttribute` on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="fa5ce-109">`RequireHttpsAttribute` 使用 HTTP 状态代码将从 HTTP 到 HTTPS 的浏览器重定向。</span><span class="sxs-lookup"><span data-stu-id="fa5ce-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="fa5ce-110">API 客户端可能无法理解或遵循从 HTTP 到 HTTPS 的重定向。</span><span class="sxs-lookup"><span data-stu-id="fa5ce-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="fa5ce-111">此类客户端可能会通过 HTTP 发送信息。</span><span class="sxs-lookup"><span data-stu-id="fa5ce-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="fa5ce-112">Web Api 应具有下列任一：</span><span class="sxs-lookup"><span data-stu-id="fa5ce-112">Web APIs should either:</span></span>
>
>* <span data-ttu-id="fa5ce-113">不在 HTTP 上侦听。</span><span class="sxs-lookup"><span data-stu-id="fa5ce-113">Not listen on HTTP.</span></span>
>* <span data-ttu-id="fa5ce-114">关闭与状态代码 400 （错误请求） 的连接并不为请求提供服务。</span><span class="sxs-lookup"><span data-stu-id="fa5ce-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="fa5ce-115">需要 HTTPS</span><span class="sxs-lookup"><span data-stu-id="fa5ce-115">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE[](~/includes/2.1.md)]

<span data-ttu-id="fa5ce-117">我们建议所有 ASP.NET 核心 web 应用调用`UseHttpsRedirection`将所有 HTTP 请求重定向到 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="fa5ce-117">We recommend all ASP.NET Core web apps call `UseHttpsRedirection` to redirect all HTTP requests to HTTPS.</span></span> <span data-ttu-id="fa5ce-118">如果`UseHsts`调用在应用中，它必须调用之前`UseHttpsRedirection`。</span><span class="sxs-lookup"><span data-stu-id="fa5ce-118">If `UseHsts` is called in the app, it must be called before `UseHttpsRedirection`.</span></span>

<span data-ttu-id="fa5ce-119">下面的代码调用`UseHttpsRedirection`中`Startup`类：</span><span class="sxs-lookup"><span data-stu-id="fa5ce-119">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

<span data-ttu-id="fa5ce-120">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span><span class="sxs-lookup"><span data-stu-id="fa5ce-120">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span></span>


<span data-ttu-id="fa5ce-121">下面的代码：</span><span class="sxs-lookup"><span data-stu-id="fa5ce-121">The following code:</span></span>

<span data-ttu-id="fa5ce-122">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span><span class="sxs-lookup"><span data-stu-id="fa5ce-122">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span></span>

* <span data-ttu-id="fa5ce-123">集`RedirectStatusCode`。</span><span class="sxs-lookup"><span data-stu-id="fa5ce-123">Sets `RedirectStatusCode`.</span></span>
* <span data-ttu-id="fa5ce-124">将 HTTPS 端口设置为 5001。</span><span class="sxs-lookup"><span data-stu-id="fa5ce-124">Sets the HTTPS port to 5001.</span></span>

::: moniker-end


::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="fa5ce-125">[RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute)用于需要 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="fa5ce-125">The [RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) is used to require HTTPS.</span></span> <span data-ttu-id="fa5ce-126">`[RequireHttpsAttribute]` 可修饰控制器或方法，或可以全局应用。</span><span class="sxs-lookup"><span data-stu-id="fa5ce-126">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="fa5ce-127">若要全局应用该属性，以下代码添加到`ConfigureServices`中`Startup`:</span><span class="sxs-lookup"><span data-stu-id="fa5ce-127">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

<span data-ttu-id="fa5ce-128">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span><span class="sxs-lookup"><span data-stu-id="fa5ce-128">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span></span>

<span data-ttu-id="fa5ce-129">前面的突出显示的代码中需要的所有请求都使用`HTTPS`; 因此，HTTP 请求将被忽略。</span><span class="sxs-lookup"><span data-stu-id="fa5ce-129">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="fa5ce-130">以下突出显示的代码将所有 HTTP 请求重都定向到 HTTPS:</span><span class="sxs-lookup"><span data-stu-id="fa5ce-130">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

<span data-ttu-id="fa5ce-131">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span><span class="sxs-lookup"><span data-stu-id="fa5ce-131">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span></span>

<span data-ttu-id="fa5ce-132">有关详细信息，请参阅[URL 重写中间件](xref:fundamentals/url-rewriting)。</span><span class="sxs-lookup"><span data-stu-id="fa5ce-132">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>

<span data-ttu-id="fa5ce-133">全局需要 HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) 是最佳安全方案。</span><span class="sxs-lookup"><span data-stu-id="fa5ce-133">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="fa5ce-134">应用`[RequireHttps]`特性应用到所有控制器/Razor 页面不会被视为尽可能安全全局需要 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="fa5ce-134">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="fa5ce-135">你不能保证`[RequireHttps]`添加新控制器和 Razor 页时应用特性。</span><span class="sxs-lookup"><span data-stu-id="fa5ce-135">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="fa5ce-136">HTTP 严格的传输安全协议 (HSTS)</span><span class="sxs-lookup"><span data-stu-id="fa5ce-136">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="fa5ce-137">每个[OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project)， [HTTP 严格传输安全 (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet)是由 web 应用程序通过使用特殊的响应标头指定可以选择使用的安全增强。</span><span class="sxs-lookup"><span data-stu-id="fa5ce-137">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that is specified by a web application through the use of a special response header.</span></span> <span data-ttu-id="fa5ce-138">支持的浏览器收到此标头后该浏览器将阻止从正在通过 HTTP 发送到指定的域的任何通信，并改为将通过 HTTPS 发送的所有通信。</span><span class="sxs-lookup"><span data-stu-id="fa5ce-138">Once a supported browser receives this header that browser will prevent any communications from being sent over HTTP to the specified domain and will instead send all communications over HTTPS.</span></span> <span data-ttu-id="fa5ce-139">它还可以防止 HTTPS 单击通过在浏览器上的提示。</span><span class="sxs-lookup"><span data-stu-id="fa5ce-139">It also prevents HTTPS click through prompts on browsers.</span></span>

<span data-ttu-id="fa5ce-140">ASP.NET 核心 2.1 预览版 1 或更高版本实现与 HSTS`UseHsts`扩展方法。</span><span class="sxs-lookup"><span data-stu-id="fa5ce-140">ASP.NET Core 2.1 preview1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="fa5ce-141">下面的代码调用`UseHsts`时应用程序不在[开发模式](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="fa5ce-141">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

<span data-ttu-id="fa5ce-142">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="fa5ce-142">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span></span>

<span data-ttu-id="fa5ce-143">`UseHsts` 不建议在开发过程中的因为 HSTS 标头是高度可通过浏览器缓存。</span><span class="sxs-lookup"><span data-stu-id="fa5ce-143">`UseHsts` is not recommend in development because the HSTS header is highly cachable by browsers.</span></span> <span data-ttu-id="fa5ce-144">默认情况下，UseHsts 排除本地环回地址。</span><span class="sxs-lookup"><span data-stu-id="fa5ce-144">By default, UseHsts excludes the local loopback address.</span></span>

<span data-ttu-id="fa5ce-145">下面的代码：</span><span class="sxs-lookup"><span data-stu-id="fa5ce-145">The following code:</span></span>

<span data-ttu-id="fa5ce-146">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span><span class="sxs-lookup"><span data-stu-id="fa5ce-146">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span></span>

* <span data-ttu-id="fa5ce-147">设置预加载参数 Strict 传输安全标头。</span><span class="sxs-lookup"><span data-stu-id="fa5ce-147">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="fa5ce-148">预加载不属于[RFC HSTS 规范](https://tools.ietf.org/html/rfc6797)，但若要预加载 HSTS 站点上执行全新安装的 web 浏览器都支持。</span><span class="sxs-lookup"><span data-stu-id="fa5ce-148">Preload is not part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="fa5ce-149">有关详细信息，请参阅 [https://hstspreload.org/](https://hstspreload.org/)。</span><span class="sxs-lookup"><span data-stu-id="fa5ce-149">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="fa5ce-150">使[includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2)，该方案 HSTS 策略适用于主机子域。</span><span class="sxs-lookup"><span data-stu-id="fa5ce-150">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="fa5ce-151">显式将 Strict 传输安全标头到的最大有效期参数设置为 60 天。</span><span class="sxs-lookup"><span data-stu-id="fa5ce-151">Explicitly sets the max-age parameter of the Strict-Transport-Security header to to 60 days.</span></span> <span data-ttu-id="fa5ce-152">如果未设置，默认值为 30 天。</span><span class="sxs-lookup"><span data-stu-id="fa5ce-152">If not set, defaults to 30 days.</span></span> <span data-ttu-id="fa5ce-153">请参阅[最长时间指令](https://tools.ietf.org/html/rfc6797#section-6.1.1)有关详细信息。</span><span class="sxs-lookup"><span data-stu-id="fa5ce-153">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="fa5ce-154">将添加`example.com`到的主机排除列表。</span><span class="sxs-lookup"><span data-stu-id="fa5ce-154">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="fa5ce-155">`UseHsts` 排除以下环回主机：</span><span class="sxs-lookup"><span data-stu-id="fa5ce-155">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="fa5ce-156">`localhost` : IPv4 环回地址。</span><span class="sxs-lookup"><span data-stu-id="fa5ce-156">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="fa5ce-157">`127.0.0.1` : IPv4 环回地址。</span><span class="sxs-lookup"><span data-stu-id="fa5ce-157">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="fa5ce-158">`[::1]` : IPv6 环回地址。</span><span class="sxs-lookup"><span data-stu-id="fa5ce-158">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="fa5ce-159">前面的示例演示如何添加其他主机。</span><span class="sxs-lookup"><span data-stu-id="fa5ce-159">The preceding example shows how to add additional hosts.</span></span>
::: moniker-end


::: moniker range=">= aspnetcore-2.1"
<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="fa5ce-160">选择退出的 HTTPS 在项目创建</span><span class="sxs-lookup"><span data-stu-id="fa5ce-160">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="fa5ce-161">ASP.NET 核心 2.1 和更高版本 （从 Visual Studio 或 dotnet 命令行） 的 web 应用程序模板启用[HTTPS 重定向](#require)和[HSTS](#hsts)。</span><span class="sxs-lookup"><span data-stu-id="fa5ce-161">The ASP.NET Core 2.1 and later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="fa5ce-162">对于不需要 HTTPS 的部署，你可以选择退出的 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="fa5ce-162">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="fa5ce-163">例如，不需要其中 HTTPS 正在处理外部在边缘，在每个节点使用 HTTPS 某些后端服务。</span><span class="sxs-lookup"><span data-stu-id="fa5ce-163">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node is not needed.</span></span>

<span data-ttu-id="fa5ce-164">若要选择退出的 HTTPS:</span><span class="sxs-lookup"><span data-stu-id="fa5ce-164">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fa5ce-165">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fa5ce-165">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="fa5ce-166">取消选中**针对 HTTPS 配置**复选框。</span><span class="sxs-lookup"><span data-stu-id="fa5ce-166">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![实体关系图](enforcing-ssl/_static/out.png)


#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="fa5ce-168">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="fa5ce-168">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="fa5ce-169">使用 `--no-https` 选项。</span><span class="sxs-lookup"><span data-stu-id="fa5ce-169">Use the `--no-https` option.</span></span> <span data-ttu-id="fa5ce-170">例如</span><span class="sxs-lookup"><span data-stu-id="fa5ce-170">For example</span></span>

```cli
dotnet new razor --no-https
```

------

::: moniker-end