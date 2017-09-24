---
title: "响应缓存在 ASP.NET 核心中的中间件"
author: guardrex
description: "配置和 ASP.NET Core 应用程序中使用的缓存响应的中间件。"
keywords: "ASP.NET 核心响应缓存，缓存，ResponseCache，ResponseCaching，缓存控制、 VaryByQueryKeys、 中间件"
ms.author: riande
manager: wpickett
ms.date: 08/22/2017
ms.topic: article
ms.assetid: f9267eab-2762-42ac-1638-4a25d2c9d67c
ms.prod: asp.net-core
uid: performance/caching/middleware
ms.openlocfilehash: 07626ae7f40dc6f704d69d71cb7f95d318e6f503
ms.sourcegitcommit: 8005eb4051e568d88ee58d48424f39916052e6e2
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2017
---
# <a name="response-caching-middleware-in-aspnet-core"></a><span data-ttu-id="58817-104">响应缓存在 ASP.NET 核心中的中间件</span><span class="sxs-lookup"><span data-stu-id="58817-104">Response Caching Middleware in ASP.NET Core</span></span>

<span data-ttu-id="58817-105">通过[Luke Latham](https://github.com/guardrex)和[John Luo](https://github.com/JunTaoLuo)</span><span class="sxs-lookup"><span data-stu-id="58817-105">By [Luke Latham](https://github.com/guardrex) and [John Luo](https://github.com/JunTaoLuo)</span></span>

[<span data-ttu-id="58817-106">查看或下载示例代码</span><span class="sxs-lookup"><span data-stu-id="58817-106">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/middleware/samples)

<span data-ttu-id="58817-107">本文档提供有关如何配置 ASP.NET Core 应用中的响应缓存中间件的详细信息。</span><span class="sxs-lookup"><span data-stu-id="58817-107">This document provides details on how to configure the Response Caching Middleware in ASP.NET Core apps.</span></span> <span data-ttu-id="58817-108">该中间件确定响应何时可缓存、 存储响应和从缓存充当响应。</span><span class="sxs-lookup"><span data-stu-id="58817-108">The middleware determines when responses are cacheable, stores responses, and serves responses from cache.</span></span> <span data-ttu-id="58817-109">有关 HTTP 缓存功能的简介和`ResponseCache`属性，请参阅[响应缓存](response.md)。</span><span class="sxs-lookup"><span data-stu-id="58817-109">For an introduction to HTTP caching and the `ResponseCache` attribute, see [Response Caching](response.md).</span></span>

## <a name="package"></a><span data-ttu-id="58817-110">Package</span><span class="sxs-lookup"><span data-stu-id="58817-110">Package</span></span>
<span data-ttu-id="58817-111">若要在项目中包含该中间件，添加到引用[ `Microsoft.AspNetCore.ResponseCaching` ](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/)包或使用[ `Microsoft.AspNetCore.All` ](https://www.nuget.org/packages/Microsoft.AspNetCore.All/)包。</span><span class="sxs-lookup"><span data-stu-id="58817-111">To include the middleware in a project, add a reference to the [`Microsoft.AspNetCore.ResponseCaching`](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/) package or use the [`Microsoft.AspNetCore.All`](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) package.</span></span>

## <a name="configuration"></a><span data-ttu-id="58817-112">配置</span><span class="sxs-lookup"><span data-stu-id="58817-112">Configuration</span></span>
<span data-ttu-id="58817-113">在`ConfigureServices`，将该中间件添加到服务集合。</span><span class="sxs-lookup"><span data-stu-id="58817-113">In `ConfigureServices`, add the middleware to the service collection.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="58817-114">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="58817-114">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](middleware/samples/2.x/Program.cs?name=snippet1&highlight=4)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="58817-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="58817-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](middleware/samples/1.x/Startup.cs?name=snippet1&highlight=3)]

---

