---
uid: signalr/overview/deployment/using-signalr-with-azure-web-sites
title: 与在 Azure App Service Web Apps 使用 SignalR |Microsoft 文档
author: pfletcher
description: 本文档介绍如何配置的 SignalR 应用程序在 Microsoft Azure 上运行。 在本教程中的软件版本使用，Visual Studio 2013 或 Vis....
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/01/2015
ms.topic: article
ms.assetid: 2a7517a0-b88c-4162-ade3-9bf6ca7062fd
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/deployment/using-signalr-with-azure-web-sites
msc.type: authoredcontent
ms.openlocfilehash: 8386441690a3fb479ffb941ebd7c0b2f83870781
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
ms.locfileid: "28043202"
---
<a name="using-signalr-with-web-apps-in-azure-app-service"></a><span data-ttu-id="9b0d1-104">与在 Azure App Service Web Apps 使用 SignalR</span><span class="sxs-lookup"><span data-stu-id="9b0d1-104">Using SignalR with Web Apps in Azure App Service</span></span>
====================
<span data-ttu-id="9b0d1-105">通过[Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="9b0d1-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="9b0d1-106">本文档介绍如何配置的 SignalR 应用程序在 Microsoft Azure 上运行。</span><span class="sxs-lookup"><span data-stu-id="9b0d1-106">This document describes how to configure a SignalR application that runs on Microsoft Azure.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="9b0d1-107">在本教程中使用的软件版本</span><span class="sxs-lookup"><span data-stu-id="9b0d1-107">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="9b0d1-108">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)或 Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="9b0d1-108">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) or Visual Studio 2012</span></span>
> - <span data-ttu-id="9b0d1-109">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="9b0d1-109">.NET 4.5</span></span>
> - <span data-ttu-id="9b0d1-110">SignalR 版本 2</span><span class="sxs-lookup"><span data-stu-id="9b0d1-110">SignalR version 2</span></span>
> - <span data-ttu-id="9b0d1-111">Azure SDK 2.3 for Visual Studio 2013 或 2012</span><span class="sxs-lookup"><span data-stu-id="9b0d1-111">Azure SDK 2.3 for Visual Studio 2013 or 2012</span></span>
>   
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="9b0d1-112">问题和意见</span><span class="sxs-lookup"><span data-stu-id="9b0d1-112">Questions and comments</span></span>
> 
> <span data-ttu-id="9b0d1-113">请留下反馈上如何喜欢本教程的方式，我们可以提高在页面底部的注释中。</span><span class="sxs-lookup"><span data-stu-id="9b0d1-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="9b0d1-114">如果你有与本教程不直接相关的问题，你可以发布到[ASP.NET SignalR 论坛](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)， [StackOverflow.com](http://stackoverflow.com/)，或[Microsoft Azure 论坛](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).</span><span class="sxs-lookup"><span data-stu-id="9b0d1-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), [StackOverflow.com](http://stackoverflow.com/), or the [Microsoft Azure forums](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).</span></span>


## <a name="table-of-contents"></a><span data-ttu-id="9b0d1-115">目录</span><span class="sxs-lookup"><span data-stu-id="9b0d1-115">Table of Contents</span></span>

- [<span data-ttu-id="9b0d1-116">介绍</span><span class="sxs-lookup"><span data-stu-id="9b0d1-116">Introduction</span></span>](#introduction)
- [<span data-ttu-id="9b0d1-117">将 SignalR Web 应用部署到 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="9b0d1-117">Deploying a SignalR Web App to Azure App Service</span></span>](#deploying)
- [<span data-ttu-id="9b0d1-118">在 Azure App Service 上的启用 Websocket</span><span class="sxs-lookup"><span data-stu-id="9b0d1-118">Enabling WebSockets on Azure App Service</span></span>](#websocket)
- [<span data-ttu-id="9b0d1-119">使用 Azure Redis 缓存底板</span><span class="sxs-lookup"><span data-stu-id="9b0d1-119">Using the Azure Redis Cache Backplane</span></span>](#backplane)
- [<span data-ttu-id="9b0d1-120">后续步骤</span><span class="sxs-lookup"><span data-stu-id="9b0d1-120">Next Steps</span></span>](#nextsteps)

<a id="introduction"></a>
## <a name="introduction"></a><span data-ttu-id="9b0d1-121">介绍</span><span class="sxs-lookup"><span data-stu-id="9b0d1-121">Introduction</span></span>

<span data-ttu-id="9b0d1-122">ASP.NET SignalR 可用来将一个新的服务器和 web 或.NET 客户端之间的交互功能级别。</span><span class="sxs-lookup"><span data-stu-id="9b0d1-122">ASP.NET SignalR can be used to bring a new level of interactivity between servers and web or .NET clients.</span></span> <span data-ttu-id="9b0d1-123">时在 Azure 中托管，SignalR 应用程序可以充分利用高度可用的可伸缩性，并运行在云中提供高性能环境。</span><span class="sxs-lookup"><span data-stu-id="9b0d1-123">When hosted in Azure, SignalR applications can take advantage of the highly available, scalable, and performant environment that running in the cloud provides.</span></span>

<a id="deploying"></a>
## <a name="deploying-a-signalr-web-app-to-azure-app-service"></a><span data-ttu-id="9b0d1-124">将 SignalR Web 应用部署到 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="9b0d1-124">Deploying a SignalR Web App to Azure App Service</span></span>

<span data-ttu-id="9b0d1-125">SignalR 不会添加到应用程序部署到 Azure 与部署到本地服务器的任何特定的复杂性。</span><span class="sxs-lookup"><span data-stu-id="9b0d1-125">SignalR doesn't add any particular complications to deploying an application to Azure versus deploying to an on-premises server.</span></span> <span data-ttu-id="9b0d1-126">使用 SignalR 的应用程序可以承载于 Azure 而无需配置或其他设置中的任何更改 (尽管 Websocket 支持，请参阅[启用 Websocket 在 Azure App Service 上](#websocket)下面。)对于本教程中，你需要将中创建的应用程序部署[入门教程](../getting-started/tutorial-getting-started-with-signalr.md)到 Azure。</span><span class="sxs-lookup"><span data-stu-id="9b0d1-126">An application that uses SignalR can be hosted in Azure without any changes in configuration or other settings (though for WebSockets support, see [Enabling WebSockets on Azure App Service](#websocket) below.) For this tutorial, you'll deploy the application created in the [Getting Started Tutorial](../getting-started/tutorial-getting-started-with-signalr.md) to Azure.</span></span>

<span data-ttu-id="9b0d1-127">**系统必备**</span><span class="sxs-lookup"><span data-stu-id="9b0d1-127">**Prerequisites**</span></span>

- <span data-ttu-id="9b0d1-128">Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="9b0d1-128">Visual Studio 2013.</span></span> <span data-ttu-id="9b0d1-129">如果你没有 Visual Studio，Visual Studio 2013 Express for Web 是在安装中包括的 Azure sdk。</span><span class="sxs-lookup"><span data-stu-id="9b0d1-129">If you don't have Visual Studio, Visual Studio 2013 Express for Web is included in the install of the Azure SDK.</span></span>
- <span data-ttu-id="9b0d1-130">[Visual Studio 2013 的 azure SDK 2.3](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409)或[for Visual Studio 2012 的 Azure SDK 2.3](https://go.microsoft.com/fwlink/p/?linkid=323511)。</span><span class="sxs-lookup"><span data-stu-id="9b0d1-130">[Azure SDK 2.3 for Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) or [Azure SDK 2.3 for Visual Studio 2012](https://go.microsoft.com/fwlink/p/?linkid=323511).</span></span>
- <span data-ttu-id="9b0d1-131">若要完成本教程，你将需要 Azure 订阅。</span><span class="sxs-lookup"><span data-stu-id="9b0d1-131">To complete this tutorial, you will need an Azure subscription.</span></span> <span data-ttu-id="9b0d1-132">你可以[激活 MSDN 订户权益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)，或[注册试用订阅](https://azure.microsoft.com/pricing/free-trial/)。</span><span class="sxs-lookup"><span data-stu-id="9b0d1-132">You can [activate your MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), or [sign up for a trial subscription](https://azure.microsoft.com/pricing/free-trial/).</span></span>

### <a name="deploying-a-signalr-web-app-to-azure"></a><span data-ttu-id="9b0d1-133">将 SignalR web 应用部署到 Azure</span><span class="sxs-lookup"><span data-stu-id="9b0d1-133">Deploying a SignalR web app to Azure</span></span>

1. <span data-ttu-id="9b0d1-134">完成[入门教程](../getting-started/tutorial-getting-started-with-signalr.md)，或下载中完成的项目[代码库](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)。</span><span class="sxs-lookup"><span data-stu-id="9b0d1-134">Complete the [Getting Started Tutorial](../getting-started/tutorial-getting-started-with-signalr.md), or download the finished project from [Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span></span>
2. <span data-ttu-id="9b0d1-135">在 Visual Studio 中，选择**生成**，**发布 SignalR 聊天**。</span><span class="sxs-lookup"><span data-stu-id="9b0d1-135">In Visual Studio, select **Build**, **Publish SignalR Chat**.</span></span>
3. <span data-ttu-id="9b0d1-136">在"发布 Web"对话框中，选择"Windows Azure 网站"。</span><span class="sxs-lookup"><span data-stu-id="9b0d1-136">In the "Publish Web" dialog, select "Windows Azure Web Sites".</span></span>

    ![选择 Azure 网站](using-signalr-with-azure-web-sites/_static/image1.png)
4. <span data-ttu-id="9b0d1-138">如果您没有登录到你的 Microsoft 帐户，请单击**登录...** 在"选择现有网站"对话框中，并登录。</span><span class="sxs-lookup"><span data-stu-id="9b0d1-138">If you aren't signed in to your Microsoft account, click **Sign In...** in the "Select Existing Web Site" dialog, and sign in.</span></span>

    ![选择现有网站](using-signalr-with-azure-web-sites/_static/image2.png)    ![登录到 Azure](using-signalr-with-azure-web-sites/_static/image3.png)
5. <span data-ttu-id="9b0d1-141">在"选择现有网站"对话框中，单击**新建**。</span><span class="sxs-lookup"><span data-stu-id="9b0d1-141">In the "Select Existing Web Site" dialog, click **New**.</span></span>

    ![新建网站](using-signalr-with-azure-web-sites/_static/image4.png)
6. <span data-ttu-id="9b0d1-143">在"Windows Azure 上的创建网站"对话框中，输入唯一的应用程序名称。</span><span class="sxs-lookup"><span data-stu-id="9b0d1-143">In the "Create site on Windows Azure" dialog, enter a unique app name.</span></span> <span data-ttu-id="9b0d1-144">在区域下拉列表中选择离您最近的区域。</span><span class="sxs-lookup"><span data-stu-id="9b0d1-144">Select the region closest to you in the Region dropdown.</span></span> <span data-ttu-id="9b0d1-145">单击 **“创建”**。</span><span class="sxs-lookup"><span data-stu-id="9b0d1-145">Click **Create**.</span></span>

    ![在 Azure 上创建站点](using-signalr-with-azure-web-sites/_static/image5.png)
7. <span data-ttu-id="9b0d1-147">在"发布 Web"对话框中，单击**发布**。</span><span class="sxs-lookup"><span data-stu-id="9b0d1-147">In the "Publish Web" dialog, click **Publish**.</span></span>

    ![发布站点](using-signalr-with-azure-web-sites/_static/image6.png)
8. <span data-ttu-id="9b0d1-149">应用程序完成后发布，将在浏览器中打开托管在 Azure App Service Web Apps 中的 SignalR 聊天应用程序。</span><span class="sxs-lookup"><span data-stu-id="9b0d1-149">When the app has completed publishing, the SignalR Chat application hosted in Azure App Service Web Apps will open in a browser.</span></span>

    ![在浏览器中打开站点](using-signalr-with-azure-web-sites/_static/image7.png)

<a id="websocket"></a>
### <a name="enabling-websockets-on-azure-app-service-web-apps"></a><span data-ttu-id="9b0d1-151">在 Azure App Service Web Apps 上的启用 Websocket</span><span class="sxs-lookup"><span data-stu-id="9b0d1-151">Enabling WebSockets on Azure App Service Web Apps</span></span>

<span data-ttu-id="9b0d1-152">Websocket 需要显式启用 web 应用中使用 SignalR 应用程序; 中否则，将使用其他协议 (请参阅[传输和回退](../getting-started/introduction-to-signalr.md#transports)有关详细信息)。</span><span class="sxs-lookup"><span data-stu-id="9b0d1-152">WebSockets needs to be explicitly enabled in your web app to be used in a SignalR application; otherwise, other protocols will be used (See [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports) for details).</span></span>

<span data-ttu-id="9b0d1-153">为了在 Azure App Service Web Apps 上使用 Websocket，在在 web 应用的配置节中启用它。</span><span class="sxs-lookup"><span data-stu-id="9b0d1-153">In order to use WebSockets on Azure App Service Web Apps, enable it in the configuration section of the web app.</span></span> <span data-ttu-id="9b0d1-154">若要执行此操作，打开你的 web 应用中[Azure 管理门户](https://manage.windowsazure.com/)，并选择配置。</span><span class="sxs-lookup"><span data-stu-id="9b0d1-154">To do this, open your web app in the [Azure Management Portal](https://manage.windowsazure.com/), and select Configure.</span></span>

![配置选项卡](using-signalr-with-azure-web-sites/_static/image8.png)

<span data-ttu-id="9b0d1-156">在配置页的顶部，确保.NET 4.5 用于你的 web 应用。</span><span class="sxs-lookup"><span data-stu-id="9b0d1-156">At the top of the configuration page, ensure that .NET 4.5 is used for your web app.</span></span>

![.NET framework 版本 4.5 设置](using-signalr-with-azure-web-sites/_static/image9.png)

<span data-ttu-id="9b0d1-158">在配置页上，在**Websocket**设置中选择**上**。</span><span class="sxs-lookup"><span data-stu-id="9b0d1-158">On the configuration page, in the **WebSockets** setting, select **On**.</span></span>

![Websocket 设置： 上](using-signalr-with-azure-web-sites/_static/image10.png)

<span data-ttu-id="9b0d1-160">在配置页的底部，选择**保存**以保存所做的更改。</span><span class="sxs-lookup"><span data-stu-id="9b0d1-160">At the bottom of the Configuration page, select **Save** to save your changes.</span></span>

![保存设置](using-signalr-with-azure-web-sites/_static/image11.png)

<a id="backplane"></a>
## <a name="using-the-azure-redis-cache-backplane"></a><span data-ttu-id="9b0d1-162">使用 Azure Redis 缓存底板</span><span class="sxs-lookup"><span data-stu-id="9b0d1-162">Using the Azure Redis Cache Backplane</span></span>

<span data-ttu-id="9b0d1-163">如果 web 应用，使用多个实例，并且这些实例的用户需要进行交互与另一个 （，以便，例如，创建一个实例中的聊天消息可以访问连接到其他实例的用户），则[Azure Redis 缓存底板](../performance/scaleout-with-redis.md)必须在你的应用程序中实现。</span><span class="sxs-lookup"><span data-stu-id="9b0d1-163">If you use multiple instances for your web app, and the users of those instances need to interact with one another (so that, for instance, chat messages created in one instance can reach the users connected to other instances), the [Azure Redis Cache backplane](../performance/scaleout-with-redis.md) must be implemented in your application.</span></span>

<a id="nextsteps"></a>
## <a name="next-steps"></a><span data-ttu-id="9b0d1-164">后续步骤</span><span class="sxs-lookup"><span data-stu-id="9b0d1-164">Next Steps</span></span>

<span data-ttu-id="9b0d1-165">有关在 Azure App Service Web Apps 的详细信息，请参阅[Web Apps 概述](https://azure.microsoft.com/documentation/articles/app-service-web-overview/)。</span><span class="sxs-lookup"><span data-stu-id="9b0d1-165">For more information on Web Apps in Azure App Service, see [Web Apps overview](https://azure.microsoft.com/documentation/articles/app-service-web-overview/).</span></span>
