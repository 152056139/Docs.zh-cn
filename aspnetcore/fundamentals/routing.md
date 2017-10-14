---
title: "ASP.NET 核心中的路由"
author: ardalis
description: "发现如何 ASP.NET Core 路由功能是负责将传入的请求映射到路由处理程序。"
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: bbbcf9e4-3c4c-4f50-b91e-175fe9cae4e2
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/routing
ms.openlocfilehash: 5dd8bee7228587d7e13f128bc8f16102fb70a412
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/13/2017
---
# <a name="routing-in-aspnet-core"></a><span data-ttu-id="08812-104">ASP.NET 核心中的路由</span><span class="sxs-lookup"><span data-stu-id="08812-104">Routing in ASP.NET Core</span></span>

<span data-ttu-id="08812-105">通过[Ryan Nowak](https://github.com/rynowak)， [Steve Smith](https://ardalis.com/)，和[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="08812-105">By [Ryan Nowak](https://github.com/rynowak), [Steve Smith](https://ardalis.com/), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="08812-106">路由功能是负责将传入的请求映射到路由处理程序。</span><span class="sxs-lookup"><span data-stu-id="08812-106">Routing functionality is responsible for mapping an incoming request to a route handler.</span></span> <span data-ttu-id="08812-107">路由在 ASP.NET 应用程序中定义并配置应用程序启动时。</span><span class="sxs-lookup"><span data-stu-id="08812-107">Routes are defined in the ASP.NET app and configured when the app starts up.</span></span> <span data-ttu-id="08812-108">路由 （可选） 可以从包含在请求中，URL 中提取值，这些值则可以用于处理请求。</span><span class="sxs-lookup"><span data-stu-id="08812-108">A route can optionally extract values from the URL contained in the request, and these values can then be used for request processing.</span></span> <span data-ttu-id="08812-109">使用 ASP.NET 应用程序中的路由信息，就还能够生成映射到路由处理程序的 Url 的路由的功能。</span><span class="sxs-lookup"><span data-stu-id="08812-109">Using route information from the ASP.NET app, the routing functionality is also able to generate URLs that map to route handlers.</span></span> <span data-ttu-id="08812-110">因此，路由可以查找基于一个 URL 或对应于给定的路由处理程序基于路由处理程序信息的 URL 路由处理程序。</span><span class="sxs-lookup"><span data-stu-id="08812-110">Therefore, routing can find a route handler based on a URL, or the URL corresponding to a given route handler based on route handler information.</span></span>

>[!IMPORTANT]
> <span data-ttu-id="08812-111">本文档介绍了较低级别的 ASP.NET Core 路由。</span><span class="sxs-lookup"><span data-stu-id="08812-111">This document covers the low level ASP.NET Core routing.</span></span> <span data-ttu-id="08812-112">有关 ASP.NET 核心 MVC 路由，请参阅[路由到控制器操作](../mvc/controllers/routing.md)</span><span class="sxs-lookup"><span data-stu-id="08812-112">For ASP.NET Core MVC routing, see [Routing to Controller Actions](../mvc/controllers/routing.md)</span></span>

<span data-ttu-id="08812-113">[查看或下载示例代码](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/routing/sample)（[如何下载](xref:tutorials/index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="08812-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/routing/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="routing-basics"></a><span data-ttu-id="08812-114">路由的基础知识</span><span class="sxs-lookup"><span data-stu-id="08812-114">Routing basics</span></span>

<span data-ttu-id="08812-115">路由将使用*路由*(的实现[IRouter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.routing.irouter)) 到：</span><span class="sxs-lookup"><span data-stu-id="08812-115">Routing uses *routes* (implementations of [IRouter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.routing.irouter)) to:</span></span>

* <span data-ttu-id="08812-116">映射到的传入请求*路由处理程序*</span><span class="sxs-lookup"><span data-stu-id="08812-116">map incoming requests to *route handlers*</span></span>

* <span data-ttu-id="08812-117">生成在响应中使用的 Url</span><span class="sxs-lookup"><span data-stu-id="08812-117">generate URLs used in responses</span></span>

<span data-ttu-id="08812-118">通常情况下，应用程序必须具有单个路由的集合。</span><span class="sxs-lookup"><span data-stu-id="08812-118">Generally, an app has a single collection of routes.</span></span> <span data-ttu-id="08812-119">当请求到达时，按顺序进行处理的路由集合。</span><span class="sxs-lookup"><span data-stu-id="08812-119">When a request arrives, the route collection is processed in order.</span></span> <span data-ttu-id="08812-120">通过调用匹配请求 URL 的路由查找传入请求`RouteAsync`路由集合中每个可用路由方法。</span><span class="sxs-lookup"><span data-stu-id="08812-120">The incoming request looks for a route that matches the request URL by calling the `RouteAsync` method on each available route in the route collection.</span></span> <span data-ttu-id="08812-121">与此相反，响应可以使用路由以生成基于路由信息 （例如，用于重定向或链接） 的 Url，并因此避免硬编码 url，它可帮助可维护性。</span><span class="sxs-lookup"><span data-stu-id="08812-121">By contrast, a response can use routing to generate URLs (for example, for redirection or links) based on route information, and thus avoid having to hard-code URLs, which helps maintainability.</span></span>

<span data-ttu-id="08812-122">路由连接到[中间件](middleware.md)按管道`RouterMiddleware`类。</span><span class="sxs-lookup"><span data-stu-id="08812-122">Routing is connected to the [middleware](middleware.md) pipeline by the `RouterMiddleware` class.</span></span> <span data-ttu-id="08812-123">[ASP.NET MVC](../mvc/overview.md)添加路由到中间件管道，其配置的一部分。</span><span class="sxs-lookup"><span data-stu-id="08812-123">[ASP.NET MVC](../mvc/overview.md) adds routing to the middleware pipeline as part of its configuration.</span></span> <span data-ttu-id="08812-124">若要了解有关使用路由作为独立组件，请参阅[使用的路由的中间件](#using-routing-middleware)。</span><span class="sxs-lookup"><span data-stu-id="08812-124">To learn about using routing as a standalone component, see [using-routing-middleware](#using-routing-middleware).</span></span>

<a name="url-matching-ref"></a>

### <a name="url-matching"></a><span data-ttu-id="08812-125">匹配 URL</span><span class="sxs-lookup"><span data-stu-id="08812-125">URL matching</span></span>

<span data-ttu-id="08812-126">匹配 URL 是通过哪些路由调度传入请求到的过程*处理程序*。</span><span class="sxs-lookup"><span data-stu-id="08812-126">URL matching is the process by which routing dispatches an incoming request to a *handler*.</span></span> <span data-ttu-id="08812-127">此过程通常基于 URL 路径中的数据，但可以对其进行扩展，以考虑请求中的任何数据。</span><span class="sxs-lookup"><span data-stu-id="08812-127">This process is generally based on data in the URL path, but can be extended to consider any data in the request.</span></span> <span data-ttu-id="08812-128">调度到单独的处理程序的请求的功能是用于缩放的大小和复杂性的应用程序密钥。</span><span class="sxs-lookup"><span data-stu-id="08812-128">The ability to dispatch requests to separate handlers is key to scaling the size and complexity of an application.</span></span>

<span data-ttu-id="08812-129">传入的请求输入`RouterMiddleware`，哪些调用`RouteAsync`序列中的每个路由方法。</span><span class="sxs-lookup"><span data-stu-id="08812-129">Incoming requests enter the `RouterMiddleware`, which calls the `RouteAsync` method on each route in sequence.</span></span> <span data-ttu-id="08812-130">`IRouter`实例选择是否*处理*通过设置请求`RouteContext.Handler`为非 null `RequestDelegate`。</span><span class="sxs-lookup"><span data-stu-id="08812-130">The `IRouter` instance chooses whether to *handle* the request by setting the `RouteContext.Handler` to a non-null `RequestDelegate`.</span></span> <span data-ttu-id="08812-131">如果路由设置的处理程序请求，将调用路由处理停止点和处理程序来处理该请求。</span><span class="sxs-lookup"><span data-stu-id="08812-131">If a route sets a handler for the request, route processing stops and the handler will be invoked to process the request.</span></span> <span data-ttu-id="08812-132">如果尝试所有路由并且没有处理程序发现请求，则该中间件将调用*下一步*和调用请求管道中的下一步中间件。</span><span class="sxs-lookup"><span data-stu-id="08812-132">If all routes are tried and no handler is found for the request, the middleware calls *next* and the next middleware in the request pipeline is invoked.</span></span>

<span data-ttu-id="08812-133">主要输入`RouteAsync`是`RouteContext.HttpContext`与当前请求相关联。</span><span class="sxs-lookup"><span data-stu-id="08812-133">The primary input to `RouteAsync` is the `RouteContext.HttpContext` associated with the current request.</span></span> <span data-ttu-id="08812-134">`RouteContext.Handler`和`RouteContext.RouteData`会将路由匹配后的输出。</span><span class="sxs-lookup"><span data-stu-id="08812-134">The `RouteContext.Handler` and `RouteContext.RouteData` are outputs that will be set after a route matches.</span></span>

<span data-ttu-id="08812-135">在匹配`RouteAsync`还将设置的属性`RouteContext.RouteData`为适当的值根据到目前为止完成的请求处理。</span><span class="sxs-lookup"><span data-stu-id="08812-135">A match during `RouteAsync` will also set the properties of the `RouteContext.RouteData` to appropriate values based on the request processing done so far.</span></span> <span data-ttu-id="08812-136">如果路由与匹配的请求，`RouteContext.RouteData`将包含重要的状态信息有关*结果*。</span><span class="sxs-lookup"><span data-stu-id="08812-136">If a route matches a request, the `RouteContext.RouteData` will contain important state information about the *result*.</span></span>

<span data-ttu-id="08812-137">`RouteData.Values`是一个字典的*路由值*路由时生成。</span><span class="sxs-lookup"><span data-stu-id="08812-137">`RouteData.Values` is a dictionary of *route values* produced from the route.</span></span> <span data-ttu-id="08812-138">这些值通常由标记化 URL，并且可以用于接受用户输入，或进一步调度决定对应用程序中。</span><span class="sxs-lookup"><span data-stu-id="08812-138">These values are usually determined by tokenizing the URL, and can be used to accept user input, or to make further dispatching decisions inside the application.</span></span>

<span data-ttu-id="08812-139">`RouteData.DataTokens`是一个属性包，为该匹配路由相关的其他数据。</span><span class="sxs-lookup"><span data-stu-id="08812-139">`RouteData.DataTokens`  is a property bag of additional data related to the matched route.</span></span> <span data-ttu-id="08812-140">`DataTokens`可用于支持将数据与每个路由，以便应用程序可以基于做出决策更高版本上哪个路由匹配的状态。</span><span class="sxs-lookup"><span data-stu-id="08812-140">`DataTokens` are provided to support associating state data with each route so the application can make decisions later based on which route matched.</span></span> <span data-ttu-id="08812-141">这些值是开发人员定义，并且**不**影响的行为的任何方式中的路由。</span><span class="sxs-lookup"><span data-stu-id="08812-141">These values are developer-defined and do **not** affect the behavior of routing in any way.</span></span> <span data-ttu-id="08812-142">此外，存储在数据令牌中的值可以是任何类型，与路由值，必须能够轻松地转换到和从字符串。</span><span class="sxs-lookup"><span data-stu-id="08812-142">Additionally, values stashed in data tokens can be of any type, in contrast to route values, which must be easily convertible to and from strings.</span></span>

<span data-ttu-id="08812-143">`RouteData.Routers`是花费在成功匹配的请求的一部分的路由的列表。</span><span class="sxs-lookup"><span data-stu-id="08812-143">`RouteData.Routers` is a list of the routes that took part in successfully matching the request.</span></span> <span data-ttu-id="08812-144">路由可以嵌套在另一个，和`Routers`属性反映通过导致匹配的路由逻辑树的路径。</span><span class="sxs-lookup"><span data-stu-id="08812-144">Routes can be nested inside one another, and the `Routers` property reflects the path through the logical tree of routes that resulted in a match.</span></span> <span data-ttu-id="08812-145">第一项通常`Routers`路由集合中，以及应该用于 URL 生成。</span><span class="sxs-lookup"><span data-stu-id="08812-145">Generally the first item in `Routers` is the route collection, and should be used for URL generation.</span></span> <span data-ttu-id="08812-146">中的最后一项`Routers`是匹配的路由处理程序。</span><span class="sxs-lookup"><span data-stu-id="08812-146">The last item in `Routers` is the route handler that matched.</span></span>

### <a name="url-generation"></a><span data-ttu-id="08812-147">URL 生成</span><span class="sxs-lookup"><span data-stu-id="08812-147">URL generation</span></span>

<span data-ttu-id="08812-148">URL 生成是过程通过哪个路由，则可以创建基于一组路由值的 URL 路径。</span><span class="sxs-lookup"><span data-stu-id="08812-148">URL generation is the process by which routing can create a URL path based on a set of route values.</span></span> <span data-ttu-id="08812-149">这使得您的处理程序和对其进行访问的 Url 之间的逻辑分隔。</span><span class="sxs-lookup"><span data-stu-id="08812-149">This allows for a logical separation between your handlers and the URLs that access them.</span></span>

<span data-ttu-id="08812-150">URL 生成遵循类似的迭代过程，但开头用户或框架代码调用到`GetVirtualPath`的路由集合的方法。</span><span class="sxs-lookup"><span data-stu-id="08812-150">URL generation follows a similar iterative process, but starts with user or framework code calling into the `GetVirtualPath` method of the route collection.</span></span> <span data-ttu-id="08812-151">每个*路由*都具有其`GetVirtualPath`序列中非 null 之前调用方法`VirtualPathData`返回。</span><span class="sxs-lookup"><span data-stu-id="08812-151">Each *route* will then have its `GetVirtualPath` method called in sequence until a non-null `VirtualPathData` is returned.</span></span>

<span data-ttu-id="08812-152">主输入到`GetVirtualPath`是：</span><span class="sxs-lookup"><span data-stu-id="08812-152">The primary inputs to `GetVirtualPath` are:</span></span>

* `VirtualPathContext.HttpContext`

* `VirtualPathContext.Values`

* `VirtualPathContext.AmbientValues`

<span data-ttu-id="08812-153">路由主要使用路由值由提供`Values`和`AmbientValues`来确定其所在可以生成的 URL 和要包括的值。</span><span class="sxs-lookup"><span data-stu-id="08812-153">Routes primarily use the route values provided by the `Values` and `AmbientValues` to decide where it is possible to generate a URL and what values to include.</span></span> <span data-ttu-id="08812-154">`AmbientValues`是生成从匹配路由系统的当前请求的路由值的一组。</span><span class="sxs-lookup"><span data-stu-id="08812-154">The `AmbientValues` are the set of route values that were produced from matching the current request with the routing system.</span></span> <span data-ttu-id="08812-155">与此相反，`Values`是路由的值，指定如何生成当前操作所需的 URL。</span><span class="sxs-lookup"><span data-stu-id="08812-155">In contrast, `Values` are the route values that specify how to generate the desired URL for the current operation.</span></span> <span data-ttu-id="08812-156">`HttpContext`路由需要获取服务或与当前上下文关联的其他数据的情况下提供。</span><span class="sxs-lookup"><span data-stu-id="08812-156">The `HttpContext` is provided in case a route needs to get services or additional data associated with the current context.</span></span>

<span data-ttu-id="08812-157">提示： 将`Values`为一组的替代`AmbientValues`。</span><span class="sxs-lookup"><span data-stu-id="08812-157">Tip: Think of `Values` as being a set of overrides for the `AmbientValues`.</span></span> <span data-ttu-id="08812-158">URL 生成尝试重复使用从当前的请求，以便可以方便地生成使用相同的路由或路由值的链接的 Url 的路由值。</span><span class="sxs-lookup"><span data-stu-id="08812-158">URL generation tries to reuse route values from the current request to make it easy to generate URLs for links using the same route or route values.</span></span>

<span data-ttu-id="08812-159">输出`GetVirtualPath`是`VirtualPathData`。</span><span class="sxs-lookup"><span data-stu-id="08812-159">The output of `GetVirtualPath` is a `VirtualPathData`.</span></span> <span data-ttu-id="08812-160">`VirtualPathData`是一种并行的`RouteData`; 它包含`VirtualPath`输出 URL 以及应该通过路由将设置中的某些其他属性。</span><span class="sxs-lookup"><span data-stu-id="08812-160">`VirtualPathData` is a parallel of `RouteData`; it contains the `VirtualPath` for the output URL as well as the some additional properties that should be set by the route.</span></span>

<span data-ttu-id="08812-161">`VirtualPathData.VirtualPath`属性包含*虚拟路径*生成的路由。</span><span class="sxs-lookup"><span data-stu-id="08812-161">The `VirtualPathData.VirtualPath` property contains the *virtual path* produced by the route.</span></span> <span data-ttu-id="08812-162">具体取决于你的需求可能需要处理更多的路径。</span><span class="sxs-lookup"><span data-stu-id="08812-162">Depending on your needs you may need to process the path further.</span></span> <span data-ttu-id="08812-163">例如，如果你想要呈现 HTML 中生成的 URL 需要预先计算的应用程序的基路径。</span><span class="sxs-lookup"><span data-stu-id="08812-163">For instance, if you want to render the generated URL in HTML you need to prepend the base path of the application.</span></span>

<span data-ttu-id="08812-164">`VirtualPathData.Router`是对已成功生成 URL 的路由的引用。</span><span class="sxs-lookup"><span data-stu-id="08812-164">The `VirtualPathData.Router` is a reference to the route that successfully generated the URL.</span></span>

<span data-ttu-id="08812-165">`VirtualPathData.DataTokens`属性是生成 URL 的路由到相关的其他数据的字典。</span><span class="sxs-lookup"><span data-stu-id="08812-165">The `VirtualPathData.DataTokens` properties is a dictionary of additional data related to the route that generated the URL.</span></span> <span data-ttu-id="08812-166">这是并行的`RouteData.DataTokens`。</span><span class="sxs-lookup"><span data-stu-id="08812-166">This is the parallel of `RouteData.DataTokens`.</span></span>

### <a name="creating-routes"></a><span data-ttu-id="08812-167">创建的路由</span><span class="sxs-lookup"><span data-stu-id="08812-167">Creating routes</span></span>

<span data-ttu-id="08812-168">路由提供`Route`作为标准的实现类`IRouter`。</span><span class="sxs-lookup"><span data-stu-id="08812-168">Routing provides the `Route` class as the standard implementation of `IRouter`.</span></span> <span data-ttu-id="08812-169">`Route`使用*路由模板*语法来定义将对 URL 路径匹配的模式时`RouteAsync`调用。</span><span class="sxs-lookup"><span data-stu-id="08812-169">`Route` uses the *route template* syntax to define patterns that will match against the URL path when `RouteAsync` is called.</span></span> <span data-ttu-id="08812-170">`Route`将使用相同的路由模板生成的 URL 时`GetVirtualPath`调用。</span><span class="sxs-lookup"><span data-stu-id="08812-170">`Route` will use the same route template to generate a URL when `GetVirtualPath` is called.</span></span>

<span data-ttu-id="08812-171">大多数应用程序将通过调用创建路由`MapRoute`或类似上定义的扩展方法之一`IRouteBuilder`。</span><span class="sxs-lookup"><span data-stu-id="08812-171">Most applications will create routes by calling `MapRoute` or one of the similar extension methods defined on `IRouteBuilder`.</span></span> <span data-ttu-id="08812-172">所有这些方法将创建的实例`Route`并将其添加到路由集合。</span><span class="sxs-lookup"><span data-stu-id="08812-172">All of these methods will create an instance of `Route` and add it to the route collection.</span></span>

<span data-ttu-id="08812-173">注意：`MapRoute`不带路由处理程序参数-它只是添加将由处理的路由`DefaultHandler`。</span><span class="sxs-lookup"><span data-stu-id="08812-173">Note: `MapRoute` doesn't take a route handler parameter - it only adds routes that will be handled by the `DefaultHandler`.</span></span> <span data-ttu-id="08812-174">由于默认处理程序是`IRouter`，它可能决定不处理该请求。</span><span class="sxs-lookup"><span data-stu-id="08812-174">Since the default handler is an `IRouter`, it may decide not to handle the request.</span></span> <span data-ttu-id="08812-175">例如，ASP.NET MVC 通常配置为仅处理的默认处理请求该匹配项，可用控制器和操作。</span><span class="sxs-lookup"><span data-stu-id="08812-175">For example, ASP.NET MVC is typically configured as a default handler that only handles requests that match an available controller and action.</span></span> <span data-ttu-id="08812-176">若要了解有关路由到 MVC 的详细信息，请参阅[路由到控制器操作](../mvc/controllers/routing.md)。</span><span class="sxs-lookup"><span data-stu-id="08812-176">To learn more about routing to MVC, see [Routing to Controller Actions](../mvc/controllers/routing.md).</span></span>

<span data-ttu-id="08812-177">这是一个示例的`MapRoute`使用典型的 ASP.NET MVC 路由定义的调用：</span><span class="sxs-lookup"><span data-stu-id="08812-177">This is an example of a `MapRoute` call used by a typical ASP.NET MVC route definition:</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="08812-178">此模板将匹配类似 URL 路径`/Products/Details/17`和提取路由值`{ controller = Products, action = Details, id = 17 }`。</span><span class="sxs-lookup"><span data-stu-id="08812-178">This template will match a URL path like `/Products/Details/17` and extract the route values `{ controller = Products, action = Details, id = 17 }`.</span></span> <span data-ttu-id="08812-179">路由值确定通过将 URL 路径拆分为段，以及与每个段进行匹配*路由参数*路由模板中的名称。</span><span class="sxs-lookup"><span data-stu-id="08812-179">The route values are determined by splitting the URL path into segments, and matching each segment with the *route parameter* name in the route template.</span></span> <span data-ttu-id="08812-180">路由参数的名称。</span><span class="sxs-lookup"><span data-stu-id="08812-180">Route parameters are named.</span></span> <span data-ttu-id="08812-181">它们由括在大括号中的参数名称定义`{ }`。</span><span class="sxs-lookup"><span data-stu-id="08812-181">They are defined by enclosing the parameter name in braces `{ }`.</span></span>

<span data-ttu-id="08812-182">上述模板还无法与匹配的 URL 路径`/`，会产生值`{ controller = Home, action = Index }`。</span><span class="sxs-lookup"><span data-stu-id="08812-182">The template above could also match the URL path `/` and would produce the values `{ controller = Home, action = Index }`.</span></span> <span data-ttu-id="08812-183">这是因为`{controller}`和`{action}`路由参数具有默认值和`id`路由参数是可选的。</span><span class="sxs-lookup"><span data-stu-id="08812-183">This happens because the `{controller}` and `{action}` route parameters have default values, and the `id` route parameter is optional.</span></span> <span data-ttu-id="08812-184">等于`=`符号后路由参数名称定义参数的默认值后跟一个值。</span><span class="sxs-lookup"><span data-stu-id="08812-184">An equals `=` sign followed by a value after the route parameter name defines a default value for the parameter.</span></span> <span data-ttu-id="08812-185">问号`?`路由参数名称定义为可选参数之后。</span><span class="sxs-lookup"><span data-stu-id="08812-185">A question mark `?` after the route parameter name defines the parameter as optional.</span></span> <span data-ttu-id="08812-186">将路由使用的默认值的参数*始终*当路由与匹配-可选参数将不会生成一个路由值，如果没有相应的 URL 路径段时产生路由值。</span><span class="sxs-lookup"><span data-stu-id="08812-186">Route parameters with a default value *always* produce a route value when the route matches - optional parameters will not produce a route value if there was no corresponding URL path segment.</span></span>

<span data-ttu-id="08812-187">请参阅[路由模板引用](#route-template-reference)的路由模板功能和语法的详尽描述。</span><span class="sxs-lookup"><span data-stu-id="08812-187">See [route-template-reference](#route-template-reference) for a thorough description of route template features and syntax.</span></span>

<span data-ttu-id="08812-188">该示例包括*路由约束*:</span><span class="sxs-lookup"><span data-stu-id="08812-188">This example includes a *route constraint*:</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

<span data-ttu-id="08812-189">此模板将匹配类似 URL 路径`/Products/Details/17`，但不是`/Products/Details/Apples`。</span><span class="sxs-lookup"><span data-stu-id="08812-189">This template will match a URL path like `/Products/Details/17`, but not `/Products/Details/Apples`.</span></span> <span data-ttu-id="08812-190">路由参数定义`{id:int}`定义*路由约束*为`id`路由参数。</span><span class="sxs-lookup"><span data-stu-id="08812-190">The route parameter definition `{id:int}` defines a *route constraint* for the `id` route parameter.</span></span> <span data-ttu-id="08812-191">路由约束实现`IRouteConstraint`并检查路由以验证它们的值。</span><span class="sxs-lookup"><span data-stu-id="08812-191">Route constraints implement `IRouteConstraint` and inspect route values to verify them.</span></span> <span data-ttu-id="08812-192">在此示例中的路由值`id`必须可转换为整数。</span><span class="sxs-lookup"><span data-stu-id="08812-192">In this example the route value `id` must be convertible to an integer.</span></span> <span data-ttu-id="08812-193">请参阅[路由约束引用](#route-constraint-reference)有关 framework 提供的路由约束的更多详细说明。</span><span class="sxs-lookup"><span data-stu-id="08812-193">See [route-constraint-reference](#route-constraint-reference) for a more detailed explanation of route constraints that are provided by the framework.</span></span>

<span data-ttu-id="08812-194">其他重载`MapRoute`接受值`constraints`， `dataTokens`，和`defaults`。</span><span class="sxs-lookup"><span data-stu-id="08812-194">Additional overloads of `MapRoute` accept values for `constraints`, `dataTokens`, and `defaults`.</span></span> <span data-ttu-id="08812-195">这些附加参数的`MapRoute`定义为类型`object`。</span><span class="sxs-lookup"><span data-stu-id="08812-195">These additional parameters of `MapRoute` are defined as type `object`.</span></span> <span data-ttu-id="08812-196">这些参数的典型用法是传递的匿名类型化的对象，其中的匿名类型匹配的属性名称将参数名称的路由。</span><span class="sxs-lookup"><span data-stu-id="08812-196">The typical usage of these parameters is to pass an anonymously typed object, where the property names of the anonymous type match route parameter names.</span></span>

<span data-ttu-id="08812-197">下面的两个示例创建等效路由：</span><span class="sxs-lookup"><span data-stu-id="08812-197">The following two examples create equivalent routes:</span></span>

```csharp
routes.MapRoute(
    name: "default_route",
    template: "{controller}/{action}/{id?}",
    defaults: new { controller = "Home", action = "Index" });

routes.MapRoute(
    name: "default_route",
    template: "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="08812-198">提示： 内联语法来定义约束和默认值可能会更方便简单路由。</span><span class="sxs-lookup"><span data-stu-id="08812-198">Tip: The inline syntax for defining constraints and defaults can be more convenient for simple routes.</span></span> <span data-ttu-id="08812-199">但是，存在一些功能如内联语法不支持数据令牌。</span><span class="sxs-lookup"><span data-stu-id="08812-199">However, there are features such as data tokens which are not supported by inline syntax.</span></span>

<span data-ttu-id="08812-200">此示例演示几个更多的功能：</span><span class="sxs-lookup"><span data-stu-id="08812-200">This example demonstrates a few more features:</span></span>

```csharp
routes.MapRoute(
  name: "blog",
  template: "Blog/{*article}",
  defaults: new { controller = "Blog", action = "ReadArticle" });
```

<span data-ttu-id="08812-201">此模板将匹配类似 URL 路径`/Blog/All-About-Routing/Introduction`和会提取值`{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`。</span><span class="sxs-lookup"><span data-stu-id="08812-201">This template will match a URL path like `/Blog/All-About-Routing/Introduction` and will extract the values `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`.</span></span> <span data-ttu-id="08812-202">默认路由值`controller`和`action`即使模板中没有相应的路由参数生成的路由。</span><span class="sxs-lookup"><span data-stu-id="08812-202">The default route values for `controller` and `action` are produced by the route even though there are no corresponding route parameters in the template.</span></span> <span data-ttu-id="08812-203">在路由模板中，可以指定默认值。</span><span class="sxs-lookup"><span data-stu-id="08812-203">Default values can be specified in the route template.</span></span> <span data-ttu-id="08812-204">`article`路由参数定义为*全方位*星号的外观由`*`之前路由参数名称。</span><span class="sxs-lookup"><span data-stu-id="08812-204">The `article` route parameter is defined as a *catch-all* by the appearance of an asterisk `*` before the route parameter name.</span></span> <span data-ttu-id="08812-205">全部捕捉路由参数捕获的 URL 路径的其余部分，还可以匹配空字符串。</span><span class="sxs-lookup"><span data-stu-id="08812-205">Catch-all route parameters capture the remainder of the URL path, and can also match the empty string.</span></span>

<span data-ttu-id="08812-206">此示例将添加路由约束和数据令牌：</span><span class="sxs-lookup"><span data-stu-id="08812-206">This example adds route constraints and data tokens:</span></span>

```csharp
routes.MapRoute(
    name: "us_english_products",
    template: "en-US/Products/{id}",
    defaults: new { controller = "Products", action = "Details" },
    constraints: new { id = new IntRouteConstraint() },
    dataTokens: new { locale = "en-US" });
```

<span data-ttu-id="08812-207">此模板将匹配类似 URL 路径`/Products/5`和会提取值`{ controller = Products, action = Details, id = 5 }`和数据令牌`{ locale = en-US }`。</span><span class="sxs-lookup"><span data-stu-id="08812-207">This template will match a URL path like `/Products/5` and will extract the values `{ controller = Products, action = Details, id = 5 }` and the data tokens `{ locale = en-US }`.</span></span>

![局部变量 Windows 令牌](routing/_static/tokens.png)

<a name="id1"></a>

### <a name="url-generation"></a><span data-ttu-id="08812-209">URL 生成</span><span class="sxs-lookup"><span data-stu-id="08812-209">URL generation</span></span>

<span data-ttu-id="08812-210">`Route`类还可以通过将其路由模板与结合使用的一组路由值执行 URL 生成。</span><span class="sxs-lookup"><span data-stu-id="08812-210">The `Route` class can also perform URL generation by combining a set of route values with its route template.</span></span> <span data-ttu-id="08812-211">此逻辑上是匹配的 URL 路径的反向过程。</span><span class="sxs-lookup"><span data-stu-id="08812-211">This is logically the reverse process of matching the URL path.</span></span>

<span data-ttu-id="08812-212">提示： 若要更好地了解 URL 生成，假设你想要生成，然后考虑如何路由模板将匹配该 URL 哪些的 URL。</span><span class="sxs-lookup"><span data-stu-id="08812-212">Tip: To better understand URL generation, imagine what URL you want to generate and then think about how a route template would match that URL.</span></span> <span data-ttu-id="08812-213">将生成值？</span><span class="sxs-lookup"><span data-stu-id="08812-213">What values would be produced?</span></span> <span data-ttu-id="08812-214">这是大致相当于 URL 生成中的工作方式`Route`类。</span><span class="sxs-lookup"><span data-stu-id="08812-214">This is the rough equivalent of how URL generation works in the `Route` class.</span></span>

<span data-ttu-id="08812-215">此示例使用一个基本的 ASP.NET MVC 样式路由：</span><span class="sxs-lookup"><span data-stu-id="08812-215">This example uses a basic ASP.NET MVC style route:</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="08812-216">使用路由值`{ controller = Products, action = List }`，此路由将生成 URL `/Products/List`。</span><span class="sxs-lookup"><span data-stu-id="08812-216">With the route values `{ controller = Products, action = List }`, this route will generate the URL `/Products/List`.</span></span> <span data-ttu-id="08812-217">路由值将替换为相应的路由参数，以形成的 URL 路径的。</span><span class="sxs-lookup"><span data-stu-id="08812-217">The route values are substituted for the corresponding route parameters to form the URL path.</span></span> <span data-ttu-id="08812-218">由于`id`是可选的路由参数，则它不具有值没问题。</span><span class="sxs-lookup"><span data-stu-id="08812-218">Since `id` is an optional route parameter, it's no problem that it doesn't have a value.</span></span>

<span data-ttu-id="08812-219">使用路由值`{ controller = Home, action = Index }`，此路由将生成 URL `/`。</span><span class="sxs-lookup"><span data-stu-id="08812-219">With the route values `{ controller = Home, action = Index }`, this route will generate the URL `/`.</span></span> <span data-ttu-id="08812-220">路由值提供匹配的默认值，因此可以安全地忽略这些值所对应的段。</span><span class="sxs-lookup"><span data-stu-id="08812-220">The route values that were provided match the default values so the segments corresponding to those values can be safely omitted.</span></span> <span data-ttu-id="08812-221">请注意，生成两个 Url 将与此路由定义往返，生成用于生成 URL 的路由相同的值。</span><span class="sxs-lookup"><span data-stu-id="08812-221">Note that both URLs generated would round-trip with this route definition and produce the same route values that were used to generate the URL.</span></span>

<span data-ttu-id="08812-222">提示： 使用 ASP.NET MVC 应用程序应使用`UrlHelper`生成 Url 而不是直接路由到的调用。</span><span class="sxs-lookup"><span data-stu-id="08812-222">Tip: An app using ASP.NET MVC should use `UrlHelper` to generate URLs instead of calling into routing directly.</span></span>

<span data-ttu-id="08812-223">有关 URL 生成过程的详细信息，请参阅[url 生成引用](#url-generation-reference)。</span><span class="sxs-lookup"><span data-stu-id="08812-223">For more details about the URL generation process, see [url-generation-reference](#url-generation-reference).</span></span>

## <a name="using-routing-middleware"></a><span data-ttu-id="08812-224">使用路由的中间件</span><span class="sxs-lookup"><span data-stu-id="08812-224">Using Routing Middleware</span></span>

<span data-ttu-id="08812-225">添加 NuGet 包"Microsoft.AspNetCore.Routing"。</span><span class="sxs-lookup"><span data-stu-id="08812-225">Add the NuGet package "Microsoft.AspNetCore.Routing".</span></span>

<span data-ttu-id="08812-226">添加路由到的服务容器中*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="08812-226">Add routing to the service container in *Startup.cs*:</span></span>

[!code-csharp[Main](../fundamentals/routing/sample/RoutingSample/Startup.cs?highlight=3&start=11&end=14)]

<span data-ttu-id="08812-227">必须在配置路由`Configure`中的方法`Startup`类。</span><span class="sxs-lookup"><span data-stu-id="08812-227">Routes must be configured in the `Configure` method in the `Startup` class.</span></span> <span data-ttu-id="08812-228">下面的示例使用这些 Api:</span><span class="sxs-lookup"><span data-stu-id="08812-228">The sample below uses these APIs:</span></span>

* `RouteBuilder`
* `Build`
* <span data-ttu-id="08812-229">`MapGet`匹配仅 HTTP GET 请求</span><span class="sxs-lookup"><span data-stu-id="08812-229">`MapGet`  Matches only HTTP GET requests</span></span>
* `UseRouter`

```csharp
public void Configure(IApplicationBuilder app, ILoggerFactory loggerFactory)
{
    var trackPackageRouteHandler = new RouteHandler(context =>
    {
        var routeValues = context.GetRouteData().Values;
        return context.Response.WriteAsync(
            $"Hello! Route values: {string.Join(", ", routeValues)}");
    });

    var routeBuilder = new RouteBuilder(app, trackPackageRouteHandler);

    routeBuilder.MapRoute(
        "Track Package Route",
        "package/{operation:regex(^(track|create|detonate)$)}/{id:int}");

    routeBuilder.MapGet("hello/{name}", context =>
    {
        var name = context.GetRouteValue("name");
        // This is the route handler when HTTP GET "hello/<anything>"  matches
        // To match HTTP GET "hello/<anything>/<anything>,
        // use routeBuilder.MapGet("hello/{*name}"
        return context.Response.WriteAsync($"Hi, {name}!");
    });

    var routes = routeBuilder.Build();
    app.UseRouter(routes);
}
```

<span data-ttu-id="08812-230">下表显示了具有给定的 Uri 的响应。</span><span class="sxs-lookup"><span data-stu-id="08812-230">The table below shows the responses with the given URIs.</span></span>

| <span data-ttu-id="08812-231">URI</span><span class="sxs-lookup"><span data-stu-id="08812-231">URI</span></span> | <span data-ttu-id="08812-232">响应</span><span class="sxs-lookup"><span data-stu-id="08812-232">Response</span></span>  |
| ------- | -------- |
| <span data-ttu-id="08812-233">/package/create/3</span><span class="sxs-lookup"><span data-stu-id="08812-233">/package/create/3</span></span>  | <span data-ttu-id="08812-234">Hello!</span><span class="sxs-lookup"><span data-stu-id="08812-234">Hello!</span></span> <span data-ttu-id="08812-235">将路由值: [操作，创建]，[id，3]</span><span class="sxs-lookup"><span data-stu-id="08812-235">Route values: [operation, create], [id, 3]</span></span> |
| <span data-ttu-id="08812-236">/ 包/跟踪/3</span><span class="sxs-lookup"><span data-stu-id="08812-236">/package/track/-3</span></span>  | <span data-ttu-id="08812-237">Hello!</span><span class="sxs-lookup"><span data-stu-id="08812-237">Hello!</span></span> <span data-ttu-id="08812-238">将路由值: [操作，跟踪]，[id，-3]</span><span class="sxs-lookup"><span data-stu-id="08812-238">Route values: [operation, track], [id, -3]</span></span> |
| <span data-ttu-id="08812-239">/ 包/跟踪/3 /</span><span class="sxs-lookup"><span data-stu-id="08812-239">/package/track/-3/</span></span> | <span data-ttu-id="08812-240">Hello!</span><span class="sxs-lookup"><span data-stu-id="08812-240">Hello!</span></span> <span data-ttu-id="08812-241">将路由值: [操作，跟踪]，[id，-3]</span><span class="sxs-lookup"><span data-stu-id="08812-241">Route values: [operation, track], [id, -3]</span></span>  |
| <span data-ttu-id="08812-242">/package/跟踪 /</span><span class="sxs-lookup"><span data-stu-id="08812-242">/package/track/</span></span> | <span data-ttu-id="08812-243">\<贯穿到任何匹配项 ></span><span class="sxs-lookup"><span data-stu-id="08812-243">\<Fall through, no match></span></span> |
| <span data-ttu-id="08812-244">获取 /hello/Joe</span><span class="sxs-lookup"><span data-stu-id="08812-244">GET /hello/Joe</span></span> | <span data-ttu-id="08812-245">您好，Joe ！</span><span class="sxs-lookup"><span data-stu-id="08812-245">Hi, Joe!</span></span> |
| <span data-ttu-id="08812-246">POST /hello/Joe</span><span class="sxs-lookup"><span data-stu-id="08812-246">POST /hello/Joe</span></span> | <span data-ttu-id="08812-247">\<贯穿，仅匹配 HTTP GET ></span><span class="sxs-lookup"><span data-stu-id="08812-247">\<Fall through, matches HTTP GET only></span></span> |
| <span data-ttu-id="08812-248">获取 /hello/Joe/Smith</span><span class="sxs-lookup"><span data-stu-id="08812-248">GET /hello/Joe/Smith</span></span> | <span data-ttu-id="08812-249">\<贯穿到任何匹配项 ></span><span class="sxs-lookup"><span data-stu-id="08812-249">\<Fall through, no match></span></span> |

<span data-ttu-id="08812-250">如果你要配置一个路由，调用`app.UseRouter`传入`IRouter`实例。</span><span class="sxs-lookup"><span data-stu-id="08812-250">If you are configuring a single route, call `app.UseRouter` passing in an `IRouter` instance.</span></span> <span data-ttu-id="08812-251">你无需调用`RouteBuilder`。</span><span class="sxs-lookup"><span data-stu-id="08812-251">You won't need to call `RouteBuilder`.</span></span>

<span data-ttu-id="08812-252">框架提供了一套例如创建的路由的扩展方法：</span><span class="sxs-lookup"><span data-stu-id="08812-252">The framework provides a set of extension methods for creating routes such as:</span></span>

* `MapRoute`
* `MapGet`
* `MapPost`
* `MapPut`
* `MapDelete`
* `MapVerb`

<span data-ttu-id="08812-253">如这些方法的一些`MapGet`需要`RequestDelegate`提供。</span><span class="sxs-lookup"><span data-stu-id="08812-253">Some of these methods such as `MapGet` require a `RequestDelegate` to be provided.</span></span> <span data-ttu-id="08812-254">`RequestDelegate`将用作*路由处理程序*路由匹配时。</span><span class="sxs-lookup"><span data-stu-id="08812-254">The `RequestDelegate` will be used as the *route handler* when the route matches.</span></span> <span data-ttu-id="08812-255">此系列中的其他方法允许配置的中间件管道，后者将用作路由处理程序。</span><span class="sxs-lookup"><span data-stu-id="08812-255">Other methods in this family allow configuring a middleware pipeline which will be used as the route handler.</span></span> <span data-ttu-id="08812-256">如果*映射*方法不接受一个处理程序，例如`MapRoute`，则它将使用`DefaultHandler`。</span><span class="sxs-lookup"><span data-stu-id="08812-256">If the *Map* method doesn't accept a handler, such as `MapRoute`, then it will use the `DefaultHandler`.</span></span>

<span data-ttu-id="08812-257">`Map[Verb]`方法使用约束来限制到中的方法名称的 HTTP 谓词的路由。</span><span class="sxs-lookup"><span data-stu-id="08812-257">The `Map[Verb]` methods use constraints to limit the route to the HTTP Verb in the method name.</span></span> <span data-ttu-id="08812-258">有关示例，请参阅[MapGet](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/RequestDelegateRouteBuilderExtensions.cs#L85-L88)和[MapVerb](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/RequestDelegateRouteBuilderExtensions.cs#L156-L180)。</span><span class="sxs-lookup"><span data-stu-id="08812-258">For example, see [MapGet](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/RequestDelegateRouteBuilderExtensions.cs#L85-L88) and [MapVerb](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/RequestDelegateRouteBuilderExtensions.cs#L156-L180).</span></span>

## <a name="route-template-reference"></a><span data-ttu-id="08812-259">路由模板参考</span><span class="sxs-lookup"><span data-stu-id="08812-259">Route Template Reference</span></span>

<span data-ttu-id="08812-260">在大括号内的令牌 (`{ }`) 定义*路由参数*后者将绑定当找到匹配项路由。</span><span class="sxs-lookup"><span data-stu-id="08812-260">Tokens within curly braces (`{ }`) define *route parameters* which will be bound if the route is matched.</span></span> <span data-ttu-id="08812-261">你可以定义多个路由参数路由段中，但它们必须按某一文字值分隔。</span><span class="sxs-lookup"><span data-stu-id="08812-261">You can define more than one route parameter in a route segment, but they must be separated by a literal value.</span></span> <span data-ttu-id="08812-262">例如`{controller=Home}{action=Index}`将不是有效的路由，因为没有文本值之间`{controller}`和`{action}`。</span><span class="sxs-lookup"><span data-stu-id="08812-262">For example `{controller=Home}{action=Index}` would not be a valid route, since there is no literal value between `{controller}` and `{action}`.</span></span> <span data-ttu-id="08812-263">这些路由参数必须具有一个名称，并且可以指定其他属性。</span><span class="sxs-lookup"><span data-stu-id="08812-263">These route parameters must have a name, and may have additional attributes specified.</span></span>

<span data-ttu-id="08812-264">路由参数以外的文字文本 (例如， `{id}`) 和路径分隔符`/`必须匹配在 URL 中的文本。</span><span class="sxs-lookup"><span data-stu-id="08812-264">Literal text other than route parameters (for example, `{id}`) and the path separator `/` must match the text in the URL.</span></span> <span data-ttu-id="08812-265">文本匹配是不区分大小写并且基于的已解码的表示形式的 Url 路径。</span><span class="sxs-lookup"><span data-stu-id="08812-265">Text matching is case-insensitive and based on the decoded representation of the URLs path.</span></span> <span data-ttu-id="08812-266">若要匹配的文本的路由参数分隔符`{`或`}`，通过重复字符对其进行转义 (`{{`或`}}`)。</span><span class="sxs-lookup"><span data-stu-id="08812-266">To match the literal route parameter delimiter `{` or  `}`, escape it by repeating the character (`{{` or `}}`).</span></span>

<span data-ttu-id="08812-267">来尝试捕获带可选文件扩展名的文件名的 URL 模式有其他注意事项。</span><span class="sxs-lookup"><span data-stu-id="08812-267">URL patterns that attempt to capture a filename with an optional file extension have additional considerations.</span></span> <span data-ttu-id="08812-268">例如，使用模板`files/{filename}.{ext?}`-当同时`filename`和`ext`存在，则将填充这两个值。</span><span class="sxs-lookup"><span data-stu-id="08812-268">For example, using the template `files/{filename}.{ext?}` - When both `filename` and `ext` exist, both values will be populated.</span></span> <span data-ttu-id="08812-269">如果仅`filename`因为 URL 路由匹配项中存在尾随句点`.`是可选的。</span><span class="sxs-lookup"><span data-stu-id="08812-269">If only `filename` exists in the URL, the route matches because the trailing period `.` is  optional.</span></span> <span data-ttu-id="08812-270">以下 Url 将与此路由匹配：</span><span class="sxs-lookup"><span data-stu-id="08812-270">The following URLs would match this route:</span></span>

* `/files/myFile.txt`
* `/files/myFile.`
* `/files/myFile`

<span data-ttu-id="08812-271">你可以使用`*`作为前缀字符到路由参数以绑定到的 URI 的 rest 调用此*全方位*参数。</span><span class="sxs-lookup"><span data-stu-id="08812-271">You can use the `*` character as a prefix to a route parameter to bind to the rest of the URI - this is called a *catch-all* parameter.</span></span> <span data-ttu-id="08812-272">例如，`blog/{*slug}`将匹配入门任何 URI`/blog`且必须跟在它后面的任何值 (其将分配到`slug`路由值)。</span><span class="sxs-lookup"><span data-stu-id="08812-272">For example, `blog/{*slug}` would match any URI that started with `/blog` and had any value following it (which would be assigned to the `slug` route value).</span></span> <span data-ttu-id="08812-273">全部捕获参数还可以匹配空字符串。</span><span class="sxs-lookup"><span data-stu-id="08812-273">Catch-all parameters can also match the empty string.</span></span>

<span data-ttu-id="08812-274">路由参数可能具有*默认值*、 通过隔开的参数名称后指定默认值指定`=`。</span><span class="sxs-lookup"><span data-stu-id="08812-274">Route parameters may have *default values*, designated by specifying the default after the parameter name, separated by an `=`.</span></span> <span data-ttu-id="08812-275">例如，`{controller=Home}`将定义`Home`的默认值为`controller`。</span><span class="sxs-lookup"><span data-stu-id="08812-275">For example, `{controller=Home}` would define `Home` as the default value for `controller`.</span></span> <span data-ttu-id="08812-276">如果没有值存在在 URL 中的参数，则使用默认值。</span><span class="sxs-lookup"><span data-stu-id="08812-276">The default value is used if no value is present in the URL for the parameter.</span></span> <span data-ttu-id="08812-277">默认值，除了路由参数可能是可选的 (指定将追加`?`到参数名称，如下所示的末尾`id?`)。</span><span class="sxs-lookup"><span data-stu-id="08812-277">In addition to default values, route parameters may be optional (specified by appending a `?` to the end of the parameter name, as in `id?`).</span></span> <span data-ttu-id="08812-278">可选之间的差异和"具有默认"是默认值的路由参数始终会生成一个值; 如果仅在一个提供时，一个可选参数将具有的值。</span><span class="sxs-lookup"><span data-stu-id="08812-278">The difference between optional and "has default" is that a route parameter with a default value always produces a value; an optional parameter has a value only when one is provided.</span></span>

<span data-ttu-id="08812-279">路由参数也可能具有必须与绑定从 URL 路由值匹配的约束。</span><span class="sxs-lookup"><span data-stu-id="08812-279">Route parameters may also have constraints, which must match the route value bound from the URL.</span></span> <span data-ttu-id="08812-280">添加冒号`:`和约束名称后的路由参数名称指定*内联约束*路由参数。</span><span class="sxs-lookup"><span data-stu-id="08812-280">Adding a colon `:` and constraint name after the route parameter name specifies an *inline constraint* on a route parameter.</span></span> <span data-ttu-id="08812-281">如果约束需要那些提供括在括号中的参数`( )`约束名称后面。</span><span class="sxs-lookup"><span data-stu-id="08812-281">If the constraint requires arguments those are provided enclosed in parentheses `( )` after the constraint name.</span></span> <span data-ttu-id="08812-282">要指定多个内联约束，只需将追加另一个冒号`:`和约束名称。</span><span class="sxs-lookup"><span data-stu-id="08812-282">Multiple inline constraints can be specified by appending another colon `:` and constraint name.</span></span> <span data-ttu-id="08812-283">约束名称传递给`IInlineConstraintResolver`服务创建的实例`IRouteConstraint`要在 URL 处理中使用。</span><span class="sxs-lookup"><span data-stu-id="08812-283">The constraint name is passed to the `IInlineConstraintResolver` service to create an instance of `IRouteConstraint` to use in URL processing.</span></span> <span data-ttu-id="08812-284">例如，路由模板`blog/{article:minlength(10)}`指定`minlength`使用参数的约束`10`。</span><span class="sxs-lookup"><span data-stu-id="08812-284">For example, the route template `blog/{article:minlength(10)}` specifies the `minlength` constraint with the argument `10`.</span></span> <span data-ttu-id="08812-285">详细说明路由约束和由框架提供的约束的列表，请参阅[路由约束引用](#route-constraint-reference)。</span><span class="sxs-lookup"><span data-stu-id="08812-285">For more description route constraints, and a listing of the constraints provided by the framework, see [route-constraint-reference](#route-constraint-reference).</span></span>

<span data-ttu-id="08812-286">下表演示某些路由模板和它们的行为。</span><span class="sxs-lookup"><span data-stu-id="08812-286">The following table demonstrates some route templates and their behavior.</span></span>

| <span data-ttu-id="08812-287">路由模板</span><span class="sxs-lookup"><span data-stu-id="08812-287">Route Template</span></span> | <span data-ttu-id="08812-288">示例匹配 URL</span><span class="sxs-lookup"><span data-stu-id="08812-288">Example Matching URL</span></span> | <span data-ttu-id="08812-289">说明</span><span class="sxs-lookup"><span data-stu-id="08812-289">Notes</span></span> |
| -------- | -------- | ------- |
| <span data-ttu-id="08812-290">hello</span><span class="sxs-lookup"><span data-stu-id="08812-290">hello</span></span>  | <span data-ttu-id="08812-291">/hello</span><span class="sxs-lookup"><span data-stu-id="08812-291">/hello</span></span>  | <span data-ttu-id="08812-292">仅与单个路径相匹配`/hello`</span><span class="sxs-lookup"><span data-stu-id="08812-292">Only matches the single path `/hello`</span></span> |
| <span data-ttu-id="08812-293">{页主页 =}</span><span class="sxs-lookup"><span data-stu-id="08812-293">{Page=Home}</span></span> | / | <span data-ttu-id="08812-294">匹配和设置`Page`到`Home`</span><span class="sxs-lookup"><span data-stu-id="08812-294">Matches and sets `Page` to `Home`</span></span> |
| <span data-ttu-id="08812-295">{页主页 =}</span><span class="sxs-lookup"><span data-stu-id="08812-295">{Page=Home}</span></span>  | <span data-ttu-id="08812-296">/ 联系人</span><span class="sxs-lookup"><span data-stu-id="08812-296">/Contact</span></span>  | <span data-ttu-id="08812-297">匹配和设置`Page`到`Contact`</span><span class="sxs-lookup"><span data-stu-id="08812-297">Matches and sets `Page` to `Contact`</span></span> |
| <span data-ttu-id="08812-298">{controller} / {action} / {id}？</span><span class="sxs-lookup"><span data-stu-id="08812-298">{controller}/{action}/{id?}</span></span> | <span data-ttu-id="08812-299">/ 产品/列表</span><span class="sxs-lookup"><span data-stu-id="08812-299">/Products/List</span></span> | <span data-ttu-id="08812-300">映射到`Products`控制器和`List`操作</span><span class="sxs-lookup"><span data-stu-id="08812-300">Maps to `Products` controller and `List`  action</span></span> |
| <span data-ttu-id="08812-301">{controller} / {action} / {id}？</span><span class="sxs-lookup"><span data-stu-id="08812-301">{controller}/{action}/{id?}</span></span> | <span data-ttu-id="08812-302">/ 产品/详细信息/123</span><span class="sxs-lookup"><span data-stu-id="08812-302">/Products/Details/123</span></span>  |  <span data-ttu-id="08812-303">映射到`Products`控制器和`Details`操作。</span><span class="sxs-lookup"><span data-stu-id="08812-303">Maps to `Products` controller and  `Details` action.</span></span>  <span data-ttu-id="08812-304">`id`设置为 123</span><span class="sxs-lookup"><span data-stu-id="08812-304">`id` set to 123</span></span> |
| <span data-ttu-id="08812-305">{控制器主页 =} / {操作 = 索引} / {id}？</span><span class="sxs-lookup"><span data-stu-id="08812-305">{controller=Home}/{action=Index}/{id?}</span></span> | /  |  <span data-ttu-id="08812-306">映射到`Home`控制器和`Index`方法;`id`将被忽略。</span><span class="sxs-lookup"><span data-stu-id="08812-306">Maps to `Home` controller and `Index`  method; `id` is ignored.</span></span> |

<span data-ttu-id="08812-307">使用模板通常是路由的最简单方法。</span><span class="sxs-lookup"><span data-stu-id="08812-307">Using a template is generally the simplest approach to routing.</span></span> <span data-ttu-id="08812-308">此外可以在路由模板外指定约束和默认值。</span><span class="sxs-lookup"><span data-stu-id="08812-308">Constraints and defaults can also be specified outside the route template.</span></span>

<span data-ttu-id="08812-309">提示： 启用[日志记录](logging.md)若要查看如何在路由实现中，如生成`Route`，匹配的请求。</span><span class="sxs-lookup"><span data-stu-id="08812-309">Tip: Enable [Logging](logging.md) to see how the built in routing implementations, such as `Route`, match requests.</span></span>

## <a name="route-constraint-reference"></a><span data-ttu-id="08812-310">路由约束引用</span><span class="sxs-lookup"><span data-stu-id="08812-310">Route Constraint Reference</span></span>

<span data-ttu-id="08812-311">路由约束时执行`Route`具有匹配传入 URL 的语法和标记化为路由值的 URL 路径。</span><span class="sxs-lookup"><span data-stu-id="08812-311">Route constraints execute when a `Route` has matched the syntax of the incoming URL and tokenized the URL path into route values.</span></span> <span data-ttu-id="08812-312">通常，路由约束检查通过路由模板关联的路由值，并且进行简单是/否决策有关的值是可接受。</span><span class="sxs-lookup"><span data-stu-id="08812-312">Route constraints generally inspect the route value associated via the route template and make a simple yes/no decision about whether or not the value is acceptable.</span></span> <span data-ttu-id="08812-313">某些路由约束使用的路由值之外的数据要考虑是否可以路由请求。</span><span class="sxs-lookup"><span data-stu-id="08812-313">Some route constraints use data outside the route value to consider whether the request can be routed.</span></span> <span data-ttu-id="08812-314">例如，`HttpMethodRouteConstraint`可以接受或拒绝基于其 HTTP 谓词的请求。</span><span class="sxs-lookup"><span data-stu-id="08812-314">For example, the `HttpMethodRouteConstraint` can accept or reject a request based on its HTTP verb.</span></span>

>[!WARNING]
> <span data-ttu-id="08812-315">避免使用约束**输入验证**，因为这样做意味着该无效的输入将导致 404 （未找到） 而不是与相应的错误消息显示 400。</span><span class="sxs-lookup"><span data-stu-id="08812-315">Avoid using constraints for **input validation**, because doing so means that invalid input will result in a 404 (Not Found) instead of a 400 with an appropriate error message.</span></span> <span data-ttu-id="08812-316">路由约束应能用于对**消除歧义**之间相似的路由，无法验证为特定的路由的输入。</span><span class="sxs-lookup"><span data-stu-id="08812-316">Route constraints should be used to **disambiguate** between similar routes, not to validate the inputs for a particular route.</span></span>

<span data-ttu-id="08812-317">下表演示一些路由约束和其预期的行为。</span><span class="sxs-lookup"><span data-stu-id="08812-317">The following table demonstrates some route constraints and their expected behavior.</span></span>

| <span data-ttu-id="08812-318">约束</span><span class="sxs-lookup"><span data-stu-id="08812-318">constraint</span></span> | <span data-ttu-id="08812-319">示例</span><span class="sxs-lookup"><span data-stu-id="08812-319">Example</span></span> | <span data-ttu-id="08812-320">示例匹配项</span><span class="sxs-lookup"><span data-stu-id="08812-320">Example Matches</span></span> | <span data-ttu-id="08812-321">说明</span><span class="sxs-lookup"><span data-stu-id="08812-321">Notes</span></span> |
| --------   | ------- | ------------- | ----- |
| `int` | `{id:int}` | <span data-ttu-id="08812-322">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="08812-322">`123456789`, `-123456789`</span></span>  | <span data-ttu-id="08812-323">任何整数匹配</span><span class="sxs-lookup"><span data-stu-id="08812-323">Matches any integer</span></span> |
| `bool`  | `{active:bool}` | <span data-ttu-id="08812-324">`true`, `FALSE`</span><span class="sxs-lookup"><span data-stu-id="08812-324">`true`, `FALSE`</span></span> | <span data-ttu-id="08812-325">匹配`true`或`false`（不区分大小写）</span><span class="sxs-lookup"><span data-stu-id="08812-325">Matches `true` or `false` (case-insensitive)</span></span> |
| `datetime` | `{dob:datetime}` | <span data-ttu-id="08812-326">`2016-12-31`, `2016-12-31 7:32pm`</span><span class="sxs-lookup"><span data-stu-id="08812-326">`2016-12-31`, `2016-12-31 7:32pm`</span></span>  | <span data-ttu-id="08812-327">匹配有效`DateTime`值 （在固定区域性的看到警告）</span><span class="sxs-lookup"><span data-stu-id="08812-327">Matches a valid `DateTime` value (in the invariant culture - see warning)</span></span> |
| `decimal` | `{price:decimal}` | <span data-ttu-id="08812-328">`49.99`, `-1,000.01`</span><span class="sxs-lookup"><span data-stu-id="08812-328">`49.99`, `-1,000.01`</span></span> | <span data-ttu-id="08812-329">匹配有效`decimal`值 （在固定区域性的看到警告）</span><span class="sxs-lookup"><span data-stu-id="08812-329">Matches a valid `decimal` value (in the invariant culture - see warning)</span></span> |
| `double`  | `{weight:double}` | <span data-ttu-id="08812-330">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="08812-330">`1.234`, `-1,001.01e8`</span></span> | <span data-ttu-id="08812-331">匹配有效`double`值 （在固定区域性的看到警告）</span><span class="sxs-lookup"><span data-stu-id="08812-331">Matches a valid `double` value (in the invariant culture - see warning)</span></span> |
| `float`  | `{weight:float}` | <span data-ttu-id="08812-332">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="08812-332">`1.234`, `-1,001.01e8`</span></span> | <span data-ttu-id="08812-333">匹配有效`float`值 （在固定区域性的看到警告）</span><span class="sxs-lookup"><span data-stu-id="08812-333">Matches a valid `float` value (in the invariant culture - see warning)</span></span> |
| `guid`  | `{id:guid}` | <span data-ttu-id="08812-334">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span><span class="sxs-lookup"><span data-stu-id="08812-334">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span></span> | <span data-ttu-id="08812-335">匹配有效`Guid`值</span><span class="sxs-lookup"><span data-stu-id="08812-335">Matches a valid `Guid` value</span></span> |
| `long` | `{ticks:long}` | <span data-ttu-id="08812-336">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="08812-336">`123456789`, `-123456789`</span></span> | <span data-ttu-id="08812-337">匹配有效`long`值</span><span class="sxs-lookup"><span data-stu-id="08812-337">Matches a valid `long` value</span></span> |
| `minlength(value)` | `{username:minlength(4)}` | `Rick` | <span data-ttu-id="08812-338">字符串必须至少 4 个字符</span><span class="sxs-lookup"><span data-stu-id="08812-338">String must be at least 4 characters</span></span> |
| `maxlength(value)` | `{filename:maxlength(8)}` | `Richard` | <span data-ttu-id="08812-339">字符串必须是不能超过 8 个字符</span><span class="sxs-lookup"><span data-stu-id="08812-339">String must be no more than 8 characters</span></span> |
| `length(length)` | `{filename:length(12)}` | `somefile.txt` | <span data-ttu-id="08812-340">字符串必须是完全 12 个字符之间</span><span class="sxs-lookup"><span data-stu-id="08812-340">String must be exactly 12 characters long</span></span> |
| `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | <span data-ttu-id="08812-341">字符串必须至少 8 并且不超过 16 个字符</span><span class="sxs-lookup"><span data-stu-id="08812-341">String must be at least 8 and no more than 16 characters long</span></span> |
| `min(value)` | `{age:min(18)}` | `19` | <span data-ttu-id="08812-342">整数值必须至少 18</span><span class="sxs-lookup"><span data-stu-id="08812-342">Integer value must be at least 18</span></span> |
| `max(value)` | `{age:max(120)}` |  `91` | <span data-ttu-id="08812-343">整数值必须不能超过 120</span><span class="sxs-lookup"><span data-stu-id="08812-343">Integer value must be no more than 120</span></span> |
| `range(min,max)` | `{age:range(18,120)}` | `91` | <span data-ttu-id="08812-344">整数值必须至少 18 但不是能超过 120</span><span class="sxs-lookup"><span data-stu-id="08812-344">Integer value must be at least 18 but no more than 120</span></span> |
| `alpha` | `{name:alpha}` | `Rick` | <span data-ttu-id="08812-345">字符串必须包含一个或多个字母字符 (`a`-`z`、 不区分大小写)</span><span class="sxs-lookup"><span data-stu-id="08812-345">String must consist of one or more alphabetical characters (`a`-`z`, case-insensitive)</span></span> |
| `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | <span data-ttu-id="08812-346">字符串必须匹配正则表达式 （参见提示有关定义正则表达式）</span><span class="sxs-lookup"><span data-stu-id="08812-346">String must match the regular expression (see tips about defining a regular expression)</span></span> |
| `required`  | `{name:required}` | `Rick` |  <span data-ttu-id="08812-347">用于强制非参数值在 URL 生成过程中存在</span><span class="sxs-lookup"><span data-stu-id="08812-347">Used to enforce that a non-parameter value is present during URL generation</span></span> |

>[!WARNING]
> <span data-ttu-id="08812-348">验证的 URL 的路由约束可以转换为 CLR 类型 (如`int`或`DateTime`) 始终使用固定区域性-它们采用的 URL 是不可本地化。</span><span class="sxs-lookup"><span data-stu-id="08812-348">Route constraints that verify the URL can be converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture - they assume the URL is non-localizable.</span></span> <span data-ttu-id="08812-349">框架提供的路由约束不要修改存储在路由值的值。</span><span class="sxs-lookup"><span data-stu-id="08812-349">The framework-provided route constraints do not modify the values stored in route values.</span></span> <span data-ttu-id="08812-350">从 URL 解析所有路由值将都存储为字符串。</span><span class="sxs-lookup"><span data-stu-id="08812-350">All route values parsed from the URL will be stored as strings.</span></span> <span data-ttu-id="08812-351">例如， [Float 路由约束](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/Constraints/FloatRouteConstraint.cs#L44-L60)将尝试将路由值转换为浮点值，但转换后的值仅用于验证它可以转换为浮点数。</span><span class="sxs-lookup"><span data-stu-id="08812-351">For example, the [Float route constraint](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/Constraints/FloatRouteConstraint.cs#L44-L60) will attempt to convert the route value to a float, but the converted value is used only to verify it can be converted to a float.</span></span>

## <a name="regular-expressions"></a><span data-ttu-id="08812-352">正则表达式</span><span class="sxs-lookup"><span data-stu-id="08812-352">Regular expressions</span></span> 

<span data-ttu-id="08812-353">ASP.NET 核心框架就会将`RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant`到正则表达式构造函数。</span><span class="sxs-lookup"><span data-stu-id="08812-353">The ASP.NET Core framework adds `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` to the regular expression constructor.</span></span> <span data-ttu-id="08812-354">请参阅[RegexOptions 枚举](https://docs.microsoft.com/dotnet/api/system.text.regularexpressions.regexoptions)有关这些成员的说明。</span><span class="sxs-lookup"><span data-stu-id="08812-354">See [RegexOptions Enumeration](https://docs.microsoft.com/dotnet/api/system.text.regularexpressions.regexoptions) for a description of these members.</span></span>

<span data-ttu-id="08812-355">正则表达式使用分隔符和类似于所使用的路由和 C# 语言的令牌。</span><span class="sxs-lookup"><span data-stu-id="08812-355">Regular expressions use delimiters and tokens similar to those used by Routing and the C# language.</span></span> <span data-ttu-id="08812-356">正则表达式令牌必须进行转义。</span><span class="sxs-lookup"><span data-stu-id="08812-356">Regular expression tokens must be escaped.</span></span> <span data-ttu-id="08812-357">例如，若要使用的正则表达式`^\d{3}-\d{2}-\d{4}$`中路由，它需要用`\`以的键入的字符`\\`C# 源文件中进行转义`\`字符串转义字符 (除非使用[逐字字符串文本](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/string)。</span><span class="sxs-lookup"><span data-stu-id="08812-357">For example, to use the regular expression `^\d{3}-\d{2}-\d{4}$` in Routing, it needs to have the `\` characters typed in as `\\` in the C# source file to escape the `\` string escape character (unless using [verbatim string literals](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/string).</span></span> <span data-ttu-id="08812-358">`{` ， `}` ，[和] 字符是需要将它们路由参数分隔符字符进行转义加倍进行转义。</span><span class="sxs-lookup"><span data-stu-id="08812-358">The `{` , `}` , '[' and ']' characters need to be escaped by doubling them to escape the Routing parameter delimiter characters.</span></span>  <span data-ttu-id="08812-359">下表显示了正则表达式和转义的版本。</span><span class="sxs-lookup"><span data-stu-id="08812-359">The table below shows a regular expression and the escaped version.</span></span>

| <span data-ttu-id="08812-360">Expression</span><span class="sxs-lookup"><span data-stu-id="08812-360">Expression</span></span>               | <span data-ttu-id="08812-361">说明</span><span class="sxs-lookup"><span data-stu-id="08812-361">Note</span></span> |
| ----------------- | ------------ | 
| `^\d{3}-\d{2}-\d{4}$` | <span data-ttu-id="08812-362">正则表达式</span><span class="sxs-lookup"><span data-stu-id="08812-362">Regular expression</span></span> |
| `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` | <span data-ttu-id="08812-363">转义</span><span class="sxs-lookup"><span data-stu-id="08812-363">Escaped</span></span>  |
| `^[a-z]{2}$` | <span data-ttu-id="08812-364">正则表达式</span><span class="sxs-lookup"><span data-stu-id="08812-364">Regular expression</span></span> |
| `^[[a-z]]{{2}}$` | <span data-ttu-id="08812-365">转义</span><span class="sxs-lookup"><span data-stu-id="08812-365">Escaped</span></span>  |

<span data-ttu-id="08812-366">在路由中使用的正则表达式通常开头`^`字符 （匹配字符串的起始位置） 和以结尾`$`字符 （匹配结束的字符串的位置）。</span><span class="sxs-lookup"><span data-stu-id="08812-366">Regular expressions used in routing will often start with the `^` character (match starting position of the string) and end with the `$` character (match ending position of the string).</span></span> <span data-ttu-id="08812-367">`^`和`$`字符，请确保正则表达式匹配的整个路由参数值。</span><span class="sxs-lookup"><span data-stu-id="08812-367">The `^` and `$` characters ensure that the regular expression match the entire route parameter value.</span></span> <span data-ttu-id="08812-368">而无需`^`和`$`字符的正则表达式将匹配在字符串中，这通常不是你所希望的任何子字符串。</span><span class="sxs-lookup"><span data-stu-id="08812-368">Without the `^` and `$` characters the regular expression will match any sub-string within the string, which is often not what you want.</span></span> <span data-ttu-id="08812-369">下表显示了一些示例，并说明为何它们匹配，否则失败以匹配。</span><span class="sxs-lookup"><span data-stu-id="08812-369">The table below shows some examples and explains why they match or fail to match.</span></span>

| <span data-ttu-id="08812-370">Expression</span><span class="sxs-lookup"><span data-stu-id="08812-370">Expression</span></span>               | <span data-ttu-id="08812-371">字符串</span><span class="sxs-lookup"><span data-stu-id="08812-371">String</span></span> | <span data-ttu-id="08812-372">匹配</span><span class="sxs-lookup"><span data-stu-id="08812-372">Match</span></span> | <span data-ttu-id="08812-373">注释</span><span class="sxs-lookup"><span data-stu-id="08812-373">Comment</span></span> |
| ----------------- | ------------ |  ------------ |  ------------ | 
| `[a-z]{2}` | <span data-ttu-id="08812-374">hello</span><span class="sxs-lookup"><span data-stu-id="08812-374">hello</span></span> | <span data-ttu-id="08812-375">是</span><span class="sxs-lookup"><span data-stu-id="08812-375">yes</span></span> | <span data-ttu-id="08812-376">匹配的子字符串</span><span class="sxs-lookup"><span data-stu-id="08812-376">substring matches</span></span> |
| `[a-z]{2}` | <span data-ttu-id="08812-377">123abc456</span><span class="sxs-lookup"><span data-stu-id="08812-377">123abc456</span></span> | <span data-ttu-id="08812-378">是</span><span class="sxs-lookup"><span data-stu-id="08812-378">yes</span></span> | <span data-ttu-id="08812-379">匹配的子字符串</span><span class="sxs-lookup"><span data-stu-id="08812-379">substring matches</span></span> |
| `[a-z]{2}` | <span data-ttu-id="08812-380">mz</span><span class="sxs-lookup"><span data-stu-id="08812-380">mz</span></span> | <span data-ttu-id="08812-381">是</span><span class="sxs-lookup"><span data-stu-id="08812-381">yes</span></span> | <span data-ttu-id="08812-382">匹配表达式</span><span class="sxs-lookup"><span data-stu-id="08812-382">matches expression</span></span> |
| `[a-z]{2}` | <span data-ttu-id="08812-383">MZ</span><span class="sxs-lookup"><span data-stu-id="08812-383">MZ</span></span> | <span data-ttu-id="08812-384">是</span><span class="sxs-lookup"><span data-stu-id="08812-384">yes</span></span> | <span data-ttu-id="08812-385">不区分大小写</span><span class="sxs-lookup"><span data-stu-id="08812-385">not case sensitive</span></span> |
| `^[a-z]{2}$` |  <span data-ttu-id="08812-386">hello</span><span class="sxs-lookup"><span data-stu-id="08812-386">hello</span></span> | <span data-ttu-id="08812-387">no</span><span class="sxs-lookup"><span data-stu-id="08812-387">no</span></span> | <span data-ttu-id="08812-388">请参阅`^`和`$`上面</span><span class="sxs-lookup"><span data-stu-id="08812-388">see `^` and `$` above</span></span> |
| `^[a-z]{2}$` |  <span data-ttu-id="08812-389">123abc456</span><span class="sxs-lookup"><span data-stu-id="08812-389">123abc456</span></span> | <span data-ttu-id="08812-390">no</span><span class="sxs-lookup"><span data-stu-id="08812-390">no</span></span> | <span data-ttu-id="08812-391">请参阅`^`和`$`上面</span><span class="sxs-lookup"><span data-stu-id="08812-391">see `^` and `$` above</span></span> |

<span data-ttu-id="08812-392">请参阅[.NET Framework 正则表达式](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference)有关正则表达式语法的详细信息。</span><span class="sxs-lookup"><span data-stu-id="08812-392">Refer to [.NET Framework Regular Expressions](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference) for more information on regular expression syntax.</span></span>

<span data-ttu-id="08812-393">若要将限制为一组已知的可能的值的参数，使用正则表达式。</span><span class="sxs-lookup"><span data-stu-id="08812-393">To constrain a parameter to a known set of possible values, use a regular expression.</span></span> <span data-ttu-id="08812-394">例如`{action:regex(^(list|get|create)$)}`仅匹配`action`将路由到的值`list`， `get`，或`create`。</span><span class="sxs-lookup"><span data-stu-id="08812-394">For example `{action:regex(^(list|get|create)$)}` only matches the `action` route value to `list`, `get`, or `create`.</span></span> <span data-ttu-id="08812-395">如果传递到约束字典，字符串"^ (列表 | get | 创建) $"将等效。</span><span class="sxs-lookup"><span data-stu-id="08812-395">If passed into the constraints dictionary, the string "^(list|get|create)$" would be equivalent.</span></span> <span data-ttu-id="08812-396">将传入约束字典 （不在模板中的内联） 不匹配的已知的约束之一的约束也被视为正则表达式。</span><span class="sxs-lookup"><span data-stu-id="08812-396">Constraints that are passed in the constraints dictionary (not inline within a template) that don't match one of the known constraints are also treated as regular expressions.</span></span>

## <a name="url-generation-reference"></a><span data-ttu-id="08812-397">URL 生成引用</span><span class="sxs-lookup"><span data-stu-id="08812-397">URL Generation Reference</span></span>

<span data-ttu-id="08812-398">下面的示例演示如何生成给定路由值的字典的路由的链接和`RouteCollection`。</span><span class="sxs-lookup"><span data-stu-id="08812-398">The example below shows how to generate a link to a route given a dictionary of route values and a `RouteCollection`.</span></span>

[!code-csharp[Main](../fundamentals/routing/sample/RoutingSample/Startup.cs?range=45-59)]

<span data-ttu-id="08812-399">`VirtualPath`生成在上面的示例末尾`/package/create/123`。</span><span class="sxs-lookup"><span data-stu-id="08812-399">The `VirtualPath` generated at the end of the sample above is `/package/create/123`.</span></span>

<span data-ttu-id="08812-400">第二个参数`VirtualPathContext`构造函数是一套*环境值*。</span><span class="sxs-lookup"><span data-stu-id="08812-400">The second parameter to the `VirtualPathContext` constructor is a collection of *ambient values*.</span></span> <span data-ttu-id="08812-401">环境值通过限制一名开发人员必须在某些请求上下文中指定的值的数目提供方便。</span><span class="sxs-lookup"><span data-stu-id="08812-401">Ambient values provide convenience by limiting the number of values a developer must specify within a certain request context.</span></span> <span data-ttu-id="08812-402">当前请求的当前路由值被视为链接生成的环境的值。</span><span class="sxs-lookup"><span data-stu-id="08812-402">The current route values of the current request are considered ambient values for link generation.</span></span> <span data-ttu-id="08812-403">例如，在 ASP.NET MVC 应用程序，如果你在`About`操作`HomeController`，不需要指定要链接到的控制器路由值`Index`操作 (的环境值`Home`将使用)。</span><span class="sxs-lookup"><span data-stu-id="08812-403">For example, in an ASP.NET MVC app if you are in the `About` action of the `HomeController`, you don't need to specify the controller route value to link to the `Index` action (the ambient value of `Home` will be used).</span></span>

<span data-ttu-id="08812-404">环境不匹配参数的值将被忽略，和环境的值也将被忽略，当显式提供的值将重写它，将从左到右在 URL 中。</span><span class="sxs-lookup"><span data-stu-id="08812-404">Ambient values that don't match a parameter are ignored, and ambient values are also ignored when an explicitly-provided value overrides it, going from left to right in the URL.</span></span>

<span data-ttu-id="08812-405">显式提供，但不匹配的任何值将添加到查询字符串。</span><span class="sxs-lookup"><span data-stu-id="08812-405">Values that are explicitly provided but which don't match anything are added to the query string.</span></span> <span data-ttu-id="08812-406">下表显示结果时使用的路由模板`{controller}/{action}/{id?}`。</span><span class="sxs-lookup"><span data-stu-id="08812-406">The following table shows the result when using the route template `{controller}/{action}/{id?}`.</span></span>

| <span data-ttu-id="08812-407">环境值</span><span class="sxs-lookup"><span data-stu-id="08812-407">Ambient Values</span></span> | <span data-ttu-id="08812-408">显式值</span><span class="sxs-lookup"><span data-stu-id="08812-408">Explicit Values</span></span> | <span data-ttu-id="08812-409">结果</span><span class="sxs-lookup"><span data-stu-id="08812-409">Result</span></span> |
| -------------   | -------------- | ------ |
| <span data-ttu-id="08812-410">控制器 ="主页"</span><span class="sxs-lookup"><span data-stu-id="08812-410">controller="Home"</span></span> | <span data-ttu-id="08812-411">操作 ="关于"</span><span class="sxs-lookup"><span data-stu-id="08812-411">action="About"</span></span> | `/Home/About` |
| <span data-ttu-id="08812-412">控制器 ="主页"</span><span class="sxs-lookup"><span data-stu-id="08812-412">controller="Home"</span></span> | <span data-ttu-id="08812-413">控制器 ="Order"，操作 ="关于"</span><span class="sxs-lookup"><span data-stu-id="08812-413">controller="Order",action="About"</span></span> | `/Order/About` |
| <span data-ttu-id="08812-414">控制器 ="主页"，颜色 ="Red"</span><span class="sxs-lookup"><span data-stu-id="08812-414">controller="Home",color="Red"</span></span> | <span data-ttu-id="08812-415">操作 ="关于"</span><span class="sxs-lookup"><span data-stu-id="08812-415">action="About"</span></span> | `/Home/About` |
| <span data-ttu-id="08812-416">控制器 ="主页"</span><span class="sxs-lookup"><span data-stu-id="08812-416">controller="Home"</span></span> | <span data-ttu-id="08812-417">操作 ="关于"，颜色 ="Red"</span><span class="sxs-lookup"><span data-stu-id="08812-417">action="About",color="Red"</span></span> | `/Home/About?color=Red`

<span data-ttu-id="08812-418">如果路由不对应于参数的默认值而显式提供此值，它必须匹配的默认值。</span><span class="sxs-lookup"><span data-stu-id="08812-418">If a route has a default value that doesn't correspond to a parameter and that value is explicitly provided, it must match the default value.</span></span> <span data-ttu-id="08812-419">例如: </span><span class="sxs-lookup"><span data-stu-id="08812-419">For example:</span></span>

```csharp
routes.MapRoute("blog_route", "blog/{*slug}",
  defaults: new { controller = "Blog", action = "ReadPost" });
```

<span data-ttu-id="08812-420">提供控制器和操作的匹配值时，链接生成仅会生成此路由链接。</span><span class="sxs-lookup"><span data-stu-id="08812-420">Link generation would only generate a link for this route when the matching values for controller and action are provided.</span></span>
