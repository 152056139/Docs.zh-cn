---
title: 要开始使用 SignalR 在 ASP.NET Core 上
author: rachelappel
description: 在本教程中，你创建使用 SignalR 为 ASP.NET Core 应用。
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 03/16/2018
ms.prod: aspnet-core
ms.topic: tutorial
ms.technology: aspnet
uid: signalr/get-started
ms.openlocfilehash: 8b27e58a66c9030d92f9b9a6b8b2a7ed292e4ca9
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/03/2018
---
# <a name="get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="8cfb9-103">要开始使用 SignalR 在 ASP.NET Core 上</span><span class="sxs-lookup"><span data-stu-id="8cfb9-103">Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="8cfb9-104">作者：[Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="8cfb9-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

[!INCLUDE [2.1 preview notice](~/includes/2.1.md)]

<span data-ttu-id="8cfb9-105">本教程教生成实时应用程序使用 ASP.NET Core SignalR 的基础知识。</span><span class="sxs-lookup"><span data-stu-id="8cfb9-105">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![解决方案](get-started/_static/signalr-get-started-finished.png)

<span data-ttu-id="8cfb9-107">本教程演示了下列 SignalR 开发任务：</span><span class="sxs-lookup"><span data-stu-id="8cfb9-107">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="8cfb9-108">在 ASP.NET 核心 web 应用上创建 SignalR。</span><span class="sxs-lookup"><span data-stu-id="8cfb9-108">Create a SignalR on ASP.NET Core web app.</span></span>
> * <span data-ttu-id="8cfb9-109">创建一个 SignalR 集线器，以将内容推送到客户端。</span><span class="sxs-lookup"><span data-stu-id="8cfb9-109">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="8cfb9-110">修改`Startup`类并将应用配置。</span><span class="sxs-lookup"><span data-stu-id="8cfb9-110">Modify the `Startup` class and configure the app.</span></span>

