---
title: "ASP.NET Core MVC 和 Visual Studio for Mac 入门"
author: rick-anderson
description: "ASP.NET Core MVC 和 Visual Studio 入门"
keywords: ASP.NET Core, MVC, Visual Studio for Mac, Entity Framework
ms.author: riande
manager: wpickett
ms.date: 8/23/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-mac/start-mvc
ms.openlocfilehash: 21f115eec924d5e4b21ad78398c8cbd99e02a0a8
ms.sourcegitcommit: 216dfac27542f10a79274a9ce60dc449e888ed20
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/29/2017
---
# <a name="getting-started-with-aspnet-core-mvc-and-visual-studio-for-mac"></a><span data-ttu-id="bb4e9-104">ASP.NET Core MVC 和 Visual Studio for Mac 入门</span><span class="sxs-lookup"><span data-stu-id="bb4e9-104">Getting started with ASP.NET Core MVC and Visual Studio for Mac</span></span>

<span data-ttu-id="bb4e9-105">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="bb4e9-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="bb4e9-106">本教程介绍使用 [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/) 生成 ASP.NET Core MVC Web 应用所涉及的基础知识。</span><span class="sxs-lookup"><span data-stu-id="bb4e9-106">This tutorial teaches you the basics of building an ASP.NET Core MVC web app using [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/).</span></span> 

[!INCLUDE[consider RP](../../includes/razor.md)]

<span data-ttu-id="bb4e9-107">本教程提供 3 个版本：</span><span class="sxs-lookup"><span data-stu-id="bb4e9-107">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="bb4e9-108">macOS：[使用 Visual Studio for Mac 生成 ASP.NET Core MVC 应用](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="bb4e9-108">macOS: [Build an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="bb4e9-109">Windows：[使用 Visual Studio 生成 ASP.NET Core MVC 应用](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="bb4e9-109">Windows: [Build an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="bb4e9-110">Linux、macOS和 Windows：[使用 Visual Studio Code 生成 ASP.NET Core MVC 应用](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="bb4e9-110">Linux, macOS, and Windows: [Build an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bb4e9-111">先决条件</span><span class="sxs-lookup"><span data-stu-id="bb4e9-111">Prerequisites</span></span>

<span data-ttu-id="bb4e9-112">本教程需要 [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="bb4e9-112">This tutorial requires the [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>

<span data-ttu-id="bb4e9-113">安装以下组件：</span><span class="sxs-lookup"><span data-stu-id="bb4e9-113">Install the following:</span></span>

- <span data-ttu-id="bb4e9-114">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) 或更高版本</span><span class="sxs-lookup"><span data-stu-id="bb4e9-114">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later</span></span>
- [<span data-ttu-id="bb4e9-115">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="bb4e9-115">Visual Studio for Mac</span></span>](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-a-web-app"></a><span data-ttu-id="bb4e9-116">创建 Web 应用</span><span class="sxs-lookup"><span data-stu-id="bb4e9-116">Create a web app</span></span>

<span data-ttu-id="bb4e9-117">在 Visual Studio 中，选择“文件”>“新建解决方案”。</span><span class="sxs-lookup"><span data-stu-id="bb4e9-117">From Visual Studio, select **File > New Solution**.</span></span>

![macOS 新建解决方案](../first-web-api-mac/_static/sln.png)

<span data-ttu-id="bb4e9-119">选择“.NET Core App”>“ASP.NET Core”>“Web 应用”>“下一步”。</span><span class="sxs-lookup"><span data-stu-id="bb4e9-119">Select **.NET Core App >  ASP.NET Core > Web App > Next**.</span></span>

![macOS“新建项目”对话框](start-mvc/1.png)

<span data-ttu-id="bb4e9-121">将项目命名为“MvcMovie”，然后选择“创建”。</span><span class="sxs-lookup"><span data-stu-id="bb4e9-121">Name the project **MvcMovie**, and then select **Create**.</span></span>

![macOS“新建项目”对话框](start-mvc/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="bb4e9-123">启动应用</span><span class="sxs-lookup"><span data-stu-id="bb4e9-123">Launch the app</span></span>

<span data-ttu-id="bb4e9-124">在 Visual Studio 中，选择“运行”>“开始执行(不调试)”以启动应用。</span><span class="sxs-lookup"><span data-stu-id="bb4e9-124">In Visual Studio, select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="bb4e9-125">Visual Studio 启动 [Kestrel](xref:fundamentals/servers/index#kestrel)，启动浏览器并导航到 `http://localhost:port`，其中的“端口”是随机选择的端口号。</span><span class="sxs-lookup"><span data-stu-id="bb4e9-125">Visual Studio starts [Kestrel](xref:fundamentals/servers/index#kestrel), launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

![具有新项目的浏览器](start-mvc/b1.png)

* <span data-ttu-id="bb4e9-127">地址栏显示 `localhost:port#`，而不是显示 `example.com`。</span><span class="sxs-lookup"><span data-stu-id="bb4e9-127">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="bb4e9-128">这是因为 `localhost` 是本地计算机的标准主机名。</span><span class="sxs-lookup"><span data-stu-id="bb4e9-128">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="bb4e9-129">Visual Studio 创建 Web 项目时，Web 服务器使用的是随机端口。</span><span class="sxs-lookup"><span data-stu-id="bb4e9-129">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="bb4e9-130">运行应用时，将看到不同的端口号。</span><span class="sxs-lookup"><span data-stu-id="bb4e9-130">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="bb4e9-131">可以从“运行”菜单中以调试或非调试模式启动应用。</span><span class="sxs-lookup"><span data-stu-id="bb4e9-131">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

<span data-ttu-id="bb4e9-132">默认模板提供了“主页”、“关于”和“联系人”的链接。</span><span class="sxs-lookup"><span data-stu-id="bb4e9-132">The default template gives you **Home, About** and **Contact** links.</span></span> <span data-ttu-id="bb4e9-133">上面的浏览器图像未显示这些链接。</span><span class="sxs-lookup"><span data-stu-id="bb4e9-133">The browser image above doesn't show these links.</span></span> <span data-ttu-id="bb4e9-134">根据浏览器的大小，可能需要单击导航图标才能显示这些链接。</span><span class="sxs-lookup"><span data-stu-id="bb4e9-134">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![具有新项目的浏览器](start-mvc/b2.png)

<span data-ttu-id="bb4e9-136">在本教程的下一部分中，你将了解 MVC 并开始撰写一些代码。</span><span class="sxs-lookup"><span data-stu-id="bb4e9-136">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="bb4e9-137">下一篇</span><span class="sxs-lookup"><span data-stu-id="bb4e9-137">Next</span></span>](adding-controller.md)  