<span data-ttu-id="58817-116">配置应用程序以使用与中间件`UseResponseCaching`扩展方法，将该中间件添加到请求处理管道。</span><span class="sxs-lookup"><span data-stu-id="58817-116">Configure the app to use the middleware with the `UseResponseCaching` extension method, which adds the middleware to the request processing pipeline.</span></span> <span data-ttu-id="58817-117">示例应用添加[ `Cache-Control` ](https://tools.ietf.org/html/rfc7234#section-5.2)最多 10 秒钟来缓存可缓存响应的响应的标头。</span><span class="sxs-lookup"><span data-stu-id="58817-117">The sample app adds a [`Cache-Control`](https://tools.ietf.org/html/rfc7234#section-5.2) header to the response that caches cacheable responses for up to 10 seconds.</span></span> <span data-ttu-id="58817-118">该示例发送[ `Vary` ](https://tools.ietf.org/html/rfc7231#section-7.1.4)标头来配置用于缓存的响应仅当该中间件[ `Accept-Encoding` ](https://tools.ietf.org/html/rfc7231#section-5.3.4)的后续请求的标头与原始请求相匹配。</span><span class="sxs-lookup"><span data-stu-id="58817-118">The sample sends a [`Vary`](https://tools.ietf.org/html/rfc7231#section-7.1.4) header to configure the middleware to serve a cached response only if the [`Accept-Encoding`](https://tools.ietf.org/html/rfc7231#section-5.3.4) header of subsequent requests matches that of the original request.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="58817-119">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="58817-119">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](middleware/samples/2.x/Program.cs?name=snippet1&highlight=8)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="58817-120">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="58817-120">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](middleware/samples/1.x/Startup.cs?name=snippet2&highlight=3)]

---

<span data-ttu-id="58817-121">响应缓存中间件仅缓存 200 （正常） 服务器响应。</span><span class="sxs-lookup"><span data-stu-id="58817-121">The Response Caching Middleware only caches 200 (OK) server responses.</span></span> <span data-ttu-id="58817-122">任何其他响应，包括[错误页](xref:fundamentals/error-handling)，将忽略的中间件。</span><span class="sxs-lookup"><span data-stu-id="58817-122">Any other responses, including [error pages](xref:fundamentals/error-handling), are ignored by the middleware.</span></span>

> [!WARNING]
> <span data-ttu-id="58817-123">响应包含内容的经过身份验证的客户端必须标记为不可缓存，以防止从存储并提供这些响应中间件。</span><span class="sxs-lookup"><span data-stu-id="58817-123">Responses containing content for authenticated clients must be marked as not cacheable to prevent the middleware from storing and serving those responses.</span></span> <span data-ttu-id="58817-124">请参阅[缓存的条件](#conditions-for-caching)有关该中间件如何确定响应是否是可缓存的详细信息。</span><span class="sxs-lookup"><span data-stu-id="58817-124">See [Conditions for caching](#conditions-for-caching) for details on how the middleware determines if a response is cacheable.</span></span>

## <a name="options"></a><span data-ttu-id="58817-125">选项</span><span class="sxs-lookup"><span data-stu-id="58817-125">Options</span></span>
<span data-ttu-id="58817-126">该中间件提供三个选项用于控制响应缓存。</span><span class="sxs-lookup"><span data-stu-id="58817-126">The middleware offers three options for controlling response caching.</span></span>

| <span data-ttu-id="58817-127">选项</span><span class="sxs-lookup"><span data-stu-id="58817-127">Option</span></span>                | <span data-ttu-id="58817-128">默认值</span><span class="sxs-lookup"><span data-stu-id="58817-128">Default Value</span></span> |
| --------------------- | ------------- |
| <span data-ttu-id="58817-129">UseCaseSensitivePaths</span><span class="sxs-lookup"><span data-stu-id="58817-129">UseCaseSensitivePaths</span></span> | <span data-ttu-id="58817-130">确定响应将在区分大小写的路径上缓存。</span><span class="sxs-lookup"><span data-stu-id="58817-130">Determines if responses are cached on case-sensitive paths.</span></span></p><p><span data-ttu-id="58817-131">默认值为 `false`。</span><span class="sxs-lookup"><span data-stu-id="58817-131">The default value is `false`.</span></span> |
| <span data-ttu-id="58817-132">MaximumBodySize</span><span class="sxs-lookup"><span data-stu-id="58817-132">MaximumBodySize</span></span>       | <span data-ttu-id="58817-133">以字节为单位的响应正文的最大缓存大小。</span><span class="sxs-lookup"><span data-stu-id="58817-133">The largest cacheable size for the response body in bytes.</span></span></p><span data-ttu-id="58817-134">默认值是`64 * 1024 * 1024`(64 MB)。</span><span class="sxs-lookup"><span data-stu-id="58817-134">The default value is `64 * 1024 * 1024` (64 MB).</span></span> |
| <span data-ttu-id="58817-135">大小限制</span><span class="sxs-lookup"><span data-stu-id="58817-135">SizeLimit</span></span>             | <span data-ttu-id="58817-136">以字节为单位的响应缓存中间件大小限制。</span><span class="sxs-lookup"><span data-stu-id="58817-136">The size limit for the response cache middleware in bytes.</span></span> <span data-ttu-id="58817-137">默认值是`100 * 1024 * 1024`(100 MB)。</span><span class="sxs-lookup"><span data-stu-id="58817-137">The default value is `100 * 1024 * 1024` (100 MB).</span></span> |

<span data-ttu-id="58817-138">下面的示例将配置小于或等于 1024 字节使用区分大小写的路径，存储的响应的缓存响应的中间件`/page1`和`/Page1`单独。</span><span class="sxs-lookup"><span data-stu-id="58817-138">The following example configures the middleware to cache responses smaller than or equal to 1,024 bytes using case-sensitive paths, storing the responses to `/page1` and `/Page1` separately.</span></span>

```csharp
services.AddResponseCaching(options =>
{
    options.UseCaseSensitivePaths = true;
    options.MaximumBodySize = 1024;
});
```

## <a name="varybyquerykeys"></a><span data-ttu-id="58817-139">VaryByQueryKeys</span><span class="sxs-lookup"><span data-stu-id="58817-139">VaryByQueryKeys</span></span>
<span data-ttu-id="58817-140">当使用 MVC，`ResponseCache`属性指定所需的设置适当的标头，为响应缓存参数。</span><span class="sxs-lookup"><span data-stu-id="58817-140">When using MVC, the `ResponseCache` attribute specifies the parameters necessary for setting appropriate headers for response caching.</span></span> <span data-ttu-id="58817-141">唯一参数`ResponseCache`严格需要中间件的属性是`VaryByQueryKeys`，这不对应于实际 HTTP 标头。</span><span class="sxs-lookup"><span data-stu-id="58817-141">The only parameter of the `ResponseCache` attribute that strictly requires the middleware is `VaryByQueryKeys`, which doesn't correspond to an actual HTTP header.</span></span> <span data-ttu-id="58817-142">有关详细信息，请参阅[ResponseCache 属性](response.md#responsecache-attribute)。</span><span class="sxs-lookup"><span data-stu-id="58817-142">For more information, see [ResponseCache Attribute](response.md#responsecache-attribute).</span></span>

<span data-ttu-id="58817-143">当未使用 MVC，您可以改变响应缓存与`VaryByQueryKeys`功能。</span><span class="sxs-lookup"><span data-stu-id="58817-143">When not using MVC, you can vary response caching with the `VaryByQueryKeys` feature.</span></span> <span data-ttu-id="58817-144">使用`ResponseCachingFeature`直接从`IFeatureCollection`的`HttpContext`:</span><span class="sxs-lookup"><span data-stu-id="58817-144">Use the `ResponseCachingFeature` directly from the `IFeatureCollection` of the `HttpContext`:</span></span>

```csharp
var responseCachingFeature = context.HttpContext.Features.Get<IResponseCachingFeature>();
if (responseCachingFeature != null)
{
    responseCachingFeature.VaryByQueryKeys = new[] { "MyKey" };
}
```

## <a name="http-headers-used-by-response-caching-middleware"></a><span data-ttu-id="58817-145">响应缓存中间件所使用的 HTTP 标头</span><span class="sxs-lookup"><span data-stu-id="58817-145">HTTP headers used by Response Caching Middleware</span></span>
<span data-ttu-id="58817-146">该中间件的缓存的响应是通过 HTTP 标头配置的。</span><span class="sxs-lookup"><span data-stu-id="58817-146">Response caching by the middleware is configured via HTTP headers.</span></span> <span data-ttu-id="58817-147">有关它们如何影响缓存的说明与下面列出了相关的标头。</span><span class="sxs-lookup"><span data-stu-id="58817-147">The relevant headers are listed below with notes on how they affect caching.</span></span>

| <span data-ttu-id="58817-148">Header</span><span class="sxs-lookup"><span data-stu-id="58817-148">Header</span></span> | <span data-ttu-id="58817-149">详细信息</span><span class="sxs-lookup"><span data-stu-id="58817-149">Details</span></span> |
| ------ | ------- |
| <span data-ttu-id="58817-150">授权</span><span class="sxs-lookup"><span data-stu-id="58817-150">Authorization</span></span> | <span data-ttu-id="58817-151">如果标头存在，则不缓存响应。</span><span class="sxs-lookup"><span data-stu-id="58817-151">The response isn't cached if the header exists.</span></span> |
| <span data-ttu-id="58817-152">缓存控制</span><span class="sxs-lookup"><span data-stu-id="58817-152">Cache-Control</span></span> | <span data-ttu-id="58817-153">该中间件只考虑缓存标记为响应`public`缓存指令。</span><span class="sxs-lookup"><span data-stu-id="58817-153">The middleware only considers caching responses marked with the `public` cache directive.</span></span> <span data-ttu-id="58817-154">你可以控制缓存使用以下参数：</span><span class="sxs-lookup"><span data-stu-id="58817-154">You can control caching with the following parameters:</span></span><ul><li><span data-ttu-id="58817-155">最长时间</span><span class="sxs-lookup"><span data-stu-id="58817-155">max-age</span></span></li><li><span data-ttu-id="58817-156">最大过时 &#8224;</span><span class="sxs-lookup"><span data-stu-id="58817-156">max-stale&#8224;</span></span></li><li><span data-ttu-id="58817-157">最小值全新</span><span class="sxs-lookup"><span data-stu-id="58817-157">min-fresh</span></span></li><li><span data-ttu-id="58817-158">必须重新验证</span><span class="sxs-lookup"><span data-stu-id="58817-158">must-revalidate</span></span></li><li><span data-ttu-id="58817-159">无缓存</span><span class="sxs-lookup"><span data-stu-id="58817-159">no-cache</span></span></li><li><span data-ttu-id="58817-160">无存储</span><span class="sxs-lookup"><span data-stu-id="58817-160">no-store</span></span></li><li><span data-ttu-id="58817-161">仅当-缓存</span><span class="sxs-lookup"><span data-stu-id="58817-161">only-if-cached</span></span></li><li><span data-ttu-id="58817-162">private</span><span class="sxs-lookup"><span data-stu-id="58817-162">private</span></span></li><li><span data-ttu-id="58817-163">public</span><span class="sxs-lookup"><span data-stu-id="58817-163">public</span></span></li><li><span data-ttu-id="58817-164">s maxage</span><span class="sxs-lookup"><span data-stu-id="58817-164">s-maxage</span></span></li><li><span data-ttu-id="58817-165">代理重新验证和 #8225;</span><span class="sxs-lookup"><span data-stu-id="58817-165">proxy-revalidate&#8225;</span></span></li></ul><span data-ttu-id="58817-166">&#8224; 如果没有限制指定到`max-stale`，中间件不执行任何操作。</span><span class="sxs-lookup"><span data-stu-id="58817-166">&#8224;If no limit is specified to `max-stale`, the middleware takes no action.</span></span><br><span data-ttu-id="58817-167">&#8225;`proxy-revalidate`具有相同的效果`must-revalidate`。</span><span class="sxs-lookup"><span data-stu-id="58817-167">&#8225;`proxy-revalidate` has the same effect as `must-revalidate`.</span></span><br><br><span data-ttu-id="58817-168">有关详细信息，请参阅[RFC 7231： 请求的缓存控制指令](https://tools.ietf.org/html/rfc7234#section-5.2.1)。</span><span class="sxs-lookup"><span data-stu-id="58817-168">For more information, see [RFC 7231: Request Cache-Control Directives](https://tools.ietf.org/html/rfc7234#section-5.2.1).</span></span> |
| <span data-ttu-id="58817-169">杂注</span><span class="sxs-lookup"><span data-stu-id="58817-169">Pragma</span></span> | <span data-ttu-id="58817-170">A`Pragma: no-cache`请求标头中的生成相同的效果`Cache-Control: no-cache`。</span><span class="sxs-lookup"><span data-stu-id="58817-170">A `Pragma: no-cache` header in the request produces the same effect as `Cache-Control: no-cache`.</span></span> <span data-ttu-id="58817-171">此标头中的相关指令来重写`Cache-Control`标头，如果存在。</span><span class="sxs-lookup"><span data-stu-id="58817-171">This header is overridden by the relevant directives in the `Cache-Control` header, if present.</span></span> <span data-ttu-id="58817-172">考虑对与 HTTP/1.0 的向后兼容性。</span><span class="sxs-lookup"><span data-stu-id="58817-172">Considered for backward compatibility with HTTP/1.0.</span></span> |
| <span data-ttu-id="58817-173">集 Cookie</span><span class="sxs-lookup"><span data-stu-id="58817-173">Set-Cookie</span></span> | <span data-ttu-id="58817-174">如果标头存在，则不缓存响应。</span><span class="sxs-lookup"><span data-stu-id="58817-174">The response isn't cached if the header exists.</span></span> |
| <span data-ttu-id="58817-175">改变</span><span class="sxs-lookup"><span data-stu-id="58817-175">Vary</span></span> | <span data-ttu-id="58817-176">`Vary`标头用于改变缓存的响应的另一个标头。</span><span class="sxs-lookup"><span data-stu-id="58817-176">The `Vary` header is used to vary the cached response by another header.</span></span> <span data-ttu-id="58817-177">例如，可以通过包括通过编码来缓存响应`Vary: Accept-Encoding`标头，来缓存响应的请求标头`Accept-Encoding: gzip`和`Accept-Encoding: text/plain`单独。</span><span class="sxs-lookup"><span data-stu-id="58817-177">For example, you can cache responses by encoding by including the `Vary: Accept-Encoding` header, which caches responses for requests with headers `Accept-Encoding: gzip` and `Accept-Encoding: text/plain` separately.</span></span> <span data-ttu-id="58817-178">标头值为响应`*`永远不会存储。</span><span class="sxs-lookup"><span data-stu-id="58817-178">A response with a header value of `*` is never stored.</span></span> |
| <span data-ttu-id="58817-179">过期</span><span class="sxs-lookup"><span data-stu-id="58817-179">Expires</span></span> | <span data-ttu-id="58817-180">通过此标头视为过时的响应不存储或检索除非重写由其他`Cache-Control`标头。</span><span class="sxs-lookup"><span data-stu-id="58817-180">A response deemed stale by this header isn't stored or retrieved unless overridden by other `Cache-Control` headers.</span></span> |
| <span data-ttu-id="58817-181">None-If-match</span><span class="sxs-lookup"><span data-stu-id="58817-181">If-None-Match</span></span> | <span data-ttu-id="58817-182">如果该值不完整的响应从缓存提供`*`和`ETag`的响应中不匹配任何提供的值。</span><span class="sxs-lookup"><span data-stu-id="58817-182">The full response is served from cache if the value isn't `*` and the `ETag` of the response doesn't match any of the values provided.</span></span> <span data-ttu-id="58817-183">否则，提供 304 （未修改） 响应。</span><span class="sxs-lookup"><span data-stu-id="58817-183">Otherwise, a 304 (Not Modified) response is served.</span></span> |
| <span data-ttu-id="58817-184">如果-修改-自</span><span class="sxs-lookup"><span data-stu-id="58817-184">If-Modified-Since</span></span> | <span data-ttu-id="58817-185">如果`If-None-Match`标头不存在，如果缓存的响应日期晚于提供的值，完整的响应从缓存中提供。</span><span class="sxs-lookup"><span data-stu-id="58817-185">If the `If-None-Match` header isn't present, a full response is served from cache if the cached response date is newer than the value provided.</span></span> <span data-ttu-id="58817-186">否则，提供 304 （未修改） 响应。</span><span class="sxs-lookup"><span data-stu-id="58817-186">Otherwise, a 304 (Not Modified) response is served.</span></span> |
| <span data-ttu-id="58817-187">日期</span><span class="sxs-lookup"><span data-stu-id="58817-187">Date</span></span> | <span data-ttu-id="58817-188">从缓存提供服务时`Date`由该中间件设置标头，如果它未在原始响应上提供。</span><span class="sxs-lookup"><span data-stu-id="58817-188">When serving from cache, the `Date` header is set by the middleware if it wasn't provided on the original response.</span></span> |
| <span data-ttu-id="58817-189">内容长度</span><span class="sxs-lookup"><span data-stu-id="58817-189">Content-Length</span></span> | <span data-ttu-id="58817-190">从缓存提供服务时`Content-Length`由该中间件设置标头，如果它未在原始响应上提供。</span><span class="sxs-lookup"><span data-stu-id="58817-190">When serving from cache, the `Content-Length` header is set by the middleware if it wasn't provided on the original response.</span></span> |
| <span data-ttu-id="58817-191">保留时间</span><span class="sxs-lookup"><span data-stu-id="58817-191">Age</span></span> | <span data-ttu-id="58817-192">`Age`在原始响应中发送的标头将被忽略。</span><span class="sxs-lookup"><span data-stu-id="58817-192">The `Age` header sent in the original response is ignored.</span></span> <span data-ttu-id="58817-193">提供缓存的响应时，该中间件将计算新值。</span><span class="sxs-lookup"><span data-stu-id="58817-193">The middleware computes a new value when serving a cached response.</span></span> |

## <a name="troubleshooting"></a><span data-ttu-id="58817-194">疑难解答</span><span class="sxs-lookup"><span data-stu-id="58817-194">Troubleshooting</span></span>
<span data-ttu-id="58817-195">如果缓存行为未按预期，，确认响应是否可缓存并且能够从缓存提供的检查请求的传入标头和响应的传出标头。</span><span class="sxs-lookup"><span data-stu-id="58817-195">If caching behavior isn't as you expect, confirm that responses are cacheable and capable of being served from the cache by examining the request's incoming headers and the response's outgoing headers.</span></span> <span data-ttu-id="58817-196">启用[日志记录](xref:fundamentals/logging)可帮助在调试时。</span><span class="sxs-lookup"><span data-stu-id="58817-196">Enabling [logging](xref:fundamentals/logging) can help when debugging.</span></span> <span data-ttu-id="58817-197">中间件日志缓存行为和从缓存中时检索的响应。</span><span class="sxs-lookup"><span data-stu-id="58817-197">The middleware logs caching behavior and when a response is retrieved from cache.</span></span>

<span data-ttu-id="58817-198">当测试和故障排除缓存行为，浏览器可能设置影响不可取的方法中的缓存的请求标头。</span><span class="sxs-lookup"><span data-stu-id="58817-198">When testing and troubleshooting caching behavior, a browser may set request headers that affect caching in undesirable ways.</span></span> <span data-ttu-id="58817-199">例如，浏览器可能设置`Cache-Control`标头到`no-cache`刷新页面时。</span><span class="sxs-lookup"><span data-stu-id="58817-199">For example, a browser may set the `Cache-Control` header to `no-cache` when you refresh the page.</span></span> <span data-ttu-id="58817-200">以下工具可以显式设置请求标头，，和测试缓存的首选：</span><span class="sxs-lookup"><span data-stu-id="58817-200">The following tools can explicitly set request headers, and are preferred for testing caching:</span></span>

* [<span data-ttu-id="58817-201">Fiddler</span><span class="sxs-lookup"><span data-stu-id="58817-201">Fiddler</span></span>](http://www.telerik.com/fiddler)
* [<span data-ttu-id="58817-202">Firebug</span><span class="sxs-lookup"><span data-stu-id="58817-202">Firebug</span></span>](http://getfirebug.com/)
* [<span data-ttu-id="58817-203">Postman</span><span class="sxs-lookup"><span data-stu-id="58817-203">Postman</span></span>](https://www.getpostman.com/)

### <a name="conditions-for-caching"></a><span data-ttu-id="58817-204">用于缓存的条件</span><span class="sxs-lookup"><span data-stu-id="58817-204">Conditions for caching</span></span>
* <span data-ttu-id="58817-205">请求必须导致来自服务器的 200 （正常） 响应。</span><span class="sxs-lookup"><span data-stu-id="58817-205">The request must result in a 200 (OK) response from the server.</span></span>
* <span data-ttu-id="58817-206">请求方法必须是 GET 或 HEAD。</span><span class="sxs-lookup"><span data-stu-id="58817-206">The request method must be GET or HEAD.</span></span>
* <span data-ttu-id="58817-207">终端中间件，如静态文件中间件，必须处理响应缓存中间件之前的响应。</span><span class="sxs-lookup"><span data-stu-id="58817-207">Terminal middleware, such as Static File Middleware, must not process the response prior to the Response Caching Middleware.</span></span>
* <span data-ttu-id="58817-208">`Authorization`标头不能存在。</span><span class="sxs-lookup"><span data-stu-id="58817-208">The `Authorization` header must not be present.</span></span>
* <span data-ttu-id="58817-209">`Cache-Control`标头参数必须是有效，并且必须标记为响应`public`和未标记`private`。</span><span class="sxs-lookup"><span data-stu-id="58817-209">`Cache-Control` header parameters must be valid, and the response must be marked `public` and not marked `private`.</span></span>
* <span data-ttu-id="58817-210">`Pragma: no-cache`标头/值不能存在如果`Cache-Control`标头不存在，作为`Cache-Control`标头重写`Pragma`标头时存在。</span><span class="sxs-lookup"><span data-stu-id="58817-210">The `Pragma: no-cache` header/value must not be present if the `Cache-Control` header isn't present, as the `Cache-Control` header overrides the `Pragma` header when present.</span></span>
* <span data-ttu-id="58817-211">`Set-Cookie`标头不能存在。</span><span class="sxs-lookup"><span data-stu-id="58817-211">The `Set-Cookie` header must not be present.</span></span>
* <span data-ttu-id="58817-212">`Vary`标头参数必须是有效且不等于`*`。</span><span class="sxs-lookup"><span data-stu-id="58817-212">`Vary` header parameters must be valid and not equal to `*`.</span></span>
* <span data-ttu-id="58817-213">`Content-Length`标头值 (如果设置) 必须与匹配的响应正文的大小。</span><span class="sxs-lookup"><span data-stu-id="58817-213">The `Content-Length` header value (if set) must match the size of the response body.</span></span>
* <span data-ttu-id="58817-214">[IHttpSendFileFeature](/aspnet/core/api/microsoft.aspnetcore.http.features.ihttpsendfilefeature)未使用。</span><span class="sxs-lookup"><span data-stu-id="58817-214">The [IHttpSendFileFeature](/aspnet/core/api/microsoft.aspnetcore.http.features.ihttpsendfilefeature) isn't used.</span></span>
* <span data-ttu-id="58817-215">响应不能为指定的陈旧`Expires`标头和`max-age`和`s-maxage`缓存指令。</span><span class="sxs-lookup"><span data-stu-id="58817-215">The response must not be stale as specified by the `Expires` header and the `max-age` and `s-maxage` cache directives.</span></span>
* <span data-ttu-id="58817-216">响应缓冲会成功，从而响应的大小小于已配置或默认`SizeLimit`。</span><span class="sxs-lookup"><span data-stu-id="58817-216">Response buffering is successful, and the size of the response is smaller than the configured or default `SizeLimit`.</span></span>
* <span data-ttu-id="58817-217">响应必须是可根据缓存[RFC 7234](https://tools.ietf.org/html/rfc7234)规范。</span><span class="sxs-lookup"><span data-stu-id="58817-217">The response must be cacheable according to the [RFC 7234](https://tools.ietf.org/html/rfc7234) specifications.</span></span> <span data-ttu-id="58817-218">例如，`no-store`指令必须在请求或响应标头字段中存在。</span><span class="sxs-lookup"><span data-stu-id="58817-218">For example, the `no-store` directive must not exist in request or response header fields.</span></span> <span data-ttu-id="58817-219">请参阅*第 3 部分： 在缓存中存储响应*的[RFC 7234](https://tools.ietf.org/html/rfc7234)有关详细信息。</span><span class="sxs-lookup"><span data-stu-id="58817-219">See *Section 3: Storing Responses in Caches* of [RFC 7234](https://tools.ietf.org/html/rfc7234) for details.</span></span>

> [!NOTE]
> <span data-ttu-id="58817-220">Antiforgery 系统用于生成安全令牌，以防止跨站点请求伪造 (CSRF) 攻击集`Cache-Control`和`Pragma`标头到`no-cache`以便不缓存响应。</span><span class="sxs-lookup"><span data-stu-id="58817-220">The Antiforgery system for generating secure tokens to prevent Cross-Site Request Forgery (CSRF) attacks sets the `Cache-Control` and `Pragma` headers to `no-cache` so that responses aren't cached.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="58817-221">其他资源</span><span class="sxs-lookup"><span data-stu-id="58817-221">Additional resources</span></span>

* [<span data-ttu-id="58817-222">应用程序启动</span><span class="sxs-lookup"><span data-stu-id="58817-222">Application Startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="58817-223">中间件</span><span class="sxs-lookup"><span data-stu-id="58817-223">Middleware</span></span>](xref:fundamentals/middleware)
