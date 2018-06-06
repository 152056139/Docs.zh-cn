---
title: 响应缓存在 ASP.NET 核心
author: rick-anderson
description: 了解如何使用缓存到较低带宽要求的响应，并增加的 ASP.NET Core 应用的性能。
manager: wpickett
ms.author: riande
ms.date: 09/20/2017
ms.prod: asp.net-core
ms.topic: article
uid: performance/caching/response
ms.openlocfilehash: e5a3877c68f8475e7dd49d44f4a92cf7b09ac7f5
ms.sourcegitcommit: 726ffab258070b4fe6cf950bf030ce10c0c07bb4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2018
ms.locfileid: "34734505"
---
# <a name="response-caching-in-aspnet-core"></a><span data-ttu-id="3f7fe-103">响应缓存在 ASP.NET 核心</span><span class="sxs-lookup"><span data-stu-id="3f7fe-103">Response caching in ASP.NET Core</span></span>

<span data-ttu-id="3f7fe-104">通过[John Luo](https://github.com/JunTaoLuo)， [Rick Anderson](https://twitter.com/RickAndMSFT)， [Steve Smith](https://ardalis.com/)，和[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="3f7fe-104">By [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), and [Luke Latham](https://github.com/guardrex)</span></span>

> [!NOTE]
> <span data-ttu-id="3f7fe-105">响应缓存在 Razor 页中是位于 ASP.NET 核心 2.1 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-105">Response caching in Razor Pages is available in ASP.NET Core 2.1 or later.</span></span>

<span data-ttu-id="3f7fe-106">[查看或下载示例代码](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/samples)（[如何下载](xref:tutorials/index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="3f7fe-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="3f7fe-107">响应缓存可减少客户端或代理对 web 服务器的请求数。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-107">Response caching reduces the number of requests a client or proxy makes to a web server.</span></span> <span data-ttu-id="3f7fe-108">响应缓存还可减少量工作的 web 服务器执行程序生成响应。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-108">Response caching also reduces the amount of work the web server performs to generate a response.</span></span> <span data-ttu-id="3f7fe-109">响应缓存由标头，指定你希望客户端、 代理和缓存响应的中间件如何控制。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-109">Response caching is controlled by headers that specify how you want client, proxy, and middleware to cache responses.</span></span>

<span data-ttu-id="3f7fe-110">添加时，web 服务器可以缓存响应[响应缓存中间件](xref:performance/caching/middleware)。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-110">The web server can cache responses when you add [Response Caching Middleware](xref:performance/caching/middleware).</span></span>

## <a name="http-based-response-caching"></a><span data-ttu-id="3f7fe-111">基于 HTTP 的响应缓存</span><span class="sxs-lookup"><span data-stu-id="3f7fe-111">HTTP-based response caching</span></span>

<span data-ttu-id="3f7fe-112">[HTTP 1.1 缓存规范](https://tools.ietf.org/html/rfc7234)介绍 Internet 缓存的行为方式。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-112">The [HTTP 1.1 Caching specification](https://tools.ietf.org/html/rfc7234) describes how Internet caches should behave.</span></span> <span data-ttu-id="3f7fe-113">用于缓存的主 HTTP 标头是[缓存控制](https://tools.ietf.org/html/rfc7234#section-5.2)，用于指定缓存*指令*。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-113">The primary HTTP header used for caching is [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), which is used to specify cache *directives*.</span></span> <span data-ttu-id="3f7fe-114">指令控制缓存行为，请求来自客户端对服务器进行其方法和响应进行地从服务器返回到客户端。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-114">The directives control caching behavior as requests make their way from clients to servers and as reponses make their way from servers back to clients.</span></span> <span data-ttu-id="3f7fe-115">请求和响应将通过代理服务器和代理服务器还必须遵循 HTTP 1.1 缓存功能规范。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-115">Requests and responses move through proxy servers, and proxy servers must also conform to the HTTP 1.1 Caching specification.</span></span>

<span data-ttu-id="3f7fe-116">常见`Cache-Control`指令下表中所示。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-116">Common `Cache-Control` directives are shown in the following table.</span></span>

| <span data-ttu-id="3f7fe-117">指令</span><span class="sxs-lookup"><span data-stu-id="3f7fe-117">Directive</span></span>                                                       | <span data-ttu-id="3f7fe-118">操作</span><span class="sxs-lookup"><span data-stu-id="3f7fe-118">Action</span></span> |
| --------------------------------------------------------------- | ------ |
| [<span data-ttu-id="3f7fe-119">public</span><span class="sxs-lookup"><span data-stu-id="3f7fe-119">public</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)   | <span data-ttu-id="3f7fe-120">缓存可能会存储响应。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-120">A cache may store the response.</span></span> |
| [<span data-ttu-id="3f7fe-121">private</span><span class="sxs-lookup"><span data-stu-id="3f7fe-121">private</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)  | <span data-ttu-id="3f7fe-122">不能通过共享缓存中存储响应。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-122">The response must not be stored by a shared cache.</span></span> <span data-ttu-id="3f7fe-123">专用缓存可以存储并重复使用响应。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-123">A private cache may store and reuse the response.</span></span> |
| [<span data-ttu-id="3f7fe-124">max-age</span><span class="sxs-lookup"><span data-stu-id="3f7fe-124">max-age</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.1)  | <span data-ttu-id="3f7fe-125">客户端将不会接受其保留时间大于指定的秒数的响应。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-125">The client won't accept a response whose age is greater than the specified number of seconds.</span></span> <span data-ttu-id="3f7fe-126">示例： `max-age=60` （60 秒）， `max-age=2592000` （1 个月）</span><span class="sxs-lookup"><span data-stu-id="3f7fe-126">Examples: `max-age=60` (60 seconds), `max-age=2592000` (1 month)</span></span> |
| [<span data-ttu-id="3f7fe-127">no-cache</span><span class="sxs-lookup"><span data-stu-id="3f7fe-127">no-cache</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.4) | <span data-ttu-id="3f7fe-128">**在请求上**： 缓存不得使用存储的响应来满足该请求。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-128">**On requests**: A cache must not use a stored response to satisfy the request.</span></span> <span data-ttu-id="3f7fe-129">注意： 对于客户端，为源服务器将重新生成响应，并且该中间件更新其缓存中存储的响应。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-129">Note: The origin server re-generates the response for the client, and the middleware updates the stored response in its cache.</span></span><br><br><span data-ttu-id="3f7fe-130">**响应**： 响应必须不能用于在不验证源服务器上的后续请求。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-130">**On responses**: The response must not be used for a subsequent request without validation on the origin server.</span></span> |
| [<span data-ttu-id="3f7fe-131">no-store</span><span class="sxs-lookup"><span data-stu-id="3f7fe-131">no-store</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.5) | <span data-ttu-id="3f7fe-132">**在请求上**： 缓存必须不会将请求的存储。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-132">**On requests**: A cache must not store the request.</span></span><br><br><span data-ttu-id="3f7fe-133">**响应**： 缓存不得存储任何响应的一部分。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-133">**On responses**: A cache must not store any part of the response.</span></span> |

<span data-ttu-id="3f7fe-134">起缓存作用其他缓存标头下表所示。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-134">Other cache headers that play a role in caching are shown in the following table.</span></span>

| <span data-ttu-id="3f7fe-135">Header</span><span class="sxs-lookup"><span data-stu-id="3f7fe-135">Header</span></span>                                                     | <span data-ttu-id="3f7fe-136">函数</span><span class="sxs-lookup"><span data-stu-id="3f7fe-136">Function</span></span> |
| ---------------------------------------------------------- | -------- |
| [<span data-ttu-id="3f7fe-137">保留时间</span><span class="sxs-lookup"><span data-stu-id="3f7fe-137">Age</span></span>](https://tools.ietf.org/html/rfc7234#section-5.1)     | <span data-ttu-id="3f7fe-138">以秒为单位由于生成或者在源服务器已成功验证响应的时间量的估计值。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-138">An estimate of the amount of time in seconds since the response was generated or successfully validated at the origin server.</span></span> |
| [<span data-ttu-id="3f7fe-139">过期</span><span class="sxs-lookup"><span data-stu-id="3f7fe-139">Expires</span></span>](https://tools.ietf.org/html/rfc7234#section-5.3) | <span data-ttu-id="3f7fe-140">日期/时间后响应被视为是陈旧。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-140">The date/time after which the response is considered stale.</span></span> |
| [<span data-ttu-id="3f7fe-141">杂注</span><span class="sxs-lookup"><span data-stu-id="3f7fe-141">Pragma</span></span>](https://tools.ietf.org/html/rfc7234#section-5.4)  | <span data-ttu-id="3f7fe-142">存在向后兼容性与 HTTP/1.0 缓存设置`no-cache`行为。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-142">Exists for backwards compatibility with HTTP/1.0 caches for setting `no-cache` behavior.</span></span> <span data-ttu-id="3f7fe-143">如果`Cache-Control`标头是否存在、`Pragma`标头将被忽略。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-143">If the `Cache-Control` header is present, the `Pragma` header is ignored.</span></span> |
| [<span data-ttu-id="3f7fe-144">改变</span><span class="sxs-lookup"><span data-stu-id="3f7fe-144">Vary</span></span>](https://tools.ietf.org/html/rfc7231#section-7.1.4)  | <span data-ttu-id="3f7fe-145">指定缓存的响应必须不发送除非所有的`Vary`中缓存的响应的原始请求和新的请求标头字段所匹配。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-145">Specifies that a cached response must not be sent unless all of the `Vary` header fields match in both the cached response's original request and the new request.</span></span> |

## <a name="http-based-caching-respects-request-cache-control-directives"></a><span data-ttu-id="3f7fe-146">基于 HTTP 的缓存方面请求的缓存控制指令</span><span class="sxs-lookup"><span data-stu-id="3f7fe-146">HTTP-based caching respects request Cache-Control directives</span></span>

<span data-ttu-id="3f7fe-147">[HTTP 1.1 缓存的缓存控制标头的规范](https://tools.ietf.org/html/rfc7234#section-5.2)需要遵守是有效的缓存`Cache-Control`客户端发送的标头。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-147">The [HTTP 1.1 Caching specification for the Cache-Control header](https://tools.ietf.org/html/rfc7234#section-5.2) requires a cache to honor a valid `Cache-Control` header sent by the client.</span></span> <span data-ttu-id="3f7fe-148">客户端可以发出请求与`no-cache`标头值和强制服务器生成的每个请求的新响应。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-148">A client can make requests with a `no-cache` header value and force the server to generate a new response for every request.</span></span>

<span data-ttu-id="3f7fe-149">始终考虑客户端`Cache-Control`请求标头是有意义，如果你考虑 HTTP 缓存功能的目标。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-149">Always honoring client `Cache-Control` request headers makes sense if you consider the goal of HTTP caching.</span></span> <span data-ttu-id="3f7fe-150">官方规范中，在缓存旨在减少在网络中客户端、 代理和服务器满足请求的延迟和网络的开销。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-150">Under the official specification, caching is meant to reduce the latency and network overhead of satisfying requests across a network of clients, proxies, and servers.</span></span> <span data-ttu-id="3f7fe-151">它不一定是一种方法来控制的源服务器上的负载。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-151">It isn't necessarily a way to control the load on an origin server.</span></span>

<span data-ttu-id="3f7fe-152">没有对此缓存行为的当前开发人员控件时使用[响应缓存中间件](xref:performance/caching/middleware)因为缓存规范正式遵循该中间件。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-152">There's no current developer control over this caching behavior when using the [Response Caching Middleware](xref:performance/caching/middleware) because the middleware adheres to the official caching specification.</span></span> <span data-ttu-id="3f7fe-153">[将来对该中间件增强](https://github.com/aspnet/ResponseCaching/issues/96)配置中间件，若要忽略的请求将允许`Cache-Control`时决定提供缓存的响应的标头。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-153">[Future enhancements to the middleware](https://github.com/aspnet/ResponseCaching/issues/96) will permit configuring the middleware to ignore a request's `Cache-Control` header when deciding to serve a cached response.</span></span> <span data-ttu-id="3f7fe-154">当你使用该中间件，这将为您提供更好地控制负载在你的服务器上的机会。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-154">This will offer you an opportunity to better control the load on your server when you use the middleware.</span></span>

## <a name="other-caching-technology-in-aspnet-core"></a><span data-ttu-id="3f7fe-155">在 ASP.NET 核心中其他缓存技术</span><span class="sxs-lookup"><span data-stu-id="3f7fe-155">Other caching technology in ASP.NET Core</span></span>

### <a name="in-memory-caching"></a><span data-ttu-id="3f7fe-156">内存中缓存</span><span class="sxs-lookup"><span data-stu-id="3f7fe-156">In-memory caching</span></span>

<span data-ttu-id="3f7fe-157">内存中缓存使用的服务器内存来存储缓存的数据。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-157">In-memory caching uses server memory to store cached data.</span></span> <span data-ttu-id="3f7fe-158">此类型的缓存适合于单个服务器或多个服务器使用*粘性会话*。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-158">This type of caching is suitable for a single server or multiple servers using *sticky sessions*.</span></span> <span data-ttu-id="3f7fe-159">粘性会话意味着，客户端的请求始终路由至同一服务器进行处理。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-159">Sticky sessions means that the requests from a client are always routed to the same server for processing.</span></span>

<span data-ttu-id="3f7fe-160">有关详细信息，请参阅[缓存内存中](xref:performance/caching/memory)。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-160">For more information, see [Cache in-memory](xref:performance/caching/memory).</span></span>

### <a name="distributed-cache"></a><span data-ttu-id="3f7fe-161">分布式的缓存</span><span class="sxs-lookup"><span data-stu-id="3f7fe-161">Distributed Cache</span></span>

<span data-ttu-id="3f7fe-162">使用分布式的缓存时云或服务器场中承载应用程序数据存储在内存中。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-162">Use a distributed cache to store data in memory when the app is hosted in a cloud or server farm.</span></span> <span data-ttu-id="3f7fe-163">处理请求的服务器之间共享缓存。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-163">The cache is shared across the servers that process requests.</span></span> <span data-ttu-id="3f7fe-164">客户端可以提交用于客户端的缓存的数据是否可用的组中的任何服务器处理的请求。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-164">A client can submit a request that's handled by any server in the group if cached data for the client is available.</span></span> <span data-ttu-id="3f7fe-165">ASP.NET Core 提供 SQL Server 和分布式的 Redis 缓存。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-165">ASP.NET Core offers SQL Server and Redis distributed caches.</span></span>

<span data-ttu-id="3f7fe-166">有关详细信息，请参阅[方式使用分布式缓存](xref:performance/caching/distributed)。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-166">For more information, see [Work with a distributed cache](xref:performance/caching/distributed).</span></span>

### <a name="cache-tag-helper"></a><span data-ttu-id="3f7fe-167">缓存标记帮助器</span><span class="sxs-lookup"><span data-stu-id="3f7fe-167">Cache Tag Helper</span></span>

<span data-ttu-id="3f7fe-168">你可以使用缓存标记帮助器缓存中的 MVC 视图或 Razor 页的内容。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-168">You can cache the content from an MVC view or Razor Page with the Cache Tag Helper.</span></span> <span data-ttu-id="3f7fe-169">缓存标记帮助器使用的内存中缓存来存储数据。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-169">The Cache Tag Helper uses in-memory caching to store data.</span></span>

<span data-ttu-id="3f7fe-170">有关详细信息，请参阅[ASP.NET 核心 mvc 缓存标记帮助器](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-170">For more information, see [Cache Tag Helper in ASP.NET Core MVC](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span></span>

### <a name="distributed-cache-tag-helper"></a><span data-ttu-id="3f7fe-171">分布式缓存标记帮助程序</span><span class="sxs-lookup"><span data-stu-id="3f7fe-171">Distributed Cache Tag Helper</span></span>

<span data-ttu-id="3f7fe-172">你可以使用分布式缓存标记帮助程序缓存的 MVC 视图或分布式的云或 web 场方案中的 Razor 页面中的内容。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-172">You can cache the content from an MVC view or Razor Page in distributed cloud or web farm scenarios with the Distributed Cache Tag Helper.</span></span> <span data-ttu-id="3f7fe-173">分布式缓存标记帮助器使用 SQL Server 或 Redis 来存储数据。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-173">The Distributed Cache Tag Helper uses SQL Server or Redis to store data.</span></span>

<span data-ttu-id="3f7fe-174">有关详细信息，请参阅[分布式缓存标记帮助器](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-174">For more information, see [Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper).</span></span>

## <a name="responsecache-attribute"></a><span data-ttu-id="3f7fe-175">ResponseCache 属性</span><span class="sxs-lookup"><span data-stu-id="3f7fe-175">ResponseCache attribute</span></span>

<span data-ttu-id="3f7fe-176">[ResponseCacheAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.ResponseCacheAttribute)指定在缓存中，响应设置适当的标头的所需的参数。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-176">The [ResponseCacheAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) specifies the parameters necessary for setting appropriate headers in response caching.</span></span>

> [!WARNING]
> <span data-ttu-id="3f7fe-177">禁用缓存的内容，其中包含已经过身份验证的客户端的信息。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-177">Disable caching for content that contains information for authenticated clients.</span></span> <span data-ttu-id="3f7fe-178">仅应为不会更改基于用户的标识或用户已登录的内容启用缓存。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-178">Caching should only be enabled for content that doesn't change based on a user's identity or whether a user is signed in.</span></span>

<span data-ttu-id="3f7fe-179">[VaryByQueryKeys](/dotnet/api/microsoft.aspnetcore.mvc.responsecacheattribute.varybyquerykeys)存储的响应因给定的查询密钥列表的值。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-179">[VaryByQueryKeys](/dotnet/api/microsoft.aspnetcore.mvc.responsecacheattribute.varybyquerykeys) varies the stored response by the values of the given list of query keys.</span></span> <span data-ttu-id="3f7fe-180">时的单个值`*`是响应的所有请求查询字符串参数提供该中间件而异。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-180">When a single value of `*` is provided, the middleware varies responses by all request query string parameters.</span></span> <span data-ttu-id="3f7fe-181">`VaryByQueryKeys` 需要 ASP.NET Core 1.1 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-181">`VaryByQueryKeys` requires ASP.NET Core 1.1 or later.</span></span>

<span data-ttu-id="3f7fe-182">必须启用响应缓存中间件设置`VaryByQueryKeys`属性; 否则，会引发一个运行时异常。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-182">The Response Caching Middleware must be enabled to set the `VaryByQueryKeys` property; otherwise, a runtime exception is thrown.</span></span> <span data-ttu-id="3f7fe-183">没有为相应的 HTTP 标头`VaryByQueryKeys`属性。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-183">There isn't a corresponding HTTP header for the `VaryByQueryKeys` property.</span></span> <span data-ttu-id="3f7fe-184">该属性是一项 HTTP 功能由响应缓存中间件。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-184">The property is an HTTP feature handled by the Response Caching Middleware.</span></span> <span data-ttu-id="3f7fe-185">若要提供缓存的响应的中间件，对于查询字符串和查询字符串值必须匹配上一个请求。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-185">For the middleware to serve a cached response, the query string and query string value must match a previous request.</span></span> <span data-ttu-id="3f7fe-186">例如，考虑的请求和下表中显示结果的序列。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-186">For example, consider the sequence of requests and results shown in the following table.</span></span>

| <span data-ttu-id="3f7fe-187">请求</span><span class="sxs-lookup"><span data-stu-id="3f7fe-187">Request</span></span>                          | <span data-ttu-id="3f7fe-188">结果</span><span class="sxs-lookup"><span data-stu-id="3f7fe-188">Result</span></span>                   |
| -------------------------------- | ------------------------ |
| `http://example.com?key1=value1` | <span data-ttu-id="3f7fe-189">从服务器返回</span><span class="sxs-lookup"><span data-stu-id="3f7fe-189">Returned from server</span></span>     |
| `http://example.com?key1=value1` | <span data-ttu-id="3f7fe-190">返回从中间件</span><span class="sxs-lookup"><span data-stu-id="3f7fe-190">Returned from middleware</span></span> |
| `http://example.com?key1=value2` | <span data-ttu-id="3f7fe-191">从服务器返回</span><span class="sxs-lookup"><span data-stu-id="3f7fe-191">Returned from server</span></span>     |

<span data-ttu-id="3f7fe-192">第一个请求是由服务器返回，并在中间件中缓冲。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-192">The first request is returned by the server and cached in middleware.</span></span> <span data-ttu-id="3f7fe-193">由于查询字符串与上一个请求，中间件会返回第二个请求。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-193">The second request is returned by middleware because the query string matches the previous request.</span></span> <span data-ttu-id="3f7fe-194">因为查询字符串值不匹配的上一个请求，第三个请求未处于中间件缓存。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-194">The third request isn't in the middleware cache because the query string value doesn't match a previous request.</span></span> 

<span data-ttu-id="3f7fe-195">`ResponseCacheAttribute`用于配置和创建 (通过`IFilterFactory`) [ResponseCacheFilter](/dotnet/api/microsoft.aspnetcore.mvc.internal.responsecachefilter)。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-195">The `ResponseCacheAttribute` is used to configure and create (via `IFilterFactory`) a [ResponseCacheFilter](/dotnet/api/microsoft.aspnetcore.mvc.internal.responsecachefilter).</span></span> <span data-ttu-id="3f7fe-196">`ResponseCacheFilter`执行的工作的更新的相应 HTTP 标头和响应功能。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-196">The `ResponseCacheFilter` performs the work of updating the appropriate HTTP headers and features of the response.</span></span> <span data-ttu-id="3f7fe-197">筛选器：</span><span class="sxs-lookup"><span data-stu-id="3f7fe-197">The filter:</span></span>

* <span data-ttu-id="3f7fe-198">删除任何现有标头`Vary`， `Cache-Control`，和`Pragma`。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-198">Removes any existing headers for `Vary`, `Cache-Control`, and `Pragma`.</span></span> 
* <span data-ttu-id="3f7fe-199">写出相应的标头设置的属性上基于`ResponseCacheAttribute`。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-199">Writes out the appropriate headers based on the properties set in the `ResponseCacheAttribute`.</span></span> 
* <span data-ttu-id="3f7fe-200">更新缓存中项 HTTP 功能，如果响应`VaryByQueryKeys`设置。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-200">Updates the response caching HTTP feature if `VaryByQueryKeys` is set.</span></span>

### <a name="vary"></a><span data-ttu-id="3f7fe-201">改变</span><span class="sxs-lookup"><span data-stu-id="3f7fe-201">Vary</span></span>

<span data-ttu-id="3f7fe-202">此标头只会写入时`VaryByHeader`属性设置。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-202">This header is only written when the `VaryByHeader` property is set.</span></span> <span data-ttu-id="3f7fe-203">设置为`Vary`属性的值。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-203">It's set to the `Vary` property's value.</span></span> <span data-ttu-id="3f7fe-204">下面的示例使用`VaryByHeader`属性：</span><span class="sxs-lookup"><span data-stu-id="3f7fe-204">The following sample uses the `VaryByHeader` property:</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="3f7fe-205">[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]</span><span class="sxs-lookup"><span data-stu-id="3f7fe-205">[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="3f7fe-206">[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]</span><span class="sxs-lookup"><span data-stu-id="3f7fe-206">[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]</span></span>

::: moniker-end

<span data-ttu-id="3f7fe-207">你可以查看你的浏览器的网络工具的响应标头。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-207">You can view the response headers with your browser's network tools.</span></span> <span data-ttu-id="3f7fe-208">下图显示了输出上边缘 F12**网络**选项卡上时`About2`刷新操作方法：</span><span class="sxs-lookup"><span data-stu-id="3f7fe-208">The following image shows the Edge F12 output on the **Network** tab when the `About2` action method is refreshed:</span></span>

![在网络选项卡上边缘 F12 输出，当调用 About2 操作方法](response/_static/vary.png)

### <a name="nostore-and-locationnone"></a><span data-ttu-id="3f7fe-210">NoStore 和 Location.None</span><span class="sxs-lookup"><span data-stu-id="3f7fe-210">NoStore and Location.None</span></span>

<span data-ttu-id="3f7fe-211">`NoStore` 重写的大多数其他属性。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-211">`NoStore` overrides most of the other properties.</span></span> <span data-ttu-id="3f7fe-212">当此属性设置为`true`、`Cache-Control`标头设置为`no-store`。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-212">When this property is set to `true`, the `Cache-Control` header is set to `no-store`.</span></span> <span data-ttu-id="3f7fe-213">如果`Location`设置为`None`:</span><span class="sxs-lookup"><span data-stu-id="3f7fe-213">If `Location` is set to `None`:</span></span>

* <span data-ttu-id="3f7fe-214">将 `Cache-Control` 设置为 `no-store,no-cache`。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-214">`Cache-Control` is set to `no-store,no-cache`.</span></span>
* <span data-ttu-id="3f7fe-215">将 `Pragma` 设置为 `no-cache`。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-215">`Pragma` is set to `no-cache`.</span></span>

<span data-ttu-id="3f7fe-216">如果`NoStore`是`false`和`Location`是`None`，`Cache-Control`和`Pragma`设置为`no-cache`。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-216">If `NoStore` is `false` and `Location` is `None`, `Cache-Control` and `Pragma` are set to `no-cache`.</span></span>

<span data-ttu-id="3f7fe-217">通常情况下设置`NoStore`到`true`错误页上。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-217">You typically set `NoStore` to `true` on error pages.</span></span> <span data-ttu-id="3f7fe-218">例如：</span><span class="sxs-lookup"><span data-stu-id="3f7fe-218">For example:</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="3f7fe-219">[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]</span><span class="sxs-lookup"><span data-stu-id="3f7fe-219">[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="3f7fe-220">[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]</span><span class="sxs-lookup"><span data-stu-id="3f7fe-220">[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]</span></span>

::: moniker-end

<span data-ttu-id="3f7fe-221">这将导致以下标头：</span><span class="sxs-lookup"><span data-stu-id="3f7fe-221">This results in the following headers:</span></span>

```
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a><span data-ttu-id="3f7fe-222">位置和持续时间</span><span class="sxs-lookup"><span data-stu-id="3f7fe-222">Location and Duration</span></span>

<span data-ttu-id="3f7fe-223">若要启用缓存，`Duration`必须设置为正值和`Location`必须是`Any`（默认值） 或`Client`。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-223">To enable caching, `Duration` must be set to a positive value and `Location` must be either `Any` (the default) or `Client`.</span></span> <span data-ttu-id="3f7fe-224">在这种情况下，`Cache-Control`标头设置为位置值后跟`max-age`的响应。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-224">In this case, the `Cache-Control` header is set to the location value followed by the `max-age` of the response.</span></span>

> [!NOTE]
> <span data-ttu-id="3f7fe-225">`Location`选项的`Any`和`Client`将转换`Cache-Control`的标头值`public`和`private`分别。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-225">`Location`'s options of `Any` and `Client` translate into `Cache-Control` header values of `public` and `private`, respectively.</span></span> <span data-ttu-id="3f7fe-226">按前面所述，设置`Location`到`None`设置同时`Cache-Control`和`Pragma`标头到`no-cache`。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-226">As noted previously, setting `Location` to `None` sets both `Cache-Control` and `Pragma` headers to `no-cache`.</span></span>

<span data-ttu-id="3f7fe-227">下面生成通过设置一个示例，演示标头`Duration`并保留默认值`Location`值：</span><span class="sxs-lookup"><span data-stu-id="3f7fe-227">Below is an example showing the headers produced by setting `Duration` and leaving the default `Location` value:</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="3f7fe-228">[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]</span><span class="sxs-lookup"><span data-stu-id="3f7fe-228">[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="3f7fe-229">[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]</span><span class="sxs-lookup"><span data-stu-id="3f7fe-229">[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]</span></span>

::: moniker-end

<span data-ttu-id="3f7fe-230">这将产生以下标头：</span><span class="sxs-lookup"><span data-stu-id="3f7fe-230">This produces the following header:</span></span>

```
Cache-Control: public,max-age=60
```

### <a name="cache-profiles"></a><span data-ttu-id="3f7fe-231">缓存配置文件</span><span class="sxs-lookup"><span data-stu-id="3f7fe-231">Cache profiles</span></span>

<span data-ttu-id="3f7fe-232">而不是复制`ResponseCache`设置 MVC 中时，可以将许多控制器操作属性而言，缓存配置文件上的设置配置为选项`ConfigureServices`中的方法`Startup`。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-232">Instead of duplicating `ResponseCache` settings on many controller action attributes, cache profiles can be configured as options when setting up MVC in the `ConfigureServices` method in `Startup`.</span></span> <span data-ttu-id="3f7fe-233">在引用的缓存配置文件中找到值用作通过默认`ResponseCache`属性和重写了指定的属性上的任何属性。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-233">Values found in a referenced cache profile are used as the defaults by the `ResponseCache` attribute and are overridden by any properties specified on the attribute.</span></span>

<span data-ttu-id="3f7fe-234">设置缓存配置文件：</span><span class="sxs-lookup"><span data-stu-id="3f7fe-234">Setting up a cache profile:</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="3f7fe-235">[!code-csharp[](response/samples/2.x/ResponseCacheSample/Startup.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="3f7fe-235">[!code-csharp[](response/samples/2.x/ResponseCacheSample/Startup.cs?name=snippet1)]</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="3f7fe-236">[!code-csharp[](response/samples/1.x/ResponseCacheSample/Startup.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="3f7fe-236">[!code-csharp[](response/samples/1.x/ResponseCacheSample/Startup.cs?name=snippet1)]</span></span>

::: moniker-end

<span data-ttu-id="3f7fe-237">引用缓存配置文件：</span><span class="sxs-lookup"><span data-stu-id="3f7fe-237">Referencing a cache profile:</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="3f7fe-238">[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]</span><span class="sxs-lookup"><span data-stu-id="3f7fe-238">[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="3f7fe-239">[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]</span><span class="sxs-lookup"><span data-stu-id="3f7fe-239">[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]</span></span>

::: moniker-end

<span data-ttu-id="3f7fe-240">`ResponseCache`特性可应用于操作 （方法） 和控制器 （类）。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-240">The `ResponseCache` attribute can be applied both to actions (methods) and controllers (classes).</span></span> <span data-ttu-id="3f7fe-241">方法级别的属性重写在类级别特性中指定的设置。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-241">Method-level attributes override the settings specified in class-level attributes.</span></span>

<span data-ttu-id="3f7fe-242">在上面的示例中，类级别属性指定持续时间为 30 秒，而方法级别属性引用与设置为 60 秒的持续时间的缓存配置文件。</span><span class="sxs-lookup"><span data-stu-id="3f7fe-242">In the above example, a class-level attribute specifies a duration of 30 seconds, while a method-level attribute references a cache profile with a duration set to 60 seconds.</span></span>

<span data-ttu-id="3f7fe-243">生成的标头：</span><span class="sxs-lookup"><span data-stu-id="3f7fe-243">The resulting header:</span></span>

```
Cache-Control: public,max-age=60
```

## <a name="additional-resources"></a><span data-ttu-id="3f7fe-244">其他资源</span><span class="sxs-lookup"><span data-stu-id="3f7fe-244">Additional resources</span></span>

* [<span data-ttu-id="3f7fe-245">在缓存中存储响应</span><span class="sxs-lookup"><span data-stu-id="3f7fe-245">Storing Responses in Caches</span></span>](https://tools.ietf.org/html/rfc7234#section-3)
* [<span data-ttu-id="3f7fe-246">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="3f7fe-246">Cache-Control</span></span>](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* [<span data-ttu-id="3f7fe-247">内存中缓存</span><span class="sxs-lookup"><span data-stu-id="3f7fe-247">Cache in-memory</span></span>](xref:performance/caching/memory)
* [<span data-ttu-id="3f7fe-248">使用分布式缓存</span><span class="sxs-lookup"><span data-stu-id="3f7fe-248">Work with a distributed cache</span></span>](xref:performance/caching/distributed)
* [<span data-ttu-id="3f7fe-249">使用更改令牌检测更改</span><span class="sxs-lookup"><span data-stu-id="3f7fe-249">Detect changes with change tokens</span></span>](xref:fundamentals/primitives/change-tokens)
* [<span data-ttu-id="3f7fe-250">响应缓存中间件</span><span class="sxs-lookup"><span data-stu-id="3f7fe-250">Response Caching Middleware</span></span>](xref:performance/caching/middleware)
* [<span data-ttu-id="3f7fe-251">缓存标记帮助程序</span><span class="sxs-lookup"><span data-stu-id="3f7fe-251">Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [<span data-ttu-id="3f7fe-252">分布式缓存标记帮助程序</span><span class="sxs-lookup"><span data-stu-id="3f7fe-252">Distributed Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