<span data-ttu-id="8cfb9-111">[查看或下载示例代码](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/)（[如何下载](xref:tutorials/index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="8cfb9-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="prerequisites"></a><span data-ttu-id="8cfb9-112">系统必备</span><span class="sxs-lookup"><span data-stu-id="8cfb9-112">Prerequisites</span></span>

<span data-ttu-id="8cfb9-113">安装以下软件：</span><span class="sxs-lookup"><span data-stu-id="8cfb9-113">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8cfb9-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8cfb9-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="8cfb9-115">[.NET 核心 2.1.0 预览 2 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview2)或更高版本</span><span class="sxs-lookup"><span data-stu-id="8cfb9-115">[.NET Core 2.1.0 Preview 2 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview2) or later</span></span>
* <span data-ttu-id="8cfb9-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) 15.7 或使用更高版本**ASP.NET 和 web 开发**工作负荷</span><span class="sxs-lookup"><span data-stu-id="8cfb9-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7 or later with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="8cfb9-117">npm</span><span class="sxs-lookup"><span data-stu-id="8cfb9-117">npm</span></span>](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8cfb9-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8cfb9-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="8cfb9-119">[.NET 核心 2.1.0 预览 2 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview2)或更高版本</span><span class="sxs-lookup"><span data-stu-id="8cfb9-119">[.NET Core 2.1.0 Preview 2 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview2) or later</span></span>
* [<span data-ttu-id="8cfb9-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8cfb9-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="8cfb9-121">用于 Visual Studio 代码的 C#</span><span class="sxs-lookup"><span data-stu-id="8cfb9-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="8cfb9-122">npm</span><span class="sxs-lookup"><span data-stu-id="8cfb9-122">npm</span></span>](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="8cfb9-123">创建 ASP.NET Core 项目承载 SignalR 客户端和服务器</span><span class="sxs-lookup"><span data-stu-id="8cfb9-123">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8cfb9-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8cfb9-124">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="8cfb9-125">使用**文件** > **新项目**菜单选项，然后选择**ASP.NET 核心 Web 应用程序**。</span><span class="sxs-lookup"><span data-stu-id="8cfb9-125">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="8cfb9-126">将项目*SignalRChat*。</span><span class="sxs-lookup"><span data-stu-id="8cfb9-126">Name the project *SignalRChat*.</span></span>

   ![Visual Studio 中的新建项目对话框](get-started/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="8cfb9-128">选择**Web 应用程序**创建使用 Razor 页项目。</span><span class="sxs-lookup"><span data-stu-id="8cfb9-128">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="8cfb9-129">然后选择**确定**。</span><span class="sxs-lookup"><span data-stu-id="8cfb9-129">Then select **OK**.</span></span> <span data-ttu-id="8cfb9-130">请确保**ASP.NET 核心 2.1**尽管 SignalR 运行在较旧版本的.NET framework 选择器，从选择。</span><span class="sxs-lookup"><span data-stu-id="8cfb9-130">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

   ![Visual Studio 中的新建项目对话框](get-started/_static/signalr-new-project-choose-type.png)

<span data-ttu-id="8cfb9-132">Visual Studio 包含`Microsoft.AspNetCore.SignalR`作为的一部分包含其服务器库包其**ASP.NET 核心 Web 应用程序**模板。</span><span class="sxs-lookup"><span data-stu-id="8cfb9-132">Visual Studio includes the `Microsoft.AspNetCore.SignalR` package containing its server libraries as part of its **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="8cfb9-133">但是，适用于 SignalR 的 JavaScript 客户端库必须安装使用*npm*。</span><span class="sxs-lookup"><span data-stu-id="8cfb9-133">However, the JavaScript client library for SignalR must be installed using *npm*.</span></span>

3. <span data-ttu-id="8cfb9-134">在中运行以下命令**程序包管理器控制台**窗口，请从项目根：</span><span class="sxs-lookup"><span data-stu-id="8cfb9-134">Run the following commands in the **Package Manager Console** window, from the project root:</span></span>

    ```console
      npm init -y
      npm install @aspnet/signalr
    ```     

4. <span data-ttu-id="8cfb9-135">复制*signalr.js*文件从*node_modules\\ @aspnet\signalr\dist\browser* 到*lib*项目文件夹中的。</span><span class="sxs-lookup"><span data-stu-id="8cfb9-135">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to the *lib* folder in your project.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8cfb9-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8cfb9-136">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="8cfb9-137">从**集成终端**，运行以下命令：</span><span class="sxs-lookup"><span data-stu-id="8cfb9-137">From the **Integrated Terminal**, run the following command:</span></span>

    ```console
      dotnet new razor -o SignalRChat
    ```

2. <span data-ttu-id="8cfb9-138">安装 JavaScript 客户端库使用*npm*。</span><span class="sxs-lookup"><span data-stu-id="8cfb9-138">Install the JavaScript client library using *npm*.</span></span>

    ```
      npm init -y
      npm install @aspnet/signalr
    ```

3. <span data-ttu-id="8cfb9-139">复制*signalr.js*文件从*node_modules\\ @aspnet\signalr\dist\browser* 到*lib*项目文件夹中的。</span><span class="sxs-lookup"><span data-stu-id="8cfb9-139">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to the *lib* folder in your project.</span></span>

-----

## <a name="create-the-signalr-hub"></a><span data-ttu-id="8cfb9-140">创建 SignalR Hub</span><span class="sxs-lookup"><span data-stu-id="8cfb9-140">Create the SignalR Hub</span></span>

<span data-ttu-id="8cfb9-141">允许客户端和服务器相互调用方法的高级管道作为服务的类，则集线器。</span><span class="sxs-lookup"><span data-stu-id="8cfb9-141">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8cfb9-142">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8cfb9-142">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="8cfb9-143">将类添加到项目中，通过选择**文件** > **新建** > **文件**并选择**Visual C# 类**。</span><span class="sxs-lookup"><span data-stu-id="8cfb9-143">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span>

2. <span data-ttu-id="8cfb9-144">继承自`Microsoft.AspNetCore.SignalR.Hub`。</span><span class="sxs-lookup"><span data-stu-id="8cfb9-144">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="8cfb9-145">`Hub`类包含属性和管理连接和组，以及发送和接收数据的事件。</span><span class="sxs-lookup"><span data-stu-id="8cfb9-145">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

3. <span data-ttu-id="8cfb9-146">创建`SendMessage`将消息发送到所有连接的聊天客户端的方法。</span><span class="sxs-lookup"><span data-stu-id="8cfb9-146">Create the `SendMessage` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="8cfb9-147">请注意它将返回[任务](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx)，这是因为 SignalR 是异步的。</span><span class="sxs-lookup"><span data-stu-id="8cfb9-147">Notice it returns a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), because SignalR is asynchronous.</span></span> <span data-ttu-id="8cfb9-148">更好地缩放异步代码。</span><span class="sxs-lookup"><span data-stu-id="8cfb9-148">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8cfb9-149">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8cfb9-149">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="8cfb9-150">打开*SignalRChat*在 Visual Studio 代码中的文件夹。</span><span class="sxs-lookup"><span data-stu-id="8cfb9-150">Open the *SignalRChat* folder in Visual Studio Code.</span></span>

2. <span data-ttu-id="8cfb9-151">将类添加到项目中，通过选择**文件** > **新文件**从菜单。</span><span class="sxs-lookup"><span data-stu-id="8cfb9-151">Add a class to the project by selecting **File** > **New File** from the menu.</span></span>

3. <span data-ttu-id="8cfb9-152">继承自`Microsoft.AspNetCore.SignalR.Hub`。</span><span class="sxs-lookup"><span data-stu-id="8cfb9-152">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="8cfb9-153">`Hub`类包含属性和管理连接和组，以及向客户端发送和接收数据的事件。</span><span class="sxs-lookup"><span data-stu-id="8cfb9-153">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data to clients.</span></span>

4. <span data-ttu-id="8cfb9-154">将 `SendMessage` 方法添加到类。</span><span class="sxs-lookup"><span data-stu-id="8cfb9-154">Add a `SendMessage` method to the class.</span></span> <span data-ttu-id="8cfb9-155">`SendMessage`方法将消息发送到所有连接的聊天客户端。</span><span class="sxs-lookup"><span data-stu-id="8cfb9-155">The `SendMessage` method sends a message to all connected chat clients.</span></span> <span data-ttu-id="8cfb9-156">请注意它将返回[任务](/dotnet/api/system.threading.tasks.task)，这是因为 SignalR 是异步的。</span><span class="sxs-lookup"><span data-stu-id="8cfb9-156">Notice it returns a [Task](/dotnet/api/system.threading.tasks.task), because SignalR is asynchronous.</span></span> <span data-ttu-id="8cfb9-157">更好地缩放异步代码。</span><span class="sxs-lookup"><span data-stu-id="8cfb9-157">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs?range=6-12)]

