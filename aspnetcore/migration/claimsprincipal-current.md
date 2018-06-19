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
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/08/2018
ms.locfileid: "33851529"
---
# <a name="migrate-from-claimsprincipalcurrent"></a>将从 ClaimsPrincipal.Current 迁移

在 ASP.NET 项目中，常见的做法是使用[ClaimsPrincipal.Current](/dotnet/api/system.security.claims.claimsprincipal.current)检索当前身份验证的用户的标识和声明。 在 ASP.NET 核，再设置此属性。 已根据它的代码需要进行更新，以通过不同方式获取当前经过身份验证的用户的标识。

## <a name="context-specific-data-instead-of-static-data"></a>特定于上下文的数据，而不是静态的数据

使用 ASP.NET 核心，这两者的值时`ClaimsPrincipal.Current`和`Thread.CurrentPrincipal`未设置。 这些属性都表示静态状态，通常避免 ASP.NET Core。 相反，ASP.NET 核心体系结构是检索特定于上下文的服务集合 （如当前用户的标识） 的依赖关系 (使用其[依赖关系注入](xref:fundamentals/dependency-injection)(DI) 模型)。 什么是详细，`Thread.CurrentPrincipal`是线程静态的因此它可能不会持续某些异步方案中的更改 (和`ClaimsPrincipal.Current`只调用`Thread.CurrentPrincipal`默认情况下)。

若要了解的问题线程类型的静态成员会导致在异步方案中，请考虑下面的代码段：

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

前面的示例代码集`Thread.CurrentPrincipal`并检查其值之前和之后等待的异步调用。 `Thread.CurrentPrincipal` 特定于*线程*在其上设置，以及该方法有可能继续在另一个线程后的执行。 因此，`Thread.CurrentPrincipal`时的存在时将首先检查，但为 null 的调用后`await Task.Yield()`。

从应用程序的 DI 服务集合获取当前用户的标识太多可测试性，因为可轻松插入测试标识。

## <a name="retrieve-the-current-user-in-an-aspnet-core-app"></a>检索当前用户在 ASP.NET Core 应用程序

有几个选项用于检索当前经过身份验证的用户的`ClaimsPrincipal`中代替了 ASP.NET Core `ClaimsPrincipal.Current`:

* **ControllerBase.User**。 MVC 控制器可以访问当前的身份验证的用户使用其[用户](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.user)属性。
* **HttpContext.User**。 有权访问当前组件`HttpContext`（中间件） 可以获取当前用户的`ClaimsPrincipal`从[HttpContext.User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user)。
* **从调用方传入**。 无需访问当前库`HttpContext`通常称为从控制器或中间件组件，并且可以具有作为参数传递的当前用户的标识。
* **IHttpContextAccessor**。 ASP.NET 项目迁移到 ASP.NET 核心可能太大，可轻松地将当前用户的标识传递到所有必要的位置。 在这种情况下， [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor)可以用作一种解决方法。 `IHttpContextAccessor` 能够访问当前`HttpContext`（如果存在）。 将获取尚未尚未更新可以使用 ASP.NET Core DI 驱动体系结构的代码中的当前用户的标识的短期解决方案：

  * 请`IHttpContextAccessor`DI 容器通过调用中可用[AddHttpContextAccessor](https://github.com/aspnet/Hosting/issues/793)中`Startup.ConfigureServices`。
  * 获取其实例`IHttpContextAccessor`在启动过程并将其存储在静态变量。 实例是可用于以前从静态属性检索当前用户的代码。
  * 检索当前用户的`ClaimsPrincipal`使用`HttpContextAccessor.HttpContext?.User`。 如果此代码使用的 HTTP 请求的上下文之外`HttpContext`为 null。

最后一个选项，请使用`IHttpContextAccessor`，行为是违背 ASP.NET 核心原则 （首选静态依赖项的插入依赖关系）。 计划最终删除依赖于静态`IHttpContextAccessor`帮助器。 不过，可以迁移以前使用的大型现有的 ASP.NET 应用时它是一个有用的桥， `ClaimsPrincipal.Current`。
