---
title: "Visual Studio 发布 ASP.NET 核心应用程序部署的配置文件"
author: rick-anderson
description: "了解如何创建 Visual Studio 中发布 ASP.NET Core 应用的配置文件。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 09/26/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: d2c4ec317f235c6d042bd130dbf79f6cb5e2d47d
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a><span data-ttu-id="c516f-103">Visual Studio 发布 ASP.NET 核心应用程序部署的配置文件</span><span class="sxs-lookup"><span data-stu-id="c516f-103">Visual Studio publish profiles for ASP.NET Core app deployment</span></span>

<span data-ttu-id="c516f-104">作者：[Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c516f-104">By [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c516f-105">本文重点介绍如何使用 Visual Studio 2017 创建发布配置文件。</span><span class="sxs-lookup"><span data-stu-id="c516f-105">This article focuses on using Visual Studio 2017 to create publish profiles.</span></span> <span data-ttu-id="c516f-106">可以从 MSBuild 和 Visual Studio 2017 运行使用 Visual Studio 创建的发布配置文件。</span><span class="sxs-lookup"><span data-stu-id="c516f-106">The publish profiles created with Visual Studio can be run from MSBuild and Visual Studio 2017.</span></span> <span data-ttu-id="c516f-107">本文提供发布过程的详细信息。</span><span class="sxs-lookup"><span data-stu-id="c516f-107">The article provides details of the publishing process.</span></span> <span data-ttu-id="c516f-108">有关发布到 Azure 的说明，请参阅[使用 Visual Studio 将 ASP.NET Core Web 应用发布到 Azure App Service](xref:tutorials/publish-to-azure-webapp-using-vs)。</span><span class="sxs-lookup"><span data-stu-id="c516f-108">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on publishing to Azure.</span></span>

<span data-ttu-id="c516f-109">使用命令 `dotnet new mvc` 创建以下 .csproj 文件：</span><span class="sxs-lookup"><span data-stu-id="c516f-109">The following *.csproj* file was created with the command `dotnet new mvc`:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c516f-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c516f-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.0" />
  </ItemGroup>

  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Tools" Version="2.0.0" />
  </ItemGroup>

</Project>
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c516f-111">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c516f-111">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp1.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore" Version="1.1.5" />
    <PackageReference Include="Microsoft.AspNetCore.Mvc" Version="1.1.6" />
    <PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="1.1.3" />
  </ItemGroup>

</Project>
```

---

<span data-ttu-id="c516f-112">上述标记的 `<Project>` 元素中的 `Sdk` 属性（位于第一行）执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="c516f-112">The `Sdk` attribute in the `<Project>` element (in the first line) of the markup above does the following:</span></span>

* <span data-ttu-id="c516f-113">导入中的属性文件*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props*开头。</span><span class="sxs-lookup"><span data-stu-id="c516f-113">Imports the properties file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* at the beginning.</span></span>
* <span data-ttu-id="c516f-114">在结束时从 $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets 导入目标文件。</span><span class="sxs-lookup"><span data-stu-id="c516f-114">Imports the targets file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* at the end.</span></span>

<span data-ttu-id="c516f-115">`MSBuildSDKsPath`（装有 Visual Studio 2017 Enterprise）的默认位置是 %programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks 文件夹。</span><span class="sxs-lookup"><span data-stu-id="c516f-115">The default location for `MSBuildSDKsPath` (with Visual Studio 2017 Enterprise) is the *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* folder.</span></span>

<span data-ttu-id="c516f-116">`Microsoft.NET.Sdk.Web` 依赖于：</span><span class="sxs-lookup"><span data-stu-id="c516f-116">`Microsoft.NET.Sdk.Web` depends on:</span></span>

* <span data-ttu-id="c516f-117">*Microsoft.NET.Sdk.Web.ProjectSystem*</span><span class="sxs-lookup"><span data-stu-id="c516f-117">*Microsoft.NET.Sdk.Web.ProjectSystem*</span></span>
* <span data-ttu-id="c516f-118">*Microsoft.NET.Sdk.Publish*</span><span class="sxs-lookup"><span data-stu-id="c516f-118">*Microsoft.NET.Sdk.Publish*</span></span>

<span data-ttu-id="c516f-119">这将导致产生以下属性和要导入的目标：</span><span class="sxs-lookup"><span data-stu-id="c516f-119">Which causes the following properties and targets to be imported:</span></span>

* <span data-ttu-id="c516f-120">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props</span><span class="sxs-lookup"><span data-stu-id="c516f-120">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props</span></span>
* <span data-ttu-id="c516f-121">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets</span><span class="sxs-lookup"><span data-stu-id="c516f-121">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets</span></span>
* <span data-ttu-id="c516f-122">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props</span><span class="sxs-lookup"><span data-stu-id="c516f-122">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props</span></span>
* <span data-ttu-id="c516f-123">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets</span><span class="sxs-lookup"><span data-stu-id="c516f-123">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets</span></span>

<span data-ttu-id="c516f-124">发布目标导入右侧集的目标根据使用的发布方法。</span><span class="sxs-lookup"><span data-stu-id="c516f-124">Publish targets import the right set of targets based on the publish method used.</span></span>

<span data-ttu-id="c516f-125">MSBuild 或 Visual Studio 加载项目时，执行下列高级别操作：</span><span class="sxs-lookup"><span data-stu-id="c516f-125">When MSBuild or Visual Studio loads a project, the following high level actions are performed:</span></span>

* <span data-ttu-id="c516f-126">生成项目</span><span class="sxs-lookup"><span data-stu-id="c516f-126">Build project</span></span>
* <span data-ttu-id="c516f-127">计算要发布的文件</span><span class="sxs-lookup"><span data-stu-id="c516f-127">Compute files to publish</span></span>
* <span data-ttu-id="c516f-128">将文件发布到目标</span><span class="sxs-lookup"><span data-stu-id="c516f-128">Publish files to destination</span></span>

### <a name="compute-project-items"></a><span data-ttu-id="c516f-129">计算项目项</span><span class="sxs-lookup"><span data-stu-id="c516f-129">Compute project items</span></span>

<span data-ttu-id="c516f-130">加载项目时，将计算项目项（文件）。</span><span class="sxs-lookup"><span data-stu-id="c516f-130">When the project is loaded, the project items (files) are computed.</span></span> <span data-ttu-id="c516f-131">`item type` 属性确定如何处理该文件。</span><span class="sxs-lookup"><span data-stu-id="c516f-131">The `item type` attribute determines how the file is processed.</span></span> <span data-ttu-id="c516f-132">默认情况下，.cs 文件包含在 `Compile` 项列表内。</span><span class="sxs-lookup"><span data-stu-id="c516f-132">By default, *.cs* files are included in the `Compile` item list.</span></span> <span data-ttu-id="c516f-133">会对 `Compile` 项列表中的文件进行编译。</span><span class="sxs-lookup"><span data-stu-id="c516f-133">Files in the `Compile` item list are compiled.</span></span>

<span data-ttu-id="c516f-134">`Content`项列表包含发布除了生成输出的文件。</span><span class="sxs-lookup"><span data-stu-id="c516f-134">The `Content` item list contains files that are published in addition to the build outputs.</span></span> <span data-ttu-id="c516f-135">默认情况下，文件与模式匹配的`wwwroot/**`都将纳入`Content`项。</span><span class="sxs-lookup"><span data-stu-id="c516f-135">By default, files matching the pattern `wwwroot/**` are included in the `Content` item.</span></span> <span data-ttu-id="c516f-136">[wwwroot /\* \*是组合模式](https://gruntjs.com/configuring-tasks#globbing-patterns)，它指定中的所有文件*wwwroot*文件夹**和**子文件夹。</span><span class="sxs-lookup"><span data-stu-id="c516f-136">[wwwroot/\*\* is a globbing pattern](https://gruntjs.com/configuring-tasks#globbing-patterns) that specifies all files in the *wwwroot* folder **and** subfolders.</span></span> <span data-ttu-id="c516f-137">若要将文件显式添加到发布列表，请直接在 .csproj 文件中添加文件，如[加入文件](#including-files)中所示。</span><span class="sxs-lookup"><span data-stu-id="c516f-137">To explicitly add a file to the publish list, add the file directly in the *.csproj* file as shown in [Including Files](#including-files).</span></span>

<span data-ttu-id="c516f-138">选择时**发布**Visual Studio 中或从命令行发布时的按钮：</span><span class="sxs-lookup"><span data-stu-id="c516f-138">When selecting the **Publish** button in Visual Studio or when publishing from the command line:</span></span>

* <span data-ttu-id="c516f-139">计算属性/项目（需要生成的文件）。</span><span class="sxs-lookup"><span data-stu-id="c516f-139">The properties/items are computed (the files that are needed to build).</span></span>
* <span data-ttu-id="c516f-140">仅限 Visual Studio：还原 NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="c516f-140">Visual Studio only: NuGet packages are restored.</span></span> <span data-ttu-id="c516f-141">（用户需要在 CLI 上执行显式还原。）</span><span class="sxs-lookup"><span data-stu-id="c516f-141">(Restore needs to be explicit by the user on the CLI.)</span></span>
* <span data-ttu-id="c516f-142">生成项目。</span><span class="sxs-lookup"><span data-stu-id="c516f-142">The project builds.</span></span>
* <span data-ttu-id="c516f-143">计算发布项（需要发布的文件）。</span><span class="sxs-lookup"><span data-stu-id="c516f-143">The publish items are computed (the files that are needed to publish).</span></span>
* <span data-ttu-id="c516f-144">发布项目。</span><span class="sxs-lookup"><span data-stu-id="c516f-144">The project is published.</span></span> <span data-ttu-id="c516f-145">（计算的文件将被复制到发布目标。）</span><span class="sxs-lookup"><span data-stu-id="c516f-145">(The computed files are copied to the publish destination.)</span></span>

<span data-ttu-id="c516f-146">当 ASP.NET Core 项目引用`Microsoft.NET.Sdk.Web`在项目文件中， *app_offline.htm*文件放置在 web 应用程序目录的根目录。</span><span class="sxs-lookup"><span data-stu-id="c516f-146">When an ASP.NET Core project references `Microsoft.NET.Sdk.Web` in the project file, an *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="c516f-147">该文件存在时，ASP.NET Core 模块会在部署过程中正常关闭该应用并提供 app_offline.htm 文件。</span><span class="sxs-lookup"><span data-stu-id="c516f-147">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="c516f-148">有关详细信息，请参阅 [ASP.NET Core 模块配置参考](xref:host-and-deploy/aspnet-core-module#appofflinehtm)。</span><span class="sxs-lookup"><span data-stu-id="c516f-148">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#appofflinehtm).</span></span>

## <a name="basic-command-line-publishing"></a><span data-ttu-id="c516f-149">基本的命令行发布</span><span class="sxs-lookup"><span data-stu-id="c516f-149">Basic command-line publishing</span></span>

<span data-ttu-id="c516f-150">命令行发布适用于所有支持的.NET 核心平台，而且不需要 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="c516f-150">Command-line publishing works on all .NET Core supported platforms and doesn't require Visual Studio.</span></span> <span data-ttu-id="c516f-151">在下面的示例中[dotnet 发布](/dotnet/core/tools/dotnet-publish)从项目目录运行命令 (其中包含*.csproj*文件)。</span><span class="sxs-lookup"><span data-stu-id="c516f-151">In the samples below, the [dotnet publish](/dotnet/core/tools/dotnet-publish) command is run from the project directory (which contains the *.csproj* file).</span></span> <span data-ttu-id="c516f-152">如果未在项目文件夹中，显式传入的项目文件路径。</span><span class="sxs-lookup"><span data-stu-id="c516f-152">If not in the project folder, explicitly pass in the project file path.</span></span> <span data-ttu-id="c516f-153">例如:</span><span class="sxs-lookup"><span data-stu-id="c516f-153">For example:</span></span>

```console
dotnet publish c:/webs/web1
```

<span data-ttu-id="c516f-154">运行以下命令以创建并发布 Web 应用：</span><span class="sxs-lookup"><span data-stu-id="c516f-154">Run the following commands to create and publish a web app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c516f-155">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c516f-155">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```console
dotnet new mvc
dotnet publish
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c516f-156">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c516f-156">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```console
dotnet new mvc
dotnet restore
dotnet publish
```

---

<span data-ttu-id="c516f-157">[Dotnet 发布](/dotnet/core/tools/dotnet-publish)命令生成类似于以下输出：</span><span class="sxs-lookup"><span data-stu-id="c516f-157">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command produces output similar to the following:</span></span>

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

<span data-ttu-id="c516f-158">默认发布文件夹为 `bin\$(Configuration)\netcoreapp<version>\publish`。</span><span class="sxs-lookup"><span data-stu-id="c516f-158">The default publish folder is `bin\$(Configuration)\netcoreapp<version>\publish`.</span></span> <span data-ttu-id="c516f-159">`$(Configuration)` 的默认值为 Debug。</span><span class="sxs-lookup"><span data-stu-id="c516f-159">The default for `$(Configuration)` is Debug.</span></span> <span data-ttu-id="c516f-160">在上述示例中，`<TargetFramework>` 是 `netcoreapp2.0`。</span><span class="sxs-lookup"><span data-stu-id="c516f-160">In the sample above, the `<TargetFramework>` is `netcoreapp2.0`.</span></span>

<span data-ttu-id="c516f-161">`dotnet publish -h` 显示用于发布的帮助信息。</span><span class="sxs-lookup"><span data-stu-id="c516f-161">`dotnet publish -h` displays help information for publish.</span></span>

<span data-ttu-id="c516f-162">以下命令指定 `Release` 生成和发布目录：</span><span class="sxs-lookup"><span data-stu-id="c516f-162">The following command specifies a `Release` build and the publishing directory:</span></span>

```console
dotnet publish -c Release -o C:/MyWebs/test
```

<span data-ttu-id="c516f-163">[Dotnet 发布](/dotnet/core/tools/dotnet-publish)命令调用时，将调用的 MSBuild`Publish`目标。</span><span class="sxs-lookup"><span data-stu-id="c516f-163">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command calls MSBuild which invokes the `Publish` target.</span></span> <span data-ttu-id="c516f-164">任何参数传递给`dotnet publish`传递到 MSBuild。</span><span class="sxs-lookup"><span data-stu-id="c516f-164">Any parameters passed to `dotnet publish` are passed to MSBuild.</span></span> <span data-ttu-id="c516f-165">`-c` 参数映射到 `Configuration` MSBuild 属性。</span><span class="sxs-lookup"><span data-stu-id="c516f-165">The `-c` parameter maps to the `Configuration` MSBuild property.</span></span> <span data-ttu-id="c516f-166">`-o` 参数映射到 `OutputPath`。</span><span class="sxs-lookup"><span data-stu-id="c516f-166">The `-o` parameter maps to `OutputPath`.</span></span>

<span data-ttu-id="c516f-167">可以使用以下格式之一传递 MSBuild 属性：</span><span class="sxs-lookup"><span data-stu-id="c516f-167">MSBuild properties can be passed using either of the following formats:</span></span>

* ` p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

<span data-ttu-id="c516f-168">以下命令将 `Release` 版本发布到网络共享：</span><span class="sxs-lookup"><span data-stu-id="c516f-168">The following command publishes a `Release` build to a network share:</span></span>

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

<span data-ttu-id="c516f-169">网络共享通过正斜杠指定 (//r8/) 并适用于所有支持 .NET Core 的平台。</span><span class="sxs-lookup"><span data-stu-id="c516f-169">The network share is specified with forward slashes (*//r8/*) and works on all .NET Core supported platforms.</span></span>

<span data-ttu-id="c516f-170">确认用于部署的发布应用未在运行。</span><span class="sxs-lookup"><span data-stu-id="c516f-170">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="c516f-171">如果应用正在运行，publish 文件夹中的文件会被锁定。</span><span class="sxs-lookup"><span data-stu-id="c516f-171">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="c516f-172">部署不会发生，因为无法复制锁定的文件。</span><span class="sxs-lookup"><span data-stu-id="c516f-172">Deployment can't occur because locked files can't be copied.</span></span>

## <a name="publish-profiles"></a><span data-ttu-id="c516f-173">发布配置文件</span><span class="sxs-lookup"><span data-stu-id="c516f-173">Publish profiles</span></span>

<span data-ttu-id="c516f-174">本部分使用 Visual Studio 2017 和更高版本创建发布配置文件。</span><span class="sxs-lookup"><span data-stu-id="c516f-174">This section uses Visual Studio 2017 and higher to create publishing profiles.</span></span> <span data-ttu-id="c516f-175">创建后，从 Visual Studio 或命令行发布是可用。</span><span class="sxs-lookup"><span data-stu-id="c516f-175">Once created, publishing from Visual Studio or the command line is available.</span></span>

<span data-ttu-id="c516f-176">发布配置文件可以简化发布过程。</span><span class="sxs-lookup"><span data-stu-id="c516f-176">Publish profiles can simplify the publishing process.</span></span> <span data-ttu-id="c516f-177">多个发布配置文件可以存在。</span><span class="sxs-lookup"><span data-stu-id="c516f-177">Multiple publish profiles can exist.</span></span> <span data-ttu-id="c516f-178">若要在 Visual Studio 中创建的发布配置文件，右键单击解决方案资源管理器中的项目，然后选择**发布**。</span><span class="sxs-lookup"><span data-stu-id="c516f-178">To create a publish profile in Visual Studio, right-click on the project in Solution Explorer and select **Publish**.</span></span> <span data-ttu-id="c516f-179">或者，选择**发布\<项目名称 >**从生成菜单。</span><span class="sxs-lookup"><span data-stu-id="c516f-179">Alternatively, select **Publish \<project name>** from the build menu.</span></span> <span data-ttu-id="c516f-180">随即显示应用程序容量页的“发布”选项卡。</span><span class="sxs-lookup"><span data-stu-id="c516f-180">The **Publish** tab of the application capacities page is displayed.</span></span> <span data-ttu-id="c516f-181">如果项目不包含发布配置文件，将显示以下页面：</span><span class="sxs-lookup"><span data-stu-id="c516f-181">If the project doesn't contain a publish profile, the following page is displayed:</span></span>

![显示 Azure、 IIS、 FTB、 文件夹与 Azure 应用程序容量页的发布选项卡选择。](visual-studio-publish-profiles/_static/az.png)

<span data-ttu-id="c516f-184">选中“文件夹”后，“发布”按钮会创建一个文件夹发布配置文件并进行发布。</span><span class="sxs-lookup"><span data-stu-id="c516f-184">When **Folder** is selected, the **Publish** button creates a folder publish profile and publishes.</span></span>

![显示 Azure、IIS、FTB、文件夹的应用程序容量页的“发布”选项卡](visual-studio-publish-profiles/_static/pub1.png)

<span data-ttu-id="c516f-186">发布配置文件创建后，**发布**选项卡更改，并选择**创建新的配置文件**以创建新的配置文件。</span><span class="sxs-lookup"><span data-stu-id="c516f-186">Once a publish profile is created, the **Publish** tab changes, and select **Create new profile** to create a new profile.</span></span>

![显示上一步骤中创建的 FolderProfile 和“创建新配置文件”链接的应用程序容量页的“发布”选项卡](visual-studio-publish-profiles/_static/create_new.png)

<span data-ttu-id="c516f-188">发布向导支持以下发布目标：</span><span class="sxs-lookup"><span data-stu-id="c516f-188">The Publish wizard supports the following publish targets:</span></span>

* <span data-ttu-id="c516f-189">Microsoft Azure App Service</span><span class="sxs-lookup"><span data-stu-id="c516f-189">Microsoft Azure App Service</span></span>
* <span data-ttu-id="c516f-190">IIS、FTP 等（适用于任何 Web 服务器）</span><span class="sxs-lookup"><span data-stu-id="c516f-190">IIS, FTP, etc (for any web server)</span></span>
* <span data-ttu-id="c516f-191">文件夹</span><span class="sxs-lookup"><span data-stu-id="c516f-191">Folder</span></span>
* <span data-ttu-id="c516f-192">导入配置文件 （允许配置文件导入）。</span><span class="sxs-lookup"><span data-stu-id="c516f-192">Import profile (allows profile import).</span></span>
* <span data-ttu-id="c516f-193">Microsoft Azure 虚拟机</span><span class="sxs-lookup"><span data-stu-id="c516f-193">Microsoft Azure Virtual Machines</span></span>

<span data-ttu-id="c516f-194">有关详细信息，请参阅[哪些发布选项适合我？](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options)。</span><span class="sxs-lookup"><span data-stu-id="c516f-194">See [What publishing options are right for me?](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options) for more information.</span></span>

<span data-ttu-id="c516f-195">使用 Visual Studio 中，创建的发布配置文件时*属性/PublishProfiles/\<发布名称 >.pubxml*创建 MSBuild 文件。</span><span class="sxs-lookup"><span data-stu-id="c516f-195">When creating a publish profile with Visual Studio, a *Properties/PublishProfiles/\<publish name>.pubxml* MSBuild file is created.</span></span> <span data-ttu-id="c516f-196">此 .pubxml 文件为 MSBuild 文件，包含发布配置设置。</span><span class="sxs-lookup"><span data-stu-id="c516f-196">This *.pubxml* file is a MSBuild file and contains publish configuration settings.</span></span> <span data-ttu-id="c516f-197">可以更改此文件为自定义生成和发布过程。</span><span class="sxs-lookup"><span data-stu-id="c516f-197">This file can be changed to customize the build and publish process.</span></span> <span data-ttu-id="c516f-198">通过发布过程读取此文件。</span><span class="sxs-lookup"><span data-stu-id="c516f-198">This file is read by the publishing process.</span></span> <span data-ttu-id="c516f-199">`<LastUsedBuildConfiguration>` 是特殊的因为它的全局属性，且不应是在生成中导入任何文件中。</span><span class="sxs-lookup"><span data-stu-id="c516f-199">`<LastUsedBuildConfiguration>` is special because it's a global property and shouldn't be in any file that's imported in the build.</span></span> <span data-ttu-id="c516f-200">有关详细信息，请参阅 [MSBuild：如何设置配置属性](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx)。</span><span class="sxs-lookup"><span data-stu-id="c516f-200">See [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) for more info.</span></span>
<span data-ttu-id="c516f-201">*.Pubxml*文件不应签入源控件，因为它依赖于*.user*文件。</span><span class="sxs-lookup"><span data-stu-id="c516f-201">The *.pubxml* file shouldn't be checked into source control because it depends on the *.user* file.</span></span> <span data-ttu-id="c516f-202">切不可将 .user 文件签入源控件，因为它可能包含敏感信息，且仅对一个用户和一台计算机有效。</span><span class="sxs-lookup"><span data-stu-id="c516f-202">The *.user* file should never be checked into source control because it can contain sensitive information and it's only valid for one user and machine.</span></span>

<span data-ttu-id="c516f-203">敏感信息（如发布密码）在每个用户/计算机级别上加密，并存储在 Properties/PublishProfiles/\<publish name>.pubxml.user 文件中。</span><span class="sxs-lookup"><span data-stu-id="c516f-203">Sensitive information (like the publish password) is encrypted on a per user/machine level and stored in the *Properties/PublishProfiles/\<publish name>.pubxml.user* file.</span></span> <span data-ttu-id="c516f-204">由于此文件可能包含敏感信息，因此，不应将其签入源控件。</span><span class="sxs-lookup"><span data-stu-id="c516f-204">Because this file can contain sensitive information, it should **not** be checked into source control.</span></span>

<span data-ttu-id="c516f-205">有关如何发布 web 应用程序在 ASP.NET Core 上的概述，请参阅[主机并将其部署](index.md)。</span><span class="sxs-lookup"><span data-stu-id="c516f-205">For an overview of how to publish a web app on ASP.NET Core see [Host and deploy](index.md).</span></span> <span data-ttu-id="c516f-206">[承载并将其部署](index.md)是在 https://github.com/aspnet/websdk 开放源项目。</span><span class="sxs-lookup"><span data-stu-id="c516f-206">[Host and deploy](index.md) is an open source project at https://github.com/aspnet/websdk.</span></span>

<span data-ttu-id="c516f-207">`dotnet publish` 可以使用文件夹，MSDeploy，和[KUDU](https://github.com/projectkudu/kudu/wiki)发布配置文件：</span><span class="sxs-lookup"><span data-stu-id="c516f-207">`dotnet publish` can use folder, MSDeploy, and [KUDU](https://github.com/projectkudu/kudu/wiki) publish profiles:</span></span>
 
<span data-ttu-id="c516f-208">（可跨平台运行） 的文件夹： `dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>`</span><span class="sxs-lookup"><span data-stu-id="c516f-208">Folder (works cross-platform): `dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>`</span></span>

<span data-ttu-id="c516f-209">MSDeploy （当前此仅适用于 windows 由于 MSDeploy 不跨平台中）： `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>`</span><span class="sxs-lookup"><span data-stu-id="c516f-209">MSDeploy (currently this only works in windows since MSDeploy isn't cross-platform): `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>`</span></span>

<span data-ttu-id="c516f-210">MSDeploy 包 （当前此仅适用于 windows 由于 MSDeploy 不跨平台中）： `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>`</span><span class="sxs-lookup"><span data-stu-id="c516f-210">MSDeploy package(currently this only works in windows since MSDeploy isn't cross-platform): `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>`</span></span>

<span data-ttu-id="c516f-211">在前一示例中，**不**传递`deployonbuild`到`dotnet publish`。</span><span class="sxs-lookup"><span data-stu-id="c516f-211">In the preceeding samples, **don't** pass `deployonbuild` to `dotnet publish`.</span></span>

<span data-ttu-id="c516f-212">有关详细信息，请参阅[Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish)。</span><span class="sxs-lookup"><span data-stu-id="c516f-212">For more information, see [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span></span>

<span data-ttu-id="c516f-213">`dotnet publish` 支持 KUDU API 从任何平台发布到 Azure。</span><span class="sxs-lookup"><span data-stu-id="c516f-213">`dotnet publish` supports KUDU apis to publish to Azure from any platform.</span></span> <span data-ttu-id="c516f-214">Visual Studio 发布执行支持 KUDU Api，但它支持通过 websdk 交叉 plat 发布到 Azure。</span><span class="sxs-lookup"><span data-stu-id="c516f-214">Visual Studio publish does support the KUDU APIs but it's supported by websdk for cross plat publish to Azure.</span></span>

<span data-ttu-id="c516f-215">向“属性/PublishProfiles”文件夹添加包含以下内容的发布配置文件：</span><span class="sxs-lookup"><span data-stu-id="c516f-215">Add a publish profile to *Properties/PublishProfiles* folder with the following content:</span></span>

```xml
<Project>
  <PropertyGroup>
    <PublishProtocol>Kudu</PublishProtocol>
    <PublishSiteName>nodewebapp</PublishSiteName>
    <UserName>username</UserName>
    <Password>password</Password>
  </PropertyGroup>
</Project>
```

<span data-ttu-id="c516f-216">运行以下命令快速发布内容，并将其发布到 Azure 中使用 KUDU Api:</span><span class="sxs-lookup"><span data-stu-id="c516f-216">Running the following command zips up the publish contents and publish it to Azure using the KUDU APIs:</span></span>

`dotnet publish /p:PublishProfile=Azure /p:Configuration=Release`

<span data-ttu-id="c516f-217">使用发布配置文件时，请设置以下 MSBuild 属性：</span><span class="sxs-lookup"><span data-stu-id="c516f-217">Set the following MSBuild properties when using a publish profile:</span></span>

* `DeployOnBuild=true`
* `PublishProfile=<Publish profile name>`

<span data-ttu-id="c516f-218">发布与名为配置文件时*FolderProfile*，可以执行以下命令之一：</span><span class="sxs-lookup"><span data-stu-id="c516f-218">When publishing with a profile named *FolderProfile*, either of the commands below can be executed:</span></span>

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="c516f-219">在调用时[dotnet 生成](/dotnet/core/tools/dotnet-build)，它调用`msbuild`来运行生成和发布过程。</span><span class="sxs-lookup"><span data-stu-id="c516f-219">When invoking [dotnet build](/dotnet/core/tools/dotnet-build), it calls `msbuild` to run the build and publish process.</span></span> <span data-ttu-id="c516f-220">调用`dotnet build`或`msbuild`文件夹配置文件中传递时实质上等同。</span><span class="sxs-lookup"><span data-stu-id="c516f-220">Calling `dotnet build` or `msbuild` is essentially equivalent when passing in a folder profile.</span></span> <span data-ttu-id="c516f-221">调用时 MSBuild 直接在 Windows 上，使用 MSBuild 的.NET Framework 版本。</span><span class="sxs-lookup"><span data-stu-id="c516f-221">When calling MSBuild directly on Windows, the .NET Framework version of MSBuild is used.</span></span> <span data-ttu-id="c516f-222">MSDeploy 目前仅限于在 Windows 计算机上进行发布。</span><span class="sxs-lookup"><span data-stu-id="c516f-222">MSDeploy is currently limited to Windows machines for publishing.</span></span> <span data-ttu-id="c516f-223">在非文件夹配置文件上调用 `dotnet build` 时，会调用 MSBuild，并且 MSBuild 在非文件夹配置文件上使用 MSDeploy。</span><span class="sxs-lookup"><span data-stu-id="c516f-223">Calling `dotnet build` on a non-folder profile invokes MSBuild, and MSBuild uses MSDeploy on non-folder profiles.</span></span> <span data-ttu-id="c516f-224">在非文件夹配置文件上调用 `dotnet build` 时，会调用 MSBuild（使用 MSDeploy）并导致失败（即使在 Windows 平台上运行也是如此）。</span><span class="sxs-lookup"><span data-stu-id="c516f-224">Calling `dotnet build` on a non-folder profile invokes MSBuild (using MSDeploy) and results in a failure (even when running on a Windows platform).</span></span> <span data-ttu-id="c516f-225">若要使用非文件夹配置文件进行发布，请直接调用 MSBuild。</span><span class="sxs-lookup"><span data-stu-id="c516f-225">To publish with a non-folder profile, call MSBuild directly.</span></span>

<span data-ttu-id="c516f-226">以下文件夹发布配置文件通过 Visual Studio 创建，并被发布到网络共享：</span><span class="sxs-lookup"><span data-stu-id="c516f-226">The following folder publish profile was created with Visual Studio and publishes to a network share:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--
This file is used by the publish/package process of your Web project.
You can customize the behavior of this process by editing this 
MSBuild file.
-->
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <WebPublishMethod>FileSystem</WebPublishMethod>
    <PublishProvider>FileSystem</PublishProvider>
    <LastUsedBuildConfiguration>Release</LastUsedBuildConfiguration>
    <LastUsedPlatform>Any CPU</LastUsedPlatform>
    <SiteUrlToLaunchAfterPublish />
    <LaunchSiteAfterPublish>True</LaunchSiteAfterPublish>
    <ExcludeApp_Data>False</ExcludeApp_Data>
    <PublishFramework>netcoreapp1.1</PublishFramework>
    <ProjectGuid>c30c453c-312e-40c4-aec9-394a145dee0b</ProjectGuid>
    <publishUrl>\\r8\Release\AdminWeb</publishUrl>
    <DeleteExistingFiles>False</DeleteExistingFiles>
  </PropertyGroup>
</Project>
```

<span data-ttu-id="c516f-227">请注意，`<LastUsedBuildConfiguration>` 已设置为 `Release`。</span><span class="sxs-lookup"><span data-stu-id="c516f-227">Note `<LastUsedBuildConfiguration>` is set to `Release`.</span></span> <span data-ttu-id="c516f-228">从 Visual Studio 发布时，在启动发布过程后将使用该值设置 `<LastUsedBuildConfiguration>` 配置属性值。</span><span class="sxs-lookup"><span data-stu-id="c516f-228">When publishing from Visual Studio, the `<LastUsedBuildConfiguration>` configuration property value is set using the value when the publish process is started.</span></span> <span data-ttu-id="c516f-229">`<LastUsedBuildConfiguration>`配置属性是特殊，并且不应在导入的 MSBuild 文件中重写。</span><span class="sxs-lookup"><span data-stu-id="c516f-229">The `<LastUsedBuildConfiguration>` configuration property is special and shouldn't be overridden in an imported MSBuild file.</span></span> <span data-ttu-id="c516f-230">从命令行，可以重写此属性。</span><span class="sxs-lookup"><span data-stu-id="c516f-230">This property can be overridden from the command line.</span></span> <span data-ttu-id="c516f-231">例如:</span><span class="sxs-lookup"><span data-stu-id="c516f-231">For example:</span></span>

`dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="c516f-232">使用 MSBuild：</span><span class="sxs-lookup"><span data-stu-id="c516f-232">Using MSBuild:</span></span>

`msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a><span data-ttu-id="c516f-233">从命令行发布到 MSDeploy 终结点</span><span class="sxs-lookup"><span data-stu-id="c516f-233">Publish to an MSDeploy endpoint from the command line</span></span>

<span data-ttu-id="c516f-234">如前所述，可以使用完成发布`dotnet publish`或`msbuild`命令。</span><span class="sxs-lookup"><span data-stu-id="c516f-234">As previously mentioned, publishing can be accomplished using `dotnet publish` or the `msbuild` command.</span></span> <span data-ttu-id="c516f-235">`dotnet publish` 在 .NET Core 的上下文中运行。</span><span class="sxs-lookup"><span data-stu-id="c516f-235">`dotnet publish` runs in the context of .NET Core.</span></span> <span data-ttu-id="c516f-236">`msbuild`命令需要.NET framework，并因此仅限于 Windows 环境。</span><span class="sxs-lookup"><span data-stu-id="c516f-236">The `msbuild` command requires .NET framework, and is therefore limited to Windows environments.</span></span>

<span data-ttu-id="c516f-237">使用 MSDeploy 发布的最简单的方法是，首先在 Visual Studio 2017 中创建发布配置文件，然后从命令行中使用配置文件。</span><span class="sxs-lookup"><span data-stu-id="c516f-237">The easiest way to publish with MSDeploy is to first create a publish profile in Visual Studio 2017 and use the profile from the command line.</span></span>

<span data-ttu-id="c516f-238">在下面的示例中，创建一个 ASP.NET 核心 web 应用 (使用`dotnet new mvc`)，并使用 Visual Studio 添加 Azure 发布配置文件。</span><span class="sxs-lookup"><span data-stu-id="c516f-238">In the following sample, an ASP.NET Core web app is created (using `dotnet new mvc`), and an Azure publish profile is added with Visual Studio.</span></span>

<span data-ttu-id="c516f-239">运行`msbuild`从**VS 2017 的开发人员命令提示符**。</span><span class="sxs-lookup"><span data-stu-id="c516f-239">Run `msbuild` from a **Developer Command Prompt for VS 2017**.</span></span> <span data-ttu-id="c516f-240">开发人员命令提示已正确*msbuild.exe*在与某些 MSBuild 变量集其路径中。</span><span class="sxs-lookup"><span data-stu-id="c516f-240">The Developer Command Prompt has the correct *msbuild.exe* in its path with some MSBuild variables set.</span></span>

<span data-ttu-id="c516f-241">MSBuild 使用以下语法：</span><span class="sxs-lookup"><span data-stu-id="c516f-241">MSBuild uses the following syntax:</span></span>

`msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile> /p:Username=<USERNAME> /p:Password=<PASSWORD>`

<span data-ttu-id="c516f-242">获取`Password`从*\<发布名称 >。PublishSettings*文件。</span><span class="sxs-lookup"><span data-stu-id="c516f-242">Get the `Password` from the *\<Publish name>.PublishSettings* file.</span></span> <span data-ttu-id="c516f-243">下载*。PublishSettings*从文件：</span><span class="sxs-lookup"><span data-stu-id="c516f-243">Download the *.PublishSettings* file from either:</span></span>

* <span data-ttu-id="c516f-244">解决方案资源管理器： 在 Web 应用上右键单击并选择**下载发布配置文件**。</span><span class="sxs-lookup"><span data-stu-id="c516f-244">Solution Explorer: Right-click on the Web App and select **Download Publish Profile**.</span></span>
* <span data-ttu-id="c516f-245">Azure 管理门户中： 选择**Get 发布配置文件**从 Web 应用边栏选项卡。</span><span class="sxs-lookup"><span data-stu-id="c516f-245">The Azure Management Portal: Select **Get publish profile** from the Web App blade.</span></span>

<span data-ttu-id="c516f-246">可在发布配置文件中找到 `Username`。</span><span class="sxs-lookup"><span data-stu-id="c516f-246">`Username` can be found in the publish profile.</span></span>

<span data-ttu-id="c516f-247">以下示例使用“Web11112 - Web 部署”发布配置文件：</span><span class="sxs-lookup"><span data-stu-id="c516f-247">The following sample uses the "Web11112 - Web Deploy" publish profile:</span></span>

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="excluding-files"></a><span data-ttu-id="c516f-248">排除文件</span><span class="sxs-lookup"><span data-stu-id="c516f-248">Excluding files</span></span>

<span data-ttu-id="c516f-249">在发布 ASP.NET Core Web 应用时，生成项目和 wwwroot 文件夹的内容包括在内。</span><span class="sxs-lookup"><span data-stu-id="c516f-249">When publishing ASP.NET Core web apps, the build artifacts and contents of the *wwwroot* folder are included.</span></span> <span data-ttu-id="c516f-250">`msbuild` 支持[通配模式](https://gruntjs.com/configuring-tasks#globbing-patterns)。</span><span class="sxs-lookup"><span data-stu-id="c516f-250">`msbuild` supports [globbing patterns](https://gruntjs.com/configuring-tasks#globbing-patterns).</span></span> <span data-ttu-id="c516f-251">例如，以下`<Content>`元素标记排除的所有文本 (*.txt*) 从文件*wwwroot/content*文件夹和所有子文件夹。</span><span class="sxs-lookup"><span data-stu-id="c516f-251">For example, the following `<Content>` element markup excludes all text (*.txt*) files from the *wwwroot/content* folder and all its subfolders.</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="c516f-252">可以将上面的标记添加到发布配置文件或 .csproj 文件。</span><span class="sxs-lookup"><span data-stu-id="c516f-252">The markup above can be added to a publish profile or the *.csproj* file.</span></span> <span data-ttu-id="c516f-253">添加到 .csproj 文件时，会将该规则添加到项目中的所有发布配置文件中。</span><span class="sxs-lookup"><span data-stu-id="c516f-253">When added to the *.csproj* file, the rule is added to all publish profiles in the project.</span></span>

<span data-ttu-id="c516f-254">以下 `<MsDeploySkipRules>` 元素标记不包括 wwwroot/content 文件夹中的所有文件：</span><span class="sxs-lookup"><span data-stu-id="c516f-254">The following `<MsDeploySkipRules>` element markup exludes all files from the *wwwroot/content* folder:</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="c516f-255">`<MsDeploySkipRules>` 不会删除*跳过*部署站点中的目标。</span><span class="sxs-lookup"><span data-stu-id="c516f-255">`<MsDeploySkipRules>` won't delete the *skip* targets from the deployment site.</span></span> <span data-ttu-id="c516f-256">`<Content>` 从部署站点删除目标的文件和文件夹。</span><span class="sxs-lookup"><span data-stu-id="c516f-256">`<Content>` targeted files and folders are deleted from the deployment site.</span></span> <span data-ttu-id="c516f-257">例如，假设已部署的 web 应用程序有以下文件：</span><span class="sxs-lookup"><span data-stu-id="c516f-257">For example, suppose a deployed web app had the following files:</span></span>

* <span data-ttu-id="c516f-258">*Views/Home/About1.cshtml*</span><span class="sxs-lookup"><span data-stu-id="c516f-258">*Views/Home/About1.cshtml*</span></span>
* <span data-ttu-id="c516f-259">*Views/Home/About2.cshtml*</span><span class="sxs-lookup"><span data-stu-id="c516f-259">*Views/Home/About2.cshtml*</span></span>
* <span data-ttu-id="c516f-260">*Views/Home/About3.cshtml*</span><span class="sxs-lookup"><span data-stu-id="c516f-260">*Views/Home/About3.cshtml*</span></span>

<span data-ttu-id="c516f-261">如果以下`<MsDeploySkipRules>`添加标记，则不会在部署站点上删除这些文件。</span><span class="sxs-lookup"><span data-stu-id="c516f-261">If the following `<MsDeploySkipRules>` markup is added, those files wouldn't be deleted on the deployment site.</span></span>

``` xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About1.cshtml</AbsolutePath>
  </MsDeploySkipRules>

  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About2.cshtml</AbsolutePath>
  </MsDeploySkipRules>

  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About3.cshtml</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="c516f-262">`<MsDeploySkipRules>`上面所示的标记阻止*跳过*遭到 depoyed 文件，但它们在部署后不会删除这些文件。</span><span class="sxs-lookup"><span data-stu-id="c516f-262">The `<MsDeploySkipRules>` markup shown above prevents the *skipped* files from being depoyed but won't delete those files once they're deployed.</span></span>

<span data-ttu-id="c516f-263">以下`<Content>`标记删除在部署站点的目标的文件：</span><span class="sxs-lookup"><span data-stu-id="c516f-263">The following `<Content>` markup deletes the targeted files at the deployment site:</span></span>

``` xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="c516f-264">使用命令行部署`<Content>`与以下类似的输出结果上方的标记：</span><span class="sxs-lookup"><span data-stu-id="c516f-264">Using command-line deployment with the `<Content>` markup above results in output similar to the following:</span></span>

``` console
MSDeployPublish:
  Starting Web deployment task from source: manifest(C:\Webs\Web1\obj\Release\netcoreapp1.1\PubTmp\Web1.SourceManifest.
  xml) to Destination: auto().
  Deleting file (Web11112\Views\Home\About1.cshtml).
  Deleting file (Web11112\Views\Home\About2.cshtml).
  Deleting file (Web11112\Views\Home\About3.cshtml).
  Updating file (Web11112\web.config).
  Updating file (Web11112\Web1.deps.json).
  Updating file (Web11112\Web1.dll).
  Updating file (Web11112\Web1.pdb).
  Updating file (Web11112\Web1.runtimeconfig.json).
  Successfully executed Web deployment task.
  Publish Succeeded.
Done Building Project "C:\Webs\Web1\Web1.csproj" (default targets).
```

## <a name="including-files"></a><span data-ttu-id="c516f-265">包含文件</span><span class="sxs-lookup"><span data-stu-id="c516f-265">Including files</span></span>

<span data-ttu-id="c516f-266">以下标记包括*映像*文件夹到在项目目录外的*wwwroot/images*发布站点文件夹：</span><span class="sxs-lookup"><span data-stu-id="c516f-266">The following markup includes an *images* folder outside the project directory to the *wwwroot/images* folder of the publish site:</span></span>

``` xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

<span data-ttu-id="c516f-267">可以将标记添加到 .csproj 文件或发布配置文件。</span><span class="sxs-lookup"><span data-stu-id="c516f-267">The markup can be added to the *.csproj* file or the publish profile.</span></span> <span data-ttu-id="c516f-268">如果将其添加到*.csproj*文件，它包含在项目中每个发布配置文件中。</span><span class="sxs-lookup"><span data-stu-id="c516f-268">If it's added to the *.csproj* file, it's included in each publish profile in the project.</span></span>

<span data-ttu-id="c516f-269">以下突出显示的标记显示如何：</span><span class="sxs-lookup"><span data-stu-id="c516f-269">The following highlighted markup shows how to:</span></span>

* <span data-ttu-id="c516f-270">将文件从项目外部复制到 wwwroot 文件夹。</span><span class="sxs-lookup"><span data-stu-id="c516f-270">Copy a file from outside the project into the *wwwroot* folder.</span></span>
* <span data-ttu-id="c516f-271">排除 wwwroot\Content 文件夹。</span><span class="sxs-lookup"><span data-stu-id="c516f-271">Exclude the *wwwroot\Content* folder.</span></span>
* <span data-ttu-id="c516f-272">排除 Views\Home\About2.cshtml。</span><span class="sxs-lookup"><span data-stu-id="c516f-272">Exclude *Views\Home\About2.cshtml*.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--
This file is used by the publish/package process of your Web project.
You can customize the behavior of this process by editing this 
MSBuild file.
-->
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <WebPublishMethod>FileSystem</WebPublishMethod>
    <PublishProvider>FileSystem</PublishProvider>
    <LastUsedBuildConfiguration>Release</LastUsedBuildConfiguration>
    <LastUsedPlatform>Any CPU</LastUsedPlatform>
    <SiteUrlToLaunchAfterPublish />
    <LaunchSiteAfterPublish>True</LaunchSiteAfterPublish>
    <ExcludeApp_Data>False</ExcludeApp_Data>
    <PublishFramework />
    <ProjectGuid>afa9f185-7ce0-4935-9da1-ab676229d68a</ProjectGuid>
    <publishUrl>bin\Release\PublishOutput</publishUrl>
    <DeleteExistingFiles>False</DeleteExistingFiles>
  </PropertyGroup>
  <ItemGroup>
    <ResolvedFileToPublish Include="..\ReadMe2.MD">
      <RelativePath>wwwroot\ReadMe2.MD</RelativePath>
    </ResolvedFileToPublish>

    <Content Update="wwwroot\Content\**\*" CopyToPublishDirectory="Never" />
    <Content Update="Views\Home\About2.cshtml" CopyToPublishDirectory="Never" />

  </ItemGroup>
</Project>
```

<span data-ttu-id="c516f-273">请参阅 [WebSDK 自述文件](https://github.com/aspnet/websdk)，了解更多部署示例。</span><span class="sxs-lookup"><span data-stu-id="c516f-273">See the [WebSDK Readme](https://github.com/aspnet/websdk) for more deployment samples.</span></span>

### <a name="run-a-target-before-or-after-publishing"></a><span data-ttu-id="c516f-274">在发布前或发布后运行目标</span><span class="sxs-lookup"><span data-stu-id="c516f-274">Run a target before or after publishing</span></span>

<span data-ttu-id="c516f-275">内置`BeforePublish`和`AfterPublish`目标可以用于执行目标之前或之后发布目标。</span><span class="sxs-lookup"><span data-stu-id="c516f-275">The built-in `BeforePublish` and `AfterPublish` targets can be used to execute a target before or after the publish target.</span></span> <span data-ttu-id="c516f-276">可以将以下标记添加到发布配置文件中，以便在发布前后将消息记录到控制台输出：</span><span class="sxs-lookup"><span data-stu-id="c516f-276">The following markup can be added to the publish profile to log messages to the console output before and after publishing:</span></span>

``` xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="the-kudu-service"></a><span data-ttu-id="c516f-277">Kudu 服务</span><span class="sxs-lookup"><span data-stu-id="c516f-277">The Kudu service</span></span>

<span data-ttu-id="c516f-278">若要查看中的文件的 Azure 应用程序服务 web 应用程序部署，使用[Kudu 服务](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service)。</span><span class="sxs-lookup"><span data-stu-id="c516f-278">To view the files in the an Azure Apps Service web app deployment, use the [Kudu service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span></span> <span data-ttu-id="c516f-279">追加`scm`令牌到 web 应用的名称。</span><span class="sxs-lookup"><span data-stu-id="c516f-279">Append the `scm` token to the name of the web app.</span></span> <span data-ttu-id="c516f-280">例如:</span><span class="sxs-lookup"><span data-stu-id="c516f-280">For example:</span></span>

| <span data-ttu-id="c516f-281">URL</span><span class="sxs-lookup"><span data-stu-id="c516f-281">URL</span></span>                                    | <span data-ttu-id="c516f-282">结果</span><span class="sxs-lookup"><span data-stu-id="c516f-282">Result</span></span>      |
| -------------------------------------- | ----------- |
| `http://mysite.azurewebsites.net/`     | <span data-ttu-id="c516f-283">Web 应用</span><span class="sxs-lookup"><span data-stu-id="c516f-283">Web App</span></span>     |
| `http://mysite.scm.azurewebsites.net/` | <span data-ttu-id="c516f-284">Kudu 服务</span><span class="sxs-lookup"><span data-stu-id="c516f-284">Kudu sevice</span></span> |

<span data-ttu-id="c516f-285">选择[调试控制台](https://github.com/projectkudu/kudu/wiki/Kudu-console)菜单项来查看/编辑/删除/添加文件。</span><span class="sxs-lookup"><span data-stu-id="c516f-285">Select the [Debug Console](https://github.com/projectkudu/kudu/wiki/Kudu-console) menu item to view/edit/delete/add files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c516f-286">其他资源</span><span class="sxs-lookup"><span data-stu-id="c516f-286">Additional resources</span></span>

* <span data-ttu-id="c516f-287">[Web 部署](https://www.iis.net/downloads/microsoft/web-deploy)(MSDeploy) 简化了部署 web 应用和到 IIS 服务器的网站。</span><span class="sxs-lookup"><span data-stu-id="c516f-287">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) simplifies deployment of web apps and websites to IIS servers.</span></span>
* <span data-ttu-id="c516f-288">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues)：文件问题和部署的请求功能。</span><span class="sxs-lookup"><span data-stu-id="c516f-288">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): File issues and request features for deployment.</span></span>