-----

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="8cfb9-158">配置项目以使用 SignalR</span><span class="sxs-lookup"><span data-stu-id="8cfb9-158">Configure the project to use SignalR</span></span>

<span data-ttu-id="8cfb9-159">必须配置 SignalR 服务器，这样就知道要传递给 SignalR 的请求。</span><span class="sxs-lookup"><span data-stu-id="8cfb9-159">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="8cfb9-160">若要配置 SignalR 项目，请修改项目的`Startup.ConfigureServices`方法。</span><span class="sxs-lookup"><span data-stu-id="8cfb9-160">To configure a SignalR project, modify the project's `Startup.ConfigureServices` method.</span></span>

   <span data-ttu-id="8cfb9-161">`services.AddSignalR` 作为的一部分添加 SignalR[中间件](xref:fundamentals/middleware/index)管道。</span><span class="sxs-lookup"><span data-stu-id="8cfb9-161">`services.AddSignalR` adds SignalR as part of the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

2. <span data-ttu-id="8cfb9-162">配置路由到你使用的中心`UseSignalR`。</span><span class="sxs-lookup"><span data-stu-id="8cfb9-162">Configure routes to your hubs using `UseSignalR`.</span></span>


   [!code-csharp[Startup](get-started/sample/Startup.cs?highlight=36,56-59)]


