---
title: "URL 重写在 ASP.NET 核心中的中间件"
author: guardrex
description: "了解有关 URL 重写并将 URL 重写中间件与 ASP.NET Core 应用程序中的重定向信息。"
keywords: "ASP.NET 核心 URL 重写，URL 重写，重定向，URL 重定向，中间件，apache_mod URL"
ms.author: riande
manager: wpickett
ms.date: 08/17/2017
ms.topic: article
ms.assetid: e6130638-c410-4161-9921-b658ce988bd1
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/url-rewriting
ms.openlocfilehash: dde0b5673c9885db2fecbb24b384752e5ddf70eb
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/13/2017
---
# <a name="url-rewriting-middleware-in-aspnet-core"></a><span data-ttu-id="8256a-104">URL 重写在 ASP.NET 核心中的中间件</span><span class="sxs-lookup"><span data-stu-id="8256a-104">URL Rewriting Middleware in ASP.NET Core</span></span>

<span data-ttu-id="8256a-105">通过[Luke Latham](https://github.com/guardrex)和[Mikael Mengistu](https://github.com/mikaelm12)</span><span class="sxs-lookup"><span data-stu-id="8256a-105">By [Luke Latham](https://github.com/guardrex) and [Mikael Mengistu](https://github.com/mikaelm12)</span></span>

<span data-ttu-id="8256a-106">[查看或下载示例代码](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/)（[如何下载](xref:tutorials/index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="8256a-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="8256a-107">URL 重写是一种修改的请求 Url 基于一个或多个预定义的规则的行为。</span><span class="sxs-lookup"><span data-stu-id="8256a-107">URL rewriting is the act of modifying request URLs based on one or more predefined rules.</span></span> <span data-ttu-id="8256a-108">URL 重写创建资源位置和其地址之间的抽象，以便在位置和地址不紧密相关。</span><span class="sxs-lookup"><span data-stu-id="8256a-108">URL rewriting creates an abstraction between resource locations and their addresses so that the locations and addresses are not tightly linked.</span></span> <span data-ttu-id="8256a-109">有几种方案，其中 URL 重写是有价值：</span><span class="sxs-lookup"><span data-stu-id="8256a-109">There are several scenarios where URL rewriting is valuable:</span></span>
* <span data-ttu-id="8256a-110">移动或替换暂时或永久性地同时保持稳定的定位符，对这些资源的服务器资源</span><span class="sxs-lookup"><span data-stu-id="8256a-110">Moving or replacing server resources temporarily or permanently while maintaining stable locators for those resources</span></span>
* <span data-ttu-id="8256a-111">拆分请求处理跨不同的应用程序或跨区域的一个应用程序</span><span class="sxs-lookup"><span data-stu-id="8256a-111">Splitting request processing across different apps or across areas of one app</span></span>
* <span data-ttu-id="8256a-112">删除、 添加或重新组织对传入请求的 URL 段</span><span class="sxs-lookup"><span data-stu-id="8256a-112">Removing, adding, or reorganizing URL segments on incoming requests</span></span>
* <span data-ttu-id="8256a-113">优化搜索引擎搜索引擎优化 (SEO) 的公共 Url</span><span class="sxs-lookup"><span data-stu-id="8256a-113">Optimizing public URLs for Search Engine Optimization (SEO)</span></span>
* <span data-ttu-id="8256a-114">允许使用友好的公共 Url，以便帮助预测以下链接来找到的内容的用户</span><span class="sxs-lookup"><span data-stu-id="8256a-114">Permitting the use of friendly public URLs to help people predict the content they will find by following a link</span></span>
* <span data-ttu-id="8256a-115">将不安全来保护终结点的请求重定向</span><span class="sxs-lookup"><span data-stu-id="8256a-115">Redirecting insecure requests to secure endpoints</span></span>
* <span data-ttu-id="8256a-116">阻止映像 hotlinking</span><span class="sxs-lookup"><span data-stu-id="8256a-116">Preventing image hotlinking</span></span>

<span data-ttu-id="8256a-117">你可以定义了更改的 URL 中有多种，包括正则表达式、 Apache mod_rewrite 模块规则、 IIS 重写模块规则和使用自定义规则逻辑的规则。</span><span class="sxs-lookup"><span data-stu-id="8256a-117">You can define rules for changing the URL in several ways, including regex, Apache mod_rewrite module rules, IIS Rewrite Module rules, and using custom rule logic.</span></span> <span data-ttu-id="8256a-118">本文档介绍了 URL 重写包含有关如何在 ASP.NET Core 应用中使用 URL 重写中间件的说明。</span><span class="sxs-lookup"><span data-stu-id="8256a-118">This document introduces URL rewriting with instructions on how to use URL Rewriting Middleware in ASP.NET Core apps.</span></span>

> [!NOTE]
> <span data-ttu-id="8256a-119">URL 重写时，可以减少应用的性能。</span><span class="sxs-lookup"><span data-stu-id="8256a-119">URL rewriting can reduce the performance of an app.</span></span> <span data-ttu-id="8256a-120">如果可行，您应限制数量和复杂度的规则。</span><span class="sxs-lookup"><span data-stu-id="8256a-120">Where feasible, you should limit the number and complexity of rules.</span></span>

## <a name="url-redirect-and-url-rewrite"></a><span data-ttu-id="8256a-121">URL 重定向和 URL 重写</span><span class="sxs-lookup"><span data-stu-id="8256a-121">URL redirect and URL rewrite</span></span>
<span data-ttu-id="8256a-122">用词之间的差异*URL 重定向*和*URL 重写*可能看起来细微在第一个，但具有对资源提供给客户端的重要影响。</span><span class="sxs-lookup"><span data-stu-id="8256a-122">The difference in wording between *URL redirect* and *URL rewrite* may seem subtle at first but has important implications for providing resources to clients.</span></span> <span data-ttu-id="8256a-123">ASP.NET 核心 URL 重写中间件能够满足他们的两个需要。</span><span class="sxs-lookup"><span data-stu-id="8256a-123">ASP.NET Core's URL Rewriting Middleware is capable of meeting the need for both.</span></span>

<span data-ttu-id="8256a-124">A *URL 重定向*是客户端操作，其中将指示客户端访问在另一个地址的资源。</span><span class="sxs-lookup"><span data-stu-id="8256a-124">A *URL redirect* is a client-side operation, where the client is instructed to access a resource at another address.</span></span> <span data-ttu-id="8256a-125">这需要到服务器，一次往返行程和返回到客户端的重定向 URL 显示在浏览器的地址栏中，当客户端对资源的新请求。</span><span class="sxs-lookup"><span data-stu-id="8256a-125">This requires a round-trip to the server, and the redirect URL returned to the client appears in the browser's address bar when the client makes a new request for the resource.</span></span> <span data-ttu-id="8256a-126">如果`/resource`是*重定向*到`/different-resource`，客户端请求`/resource`，服务器做出响应客户端应获取处的资源`/different-resource`与一个状态代码，指示重定向临时或永久。</span><span class="sxs-lookup"><span data-stu-id="8256a-126">If `/resource` is *redirected* to `/different-resource`, the client requests `/resource`, and the server responds that the client should obtain the resource at `/different-resource` with a status code indicating that the redirect is either temporary or permanent.</span></span> <span data-ttu-id="8256a-127">客户端来执行重定向 URL 处的资源的新请求。</span><span class="sxs-lookup"><span data-stu-id="8256a-127">The client executes a new request for the resource at the redirect URL.</span></span>

![WebAPI 服务终结点已被暂时更改从版本 1 (v1) 到服务器上的版本 2 (v2)。](url-rewriting/_static/url_redirect.png)

<span data-ttu-id="8256a-133">将请求重定向到另一个 URL，你可以指示重定向是永久或临时。</span><span class="sxs-lookup"><span data-stu-id="8256a-133">When redirecting requests to a different URL, you indicate whether the redirect is permanent or temporary.</span></span> <span data-ttu-id="8256a-134">资源具有新的永久性 URL，其中你想使用 301 （永久移动） 状态代码，以指示客户端资源的所有将来的请求，应使用新的 URL。</span><span class="sxs-lookup"><span data-stu-id="8256a-134">The 301 (Moved Permanently) status code is used where the resource has a new, permanent URL and you wish to instruct the client that all future requests for the resource should use the new URL.</span></span> <span data-ttu-id="8256a-135">*收到一个 301 状态代码时，客户端可能缓存响应。*</span><span class="sxs-lookup"><span data-stu-id="8256a-135">*The client may cache the response when a 301 status code is received.*</span></span> <span data-ttu-id="8256a-136">302 （找到） 状态代码使用其中重定向是临时或通常使用者若要更改，以便客户端不应存储并在将来重复使用重定向 URL。</span><span class="sxs-lookup"><span data-stu-id="8256a-136">The 302 (Found) status code is used where the redirection is temporary or generally subject to change, such that the client shouldn't store and reuse the redirect URL in the future.</span></span> <span data-ttu-id="8256a-137">有关详细信息，请参阅[RFC 2616： 状态代码定义](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)。</span><span class="sxs-lookup"><span data-stu-id="8256a-137">For more information, see [RFC 2616: Status Code Definitions](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

<span data-ttu-id="8256a-138">A *URL 重写*是服务器端操作，以提供来自不同资源地址的资源。</span><span class="sxs-lookup"><span data-stu-id="8256a-138">A *URL rewrite* is a server-side operation to provide a resource from a different resource address.</span></span> <span data-ttu-id="8256a-139">重写 URL，则不需要保存/还原到服务器。</span><span class="sxs-lookup"><span data-stu-id="8256a-139">Rewriting a URL doesn't require a round-trip to the server.</span></span> <span data-ttu-id="8256a-140">重写的 URL 不返回到客户端，不会显示在浏览器的地址栏中。</span><span class="sxs-lookup"><span data-stu-id="8256a-140">The rewritten URL isn't returned to the client and won't appear in the browser's address bar.</span></span> <span data-ttu-id="8256a-141">当`/resource`是*重写*到`/different-resource`，客户端请求`/resource`，和服务器*内部*提取处的资源`/different-resource`。</span><span class="sxs-lookup"><span data-stu-id="8256a-141">When `/resource` is *rewritten* to `/different-resource`, the client requests `/resource`, and the server *internally* fetches the resource at `/different-resource`.</span></span> <span data-ttu-id="8256a-142">尽管客户端可能能够检索重写 URL 处的资源，则不会通知客户端在重写 URL 存在相应的资源时它将建立其请求并接收响应。</span><span class="sxs-lookup"><span data-stu-id="8256a-142">Although the client might be able to retrieve the resource at the rewritten URL, the client won't be informed that the resource exists at the rewritten URL when it makes its request and receives the response.</span></span>

![WebAPI 服务终结点已从版本 1 (v1) 更改为服务器上的版本 2 (v2)。](url-rewriting/_static/url_rewrite.png)

## <a name="url-rewriting-sample-app"></a><span data-ttu-id="8256a-147">URL 重写示例应用程序</span><span class="sxs-lookup"><span data-stu-id="8256a-147">URL rewriting sample app</span></span>
<span data-ttu-id="8256a-148">你可以浏览 URL 重写中间件中使用的功能[URL 重写示例应用程序](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/)。</span><span class="sxs-lookup"><span data-stu-id="8256a-148">You can explore the features of the URL Rewriting Middleware with the [URL rewriting sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/).</span></span> <span data-ttu-id="8256a-149">应用适用重写和重定向规则，并显示重写或重定向 URL。</span><span class="sxs-lookup"><span data-stu-id="8256a-149">The app applies rewrite and redirect rules and shows the rewritten or redirected URL.</span></span>

## <a name="when-to-use-url-rewriting-middleware"></a><span data-ttu-id="8256a-150">何时使用 URL 重写中间件</span><span class="sxs-lookup"><span data-stu-id="8256a-150">When to use URL Rewriting Middleware</span></span>
<span data-ttu-id="8256a-151">当你无法使用时，使用 URL 重写中间件[URL 重写模块](https://www.iis.net/downloads/microsoft/url-rewrite)与 Windows Server 上的 IIS [Apache mod_rewrite 模块](https://httpd.apache.org/docs/2.4/rewrite/)Apache 服务器上[URL 重写上 Nginx](https://www.nginx.com/blog/creating-nginx-rewrite-rules/)，或你的应用程序承载于[HTTP.sys 服务器](xref:fundamentals/servers/httpsys)(以前称为[WebListener](xref:fundamentals/servers/weblistener))。</span><span class="sxs-lookup"><span data-stu-id="8256a-151">Use URL Rewriting Middleware when you are unable to use the [URL Rewrite module](https://www.iis.net/downloads/microsoft/url-rewrite) with IIS on Windows Server, the [Apache mod_rewrite module](https://httpd.apache.org/docs/2.4/rewrite/) on Apache Server, [URL rewriting on Nginx](https://www.nginx.com/blog/creating-nginx-rewrite-rules/), or your app is hosted on [HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="8256a-152">使用基于服务器的 URL 重写中的技术 IIS、 Apache 或 Nginx 的主要原因是，中间件不支持这些模块的完整功能，但的中间件的性能可能不会与匹配的模块。</span><span class="sxs-lookup"><span data-stu-id="8256a-152">The main reasons to use the server-based URL rewriting technologies in IIS, Apache, or Nginx are that the middleware doesn't support the full features of these modules and the performance of the middleware probably won't match that of the modules.</span></span> <span data-ttu-id="8256a-153">但是，有不适用于 ASP.NET 核心项目，如服务器模块的某些功能`IsFile`和`IsDirectory`IIS 重写模块的约束。</span><span class="sxs-lookup"><span data-stu-id="8256a-153">However, there are some features of the server modules that don't work with ASP.NET Core projects, such as the `IsFile` and `IsDirectory` constraints of the IIS Rewrite module.</span></span> <span data-ttu-id="8256a-154">在这些情况下，改为使用该中间件。</span><span class="sxs-lookup"><span data-stu-id="8256a-154">In these scenarios, use the middleware instead.</span></span>

## <a name="package"></a><span data-ttu-id="8256a-155">Package</span><span class="sxs-lookup"><span data-stu-id="8256a-155">Package</span></span>
<span data-ttu-id="8256a-156">若要在你的项目中包含该中间件，添加到引用[ `Microsoft.AspNetCore.Rewrite` ](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/)包。</span><span class="sxs-lookup"><span data-stu-id="8256a-156">To include the middleware in your project, add a reference to the [`Microsoft.AspNetCore.Rewrite`](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/) package.</span></span> <span data-ttu-id="8256a-157">此功能是可用于应用程序面向 ASP.NET 核心 1.1 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="8256a-157">This feature is available for apps that target ASP.NET Core 1.1 or later.</span></span>

## <a name="extension-and-options"></a><span data-ttu-id="8256a-158">扩展和选项</span><span class="sxs-lookup"><span data-stu-id="8256a-158">Extension and options</span></span>
<span data-ttu-id="8256a-159">建立 URL 重写，并将规则重定向通过创建的实例`RewriteOptions`类使用的每个规则的扩展方法。</span><span class="sxs-lookup"><span data-stu-id="8256a-159">Establish your URL rewrite and redirect rules by creating an instance of the `RewriteOptions` class with extension methods for each of your rules.</span></span> <span data-ttu-id="8256a-160">链接你想要处理它们的顺序中的多个规则。</span><span class="sxs-lookup"><span data-stu-id="8256a-160">Chain multiple rules in the order that you would like them processed.</span></span> <span data-ttu-id="8256a-161">`RewriteOptions`如将其添加到请求管道与传递到该 URL 重写中间件`app.UseRewriter(options);`。</span><span class="sxs-lookup"><span data-stu-id="8256a-161">The `RewriteOptions` are passed into the URL Rewriting Middleware as it's added to the request pipeline with `app.UseRewriter(options);`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8256a-162">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8256a-162">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8256a-163">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8256a-163">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1)]

---

### <a name="url-redirect"></a><span data-ttu-id="8256a-164">URL 重定向</span><span class="sxs-lookup"><span data-stu-id="8256a-164">URL redirect</span></span>
<span data-ttu-id="8256a-165">使用`AddRedirect`将请求重定向。</span><span class="sxs-lookup"><span data-stu-id="8256a-165">Use `AddRedirect` to redirect requests.</span></span> <span data-ttu-id="8256a-166">第一个参数包含为匹配的路径中的传入 URL 你正则表达式。</span><span class="sxs-lookup"><span data-stu-id="8256a-166">The first parameter contains your regex for matching on the path of the incoming URL.</span></span> <span data-ttu-id="8256a-167">第二个参数是替换字符串。</span><span class="sxs-lookup"><span data-stu-id="8256a-167">The second parameter is the replacement string.</span></span> <span data-ttu-id="8256a-168">第三个参数，如果存在，则指定的状态代码。</span><span class="sxs-lookup"><span data-stu-id="8256a-168">The third parameter, if present, specifies the status code.</span></span> <span data-ttu-id="8256a-169">如果未指定的状态代码，则默认为 302 （找到），指示该资源暂时移动或替换。</span><span class="sxs-lookup"><span data-stu-id="8256a-169">If you don't specify the status code, it defaults to 302 (Found), which indicates that the resource is temporarily moved or replaced.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8256a-170">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8256a-170">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8256a-171">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8256a-171">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=2)]

---

<span data-ttu-id="8256a-172">在开发人员工具启用浏览器，对具有路径示例应用程序发出请求`/redirect-rule/1234/5678`。</span><span class="sxs-lookup"><span data-stu-id="8256a-172">In a browser with developer tools enabled, make a request to the sample app with the path `/redirect-rule/1234/5678`.</span></span> <span data-ttu-id="8256a-173">正则表达式匹配的请求路径上`redirect-rule/(.*)`，而路径被替换`/redirected/1234/5678`。</span><span class="sxs-lookup"><span data-stu-id="8256a-173">The regex matches the request path on `redirect-rule/(.*)`, and the path is replaced with `/redirected/1234/5678`.</span></span> <span data-ttu-id="8256a-174">重定向 URL 发送回客户端与 302 （找到） 状态代码。</span><span class="sxs-lookup"><span data-stu-id="8256a-174">The redirect URL is sent back to the client with a 302 (Found) status code.</span></span> <span data-ttu-id="8256a-175">浏览器在浏览器的地址栏中的重定向 URL 发出新请求。</span><span class="sxs-lookup"><span data-stu-id="8256a-175">The browser makes a new request at the redirect URL, which appears in the browser's address bar.</span></span> <span data-ttu-id="8256a-176">由于重定向 URL 上与不匹配的示例应用中的任何规则，第二次请求从应用程序接收响应 200 （正常） 和响应的正文显示重定向 URL。</span><span class="sxs-lookup"><span data-stu-id="8256a-176">Since no rules in the sample app match on the redirect URL, the second request receives a 200 (OK) response from the app and the body of the response shows the redirect URL.</span></span> <span data-ttu-id="8256a-177">URL 时，向服务器发出一次往返*重定向*。</span><span class="sxs-lookup"><span data-stu-id="8256a-177">A roundtrip is made to the server when a URL is *redirected*.</span></span>

> [!WARNING]
> <span data-ttu-id="8256a-178">建立你重定向规则时务必小心。</span><span class="sxs-lookup"><span data-stu-id="8256a-178">Be cautious when establishing your redirect rules.</span></span> <span data-ttu-id="8256a-179">对应用程序中，包括重定向后的每个请求，重定向规则进行评估。</span><span class="sxs-lookup"><span data-stu-id="8256a-179">Your redirect rules are evaluated on each request to the app, including after a redirect.</span></span> <span data-ttu-id="8256a-180">很容易意外创建的无限重定向循环。</span><span class="sxs-lookup"><span data-stu-id="8256a-180">It's easy to accidently create a loop of infinite redirects.</span></span>

<span data-ttu-id="8256a-181">原始请求：`/redirect-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="8256a-181">Original Request: `/redirect-rule/1234/5678`</span></span>

![使用跟踪的请求和响应的开发人员工具的浏览器窗口](url-rewriting/_static/add_redirect.png)

<span data-ttu-id="8256a-183">包含在括号内的表达式的一部分被称作*捕获组*。</span><span class="sxs-lookup"><span data-stu-id="8256a-183">The part of the expression contained within parentheses is called a *capture group*.</span></span> <span data-ttu-id="8256a-184">点 (`.`) 的表达式意味着*匹配任何字符*。</span><span class="sxs-lookup"><span data-stu-id="8256a-184">The dot (`.`) of the expression means *match any character*.</span></span> <span data-ttu-id="8256a-185">星号 (`*`) 指示*零次或多次匹配前面的字符*。</span><span class="sxs-lookup"><span data-stu-id="8256a-185">The asterisk (`*`) indicates *match the preceding character zero or more times*.</span></span> <span data-ttu-id="8256a-186">因此的 url 的最后两个路径段`1234/5678`，由捕获组捕获`(.*)`。</span><span class="sxs-lookup"><span data-stu-id="8256a-186">Therefore, the last two path segments of the URL, `1234/5678`, are captured by capture group `(.*)`.</span></span> <span data-ttu-id="8256a-187">在请求 URL 后提供任何值`redirect-rule/`此单个捕获组捕获。</span><span class="sxs-lookup"><span data-stu-id="8256a-187">Any value you provide in the request URL after `redirect-rule/` is captured by this single capture group.</span></span>

<span data-ttu-id="8256a-188">替换字符串中捕获的组注入到美元符号的字符串 (`$`) 跟捕获的序列号。</span><span class="sxs-lookup"><span data-stu-id="8256a-188">In the replacement string, captured groups are injected into the string with the dollar sign (`$`) followed by the sequence number of the capture.</span></span> <span data-ttu-id="8256a-189">第一个捕获组值一起被获取`$1`、 与第二个`$2`，并将不断序列中你正则表达式的捕获组中。</span><span class="sxs-lookup"><span data-stu-id="8256a-189">The first capture group value is obtained with `$1`, the second with `$2`, and they continue in sequence for the capture groups in your regex.</span></span> <span data-ttu-id="8256a-190">没有只有一个捕获的组中的重定向规则正则表达式在示例应用中，因此替换字符串，这是中只有一个插入的组`$1`。</span><span class="sxs-lookup"><span data-stu-id="8256a-190">There's only one captured group in the redirect rule regex in the sample app, so there's only one injected group in the replacement string, which is `$1`.</span></span> <span data-ttu-id="8256a-191">应用规则时，URL 将变为`/redirected/1234/5678`。</span><span class="sxs-lookup"><span data-stu-id="8256a-191">When the rule is applied, the URL becomes `/redirected/1234/5678`.</span></span>

<a name="url-redirect-to-secure-endpoint"></a>
### <a name="url-redirect-to-a-secure-endpoint"></a><span data-ttu-id="8256a-192">URL 重定向到安全的终结点</span><span class="sxs-lookup"><span data-stu-id="8256a-192">URL redirect to a secure endpoint</span></span>
<span data-ttu-id="8256a-193">使用`AddRedirectToHttps`将 HTTP 请求重定向到相同的主机，并使用 HTTPS 的路径 (`https://`)。</span><span class="sxs-lookup"><span data-stu-id="8256a-193">Use `AddRedirectToHttps` to redirect HTTP requests to the same host and path using HTTPS (`https://`).</span></span> <span data-ttu-id="8256a-194">如果未提供的状态代码，该中间件将默认为 302 （找到）。</span><span class="sxs-lookup"><span data-stu-id="8256a-194">If the status code isn't supplied, the middleware defaults to 302 (Found).</span></span> <span data-ttu-id="8256a-195">如果未提供端口，该中间件将默认为`null`，这意味着协议更改为`https://`和客户端访问端口 443 上的资源。</span><span class="sxs-lookup"><span data-stu-id="8256a-195">If the port isn't supplied, the middleware defaults to `null`, which means the protocol changes to `https://` and the client accesses the resource on port 443.</span></span> <span data-ttu-id="8256a-196">示例演示如何将状态代码设置为 301 （永久移动） 并将端口更改为 5001。</span><span class="sxs-lookup"><span data-stu-id="8256a-196">The example shows how to set the status code to 301 (Moved Permanently) and change the port to 5001.</span></span>
```csharp
var options = new RewriteOptions()
    .AddRedirectToHttps(301, 5001);

app.UseRewriter(options);
```
<span data-ttu-id="8256a-197">使用`AddRedirectToHttpsPermanent`将不安全的请求重定向到相同的主机，并使用安全 HTTPS 协议的路径 (`https://`端口 443 上)。</span><span class="sxs-lookup"><span data-stu-id="8256a-197">Use `AddRedirectToHttpsPermanent` to redirect insecure requests to the same host and path with secure HTTPS protocol (`https://` on port 443).</span></span> <span data-ttu-id="8256a-198">该中间件将状态代码设置为 301 （永久移动）。</span><span class="sxs-lookup"><span data-stu-id="8256a-198">The middleware sets the status code to 301 (Moved Permanently).</span></span>

<span data-ttu-id="8256a-199">示例应用程序均可演示如何使用`AddRedirectToHttps`或`AddRedirectToHttpsPermanent`。</span><span class="sxs-lookup"><span data-stu-id="8256a-199">The sample app is capable of demonstrating how to use `AddRedirectToHttps` or `AddRedirectToHttpsPermanent`.</span></span> <span data-ttu-id="8256a-200">添加扩展方法`RewriteOptions`。</span><span class="sxs-lookup"><span data-stu-id="8256a-200">Add the extension method to the `RewriteOptions`.</span></span> <span data-ttu-id="8256a-201">对任何 URL 时的应用进行的不安全的请求。</span><span class="sxs-lookup"><span data-stu-id="8256a-201">Make an insecure request to the app at any URL.</span></span> <span data-ttu-id="8256a-202">关闭浏览器安全警告自签名的证书不受信任。</span><span class="sxs-lookup"><span data-stu-id="8256a-202">Dismiss the browser security warning that the self-signed certificate is untrusted.</span></span>

<span data-ttu-id="8256a-203">使用原始请求`AddRedirectToHttps(301, 5001)`:`/secure`</span><span class="sxs-lookup"><span data-stu-id="8256a-203">Original Request using `AddRedirectToHttps(301, 5001)`: `/secure`</span></span>

![使用跟踪的请求和响应的开发人员工具的浏览器窗口](url-rewriting/_static/add_redirect_to_https.png)

<span data-ttu-id="8256a-205">使用原始请求`AddRedirectToHttpsPermanent`:`/secure`</span><span class="sxs-lookup"><span data-stu-id="8256a-205">Original Request using `AddRedirectToHttpsPermanent`: `/secure`</span></span>

![使用跟踪的请求和响应的开发人员工具的浏览器窗口](url-rewriting/_static/add_redirect_to_https_permanent.png)

### <a name="url-rewrite"></a><span data-ttu-id="8256a-207">URL 重写</span><span class="sxs-lookup"><span data-stu-id="8256a-207">URL rewrite</span></span>
<span data-ttu-id="8256a-208">使用`AddRewrite`创建用于 Url 重写规则。</span><span class="sxs-lookup"><span data-stu-id="8256a-208">Use `AddRewrite` to create a rules for rewriting URLs.</span></span> <span data-ttu-id="8256a-209">第一个参数将包含有关在传入的 URL 路径匹配你正则表达式。</span><span class="sxs-lookup"><span data-stu-id="8256a-209">The first parameter contains your regex for matching on the incoming URL path.</span></span> <span data-ttu-id="8256a-210">第二个参数是替换字符串。</span><span class="sxs-lookup"><span data-stu-id="8256a-210">The second parameter is the replacement string.</span></span> <span data-ttu-id="8256a-211">第三个参数， `skipRemainingRules: {true|false}`，是否要跳过其他重写规则，如果在应用的当前规则指示该中间件。</span><span class="sxs-lookup"><span data-stu-id="8256a-211">The third parameter, `skipRemainingRules: {true|false}`, indicates to the middleware whether or not to skip additional rewrite rules if the current rule is applied.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8256a-212">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8256a-212">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=6)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8256a-213">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8256a-213">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=3)]

