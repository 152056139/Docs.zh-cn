---
title: "简单的授权"
author: rick-anderson
description: "本文档说明如何使用 Authorize 属性限制对 ASP.NET Core 控制器和操作的访问。"
keywords: "ASP.NET 核心，授权，AuthorizeAttribute"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 391bcaad-205f-43e4-badc-fa592d6f79f3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/simple
ms.openlocfilehash: f2dad58ffa17259412077d31f512b561e79ac595
ms.sourcegitcommit: b38796ea3806bf39b89806adfa681b2a33762907
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/07/2017
---
# <a name="simple-authorization"></a><span data-ttu-id="d1fca-104">简单的授权</span><span class="sxs-lookup"><span data-stu-id="d1fca-104">Simple Authorization</span></span>

<a name="security-authorization-simple"></a>

<span data-ttu-id="d1fca-105">通过控制在 MVC 中的授权`AuthorizeAttribute`属性和其各种参数。</span><span class="sxs-lookup"><span data-stu-id="d1fca-105">Authorization in MVC is controlled through the `AuthorizeAttribute` attribute and its various parameters.</span></span> <span data-ttu-id="d1fca-106">简单地说，应用`AuthorizeAttribute`属性设为到控制器的控制器或操作限制访问或对任何经过身份验证的用户的操作。</span><span class="sxs-lookup"><span data-stu-id="d1fca-106">At its simplest, applying the `AuthorizeAttribute` attribute to a controller or action limits access to the controller or action to any authenticated user.</span></span>

<span data-ttu-id="d1fca-107">例如，下面的代码可访问的限制`AccountController`到任何经过身份验证的用户。</span><span class="sxs-lookup"><span data-stu-id="d1fca-107">For example, the following code limits access to the `AccountController` to any authenticated user.</span></span>

```csharp
[Authorize]
public class AccountController : Controller
{
    public ActionResult Login()
    {
    }

    public ActionResult Logout()
    {
    }
}
```

<span data-ttu-id="d1fca-108">如果你想要应用于操作，而不是控制器的授权，应用`AuthorizeAttribute`属性设为操作本身：</span><span class="sxs-lookup"><span data-stu-id="d1fca-108">If you want to apply authorization to an action rather than the controller, apply the `AuthorizeAttribute` attribute to the action itself:</span></span>

```csharp
public class AccountController : Controller
{
   public ActionResult Login()
   {
   }

   [Authorize]
   public ActionResult Logout()
   {
   }
}
```

<span data-ttu-id="d1fca-109">现在，只有经过身份验证的用户可以访问`Logout`函数。</span><span class="sxs-lookup"><span data-stu-id="d1fca-109">Now only authenticated users can access the `Logout` function.</span></span>

<span data-ttu-id="d1fca-110">你还可以使用`AllowAnonymousAttribute`特性以允许使用的未经身份验证用户添加到各个操作进行访问。</span><span class="sxs-lookup"><span data-stu-id="d1fca-110">You can also use the `AllowAnonymousAttribute` attribute to allow access by non-authenticated users to individual actions.</span></span> <span data-ttu-id="d1fca-111">例如: </span><span class="sxs-lookup"><span data-stu-id="d1fca-111">For example:</span></span>

```csharp
[Authorize]
public class AccountController : Controller
{
    [AllowAnonymous]
    public ActionResult Login()
    {
    }

    public ActionResult Logout()
    {
    }
}
```

<span data-ttu-id="d1fca-112">这将允许仅经过身份验证的用户到`AccountController`，除`Login`操作，这是可访问的所有用户，而不考虑其经过身份验证或未经身份验证 / 匿名状态。</span><span class="sxs-lookup"><span data-stu-id="d1fca-112">This would allow only authenticated users to the `AccountController`, except for the `Login` action, which is accessible by everyone, regardless of their authenticated or unauthenticated / anonymous status.</span></span>

>[!WARNING]
> <span data-ttu-id="d1fca-113">`[AllowAnonymous]`绕过所有授权语句。</span><span class="sxs-lookup"><span data-stu-id="d1fca-113">`[AllowAnonymous]` bypasses all authorization statements.</span></span> <span data-ttu-id="d1fca-114">如果将应用组合`[AllowAnonymous]`和任何`[Authorize]`属性然后 Authorize 属性将始终忽略。</span><span class="sxs-lookup"><span data-stu-id="d1fca-114">If you apply combine `[AllowAnonymous]` and any `[Authorize]` attribute then the Authorize attributes will always be ignored.</span></span> <span data-ttu-id="d1fca-115">例如，如果将应用`[AllowAnonymous]`在控制器级别任何`[Authorize]`特性在同一个控制器上，或其中的任何操作都将被忽略。</span><span class="sxs-lookup"><span data-stu-id="d1fca-115">For example if you apply `[AllowAnonymous]` at the controller level any `[Authorize]` attributes on the same controller, or on any action within it will be ignored.</span></span>
