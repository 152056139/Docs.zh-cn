---
title: 使用 Visual Studio Code 将模型添加到 ASP.NET Core Razor 页面应用
author: rick-anderson
description: 了解如何使用 Visual Studio Code 将模型添加到 ASP.NET Core 中的 Razor 页面应用。
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
uid: tutorials/razor-pages-vsc/model
ms.openlocfilehash: 3552b541c43375aef43838800855ec63e7fed372
ms.sourcegitcommit: ea7ec8d47f94cfb8e008d771f647f86bbb4baa44
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/06/2018
ms.locfileid: "37894038"
---
# <a name="add-a-model-to-an-aspnet-core-razor-pages-app-with-visual-studio-code"></a><span data-ttu-id="8b67c-103">使用 Visual Studio Code 将模型添加到 ASP.NET Core Razor 页面应用</span><span class="sxs-lookup"><span data-stu-id="8b67c-103">Add a model to an ASP.NET Core Razor Pages app with Visual Studio Code</span></span>

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="8b67c-104">添加数据模型</span><span class="sxs-lookup"><span data-stu-id="8b67c-104">Add a data model</span></span>

* <span data-ttu-id="8b67c-105">添加名为“Models”的文件夹。</span><span class="sxs-lookup"><span data-stu-id="8b67c-105">Add a folder named *Models*.</span></span>
* <span data-ttu-id="8b67c-106">将类添加到名为“Movie.cs”的“Models”文件夹。</span><span class="sxs-lookup"><span data-stu-id="8b67c-106">Add a class to the *Models* folder named *Movie.cs*.</span></span>
* <span data-ttu-id="8b67c-107">将以下代码添加到“Models/Movie.cs”文件：</span><span class="sxs-lookup"><span data-stu-id="8b67c-107">Add the following code to the *Models/Movie.cs* file:</span></span>

[!INCLUDE [model 2](../../includes/RP/model2.md)]

[!INCLUDE [model 2a](../../includes/RP/model2a.md)]

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-4)]

<span data-ttu-id="8b67c-108">将以下 `using` 语句添加到 Startup.cs 顶部：</span><span class="sxs-lookup"><span data-stu-id="8b67c-108">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="8b67c-109">生成项目以验证有没有任何错误存在。</span><span class="sxs-lookup"><span data-stu-id="8b67c-109">Build the project to verify you don't have any errors.</span></span>

### <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="8b67c-110">用于进行迁移的 Entity Framework Core NuGet 包</span><span class="sxs-lookup"><span data-stu-id="8b67c-110">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="8b67c-111">[Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) 中提供了适用于命令行接口 (CLI) 的 EF 工具。</span><span class="sxs-lookup"><span data-stu-id="8b67c-111">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="8b67c-112">若要安装此包，请将它添加到 .csproj 文件中的 `DotNetCliToolReference` 集合。</span><span class="sxs-lookup"><span data-stu-id="8b67c-112">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file.</span></span> <span data-ttu-id="8b67c-113">注意：必须通过编辑 .csproj 文件来安装此包；不能使用 `install-package` 命令或程序包管理器 GUI。</span><span class="sxs-lookup"><span data-stu-id="8b67c-113">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span>

<span data-ttu-id="8b67c-114">编辑 RazorPagesMovie.csproj 文件：</span><span class="sxs-lookup"><span data-stu-id="8b67c-114">Edit the *RazorPagesMovie.csproj* file:</span></span>

* <span data-ttu-id="8b67c-115">选择“文件” > “打开文件”，然后选择 RazorPagesMovie.csproj 文件。</span><span class="sxs-lookup"><span data-stu-id="8b67c-115">Select **File** > **Open File**, and then select the *RazorPagesMovie.csproj* file.</span></span>
* <span data-ttu-id="8b67c-116">将 `Microsoft.EntityFrameworkCore.Tools.DotNet` 的 工具引用添加至第二个 \<ItemGroup>：</span><span class="sxs-lookup"><span data-stu-id="8b67c-116">Add tool reference for `Microsoft.EntityFrameworkCore.Tools.DotNet` to the second **\<ItemGroup>**:</span></span>

[!code-xml[](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj)]

[!INCLUDE [model 3](../../includes/RP/model3.md)]

<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="8b67c-117">搭建“电影”模型的基架</span><span class="sxs-lookup"><span data-stu-id="8b67c-117">Scaffold the Movie model</span></span>

* <span data-ttu-id="8b67c-118">打开项目目录（包含 Program.cs、Startup.cs 和 .csproj 文件的目录）中的命令窗口。</span><span class="sxs-lookup"><span data-stu-id="8b67c-118">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="8b67c-119">运行下面的命令：</span><span class="sxs-lookup"><span data-stu-id="8b67c-119">Run the following command:</span></span>

<span data-ttu-id="8b67c-120">**注意：请在 Windows 上运行以下命令。对于 MacOS 和 Linux，请参阅下一个命令**</span><span class="sxs-lookup"><span data-stu-id="8b67c-120">**Note: Run the following command on Windows. For MacOS and Linux, see the next command**</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="8b67c-121">在 MacOS 和 Linux 上，请运行以下命令：</span><span class="sxs-lookup"><span data-stu-id="8b67c-121">On MacOS and Linux, run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

<span data-ttu-id="8b67c-122">如果收到错误：</span><span class="sxs-lookup"><span data-stu-id="8b67c-122">If you get the error:</span></span>
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

<span data-ttu-id="8b67c-123">退出 Visual Studio，然后重新运行命令。</span><span class="sxs-lookup"><span data-stu-id="8b67c-123">Exit Visual Studio and run the command again.</span></span>

[!INCLUDE [model 4](../../includes/RP/model4.md)]

<span data-ttu-id="8b67c-124">下一个教程介绍由基架创建的文件。</span><span class="sxs-lookup"><span data-stu-id="8b67c-124">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8b67c-125">[上一篇：入门](xref:tutorials/razor-pages-vsc/razor-pages-start)
> [下一篇：已搭建基架的 Razor 页面](xref:tutorials/razor-pages-vsc/page)</span><span class="sxs-lookup"><span data-stu-id="8b67c-125">[Previous: Get Started](xref:tutorials/razor-pages-vsc/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages-vsc/page)</span></span>