---

<span data-ttu-id="8256a-214">原始请求：`/rewrite-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="8256a-214">Original Request: `/rewrite-rule/1234/5678`</span></span>

![与跟踪请求和响应的开发人员工具的浏览器窗口](url-rewriting/_static/add_rewrite.png)

<span data-ttu-id="8256a-216">你注意到正则表达式中的第一件事是插入 (`^`) 开头的表达式。</span><span class="sxs-lookup"><span data-stu-id="8256a-216">The first thing you notice in the regex is the carat (`^`) at the beginning of the expression.</span></span> <span data-ttu-id="8256a-217">这意味着，匹配的 URL 路径的开头开始。</span><span class="sxs-lookup"><span data-stu-id="8256a-217">This means that matching starts at the beginning of the URL path.</span></span>

<span data-ttu-id="8256a-218">重定向规则，与前面的示例中`redirect-rule/(.*)`，正则表达式的开头没有任何插入; 因此，任何字符可能优先于`redirect-rule/`中成功匹配的路径。</span><span class="sxs-lookup"><span data-stu-id="8256a-218">In the earlier example with the redirect rule, `redirect-rule/(.*)`, there's no carat at the start of the regex; therefore, any characters may precede `redirect-rule/` in the path for a successful match.</span></span>

| <span data-ttu-id="8256a-219">路径</span><span class="sxs-lookup"><span data-stu-id="8256a-219">Path</span></span>                               | <span data-ttu-id="8256a-220">匹配</span><span class="sxs-lookup"><span data-stu-id="8256a-220">Match</span></span> |
| ---------------------------------- | :---: |
| `/redirect-rule/1234/5678`         | <span data-ttu-id="8256a-221">是</span><span class="sxs-lookup"><span data-stu-id="8256a-221">Yes</span></span>   |
| `/my-cool-redirect-rule/1234/5678` | <span data-ttu-id="8256a-222">是</span><span class="sxs-lookup"><span data-stu-id="8256a-222">Yes</span></span>   |
| `/anotherredirect-rule/1234/5678`  | <span data-ttu-id="8256a-223">是</span><span class="sxs-lookup"><span data-stu-id="8256a-223">Yes</span></span>   |

<span data-ttu-id="8256a-224">重写规则， `^rewrite-rule/(\d+)/(\d+)`，仅在启动时出现匹配路径`rewrite-rule/`。</span><span class="sxs-lookup"><span data-stu-id="8256a-224">The rewrite rule, `^rewrite-rule/(\d+)/(\d+)`, only matches paths if they start with `rewrite-rule/`.</span></span> <span data-ttu-id="8256a-225">请注意匹配重写规则下面和上面的重定向规则之间的差异。</span><span class="sxs-lookup"><span data-stu-id="8256a-225">Notice the difference in matching between the rewrite rule below and the redirect rule above.</span></span>

| <span data-ttu-id="8256a-226">路径</span><span class="sxs-lookup"><span data-stu-id="8256a-226">Path</span></span>                              | <span data-ttu-id="8256a-227">匹配</span><span class="sxs-lookup"><span data-stu-id="8256a-227">Match</span></span> |
| --------------------------------- | :---: |
| `/rewrite-rule/1234/5678`         | <span data-ttu-id="8256a-228">是</span><span class="sxs-lookup"><span data-stu-id="8256a-228">Yes</span></span>   |
| `/my-cool-rewrite-rule/1234/5678` | <span data-ttu-id="8256a-229">No</span><span class="sxs-lookup"><span data-stu-id="8256a-229">No</span></span>    |
| `/anotherrewrite-rule/1234/5678`  | <span data-ttu-id="8256a-230">No</span><span class="sxs-lookup"><span data-stu-id="8256a-230">No</span></span>    |

<span data-ttu-id="8256a-231">以下`^rewrite-rule/`部分的表达式中，有两个捕获组， `(\d+)/(\d+)`。</span><span class="sxs-lookup"><span data-stu-id="8256a-231">Following the `^rewrite-rule/` portion of the expression, there are two capture groups, `(\d+)/(\d+)`.</span></span> <span data-ttu-id="8256a-232">`\d`表示*匹配数字 （数字）*。</span><span class="sxs-lookup"><span data-stu-id="8256a-232">The `\d` signifies *match a digit (number)*.</span></span> <span data-ttu-id="8256a-233">加号 (`+`) 意味着*匹配一个或多个前面的字符*。</span><span class="sxs-lookup"><span data-stu-id="8256a-233">The plus sign (`+`) means *match one or more of the preceding character*.</span></span> <span data-ttu-id="8256a-234">因此，该 URL 必须包含大量跟正斜杠跟另一个数字。</span><span class="sxs-lookup"><span data-stu-id="8256a-234">Therefore, the URL must contain a number followed by a forward-slash followed by another number.</span></span> <span data-ttu-id="8256a-235">组被注入到重写 URL 作为这些捕获`$1`和`$2`。</span><span class="sxs-lookup"><span data-stu-id="8256a-235">These capture groups are injected into the rewritten URL as `$1` and `$2`.</span></span> <span data-ttu-id="8256a-236">重写规则替换字符串将放在查询字符串的捕获的组。</span><span class="sxs-lookup"><span data-stu-id="8256a-236">The rewrite rule replacement string places the captured groups into the querystring.</span></span> <span data-ttu-id="8256a-237">所请求的路径的`/rewrite-rule/1234/5678`重新编写，以便获取处的资源`/rewritten?var1=1234&var2=5678`。</span><span class="sxs-lookup"><span data-stu-id="8256a-237">The requested path of `/rewrite-rule/1234/5678` is rewritten to obtain the resource at `/rewritten?var1=1234&var2=5678`.</span></span> <span data-ttu-id="8256a-238">如果出现在原始请求查询字符串，它被保留在 URL 重写时。</span><span class="sxs-lookup"><span data-stu-id="8256a-238">If a querystring is present on the original request, it's preserved when the URL is rewritten.</span></span>

<span data-ttu-id="8256a-239">不没有到服务器以获取资源的任何往返。</span><span class="sxs-lookup"><span data-stu-id="8256a-239">There's no roundtrip to the server to obtain the resource.</span></span> <span data-ttu-id="8256a-240">如果该资源已存在，它提取并返回到客户端与为 200 （正常） 的状态代码。</span><span class="sxs-lookup"><span data-stu-id="8256a-240">If the resource exists, it's fetched and returned to the client with a 200 (OK) status code.</span></span> <span data-ttu-id="8256a-241">因为客户端不重定向，则不会更改浏览器地址栏中的 URL。</span><span class="sxs-lookup"><span data-stu-id="8256a-241">Because the client isn't redirected, the URL in the browser address bar doesn't change.</span></span> <span data-ttu-id="8256a-242">就关心的是客户端，该 URL 重写操作永远不会出现。</span><span class="sxs-lookup"><span data-stu-id="8256a-242">As far as the client is concerned, the URL rewrite operation never occurred.</span></span>

> [!NOTE]
> <span data-ttu-id="8256a-243">使用`skipRemainingRules: true`只要有可能，因为匹配规则是代价高昂的过程并减少应用程序响应时间。</span><span class="sxs-lookup"><span data-stu-id="8256a-243">Use `skipRemainingRules: true` whenever possible, because matching rules is an expensive process and reduces app response time.</span></span> <span data-ttu-id="8256a-244">最快的应用程序响应：</span><span class="sxs-lookup"><span data-stu-id="8256a-244">For the fastest app response:</span></span>
> * <span data-ttu-id="8256a-245">订购到最不常匹配规则从最常匹配规则重写规则。</span><span class="sxs-lookup"><span data-stu-id="8256a-245">Order your rewrite rules from the most frequently matched rule to the least frequently matched rule.</span></span>
> * <span data-ttu-id="8256a-246">在出现匹配项和所需的任何其他规则处理时，跳过剩余的规则进行处理。</span><span class="sxs-lookup"><span data-stu-id="8256a-246">Skip the processing of the remaining rules when a match occurs and no additional rule processing is required.</span></span>

### <a name="apache-modrewrite"></a><span data-ttu-id="8256a-247">Apache mod_rewrite</span><span class="sxs-lookup"><span data-stu-id="8256a-247">Apache mod_rewrite</span></span>
<span data-ttu-id="8256a-248">应用与 Apache mod_rewrite 规则`AddApacheModRewrite`。</span><span class="sxs-lookup"><span data-stu-id="8256a-248">Apply Apache mod_rewrite rules with `AddApacheModRewrite`.</span></span> <span data-ttu-id="8256a-249">请确保规则将文件部署的应用程序。</span><span class="sxs-lookup"><span data-stu-id="8256a-249">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="8256a-250">有关详细信息和 mod_rewrite 规则的示例，请参阅[Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/)。</span><span class="sxs-lookup"><span data-stu-id="8256a-250">For more information and examples of mod_rewrite rules, see [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8256a-251">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8256a-251">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="8256a-252">A`StreamReader`用于读取从规则*ApacheModRewrite.txt*规则文件。</span><span class="sxs-lookup"><span data-stu-id="8256a-252">A `StreamReader` is used to read the rules from the *ApacheModRewrite.txt* rules file.</span></span>

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=1,7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8256a-253">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8256a-253">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="8256a-254">第一个参数的取值应`IFileProvider`，这可通过提供[依赖关系注入](dependency-injection.md)。</span><span class="sxs-lookup"><span data-stu-id="8256a-254">The first parameter takes an `IFileProvider`, which is provided via [Dependency Injection](dependency-injection.md).</span></span> <span data-ttu-id="8256a-255">`IHostingEnvironment`注入提供`ContentRootFileProvider`。</span><span class="sxs-lookup"><span data-stu-id="8256a-255">The `IHostingEnvironment` is injected to provide the `ContentRootFileProvider`.</span></span> <span data-ttu-id="8256a-256">第二个参数是你的规则文件，即路径*ApacheModRewrite.txt*示例应用中。</span><span class="sxs-lookup"><span data-stu-id="8256a-256">The second parameter is the path to your rules file, which is *ApacheModRewrite.txt* in the sample app.</span></span>

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=4)]

---

<span data-ttu-id="8256a-257">示例应用程序将从请求重定向`/apache-mod-rules-redirect/(.\*)`到`/redirected?id=$1`。</span><span class="sxs-lookup"><span data-stu-id="8256a-257">The sample app redirects requests from `/apache-mod-rules-redirect/(.\*)` to `/redirected?id=$1`.</span></span> <span data-ttu-id="8256a-258">响应状态代码为 302 （找到）。</span><span class="sxs-lookup"><span data-stu-id="8256a-258">The response status code is 302 (Found).</span></span>

[!code[Main](url-rewriting/samples/2.x/ApacheModRewrite.txt)]

<span data-ttu-id="8256a-259">原始请求：`/apache-mod-rules-redirect/1234`</span><span class="sxs-lookup"><span data-stu-id="8256a-259">Original Request: `/apache-mod-rules-redirect/1234`</span></span>

![使用跟踪的请求和响应的开发人员工具的浏览器窗口](url-rewriting/_static/add_apache_mod_redirect.png)

##### <a name="supported-server-variables"></a><span data-ttu-id="8256a-261">支持的服务器变量</span><span class="sxs-lookup"><span data-stu-id="8256a-261">Supported server variables</span></span>
<span data-ttu-id="8256a-262">该中间件支持以下 Apache mod_rewrite 服务器变量：</span><span class="sxs-lookup"><span data-stu-id="8256a-262">The middleware supports the following Apache mod_rewrite server variables:</span></span>
* <span data-ttu-id="8256a-263">CONN_REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="8256a-263">CONN_REMOTE_ADDR</span></span>
* <span data-ttu-id="8256a-264">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="8256a-264">HTTP_ACCEPT</span></span>
* <span data-ttu-id="8256a-265">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="8256a-265">HTTP_CONNECTION</span></span>
* <span data-ttu-id="8256a-266">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="8256a-266">HTTP_COOKIE</span></span>
* <span data-ttu-id="8256a-267">HTTP_FORWARDED</span><span class="sxs-lookup"><span data-stu-id="8256a-267">HTTP_FORWARDED</span></span>
* <span data-ttu-id="8256a-268">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="8256a-268">HTTP_HOST</span></span>
* <span data-ttu-id="8256a-269">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="8256a-269">HTTP_REFERER</span></span>
* <span data-ttu-id="8256a-270">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="8256a-270">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="8256a-271">HTTPS</span><span class="sxs-lookup"><span data-stu-id="8256a-271">HTTPS</span></span>
* <span data-ttu-id="8256a-272">IPV6</span><span class="sxs-lookup"><span data-stu-id="8256a-272">IPV6</span></span>
* <span data-ttu-id="8256a-273">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="8256a-273">QUERY_STRING</span></span>
* <span data-ttu-id="8256a-274">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="8256a-274">REMOTE_ADDR</span></span>
* <span data-ttu-id="8256a-275">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="8256a-275">REMOTE_PORT</span></span>
* <span data-ttu-id="8256a-276">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="8256a-276">REQUEST_FILENAME</span></span>
* <span data-ttu-id="8256a-277">REQUEST_METHOD</span><span class="sxs-lookup"><span data-stu-id="8256a-277">REQUEST_METHOD</span></span>
* <span data-ttu-id="8256a-278">REQUEST_SCHEME</span><span class="sxs-lookup"><span data-stu-id="8256a-278">REQUEST_SCHEME</span></span>
* <span data-ttu-id="8256a-279">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="8256a-279">REQUEST_URI</span></span>
* <span data-ttu-id="8256a-280">SCRIPT_FILENAME</span><span class="sxs-lookup"><span data-stu-id="8256a-280">SCRIPT_FILENAME</span></span>
* <span data-ttu-id="8256a-281">SERVER_ADDR</span><span class="sxs-lookup"><span data-stu-id="8256a-281">SERVER_ADDR</span></span>
* <span data-ttu-id="8256a-282">SERVER_PORT</span><span class="sxs-lookup"><span data-stu-id="8256a-282">SERVER_PORT</span></span>
* <span data-ttu-id="8256a-283">SERVER_PROTOCOL</span><span class="sxs-lookup"><span data-stu-id="8256a-283">SERVER_PROTOCOL</span></span>
* <span data-ttu-id="8256a-284">时间</span><span class="sxs-lookup"><span data-stu-id="8256a-284">TIME</span></span>
* <span data-ttu-id="8256a-285">TIME_DAY</span><span class="sxs-lookup"><span data-stu-id="8256a-285">TIME_DAY</span></span>
* <span data-ttu-id="8256a-286">TIME_HOUR</span><span class="sxs-lookup"><span data-stu-id="8256a-286">TIME_HOUR</span></span>
* <span data-ttu-id="8256a-287">TIME_MIN</span><span class="sxs-lookup"><span data-stu-id="8256a-287">TIME_MIN</span></span>
* <span data-ttu-id="8256a-288">TIME_MON</span><span class="sxs-lookup"><span data-stu-id="8256a-288">TIME_MON</span></span>
* <span data-ttu-id="8256a-289">TIME_SEC</span><span class="sxs-lookup"><span data-stu-id="8256a-289">TIME_SEC</span></span>
* <span data-ttu-id="8256a-290">TIME_WDAY</span><span class="sxs-lookup"><span data-stu-id="8256a-290">TIME_WDAY</span></span>
* <span data-ttu-id="8256a-291">TIME_YEAR</span><span class="sxs-lookup"><span data-stu-id="8256a-291">TIME_YEAR</span></span>

### <a name="iis-url-rewrite-module-rules"></a><span data-ttu-id="8256a-292">IIS URL 重写模块规则</span><span class="sxs-lookup"><span data-stu-id="8256a-292">IIS URL Rewrite Module rules</span></span>
<span data-ttu-id="8256a-293">若要使用适用于 IIS URL 重写模块的规则，请使用`AddIISUrlRewrite`。</span><span class="sxs-lookup"><span data-stu-id="8256a-293">To use rules that apply to the IIS URL Rewrite Module, use `AddIISUrlRewrite`.</span></span> <span data-ttu-id="8256a-294">请确保规则将文件部署的应用程序。</span><span class="sxs-lookup"><span data-stu-id="8256a-294">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="8256a-295">不直接使用的中间件你*web.config*文件在 Windows Server IIS 上运行时。</span><span class="sxs-lookup"><span data-stu-id="8256a-295">Don't direct the middleware to use your *web.config* file when running on Windows Server IIS.</span></span> <span data-ttu-id="8256a-296">使用 IIS，这些规则应存储之外你*web.config*以避免与 IIS 重写模块冲突。</span><span class="sxs-lookup"><span data-stu-id="8256a-296">With IIS, these rules should be stored outside of your *web.config* to avoid conflicts with the IIS Rewrite module.</span></span> <span data-ttu-id="8256a-297">有关详细信息和 IIS URL 重写模块规则的示例，请参阅[使用 Url 重写模块 2.0](https://docs.microsoft.com/iis/extensions/url-rewrite-module/using-url-rewrite-module-20)和[URL 重写模块配置引用](https://docs.microsoft.com/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)。</span><span class="sxs-lookup"><span data-stu-id="8256a-297">For more information and examples of IIS URL Rewrite Module rules, see [Using Url Rewrite Module 2.0](https://docs.microsoft.com/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) and [URL Rewrite Module Configuration Reference](https://docs.microsoft.com/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8256a-298">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8256a-298">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="8256a-299">A`StreamReader`用于读取从规则*IISUrlRewrite.xml*规则文件。</span><span class="sxs-lookup"><span data-stu-id="8256a-299">A `StreamReader` is used to read the rules from the *IISUrlRewrite.xml* rules file.</span></span>

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=2,8)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8256a-300">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8256a-300">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="8256a-301">第一个参数的取值应`IFileProvider`，而第二个参数是将 XML 规则文件，即路径*IISUrlRewrite.xml*示例应用中。</span><span class="sxs-lookup"><span data-stu-id="8256a-301">The first parameter takes an `IFileProvider`, while the second parameter is the path to your XML rules file, which is *IISUrlRewrite.xml* in the sample app.</span></span>

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=5)]

---

<span data-ttu-id="8256a-302">示例应用程序重写来自请求`/iis-rules-rewrite/(.*)`到`/rewritten?id=$1`。</span><span class="sxs-lookup"><span data-stu-id="8256a-302">The sample app rewrites requests from `/iis-rules-rewrite/(.*)` to `/rewritten?id=$1`.</span></span> <span data-ttu-id="8256a-303">将响应发送到客户端 200 （正常） 状态代码。</span><span class="sxs-lookup"><span data-stu-id="8256a-303">The response is sent to the client with a 200 (OK) status code.</span></span>

[!code-xml[Main](url-rewriting/samples/2.x/IISUrlRewrite.xml)]

<span data-ttu-id="8256a-304">原始请求：`/iis-rules-rewrite/1234`</span><span class="sxs-lookup"><span data-stu-id="8256a-304">Original Request: `/iis-rules-rewrite/1234`</span></span>

![与跟踪请求和响应的开发人员工具的浏览器窗口](url-rewriting/_static/add_iis_url_rewrite.png)

<span data-ttu-id="8256a-306">如果你使用的服务器级规则配置会以意外方式影响你的应用程序有一个 active IIS 重写模块，你可以禁用 IIS 重写模块，以使应用程序。</span><span class="sxs-lookup"><span data-stu-id="8256a-306">If you have an active IIS Rewrite Module with server-level rules configured that would impact your app in undesirable ways, you can disable the IIS Rewrite Module for an app.</span></span> <span data-ttu-id="8256a-307">有关详细信息，请参阅[禁用 IIS 模块](xref:hosting/iis-modules#disabling-iis-modules)。</span><span class="sxs-lookup"><span data-stu-id="8256a-307">For more information, see [Disabling IIS modules](xref:hosting/iis-modules#disabling-iis-modules).</span></span>

#### <a name="unsupported-features"></a><span data-ttu-id="8256a-308">不支持的功能</span><span class="sxs-lookup"><span data-stu-id="8256a-308">Unsupported features</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8256a-309">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8256a-309">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="8256a-310">发布的中间件与 ASP.NET 核心 2.x 不支持以下 IIS URL 重写模块功能：</span><span class="sxs-lookup"><span data-stu-id="8256a-310">The middleware released with ASP.NET Core 2.x doesn't support the following IIS URL Rewrite Module features:</span></span>
* <span data-ttu-id="8256a-311">出站规则</span><span class="sxs-lookup"><span data-stu-id="8256a-311">Outbound Rules</span></span>
* <span data-ttu-id="8256a-312">自定义服务器变量</span><span class="sxs-lookup"><span data-stu-id="8256a-312">Custom Server Variables</span></span>
* <span data-ttu-id="8256a-313">通配符</span><span class="sxs-lookup"><span data-stu-id="8256a-313">Wildcards</span></span>
* <span data-ttu-id="8256a-314">LogRewrittenUrl</span><span class="sxs-lookup"><span data-stu-id="8256a-314">LogRewrittenUrl</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8256a-315">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8256a-315">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="8256a-316">发布的中间件与 ASP.NET 核心 1.x 不支持以下 IIS URL 重写模块功能：</span><span class="sxs-lookup"><span data-stu-id="8256a-316">The middleware released with ASP.NET Core 1.x doesn't support the following IIS URL Rewrite Module features:</span></span>
* <span data-ttu-id="8256a-317">全局规则</span><span class="sxs-lookup"><span data-stu-id="8256a-317">Global Rules</span></span>
* <span data-ttu-id="8256a-318">出站规则</span><span class="sxs-lookup"><span data-stu-id="8256a-318">Outbound Rules</span></span>
* <span data-ttu-id="8256a-319">重写映射</span><span class="sxs-lookup"><span data-stu-id="8256a-319">Rewrite Maps</span></span>
* <span data-ttu-id="8256a-320">CustomResponse 操作</span><span class="sxs-lookup"><span data-stu-id="8256a-320">CustomResponse Action</span></span>
* <span data-ttu-id="8256a-321">自定义服务器变量</span><span class="sxs-lookup"><span data-stu-id="8256a-321">Custom Server Variables</span></span>
* <span data-ttu-id="8256a-322">通配符</span><span class="sxs-lookup"><span data-stu-id="8256a-322">Wildcards</span></span>
* <span data-ttu-id="8256a-323">操作： CustomResponse</span><span class="sxs-lookup"><span data-stu-id="8256a-323">Action:CustomResponse</span></span>
* <span data-ttu-id="8256a-324">LogRewrittenUrl</span><span class="sxs-lookup"><span data-stu-id="8256a-324">LogRewrittenUrl</span></span>

---

#### <a name="supported-server-variables"></a><span data-ttu-id="8256a-325">支持的服务器变量</span><span class="sxs-lookup"><span data-stu-id="8256a-325">Supported server variables</span></span>
<span data-ttu-id="8256a-326">该中间件支持以下 IIS URL 重写模块服务器变量：</span><span class="sxs-lookup"><span data-stu-id="8256a-326">The middleware supports the following IIS URL Rewrite Module server variables:</span></span>
* <span data-ttu-id="8256a-327">CONTENT_LENGTH</span><span class="sxs-lookup"><span data-stu-id="8256a-327">CONTENT_LENGTH</span></span>
* <span data-ttu-id="8256a-328">P E</span><span class="sxs-lookup"><span data-stu-id="8256a-328">CONTENT_TYPE</span></span>
* <span data-ttu-id="8256a-329">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="8256a-329">HTTP_ACCEPT</span></span>
* <span data-ttu-id="8256a-330">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="8256a-330">HTTP_CONNECTION</span></span>
* <span data-ttu-id="8256a-331">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="8256a-331">HTTP_COOKIE</span></span>
* <span data-ttu-id="8256a-332">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="8256a-332">HTTP_HOST</span></span>
* <span data-ttu-id="8256a-333">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="8256a-333">HTTP_REFERER</span></span>
* <span data-ttu-id="8256a-334">HTTP_URL</span><span class="sxs-lookup"><span data-stu-id="8256a-334">HTTP_URL</span></span>
* <span data-ttu-id="8256a-335">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="8256a-335">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="8256a-336">HTTPS</span><span class="sxs-lookup"><span data-stu-id="8256a-336">HTTPS</span></span>
* <span data-ttu-id="8256a-337">LOCAL_ADDR</span><span class="sxs-lookup"><span data-stu-id="8256a-337">LOCAL_ADDR</span></span>
* <span data-ttu-id="8256a-338">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="8256a-338">QUERY_STRING</span></span>
* <span data-ttu-id="8256a-339">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="8256a-339">REMOTE_ADDR</span></span>
* <span data-ttu-id="8256a-340">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="8256a-340">REMOTE_PORT</span></span>
* <span data-ttu-id="8256a-341">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="8256a-341">REQUEST_FILENAME</span></span>
* <span data-ttu-id="8256a-342">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="8256a-342">REQUEST_URI</span></span>

> [!NOTE]
> <span data-ttu-id="8256a-343">你还可以获取`IFileProvider`通过`PhysicalFileProvider`。</span><span class="sxs-lookup"><span data-stu-id="8256a-343">You can also obtain an `IFileProvider` via a `PhysicalFileProvider`.</span></span> <span data-ttu-id="8256a-344">此方法可能会提供更大的灵活性，你重写的位置规则文件。</span><span class="sxs-lookup"><span data-stu-id="8256a-344">This approach may provide greater flexibility for the location of your rewrite rules files.</span></span> <span data-ttu-id="8256a-345">请确保你重写规则文件部署到你提供的路径上的服务器。</span><span class="sxs-lookup"><span data-stu-id="8256a-345">Make sure that your rewrite rules files are deployed to the server at the path you provide.</span></span>
> ```csharp
> PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
> ```

### <a name="method-based-rule"></a><span data-ttu-id="8256a-346">基于方法的规则</span><span class="sxs-lookup"><span data-stu-id="8256a-346">Method-based rule</span></span>
<span data-ttu-id="8256a-347">使用`Add(Action<RewriteContext> applyRule)`实现您自己的规则逻辑的方法中。</span><span class="sxs-lookup"><span data-stu-id="8256a-347">Use `Add(Action<RewriteContext> applyRule)` to implement your own rule logic in a method.</span></span> <span data-ttu-id="8256a-348">`RewriteContext`公开`HttpContext`在你的方法中使用。</span><span class="sxs-lookup"><span data-stu-id="8256a-348">The `RewriteContext` exposes the `HttpContext` for use in your method.</span></span> <span data-ttu-id="8256a-349">`context.Result`确定其他管道处理。</span><span class="sxs-lookup"><span data-stu-id="8256a-349">The `context.Result` determines how additional pipeline processing is handled.</span></span>

| <span data-ttu-id="8256a-350">上下文。结果</span><span class="sxs-lookup"><span data-stu-id="8256a-350">context.Result</span></span>                       | <span data-ttu-id="8256a-351">操作</span><span class="sxs-lookup"><span data-stu-id="8256a-351">Action</span></span>                                                          |
| ------------------------------------ | --------------------------------------------------------------- |
| <span data-ttu-id="8256a-352">`RuleResult.ContinueRules`（默认值）</span><span class="sxs-lookup"><span data-stu-id="8256a-352">`RuleResult.ContinueRules` (default)</span></span> | <span data-ttu-id="8256a-353">继续应用规则</span><span class="sxs-lookup"><span data-stu-id="8256a-353">Continue applying rules</span></span>                                         |
| `RuleResult.EndResponse`             | <span data-ttu-id="8256a-354">停止应用规则和发送响应</span><span class="sxs-lookup"><span data-stu-id="8256a-354">Stop applying rules and send the response</span></span>                       |
| `RuleResult.SkipRemainingRules`      | <span data-ttu-id="8256a-355">停止应用规则并将上下文发送到下一步的中间件</span><span class="sxs-lookup"><span data-stu-id="8256a-355">Stop applying rules and send the context to the next middleware</span></span> |

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8256a-356">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8256a-356">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8256a-357">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8256a-357">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=6)]

---

<span data-ttu-id="8256a-358">示例应用演示的方法，将路径的请求重定向结束的*.xml*。</span><span class="sxs-lookup"><span data-stu-id="8256a-358">The sample app demonstrates a method that redirects requests for paths that end with *.xml*.</span></span> <span data-ttu-id="8256a-359">如果发出请求`/file.xml`，重定向到`/xmlfiles/file.xml`。</span><span class="sxs-lookup"><span data-stu-id="8256a-359">If you make a request for `/file.xml`, it's redirected to `/xmlfiles/file.xml`.</span></span> <span data-ttu-id="8256a-360">状态代码设置为 301 （永久移动）。</span><span class="sxs-lookup"><span data-stu-id="8256a-360">The status code is set to 301 (Moved Permanently).</span></span> <span data-ttu-id="8256a-361">有关重定向，你必须显式设置响应; 的状态代码否则为返回 200 （正常） 状态代码和重定向不会发生客户端上。</span><span class="sxs-lookup"><span data-stu-id="8256a-361">For a redirect, you must explicitly set the status code of the response; otherwise, a 200 (OK) status code is returned and the redirect won't occur on the client.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8256a-362">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8256a-362">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/RewriteRules.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8256a-363">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8256a-363">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet2)]

---

<span data-ttu-id="8256a-364">原始请求：`/file.xml`</span><span class="sxs-lookup"><span data-stu-id="8256a-364">Original Request: `/file.xml`</span></span>

![使用跟踪的请求和响应，则为 file.xml 的开发人员工具的浏览器窗口](url-rewriting/_static/add_redirect_xml_requests.png)

### <a name="irule-based-rule"></a><span data-ttu-id="8256a-366">基于 IRule 的规则</span><span class="sxs-lookup"><span data-stu-id="8256a-366">IRule-based rule</span></span>
<span data-ttu-id="8256a-367">使用`Add(IRule)`派生自的类中实现您自己的规则逻辑`IRule`。</span><span class="sxs-lookup"><span data-stu-id="8256a-367">Use `Add(IRule)` to implement your own rule logic in a class that derives from `IRule`.</span></span> <span data-ttu-id="8256a-368">使用`IRule`基础上使用基于方法的规则方法提供了更大的灵活性。</span><span class="sxs-lookup"><span data-stu-id="8256a-368">Using an `IRule` provides greater flexibility over using the method-based rule approach.</span></span> <span data-ttu-id="8256a-369">在派生的类可能包括构造函数，其中你可以传入的参数`ApplyRule`方法。</span><span class="sxs-lookup"><span data-stu-id="8256a-369">Your derived class may include a constructor, where you can pass in parameters for the `ApplyRule` method.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8256a-370">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8256a-370">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=10-11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8256a-371">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8256a-371">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=7-8)]

---

<span data-ttu-id="8256a-372">示例应用程序中的参数的值`extension`和`newPath`会检查，以满足几个条件。</span><span class="sxs-lookup"><span data-stu-id="8256a-372">The values of the parameters in the sample app for the `extension` and the `newPath` are checked to meet several conditions.</span></span> <span data-ttu-id="8256a-373">`extension`必须包含一个值，和的值必须为*.png*， *.jpg*，或*.gif*。</span><span class="sxs-lookup"><span data-stu-id="8256a-373">The `extension` must contain a value, and the value must be *.png*, *.jpg*, or *.gif*.</span></span> <span data-ttu-id="8256a-374">如果`newPath`无效，`ArgumentException`引发。</span><span class="sxs-lookup"><span data-stu-id="8256a-374">If the `newPath` isn't valid, an `ArgumentException` is thrown.</span></span> <span data-ttu-id="8256a-375">如果发出请求*image.png*，重定向到`/png-images/image.png`。</span><span class="sxs-lookup"><span data-stu-id="8256a-375">If you make a request for *image.png*, it's redirected to `/png-images/image.png`.</span></span> <span data-ttu-id="8256a-376">如果发出请求*image.jpg*，重定向到`/jpg-images/image.jpg`。</span><span class="sxs-lookup"><span data-stu-id="8256a-376">If you make a request for *image.jpg*, it's redirected to `/jpg-images/image.jpg`.</span></span> <span data-ttu-id="8256a-377">状态代码设置为 301 （永久移动），和`context.Result`设置为停止处理规则和发送响应。</span><span class="sxs-lookup"><span data-stu-id="8256a-377">The status code is set to 301 (Moved Permanently), and the `context.Result` is set to stop processing rules and send the response.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8256a-378">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8256a-378">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/RewriteRules.cs?name=snippet2)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8256a-379">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8256a-379">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/RewriteRule.cs?name=snippet1)]

---

<span data-ttu-id="8256a-380">原始请求：`/image.png`</span><span class="sxs-lookup"><span data-stu-id="8256a-380">Original Request: `/image.png`</span></span>

![使用跟踪的请求和响应，则为 image.png 的开发人员工具的浏览器窗口](url-rewriting/_static/add_redirect_png_requests.png)

<span data-ttu-id="8256a-382">原始请求：`/image.jpg`</span><span class="sxs-lookup"><span data-stu-id="8256a-382">Original Request: `/image.jpg`</span></span>

![使用跟踪的请求和响应，则为 image.jpg 的开发人员工具的浏览器窗口](url-rewriting/_static/add_redirect_jpg_requests.png)

## <a name="regex-examples"></a><span data-ttu-id="8256a-384">正则表达式示例</span><span class="sxs-lookup"><span data-stu-id="8256a-384">Regex examples</span></span>

| <span data-ttu-id="8256a-385">目标</span><span class="sxs-lookup"><span data-stu-id="8256a-385">Goal</span></span> | <span data-ttu-id="8256a-386">正则表达式字符串 （& a)</span><span class="sxs-lookup"><span data-stu-id="8256a-386">Regex String &</span></span><br><span data-ttu-id="8256a-387">匹配示例</span><span class="sxs-lookup"><span data-stu-id="8256a-387">Match Example</span></span> | <span data-ttu-id="8256a-388">替换字符串 （& a)</span><span class="sxs-lookup"><span data-stu-id="8256a-388">Replacement String &</span></span><br><span data-ttu-id="8256a-389">输出示例</span><span class="sxs-lookup"><span data-stu-id="8256a-389">Output Example</span></span> |
| ---- | :-----------------------------: | :------------------------------------: |
| <span data-ttu-id="8256a-390">重写到查询字符串的路径</span><span class="sxs-lookup"><span data-stu-id="8256a-390">Rewrite path into querystring</span></span> | `^path/(.*)/(.*)`<br>`/path/abc/123` | `path?var1=$1&var2=$2`<br>`/path?var1=abc&var2=123` |
| <span data-ttu-id="8256a-391">条带尾部反斜杠</span><span class="sxs-lookup"><span data-stu-id="8256a-391">Strip trailing slash</span></span> | `(.*)/$`<br>`/path/` | `$1`<br>`/path` |
| <span data-ttu-id="8256a-392">强制实施尾部反斜杠</span><span class="sxs-lookup"><span data-stu-id="8256a-392">Enforce trailing slash</span></span> | `(.*[^/])$`<br>`/path` | `$1/`<br>`/path/` |
| <span data-ttu-id="8256a-393">避免重写特定的请求</span><span class="sxs-lookup"><span data-stu-id="8256a-393">Avoid rewriting specific requests</span></span> | `(.*[^(\.axd)])$`<br><span data-ttu-id="8256a-394">是的：`/resource.htm`</span><span class="sxs-lookup"><span data-stu-id="8256a-394">Yes: `/resource.htm`</span></span><br><span data-ttu-id="8256a-395">不：`/resource.axd`</span><span class="sxs-lookup"><span data-stu-id="8256a-395">No: `/resource.axd`</span></span> | `rewritten/$1`<br>`/rewritten/resource.htm`<br>`/resource.axd` |
| <span data-ttu-id="8256a-396">重新排列 URL 段</span><span class="sxs-lookup"><span data-stu-id="8256a-396">Rearrange URL segments</span></span> | `path/(.*)/(.*)/(.*)`<br>`path/1/2/3` | `path/$3/$2/$1`<br>`path/3/2/1` |
| <span data-ttu-id="8256a-397">将 URL 段</span><span class="sxs-lookup"><span data-stu-id="8256a-397">Replace a URL segment</span></span> | `^(.*)/segment2/(.*)`<br>`/segment1/segment2/segment3` | `$1/replaced/$2`<br>`/segment1/replaced/segment3` |

## <a name="additional-resources"></a><span data-ttu-id="8256a-398">其他资源</span><span class="sxs-lookup"><span data-stu-id="8256a-398">Additional resources</span></span>
* [<span data-ttu-id="8256a-399">应用程序启动</span><span class="sxs-lookup"><span data-stu-id="8256a-399">Application Startup</span></span>](startup.md)
* [<span data-ttu-id="8256a-400">中间件</span><span class="sxs-lookup"><span data-stu-id="8256a-400">Middleware</span></span>](middleware.md)
* [<span data-ttu-id="8256a-401">.NET 中的正则表达式</span><span class="sxs-lookup"><span data-stu-id="8256a-401">Regular expressions in .NET</span></span>](/dotnet/articles/standard/base-types/regular-expressions)
* [<span data-ttu-id="8256a-402">正则表达式语言 - 快速参考</span><span class="sxs-lookup"><span data-stu-id="8256a-402">Regular expression language - quick reference</span></span>](/dotnet/articles/standard/base-types/quick-ref)
* [<span data-ttu-id="8256a-403">Apache mod_rewrite</span><span class="sxs-lookup"><span data-stu-id="8256a-403">Apache mod_rewrite</span></span>](https://httpd.apache.org/docs/2.4/rewrite/)
* [<span data-ttu-id="8256a-404">用于 Url 重写模块 2.0 （IIS)</span><span class="sxs-lookup"><span data-stu-id="8256a-404">Using Url Rewrite Module 2.0 (for IIS)</span></span>](https://docs.microsoft.com/iis/extensions/url-rewrite-module/using-url-rewrite-module-20)
* [<span data-ttu-id="8256a-405">URL 重写模块配置参考</span><span class="sxs-lookup"><span data-stu-id="8256a-405">URL Rewrite Module Configuration Reference</span></span>](https://docs.microsoft.com/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)
* [<span data-ttu-id="8256a-406">IIS URL 重写模块论坛</span><span class="sxs-lookup"><span data-stu-id="8256a-406">IIS URL Rewrite Module Forum</span></span>](https://forums.iis.net/1152.aspx)
* [<span data-ttu-id="8256a-407">保持简单 URL 结构</span><span class="sxs-lookup"><span data-stu-id="8256a-407">Keep a simple URL structure</span></span>](https://support.google.com/webmasters/answer/76329?hl=en)
* [<span data-ttu-id="8256a-408">10 URL 重写提示和技巧</span><span class="sxs-lookup"><span data-stu-id="8256a-408">10 URL Rewriting Tips and Tricks</span></span>](http://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/)
* [<span data-ttu-id="8256a-409">以正斜杠或不以斜杠</span><span class="sxs-lookup"><span data-stu-id="8256a-409">To slash or not to slash</span></span>](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html)
