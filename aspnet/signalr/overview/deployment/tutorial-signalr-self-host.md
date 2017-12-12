---
uid: signalr/overview/deployment/tutorial-signalr-self-host
title: "教程： 自承载 SignalR |Microsoft 文档"
author: pfletcher
description: "本教程演示如何创建自承载的 SignalR 2 服务器，以及如何与 JavaScript 客户端连接到它。 教程 V 中使用的软件版本..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 400db427-27af-4f2f-abf0-5486d5e024b5
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/deployment/tutorial-signalr-self-host
msc.type: authoredcontent
ms.openlocfilehash: 997756ff8d48e41da981491d6154f3107ec7a051
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="tutorial-signalr-self-host"></a><span data-ttu-id="af5a5-104">自承载的教程： SignalR</span><span class="sxs-lookup"><span data-stu-id="af5a5-104">Tutorial: SignalR Self-Host</span></span>
====================
<span data-ttu-id="af5a5-105">通过[Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="af5a5-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[<span data-ttu-id="af5a5-106">下载已完成的项目</span><span class="sxs-lookup"><span data-stu-id="af5a5-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/SignalR-Self-Host-Sample-6da0f383)

> <span data-ttu-id="af5a5-107">本教程演示如何创建自承载的 SignalR 2 服务器，以及如何与 JavaScript 客户端连接到它。</span><span class="sxs-lookup"><span data-stu-id="af5a5-107">This tutorial shows how to create a self-hosted SignalR 2 server, and how to connect to it with a JavaScript client.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="af5a5-108">在本教程中使用的软件版本</span><span class="sxs-lookup"><span data-stu-id="af5a5-108">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="af5a5-109">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="af5a5-109">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="af5a5-110">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="af5a5-110">.NET 4.5</span></span>
> - <span data-ttu-id="af5a5-111">SignalR 版本 2</span><span class="sxs-lookup"><span data-stu-id="af5a5-111">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="af5a5-112">本教程使用 Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="af5a5-112">Using Visual Studio 2012 with this tutorial</span></span>
> 
> 
> <span data-ttu-id="af5a5-113">若要使用本教程使用 Visual Studio 2012，请执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="af5a5-113">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
> 
> - <span data-ttu-id="af5a5-114">更新你[程序包管理器](http://docs.nuget.org/docs/start-here/installing-nuget)的最新版本。</span><span class="sxs-lookup"><span data-stu-id="af5a5-114">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="af5a5-115">安装[Web 平台安装程序](https://www.microsoft.com/web/downloads/platform.aspx)。</span><span class="sxs-lookup"><span data-stu-id="af5a5-115">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="af5a5-116">在 Web 平台安装程序中，搜索并安装**ASP.NET 和 Web Tools for Visual Studio 2012 的 2013.1**。</span><span class="sxs-lookup"><span data-stu-id="af5a5-116">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="af5a5-117">这将安装 SignalR 类的 Visual Studio 模板，如**中心**。</span><span class="sxs-lookup"><span data-stu-id="af5a5-117">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="af5a5-118">某些模板 (如**OWIN Startup 类**) 将不可用; 对于这些数据，改为使用的类文件。</span><span class="sxs-lookup"><span data-stu-id="af5a5-118">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="af5a5-119">问题和意见</span><span class="sxs-lookup"><span data-stu-id="af5a5-119">Questions and comments</span></span>
> 
> <span data-ttu-id="af5a5-120">请留下反馈上如何喜欢本教程的方式，我们可以提高在页面底部的注释中。</span><span class="sxs-lookup"><span data-stu-id="af5a5-120">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="af5a5-121">如果你有与本教程不直接相关的问题，你可以发布到[ASP.NET SignalR 论坛](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或[StackOverflow.com](http://stackoverflow.com/)。</span><span class="sxs-lookup"><span data-stu-id="af5a5-121">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="af5a5-122">概述</span><span class="sxs-lookup"><span data-stu-id="af5a5-122">Overview</span></span>

<span data-ttu-id="af5a5-123">SignalR 服务器通常承载在 IIS 中，ASP.NET 应用程序中，但它也可以是自承载 （如控制台应用程序或 Windows 服务） 使用的自承载的库。</span><span class="sxs-lookup"><span data-stu-id="af5a5-123">A SignalR server is usually hosted in an ASP.NET application in IIS, but it can also be self-hosted (such as in a console application or Windows service) using the self-host library.</span></span> <span data-ttu-id="af5a5-124">此库，就像所有 SignalR 2 基于 OWIN ([打开适用于.NET 的 Web 界面](http://owin.org))。</span><span class="sxs-lookup"><span data-stu-id="af5a5-124">This library, like all of SignalR 2, is built on OWIN ([Open Web Interface for .NET](http://owin.org)).</span></span> <span data-ttu-id="af5a5-125">OWIN 定义.NET web 服务器和 web 应用程序之间的抽象。</span><span class="sxs-lookup"><span data-stu-id="af5a5-125">OWIN defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="af5a5-126">OWIN 将在服务器上，这使 OWIN 适合于你自己的过程，在 IIS 外部的 web 应用程序的自承载的 web 应用程序中脱离出来。</span><span class="sxs-lookup"><span data-stu-id="af5a5-126">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS.</span></span>

<span data-ttu-id="af5a5-127">不在 IIS 中承载的原因包括：</span><span class="sxs-lookup"><span data-stu-id="af5a5-127">Reasons for not hosting in IIS include:</span></span>

- <span data-ttu-id="af5a5-128">IIS 不可用或必要的如不含 IIS 的现有服务器场的其中的环境。</span><span class="sxs-lookup"><span data-stu-id="af5a5-128">Environments where IIS is not available or desirable, such as an existing server farm without IIS.</span></span>
- <span data-ttu-id="af5a5-129">IIS 的性能开销需要避免这样做。</span><span class="sxs-lookup"><span data-stu-id="af5a5-129">The performance overhead of IIS needs to be avoided.</span></span>
- <span data-ttu-id="af5a5-130">SignalR 功能是添加到的现有应用程序在 Windows 服务、 Azure 辅助角色或其他进程中运行。</span><span class="sxs-lookup"><span data-stu-id="af5a5-130">SignalR functionality is to be added to an exising application that runs in a Windows Service, Azure worker role, or other process.</span></span>

<span data-ttu-id="af5a5-131">如果正在作为自承载出于性能原因开发解决方案，它具有测试也建议在 IIS 中托管的应用程序来确定的性能优势。</span><span class="sxs-lookup"><span data-stu-id="af5a5-131">If a solution is being developed as self-host for performance reasons, it's recommended to also test the application hosted in IIS to determine the performance benefit.</span></span>

<span data-ttu-id="af5a5-132">本教程包含以下各节：</span><span class="sxs-lookup"><span data-stu-id="af5a5-132">This tutorial contains the following sections:</span></span>

- [<span data-ttu-id="af5a5-133">创建服务器</span><span class="sxs-lookup"><span data-stu-id="af5a5-133">Creating the server</span></span>](#server)
- [<span data-ttu-id="af5a5-134">访问与 JavaScript 客户端的服务器</span><span class="sxs-lookup"><span data-stu-id="af5a5-134">Accessing the server with a JavaScript client</span></span>](#js)

<a id="server"></a>

## <a name="creating-the-server"></a><span data-ttu-id="af5a5-135">创建服务器</span><span class="sxs-lookup"><span data-stu-id="af5a5-135">Creating the server</span></span>

<span data-ttu-id="af5a5-136">在本教程中，你将在控制台应用程序中，创建托管的服务器，但服务器可以承载于任何种类的过程中，如 Windows 服务或 Azure 辅助角色。</span><span class="sxs-lookup"><span data-stu-id="af5a5-136">In this tutorial, you'll create a server that's hosted in a console application, but the server can be hosted in any sort of process, such as a Windows service or Azure worker role.</span></span> <span data-ttu-id="af5a5-137">有关用于宿主 SignalR 服务器 Windows 服务中的示例代码，请参阅[Windows 服务中的 Self-Hosting SignalR](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3)。</span><span class="sxs-lookup"><span data-stu-id="af5a5-137">For sample code for hosting a SignalR server in a Windows Service, see [Self-Hosting SignalR in a Windows Service](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).</span></span>

1. <span data-ttu-id="af5a5-138">使用管理员特权打开 Visual Studio 2013。</span><span class="sxs-lookup"><span data-stu-id="af5a5-138">Open Visual Studio 2013 with administrator privileges.</span></span> <span data-ttu-id="af5a5-139">选择**文件**，**新项目**。</span><span class="sxs-lookup"><span data-stu-id="af5a5-139">Select **File**, **New Project**.</span></span> <span data-ttu-id="af5a5-140">选择**Windows**下**Visual C#**中的节点**模板**窗格中，然后选择**控制台应用程序**模板。</span><span class="sxs-lookup"><span data-stu-id="af5a5-140">Select **Windows** under the **Visual C#** node in the **Templates** pane, and select the **Console Application** template.</span></span> <span data-ttu-id="af5a5-141">将新项目"SignalRSelfHost"，然后单击**确定**。</span><span class="sxs-lookup"><span data-stu-id="af5a5-141">Name the new project "SignalRSelfHost" and click **OK**.</span></span>

    ![](tutorial-signalr-self-host/_static/image1.png)
2. <span data-ttu-id="af5a5-142">通过选择打开库包管理器控制台**工具**，**库程序包管理器**，**程序包管理器控制台**。</span><span class="sxs-lookup"><span data-stu-id="af5a5-142">Open the library package manager console by selecting **Tools**, **Library Package Manager**, **Package Manager Console**.</span></span>
3. <span data-ttu-id="af5a5-143">在包管理器控制台中，输入以下命令：</span><span class="sxs-lookup"><span data-stu-id="af5a5-143">In the package manager console, enter the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample1.ps1)]

    <span data-ttu-id="af5a5-144">此命令将 SignalR 2 自承载库添加到项目。</span><span class="sxs-lookup"><span data-stu-id="af5a5-144">This command adds the SignalR 2 Self-Host libraries to the project.</span></span>
4. <span data-ttu-id="af5a5-145">在包管理器控制台中，输入以下命令：</span><span class="sxs-lookup"><span data-stu-id="af5a5-145">In the package manager console, enter the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample2.ps1)]

    <span data-ttu-id="af5a5-146">此命令将 Microsoft.Owin.Cors 库添加到项目。</span><span class="sxs-lookup"><span data-stu-id="af5a5-146">This command adds the Microsoft.Owin.Cors library to the project.</span></span> <span data-ttu-id="af5a5-147">此库将用于跨域支持，需要托管 SignalR 和不同的域中的网页客户端应用程序。</span><span class="sxs-lookup"><span data-stu-id="af5a5-147">This library will be used for cross-domain support, which is required for applications that host SignalR and a web page client in different domains.</span></span> <span data-ttu-id="af5a5-148">由于你将托管 SignalR 服务器和 web 客户端在不同的端口，这意味着必须为这些组件之间的通信启用该跨域。</span><span class="sxs-lookup"><span data-stu-id="af5a5-148">Since you'll be hosting the SignalR server and the web client on different ports, this means that cross-domain must be enabled for communication between these components.</span></span>
5. <span data-ttu-id="af5a5-149">将 Program.cs 的内容替换为以下代码。</span><span class="sxs-lookup"><span data-stu-id="af5a5-149">Replace the contents of Program.cs with the following code.</span></span>

    [!code-csharp[Main](tutorial-signalr-self-host/samples/sample3.cs)]

    <span data-ttu-id="af5a5-150">上面的代码包括三个类：</span><span class="sxs-lookup"><span data-stu-id="af5a5-150">The above code includes three classes:</span></span>

    - <span data-ttu-id="af5a5-151">**程序**，包括**Main**方法定义执行的主要路径。</span><span class="sxs-lookup"><span data-stu-id="af5a5-151">**Program**, including the **Main** method defining the primary path of execution.</span></span> <span data-ttu-id="af5a5-152">在此方法中，类型的 web 应用程序**启动**开始时间为指定的 URL (`http://localhost:8080`)。</span><span class="sxs-lookup"><span data-stu-id="af5a5-152">In this method, a web application of type **Startup** is started at the specified URL (`http://localhost:8080`).</span></span> <span data-ttu-id="af5a5-153">如果安全终结点上需要，可以实现 SSL。</span><span class="sxs-lookup"><span data-stu-id="af5a5-153">If security is required on the endpoint, SSL can be implemented.</span></span> <span data-ttu-id="af5a5-154">请参阅[如何： 使用 SSL 证书配置端口](https://msdn.microsoft.com/en-us/library/ms733791.aspx)有关详细信息。</span><span class="sxs-lookup"><span data-stu-id="af5a5-154">See [How to: Configure a Port with an SSL Certificate](https://msdn.microsoft.com/en-us/library/ms733791.aspx) for more information.</span></span>
    - <span data-ttu-id="af5a5-155">**启动**、 包含 SignalR 服务器的配置的类 (本教程使用的唯一配置是对调用`UseCors`)，和对`MapSignalR`，其中的项目中创建中心的任何对象的路由。</span><span class="sxs-lookup"><span data-stu-id="af5a5-155">**Startup**, the class containing the configuration for the SignalR server (the only configuration this tutorial uses is the call to `UseCors`), and the call to `MapSignalR`, which creates routes for any Hub objects in the project.</span></span>
    - <span data-ttu-id="af5a5-156">**MyHub**，应用程序将提供给客户端的 SignalR Hub 类。</span><span class="sxs-lookup"><span data-stu-id="af5a5-156">**MyHub**, the SignalR Hub class that the application will provide to clients.</span></span> <span data-ttu-id="af5a5-157">此类具有一个方法，**发送**，客户端将调用将一条消息广播到所有其他连接的客户端。</span><span class="sxs-lookup"><span data-stu-id="af5a5-157">This class has a single method, **Send**, that clients will call to broadcast a message to all other connected clients.</span></span>
6. <span data-ttu-id="af5a5-158">编译并运行该应用程序。</span><span class="sxs-lookup"><span data-stu-id="af5a5-158">Compile and run the application.</span></span> <span data-ttu-id="af5a5-159">服务器正在运行的地址应显示在控制台窗口中。</span><span class="sxs-lookup"><span data-stu-id="af5a5-159">The address that the server is running should show in a console window.</span></span>

    ![](tutorial-signalr-self-host/_static/image2.png)
7. <span data-ttu-id="af5a5-160">如果执行失败，异常`System.Reflection.TargetInvocationException was unhandled`，你将需要重新启动 Visual Studio，使用管理员特权。</span><span class="sxs-lookup"><span data-stu-id="af5a5-160">If execution fails with the exception `System.Reflection.TargetInvocationException was unhandled`, you will need to restart Visual Studio with administrator privileges.</span></span>
8. <span data-ttu-id="af5a5-161">停止应用程序，然后继续下一节。</span><span class="sxs-lookup"><span data-stu-id="af5a5-161">Stop the application before proceeding to the next section.</span></span>

<a id="js"></a>

## <a name="accessing-the-server-with-a-javascript-client"></a><span data-ttu-id="af5a5-162">访问与 JavaScript 客户端的服务器</span><span class="sxs-lookup"><span data-stu-id="af5a5-162">Accessing the server with a JavaScript client</span></span>

<span data-ttu-id="af5a5-163">在本部分中，你将使用相同的 JavaScript 客户端从[入门教程](../getting-started/tutorial-getting-started-with-signalr.md)。</span><span class="sxs-lookup"><span data-stu-id="af5a5-163">In this section, you'll use the same JavaScript client from the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="af5a5-164">我们将仅使到客户端，则需要显式定义的中心 URL 的一个修改。</span><span class="sxs-lookup"><span data-stu-id="af5a5-164">We'll only make one modification to the client, which is to explicitly define the hub URL.</span></span> <span data-ttu-id="af5a5-165">使用自承载的应用程序，服务器可能不一定是在相同的地址作为连接 URL （由于反向代理和负载平衡器），因此需要显式定义该 URL。</span><span class="sxs-lookup"><span data-stu-id="af5a5-165">With a self-hosted application, the server may not necessarily be at the same address as the connection URL (due to reverse proxies and load balancers), so the URL needs to be defined explicitly.</span></span>

1. <span data-ttu-id="af5a5-166">在**解决方案资源管理器**，右击该解决方案并选择**添加**，**新项目**。</span><span class="sxs-lookup"><span data-stu-id="af5a5-166">In **Solution Explorer**, right-click on the solution and select **Add**, **New Project**.</span></span> <span data-ttu-id="af5a5-167">选择**Web**节点，然后选择**ASP.NET Web 应用程序**模板。</span><span class="sxs-lookup"><span data-stu-id="af5a5-167">Select the **Web** node, and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="af5a5-168">将"JavascriptClient"的项目，然后单击**确定**。</span><span class="sxs-lookup"><span data-stu-id="af5a5-168">Name the project "JavascriptClient" and click **OK**.</span></span>

    ![](tutorial-signalr-self-host/_static/image3.png)
2. <span data-ttu-id="af5a5-169">选择**空**模板，并保留未选定的剩余选项。</span><span class="sxs-lookup"><span data-stu-id="af5a5-169">Select the **Empty** template, and leave the remaining options unselected.</span></span> <span data-ttu-id="af5a5-170">选择**创建项目**。</span><span class="sxs-lookup"><span data-stu-id="af5a5-170">Select **Create Project**.</span></span>

    ![](tutorial-signalr-self-host/_static/image4.png)
3. <span data-ttu-id="af5a5-171">在包管理器控制台中，选择中的"JavascriptClient"项目**默认项目**下拉列表中，并执行以下命令：</span><span class="sxs-lookup"><span data-stu-id="af5a5-171">In the package manager console, select the "JavascriptClient" project in the **Default project** drop-down, and execute the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample4.ps1)]

    <span data-ttu-id="af5a5-172">此命令将在客户端安装你将需要的 SignalR 和 JQuery 库。</span><span class="sxs-lookup"><span data-stu-id="af5a5-172">This command installs the SignalR and JQuery libraries that you'll need in the client.</span></span>
4. <span data-ttu-id="af5a5-173">右键单击项目并选择**添加**，**新项**。</span><span class="sxs-lookup"><span data-stu-id="af5a5-173">Right-click on your project and select **Add**, **New Item**.</span></span> <span data-ttu-id="af5a5-174">选择**Web**节点，然后选择 HTML 页。</span><span class="sxs-lookup"><span data-stu-id="af5a5-174">Select the **Web** node, and select HTML Page.</span></span> <span data-ttu-id="af5a5-175">将该页命名为**Default.html**。</span><span class="sxs-lookup"><span data-stu-id="af5a5-175">Name the page **Default.html**.</span></span>

    ![](tutorial-signalr-self-host/_static/image5.png)
5. <span data-ttu-id="af5a5-176">新的 HTML 页的内容替换为以下代码。</span><span class="sxs-lookup"><span data-stu-id="af5a5-176">Replace the contents of the new HTML page with the following code.</span></span> <span data-ttu-id="af5a5-177">请验证脚本引用此处匹配项目的脚本文件夹中的脚本。</span><span class="sxs-lookup"><span data-stu-id="af5a5-177">Verify that the script references here match the scripts in the Scripts folder of the project.</span></span>

    [!code-html[Main](tutorial-signalr-self-host/samples/sample5.html?highlight=31-32)]

    <span data-ttu-id="af5a5-178">下面的代码 （在上面的代码示例中突出显示） 是对客户端使用 （除了 SignalR beta 版 2 到升级代码） 获取开始本教程中所做的添加。</span><span class="sxs-lookup"><span data-stu-id="af5a5-178">The following code (highlighted in the code sample above) is the addition that you've made to the client used in the Getting Stared tutorial (in addition to upgrading the code to SignalR version 2 beta).</span></span> <span data-ttu-id="af5a5-179">此代码行显式设置的基本连接 URL SignalR 在服务器上。</span><span class="sxs-lookup"><span data-stu-id="af5a5-179">This line of code explicitly sets the base connection URL for SignalR on the server.</span></span>

    [!code-javascript[Main](tutorial-signalr-self-host/samples/sample6.js)]
6. <span data-ttu-id="af5a5-180">右键单击该解决方案，然后选择**设置启动项目...**.选择**多启动项目**单选按钮，并设置这两个项目的**操作**到**启动**。</span><span class="sxs-lookup"><span data-stu-id="af5a5-180">Right-click on the solution, and select **Set Startup Projects...**. Select the **Multiple startup projects** radio button, and set both projects' **Action** to **Start**.</span></span>

    ![](tutorial-signalr-self-host/_static/image6.png)
7. <span data-ttu-id="af5a5-181">右键单击"Default.html"并选择**设为起始页**。</span><span class="sxs-lookup"><span data-stu-id="af5a5-181">Right-click on "Default.html" and select **Set As Start Page**.</span></span>
8. <span data-ttu-id="af5a5-182">运行该应用程序。</span><span class="sxs-lookup"><span data-stu-id="af5a5-182">Run the application.</span></span> <span data-ttu-id="af5a5-183">服务器和页上将启动。</span><span class="sxs-lookup"><span data-stu-id="af5a5-183">The server and page will launch.</span></span> <span data-ttu-id="af5a5-184">你可能需要重新加载网页 (或选择**继续**在调试器中) 如果该服务器已启动之前加载页面。</span><span class="sxs-lookup"><span data-stu-id="af5a5-184">You may need to reload the web page (or select **Continue** in the debugger) if the page loads before the server is started.</span></span>
9. <span data-ttu-id="af5a5-185">在浏览器中，提供用户名出现提示时。</span><span class="sxs-lookup"><span data-stu-id="af5a5-185">In the browser, provide a username when prompted.</span></span> <span data-ttu-id="af5a5-186">该页面的 URL 复制到另一个浏览器选项卡或窗口，并提供不同的用户名。</span><span class="sxs-lookup"><span data-stu-id="af5a5-186">Copy the page's URL into another browser tab or window and provide a different username.</span></span> <span data-ttu-id="af5a5-187">你将能够将消息从一个浏览器窗格中发送到另一个，如下所示入门教程。</span><span class="sxs-lookup"><span data-stu-id="af5a5-187">You will be able to send messages from one browser pane to the other, as in the Getting Started tutorial.</span></span>
