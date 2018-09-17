---
title: ASP.NET Core 2.1 及更高版本的 Microsoft.AspNetCore.App 元包
author: Rick-Anderson
description: Microsoft.AspNetCore.App 元包包含所有受支持的 ASP.NET Core 和 Entity Framework Core 包。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 09/20/2017
uid: fundamentals/metapackage-app
ms.openlocfilehash: d27c3aa53d6edd235006dc136f09558395e15b6e
ms.sourcegitcommit: a742b55e4b8276a48b8b4394784554fecd883c84
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/13/2018
ms.locfileid: "45538448"
---
# <a name="microsoftaspnetcoreapp-metapackage-for-aspnet-core-21"></a><span data-ttu-id="3ca02-103">ASP.NET Core 2.1 的 Microsoft.AspNetCore.App 元包</span><span class="sxs-lookup"><span data-stu-id="3ca02-103">Microsoft.AspNetCore.App metapackage for ASP.NET Core 2.1</span></span>

<span data-ttu-id="3ca02-104">此功能需要面向 .NET Core 2.1 及更高版本的 ASP.NET Core 2.1。</span><span class="sxs-lookup"><span data-stu-id="3ca02-104">This feature requires ASP.NET Core 2.1 and later targeting .NET Core 2.1 and later.</span></span>

