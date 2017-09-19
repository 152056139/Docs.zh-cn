---
title: "在 ASP.NET Core 上的标识简介"
author: rick-anderson
description: "ASP.NET Core 应用上使用标识"
keywords: "ASP.NET 核心，标识，授权安全"
ms.author: riande
manager: wpickett
ms.date: 07/07/2017
ms.topic: article
ms.assetid: cf119f21-1a2b-49a2-b052-547ccb66ee83
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity
ms.openlocfilehash: 0c17daa96bc69dc0b8393811a4dfe0e5dc4a1884
ms.sourcegitcommit: 67f54fabbfa4e3942f5bfe1f8a7fdfe4a7a75358
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/19/2017
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="d113e-104">在 ASP.NET Core 上的标识简介</span><span class="sxs-lookup"><span data-stu-id="d113e-104">Introduction to Identity on ASP.NET Core</span></span>

<span data-ttu-id="d113e-105">通过[Pranav Rastogi](https://github.com/rustd)， [Rick Anderson](https://twitter.com/RickAndMSFT)， [Tom Dykstra](https://github.com/tdykstra)，Jon Galloway[艾力克 Reitan](https://github.com/Erikre)，和[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="d113e-105">By [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Jon Galloway, [Erik Reitan](https://github.com/Erikre), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="d113e-106">ASP.NET 核心标识是允许你向你的应用程序添加登录功能的成员身份系统。</span><span class="sxs-lookup"><span data-stu-id="d113e-106">ASP.NET Core Identity is a membership system which allows you to add login functionality to your application.</span></span> <span data-ttu-id="d113e-107">用户可以创建帐户和登录名使用的用户名和密码或它们可以使用如 Facebook、 Google、 Microsoft 帐户、 Twitter 或其他外部登录提供程序。</span><span class="sxs-lookup"><span data-stu-id="d113e-107">Users can create an account and login with a user name and password or they can use an external login provider such as Facebook, Google, Microsoft Account, Twitter or others.</span></span>

<span data-ttu-id="d113e-108">你可以配置 ASP.NET 核心标识来使用 SQL Server 数据库来存储用户名、 密码和配置文件数据。</span><span class="sxs-lookup"><span data-stu-id="d113e-108">You can configure ASP.NET Core Identity to use a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="d113e-109">或者，你可以使用你自己的持久存储区，例如 Azure 表存储。</span><span class="sxs-lookup"><span data-stu-id="d113e-109">Alternatively, you can use your own persistent store, for example Azure Table Storage.</span></span> <span data-ttu-id="d113e-110">本文档包含用于 Visual Studio 以及有关使用 CLI 的说明。</span><span class="sxs-lookup"><span data-stu-id="d113e-110">This document contains instructions for Visual Studio and for using the CLI.</span></span>

## <a name="overview-of-identity"></a><span data-ttu-id="d113e-111">标识的概述</span><span class="sxs-lookup"><span data-stu-id="d113e-111">Overview of Identity</span></span>

<span data-ttu-id="d113e-112">在本主题中，你将了解如何使用 ASP.NET 核心标识来添加功能，用于注册、 登录，并注销用户。</span><span class="sxs-lookup"><span data-stu-id="d113e-112">In this topic, you'll learn how to use ASP.NET Core Identity to add functionality to register, log in, and log out a user.</span></span> <span data-ttu-id="d113e-113">有关创建使用 ASP.NET 核心标识应用的更多详细说明，请参阅本文末尾的后续步骤部分。</span><span class="sxs-lookup"><span data-stu-id="d113e-113">For more detailed instructions about creating apps using ASP.NET Core Identity, see the Next Steps section at the end of this article.</span></span>

1.  <span data-ttu-id="d113e-114">使用单个用户帐户创建一个 ASP.NET 核心 Web 应用程序项目。</span><span class="sxs-lookup"><span data-stu-id="d113e-114">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

    # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d113e-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d113e-115">Visual Studio</span></span>](#tab/visual-studio)
    <span data-ttu-id="d113e-116">在 Visual Studio 中，选择**文件** -> **新建** -> **项目**。</span><span class="sxs-lookup"><span data-stu-id="d113e-116">In Visual Studio, select **File** -> **New** -> **Project**.</span></span> <span data-ttu-id="d113e-117">选择**ASP.NET Web 应用程序**从**新项目**对话框。</span><span class="sxs-lookup"><span data-stu-id="d113e-117">Select the **ASP.NET Web Application** from the **New Project** dialog box.</span></span> <span data-ttu-id="d113e-118">选择 ASP.NET Core **Web 应用程序**与**单个用户帐户**作为身份验证方法。</span><span class="sxs-lookup"><span data-stu-id="d113e-118">Selecting an ASP.NET Core **Web Application** with **Individual User Accounts** as the authentication method.</span></span>

    <span data-ttu-id="d113e-119">注意： 你必须选择**单个用户帐户**。</span><span class="sxs-lookup"><span data-stu-id="d113e-119">Note: You must select **Individual User Accounts**.</span></span>
 
    ![“新建项目”对话框](identity/_static/01-mvc.png)
    
    # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d113e-121">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="d113e-121">.NET Core CLI</span></span>](#tab/netcore-cli)
    <span data-ttu-id="d113e-122">如果使用.NET 核心 CLI，创建新的项目使用``dotnet new mvc --auth Individual``。</span><span class="sxs-lookup"><span data-stu-id="d113e-122">If using the .NET Core CLI, create the new project using ``dotnet new mvc --auth Individual``.</span></span> <span data-ttu-id="d113e-123">这将创建一个新的项目与 Visual Studio 将创建相同的标识模板代码。</span><span class="sxs-lookup"><span data-stu-id="d113e-123">This will create a new project with the same Identity template code Visual Studio creates.</span></span>
 
    <span data-ttu-id="d113e-124">创建的项目包含`Microsoft.AspNetCore.Identity.EntityFrameworkCore`包，其中将保持的标识数据和架构与 SQL Server 使用[实体框架核心](https://docs.microsoft.com/ef/)。</span><span class="sxs-lookup"><span data-stu-id="d113e-124">The created project contains the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` package, which will persist the Identity data and schema to SQL Server using [Entity Framework Core](https://docs.microsoft.com/ef/).</span></span>
    
    ---
 
2.  <span data-ttu-id="d113e-125">配置标识服务，并添加中的中间件`Startup`。</span><span class="sxs-lookup"><span data-stu-id="d113e-125">Configure Identity services and add middleware in `Startup`.</span></span>

    <span data-ttu-id="d113e-126">标识服务添加到中的应用程序`ConfigureServices`中的方法`Startup`类：</span><span class="sxs-lookup"><span data-stu-id="d113e-126">The Identity services are added to the application in the `ConfigureServices` method in the `Startup` class:</span></span>

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d113e-127">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d113e-127">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]
    
    <span data-ttu-id="d113e-128">这些服务都提供给应用程序通过[依赖关系注入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="d113e-128">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
    
    <span data-ttu-id="d113e-129">通过调用情况下，启用应用程序的标识`UseAuthentication`中`Configure`方法。</span><span class="sxs-lookup"><span data-stu-id="d113e-129">Identity is enabled for the application by calling `UseAuthentication` in the `Configure` method.</span></span> <span data-ttu-id="d113e-130">`UseAuthentication`添加了身份验证[中间件](xref:fundamentals/middleware)向请求管道。</span><span class="sxs-lookup"><span data-stu-id="d113e-130">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware) to the request pipeline.</span></span>
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d113e-131">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d113e-131">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-34)]
    
    <span data-ttu-id="d113e-132">这些服务都提供给应用程序通过[依赖关系注入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="d113e-132">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
    
    <span data-ttu-id="d113e-133">通过调用情况下，启用应用程序的标识`UseIdentity`中`Configure`方法。</span><span class="sxs-lookup"><span data-stu-id="d113e-133">Identity is enabled for the application by calling `UseIdentity` in the `Configure` method.</span></span> <span data-ttu-id="d113e-134">`UseIdentity`添加了基于 cookie 的身份验证[中间件](xref:fundamentals/middleware)向请求管道。</span><span class="sxs-lookup"><span data-stu-id="d113e-134">`UseIdentity` adds cookie-based authentication [middleware](xref:fundamentals/middleware) to the request pipeline.</span></span>
        
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]
    
    ---
     
    <span data-ttu-id="d113e-135">有关进程的应用程序启动的详细信息，请参阅[应用程序启动](xref:fundamentals/startup)。</span><span class="sxs-lookup"><span data-stu-id="d113e-135">For more information about the application start up process, see [Application Startup](xref:fundamentals/startup).</span></span>

