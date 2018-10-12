---
title: ASP.NET Core MVC 中的缓存标记帮助程序
author: pkellner
description: 了解如何使用缓存标记帮助程序。
ms.author: riande
ms.date: 02/14/2017
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: 11754d2858d8f02c7eb9baac8feda9b50ddb3d79
ms.sourcegitcommit: 4d5f8680d68b39c411b46c73f7014f8aa0f12026
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2018
ms.locfileid: "47028150"
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a><span data-ttu-id="ea723-103">ASP.NET Core MVC 中的缓存标记帮助程序</span><span class="sxs-lookup"><span data-stu-id="ea723-103">Cache Tag Helper in ASP.NET Core MVC</span></span>

<span data-ttu-id="ea723-104">作者：[Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="ea723-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="ea723-105">缓存标记帮助程序通过将其内容缓存到内部 ASP.NET Core 缓存提供程序中，可极大地提高 ASP.NET Core 应用的性能。</span><span class="sxs-lookup"><span data-stu-id="ea723-105">The Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to the internal ASP.NET Core cache provider.</span></span>

<span data-ttu-id="ea723-106">Razor 视图引擎将默认的 `expires-after` 设置为 20 分钟。</span><span class="sxs-lookup"><span data-stu-id="ea723-106">The Razor View Engine sets the default `expires-after` to twenty minutes.</span></span>

<span data-ttu-id="ea723-107">以下 Razor 标记将缓存日期/时间：</span><span class="sxs-lookup"><span data-stu-id="ea723-107">The following Razor markup caches the date/time:</span></span>

```cshtml
<cache>@DateTime.Now</cache>
```

<span data-ttu-id="ea723-108">针对包含 `CacheTagHelper` 页面的第一个请求将显示当前的日期/时间。</span><span class="sxs-lookup"><span data-stu-id="ea723-108">The first request to the page that contains `CacheTagHelper` will display the current date/time.</span></span> <span data-ttu-id="ea723-109">其他请求将显示已缓存的值，直到缓存过期（默认 20 分钟）或被内存压力逐出。</span><span class="sxs-lookup"><span data-stu-id="ea723-109">Additional requests will show the cached value until the cache expires (default 20 minutes) or is evicted by memory pressure.</span></span>

<span data-ttu-id="ea723-110">可使用以下属性设置缓存持续时间：</span><span class="sxs-lookup"><span data-stu-id="ea723-110">You can set the cache duration with the following attributes:</span></span>

## <a name="cache-tag-helper-attributes"></a><span data-ttu-id="ea723-111">缓存标记帮助程序属性</span><span class="sxs-lookup"><span data-stu-id="ea723-111">Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled"></a><span data-ttu-id="ea723-112">enabled</span><span class="sxs-lookup"><span data-stu-id="ea723-112">enabled</span></span>    


| <span data-ttu-id="ea723-113">属性类型</span><span class="sxs-lookup"><span data-stu-id="ea723-113">Attribute Type</span></span>    | <span data-ttu-id="ea723-114">有效值</span><span class="sxs-lookup"><span data-stu-id="ea723-114">Valid Values</span></span>      |
|----------------   |----------------   |
| <span data-ttu-id="ea723-115">boolean</span><span class="sxs-lookup"><span data-stu-id="ea723-115">boolean</span></span>           | <span data-ttu-id="ea723-116">“true”（默认值）</span><span class="sxs-lookup"><span data-stu-id="ea723-116">"true" (default)</span></span>  |
|                   | <span data-ttu-id="ea723-117">“false”</span><span class="sxs-lookup"><span data-stu-id="ea723-117">"false"</span></span>   |


<span data-ttu-id="ea723-118">确定是否缓存了缓存标记帮助程序所包含的内容。</span><span class="sxs-lookup"><span data-stu-id="ea723-118">Determines whether the content enclosed by the Cache Tag Helper is cached.</span></span> <span data-ttu-id="ea723-119">默认值为 `true`。</span><span class="sxs-lookup"><span data-stu-id="ea723-119">The default is `true`.</span></span>  <span data-ttu-id="ea723-120">如果设置为 `false`，则此缓存标记帮助程序对于呈现的输出没有缓存效果。</span><span class="sxs-lookup"><span data-stu-id="ea723-120">If set to `false` this Cache Tag Helper will have no caching effect on the rendered output.</span></span>

<span data-ttu-id="ea723-121">示例:</span><span class="sxs-lookup"><span data-stu-id="ea723-121">Example:</span></span>

```cshtml
<cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-on"></a><span data-ttu-id="ea723-122">expires-on</span><span class="sxs-lookup"><span data-stu-id="ea723-122">expires-on</span></span> 

| <span data-ttu-id="ea723-123">属性类型</span><span class="sxs-lookup"><span data-stu-id="ea723-123">Attribute Type</span></span> |           <span data-ttu-id="ea723-124">示例值</span><span class="sxs-lookup"><span data-stu-id="ea723-124">Example Value</span></span>            |
|----------------|------------------------------------|
| <span data-ttu-id="ea723-125">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="ea723-125">DateTimeOffset</span></span> | <span data-ttu-id="ea723-126">"@new DateTime(2025,1,29,17,02,0)"</span><span class="sxs-lookup"><span data-stu-id="ea723-126">"@new DateTime(2025,1,29,17,02,0)"</span></span> |

<span data-ttu-id="ea723-127">设置一个绝对到期日期。</span><span class="sxs-lookup"><span data-stu-id="ea723-127">Sets an absolute expiration date.</span></span> <span data-ttu-id="ea723-128">以下示例将在 2025 年 1 月 29 日下午 5:02 之前缓存缓存标记帮助程序的内容。</span><span class="sxs-lookup"><span data-stu-id="ea723-128">The following example will cache the contents of the Cache Tag Helper until 5:02 PM on January 29, 2025.</span></span>

<span data-ttu-id="ea723-129">示例:</span><span class="sxs-lookup"><span data-stu-id="ea723-129">Example:</span></span>

```cshtml
<cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-after"></a><span data-ttu-id="ea723-130">expires-after</span><span class="sxs-lookup"><span data-stu-id="ea723-130">expires-after</span></span>

| <span data-ttu-id="ea723-131">属性类型</span><span class="sxs-lookup"><span data-stu-id="ea723-131">Attribute Type</span></span> |        <span data-ttu-id="ea723-132">示例值</span><span class="sxs-lookup"><span data-stu-id="ea723-132">Example Value</span></span>         |
|----------------|------------------------------|
|    <span data-ttu-id="ea723-133">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="ea723-133">TimeSpan</span></span>    | <span data-ttu-id="ea723-134">"@TimeSpan.FromSeconds(120)"</span><span class="sxs-lookup"><span data-stu-id="ea723-134">"@TimeSpan.FromSeconds(120)"</span></span> |

<span data-ttu-id="ea723-135">设置从第一个请求时间到缓存内容的时间长度。</span><span class="sxs-lookup"><span data-stu-id="ea723-135">Sets the length of time from the first request time to cache the contents.</span></span> 

<span data-ttu-id="ea723-136">示例:</span><span class="sxs-lookup"><span data-stu-id="ea723-136">Example:</span></span>

```cshtml
<cache expires-after="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-sliding"></a><span data-ttu-id="ea723-137">expires-sliding</span><span class="sxs-lookup"><span data-stu-id="ea723-137">expires-sliding</span></span>

| <span data-ttu-id="ea723-138">属性类型</span><span class="sxs-lookup"><span data-stu-id="ea723-138">Attribute Type</span></span> |        <span data-ttu-id="ea723-139">示例值</span><span class="sxs-lookup"><span data-stu-id="ea723-139">Example Value</span></span>        |
|----------------|-----------------------------|
|    <span data-ttu-id="ea723-140">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="ea723-140">TimeSpan</span></span>    | <span data-ttu-id="ea723-141">"@TimeSpan.FromSeconds(60)"</span><span class="sxs-lookup"><span data-stu-id="ea723-141">"@TimeSpan.FromSeconds(60)"</span></span> |

<span data-ttu-id="ea723-142">设置缓存条目在未被访问时应被逐出的时间。</span><span class="sxs-lookup"><span data-stu-id="ea723-142">Sets the time that a cache entry should be evicted if it has not been accessed.</span></span>

<span data-ttu-id="ea723-143">示例:</span><span class="sxs-lookup"><span data-stu-id="ea723-143">Example:</span></span>

```cshtml
<cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-header"></a><span data-ttu-id="ea723-144">vary-by-header</span><span class="sxs-lookup"><span data-stu-id="ea723-144">vary-by-header</span></span>

| <span data-ttu-id="ea723-145">属性类型</span><span class="sxs-lookup"><span data-stu-id="ea723-145">Attribute Type</span></span>    | <span data-ttu-id="ea723-146">示例值</span><span class="sxs-lookup"><span data-stu-id="ea723-146">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="ea723-147">String</span><span class="sxs-lookup"><span data-stu-id="ea723-147">String</span></span>            | <span data-ttu-id="ea723-148">"User-Agent"</span><span class="sxs-lookup"><span data-stu-id="ea723-148">"User-Agent"</span></span>                  |
|                   | <span data-ttu-id="ea723-149">"User-Agent,content-encoding"</span><span class="sxs-lookup"><span data-stu-id="ea723-149">"User-Agent,content-encoding"</span></span> |

<span data-ttu-id="ea723-150">接受单个标头值或逗号分隔的标头值列表，在更改时触发缓存刷新。</span><span class="sxs-lookup"><span data-stu-id="ea723-150">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when they change.</span></span> <span data-ttu-id="ea723-151">以下示例监视标头值 `User-Agent`。</span><span class="sxs-lookup"><span data-stu-id="ea723-151">The following example monitors the header value `User-Agent`.</span></span> <span data-ttu-id="ea723-152">该示例将缓存提供给 Web 服务器的每个不同 `User-Agent` 的内容。</span><span class="sxs-lookup"><span data-stu-id="ea723-152">The example will cache the content for every different `User-Agent` presented to the web server.</span></span>

<span data-ttu-id="ea723-153">示例:</span><span class="sxs-lookup"><span data-stu-id="ea723-153">Example:</span></span>

```cshtml
<cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-query"></a><span data-ttu-id="ea723-154">vary-by-query</span><span class="sxs-lookup"><span data-stu-id="ea723-154">vary-by-query</span></span>

| <span data-ttu-id="ea723-155">属性类型</span><span class="sxs-lookup"><span data-stu-id="ea723-155">Attribute Type</span></span>    | <span data-ttu-id="ea723-156">示例值</span><span class="sxs-lookup"><span data-stu-id="ea723-156">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="ea723-157">String</span><span class="sxs-lookup"><span data-stu-id="ea723-157">String</span></span>            | <span data-ttu-id="ea723-158">"Make"</span><span class="sxs-lookup"><span data-stu-id="ea723-158">"Make"</span></span>                |
|                   | <span data-ttu-id="ea723-159">"Make,Model"</span><span class="sxs-lookup"><span data-stu-id="ea723-159">"Make,Model"</span></span> |

<span data-ttu-id="ea723-160">接受单个标头值或逗号分隔的标头值列表，当标头值更改时触发缓存刷新。</span><span class="sxs-lookup"><span data-stu-id="ea723-160">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header value changes.</span></span> <span data-ttu-id="ea723-161">以下示例查看 `Make` 和 `Model` 的值。</span><span class="sxs-lookup"><span data-stu-id="ea723-161">The following example looks at the values of `Make` and `Model`.</span></span>

<span data-ttu-id="ea723-162">示例:</span><span class="sxs-lookup"><span data-stu-id="ea723-162">Example:</span></span>

```cshtml
<cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-route"></a><span data-ttu-id="ea723-163">vary-by-route</span><span class="sxs-lookup"><span data-stu-id="ea723-163">vary-by-route</span></span>

| <span data-ttu-id="ea723-164">属性类型</span><span class="sxs-lookup"><span data-stu-id="ea723-164">Attribute Type</span></span>    | <span data-ttu-id="ea723-165">示例值</span><span class="sxs-lookup"><span data-stu-id="ea723-165">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="ea723-166">String</span><span class="sxs-lookup"><span data-stu-id="ea723-166">String</span></span>            | <span data-ttu-id="ea723-167">"Make"</span><span class="sxs-lookup"><span data-stu-id="ea723-167">"Make"</span></span>                |
|                   | <span data-ttu-id="ea723-168">"Make,Model"</span><span class="sxs-lookup"><span data-stu-id="ea723-168">"Make,Model"</span></span> |

<span data-ttu-id="ea723-169">接受单个标头值或逗号分隔的标头值列表，当路由数据参数值更改时触发缓存刷新。</span><span class="sxs-lookup"><span data-stu-id="ea723-169">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the route data parameter value(s) change.</span></span> <span data-ttu-id="ea723-170">示例:</span><span class="sxs-lookup"><span data-stu-id="ea723-170">Example:</span></span>

<span data-ttu-id="ea723-171">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="ea723-171">*Startup.cs*</span></span> 

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```

<span data-ttu-id="ea723-172">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="ea723-172">*Index.cshtml*</span></span>

```cshtml
<cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-cookie"></a><span data-ttu-id="ea723-173">vary-by-cookie</span><span class="sxs-lookup"><span data-stu-id="ea723-173">vary-by-cookie</span></span>

| <span data-ttu-id="ea723-174">属性类型</span><span class="sxs-lookup"><span data-stu-id="ea723-174">Attribute Type</span></span>    | <span data-ttu-id="ea723-175">示例值</span><span class="sxs-lookup"><span data-stu-id="ea723-175">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="ea723-176">String</span><span class="sxs-lookup"><span data-stu-id="ea723-176">String</span></span>            | <span data-ttu-id="ea723-177">".AspNetCore.Identity.Application"</span><span class="sxs-lookup"><span data-stu-id="ea723-177">".AspNetCore.Identity.Application"</span></span>                |
|                   | <span data-ttu-id="ea723-178">".AspNetCore.Identity.Application,HairColor"</span><span class="sxs-lookup"><span data-stu-id="ea723-178">".AspNetCore.Identity.Application,HairColor"</span></span> |

<span data-ttu-id="ea723-179">接受单个标头值或逗号分隔的标头值列表，当标头值更改时触发缓存刷新。</span><span class="sxs-lookup"><span data-stu-id="ea723-179">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header values(s) change.</span></span> <span data-ttu-id="ea723-180">以下示例查看与 ASP.NET Core Identity 相关联的 cookie。</span><span class="sxs-lookup"><span data-stu-id="ea723-180">The following example looks at the cookie associated with ASP.NET Core Identity.</span></span> <span data-ttu-id="ea723-181">当用户经过身份验证时，要设置的请求 cookie 将触发缓存刷新。</span><span class="sxs-lookup"><span data-stu-id="ea723-181">When a user is authenticated the request cookie to be set which triggers a cache refresh.</span></span>

<span data-ttu-id="ea723-182">示例:</span><span class="sxs-lookup"><span data-stu-id="ea723-182">Example:</span></span>

```cshtml
<cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-user"></a><span data-ttu-id="ea723-183">vary-by-user</span><span class="sxs-lookup"><span data-stu-id="ea723-183">vary-by-user</span></span>

| <span data-ttu-id="ea723-184">属性类型</span><span class="sxs-lookup"><span data-stu-id="ea723-184">Attribute Type</span></span>    | <span data-ttu-id="ea723-185">示例值</span><span class="sxs-lookup"><span data-stu-id="ea723-185">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="ea723-186">Boolean</span><span class="sxs-lookup"><span data-stu-id="ea723-186">Boolean</span></span>             | <span data-ttu-id="ea723-187">“true”</span><span class="sxs-lookup"><span data-stu-id="ea723-187">"true"</span></span>                  |
|                     | <span data-ttu-id="ea723-188">“false”（默认值）</span><span class="sxs-lookup"><span data-stu-id="ea723-188">"false" (default)</span></span> |

<span data-ttu-id="ea723-189">指定当已登录用户（或上下文主体）更改时是否应该重置缓存。</span><span class="sxs-lookup"><span data-stu-id="ea723-189">Specifies whether or not the cache should reset when the logged-in user (or Context Principal) changes.</span></span> <span data-ttu-id="ea723-190">当前用户也称为请求上下文主体，可通过引用 `@User.Identity.Name` 在 Razor 视图中查看。</span><span class="sxs-lookup"><span data-stu-id="ea723-190">The current user is also known as the Request Context Principal and can be viewed in a Razor view by referencing `@User.Identity.Name`.</span></span>

<span data-ttu-id="ea723-191">以下示例查看当前登录的用户。</span><span class="sxs-lookup"><span data-stu-id="ea723-191">The following example looks at the current logged in user.</span></span>  

<span data-ttu-id="ea723-192">示例:</span><span class="sxs-lookup"><span data-stu-id="ea723-192">Example:</span></span>

```cshtml
<cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="ea723-193">通过登录和注销周期，使用此属性将内容维护在缓存中。</span><span class="sxs-lookup"><span data-stu-id="ea723-193">Using this attribute maintains the contents in cache through a log-in and log-out cycle.</span></span>  <span data-ttu-id="ea723-194">使用 `vary-by-user="true"` 时，登录和注销操作会使经过身份验证的用户的缓存无效。</span><span class="sxs-lookup"><span data-stu-id="ea723-194">When using `vary-by-user="true"`, a log-in and log-out action invalidates the cache for the authenticated user.</span></span>  <span data-ttu-id="ea723-195">缓存无效，因为登录时会生成一个新的唯一 cookie 值。</span><span class="sxs-lookup"><span data-stu-id="ea723-195">The cache is invalidated because a new unique cookie value is generated on login.</span></span> <span data-ttu-id="ea723-196">当 cookie 不存在或已过期时，则维持缓存以呈现匿名状态。</span><span class="sxs-lookup"><span data-stu-id="ea723-196">Cache is maintained for the anonymous state when no cookie is present or has expired.</span></span> <span data-ttu-id="ea723-197">这意味着如果没有用户登录，将维持缓存。</span><span class="sxs-lookup"><span data-stu-id="ea723-197">This means if no user is logged in, the cache will be maintained.</span></span>

- - -

### <a name="vary-by"></a><span data-ttu-id="ea723-198">vary-by</span><span class="sxs-lookup"><span data-stu-id="ea723-198">vary-by</span></span>

| <span data-ttu-id="ea723-199">属性类型</span><span class="sxs-lookup"><span data-stu-id="ea723-199">Attribute Type</span></span> | <span data-ttu-id="ea723-200">示例值</span><span class="sxs-lookup"><span data-stu-id="ea723-200">Example Values</span></span> |
|----------------|----------------|
|     <span data-ttu-id="ea723-201">String</span><span class="sxs-lookup"><span data-stu-id="ea723-201">String</span></span>     |    <span data-ttu-id="ea723-202">“@Model”</span><span class="sxs-lookup"><span data-stu-id="ea723-202">"@Model"</span></span>    |

<span data-ttu-id="ea723-203">允许自定义缓存的数据。</span><span class="sxs-lookup"><span data-stu-id="ea723-203">Allows for customization of what data gets cached.</span></span> <span data-ttu-id="ea723-204">当属性的字符串值引用的对象发生更改时，会更新缓存标记帮助程序的内容。</span><span class="sxs-lookup"><span data-stu-id="ea723-204">When the object referenced by the attribute's string value changes, the content of the Cache Tag Helper is updated.</span></span> <span data-ttu-id="ea723-205">通常将模型值的字符串串联分配给此属性。</span><span class="sxs-lookup"><span data-stu-id="ea723-205">Often a string-concatenation of model values are assigned to this attribute.</span></span>  <span data-ttu-id="ea723-206">从效果上看，这意味着更新任何已连接的值都会使缓存无效。</span><span class="sxs-lookup"><span data-stu-id="ea723-206">Effectively, that means an update to any of the concatenated values invalidates the cache.</span></span>

<span data-ttu-id="ea723-207">以下示例假定视图的控制器方法将两个路由参数 `myParam1` 和 `myParam2` 的整数值相加，并将其作为单个模型属性返回。</span><span class="sxs-lookup"><span data-stu-id="ea723-207">The following example assumes the controller method rendering the view sums the integer value of the two route parameters, `myParam1` and `myParam2`, and returns that as the single model property.</span></span> <span data-ttu-id="ea723-208">当此总和更改时，会再次呈现并缓存缓存标记帮助程序的内容。</span><span class="sxs-lookup"><span data-stu-id="ea723-208">When this sum changes, the content of the Cache Tag Helper is rendered and cached again.</span></span>  

<span data-ttu-id="ea723-209">示例:</span><span class="sxs-lookup"><span data-stu-id="ea723-209">Example:</span></span>

<span data-ttu-id="ea723-210">操作：</span><span class="sxs-lookup"><span data-stu-id="ea723-210">Action:</span></span>

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

<span data-ttu-id="ea723-211">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="ea723-211">*Index.cshtml*</span></span>

```cshtml
<cache vary-by="@Model"">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="priority"></a><span data-ttu-id="ea723-212">priority</span><span class="sxs-lookup"><span data-stu-id="ea723-212">priority</span></span>

| <span data-ttu-id="ea723-213">属性类型</span><span class="sxs-lookup"><span data-stu-id="ea723-213">Attribute Type</span></span>    | <span data-ttu-id="ea723-214">示例值</span><span class="sxs-lookup"><span data-stu-id="ea723-214">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="ea723-215">CacheItemPriority</span><span class="sxs-lookup"><span data-stu-id="ea723-215">CacheItemPriority</span></span>  | <span data-ttu-id="ea723-216">"High"</span><span class="sxs-lookup"><span data-stu-id="ea723-216">"High"</span></span>                   |
|                    | <span data-ttu-id="ea723-217">"Low"</span><span class="sxs-lookup"><span data-stu-id="ea723-217">"Low"</span></span> |
|                    | <span data-ttu-id="ea723-218">"NeverRemove"</span><span class="sxs-lookup"><span data-stu-id="ea723-218">"NeverRemove"</span></span> |
|                    | <span data-ttu-id="ea723-219">"Normal"</span><span class="sxs-lookup"><span data-stu-id="ea723-219">"Normal"</span></span> |

<span data-ttu-id="ea723-220">为内置缓存提供程序提供缓存逐出指导。</span><span class="sxs-lookup"><span data-stu-id="ea723-220">Provides cache eviction guidance to the built-in cache provider.</span></span> <span data-ttu-id="ea723-221">在内存压力下，Web 服务器将首先逐出 `Low` 缓存条目。</span><span class="sxs-lookup"><span data-stu-id="ea723-221">The web server will evict `Low` cache entries first when it's under memory pressure.</span></span>

<span data-ttu-id="ea723-222">示例:</span><span class="sxs-lookup"><span data-stu-id="ea723-222">Example:</span></span>

```cshtml
<cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="ea723-223">`priority` 属性并不能保证特定级别的缓存保留。</span><span class="sxs-lookup"><span data-stu-id="ea723-223">The `priority` attribute doesn't guarantee a specific level of cache retention.</span></span> <span data-ttu-id="ea723-224">`CacheItemPriority` 仅供参考。</span><span class="sxs-lookup"><span data-stu-id="ea723-224">`CacheItemPriority` is only a suggestion.</span></span> <span data-ttu-id="ea723-225">将此属性设置为 `NeverRemove` 并不能保证缓存将始终保留。</span><span class="sxs-lookup"><span data-stu-id="ea723-225">Setting this attribute to `NeverRemove` doesn't guarantee that the cache will always be retained.</span></span> <span data-ttu-id="ea723-226">有关详细信息，请参阅[其他资源](#additional-resources)。</span><span class="sxs-lookup"><span data-stu-id="ea723-226">See [Additional Resources](#additional-resources) for more information.</span></span>

<span data-ttu-id="ea723-227">缓存标记帮助程序依赖于[内存缓存服务](xref:performance/caching/memory)。</span><span class="sxs-lookup"><span data-stu-id="ea723-227">The Cache Tag Helper is dependent on the [memory cache service](xref:performance/caching/memory).</span></span> <span data-ttu-id="ea723-228">如果尚未添加该服务，缓存标记帮助程序将添加。</span><span class="sxs-lookup"><span data-stu-id="ea723-228">The Cache Tag Helper adds the service if it has not been added.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ea723-229">其他资源</span><span class="sxs-lookup"><span data-stu-id="ea723-229">Additional resources</span></span>

* [<span data-ttu-id="ea723-230">内存中缓存</span><span class="sxs-lookup"><span data-stu-id="ea723-230">Cache in-memory</span></span>](xref:performance/caching/memory)
* [<span data-ttu-id="ea723-231">标识简介</span><span class="sxs-lookup"><span data-stu-id="ea723-231">Introduction to Identity</span></span>](xref:security/authentication/identity)
