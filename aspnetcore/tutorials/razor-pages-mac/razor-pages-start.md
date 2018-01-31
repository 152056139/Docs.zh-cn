---
title: "在 ASP.NET Core on Mac 中开始使用 Razor 页面"
author: rick-anderson
description: "利用 Visual Studio for Mac 在 ASP.NET Core 中开始使用 Razor 页面"
manager: wpickett
ms.author: riande
ms.date: 07/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages-mac/razor-pages-start
ms.openlocfilehash: 9e7d1db47e4cc9d753b1629e20345ca1f4403b2f
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="getting-started-with-razor-pages-in-aspnet-core-with-visual-studio-for-mac"></a><span data-ttu-id="3ce1d-103">借助 Visual Studio for Mac 在 ASP.NET Core 中开始使用 Razor 页面</span><span class="sxs-lookup"><span data-stu-id="3ce1d-103">Getting started with Razor Pages in ASP.NET Core with Visual Studio for Mac</span></span>

<span data-ttu-id="3ce1d-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3ce1d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="3ce1d-105">本教程介绍构建 ASP.NET Core Razor Pages Web 应用的基础知识。</span><span class="sxs-lookup"><span data-stu-id="3ce1d-105">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="3ce1d-106">建议在学习本教程前先查看 [Razor 页面介绍](xref:mvc/razor-pages/index)。</span><span class="sxs-lookup"><span data-stu-id="3ce1d-106">We recommend you review [Introduction to Razor Pages](xref:mvc/razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="3ce1d-107">Razor 页面是在 ASP.NET Core 中为 Web 应用程序生成 UI 时建议使用的方法。</span><span class="sxs-lookup"><span data-stu-id="3ce1d-107">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3ce1d-108">系统必备</span><span class="sxs-lookup"><span data-stu-id="3ce1d-108">Prerequisites</span></span>

<span data-ttu-id="3ce1d-109">安装以下组件：</span><span class="sxs-lookup"><span data-stu-id="3ce1d-109">Install the following:</span></span>

* <span data-ttu-id="3ce1d-110">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) 或更高版本</span><span class="sxs-lookup"><span data-stu-id="3ce1d-110">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later</span></span>
* [<span data-ttu-id="3ce1d-111">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="3ce1d-111">Visual Studio for Mac</span></span>](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-a-razor-web-app"></a><span data-ttu-id="3ce1d-112">创建 Razor Web 应用</span><span class="sxs-lookup"><span data-stu-id="3ce1d-112">Create a Razor web app</span></span>

<span data-ttu-id="3ce1d-113">从终端运行以下命令：</span><span class="sxs-lookup"><span data-stu-id="3ce1d-113">From a terminal, run the following commands:</span></span>

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

<span data-ttu-id="3ce1d-114">上述命令使用 [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) 创建并运行 Razor 页面项目。</span><span class="sxs-lookup"><span data-stu-id="3ce1d-114">The preceding commands use the [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="3ce1d-115">打开浏览器，转到 http://localhost:5000 查看应用程序。</span><span class="sxs-lookup"><span data-stu-id="3ce1d-115">Open a browser to http://localhost:5000 to view the application.</span></span>

![主页或索引页](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="3ce1d-117">打开项目</span><span class="sxs-lookup"><span data-stu-id="3ce1d-117">Open the project</span></span>

<span data-ttu-id="3ce1d-118">按 Ctrl+C 关闭应用程序。</span><span class="sxs-lookup"><span data-stu-id="3ce1d-118">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="3ce1d-119">在 Visual Studio 中，选择“文件”>“打开”，然后选择“RazorPagesMovie.csproj”文件。</span><span class="sxs-lookup"><span data-stu-id="3ce1d-119">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="3ce1d-120">启动应用</span><span class="sxs-lookup"><span data-stu-id="3ce1d-120">Launch the app</span></span>

<span data-ttu-id="3ce1d-121">在 Visual Studio 中，选择“运行”>“启动但不调试”以启动应用。</span><span class="sxs-lookup"><span data-stu-id="3ce1d-121">In Visual Studio, select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="3ce1d-122">Visual Studio 启动 [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview)、启动浏览器并导航到 `http://localhost:5000`。</span><span class="sxs-lookup"><span data-stu-id="3ce1d-122">Visual Studio starts [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview), launches a browser, and navigates to `http://localhost:5000`.</span></span>

<span data-ttu-id="3ce1d-123">在下一个教程中，我们将向项目添加模型。</span><span class="sxs-lookup"><span data-stu-id="3ce1d-123">In the next tutorial, we add a model to the project.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="3ce1d-124">下一篇：添加模型</span><span class="sxs-lookup"><span data-stu-id="3ce1d-124">Next: Adding a model</span></span>](xref:tutorials/razor-pages-mac/model)
