---
uid: mvc/overview/getting-started/introduction/getting-started
title: "ASP.NET MVC 5 入门 |Microsoft 文档"
author: Rick-Anderson
description: "注意： 本教程的更新的版本是可以在此处使用 Visual Studio 2015。 新的教程使用 ASP.NET 核心 MVC 6，它提供许多 improvem..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: f3d8adbe-55e7-4fd4-84a8-7155bc45c676
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 8f2cb9b3f14ad95acd9e4c7a0ed26228d3c480e0
ms.sourcegitcommit: ec9371e2fbfcb8d62e7e7cae69e7752f3f205385
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2017
---
<a name="getting-started-with-aspnet-mvc-5"></a><span data-ttu-id="1b3dc-104">ASP.NET MVC 5 入门</span><span class="sxs-lookup"><span data-stu-id="1b3dc-104">Getting Started with ASP.NET MVC 5</span></span>
====================
<span data-ttu-id="1b3dc-105">通过[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="1b3dc-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE[consider RP](../../../../includes/razor.md)]

 
 <span data-ttu-id="1b3dc-106">本教程将教您生成 ASP.NET MVC 5 web 应用程序使用的基础知识[Visual Studio 2017](https://www.visualstudio.com/)。</span><span class="sxs-lookup"><span data-stu-id="1b3dc-106">This tutorial will teach you the basics of building an ASP.NET MVC 5 web app using [Visual Studio 2017](https://www.visualstudio.com/).</span></span> <span data-ttu-id="1b3dc-107">对于教程的最后一个源位于[GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie)</span><span class="sxs-lookup"><span data-stu-id="1b3dc-107">Final Source for tutorial located on [GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie)</span></span>
 
 
 <span data-ttu-id="1b3dc-108">本教程编写的[Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter[ @scottgu ](https://twitter.com/scottgu) )， [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](https://twitter.com/shanselman) )和[Rick Anderson](https://twitter.com/RickAndMSFT) ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )</span><span class="sxs-lookup"><span data-stu-id="1b3dc-108">This tutorial was written by [Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter[@scottgu](https://twitter.com/scottgu) ), [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [@shanselman](https://twitter.com/shanselman) ), and [Rick Anderson](https://twitter.com/RickAndMSFT) ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) )</span></span>
 
 <span data-ttu-id="1b3dc-109">你需要将此应用程序部署到 Azure 的 Azure 帐户：</span><span class="sxs-lookup"><span data-stu-id="1b3dc-109">You need an Azure account to deploy this app to Azure:</span></span>
 
 - <span data-ttu-id="1b3dc-110">你可以[免费建立一个 Azure 帐户](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A443DD604)-获取信用额度来试用付费版 Azure 服务，你可以使用和甚至在用完后，最多可以保留帐户和使用免费的 Azure 服务。</span><span class="sxs-lookup"><span data-stu-id="1b3dc-110">You can [open an Azure account for free](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
 - <span data-ttu-id="1b3dc-111">你可以[激活 MSDN 订户权益](https://azure.microsoft.com/en-us/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604)-MSDN 订阅为你的信用额度可以用于付费版 Azure 服务的每个月。</span><span class="sxs-lookup"><span data-stu-id="1b3dc-111">You can [activate MSDN subscriber benefits](https://azure.microsoft.com/en-us/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>


## <a name="getting-started"></a><span data-ttu-id="1b3dc-112">入门</span><span class="sxs-lookup"><span data-stu-id="1b3dc-112">Getting Started</span></span>

<span data-ttu-id="1b3dc-113">首先，安装并运行[Visual Studio 2017](https://www.visualstudio.com/)。</span><span class="sxs-lookup"><span data-stu-id="1b3dc-113">Start by installing and running [Visual Studio 2017](https://www.visualstudio.com/).</span></span>

<span data-ttu-id="1b3dc-114">Visual Studio 是一个 IDE 或集成的开发环境。</span><span class="sxs-lookup"><span data-stu-id="1b3dc-114">Visual Studio is an IDE, or integrated development environment.</span></span> <span data-ttu-id="1b3dc-115">就像使用 Microsoft Word 编写文档，你将使用 IDE 来创建应用程序。</span><span class="sxs-lookup"><span data-stu-id="1b3dc-115">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="1b3dc-116">Visual Studio 中没有向你显示各个可用的选项的底部的列表。</span><span class="sxs-lookup"><span data-stu-id="1b3dc-116">In Visual Studio there's a list along the bottom showing various options available to you.</span></span> <span data-ttu-id="1b3dc-117">此外还有一个菜单，提供另一种方法在 IDE 中执行任务。</span><span class="sxs-lookup"><span data-stu-id="1b3dc-117">There's also a menu that provides another way to perform tasks in the IDE.</span></span> <span data-ttu-id="1b3dc-118">(例如，而不是选择**新项目**从**启动**页上，你可以使用菜单并选择**文件** &gt; **新项目**.)</span><span class="sxs-lookup"><span data-stu-id="1b3dc-118">(For example, instead of selecting **New Project** from the **Start** page, you can use the menu and select **File** &gt; **New Project**.)</span></span>

   
![](getting-started/_static/image1.png)  
 

## <a name="creating-your-first-application"></a><span data-ttu-id="1b3dc-119">创建第一个应用程序</span><span class="sxs-lookup"><span data-stu-id="1b3dc-119">Creating Your First Application</span></span>

<span data-ttu-id="1b3dc-120">单击**新项目**，然后选择 Visual C# 在左侧，然后**Web** ，然后选择**ASP.NET Web 应用程序 (.NET Framework)**。</span><span class="sxs-lookup"><span data-stu-id="1b3dc-120">Click **New Project**, then select Visual C# on the left, then **Web** and then select **ASP.NET Web Application (.NET Framework)**.</span></span> <span data-ttu-id="1b3dc-121">将你的项目"MvcMovie"，然后单击**确定**。</span><span class="sxs-lookup"><span data-stu-id="1b3dc-121">Name your project "MvcMovie" and then click **OK**.</span></span>

![](getting-started/_static/image2.png)

<span data-ttu-id="1b3dc-122">在**新建 ASP.NET 项目**对话框中，单击**MVC** ，然后单击**确定**。</span><span class="sxs-lookup"><span data-stu-id="1b3dc-122">In the **New ASP.NET Project** dialog, click **MVC** and then click **OK**.</span></span>

![](getting-started/_static/image3.png)

<span data-ttu-id="1b3dc-123">因此，必须运行的应用程序现在不执行任何操作的情况下，visual Studio 刚创建的 ASP.NET MVC 项目使用默认模板 ！</span><span class="sxs-lookup"><span data-stu-id="1b3dc-123">Visual Studio used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="1b3dc-124">这是简单"Hello World ！"</span><span class="sxs-lookup"><span data-stu-id="1b3dc-124">This is a simple "Hello World!"</span></span> <span data-ttu-id="1b3dc-125">项目和它的启动你的应用程序的好时机。</span><span class="sxs-lookup"><span data-stu-id="1b3dc-125">project, and it's a good place to start your application.</span></span>

![](getting-started/_static/image4.png)

<span data-ttu-id="1b3dc-126">单击 F5 启动调试。</span><span class="sxs-lookup"><span data-stu-id="1b3dc-126">Click F5 to start debugging.</span></span> <span data-ttu-id="1b3dc-127">F5 导致 Visual Studio 启动[IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview)并运行你的 web 应用。</span><span class="sxs-lookup"><span data-stu-id="1b3dc-127">F5 causes Visual Studio to start [IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) and run your web app.</span></span> <span data-ttu-id="1b3dc-128">然后，visual Studio 启动浏览器，并打开应用程序的主页。</span><span class="sxs-lookup"><span data-stu-id="1b3dc-128">Visual Studio then launches a browser and opens the application's home page.</span></span> <span data-ttu-id="1b3dc-129">请注意，浏览器的地址栏显示`localhost:port#`和不类似于`example.com`。</span><span class="sxs-lookup"><span data-stu-id="1b3dc-129">Notice that the address bar of the browser says `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="1b3dc-130">这是因为`localhost`始终指向你自己本地的计算机，在这种情况下运行你刚才生成应用程序。</span><span class="sxs-lookup"><span data-stu-id="1b3dc-130">That's because `localhost` always points to your own local computer, which in this case is running the application you just built.</span></span> <span data-ttu-id="1b3dc-131">当 Visual Studio 运行 web 项目时，随机端口适用于 web 服务器。</span><span class="sxs-lookup"><span data-stu-id="1b3dc-131">When Visual Studio runs a web project, a random port is used for the web server.</span></span> <span data-ttu-id="1b3dc-132">在下图中，端口号是 1234年。</span><span class="sxs-lookup"><span data-stu-id="1b3dc-132">In the image below, the port number is 1234.</span></span> <span data-ttu-id="1b3dc-133">运行应用程序时，你将看到不同的端口号。</span><span class="sxs-lookup"><span data-stu-id="1b3dc-133">When you run the application, you'll see a different port number.</span></span>

![](getting-started/_static/image5.png)

<span data-ttu-id="1b3dc-134">现成此默认模板也提供家庭、 联系人和关于页。</span><span class="sxs-lookup"><span data-stu-id="1b3dc-134">Right out of the box this default template gives you Home, Contact and About pages.</span></span> <span data-ttu-id="1b3dc-135">图中不显示**主页**，**有关**和**联系人**链接。</span><span class="sxs-lookup"><span data-stu-id="1b3dc-135">The image above doesn't show the **Home**, **About** and **Contact** links.</span></span> <span data-ttu-id="1b3dc-136">根据你的浏览器窗口的大小，你可能需要单击导航图标以查看这些链接。</span><span class="sxs-lookup"><span data-stu-id="1b3dc-136">Depending on the size of your browser window, you might need to click the navigation icon to see these links.</span></span>

![](getting-started/_static/image6.png)  

<span data-ttu-id="1b3dc-137">应用程序还提供支持注册和登录。</span><span class="sxs-lookup"><span data-stu-id="1b3dc-137">The application also provides support to register and log in.</span></span> <span data-ttu-id="1b3dc-138">下一步是更改此应用程序的工作原理，并得有点了解 ASP.NET MVC。</span><span class="sxs-lookup"><span data-stu-id="1b3dc-138">The next step is to change how this application works and learn a little bit about ASP.NET MVC.</span></span> <span data-ttu-id="1b3dc-139">关闭 ASP.NET MVC 应用程序，让我们更改一些代码。</span><span class="sxs-lookup"><span data-stu-id="1b3dc-139">Close the ASP.NET MVC application and let's change some code.</span></span>

<span data-ttu-id="1b3dc-140">当前的教程的列表，请参阅[MVC 建议文章](../mvc-learning-sequence.md)。</span><span class="sxs-lookup"><span data-stu-id="1b3dc-140">For a list of current tutorials, see [MVC recommended articles](../mvc-learning-sequence.md).</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="1b3dc-141">请参阅在 Azure 上运行此应用程序</span><span class="sxs-lookup"><span data-stu-id="1b3dc-141">See this App Running on Azure</span></span>

<span data-ttu-id="1b3dc-142">若要查看与实时 web 应用运行的已完成的站点吗？</span><span class="sxs-lookup"><span data-stu-id="1b3dc-142">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="1b3dc-143">只需单击下面的按钮，可以将应用程序的完整版本部署到你的 Azure 帐户。</span><span class="sxs-lookup"><span data-stu-id="1b3dc-143">You can deploy a complete version of the app to your Azure account by simply clicking the following button.</span></span>

[![](https://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?repository=https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie&amp;WT.mc_id=deploy_azure_aspnet)

<span data-ttu-id="1b3dc-144">你需要将此解决方案部署到 Azure 的 Azure 帐户。</span><span class="sxs-lookup"><span data-stu-id="1b3dc-144">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="1b3dc-145">如果你还没有帐户，可以使用以下选项：</span><span class="sxs-lookup"><span data-stu-id="1b3dc-145">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="1b3dc-146">[免费建立一个 Azure 帐户](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A443DD604)-获取信用额度来试用付费版 Azure 服务，你可以使用和甚至在用完后，最多可以保留帐户和使用免费的 Azure 服务。</span><span class="sxs-lookup"><span data-stu-id="1b3dc-146">[Open an Azure account for free](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="1b3dc-147">[激活 MSDN 订户权益](https://azure.microsoft.com/en-us/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604)-MSDN 订阅为你的信用额度可以用于付费版 Azure 服务的每个月。</span><span class="sxs-lookup"><span data-stu-id="1b3dc-147">[Activate MSDN subscriber benefits](https://azure.microsoft.com/en-us/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="1b3dc-148">下一篇</span><span class="sxs-lookup"><span data-stu-id="1b3dc-148">Next</span></span>](adding-a-controller.md)
