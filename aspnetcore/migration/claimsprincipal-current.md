---
title: 将从 ClaimsPrincipal.Current 迁移
author: mjrousos
description: 了解如何摆脱 ClaimsPrincipal.Current 检索当前经过身份验证的用户的标识和 ASP.NET Core 中的声明。
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 05/04/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/claimsprincipal-current
ms.openlocfilehash: ea43d17e76380baf57cd9debbc508e8812cfa4a6
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.contentlocale: zh-CN
ms.lasthandoff: 05/08/2018
---
# <a name="migrate-from-claimsprincipalcurrent"></a><span data-ttu-id="9e2db-103">将从 ClaimsPrincipal.Current 迁移</span><span class="sxs-lookup"><span data-stu-id="9e2db-103">Migrate from ClaimsPrincipal.Current</span></span>

<span data-ttu-id="9e2db-104">在 ASP.NET 项目中，常见的做法是使用[ClaimsPrincipal.Current](/dotnet/api/system.security.claims.claimsprincipal.current)检索当前身份验证的用户的标识和声明。</span><span class="sxs-lookup"><span data-stu-id="9e2db-104">In ASP.NET projects, it was common to use [ClaimsPrincipal.Current](/dotnet/api/system.security.claims.claimsprincipal.current) to retrieve the current authenticated user's identity and claims.</span></span> <span data-ttu-id="9e2db-105">在 ASP.NET 核，再设置此属性。</span><span class="sxs-lookup"><span data-stu-id="9e2db-105">In ASP.NET Core, this property is no longer set.</span></span> <span data-ttu-id="9e2db-106">已根据它的代码需要进行更新，以通过不同方式获取当前经过身份验证的用户的标识。</span><span class="sxs-lookup"><span data-stu-id="9e2db-106">Code that was depending on it needs to be updated to get the current authenticated user's identity through a different means.</span></span>

## <a name="context-specific-data-instead-of-static-data"></a><span data-ttu-id="9e2db-107">特定于上下文的数据，而不是静态的数据</span><span class="sxs-lookup"><span data-stu-id="9e2db-107">Context-specific data instead of static data</span></span>