## <a name="create-the-signalr-client-code"></a><span data-ttu-id="8cfb9-163">创建 SignalR 客户端代码</span><span class="sxs-lookup"><span data-stu-id="8cfb9-163">Create the SignalR client code</span></span>

1. <span data-ttu-id="8cfb9-164">替换中的内容*Pages\Index.cshtml*替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="8cfb9-164">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

   [!code-cshtml[Index](get-started/sample/Pages/Index.cshtml)]

   <span data-ttu-id="8cfb9-165">前面的 HTML 显示名称和消息字段和提交按钮。</span><span class="sxs-lookup"><span data-stu-id="8cfb9-165">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="8cfb9-166">请注意在底部的脚本引用： 至 SignalR 的引用和*chat.js*。</span><span class="sxs-lookup"><span data-stu-id="8cfb9-166">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>

2. <span data-ttu-id="8cfb9-167">添加一个名为的 JavaScript 文件*chat.js*到*wwwroot\js*文件夹。</span><span class="sxs-lookup"><span data-stu-id="8cfb9-167">Add a JavaScript file, named *chat.js*, to the *wwwroot\js* folder.</span></span> <span data-ttu-id="8cfb9-168">向新文件添加以下代码：</span><span class="sxs-lookup"><span data-stu-id="8cfb9-168">Add the following code to it:</span></span>

   [!code-javascript[Index](get-started/sample/wwwroot/js/chat.js)]

## <a name="run-the-app"></a><span data-ttu-id="8cfb9-169">运行应用</span><span class="sxs-lookup"><span data-stu-id="8cfb9-169">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8cfb9-170">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8cfb9-170">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="8cfb9-171">选择**调试** > **启动而不调试**启动浏览器并加载网站本地。</span><span class="sxs-lookup"><span data-stu-id="8cfb9-171">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="8cfb9-172">从地址栏复制 URL。</span><span class="sxs-lookup"><span data-stu-id="8cfb9-172">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="8cfb9-173">打开另一个浏览器实例 （任何浏览器），然后在地址栏中粘贴该 URL。</span><span class="sxs-lookup"><span data-stu-id="8cfb9-173">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="8cfb9-174">选择任一浏览器，输入名称和消息，然后单击**发送**按钮。</span><span class="sxs-lookup"><span data-stu-id="8cfb9-174">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="8cfb9-175">名称和消息会显示在两个页面上立即。</span><span class="sxs-lookup"><span data-stu-id="8cfb9-175">The name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8cfb9-176">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8cfb9-176">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="8cfb9-177">按“调试”(F5) 生成并运行程序。</span><span class="sxs-lookup"><span data-stu-id="8cfb9-177">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="8cfb9-178">运行程序将打开一个浏览器窗口。</span><span class="sxs-lookup"><span data-stu-id="8cfb9-178">Running the program opens a browser window.</span></span>

1. <span data-ttu-id="8cfb9-179">打开另一个浏览器窗口并加载在本地网站。</span><span class="sxs-lookup"><span data-stu-id="8cfb9-179">Open another browser window and load the website locally in it.</span></span>

1. <span data-ttu-id="8cfb9-180">选择任一浏览器，输入名称和消息，然后单击**发送**按钮。</span><span class="sxs-lookup"><span data-stu-id="8cfb9-180">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="8cfb9-181">名称和消息会显示在两个页面上立即。</span><span class="sxs-lookup"><span data-stu-id="8cfb9-181">The name and message are displayed on both pages instantly.</span></span>

-----

  ![解决方案](get-started/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a><span data-ttu-id="8cfb9-183">相关资源</span><span class="sxs-lookup"><span data-stu-id="8cfb9-183">Related resources</span></span>

[<span data-ttu-id="8cfb9-184">ASP.NET 核心 SignalR 简介</span><span class="sxs-lookup"><span data-stu-id="8cfb9-184">Introduction to ASP.NET Core SignalR</span></span>](introduction.md)