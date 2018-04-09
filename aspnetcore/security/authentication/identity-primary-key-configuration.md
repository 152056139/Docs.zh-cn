---
title: 在 ASP.NET 核心中配置标识主键数据类型
author: AdrienTorris
description: 了解有关配置用于 ASP.NET 核心标识为主键的所需的数据类型的步骤。
manager: wpickett
ms.author: scaddie
ms.date: 09/28/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: db47055aecc5252dbb3991f29a8255b946deaeb7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2018
---
# <a name="configure-identity-primary-key-data-type-in-aspnet-core"></a><span data-ttu-id="8f1dc-103">在 ASP.NET 核心中配置标识主键数据类型</span><span class="sxs-lookup"><span data-stu-id="8f1dc-103">Configure Identity primary key data type in ASP.NET Core</span></span>

<span data-ttu-id="8f1dc-104">ASP.NET 核心标识可以配置用于表示为主键的数据类型。</span><span class="sxs-lookup"><span data-stu-id="8f1dc-104">ASP.NET Core Identity allows you to configure the data type used to represent a primary key.</span></span> <span data-ttu-id="8f1dc-105">标识使用`string`默认的数据类型。</span><span class="sxs-lookup"><span data-stu-id="8f1dc-105">Identity uses the `string` data type by default.</span></span> <span data-ttu-id="8f1dc-106">你可以重写此行为。</span><span class="sxs-lookup"><span data-stu-id="8f1dc-106">You can override this behavior.</span></span>

## <a name="customize-the-primary-key-data-type"></a><span data-ttu-id="8f1dc-107">自定义的主键数据类型</span><span class="sxs-lookup"><span data-stu-id="8f1dc-107">Customize the primary key data type</span></span>

1. <span data-ttu-id="8f1dc-108">创建的自定义实现[IdentityUser](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuser-1)类。</span><span class="sxs-lookup"><span data-stu-id="8f1dc-108">Create a custom implementation of the [IdentityUser](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuser-1) class.</span></span> <span data-ttu-id="8f1dc-109">它表示要用于创建用户对象的类型。</span><span class="sxs-lookup"><span data-stu-id="8f1dc-109">It represents the type to be used for creating user objects.</span></span> <span data-ttu-id="8f1dc-110">在下面的示例中，默认值`string`类型将替换`Guid`。</span><span class="sxs-lookup"><span data-stu-id="8f1dc-110">In the following example, the default `string` type is replaced with `Guid`.</span></span>

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4&range=7-13)]

2. <span data-ttu-id="8f1dc-111">创建的自定义实现[IdentityRole](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityrole-1)类。</span><span class="sxs-lookup"><span data-stu-id="8f1dc-111">Create a custom implementation of the [IdentityRole](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityrole-1) class.</span></span> <span data-ttu-id="8f1dc-112">它表示要用于创建角色对象的类型。</span><span class="sxs-lookup"><span data-stu-id="8f1dc-112">It represents the type to be used for creating role objects.</span></span> <span data-ttu-id="8f1dc-113">在下面的示例中，默认值`string`类型将替换`Guid`。</span><span class="sxs-lookup"><span data-stu-id="8f1dc-113">In the following example, the default `string` type is replaced with `Guid`.</span></span>

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3&range=7-12)]

3. <span data-ttu-id="8f1dc-114">创建一个自定义数据库上下文类。</span><span class="sxs-lookup"><span data-stu-id="8f1dc-114">Create a custom database context class.</span></span> <span data-ttu-id="8f1dc-115">它继承自用于标识的实体框架数据库上下文类。</span><span class="sxs-lookup"><span data-stu-id="8f1dc-115">It inherits from the Entity Framework database context class used for Identity.</span></span> <span data-ttu-id="8f1dc-116">`TUser`和`TRole`自变量引用分别在前一步骤中创建的自定义用户和角色类。</span><span class="sxs-lookup"><span data-stu-id="8f1dc-116">The `TUser` and `TRole` arguments reference the custom user and role classes created in the previous step, respectively.</span></span> <span data-ttu-id="8f1dc-117">`Guid`为主键定义的数据类型。</span><span class="sxs-lookup"><span data-stu-id="8f1dc-117">The `Guid` data type is defined for the primary key.</span></span>

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]

4. <span data-ttu-id="8f1dc-118">在应用程序的启动类添加身份验证服务时，请注册自定义数据库上下文类。</span><span class="sxs-lookup"><span data-stu-id="8f1dc-118">Register the custom database context class when adding the Identity service in the app's startup class.</span></span>

   #### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8f1dc-119">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8f1dc-119">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
    <span data-ttu-id="8f1dc-120">`AddEntityFrameworkStores`方法不接受`TKey`自变量，因为它未在 ASP.NET Core 1.x。</span><span class="sxs-lookup"><span data-stu-id="8f1dc-120">The `AddEntityFrameworkStores` method doesn't accept a `TKey` argument as it did in ASP.NET Core 1.x.</span></span> <span data-ttu-id="8f1dc-121">主键的数据类型推断通过分析`DbContext`对象。</span><span class="sxs-lookup"><span data-stu-id="8f1dc-121">The primary key's data type is inferred by analyzing the `DbContext` object.</span></span>

    [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=6-8&range=25-37)]

   #### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8f1dc-122">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8f1dc-122">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
    <span data-ttu-id="8f1dc-123">`AddEntityFrameworkStores`方法接受`TKey`，该值指示主键的数据类型的自变量。</span><span class="sxs-lookup"><span data-stu-id="8f1dc-123">The `AddEntityFrameworkStores` method accepts a `TKey` argument indicating the primary key's data type.</span></span>

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-55)]

   * * *
## <a name="test-the-changes"></a><span data-ttu-id="8f1dc-124">测试更改</span><span class="sxs-lookup"><span data-stu-id="8f1dc-124">Test the changes</span></span>

<span data-ttu-id="8f1dc-125">在完成配置更改，表示为主键的属性将反映新的数据类型。</span><span class="sxs-lookup"><span data-stu-id="8f1dc-125">Upon completion of the configuration changes, the property representing the primary key reflects the new data type.</span></span> <span data-ttu-id="8f1dc-126">下面的示例演示如何访问一个 MVC 控制器中的属性。</span><span class="sxs-lookup"><span data-stu-id="8f1dc-126">The following example demonstrates accessing the property in an MVC controller.</span></span>

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Controllers/AccountController.cs?name=snippet_GetCurrentUserId&highlight=6)]