<span data-ttu-id="9e2db-108">使用 ASP.NET 核心，这两者的值时`ClaimsPrincipal.Current`和`Thread.CurrentPrincipal`未设置。</span><span class="sxs-lookup"><span data-stu-id="9e2db-108">When using ASP.NET Core, the values of both `ClaimsPrincipal.Current` and `Thread.CurrentPrincipal` aren't set.</span></span> <span data-ttu-id="9e2db-109">这些属性都表示静态状态，通常避免 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="9e2db-109">These properties both represent static state, which ASP.NET Core generally avoids.</span></span> <span data-ttu-id="9e2db-110">相反，ASP.NET 核心体系结构是检索特定于上下文的服务集合 （如当前用户的标识） 的依赖关系 (使用其[依赖关系注入](xref:fundamentals/dependency-injection)(DI) 模型)。</span><span class="sxs-lookup"><span data-stu-id="9e2db-110">Instead, ASP.NET Core's architecture is to retrieve dependencies (like the current user's identity) from context-specific service collections (using its [dependency injection](xref:fundamentals/dependency-injection) (DI) model).</span></span> <span data-ttu-id="9e2db-111">什么是详细，`Thread.CurrentPrincipal`是线程静态的因此它可能不会持续某些异步方案中的更改 (和`ClaimsPrincipal.Current`只调用`Thread.CurrentPrincipal`默认情况下)。</span><span class="sxs-lookup"><span data-stu-id="9e2db-111">What's more, `Thread.CurrentPrincipal` is thread static, so it may not persist changes in some asynchronous scenarios (and `ClaimsPrincipal.Current` just calls `Thread.CurrentPrincipal` by default).</span></span>

<span data-ttu-id="9e2db-112">若要了解的问题线程类型的静态成员会导致在异步方案中，请考虑下面的代码段：</span><span class="sxs-lookup"><span data-stu-id="9e2db-112">To understand the sorts of problems thread static members can lead to in asynchronous scenarios, consider the following code snippet:</span></span>

```csharp
// Create a ClaimsPrincipal and set Thread.CurrentPrincipal
var identity = new ClaimsIdentity();
identity.AddClaim(new Claim(ClaimTypes.Name, "User1"));
Thread.CurrentPrincipal = new ClaimsPrincipal(identity);

// Check the current user
Console.WriteLine($"Current user: {Thread.CurrentPrincipal?.Identity.Name}");

// For the method to complete asynchronously
await Task.Yield();

// Check the current user after
Console.WriteLine($"Current user: {Thread.CurrentPrincipal?.Identity.Name}");
```

<span data-ttu-id="9e2db-113">前面的示例代码集`Thread.CurrentPrincipal`并检查其值之前和之后等待的异步调用。</span><span class="sxs-lookup"><span data-stu-id="9e2db-113">The preceding sample code sets `Thread.CurrentPrincipal` and checks its value before and after awaiting an asynchronous call.</span></span> <span data-ttu-id="9e2db-114">`Thread.CurrentPrincipal` 特定于*线程*在其上设置，以及该方法有可能继续在另一个线程后的执行。</span><span class="sxs-lookup"><span data-stu-id="9e2db-114">`Thread.CurrentPrincipal` is specific to the *thread* on which it's set, and the method is likely to resume execution on a different thread after the await.</span></span> <span data-ttu-id="9e2db-115">因此，`Thread.CurrentPrincipal`时的存在时将首先检查，但为 null 的调用后`await Task.Yield()`。</span><span class="sxs-lookup"><span data-stu-id="9e2db-115">Consequently, `Thread.CurrentPrincipal` is present when it's first checked but is null after the call to `await Task.Yield()`.</span></span>

<span data-ttu-id="9e2db-116">从应用程序的 DI 服务集合获取当前用户的标识太多可测试性，因为可轻松插入测试标识。</span><span class="sxs-lookup"><span data-stu-id="9e2db-116">Getting the current user's identity from the app's DI service collection is more testable, too, since test identities can be easily injected.</span></span>

## <a name="retrieve-the-current-user-in-an-aspnet-core-app"></a><span data-ttu-id="9e2db-117">检索当前用户在 ASP.NET Core 应用程序</span><span class="sxs-lookup"><span data-stu-id="9e2db-117">Retrieve the current user in an ASP.NET Core app</span></span>

<span data-ttu-id="9e2db-118">有几个选项用于检索当前经过身份验证的用户的`ClaimsPrincipal`中代替了 ASP.NET Core `ClaimsPrincipal.Current`:</span><span class="sxs-lookup"><span data-stu-id="9e2db-118">There are several options for retrieving the current authenticated user's `ClaimsPrincipal` in ASP.NET Core in place of `ClaimsPrincipal.Current`:</span></span>

* <span data-ttu-id="9e2db-119">**ControllerBase.User**。</span><span class="sxs-lookup"><span data-stu-id="9e2db-119">**ControllerBase.User**.</span></span> <span data-ttu-id="9e2db-120">MVC 控制器可以访问当前的身份验证的用户使用其[用户](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.user)属性。</span><span class="sxs-lookup"><span data-stu-id="9e2db-120">MVC controllers can access the current authenticated user with their [User](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.user) property.</span></span>
* <span data-ttu-id="9e2db-121">**HttpContext.User**。</span><span class="sxs-lookup"><span data-stu-id="9e2db-121">**HttpContext.User**.</span></span> <span data-ttu-id="9e2db-122">有权访问当前组件`HttpContext`（中间件） 可以获取当前用户的`ClaimsPrincipal`从[HttpContext.User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user)。</span><span class="sxs-lookup"><span data-stu-id="9e2db-122">Components with access to the current `HttpContext` (middleware, for example) can get the current user's `ClaimsPrincipal` from [HttpContext.User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user).</span></span>
* <span data-ttu-id="9e2db-123">**从调用方传入**。</span><span class="sxs-lookup"><span data-stu-id="9e2db-123">**Passed in from caller**.</span></span> <span data-ttu-id="9e2db-124">无需访问当前库`HttpContext`通常称为从控制器或中间件组件，并且可以具有作为参数传递的当前用户的标识。</span><span class="sxs-lookup"><span data-stu-id="9e2db-124">Libraries without access to the current `HttpContext` are often called from controllers or middleware components and can have the current user's identity passed as an argument.</span></span>
* <span data-ttu-id="9e2db-125">**IHttpContextAccessor**。</span><span class="sxs-lookup"><span data-stu-id="9e2db-125">**IHttpContextAccessor**.</span></span> <span data-ttu-id="9e2db-126">ASP.NET 项目迁移到 ASP.NET 核心可能太大，可轻松地将当前用户的标识传递到所有必要的位置。</span><span class="sxs-lookup"><span data-stu-id="9e2db-126">The ASP.NET project being migrated to ASP.NET Core may be too large to easily pass the current user's identity to all necessary locations.</span></span> <span data-ttu-id="9e2db-127">在这种情况下， [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor)可以用作一种解决方法。</span><span class="sxs-lookup"><span data-stu-id="9e2db-127">In such cases, [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) can be used as a workaround.</span></span> <span data-ttu-id="9e2db-128">`IHttpContextAccessor` 能够访问当前`HttpContext`（如果存在）。</span><span class="sxs-lookup"><span data-stu-id="9e2db-128">`IHttpContextAccessor` is able to access the current `HttpContext` (if one exists).</span></span> <span data-ttu-id="9e2db-129">将获取尚未尚未更新可以使用 ASP.NET Core DI 驱动体系结构的代码中的当前用户的标识的短期解决方案：</span><span class="sxs-lookup"><span data-stu-id="9e2db-129">A short-term solution to getting the current user's identity in code that hasn't yet been updated to work with ASP.NET Core's DI-driven architecture would be:</span></span>

  * <span data-ttu-id="9e2db-130">请`IHttpContextAccessor`DI 容器通过调用中可用[AddHttpContextAccessor](https://github.com/aspnet/Hosting/issues/793)中`Startup.ConfigureServices`。</span><span class="sxs-lookup"><span data-stu-id="9e2db-130">Make `IHttpContextAccessor` available in the DI container by calling [AddHttpContextAccessor](https://github.com/aspnet/Hosting/issues/793) in `Startup.ConfigureServices`.</span></span>
  * <span data-ttu-id="9e2db-131">获取其实例`IHttpContextAccessor`在启动过程并将其存储在静态变量。</span><span class="sxs-lookup"><span data-stu-id="9e2db-131">Get an instance of `IHttpContextAccessor` during startup and store it in a static variable.</span></span> <span data-ttu-id="9e2db-132">实例是可用于以前从静态属性检索当前用户的代码。</span><span class="sxs-lookup"><span data-stu-id="9e2db-132">The instance is made available to code that was previously retrieving the current user from a static property.</span></span>
  * <span data-ttu-id="9e2db-133">检索当前用户的`ClaimsPrincipal`使用`HttpContextAccessor.HttpContext?.User`。</span><span class="sxs-lookup"><span data-stu-id="9e2db-133">Retrieve the current user's `ClaimsPrincipal` using `HttpContextAccessor.HttpContext?.User`.</span></span> <span data-ttu-id="9e2db-134">如果此代码使用的 HTTP 请求的上下文之外`HttpContext`为 null。</span><span class="sxs-lookup"><span data-stu-id="9e2db-134">If this code is used outside of the context of an HTTP request, the `HttpContext` is null.</span></span>

<span data-ttu-id="9e2db-135">最后一个选项，请使用`IHttpContextAccessor`，行为是违背 ASP.NET 核心原则 （首选静态依赖项的插入依赖关系）。</span><span class="sxs-lookup"><span data-stu-id="9e2db-135">The final option, using `IHttpContextAccessor`, is contrary to ASP.NET Core principles (preferring injected dependencies to static dependencies).</span></span> <span data-ttu-id="9e2db-136">计划最终删除依赖于静态`IHttpContextAccessor`帮助器。</span><span class="sxs-lookup"><span data-stu-id="9e2db-136">Plan to eventually remove the dependency on the static `IHttpContextAccessor` helper.</span></span> <span data-ttu-id="9e2db-137">不过，可以迁移以前使用的大型现有的 ASP.NET 应用时它是一个有用的桥， `ClaimsPrincipal.Current`。</span><span class="sxs-lookup"><span data-stu-id="9e2db-137">It can be a useful bridge, though, when migrating large existing ASP.NET apps that were previously using `ClaimsPrincipal.Current`.</span></span>
