---
title: "配置标识主键数据类型"
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: 2afcdf89b2a39d82a4ba72dc780be71ac98ab664
ms.sourcegitcommit: 76d42f09f3e0dd2f2105493eca6b29994aa47706
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/19/2017
---
# <a name="configure-identity-primary-keys-data-type"></a><span data-ttu-id="df082-102">配置标识主键数据类型</span><span class="sxs-lookup"><span data-stu-id="df082-102">Configure Identity primary keys data type</span></span>

<span data-ttu-id="df082-103">ASP.NET 核心标识可以轻松地配置为主键所需的数据类型。</span><span class="sxs-lookup"><span data-stu-id="df082-103">ASP.NET Core Identity allows you to easily configure the data type you want for the primary keys.</span></span> <span data-ttu-id="df082-104">默认情况下，标识使用字符串数据类型，但你可以非常快速地重写此行为。</span><span class="sxs-lookup"><span data-stu-id="df082-104">By default, Identity uses string data type but you can very quickly override this behavior.</span></span>

## <a name="how-to"></a><span data-ttu-id="df082-105">如何</span><span class="sxs-lookup"><span data-stu-id="df082-105">How to</span></span>

1.  <span data-ttu-id="df082-106">第一步是实现标识的模型中，并重写具有所需的数据类型的字符串类型。</span><span class="sxs-lookup"><span data-stu-id="df082-106">The first step is to implement the Identity's model, and override the string type with the data type you want.</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4-6&range=7-13)]

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3-5&range=7-12)]
    
2.  <span data-ttu-id="df082-107">实现与您的模型和所需的主键的数据类型标识的数据库上下文</span><span class="sxs-lookup"><span data-stu-id="df082-107">Implement the database context of Identity with your models and the data type you want for primary keys</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]
    
3.  <span data-ttu-id="df082-108">使用您的模型和数据类型时声明应用程序的启动类中的标识服务，你需要的主键</span><span class="sxs-lookup"><span data-stu-id="df082-108">Use your models and the data type you want for primary keys when you declare the identity service in your application's startup class</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-79)]
