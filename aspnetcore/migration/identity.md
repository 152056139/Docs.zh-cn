---
title: "迁移的身份验证和标识"
author: ardalis
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 0db145cb-41a5-448a-b889-72e2d789ad7f
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/identity
ms.openlocfilehash: 6972329e3d434e1b67131843118f2f18229807b9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
# <a name="migrating-authentication-and-identity"></a><span data-ttu-id="ae828-103">迁移的身份验证和标识</span><span class="sxs-lookup"><span data-stu-id="ae828-103">Migrating Authentication and Identity</span></span>

<a name="migration-identity"></a>

<span data-ttu-id="ae828-104">通过[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="ae828-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="ae828-105">在前面文章我们[配置从 ASP.NET MVC 项目迁移到 ASP.NET 核心 MVC](configuration.md)。</span><span class="sxs-lookup"><span data-stu-id="ae828-105">In the previous article we [migrated configuration from an ASP.NET MVC project to ASP.NET Core MVC](configuration.md).</span></span> <span data-ttu-id="ae828-106">在本文中，我们将迁移的注册、 登录名和用户管理功能。</span><span class="sxs-lookup"><span data-stu-id="ae828-106">In this article, we migrate the registration, login, and user management features.</span></span>

## <a name="configure-identity-and-membership"></a><span data-ttu-id="ae828-107">配置的标识和成员身份</span><span class="sxs-lookup"><span data-stu-id="ae828-107">Configure Identity and Membership</span></span>

<span data-ttu-id="ae828-108">在 ASP.NET MVC 中，使用 ASP.NET 标识 Startup.Auth.cs 和 IdentityConfig.cs，位于 App_Start 文件夹中配置身份验证和标识功能。</span><span class="sxs-lookup"><span data-stu-id="ae828-108">In ASP.NET MVC, authentication and identity features are configured using ASP.NET Identity in Startup.Auth.cs and IdentityConfig.cs, located in the App_Start folder.</span></span> <span data-ttu-id="ae828-109">在 ASP.NET 核心 MVC，这些功能在中配置*Startup.cs*。</span><span class="sxs-lookup"><span data-stu-id="ae828-109">In ASP.NET Core MVC, these features are configured in *Startup.cs*.</span></span>

<span data-ttu-id="ae828-110">安装`Microsoft.AspNetCore.Identity.EntityFrameworkCore`和`Microsoft.AspNetCore.Authentication.Cookies`NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="ae828-110">Install the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` and `Microsoft.AspNetCore.Authentication.Cookies` NuGet packages.</span></span>

<span data-ttu-id="ae828-111">然后，打开 Startup.cs 并更新`ConfigureServices()`要使用实体框架和标识服务的方法：</span><span class="sxs-lookup"><span data-stu-id="ae828-111">Then, open Startup.cs and update the `ConfigureServices()` method to use Entity Framework and Identity services:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
  // Add EF services to the services container.
  services.AddEntityFramework(Configuration)
    .AddSqlServer()
    .AddDbContext<ApplicationDbContext>();

  // Add Identity services to the services container.
  services.AddIdentity<ApplicationUser, IdentityRole>(Configuration)
    .AddEntityFrameworkStores<ApplicationDbContext>();

  services.AddMvc();
}
```

<span data-ttu-id="ae828-112">在这种情况下，有两种在上面的代码中，我们尚未尚未迁移从 ASP.NET MVC 项目中引用的类型：`ApplicationDbContext`和`ApplicationUser`。</span><span class="sxs-lookup"><span data-stu-id="ae828-112">At this point, there are two types referenced in the above code that we haven't yet migrated from the ASP.NET MVC project: `ApplicationDbContext` and `ApplicationUser`.</span></span> <span data-ttu-id="ae828-113">创建一个新*模型*文件夹中 ASP.NET Core 项目，并将两个类添加到它对应于这些类型。</span><span class="sxs-lookup"><span data-stu-id="ae828-113">Create a new *Models* folder in the ASP.NET Core project, and add two classes to it corresponding to these types.</span></span> <span data-ttu-id="ae828-114">你将找到 ASP.NET MVC 的这些类中的版本`/Models/IdentityModels.cs`，但我们将使用每个已迁移的项目中的类的一个文件，因为它是更清晰。</span><span class="sxs-lookup"><span data-stu-id="ae828-114">You will find the ASP.NET MVC versions of these classes in `/Models/IdentityModels.cs`, but we will use one file per class in the migrated project since that's more clear.</span></span>

<span data-ttu-id="ae828-115">ApplicationUser.cs:</span><span class="sxs-lookup"><span data-stu-id="ae828-115">ApplicationUser.cs:</span></span>

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

namespace NewMvc6Project.Models
{
  public class ApplicationUser : IdentityUser
  {
  }
}
```

<span data-ttu-id="ae828-116">ApplicationDbContext.cs:</span><span class="sxs-lookup"><span data-stu-id="ae828-116">ApplicationDbContext.cs:</span></span>

```csharp
using Microsoft.AspNetCore.Identity.EntityFramework;
using Microsoft.Data.Entity;

namespace NewMvc6Project.Models
{
  public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
  {
    public ApplicationDbContext()
    {
      Database.EnsureCreated();
    }

    protected override void OnConfiguring(DbContextOptionsBuilder options)
    {
      options.UseSqlServer();
    }
  }
}
```

<span data-ttu-id="ae828-117">ASP.NET 核心 MVC 初学者 Web 项目不包括的用户，多自定义项或 ApplicationDbContext。</span><span class="sxs-lookup"><span data-stu-id="ae828-117">The ASP.NET Core MVC Starter Web project doesn't include much customization of users, or the ApplicationDbContext.</span></span> <span data-ttu-id="ae828-118">当迁移实际的应用程序时，将需要迁移的所有自定义属性和方法的应用程序的用户和 DbContext 类，以及任何其他应用程序使用 （例如，如果你 DbContext 具有 DbSet的模型类<Album>，当然你将需要迁移唱片集类)。</span><span class="sxs-lookup"><span data-stu-id="ae828-118">When migrating a real application, you will also need to migrate all of the custom properties and methods of your application's user and DbContext classes, as well as any other Model classes your application utilizes (for example, if your DbContext has a DbSet<Album>, you will of course need to migrate the Album class).</span></span>

<span data-ttu-id="ae828-119">使用就地这些文件，Startup.cs 文件可用于通过更新其使用编译语句：</span><span class="sxs-lookup"><span data-stu-id="ae828-119">With these files in place, the Startup.cs file can be made to compile by updating its using statements:</span></span>

```csharp
using Microsoft.Framework.ConfigurationModel;
using Microsoft.AspNetCore.Hosting;
using NewMvc6Project.Models;
using Microsoft.AspNetCore.Identity;
```

<span data-ttu-id="ae828-120">我们的应用程序现已准备好支持身份验证和标识服务-它只需要向用户公开这些功能。</span><span class="sxs-lookup"><span data-stu-id="ae828-120">Our application is now ready to support authentication and identity services - it just needs to have these features exposed to users.</span></span>

## <a name="migrate-registration-and-login-logic"></a><span data-ttu-id="ae828-121">迁移注册和登录名逻辑</span><span class="sxs-lookup"><span data-stu-id="ae828-121">Migrate Registration and Login Logic</span></span>

<span data-ttu-id="ae828-122">标识配置的服务应用程序和数据访问使用 Entity Framework 和 SQL Server 配置，我们现在就将支持注册和登录名添加到应用程序。</span><span class="sxs-lookup"><span data-stu-id="ae828-122">With identity services configured for the application and data access configured using Entity Framework and SQL Server, we are now ready to add support for registration and login to the application.</span></span> <span data-ttu-id="ae828-123">回想一下，[前面的迁移过程](mvc.md#migrate-layout-file)我们注释掉对 _LoginPartial _Layout.cshtml 中的引用。</span><span class="sxs-lookup"><span data-stu-id="ae828-123">Recall that [earlier in the migration process](mvc.md#migrate-layout-file) we commented out a reference to _LoginPartial in _Layout.cshtml.</span></span> <span data-ttu-id="ae828-124">现在，它是返回到该代码中，取消注释它，并在必要的控制器视图和视图，以支持登录功能将添加的时间。</span><span class="sxs-lookup"><span data-stu-id="ae828-124">Now it's time to return to that code, uncomment it, and add in the necessary controllers and views to support login functionality.</span></span>

<span data-ttu-id="ae828-125">更新 _Layout.cshtml;取消注释@Html.Partial行：</span><span class="sxs-lookup"><span data-stu-id="ae828-125">Update _Layout.cshtml; uncomment the @Html.Partial line:</span></span>

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

<span data-ttu-id="ae828-126">现在，添加新的 MVC 视图页调用 _LoginPartial 到视图/共享文件夹：</span><span class="sxs-lookup"><span data-stu-id="ae828-126">Now, add a new MVC View Page called _LoginPartial to the Views/Shared folder:</span></span>

<span data-ttu-id="ae828-127">替换为以下代码更新 _LoginPartial.cshtml （替换其内容的所有）：</span><span class="sxs-lookup"><span data-stu-id="ae828-127">Update _LoginPartial.cshtml with the following code (replace all of its contents):</span></span>

```cshtml
@inject SignInManager<User> SignInManager
@inject UserManager<User> UserManager

@if (SignInManager.IsSignedIn(User))
{
    <form asp-area="" asp-controller="Account" asp-action="LogOff" method="post" id="logoutForm" class="navbar-right">
        <ul class="nav navbar-nav navbar-right">
            <li>
                <a asp-area="" asp-controller="Manage" asp-action="Index" title="Manage">Hello @UserManager.GetUserName(User)!</a>
            </li>
            <li>
                <button type="submit" class="btn btn-link navbar-btn navbar-link">Log off</button>
            </li>
        </ul>
    </form>
}
else
{
    <ul class="nav navbar-nav navbar-right">
        <li><a asp-area="" asp-controller="Account" asp-action="Register">Register</a></li>
        <li><a asp-area="" asp-controller="Account" asp-action="Login">Log in</a></li>
    </ul>
}
```

<span data-ttu-id="ae828-128">此时，你应能够刷新浏览器中的站点。</span><span class="sxs-lookup"><span data-stu-id="ae828-128">At this point, you should be able to refresh the site in your browser.</span></span>

## <a name="summary"></a><span data-ttu-id="ae828-129">摘要</span><span class="sxs-lookup"><span data-stu-id="ae828-129">Summary</span></span>

<span data-ttu-id="ae828-130">ASP.NET 核心引入 ASP.NET 标识功能更改。</span><span class="sxs-lookup"><span data-stu-id="ae828-130">ASP.NET Core introduces changes to the ASP.NET Identity features.</span></span> <span data-ttu-id="ae828-131">在本文中，你看到了如何将 ASP.NET 标识的身份验证和用户管理功能迁移到 ASP.NET 核心。</span><span class="sxs-lookup"><span data-stu-id="ae828-131">In this article, you have seen how to migrate the authentication and user management features of an ASP.NET Identity to ASP.NET Core.</span></span>