3.  <span data-ttu-id="d113e-136">创建一个用户。</span><span class="sxs-lookup"><span data-stu-id="d113e-136">Create a user.</span></span>

    <span data-ttu-id="d113e-137">启动应用程序，然后单击**注册**链接。</span><span class="sxs-lookup"><span data-stu-id="d113e-137">Launch the application and then click on the **Register** link.</span></span>

    <span data-ttu-id="d113e-138">如果这是你要执行此操作的第一个时间，你可能需要运行迁移。</span><span class="sxs-lookup"><span data-stu-id="d113e-138">If this is the first time you're performing this action, you may be required to run migrations.</span></span> <span data-ttu-id="d113e-139">应用程序会提示您**应用迁移**:</span><span class="sxs-lookup"><span data-stu-id="d113e-139">The application prompts you to **Apply Migrations**:</span></span>
    
    ![将应用迁移网页](identity/_static/apply-migrations.png)
    
    <span data-ttu-id="d113e-141">或者，你可以测试与你的应用不持久的数据库的情况下通过使用内存中数据库的 ASP.NET 核心标识。</span><span class="sxs-lookup"><span data-stu-id="d113e-141">Alternately, you can test using ASP.NET Core Identity with your app without a persistent database by using an in-memory database.</span></span> <span data-ttu-id="d113e-142">若要使用的内存中数据库，添加``Microsoft.EntityFrameworkCore.InMemory``包到你的应用程序和修改您的应用程序调用``AddDbContext``中``ConfigureServices``，如下所示：</span><span class="sxs-lookup"><span data-stu-id="d113e-142">To use an in-memory database, add the ``Microsoft.EntityFrameworkCore.InMemory`` package to your app and modify your app's call to ``AddDbContext`` in ``ConfigureServices`` as follows:</span></span>

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseInMemoryDatabase(Guid.NewGuid().ToString()));
    ```
    
    <span data-ttu-id="d113e-143">当用户单击**注册**链接，``Register``上调用操作``AccountController``。</span><span class="sxs-lookup"><span data-stu-id="d113e-143">When the user clicks the **Register** link, the ``Register`` action is invoked on ``AccountController``.</span></span> <span data-ttu-id="d113e-144">``Register``操作通过调用创建用户`CreateAsync`上`_userManager`对象 (提供给``AccountController``通过依赖关系注入):</span><span class="sxs-lookup"><span data-stu-id="d113e-144">The ``Register`` action creates the user by calling `CreateAsync` on the  `_userManager` object (provided to ``AccountController`` by dependency injection):</span></span>
 
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

    <span data-ttu-id="d113e-145">如果已成功创建用户，用户记录通过调用``_signInManager.SignInAsync``。</span><span class="sxs-lookup"><span data-stu-id="d113e-145">If the user was created successfully, the user is logged in by the call to ``_signInManager.SignInAsync``.</span></span>

    <span data-ttu-id="d113e-146">**注意：**请参阅[帐户确认](xref:security/authentication/accconfirm#prevent-login-at-registration)有关步骤，以防止在注册的即时登录名。</span><span class="sxs-lookup"><span data-stu-id="d113e-146">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>
 
4.  <span data-ttu-id="d113e-147">登录。</span><span class="sxs-lookup"><span data-stu-id="d113e-147">Log in.</span></span>
 
    <span data-ttu-id="d113e-148">用户可以通过单击登录**登录**链接顶部的站点，或可能的登录页导航它们，如果用户尝试访问要求获得授权的站点的一部分。</span><span class="sxs-lookup"><span data-stu-id="d113e-148">Users can sign in by clicking the **Log in** link at the top of the site, or they may be navigated to the Login page if they attempt to access a part of the site that requires authorization.</span></span> <span data-ttu-id="d113e-149">当用户提交的登录页中上, 窗体``AccountController````Login``调用操作。</span><span class="sxs-lookup"><span data-stu-id="d113e-149">When the user submits the form on the Login page, the ``AccountController`` ``Login`` action is called.</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]
 
    <span data-ttu-id="d113e-150">``Login``操作调用``PasswordSignInAsync``上``_signInManager``对象 (提供给``AccountController``通过依赖关系注入)。</span><span class="sxs-lookup"><span data-stu-id="d113e-150">The ``Login`` action calls ``PasswordSignInAsync`` on the ``_signInManager`` object (provided to ``AccountController`` by dependency injection).</span></span>
 
    <span data-ttu-id="d113e-151">基``Controller``类会公开``User``你可以从控制器方法访问的属性。</span><span class="sxs-lookup"><span data-stu-id="d113e-151">The base ``Controller`` class exposes a ``User`` property that you can access from controller methods.</span></span> <span data-ttu-id="d113e-152">例如，可以枚举`User.Claims`并做出授权决策。</span><span class="sxs-lookup"><span data-stu-id="d113e-152">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="d113e-153">有关详细信息，请参阅[授权](xref:security/authorization/index)。</span><span class="sxs-lookup"><span data-stu-id="d113e-153">For more information, see [Authorization](xref:security/authorization/index).</span></span>
 
5.  <span data-ttu-id="d113e-154">注销。</span><span class="sxs-lookup"><span data-stu-id="d113e-154">Log out.</span></span>
 
    <span data-ttu-id="d113e-155">单击**注销**链接调用`LogOut`操作。</span><span class="sxs-lookup"><span data-stu-id="d113e-155">Clicking the **Log out** link calls the `LogOut` action.</span></span>
 
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]
 
    <span data-ttu-id="d113e-156">前面的代码调用上面`_signInManager.SignOutAsync`方法。</span><span class="sxs-lookup"><span data-stu-id="d113e-156">The preceding code above calls the `_signInManager.SignOutAsync` method.</span></span> <span data-ttu-id="d113e-157">`SignOutAsync`方法清除在 cookie 中存储的用户的声明。</span><span class="sxs-lookup"><span data-stu-id="d113e-157">The `SignOutAsync` method clears the user's claims stored in a cookie.</span></span>
 
6.  <span data-ttu-id="d113e-158">配置。</span><span class="sxs-lookup"><span data-stu-id="d113e-158">Configuration.</span></span>

    <span data-ttu-id="d113e-159">标识具有一些你可以在应用程序的启动类中重写的默认行为。</span><span class="sxs-lookup"><span data-stu-id="d113e-159">Identity has some default behaviors that you can override in your application's startup class.</span></span> <span data-ttu-id="d113e-160">不需要配置``IdentityOptions``如果你使用的默认行为。</span><span class="sxs-lookup"><span data-stu-id="d113e-160">You do not need to configure ``IdentityOptions`` if you are using the default behaviors.</span></span>

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d113e-161">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d113e-161">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d113e-162">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d113e-162">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=13-34)]

    ---
    
    <span data-ttu-id="d113e-163">有关如何配置标识的详细信息，请参阅[配置标识](xref:security/authentication/identity-configuration)。</span><span class="sxs-lookup"><span data-stu-id="d113e-163">For more information about how to configure Identity, see [Configure Identity](xref:security/authentication/identity-configuration).</span></span>
    
    <span data-ttu-id="d113e-164">你还可以配置的数据类型的主键，请参阅[配置标识主键数据类型](xref:security/authentication/identity-primary-key-configuration)。</span><span class="sxs-lookup"><span data-stu-id="d113e-164">You also can configure the data type of the primary key, see [Configure Identity primary keys data type](xref:security/authentication/identity-primary-key-configuration).</span></span>
 
7.  <span data-ttu-id="d113e-165">查看数据库。</span><span class="sxs-lookup"><span data-stu-id="d113e-165">View the database.</span></span>

    <span data-ttu-id="d113e-166">如果你的应用使用 SQL Server 数据库 （默认在 Windows 上以及为 Visual Studio 用户），你可以查看数据库中创建的应用。</span><span class="sxs-lookup"><span data-stu-id="d113e-166">If your app is using a SQL Server database (the default on Windows and for Visual Studio users), you can view the database the app created.</span></span> <span data-ttu-id="d113e-167">你可以使用**SQL Server Management Studio**。</span><span class="sxs-lookup"><span data-stu-id="d113e-167">You can use **SQL Server Management Studio**.</span></span> <span data-ttu-id="d113e-168">或者，从 Visual Studio 中，选择**视图** -> **SQL Server 对象资源管理器**。</span><span class="sxs-lookup"><span data-stu-id="d113e-168">Alternatively, from Visual Studio, select **View** -> **SQL Server Object Explorer**.</span></span> <span data-ttu-id="d113e-169">连接到**(localdb) \MSSQLLocalDB**。</span><span class="sxs-lookup"><span data-stu-id="d113e-169">Connect to **(localdb)\MSSQLLocalDB**.</span></span> <span data-ttu-id="d113e-170">名称匹配的数据库* *aspnet-<*的你的项目名称*>-<*日期字符串*> * * 显示。</span><span class="sxs-lookup"><span data-stu-id="d113e-170">The database with a name matching **aspnet-<*name of your project*>-<*date string*>** is displayed.</span></span>

    ![在 AspNetUsers 数据库表的上下文菜单](identity/_static/04-db.png)
    
    <span data-ttu-id="d113e-172">展开的数据库并将其**表**，然后右键单击**dbo。AspNetUsers**表，然后选择**查看数据**。</span><span class="sxs-lookup"><span data-stu-id="d113e-172">Expand the database and its **Tables**, then right-click the **dbo.AspNetUsers** table and select **View Data**.</span></span>

## <a name="identity-components"></a><span data-ttu-id="d113e-173">标识组件</span><span class="sxs-lookup"><span data-stu-id="d113e-173">Identity Components</span></span>

<span data-ttu-id="d113e-174">标识系统的主引用程序集是`Microsoft.AspNetCore.Identity`。</span><span class="sxs-lookup"><span data-stu-id="d113e-174">The primary reference assembly for the Identity system is `Microsoft.AspNetCore.Identity`.</span></span> <span data-ttu-id="d113e-175">此程序包包含 ASP.NET 核心标识接口的核心集，包括`Microsoft.AspNetCore.Identity.EntityFrameworkCore`。</span><span class="sxs-lookup"><span data-stu-id="d113e-175">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

<span data-ttu-id="d113e-176">这些依赖关系需要 ASP.NET Core 应用程序中使用的标识系统：</span><span class="sxs-lookup"><span data-stu-id="d113e-176">These dependencies are needed to use the Identity system in ASP.NET Core applications:</span></span>

* <span data-ttu-id="d113e-177">`Microsoft.AspNetCore.Identity.EntityFrameworkCore`-包含要用于实体框架核心标识所需的类型。</span><span class="sxs-lookup"><span data-stu-id="d113e-177">`Microsoft.AspNetCore.Identity.EntityFrameworkCore` - Contains the required types to use Identity with Entity Framework Core.</span></span>

* <span data-ttu-id="d113e-178">`Microsoft.EntityFrameworkCore.SqlServer`实体框架的核心是用于类似于 SQL Server 关系数据库的 Microsoft 的推荐使用此数据访问技术。</span><span class="sxs-lookup"><span data-stu-id="d113e-178">`Microsoft.EntityFrameworkCore.SqlServer` - Entity Framework Core is Microsoft's recommended data access technology for relational databases like SQL Server.</span></span> <span data-ttu-id="d113e-179">对于测试，你可以使用`Microsoft.EntityFrameworkCore.InMemory`。</span><span class="sxs-lookup"><span data-stu-id="d113e-179">For testing, you can use `Microsoft.EntityFrameworkCore.InMemory`.</span></span>

* <span data-ttu-id="d113e-180">`Microsoft.AspNetCore.Authentication.Cookies`-使应用可以使用基于 cookie 的身份验证的中间。</span><span class="sxs-lookup"><span data-stu-id="d113e-180">`Microsoft.AspNetCore.Authentication.Cookies` - Middleware that enables an app to use cookie-based authentication.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="d113e-181">迁移到 ASP.NET 核心标识</span><span class="sxs-lookup"><span data-stu-id="d113e-181">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="d113e-182">有关其他信息和指南迁移你现有的标识存储，请参阅[迁移身份验证和标识](xref:migration/identity)。</span><span class="sxs-lookup"><span data-stu-id="d113e-182">For additional information and guidance on migrating your existing Identity store see [Migrating Authentication and Identity](xref:migration/identity).</span></span>

## <a name="next-steps"></a><span data-ttu-id="d113e-183">后续步骤</span><span class="sxs-lookup"><span data-stu-id="d113e-183">Next Steps</span></span>

* [<span data-ttu-id="d113e-184">迁移身份验证和标识</span><span class="sxs-lookup"><span data-stu-id="d113e-184">Migrating Authentication and Identity</span></span>](xref:migration/identity)
* [<span data-ttu-id="d113e-185">帐户确认和密码恢复</span><span class="sxs-lookup"><span data-stu-id="d113e-185">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="d113e-186">使用 SMS 设置双因素身份验证</span><span class="sxs-lookup"><span data-stu-id="d113e-186">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="d113e-187">启用使用 Facebook、Google 和其他外部提供程序的身份验证</span><span class="sxs-lookup"><span data-stu-id="d113e-187">Enabling authentication using Facebook, Google and other external providers</span></span>](xref:security/authentication/social/index)
