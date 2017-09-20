---
title: "缓存 ASP.NET Core MVC 中的标记帮助器"
author: pkellner
description: "演示如何使用缓存标记帮助器"
keywords: "ASP.NET Core, 标记帮助程序"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: c045d485-d1dc-4cea-a675-46be83b7a012
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/CacheTagHelper
ms.openlocfilehash: 31c362a70e44c7178efe1c35b28a02fd712ac2b4
ms.sourcegitcommit: 74a8ad9c1ba5c155d7c4303e67632a0922c38e86
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/20/2017
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a><span data-ttu-id="1c7f9-104">缓存 ASP.NET Core MVC 中的标记帮助器</span><span class="sxs-lookup"><span data-stu-id="1c7f9-104">Cache Tag Helper in ASP.NET Core MVC</span></span>

<span data-ttu-id="1c7f9-105">作者：[Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="1c7f9-105">By [Peter Kellner](http://peterkellner.net)</span></span> 


<span data-ttu-id="1c7f9-106">缓存标记帮助器提供的功能可以显著提高通过缓存其内容与内部 ASP.NET Core 缓存提供程序的 ASP.NET Core 应用的性能。</span><span class="sxs-lookup"><span data-stu-id="1c7f9-106">The  Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to the internal ASP.NET Core cache provider.</span></span>

<span data-ttu-id="1c7f9-107">Razor 视图引擎设置的默认`expires-after`到 20 分钟。</span><span class="sxs-lookup"><span data-stu-id="1c7f9-107">The Razor View Engine sets the default `expires-after` to twenty minutes.</span></span>

<span data-ttu-id="1c7f9-108">以下 Razor 标记缓存日期/时间：</span><span class="sxs-lookup"><span data-stu-id="1c7f9-108">The following Razor markup caches the date/time:</span></span>

```cshtml
<Cache>@DateTime.Now<Cache>
```

<span data-ttu-id="1c7f9-109">对包含的页的第一个请求`CacheTagHelper`将显示当前日期/时间。</span><span class="sxs-lookup"><span data-stu-id="1c7f9-109">The first request to the page that contains `CacheTagHelper` will display the current date/time.</span></span> <span data-ttu-id="1c7f9-110">缓存过期 （默认值 20 分钟），或内存压力逐出之前，其他请求将显示缓存的值。</span><span class="sxs-lookup"><span data-stu-id="1c7f9-110">Additional requests will show the cached value until the cache expires (default 20 minutes) or is evicted by memory pressure.</span></span>

<span data-ttu-id="1c7f9-111">你可以设置具有以下属性将缓存持续时间：</span><span class="sxs-lookup"><span data-stu-id="1c7f9-111">You can set the cache duration with the following attributes:</span></span>

## <a name="cache-tag-helper-attributes"></a><span data-ttu-id="1c7f9-112">缓存标记帮助器属性</span><span class="sxs-lookup"><span data-stu-id="1c7f9-112">Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled"></a><span data-ttu-id="1c7f9-113">enabled</span><span class="sxs-lookup"><span data-stu-id="1c7f9-113">enabled</span></span>    


| <span data-ttu-id="1c7f9-114">特性类型</span><span class="sxs-lookup"><span data-stu-id="1c7f9-114">Attribute Type</span></span>    | <span data-ttu-id="1c7f9-115">有效值</span><span class="sxs-lookup"><span data-stu-id="1c7f9-115">Valid Values</span></span>      |
|----------------   |----------------   |
| <span data-ttu-id="1c7f9-116">boolean</span><span class="sxs-lookup"><span data-stu-id="1c7f9-116">boolean</span></span>           | <span data-ttu-id="1c7f9-117">"true"（默认值）</span><span class="sxs-lookup"><span data-stu-id="1c7f9-117">"true" (default)</span></span>  |
|                   | <span data-ttu-id="1c7f9-118">“false”</span><span class="sxs-lookup"><span data-stu-id="1c7f9-118">"false"</span></span>   |


<span data-ttu-id="1c7f9-119">确定是否缓存的缓存标记帮助程序括起来的内容。</span><span class="sxs-lookup"><span data-stu-id="1c7f9-119">Determines whether the content enclosed by the Cache Tag Helper is cached.</span></span> <span data-ttu-id="1c7f9-120">默认值为 `true`。</span><span class="sxs-lookup"><span data-stu-id="1c7f9-120">The default is `true`.</span></span>  <span data-ttu-id="1c7f9-121">如果设置为`false`此缓存标记帮助器将不起缓存上呈现的输出。</span><span class="sxs-lookup"><span data-stu-id="1c7f9-121">If set to `false` this Cache Tag Helper will have no caching effect on the rendered output.</span></span>

<span data-ttu-id="1c7f9-122">示例:</span><span class="sxs-lookup"><span data-stu-id="1c7f9-122">Example:</span></span>

```cshtml
<Cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="expires-on"></a><span data-ttu-id="1c7f9-123">过期上</span><span class="sxs-lookup"><span data-stu-id="1c7f9-123">expires-on</span></span> 

| <span data-ttu-id="1c7f9-124">特性类型</span><span class="sxs-lookup"><span data-stu-id="1c7f9-124">Attribute Type</span></span>    | <span data-ttu-id="1c7f9-125">示例值</span><span class="sxs-lookup"><span data-stu-id="1c7f9-125">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="1c7f9-126">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="1c7f9-126">DateTimeOffset</span></span>    | <span data-ttu-id="1c7f9-127">"@new DateTime(2025,1,29,17,02,0)"</span><span class="sxs-lookup"><span data-stu-id="1c7f9-127">"@new DateTime(2025,1,29,17,02,0)"</span></span>    |


<span data-ttu-id="1c7f9-128">设置一个绝对过期日期。</span><span class="sxs-lookup"><span data-stu-id="1c7f9-128">Sets an absolute expiration date.</span></span> <span data-ttu-id="1c7f9-129">下面的示例将之前 2025 年 1 月 29，下午 5:02 缓存的缓存标记帮助程序中的内容。</span><span class="sxs-lookup"><span data-stu-id="1c7f9-129">The following example will cache the contents of the Cache Tag Helper until 5:02 PM on January 29, 2025.</span></span>

<span data-ttu-id="1c7f9-130">示例:</span><span class="sxs-lookup"><span data-stu-id="1c7f9-130">Example:</span></span>

```cshtml
<Cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="expires-after"></a><span data-ttu-id="1c7f9-131">过期时间</span><span class="sxs-lookup"><span data-stu-id="1c7f9-131">expires-after</span></span>

| <span data-ttu-id="1c7f9-132">特性类型</span><span class="sxs-lookup"><span data-stu-id="1c7f9-132">Attribute Type</span></span>    | <span data-ttu-id="1c7f9-133">示例值</span><span class="sxs-lookup"><span data-stu-id="1c7f9-133">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="1c7f9-134">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="1c7f9-134">TimeSpan</span></span>    | <span data-ttu-id="1c7f9-135">"@TimeSpan.FromSeconds(120)"</span><span class="sxs-lookup"><span data-stu-id="1c7f9-135">"@TimeSpan.FromSeconds(120)"</span></span>    |


<span data-ttu-id="1c7f9-136">从缓存中的内容的第一个请求时间设置时间的长度。</span><span class="sxs-lookup"><span data-stu-id="1c7f9-136">Sets the length of time from the first request time to cache the contents.</span></span> 

<span data-ttu-id="1c7f9-137">示例:</span><span class="sxs-lookup"><span data-stu-id="1c7f9-137">Example:</span></span>

```cshtml
<Cache expires-on="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="expires-sliding"></a><span data-ttu-id="1c7f9-138">过期-滑动</span><span class="sxs-lookup"><span data-stu-id="1c7f9-138">expires-sliding</span></span>

| <span data-ttu-id="1c7f9-139">特性类型</span><span class="sxs-lookup"><span data-stu-id="1c7f9-139">Attribute Type</span></span>    | <span data-ttu-id="1c7f9-140">示例值</span><span class="sxs-lookup"><span data-stu-id="1c7f9-140">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="1c7f9-141">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="1c7f9-141">TimeSpan</span></span>    | <span data-ttu-id="1c7f9-142">"@TimeSpan.FromSeconds(60)"</span><span class="sxs-lookup"><span data-stu-id="1c7f9-142">"@TimeSpan.FromSeconds(60)"</span></span>     |


<span data-ttu-id="1c7f9-143">设置应被逐出缓存项，如果未访问的时间。</span><span class="sxs-lookup"><span data-stu-id="1c7f9-143">Sets the time that a cache entry should be evicted if it has not been accessed.</span></span>

<span data-ttu-id="1c7f9-144">示例:</span><span class="sxs-lookup"><span data-stu-id="1c7f9-144">Example:</span></span>

```cshtml
<Cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-header"></a><span data-ttu-id="1c7f9-145">不同的标题</span><span class="sxs-lookup"><span data-stu-id="1c7f9-145">vary-by-header</span></span>

| <span data-ttu-id="1c7f9-146">特性类型</span><span class="sxs-lookup"><span data-stu-id="1c7f9-146">Attribute Type</span></span>    | <span data-ttu-id="1c7f9-147">示例值</span><span class="sxs-lookup"><span data-stu-id="1c7f9-147">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="1c7f9-148">字符串</span><span class="sxs-lookup"><span data-stu-id="1c7f9-148">String</span></span>            | <span data-ttu-id="1c7f9-149">"用户代理"</span><span class="sxs-lookup"><span data-stu-id="1c7f9-149">"User-Agent"</span></span>                  |
|                   | <span data-ttu-id="1c7f9-150">"用户代理，内容编码"</span><span class="sxs-lookup"><span data-stu-id="1c7f9-150">"User-Agent,content-encoding"</span></span> |

<span data-ttu-id="1c7f9-151">接受的单个标头值或在更改时触发缓存刷新的标头值的以逗号分隔列表。</span><span class="sxs-lookup"><span data-stu-id="1c7f9-151">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when they change.</span></span> <span data-ttu-id="1c7f9-152">下面的示例监视标头值`User-Agent`。</span><span class="sxs-lookup"><span data-stu-id="1c7f9-152">The following example monitors the header value `User-Agent`.</span></span> <span data-ttu-id="1c7f9-153">该示例将缓存的内容的每个不同`User-Agent`向 web 服务器显示。</span><span class="sxs-lookup"><span data-stu-id="1c7f9-153">The example will cache the content for every different `User-Agent` presented to the web server.</span></span>

<span data-ttu-id="1c7f9-154">示例:</span><span class="sxs-lookup"><span data-stu-id="1c7f9-154">Example:</span></span>

```cshtml
<Cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-query"></a><span data-ttu-id="1c7f9-155">不同的查询</span><span class="sxs-lookup"><span data-stu-id="1c7f9-155">vary-by-query</span></span>

| <span data-ttu-id="1c7f9-156">特性类型</span><span class="sxs-lookup"><span data-stu-id="1c7f9-156">Attribute Type</span></span>    | <span data-ttu-id="1c7f9-157">示例值</span><span class="sxs-lookup"><span data-stu-id="1c7f9-157">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="1c7f9-158">字符串</span><span class="sxs-lookup"><span data-stu-id="1c7f9-158">String</span></span>            | <span data-ttu-id="1c7f9-159">"使"</span><span class="sxs-lookup"><span data-stu-id="1c7f9-159">"Make"</span></span>                |
|                   | <span data-ttu-id="1c7f9-160">"生成，模型"</span><span class="sxs-lookup"><span data-stu-id="1c7f9-160">"Make,Model"</span></span> |

<span data-ttu-id="1c7f9-161">接受的单个标头值或以逗号分隔的标头值更改时触发缓存刷新的标头值的列表。</span><span class="sxs-lookup"><span data-stu-id="1c7f9-161">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header value changes.</span></span> <span data-ttu-id="1c7f9-162">下面的示例查找值`Make`和`Model`。</span><span class="sxs-lookup"><span data-stu-id="1c7f9-162">The following example looks at the values of `Make` and `Model`.</span></span>

<span data-ttu-id="1c7f9-163">示例:</span><span class="sxs-lookup"><span data-stu-id="1c7f9-163">Example:</span></span>

```cshtml
<Cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-route"></a><span data-ttu-id="1c7f9-164">不同的路由</span><span class="sxs-lookup"><span data-stu-id="1c7f9-164">vary-by-route</span></span>

| <span data-ttu-id="1c7f9-165">特性类型</span><span class="sxs-lookup"><span data-stu-id="1c7f9-165">Attribute Type</span></span>    | <span data-ttu-id="1c7f9-166">示例值</span><span class="sxs-lookup"><span data-stu-id="1c7f9-166">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="1c7f9-167">字符串</span><span class="sxs-lookup"><span data-stu-id="1c7f9-167">String</span></span>            | <span data-ttu-id="1c7f9-168">"使"</span><span class="sxs-lookup"><span data-stu-id="1c7f9-168">"Make"</span></span>                |
|                   | <span data-ttu-id="1c7f9-169">"生成，模型"</span><span class="sxs-lookup"><span data-stu-id="1c7f9-169">"Make,Model"</span></span> |

<span data-ttu-id="1c7f9-170">接受的单个标头值或路由数据参数值更改时触发缓存刷新的标头值的以逗号分隔列表。</span><span class="sxs-lookup"><span data-stu-id="1c7f9-170">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the route data parameter value(s) change.</span></span> <span data-ttu-id="1c7f9-171">示例:</span><span class="sxs-lookup"><span data-stu-id="1c7f9-171">Example:</span></span>

<span data-ttu-id="1c7f9-172">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="1c7f9-172">*Startup.cs*</span></span> 

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```
  
<span data-ttu-id="1c7f9-173">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="1c7f9-173">*Index.cshtml*</span></span>

```cshtml
<Cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-cookie"></a><span data-ttu-id="1c7f9-174">不同的 cookie</span><span class="sxs-lookup"><span data-stu-id="1c7f9-174">vary-by-cookie</span></span>

| <span data-ttu-id="1c7f9-175">特性类型</span><span class="sxs-lookup"><span data-stu-id="1c7f9-175">Attribute Type</span></span>    | <span data-ttu-id="1c7f9-176">示例值</span><span class="sxs-lookup"><span data-stu-id="1c7f9-176">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="1c7f9-177">字符串</span><span class="sxs-lookup"><span data-stu-id="1c7f9-177">String</span></span>            | <span data-ttu-id="1c7f9-178">".AspNetCore.Identity.Application"</span><span class="sxs-lookup"><span data-stu-id="1c7f9-178">".AspNetCore.Identity.Application"</span></span>                |
|                   | <span data-ttu-id="1c7f9-179">".AspNetCore.Identity.Application,HairColor"</span><span class="sxs-lookup"><span data-stu-id="1c7f9-179">".AspNetCore.Identity.Application,HairColor"</span></span> |

<span data-ttu-id="1c7f9-180">接受的单个标头值或在标头值 (s) 更改时触发缓存刷新的标头值的以逗号分隔列表。</span><span class="sxs-lookup"><span data-stu-id="1c7f9-180">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header values(s) change.</span></span> <span data-ttu-id="1c7f9-181">下面的示例查找在与 ASP.NET 标识关联的 cookie。</span><span class="sxs-lookup"><span data-stu-id="1c7f9-181">The following example looks at the cookie associated with ASP.NET Identity.</span></span> <span data-ttu-id="1c7f9-182">用户进行身份验证时请求 cookie 设置随即将会触发缓存刷新。</span><span class="sxs-lookup"><span data-stu-id="1c7f9-182">When a user is authenticated the request cookie to be set which triggers a cache refresh.</span></span>

<span data-ttu-id="1c7f9-183">示例:</span><span class="sxs-lookup"><span data-stu-id="1c7f9-183">Example:</span></span>

```cshtml
<Cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-user"></a><span data-ttu-id="1c7f9-184">不同的用户</span><span class="sxs-lookup"><span data-stu-id="1c7f9-184">vary-by-user</span></span>

| <span data-ttu-id="1c7f9-185">特性类型</span><span class="sxs-lookup"><span data-stu-id="1c7f9-185">Attribute Type</span></span>    | <span data-ttu-id="1c7f9-186">示例值</span><span class="sxs-lookup"><span data-stu-id="1c7f9-186">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="1c7f9-187">Boolean</span><span class="sxs-lookup"><span data-stu-id="1c7f9-187">Boolean</span></span>             | <span data-ttu-id="1c7f9-188">“true”</span><span class="sxs-lookup"><span data-stu-id="1c7f9-188">"true"</span></span>                  |
|                     | <span data-ttu-id="1c7f9-189">"false"（默认值）</span><span class="sxs-lookup"><span data-stu-id="1c7f9-189">"false" (default)</span></span> |

<span data-ttu-id="1c7f9-190">指定的登录的用户 （或上下文主体） 更改时，缓存应重置。</span><span class="sxs-lookup"><span data-stu-id="1c7f9-190">Specifies whether or not the cache should reset when the logged-in user (or Context Principal) changes.</span></span> <span data-ttu-id="1c7f9-191">当前用户是也称为请求上下文主体和可以在 Razor 视图中查看通过引用`@User.Identity.Name`。</span><span class="sxs-lookup"><span data-stu-id="1c7f9-191">The current user is also known as the Request Context Principal and can be viewed in a Razor view by referencing `@User.Identity.Name`.</span></span>

<span data-ttu-id="1c7f9-192">下面的示例查找在当前登录用户。</span><span class="sxs-lookup"><span data-stu-id="1c7f9-192">The following example looks at the current logged in user.</span></span>  

<span data-ttu-id="1c7f9-193">示例:</span><span class="sxs-lookup"><span data-stu-id="1c7f9-193">Example:</span></span>

```cshtml
<Cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

<span data-ttu-id="1c7f9-194">使用此属性可保持缓存通过日志和日志打卡周期中的内容。</span><span class="sxs-lookup"><span data-stu-id="1c7f9-194">Using this attribute maintains the contents in cache through a log-in and log-out cycle.</span></span>  <span data-ttu-id="1c7f9-195">使用时`vary-by-user="true"`，身份验证的用户的缓存失效的登录和注销操作。</span><span class="sxs-lookup"><span data-stu-id="1c7f9-195">When using `vary-by-user="true"`, a log-in and log-out action invalidates the cache for the authenticated user.</span></span>  <span data-ttu-id="1c7f9-196">因为在登录时会生成新的唯一的 cookie 值，缓存会失效。</span><span class="sxs-lookup"><span data-stu-id="1c7f9-196">The cache is invalidated because a new unique cookie value is generated on login.</span></span> <span data-ttu-id="1c7f9-197">在任何 cookie 或已过期时，缓存将保留匿名的状态。</span><span class="sxs-lookup"><span data-stu-id="1c7f9-197">Cache is maintained for the anonymous state when no cookie is present or has expired.</span></span> <span data-ttu-id="1c7f9-198">这意味着如果没有用户登录，将维护缓存。</span><span class="sxs-lookup"><span data-stu-id="1c7f9-198">This means if no user is logged in, the cache will be maintained.</span></span>

- - -

### <a name="vary-by"></a><span data-ttu-id="1c7f9-199">变化因素</span><span class="sxs-lookup"><span data-stu-id="1c7f9-199">vary-by</span></span>

| <span data-ttu-id="1c7f9-200">特性类型</span><span class="sxs-lookup"><span data-stu-id="1c7f9-200">Attribute Type</span></span>    | <span data-ttu-id="1c7f9-201">示例值</span><span class="sxs-lookup"><span data-stu-id="1c7f9-201">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="1c7f9-202">字符串</span><span class="sxs-lookup"><span data-stu-id="1c7f9-202">String</span></span>             | <span data-ttu-id="1c7f9-203">“@Model”</span><span class="sxs-lookup"><span data-stu-id="1c7f9-203">"@Model"</span></span>                 |


<span data-ttu-id="1c7f9-204">允许获取缓存哪些数据的自定义项。</span><span class="sxs-lookup"><span data-stu-id="1c7f9-204">Allows for customization of what data gets cached.</span></span> <span data-ttu-id="1c7f9-205">当更新属性的字符串值发生更改，缓存标记帮助器的内容所引用的对象。</span><span class="sxs-lookup"><span data-stu-id="1c7f9-205">When the object referenced by the attribute's string value changes, the content of the Cache Tag Helper is updated.</span></span> <span data-ttu-id="1c7f9-206">通常模型值字符串串联分配给此属性。</span><span class="sxs-lookup"><span data-stu-id="1c7f9-206">Often a string-concatenation of model values are assigned to this attribute.</span></span>  <span data-ttu-id="1c7f9-207">实际上，这意味着对任何的串连值的更新令缓存失效。</span><span class="sxs-lookup"><span data-stu-id="1c7f9-207">Effectively, that means an update to any of the concatenated values invalidates the cache.</span></span>

<span data-ttu-id="1c7f9-208">下面的示例假定呈现的两个的路由参数的整数值的视图和控制器方法`myParam1`和`myParam2`，并作为单个模型属性返回的。</span><span class="sxs-lookup"><span data-stu-id="1c7f9-208">The following example assumes the controller method rendering the view sums the integer value of the two route parameters, `myParam1` and `myParam2`, and returns that as the single model property.</span></span> <span data-ttu-id="1c7f9-209">当此总和更改时，呈现和再次缓存的缓存标记帮助程序的内容。</span><span class="sxs-lookup"><span data-stu-id="1c7f9-209">When this sum changes, the content of the Cache Tag Helper is rendered and cached again.</span></span>  

<span data-ttu-id="1c7f9-210">示例:</span><span class="sxs-lookup"><span data-stu-id="1c7f9-210">Example:</span></span>

<span data-ttu-id="1c7f9-211">操作：</span><span class="sxs-lookup"><span data-stu-id="1c7f9-211">Action:</span></span>

```csharp
public IActionResult Index(string myParam1,string myParam2,string myParam3)
{
    int num1;
    int num2;
    int.TryParse(myParam1, out num1);
    int.TryParse(myParam2, out num2);
    return View(viewName, num1 + num2);
}
```

<span data-ttu-id="1c7f9-212">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="1c7f9-212">*Index.cshtml*</span></span>

```cshtml
<Cache vary-by="@Model"">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="priority"></a><span data-ttu-id="1c7f9-213">priority</span><span class="sxs-lookup"><span data-stu-id="1c7f9-213">priority</span></span>

| <span data-ttu-id="1c7f9-214">特性类型</span><span class="sxs-lookup"><span data-stu-id="1c7f9-214">Attribute Type</span></span>    | <span data-ttu-id="1c7f9-215">示例值</span><span class="sxs-lookup"><span data-stu-id="1c7f9-215">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="1c7f9-216">CacheItemPriority</span><span class="sxs-lookup"><span data-stu-id="1c7f9-216">CacheItemPriority</span></span>  | <span data-ttu-id="1c7f9-217">"高"</span><span class="sxs-lookup"><span data-stu-id="1c7f9-217">"High"</span></span>                   |
|                    | <span data-ttu-id="1c7f9-218">"低"</span><span class="sxs-lookup"><span data-stu-id="1c7f9-218">"Low"</span></span> |
|                    | <span data-ttu-id="1c7f9-219">"NeverRemove"</span><span class="sxs-lookup"><span data-stu-id="1c7f9-219">"NeverRemove"</span></span> |
|                    | <span data-ttu-id="1c7f9-220">"Normal"</span><span class="sxs-lookup"><span data-stu-id="1c7f9-220">"Normal"</span></span> |

<span data-ttu-id="1c7f9-221">提供内置的缓存提供程序的缓存逐出指导。</span><span class="sxs-lookup"><span data-stu-id="1c7f9-221">Provides cache eviction guidance to the built-in cache provider.</span></span> <span data-ttu-id="1c7f9-222">Web 服务器将逐出`Low`处于内存压力下时，第一次缓存条目。</span><span class="sxs-lookup"><span data-stu-id="1c7f9-222">The web server will evict `Low` cache entries first when it's under memory pressure.</span></span>

<span data-ttu-id="1c7f9-223">示例:</span><span class="sxs-lookup"><span data-stu-id="1c7f9-223">Example:</span></span>

```cshtml
<Cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

<span data-ttu-id="1c7f9-224">`priority`属性不能保证特定级别的缓存保留。</span><span class="sxs-lookup"><span data-stu-id="1c7f9-224">The `priority` attribute does not guarantee a specific level of cache retention.</span></span> <span data-ttu-id="1c7f9-225">`CacheItemPriority`是只是建议。</span><span class="sxs-lookup"><span data-stu-id="1c7f9-225">`CacheItemPriority` is only a suggestion.</span></span> <span data-ttu-id="1c7f9-226">此属性设置为`NeverRemove`不保证始终将保留缓存。</span><span class="sxs-lookup"><span data-stu-id="1c7f9-226">Setting this attribute to `NeverRemove` does not guarantee that the cache will always be retained.</span></span> <span data-ttu-id="1c7f9-227">请参阅[其他资源](#additional-resources)有关详细信息。</span><span class="sxs-lookup"><span data-stu-id="1c7f9-227">See [Additional Resources](#additional-resources) for more information.</span></span>

<span data-ttu-id="1c7f9-228">缓存标记帮助程序是依赖于[内存缓存服务](xref:performance/caching/memory)。</span><span class="sxs-lookup"><span data-stu-id="1c7f9-228">The Cache Tag Helper is dependent on the [memory cache service](xref:performance/caching/memory).</span></span> <span data-ttu-id="1c7f9-229">如果尚未添加，缓存标记帮助器将服务添加。</span><span class="sxs-lookup"><span data-stu-id="1c7f9-229">The Cache Tag Helper adds the service if it has not been added.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1c7f9-230">其他资源</span><span class="sxs-lookup"><span data-stu-id="1c7f9-230">Additional resources</span></span>

* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