<span data-ttu-id="3ca02-105">ASP.NET Core 的 [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) [元包](/dotnet/core/packages#metapackages)：</span><span class="sxs-lookup"><span data-stu-id="3ca02-105">The [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) [metapackage](/dotnet/core/packages#metapackages) for ASP.NET Core:</span></span>

* <span data-ttu-id="3ca02-106">不包括第三方依赖项，[Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/)、[Remotion.Linq](https://www.nuget.org/packages/Remotion.Linq/) 和 [IX-Async](https://www.nuget.org/packages/System.Interactive.Async/) 除外。</span><span class="sxs-lookup"><span data-stu-id="3ca02-106">Does not include third-party dependencies except for [Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/), [Remotion.Linq](https://www.nuget.org/packages/Remotion.Linq/), and [IX-Async](https://www.nuget.org/packages/System.Interactive.Async/).</span></span> <span data-ttu-id="3ca02-107">为确保正常使用主要框架功能，这些第三方依赖项均为必要条件。</span><span class="sxs-lookup"><span data-stu-id="3ca02-107">These 3rd-party dependencies are deemed necessary to ensure the major frameworks features function.</span></span>
* <span data-ttu-id="3ca02-108">包括 ASP.NET Core 团队支持的所有软件包，包含第三方依赖项的软件包（不包括上文所述）除外。</span><span class="sxs-lookup"><span data-stu-id="3ca02-108">Includes all supported packages by the ASP.NET Core team except those that contain third-party dependencies (other than those previously mentioned).</span></span>
* <span data-ttu-id="3ca02-109">包括 Entity Framework Core 团队支持的所有软件包，包含第三方依赖项的软件包（不包括上文所述）除外。</span><span class="sxs-lookup"><span data-stu-id="3ca02-109">Includes all supported packages by the Entity Framework Core team except those that contain third-party dependencies (other than those previously mentioned).</span></span>

<span data-ttu-id="3ca02-110">`Microsoft.AspNetCore.App` 包中包含了 ASP.NET Core 2.1 及更高版本和 Entity Framework Core 2.1 及更高版本的所有功能。</span><span class="sxs-lookup"><span data-stu-id="3ca02-110">All the features of ASP.NET Core 2.1 and later and Entity Framework Core 2.1 and later are included in the `Microsoft.AspNetCore.App` package.</span></span> <span data-ttu-id="3ca02-111">面向 ASP.NET Core 2.1 及更高版本的默认项目模板使用此包。</span><span class="sxs-lookup"><span data-stu-id="3ca02-111">The default project templates targeting ASP.NET Core 2.1 and later use this package.</span></span> <span data-ttu-id="3ca02-112">建议面向 ASP.NET Core 2.1 及更高版本和 Entity Framework Core 2.1 及更高版本的应用程序使用 `Microsoft.AspNetCore.App` 包。</span><span class="sxs-lookup"><span data-stu-id="3ca02-112">We recommend applications targeting ASP.NET Core 2.1 and later and Entity Framework Core 2.1 and later use the `Microsoft.AspNetCore.App` package.</span></span>

<span data-ttu-id="3ca02-113">`Microsoft.AspNetCore.App` 元包的版本号表示 ASP.NET Core 版本和 Entity Framework Core 版本。</span><span class="sxs-lookup"><span data-stu-id="3ca02-113">The version number of the `Microsoft.AspNetCore.App` metapackage represents the ASP.NET Core version and Entity Framework Core version.</span></span>

<span data-ttu-id="3ca02-114">使用 `Microsoft.AspNetCore.App` 元包可提供用于保护应用的版本限制：</span><span class="sxs-lookup"><span data-stu-id="3ca02-114">Using the `Microsoft.AspNetCore.App` metapackage provides version restrictions that protect your app:</span></span>

* <span data-ttu-id="3ca02-115">如果包含的包对 `Microsoft.AspNetCore.App` 中的包具有可传递的（非直接）依赖项，且这些版本号不同，则 NuGet 将生成错误。</span><span class="sxs-lookup"><span data-stu-id="3ca02-115">If a package is included that has a transitive (not direct) dependency on a package in `Microsoft.AspNetCore.App`, and those version numbers differ, NuGet will generate an error.</span></span>
* <span data-ttu-id="3ca02-116">添加到应用的其他包无法更改 `Microsoft.AspNetCore.App` 中包含的包版本。</span><span class="sxs-lookup"><span data-stu-id="3ca02-116">Other packages added to your app cannot change the version of packages included in `Microsoft.AspNetCore.App`.</span></span>
* <span data-ttu-id="3ca02-117">版本一致性能确保可靠的体验。</span><span class="sxs-lookup"><span data-stu-id="3ca02-117">Version consistency ensures a reliable experience.</span></span> <span data-ttu-id="3ca02-118">`Microsoft.AspNetCore.App` 旨在防止在同一应用中结合使用未经测试的相关位版本组合。</span><span class="sxs-lookup"><span data-stu-id="3ca02-118">`Microsoft.AspNetCore.App` was designed to prevent untested version combinations of related bits being used together in the same app.</span></span>

<span data-ttu-id="3ca02-119">使用 `Microsoft.AspNetCore.App` 元包的应用程序会自动使用 ASP.NET Core 共享框架。</span><span class="sxs-lookup"><span data-stu-id="3ca02-119">Applications that use the `Microsoft.AspNetCore.App` metapackage automatically take advantage of the ASP.NET Core shared framework.</span></span> <span data-ttu-id="3ca02-120">使用 `Microsoft.AspNetCore.App` 元包时，应用程序不部署所引用的 ASP.NET Core NuGet 包中的任何资产 &mdash; .NET Core 共享框架包含这些资产。</span><span class="sxs-lookup"><span data-stu-id="3ca02-120">When you use the `Microsoft.AspNetCore.App` metapackage, **no** assets from the referenced ASP.NET Core NuGet packages are deployed with the application&mdash;the ASP.NET Core shared framework contains these assets.</span></span> <span data-ttu-id="3ca02-121">共享框架中的资产已经过预编译，以便缩短应用程序启动时间。</span><span class="sxs-lookup"><span data-stu-id="3ca02-121">The assets in the shared framework are precompiled to improve application startup time.</span></span> <span data-ttu-id="3ca02-122">有关详细信息，请参阅 [.NET Core 分发打包](/dotnet/core/build/distribution-packaging)中的“共享框架”。</span><span class="sxs-lookup"><span data-stu-id="3ca02-122">For more information, see "shared framework" in [.NET Core distribution packaging](/dotnet/core/build/distribution-packaging).</span></span>

<span data-ttu-id="3ca02-123">以下项目文件为 ASP.NET Core 引用 `Microsoft.AspNetCore.App` 元包，该文件是一种典型的 ASP.NET Core 2.1 模板：</span><span class="sxs-lookup"><span data-stu-id="3ca02-123">The following project file references the `Microsoft.AspNetCore.App` metapackage for ASP.NET Core and represents a typical ASP.NET Core 2.1 template:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" Version="2.1.4" />
  </ItemGroup>

</Project>
```

<span data-ttu-id="3ca02-124">`Microsoft.AspNetCore.App` 引用上的版本号不保证要使用共享框架的版本。</span><span class="sxs-lookup"><span data-stu-id="3ca02-124">The version number on the `Microsoft.AspNetCore.App` reference does **not** guarantee that version of the shared framework will be used.</span></span> <span data-ttu-id="3ca02-125">例如，假设指定的版本是 `2.1.1`，但安装的是 `2.1.3`。</span><span class="sxs-lookup"><span data-stu-id="3ca02-125">For example, suppose version `2.1.1` is specified, but `2.1.3` is installed.</span></span> <span data-ttu-id="3ca02-126">此情况下，应用将使用 `2.1.3`。</span><span class="sxs-lookup"><span data-stu-id="3ca02-126">In that case, the app uses `2.1.3`.</span></span> <span data-ttu-id="3ca02-127">不建议如此操作，但你可禁用前滚行为（修补程序和/或次要版本）。</span><span class="sxs-lookup"><span data-stu-id="3ca02-127">Although not recommended, you can disable roll-forward behavior (patch and/or minor).</span></span> <span data-ttu-id="3ca02-128">要详细了解包版本前滚行为，请参阅 [dotnet 主机前滚](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md)。</span><span class="sxs-lookup"><span data-stu-id="3ca02-128">For more information on package version roll-forward behavior, see [dotnet host roll forward](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md).</span></span>

## <a name="update-aspnet-core"></a><span data-ttu-id="3ca02-129">更新 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3ca02-129">Update ASP.NET Core</span></span>

<span data-ttu-id="3ca02-130">`Microsoft.AspNetCore.App` [元包](/dotnet/core/packages#metapackages)不是从 NuGet 更新的传统包。</span><span class="sxs-lookup"><span data-stu-id="3ca02-130">The `Microsoft.AspNetCore.App` [metapackage](/dotnet/core/packages#metapackages) isn't a traditional package that's updated from NuGet.</span></span> <span data-ttu-id="3ca02-131">类似于 `Microsoft.NETCore.App`，`Microsoft.AspNetCore.App` 表示共享运行时，它具有在 NuGet 之外处理的特殊版本控制语义。</span><span class="sxs-lookup"><span data-stu-id="3ca02-131">Similar to `Microsoft.NETCore.App`, `Microsoft.AspNetCore.App` represents a shared runtime, which has special versioning semantics handled outside of NuGet.</span></span> <span data-ttu-id="3ca02-132">有关详细信息，请参阅[包、元包和框架](/dotnet/core/packages)。</span><span class="sxs-lookup"><span data-stu-id="3ca02-132">For more information, see [Packages, metapackages and frameworks](/dotnet/core/packages).</span></span>

<span data-ttu-id="3ca02-133">更新 ASP.NET Core：</span><span class="sxs-lookup"><span data-stu-id="3ca02-133">To update ASP.NET Core:</span></span>

* <span data-ttu-id="3ca02-134">在开发计算机和构建服务器上：下载并安装 [.NET Core SDK](https://www.microsoft.com/net/download)。</span><span class="sxs-lookup"><span data-stu-id="3ca02-134">On development machines and build servers: Download and install the [.NET Core SDK](https://www.microsoft.com/net/download).</span></span>
* <span data-ttu-id="3ca02-135">在部署服务器上：下载并安装 [.NET Core 运行时](https://www.microsoft.com/net/download)。</span><span class="sxs-lookup"><span data-stu-id="3ca02-135">On deployment servers: Download and install the [.NET Core runtime](https://www.microsoft.com/net/download).</span></span>

 <span data-ttu-id="3ca02-136">在应用程序重启时，应用程序将前滚到最新安装的版本。</span><span class="sxs-lookup"><span data-stu-id="3ca02-136">Applications will roll forward to the latest installed version on application restart.</span></span> <span data-ttu-id="3ca02-137">无需更新项目文件中的 `Microsoft.AspNetCore.App` 版本号。</span><span class="sxs-lookup"><span data-stu-id="3ca02-137">It's not necessary to update the `Microsoft.AspNetCore.App` version number in the project file.</span></span> <span data-ttu-id="3ca02-138">有关详细信息，请参阅[依赖于框架的应用会前滚](/dotnet/core/versions/selection#framework-dependent-apps-roll-forward)。</span><span class="sxs-lookup"><span data-stu-id="3ca02-138">For more information, see [Framework-dependent apps roll forward](/dotnet/core/versions/selection#framework-dependent-apps-roll-forward).</span></span>

<span data-ttu-id="3ca02-139">如果应用程序之前使用 `Microsoft.AspNetCore.All`，请参阅[从 Microsoft.AspNetCore.All 迁移到 Microsoft.AspNetCore.App](xref:fundamentals/metapackage#migrate)。</span><span class="sxs-lookup"><span data-stu-id="3ca02-139">If your application previously used `Microsoft.AspNetCore.All`, see [Migrating from Microsoft.AspNetCore.All to Microsoft.AspNetCore.App](xref:fundamentals/metapackage#migrate).</span></span>
