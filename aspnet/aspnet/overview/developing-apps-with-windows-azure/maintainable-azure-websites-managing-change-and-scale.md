---
uid: aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
title: "在实验室上处理： 容易维护的 Azure 网站： 管理更改和缩放 |Microsoft 文档"
author: rick-anderson
description: "Microsoft Azure，可以轻松生成并部署到生产环境的网站。 但你还未完成你的应用程序处于活动状态时，你刚开始 ！ 你..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: ecfd0eb4-c4ad-44e6-9db9-a2a66611ff6a
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
msc.type: authoredcontent
ms.openlocfilehash: 3d24c633368abc14efcd9fcf200a4d05c5b182c9
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="hands-on-lab-maintainable-azure-websites-managing-change-and-scale"></a><span data-ttu-id="5c2bd-105">在实验室上处理： 容易维护的 Azure 网站： 管理更改和缩放</span><span class="sxs-lookup"><span data-stu-id="5c2bd-105">Hands on Lab: Maintainable Azure Websites: Managing Change and Scale</span></span>
====================
<span data-ttu-id="5c2bd-106">通过[Web 营地团队](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="5c2bd-106">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="5c2bd-107">下载 Web 营地培训工具包</span><span class="sxs-lookup"><span data-stu-id="5c2bd-107">Download Web Camps Training Kit</span></span>](http://aka.ms/webcamps-training-kit)

> <span data-ttu-id="5c2bd-108">Microsoft Azure，可以轻松生成并部署到生产环境的网站。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-108">Microsoft Azure makes it easy to build and deploy websites to production.</span></span> <span data-ttu-id="5c2bd-109">但你还未完成你的应用程序处于活动状态时，你刚开始 ！</span><span class="sxs-lookup"><span data-stu-id="5c2bd-109">But you're not done when your application is live, you're just getting started!</span></span> <span data-ttu-id="5c2bd-110">你将需要处理更改要求、 数据库更新、 缩放和的详细信息。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-110">You'll need to handle changing requirements, database updates, scale, and more.</span></span> <span data-ttu-id="5c2bd-111">幸运的是，Azure App Service 具有您的要求，并提供充足的功能来帮助您保留您的网站平稳运行。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-111">Fortunately, Azure App Service has you covered, with plenty of features to help you keep your sites running smoothly.</span></span>
> 
> <span data-ttu-id="5c2bd-112">Azure 提供安全、 灵活的开发、 部署和缩放选项任何规模的 web 应用程序。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-112">Azure offers secure and flexible development, deployment and scaling options for any size web application.</span></span> <span data-ttu-id="5c2bd-113">利用现有工具创建和部署应用程序而无需管理基础结构。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-113">Leverage your existing tools to create and deploy applications without the hassle of managing infrastructure.</span></span>
> 
> <span data-ttu-id="5c2bd-114">生产 web 应用程序自行以分钟为单位的设置轻松地部署使用你最喜欢的开发工具创建的内容。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-114">Provision a production web application yourself in minutes by easily deploying content created using your favorite development tool.</span></span> <span data-ttu-id="5c2bd-115">你可以部署现有网站直接从支持的源控件**Git**， **GitHub**， **Bitbucket**， **TFS**，并甚至**DropBox**。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-115">You can deploy an existing site directly from source control with support for **Git**, **GitHub**, **Bitbucket**, **TFS**, and even **DropBox**.</span></span> <span data-ttu-id="5c2bd-116">部署直接通过你喜欢的 IDE 或通过脚本使用**PowerShell** Windows 中或**CLI**在任何操作系统上运行的工具。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-116">Deploy directly from your favorite IDE or from scripts using **PowerShell** in Windows or **CLI** tools running on any OS.</span></span> <span data-ttu-id="5c2bd-117">部署后，跟踪你的站点进行连续部署的支持不断最新。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-117">Once deployed, keep your sites constantly up-to-date with support for continuous deployment.</span></span>
> 
> <span data-ttu-id="5c2bd-118">无论大小，azure 会为任何数据，提供可缩放、 持久的云存储、 备份和恢复解决方案。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-118">Azure provides scalable, durable cloud storage, backup, and recovery solutions for any data, big or small.</span></span> <span data-ttu-id="5c2bd-119">在部署到生产环境，如表、 Blob 和 SQL 数据库的存储服务的应用程序，使你在云中将应用程序缩放。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-119">When deploying applications to a production environment, storage services such as Tables, Blobs and SQL Databases, help you scale your application in the cloud.</span></span>
> 
> <span data-ttu-id="5c2bd-120">使用 SQL 数据库，务必使你的工作效率的数据库部署你的应用程序的新版本时保持最新。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-120">With SQL Databases, it is important to keep your productive database up-to-date when deploying new versions of your application.</span></span> <span data-ttu-id="5c2bd-121">感谢到**Entity Framework Code First 迁移**，已简化的开发和部署你的数据模型以在几分钟内更新你的环境。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-121">Thanks to **Entity Framework Code First Migrations**, the development and deployment of your data model has been simplified to update your environments in minutes.</span></span> <span data-ttu-id="5c2bd-122">本动手实验将显示在 web 应用部署到 Microsoft Azure 中的生产环境时，可能会遇到的不同主题。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-122">This hands-on lab will show you the different topics you could encounter when deploying your web app to production environments in Microsoft Azure.</span></span>
> 
> <span data-ttu-id="5c2bd-123">在 Web 营地培训工具包中，在包括所有的示例代码和代码段[http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit)。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-123">All sample code and snippets are included in the Web Camps Training Kit, available at [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).</span></span>
> 
> <span data-ttu-id="5c2bd-124">本主题的详细深入覆盖率，请参阅[构建真实世界云应用程序与 Azure 的电子书](building-real-world-cloud-apps-with-windows-azure/introduction.md)。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-124">For more in-depth coverage of this topic, see the [Building Real-World Cloud Apps with Azure e-book](building-real-world-cloud-apps-with-windows-azure/introduction.md).</span></span>


<a id="Overview"></a>
## <a name="overview"></a><span data-ttu-id="5c2bd-125">概述</span><span class="sxs-lookup"><span data-stu-id="5c2bd-125">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="5c2bd-126">目标</span><span class="sxs-lookup"><span data-stu-id="5c2bd-126">Objectives</span></span>

<span data-ttu-id="5c2bd-127">在此动手实验中，你将了解如何：</span><span class="sxs-lookup"><span data-stu-id="5c2bd-127">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="5c2bd-128">使用现有模型中启用 Entity Framework 迁移</span><span class="sxs-lookup"><span data-stu-id="5c2bd-128">Enable Entity Framework Migrations with an existing model</span></span>
- <span data-ttu-id="5c2bd-129">更新的对象模型和相应地使用 Entity Framework 迁移数据库</span><span class="sxs-lookup"><span data-stu-id="5c2bd-129">Update the object model and database accordingly using Entity Framework Migrations</span></span>
- <span data-ttu-id="5c2bd-130">将部署到 Azure App Service 中使用 Git</span><span class="sxs-lookup"><span data-stu-id="5c2bd-130">Deploy to Azure App Service using Git</span></span>
- <span data-ttu-id="5c2bd-131">回滚到以前的部署使用 Azure 管理门户</span><span class="sxs-lookup"><span data-stu-id="5c2bd-131">Rollback to a previous deployment using the Azure Management portal</span></span>
- <span data-ttu-id="5c2bd-132">使用 Azure 存储空间来扩展 web 应用</span><span class="sxs-lookup"><span data-stu-id="5c2bd-132">Use Azure Storage to scale a web app</span></span>
- <span data-ttu-id="5c2bd-133">配置使用 Azure 管理门户的 web 应用程序的自动缩放</span><span class="sxs-lookup"><span data-stu-id="5c2bd-133">Configure auto-scaling for a web app using the Azure Management Portal</span></span>
- <span data-ttu-id="5c2bd-134">创建和配置 Visual Studio 中的负载测试项目</span><span class="sxs-lookup"><span data-stu-id="5c2bd-134">Create and configure a load test project in Visual Studio</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="5c2bd-135">系统必备</span><span class="sxs-lookup"><span data-stu-id="5c2bd-135">Prerequisites</span></span>

<span data-ttu-id="5c2bd-136">完成本动手实验需要以下：</span><span class="sxs-lookup"><span data-stu-id="5c2bd-136">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="5c2bd-137">[Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/)或更高版本</span><span class="sxs-lookup"><span data-stu-id="5c2bd-137">[Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/) or greater</span></span>
- [<span data-ttu-id="5c2bd-138">Azure SDK for .NET 2.2</span><span class="sxs-lookup"><span data-stu-id="5c2bd-138">Azure SDK for .NET 2.2</span></span>](https://www.microsoft.com/windowsazure/sdk/)
- [<span data-ttu-id="5c2bd-139">GIT 版本控制系统</span><span class="sxs-lookup"><span data-stu-id="5c2bd-139">GIT Version Control System</span></span>](http://git-scm.com/download)
- <span data-ttu-id="5c2bd-140">Microsoft Azure 订阅</span><span class="sxs-lookup"><span data-stu-id="5c2bd-140">A Microsoft Azure subscription</span></span> 

    - <span data-ttu-id="5c2bd-141">注册[免费试用版](http://aka.ms/watk-freetrial)</span><span class="sxs-lookup"><span data-stu-id="5c2bd-141">Sign up for a [Free Trial](http://aka.ms/watk-freetrial)</span></span>
    - <span data-ttu-id="5c2bd-142">如果你是 Visual Studio 专业版、 专业测试工具版、 高级版或旗舰版与 MSDN 或 MSDN 平台订阅服务器，激活你[MSDN 权益](http://aka.ms/watk-msdn)现在，若要开始开发和测试在 Azure 上</span><span class="sxs-lookup"><span data-stu-id="5c2bd-142">If you are a Visual Studio Professional, Test Professional, Premium or Ultimate with MSDN or MSDN Platforms subscriber, activate your [MSDN benefit](http://aka.ms/watk-msdn) now to start developing and testing on Azure</span></span>
    - <span data-ttu-id="5c2bd-143">[BizSpark](http://aka.ms/watk-bizspark)成员自动接收 Azure 通过其 Visual Studio Ultimate with MSDN 订阅权益</span><span class="sxs-lookup"><span data-stu-id="5c2bd-143">[BizSpark](http://aka.ms/watk-bizspark) members automatically receive the Azure benefit through their Visual Studio Ultimate with MSDN subscriptions</span></span>
    - <span data-ttu-id="5c2bd-144">成员[Microsoft 合作伙伴网络](http://aka.ms/watk-mpn)Cloud Essentials 程序中收到免费的月度 Azure 信用额度</span><span class="sxs-lookup"><span data-stu-id="5c2bd-144">Members of the [Microsoft Partner Network](http://aka.ms/watk-mpn) Cloud Essentials program receive monthly Azure credits at no charge</span></span>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="5c2bd-145">安装</span><span class="sxs-lookup"><span data-stu-id="5c2bd-145">Setup</span></span>

<span data-ttu-id="5c2bd-146">若要运行本动手实验中的练习，你将需要先设置你的环境。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-146">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="5c2bd-147">打开 Windows 资源管理器并浏览到本实验的**源**文件夹。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-147">Open Windows Explorer and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="5c2bd-148">右键单击**Setup.cmd**和选择**以管理员身份运行**以启动安装过程将配置您的环境并安装本实验的 Visual Studio 代码段。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-148">Right-click on **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="5c2bd-149">如果显示用户帐户控制对话框中，确认操作以继续。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-149">If the User Account Control dialog is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="5c2bd-150">请确保您已在运行安装程序之前检查本实验的所有依赖项。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-150">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="5c2bd-151">使用代码片段</span><span class="sxs-lookup"><span data-stu-id="5c2bd-151">Using the Code Snippets</span></span>

<span data-ttu-id="5c2bd-152">在整个实验文档中，你将指示要插入代码块。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-152">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="5c2bd-153">为方便起见，大多数的此代码提供作为 Visual Studio 代码段，你可以从访问在 Visual Studio 2013，为了避免必须手动添加它。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-153">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="5c2bd-154">每个练习均附带的开始解决方案位于**开始**本练习，您可以完成相互独立地每个练习的文件夹。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-154">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="5c2bd-155">请注意在练习过程中添加的代码段缺少这些开始解决方案中，并且可能不工作，直到完成练习。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-155">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="5c2bd-156">在练习的源代码，你将找到**结束**包含具有完成相应练习中的步骤得到的代码的 Visual Studio 解决方案文件夹。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-156">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="5c2bd-157">如果您在演练本动手实验需要其他帮助，你可以使用这些解决方案作为指南。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-157">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>


* * *

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="5c2bd-158">练习</span><span class="sxs-lookup"><span data-stu-id="5c2bd-158">Exercises</span></span>

<span data-ttu-id="5c2bd-159">本动手实验包括以下练习：</span><span class="sxs-lookup"><span data-stu-id="5c2bd-159">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="5c2bd-160">使用 Entity Framework 迁移</span><span class="sxs-lookup"><span data-stu-id="5c2bd-160">Using Entity Framework Migrations</span></span>](#Exercise1)
2. [<span data-ttu-id="5c2bd-161">将 Web 应用部署到过渡环境</span><span class="sxs-lookup"><span data-stu-id="5c2bd-161">Deploying a Web App to Staging</span></span>](#Exercise2)
3. [<span data-ttu-id="5c2bd-162">在生产环境中执行部署回滚</span><span class="sxs-lookup"><span data-stu-id="5c2bd-162">Performing Deployment Rollback in Production</span></span>](#Exercise3)
4. [<span data-ttu-id="5c2bd-163">使用 Azure 存储进行缩放</span><span class="sxs-lookup"><span data-stu-id="5c2bd-163">Scaling Using Azure Storage</span></span>](#Exercise4)
5. <span data-ttu-id="5c2bd-164">[使用 Web Apps 自动缩放](#Exercise5)（对于 Visual Studio 2013 Ultimate edition 可选）</span><span class="sxs-lookup"><span data-stu-id="5c2bd-164">[Using Autoscale for Web Apps](#Exercise5) (Optional for Visual Studio 2013 Ultimate edition)</span></span>

<span data-ttu-id="5c2bd-165">估计时间来完成该实验： **75 分钟**</span><span class="sxs-lookup"><span data-stu-id="5c2bd-165">Estimated time to complete this lab: **75 minutes**</span></span>

> [!NOTE]
> <span data-ttu-id="5c2bd-166">当你首次启动 Visual Studio 时，你必须选择一个预定义的设置集合。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-166">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="5c2bd-167">每个预定义的集合用于匹配特定的开发风格和确定窗口布局、 编辑器行为、 IntelliSense 代码段和对话框选项。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-167">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="5c2bd-168">在本实验中的过程描述完成给定的任务 Visual Studio 中使用时所需的操作**常规开发设置**集合。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-168">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="5c2bd-169">如果您为开发环境选择不同的设置集合，则表明可能存在你应考虑到帐户的步骤中的差异。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-169">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>


<a id="Exercise1"></a>
### <a name="exercise-1-using-entity-framework-migrations"></a><span data-ttu-id="5c2bd-170">练习 1： 使用 Entity Framework 迁移</span><span class="sxs-lookup"><span data-stu-id="5c2bd-170">Exercise 1: Using Entity Framework Migrations</span></span>

<span data-ttu-id="5c2bd-171">当正在开发的应用时，你的数据模型可能随时间变化。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-171">When you are developing an application, your data model might change over time.</span></span> <span data-ttu-id="5c2bd-172">（如果要创建新的版本），这些更改可能会影响你的数据库中的现有模型，而且，务必使数据库保持为最新，以防止出现错误。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-172">These changes could affect the existing model in your database (if you are creating a new version) and it is important to keep your database up-to-date to prevent errors.</span></span>

<span data-ttu-id="5c2bd-173">若要简化在模型中，这些更改的跟踪**Entity Framework Code First 迁移**自动检测更改将数据库架构与您模型进行比较，生成特定的代码以更新你的数据库，创建新*版本*的你的数据库。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-173">To simplify the tracking of these changes in your model, **Entity Framework Code First Migrations** automatically detect changes comparing your model with the database schema and generates specific code to update your database, creating new *versions* of your database.</span></span>

<span data-ttu-id="5c2bd-174">此练习展示如何启用**迁移**为你的应用程序以及如何可以轻松地检测和生成更改来更新你的数据库。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-174">This exercise shows you how to enable **Migrations** for your application and how you can easily detect and generate changes to update your databases.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1--enabling-migrations"></a><span data-ttu-id="5c2bd-175">任务 1 – 启用迁移</span><span class="sxs-lookup"><span data-stu-id="5c2bd-175">Task 1 – Enabling Migrations</span></span>

<span data-ttu-id="5c2bd-176">在此任务中，你将经历的步骤启用**Entity Framework Code First 迁移**到**专家 Quiz**数据库，更改模型和了解如何将这些更改反映在数据库。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-176">In this task, you will go through the steps of enabling **Entity Framework Code First Migrations** to the **Geek Quiz** database, changing the model and understanding how those changes are reflected in the database.</span></span>

1. <span data-ttu-id="5c2bd-177">打开 Visual Studio 并打开**GeekQuiz.sln**解决方案文件从**Source\Ex1 UsingEntityFrameworkMigrations\Begin**。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-177">Open Visual Studio and open the **GeekQuiz.sln** solution file from **Source\Ex1-UsingEntityFrameworkMigrations\Begin**.</span></span>
2. <span data-ttu-id="5c2bd-178">生成才能下载和安装解决方案**NuGet**包的依赖关系。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-178">Build the solution in order to download and install the **NuGet** package dependencies.</span></span> <span data-ttu-id="5c2bd-179">若要执行此操作，右键单击该解决方案，然后单击**生成解决方案**或按**Ctrl + Shift + B**。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-179">To do this, right-click the solution and click **Build Solution** or press **Ctrl + Shift + B**.</span></span>
3. <span data-ttu-id="5c2bd-180">从**工具**菜单在 Visual Studio 中，选择**库程序包管理器**，然后单击**程序包管理器控制台**。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-180">From the **Tools** menu in Visual Studio, select **Library Package Manager**, and then click **Package Manager Console**.</span></span>
4. <span data-ttu-id="5c2bd-181">在**程序包管理器控制台**，输入以下命令，然后按**Enter**。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-181">In the **Package Manager Console**, enter the following command and then press **Enter**.</span></span> <span data-ttu-id="5c2bd-182">将创建基于现有模型的初始迁移。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-182">An initial migration based on the existing model will be created.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample1.ps1)]

    <span data-ttu-id="5c2bd-183">![启用迁移](maintainable-azure-websites-managing-change-and-scale/_static/image1.png "启用迁移")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-183">![Enabling Migrations](maintainable-azure-websites-managing-change-and-scale/_static/image1.png "Enabling Migrations")</span></span>

    <span data-ttu-id="5c2bd-184">*启用迁移*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-184">*Enabling Migrations*</span></span>

    > [!NOTE]
    > <span data-ttu-id="5c2bd-185">此命令将添加**迁移**到奇 Quiz 项目包含文件的文件夹调用**Configuration.cs**。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-185">This command adds a **Migrations** folder to Geek Quiz project that contains a file called **Configuration.cs**.</span></span> <span data-ttu-id="5c2bd-186">**配置**类允许你配置迁移你上下文的行为方式。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-186">The **Configuration** class allows you to configure how Migrations behaves for your context.</span></span>
5. <span data-ttu-id="5c2bd-187">借助启用迁移，你需要更新**配置**类使用的初始数据填充数据库的**专家 Quiz**要求。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-187">With Migrations enabled, you need to update the **Configuration** class to populate the database with the initial data that **Geek Quiz** requires.</span></span> <span data-ttu-id="5c2bd-188">下**迁移**，替换**Configuration.cs**替换为文件位于**Source\Assets**这个实验室的文件夹。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-188">Under **Migrations**, replace the **Configuration.cs** file with the one located in the **Source\Assets** folder of this lab.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5c2bd-189">由于**迁移**将调用**种子**与数据库中的每个更新的方法，你需要以确保在数据库中的用户不能重复记录。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-189">Since **Migrations** will call the **Seed** method with every database update, you need to be sure that records are not duplicated in the database.</span></span> <span data-ttu-id="5c2bd-190">**AddOrUpdate**方法将有助于防止重复数据。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-190">The **AddOrUpdate** method will help to prevent duplicate data.</span></span>
6. <span data-ttu-id="5c2bd-191">若要添加的初始迁移，输入以下命令，然后按**Enter**。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-191">To add an initial migration, enter the following command and then press **Enter**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5c2bd-192">请确保没有名为数据库&quot;GeekQuizProd&quot; LocalDB 实例中。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-192">Make sure that there is no database named &quot;GeekQuizProd&quot; in your LocalDB instance.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample2.ps1)]

    <span data-ttu-id="5c2bd-193">![添加基础架构迁移](maintainable-azure-websites-managing-change-and-scale/_static/image2.png "添加基础架构迁移")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-193">![Adding base schema migration](maintainable-azure-websites-managing-change-and-scale/_static/image2.png "Adding base schema migration")</span></span>

    <span data-ttu-id="5c2bd-194">*添加基础架构迁移*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-194">*Adding base schema migration*</span></span>

    > [!NOTE]
    > <span data-ttu-id="5c2bd-195">**添加迁移**将创建基于自上次迁移创建以来对您的模型所做的更改的下一次迁移的基架。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-195">**Add-Migration** will scaffold the next migration based on changes you have made to your model since the last migration was created.</span></span> <span data-ttu-id="5c2bd-196">在这种情况下，由于它是项目的初始迁移时，它将添加用于创建在中定义的所有表的脚本**TriviaContext**类。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-196">In this case, as it is the first migration of the project, it will add the scripts to create all the tables defined in the **TriviaContext** class.</span></span>
7. <span data-ttu-id="5c2bd-197">执行迁移后，以通过运行以下命令更新数据库。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-197">Execute the migration to update the database by running the following command.</span></span> <span data-ttu-id="5c2bd-198">对于此命令将指定**详细**标志以查看应用到目标数据库的 SQL 语句。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-198">For this command you will specify the **Verbose** flag to view the SQL statements being applied to the target database.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample3.ps1)]

    <span data-ttu-id="5c2bd-199">![创建初始数据库](maintainable-azure-websites-managing-change-and-scale/_static/image3.png "创建初始数据库")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-199">![Creating initial database](maintainable-azure-websites-managing-change-and-scale/_static/image3.png "Creating initial database")</span></span>

    <span data-ttu-id="5c2bd-200">*创建初始数据库*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-200">*Creating initial database*</span></span>

    > [!NOTE]
    > <span data-ttu-id="5c2bd-201">**更新数据库**将应用所有挂起的迁移到数据库。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-201">**Update-Database** will apply any pending migrations to the database.</span></span> <span data-ttu-id="5c2bd-202">在这种情况下，它将创建使用 web.config 文件中定义的连接字符串的数据库。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-202">In this case, it will create the database using the connection string defined in your web.config file.</span></span>
8. <span data-ttu-id="5c2bd-203">转到**视图**菜单并打开**SQL Server 对象资源管理器**。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-203">Go to **View** menu and open **SQL Server Object Explorer**.</span></span>

    <span data-ttu-id="5c2bd-204">![在 SQL Server 对象资源管理器中打开](maintainable-azure-websites-managing-change-and-scale/_static/image4.png "在 SQL Server 对象资源管理器中打开")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-204">![Open in SQL Server Object Explorer](maintainable-azure-websites-managing-change-and-scale/_static/image4.png "Open in SQL Server Object Explorer")</span></span>

    <span data-ttu-id="5c2bd-205">*在 SQL Server 对象资源管理器中打开*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-205">*Open in SQL Server Object Explorer*</span></span>
9. <span data-ttu-id="5c2bd-206">在**SQL Server 对象资源管理器**窗口中，右键单击连接到 LocalDB 实例**SQL Server**节点并选择**添加 SQL Server...**选项。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-206">In the **SQL Server Object Explorer** window, connect to your LocalDB instance by right-clicking the **SQL Server** node and selecting **Add SQL Server...** option.</span></span>

    <span data-ttu-id="5c2bd-207">![添加 SQL Server 实例](maintainable-azure-websites-managing-change-and-scale/_static/image5.png "添加 SQL Server 实例")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-207">![Adding a SQL Server Instance](maintainable-azure-websites-managing-change-and-scale/_static/image5.png "Adding a SQL Server Instance")</span></span>

    <span data-ttu-id="5c2bd-208">*将 SQL Server 实例添加到 SQL Server 对象资源管理器*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-208">*Adding a SQL Server instance to SQL Server Object Explorer*</span></span>
10. <span data-ttu-id="5c2bd-209">设置**服务器名称**到*(localdb) \v11.0*并使**Windows 身份验证**作为身份验证模式。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-209">Set the **server name** to *(localdb)\v11.0* and leave **Windows Authentication** as your authentication mode.</span></span> <span data-ttu-id="5c2bd-210">单击**连接**以继续。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-210">Click **Connect** to continue.</span></span>

    <span data-ttu-id="5c2bd-211">![连接到 LocalDB](maintainable-azure-websites-managing-change-and-scale/_static/image6.png "连接到 LocalDB")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-211">![Connecting to LocalDB](maintainable-azure-websites-managing-change-and-scale/_static/image6.png "Connecting to LocalDB")</span></span>

    <span data-ttu-id="5c2bd-212">*连接到 LocalDB*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-212">*Connecting to LocalDB*</span></span>
11. <span data-ttu-id="5c2bd-213">打开**GeekQuizProd**数据库中，展开**表**节点。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-213">Open the **GeekQuizProd** database and expand the **Tables** node.</span></span> <span data-ttu-id="5c2bd-214">如你所见，**更新数据库**命令生成定义中的所有表**TriviaContext**类。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-214">As you can see, the **Update-Database** command generated all the tables defined in the **TriviaContext** class.</span></span> <span data-ttu-id="5c2bd-215">找到**dbo。TriviaQuestions**表，然后打开列节点。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-215">Locate the **dbo.TriviaQuestions** table and open the columns node.</span></span> <span data-ttu-id="5c2bd-216">在下一步的任务中，你将向此表中添加新列，并更新数据库使用**迁移**。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-216">In the next task, you will add a new column to this table and update the database using **Migrations**.</span></span>

    <span data-ttu-id="5c2bd-217">![琐事问题列](maintainable-azure-websites-managing-change-and-scale/_static/image7.png "琐事问题列")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-217">![Trivia Questions Columns](maintainable-azure-websites-managing-change-and-scale/_static/image7.png "Trivia Questions Columns")</span></span>

    <span data-ttu-id="5c2bd-218">*琐事问题列*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-218">*Trivia Questions Columns*</span></span>

<a id="Ex1Task2"></a>
#### <a name="task-2--updating-database-schema-using-migrations"></a><span data-ttu-id="5c2bd-219">任务 2 – 使用迁移更新数据库架构</span><span class="sxs-lookup"><span data-stu-id="5c2bd-219">Task 2 – Updating Database Schema Using Migrations</span></span>

<span data-ttu-id="5c2bd-220">在此任务中，你将使用**Entity Framework Code First 迁移**检测你的模型中的更改并生成必要的代码以更新数据库。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-220">In this task, you will use **Entity Framework Code First Migrations** to detect a change in your model and generate the necessary code to update the database.</span></span> <span data-ttu-id="5c2bd-221">你将更新**TriviaQuestions**通过向其添加一个新属性的实体。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-221">You will update the **TriviaQuestions** entity by adding a new property to it.</span></span> <span data-ttu-id="5c2bd-222">然后，将运行命令以创建新的迁移，以包括表中的新列。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-222">Then you will run commands to create a new Migration to include the new column in the table.</span></span>

1. <span data-ttu-id="5c2bd-223">在**解决方案资源管理器**，双击**TriviaQuestion.cs**文件位于内**模型**文件夹。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-223">In **Solution Explorer**, double-click the **TriviaQuestion.cs** file located inside the **Models** folder.</span></span>
2. <span data-ttu-id="5c2bd-224">添加名为的新属性**提示**，下面的代码段中所示。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-224">Add a new property named **Hint**, as shown in the following code snippet.</span></span>

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample4.cs)]
3. <span data-ttu-id="5c2bd-225">在**程序包管理器控制台**，输入以下命令，然后按**Enter**。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-225">In the **Package Manager Console**, enter the following command and then press **Enter**.</span></span> <span data-ttu-id="5c2bd-226">以反映在我们的模型更改，将创建一个新迁移。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-226">A new migration will be created reflecting the change in our model.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample5.ps1)]

    <span data-ttu-id="5c2bd-227">![Add-Migration](maintainable-azure-websites-managing-change-and-scale/_static/image8.png "Add-Migration")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-227">![Add-Migration](maintainable-azure-websites-managing-change-and-scale/_static/image8.png "Add-Migration")</span></span>

    <span data-ttu-id="5c2bd-228">*Add-Migration*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-228">*Add-Migration*</span></span>

    > [!NOTE]
    > <span data-ttu-id="5c2bd-229">迁移文件组成两种方法，**向上**和**下**。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-229">A Migration file is composed of two methods, **Up** and **Down**.</span></span>
    > 
    > - <span data-ttu-id="5c2bd-230">**向上**方法将用于指定哪些更改我们的应用程序需要将应用于数据库的当前版本。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-230">The **Up** method will be used to specify what changes the current version of our application need to apply to the database.</span></span>
    > - <span data-ttu-id="5c2bd-231">**下**用于反向我们已添加到的更改**向上**方法。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-231">The **Down** is used to reverse the changes we have added to the **Up** method.</span></span>
    > 
    > <span data-ttu-id="5c2bd-232">当数据库迁移更新数据库时，它将按时间戳顺序仅显示那些尚未使用自上次更新后运行的所有迁移 ( \_MigrationHistory 表将跟踪的哪些迁移已应用)。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-232">When the Database Migration updates the database, it will run all migrations in the timestamp order, and only those that have not been used since the last update (The \_MigrationHistory table keeps track of which migrations have been applied).</span></span> <span data-ttu-id="5c2bd-233">**向上**的所有迁移的方法将调用并将使我们指定到数据库的更改。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-233">The **Up** method of all migrations will be called and will make the changes we have specified to the database.</span></span> <span data-ttu-id="5c2bd-234">如果我们决定回到先前的迁移，**下**将调用方法，以重做以反向顺序中的更改。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-234">If we decide to go back to a previous migration, the **Down** method will be called to redo the changes in a reverse order.</span></span>
4. <span data-ttu-id="5c2bd-235">在**程序包管理器控制台**，输入以下命令，然后按**Enter**。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-235">In the **Package Manager Console**, enter the following command and then press **Enter**.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample6.ps1)]
5. <span data-ttu-id="5c2bd-236">输出**更新数据库**生成命令**Alter Table** SQL 语句添加到新列**TriviaQuestions**表下, 图中所示。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-236">The output of the **Update-Database** command generated an **Alter Table** SQL statement to add a new column to the **TriviaQuestions** table, as shown in the image below.</span></span>

    <span data-ttu-id="5c2bd-237">![添加列 SQL 语句生成](maintainable-azure-websites-managing-change-and-scale/_static/image9.png "添加列生成的 SQL 语句")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-237">![Add column SQL statement generated](maintainable-azure-websites-managing-change-and-scale/_static/image9.png "Add column SQL statement generated")</span></span>

    <span data-ttu-id="5c2bd-238">*添加列生成的 SQL 语句*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-238">*Add column SQL statement generated*</span></span>
6. <span data-ttu-id="5c2bd-239">在**SQL Server 对象资源管理器**，刷新**dbo。TriviaQuestions**表，以检查新**提示**显示列。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-239">In **SQL Server Object Explorer**, refresh the **dbo.TriviaQuestions** table and check that the new **Hint** column is displayed.</span></span>

    <span data-ttu-id="5c2bd-240">![显示新的提示列](maintainable-azure-websites-managing-change-and-scale/_static/image10.png "显示新的提示列")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-240">![Showing the new Hint Column](maintainable-azure-websites-managing-change-and-scale/_static/image10.png "Showing the new Hint Column")</span></span>

    <span data-ttu-id="5c2bd-241">*显示新的提示列*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-241">*Showing the new Hint Column*</span></span>
7. <span data-ttu-id="5c2bd-242">返回**TriviaQuestion.cs**编辑器中，添加**StringLength**约束*提示*属性，如下面的代码段中所示。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-242">Back in the **TriviaQuestion.cs** editor, add a **StringLength** constraint to the *Hint* property, as shown in the following code snippet.</span></span>

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample7.cs)]
8. <span data-ttu-id="5c2bd-243">在**程序包管理器控制台**，输入以下命令，然后按**Enter**。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-243">In the **Package Manager Console**, enter the following command and then press **Enter**.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample8.ps1)]
9. <span data-ttu-id="5c2bd-244">在**程序包管理器控制台**，输入以下命令，然后按**Enter**。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-244">In the **Package Manager Console**, enter the following command and then press **Enter**.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample9.ps1)]
10. <span data-ttu-id="5c2bd-245">输出**更新数据库**生成命令**Alter Table** SQL 语句，以更新*提示*列类型**TriviaQuestions**表下, 图中所示。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-245">The output of the **Update-Database** command generated an **Alter Table** SQL statement to update the *hint* column type of the **TriviaQuestions** table, as shown in the image below.</span></span>

    <span data-ttu-id="5c2bd-246">![Alter column SQL 语句生成](maintainable-azure-websites-managing-change-and-scale/_static/image11.png "Alter 列生成的 SQL 语句")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-246">![Alter column SQL statement generated](maintainable-azure-websites-managing-change-and-scale/_static/image11.png "Alter column SQL statement generated")</span></span>

    <span data-ttu-id="5c2bd-247">*更改列生成的 SQL 语句*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-247">*Alter column SQL statement generated*</span></span>
11. <span data-ttu-id="5c2bd-248">在**SQL Server 对象资源管理器**，刷新**dbo。TriviaQuestions**表，然后检查**提示**列类型是**nvarchar(150)**。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-248">In **SQL Server Object Explorer**, refresh the **dbo.TriviaQuestions** table and check that the **Hint** column type is **nvarchar(150)**.</span></span>

    <span data-ttu-id="5c2bd-249">![显示新约束](maintainable-azure-websites-managing-change-and-scale/_static/image12.png "显示新约束")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-249">![Showing the new constraint](maintainable-azure-websites-managing-change-and-scale/_static/image12.png "Showing the new constraint")</span></span>

    <span data-ttu-id="5c2bd-250">*显示新约束*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-250">*Showing the new constraint*</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-deploying-a-web-app-to-staging"></a><span data-ttu-id="5c2bd-251">练习 2： 将 Web 应用部署到过渡环境</span><span class="sxs-lookup"><span data-stu-id="5c2bd-251">Exercise 2: Deploying a Web App to Staging</span></span>

<span data-ttu-id="5c2bd-252">**Web 应用在 Azure App Service**使您可以执行过渡的发布。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-252">**Web Apps in Azure App Service** enables you to perform staged publishing.</span></span> <span data-ttu-id="5c2bd-253">过渡的发布创建为每个默认生产站点过渡站点槽，并使你能够交换这些槽位而无需停机时间。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-253">Staged publishing creates a staging site slot for each default production site and enables you to swap these slots with no down time.</span></span> <span data-ttu-id="5c2bd-254">这是真正发挥作用，在发布到公共之前验证更改，逐步集成站点内容，并回滚更改未按预期工作。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-254">This is really useful to validate changes before releasing to the public, incrementally integrate site content, and roll back if changes are not working as expected.</span></span>

<span data-ttu-id="5c2bd-255">在本练习中，你将部署**专家 Quiz**到你使用 Git 源代码管理的 web 应用的过渡环境的应用程序。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-255">In this exercise, you will deploy the **Geek Quiz** application to the staging environment of your web app using Git source control.</span></span> <span data-ttu-id="5c2bd-256">若要执行此操作，将创建 web 应用和设置在管理门户所需的组件，配置**Git**的源存储库和推送应用程序从本地计算机到过渡槽的代码。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-256">To do this, you will create the web app and provision the required components at the management portal, configure a **Git** repository and push the application source code from your local computer to the staging slot.</span></span> <span data-ttu-id="5c2bd-257">你还要更新与生产数据库**Code First 迁移**你在上一练习中创建。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-257">You will also update your production database with the **Code First Migrations** you created in the previous exercise.</span></span> <span data-ttu-id="5c2bd-258">然后，您将在此测试环境以验证其操作中执行应用程序。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-258">You will then execute the application in this test environment to verify its operation.</span></span> <span data-ttu-id="5c2bd-259">一旦您满意后它按预期工作，将提升到生产应用程序。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-259">Once you are satisfied that it is working according to your expectations, you will promote the application to production.</span></span>

> [!NOTE]
> <span data-ttu-id="5c2bd-260">若要启用暂存的发布，web 应用必须处于**标准模式**。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-260">To enable staged publishing, the web app must be in **Standard mode**.</span></span> <span data-ttu-id="5c2bd-261">请注意，是否将 web 应用程序更改为标准模式时将会产生额外费用。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-261">Note that additional charges will be incurred if you change your web app to Standard mode.</span></span> <span data-ttu-id="5c2bd-262">有关定价的详细信息，请参阅[App Service 定价](https://azure.microsoft.com/pricing/details/app-service/)。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-262">For more information about pricing, see [App Service Pricing](https://azure.microsoft.com/pricing/details/app-service/).</span></span>


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-web-app-in-azure-app-service"></a><span data-ttu-id="5c2bd-263">任务 1 – 在 Azure App Service 中创建 Web 应用</span><span class="sxs-lookup"><span data-stu-id="5c2bd-263">Task 1 – Creating a Web App in Azure App Service</span></span>

<span data-ttu-id="5c2bd-264">在此任务中，你将创建中的 web 应用**Azure App Service**从管理门户。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-264">In this task, you will create a web app in **Azure App Service** from the management portal.</span></span> <span data-ttu-id="5c2bd-265">你还将配置**SQL 数据库**以持久保存应用程序数据，并配置源代码管理的本地 Git 存储库。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-265">You will also configure a **SQL Database** to persist the application data, and configure a local Git repository for source control.</span></span>

1. <span data-ttu-id="5c2bd-266">转到[Azure 管理门户](https://manage.windowsazure.com)并使用与你的订阅关联的 Microsoft 帐户登录。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-266">Go to the [Azure management portal](https://manage.windowsazure.com) and sign in using the Microsoft account associated with your subscription.</span></span>

    ![登录到 Azure 管理门户](maintainable-azure-websites-managing-change-and-scale/_static/image13.png)

    <span data-ttu-id="5c2bd-268">*登录到 Azure 管理门户*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-268">*Sign in to the Azure management portal*</span></span>
2. <span data-ttu-id="5c2bd-269">单击**新建**在页面底部的命令栏中。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-269">Click **New** in the command bar at the bottom of the page.</span></span>

    <span data-ttu-id="5c2bd-270">![创建新的 web 应用](maintainable-azure-websites-managing-change-and-scale/_static/image14.png "创建新的 web 应用程序")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-270">![Creating a new web app](maintainable-azure-websites-managing-change-and-scale/_static/image14.png "Creating a new web app")</span></span>

    <span data-ttu-id="5c2bd-271">*创建新的 web 应用程序*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-271">*Creating a new web app*</span></span>
3. <span data-ttu-id="5c2bd-272">单击**计算**，**网站**然后**自定义创建**。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-272">Click **Compute**, **Website** and then **Custom Create**.</span></span>

    <span data-ttu-id="5c2bd-273">![创建新的 web 应用程序使用自定义创建](maintainable-azure-websites-managing-change-and-scale/_static/image15.png "创建使用自定义创建新的 web 应用程序")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-273">![Creating a new web app using Custom Create](maintainable-azure-websites-managing-change-and-scale/_static/image15.png "Creating a new web app using Custom Create")</span></span>

    <span data-ttu-id="5c2bd-274">*创建使用自定义创建新的 web 应用程序*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-274">*Creating a new web app using Custom Create*</span></span>
4. <span data-ttu-id="5c2bd-275">在**新建网站-自定义创建**对话框框中，提供可用**URL** (例如*专家测验*) 中, 选择一个位置**区域**下拉列表，然后选择**创建新的 SQL 数据库**中**数据库**下拉列表。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-275">In the **New Website - Custom Create** dialog box, provide an available **URL** (e.g. *geek-quiz*), select a location in the **Region** drop-down list, and select **Create a new SQL database** in the **Database** drop-down list.</span></span> <span data-ttu-id="5c2bd-276">最后，选择**--从源代码管理发布**复选框，然后单击**下一步**。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-276">Finally, select the **Publish from source control** check box and click **Next**.</span></span>

    ![自定义新的 web 应用](maintainable-azure-websites-managing-change-and-scale/_static/image16.png)

    <span data-ttu-id="5c2bd-278">*自定义新的 web 应用*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-278">*Customizing the new web app*</span></span>
5. <span data-ttu-id="5c2bd-279">指定数据库设置的以下信息：</span><span class="sxs-lookup"><span data-stu-id="5c2bd-279">Specify the following information for the database settings:</span></span>

    - <span data-ttu-id="5c2bd-280">在**名称**文本框中，输入数据库名称 (例如*geekquiz\_db*)</span><span class="sxs-lookup"><span data-stu-id="5c2bd-280">In the **Name** text box, enter a database name (e.g. *geekquiz\_db*)</span></span>
    - <span data-ttu-id="5c2bd-281">在服务器中**下拉**列表中，选择**新建 SQL 数据库服务器**。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-281">In the Server **drop-down** list, select **New SQL database server**.</span></span> <span data-ttu-id="5c2bd-282">或者，你可以选择现有的服务器。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-282">Alternatively, you can select an existing server.</span></span>
    - <span data-ttu-id="5c2bd-283">在**数据库用户名**和**数据库密码**框中，为 SQL 数据库服务器输入管理员用户名和密码。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-283">In the **Database username** and **Database password** boxes, enter the administrator username and password for the SQL database server.</span></span> <span data-ttu-id="5c2bd-284">如果你选择一个服务器已创建，系统将提示你输入密码。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-284">If you select a server you have already created, you will be prompted for the password.</span></span>

    ![指定数据库设置](maintainable-azure-websites-managing-change-and-scale/_static/image17.png)

    <span data-ttu-id="5c2bd-286">*指定数据库设置*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-286">*Specifying the database settings*</span></span>
6. <span data-ttu-id="5c2bd-287">单击 **“下一步”** 以继续。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-287">Click **Next** to continue.</span></span>
7. <span data-ttu-id="5c2bd-288">选择**本地 Git 存储库**进行源代码管理，以使用单击**下一步**。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-288">Select **Local Git repository** for the source control to use and click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5c2bd-289">可能会提示你输入部署凭据 （用户名和密码）。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-289">You may be prompted for the deployment credentials (a username and password).</span></span>

    ![创建 Git 存储库](maintainable-azure-websites-managing-change-and-scale/_static/image18.png)

    <span data-ttu-id="5c2bd-291">*创建 Git 存储库*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-291">*Creating the Git repository*</span></span>
8. <span data-ttu-id="5c2bd-292">等待，直到创建新的 web 应用。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-292">Wait until the new web app is created.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5c2bd-293">默认情况下，Azure 提供在域*azurewebsites.net*但还为你提供将使用 Azure 管理门户的自定义域的可能性。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-293">By default, Azure provides domains at *azurewebsites.net* but also gives you the possibility to set custom domains using the Azure management portal.</span></span> <span data-ttu-id="5c2bd-294">但是，如果你使用某些 Azure App Service 模式下，仅可以管理自定义域。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-294">However, you can only manage custom domains if you are using certain Azure App Service modes.</span></span>
    > 
    > <span data-ttu-id="5c2bd-295">Azure App Service 是在免费、 共享、 基本、 标准和高级版中可用。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-295">Azure App Service is available in Free, Shared, Basic, Standard, and Premium editions.</span></span> <span data-ttu-id="5c2bd-296">在免费和共享模式下的所有 web 应用在多租户环境中运行，并且都 CPU、 内存和网络使用量配额。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-296">In Free and Shared mode, all web apps run in a multi-tenant environment and have quotas for CPU, Memory, and Network usage.</span></span> <span data-ttu-id="5c2bd-297">免费应用程序的最大数可能因您的计划。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-297">The maximum number of free apps may vary with your plan.</span></span> <span data-ttu-id="5c2bd-298">在标准模式下，你可以选择到标准 Azure 对应的专用虚拟机上运行的应用的计算资源。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-298">In Standard mode, you choose which apps run on dedicated virtual machines that correspond to the standard Azure compute resources.</span></span> <span data-ttu-id="5c2bd-299">你可以查找中的 web 应用模式配置**缩放**的 web 应用程序的菜单。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-299">You can find the web app mode configuration in the **Scale** menu of your web app.</span></span>
    > 
    > <span data-ttu-id="5c2bd-300">![Azure App Service 模式](maintainable-azure-websites-managing-change-and-scale/_static/image19.png "Azure App Service 模式")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-300">![Azure App Service Modes](maintainable-azure-websites-managing-change-and-scale/_static/image19.png "Azure App Service Modes")</span></span>
    > 
    > <span data-ttu-id="5c2bd-301">如果你使用**共享**或**标准**模式下，你将能够通过转到您的应用程序管理有关你的 web 应用的自定义域**配置**菜单，然后单击**管理域**下*域名*。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-301">If you are using **Shared** or **Standard** mode, you will be able to manage custom domains for your web app by going to your app's **Configure** menu and clicking **Manage Domains** under *domain names*.</span></span>
    > 
    > <span data-ttu-id="5c2bd-302">![管理域](maintainable-azure-websites-managing-change-and-scale/_static/image20.png "管理域")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-302">![Manage Domains](maintainable-azure-websites-managing-change-and-scale/_static/image20.png "Manage Domains")</span></span>
    > 
    > <span data-ttu-id="5c2bd-303">![管理自定义域](maintainable-azure-websites-managing-change-and-scale/_static/image21.png "管理自定义域")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-303">![Manage Custom Domains](maintainable-azure-websites-managing-change-and-scale/_static/image21.png "Manage Custom Domains")</span></span>
9. <span data-ttu-id="5c2bd-304">创建 web 应用后，单击下的链接**URL**列来检查新的 web 应用正在运行。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-304">Once the web app is created, click the link under the **URL** column to check that the new web app is running.</span></span>

    ![浏览到新的 web 应用](maintainable-azure-websites-managing-change-and-scale/_static/image22.png)

    <span data-ttu-id="5c2bd-306">*浏览到新的 web 应用*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-306">*Browsing to the new web app*</span></span>

    ![运行的 web 应用程序](maintainable-azure-websites-managing-change-and-scale/_static/image23.png)

    <span data-ttu-id="5c2bd-308">*运行的 web 应用程序*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-308">*web app running*</span></span>

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-production-sql-database"></a><span data-ttu-id="5c2bd-309">任务 2 – 创建生产 SQL 数据库</span><span class="sxs-lookup"><span data-stu-id="5c2bd-309">Task 2 – Creating the Production SQL Database</span></span>

<span data-ttu-id="5c2bd-310">在此任务中，你将使用**Entity Framework Code First 迁移**若要创建的数据库目标**Azure SQL 数据库**你在上一任务中创建的实例。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-310">In this task, you will use the **Entity Framework Code First Migrations** to create the database targeting the **Azure SQL Database** instance you created in the previous task.</span></span>

1. <span data-ttu-id="5c2bd-311">在管理门户中，导航到你在上一任务中创建的 web 应用，并转到其**仪表板**。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-311">In the Management Portal, navigate to the web app you created in the previous task and go to its **Dashboard**.</span></span>
2. <span data-ttu-id="5c2bd-312">在**仪表板**页上，单击**查看连接字符串**链接下**速览**部分。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-312">In the **Dashboard** page, click **View connection strings** link under the **quick glance** section.</span></span>

    <span data-ttu-id="5c2bd-313">![查看连接字符串](maintainable-azure-websites-managing-change-and-scale/_static/image24.png "查看连接字符串")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-313">![View connection strings](maintainable-azure-websites-managing-change-and-scale/_static/image24.png "View connection strings")</span></span>

    <span data-ttu-id="5c2bd-314">*查看连接字符串*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-314">*View connection strings*</span></span>
3. <span data-ttu-id="5c2bd-315">复制**连接字符串**值并关闭对话框。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-315">Copy the **connection string** value and close the dialog box.</span></span>

    <span data-ttu-id="5c2bd-316">![在 Azure 管理门户中的连接字符串](maintainable-azure-websites-managing-change-and-scale/_static/image25.png "在 Azure 管理门户中的连接字符串")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-316">![Connection String in Azure Management Portal](maintainable-azure-websites-managing-change-and-scale/_static/image25.png "Connection String in Azure Management Portal")</span></span>

    <span data-ttu-id="5c2bd-317">*在 Azure 管理门户中的连接字符串*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-317">*Connection String in Azure Management Portal*</span></span>
4. <span data-ttu-id="5c2bd-318">单击**SQL 数据库**若要查看 Azure 中的 SQL 数据库的列表</span><span class="sxs-lookup"><span data-stu-id="5c2bd-318">Click **SQL Databases** to see the list of SQL databases in Azure</span></span>

    <span data-ttu-id="5c2bd-319">![SQL 数据库菜单](maintainable-azure-websites-managing-change-and-scale/_static/image26.png "SQL 数据库菜单")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-319">![SQL Database menu](maintainable-azure-websites-managing-change-and-scale/_static/image26.png "SQL Database menu")</span></span>

    <span data-ttu-id="5c2bd-320">*SQL 数据库菜单*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-320">*SQL Database menu*</span></span>
5. <span data-ttu-id="5c2bd-321">找到你在上一任务中创建的数据库并单击该服务器。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-321">Locate the database you created in the previous task and click on the Server.</span></span>

    <span data-ttu-id="5c2bd-322">![SQL 数据库服务器](maintainable-azure-websites-managing-change-and-scale/_static/image27.png "SQL 数据库服务器")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-322">![SQL Database server](maintainable-azure-websites-managing-change-and-scale/_static/image27.png "SQL Database server")</span></span>

    <span data-ttu-id="5c2bd-323">*SQL 数据库服务器*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-323">*SQL Database server*</span></span>
6. <span data-ttu-id="5c2bd-324">在**快速启动**页的服务器上，单击**配置**。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-324">In the **Quick Start** page of the server, click on **Configure**.</span></span>

    <span data-ttu-id="5c2bd-325">![配置菜单](maintainable-azure-websites-managing-change-and-scale/_static/image28.png "配置菜单")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-325">![Configure menu](maintainable-azure-websites-managing-change-and-scale/_static/image28.png "Configure menu")</span></span>

    <span data-ttu-id="5c2bd-326">*配置菜单*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-326">*Configure menu*</span></span>
7. <span data-ttu-id="5c2bd-327">在**允许的 IP 地址**部分中，单击**将添加到允许的 IP 地址**链接以启用你的 IP 连接到 SQL 数据库服务器。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-327">In the **Allowed IP addresses** section, click on **Add to the allowed IP addresses** link to enable your IP to connect to the SQL Database server.</span></span>

    <span data-ttu-id="5c2bd-328">![允许的 IP 地址](maintainable-azure-websites-managing-change-and-scale/_static/image29.png "允许的 IP 地址")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-328">![Allowed IP addresses](maintainable-azure-websites-managing-change-and-scale/_static/image29.png "Allowed IP addresses")</span></span>

    <span data-ttu-id="5c2bd-329">*允许的 IP 地址*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-329">*Allowed IP addresses*</span></span>
8. <span data-ttu-id="5c2bd-330">单击**保存**要完成该步骤的页的底部。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-330">Click **Save** at the bottom of the page to complete the step.</span></span>
9. <span data-ttu-id="5c2bd-331">切换回 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-331">Switch back to Visual Studio.</span></span>
10. <span data-ttu-id="5c2bd-332">在**程序包管理器控制台**，执行以下命令将*[您的连接字符串]*占位符替换为从 Azure 复制的连接字符串</span><span class="sxs-lookup"><span data-stu-id="5c2bd-332">In the **Package Manager Console**, execute the following command replacing *[YOUR-CONNECTION-STRING]* placeholder with the connection string you copied from Azure</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample10.ps1)]

    <span data-ttu-id="5c2bd-333">![面向 Windows Azure SQL 数据库更新数据库](maintainable-azure-websites-managing-change-and-scale/_static/image30.png "面向 Windows Azure SQL 数据库更新数据库")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-333">![Update database targeting Windows Azure SQL Database](maintainable-azure-websites-managing-change-and-scale/_static/image30.png "Update database targeting Windows Azure SQL Database")</span></span>

    <span data-ttu-id="5c2bd-334">*更新目标 Azure SQL 数据库的数据库*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-334">*Update database targeting Azure SQL Database*</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3--deploying-geek-quiz-to-staging-using-git"></a><span data-ttu-id="5c2bd-335">任务 3 – 部署到过渡环境使用 Git 的专家测验</span><span class="sxs-lookup"><span data-stu-id="5c2bd-335">Task 3 – Deploying Geek Quiz to Staging Using Git</span></span>

<span data-ttu-id="5c2bd-336">在此任务中，你将 web 应用中启用过渡的发布。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-336">In this task, you will enable staged publishing in your web app.</span></span> <span data-ttu-id="5c2bd-337">然后，将使用 Git 发布专家 Quiz 应用程序直接从本地计算机到你的 web 应用的过渡环境。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-337">Then, you will use Git to publish the Geek Quiz application directly from your local computer to the staging environment of your web app.</span></span>

1. <span data-ttu-id="5c2bd-338">返回到门户并单击下的 web 应用的名称**名称**列以显示的管理页。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-338">Go back to the portal and click the name of the web app under the **Name** column to display the management pages.</span></span>

    ![打开 web 应用程序管理页面](maintainable-azure-websites-managing-change-and-scale/_static/image31.png)

    <span data-ttu-id="5c2bd-340">*打开 web 应用程序管理页面*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-340">*Opening the web app management pages*</span></span>
2. <span data-ttu-id="5c2bd-341">导航到**缩放**页。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-341">Navigate to the **Scale** page.</span></span> <span data-ttu-id="5c2bd-342">下**常规**部分中，选择**标准**配置和单击**保存**命令栏中。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-342">Under the **general** section, select **Standard** for the configuration and click **Save** in the command bar.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5c2bd-343">若要在当前区域和订阅中的运行的所有 web 应用**标准**模式下，保留**选择所有**复选框中选择**选择站点**配置。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-343">To run all web apps in the current region and subscription in **Standard** mode, leave the **Select All** check box selected in the **Choose Sites** configuration.</span></span> <span data-ttu-id="5c2bd-344">否则，请清除**选择所有**复选框。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-344">Otherwise, clear the **Select All** check box.</span></span>

    <span data-ttu-id="5c2bd-345">![将 web 应用程序升级到标准模式](maintainable-azure-websites-managing-change-and-scale/_static/image32.png "将 web 应用程序升级到标准模式")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-345">![Upgrading the web app to Standard mode](maintainable-azure-websites-managing-change-and-scale/_static/image32.png "Upgrading the web app to Standard mode")</span></span>

    <span data-ttu-id="5c2bd-346">*将 Web 应用程序升级到标准模式*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-346">*Upgrading the Web App to Standard mode*</span></span>
3. <span data-ttu-id="5c2bd-347">单击**是**以确认所做的更改。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-347">Click **Yes** to confirm the changes.</span></span>

    <span data-ttu-id="5c2bd-348">![确认更改为标准模式](maintainable-azure-websites-managing-change-and-scale/_static/image33.png "继续执行更改的 web 应用程序模式")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-348">![Confirming the change to Standard mode](maintainable-azure-websites-managing-change-and-scale/_static/image33.png "Continuing with the changing of the web app mode")</span></span>

    <span data-ttu-id="5c2bd-349">*确认更改为标准模式*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-349">*Confirming the change to Standard mode*</span></span>
4. <span data-ttu-id="5c2bd-350">转到**仪表板**页上，单击**启用暂存发布**下**速览**部分。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-350">Go to the **Dashboard** page and click **Enable staged publishing** under the **quick glance** section.</span></span>

    <span data-ttu-id="5c2bd-351">![启用过渡的发布](maintainable-azure-websites-managing-change-and-scale/_static/image34.png "启用暂存发布")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-351">![Enabling staged publishing](maintainable-azure-websites-managing-change-and-scale/_static/image34.png "Enabling staged publishing")</span></span>

    <span data-ttu-id="5c2bd-352">*启用过渡的发布*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-352">*Enabling staged publishing*</span></span>
5. <span data-ttu-id="5c2bd-353">单击**是**才能启用过渡的发布。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-353">Click **Yes** to enable staged publishing.</span></span>

    <span data-ttu-id="5c2bd-354">![确认暂存发布](maintainable-azure-websites-managing-change-and-scale/_static/image35.png "单击是以启用暂存的发布")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-354">![Confirming staged publishing](maintainable-azure-websites-managing-change-and-scale/_static/image35.png "Clicking Yes to enable staged publishing")</span></span>

    <span data-ttu-id="5c2bd-355">*确认过渡的发布*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-355">*Confirming staged publishing*</span></span>
6. <span data-ttu-id="5c2bd-356">在 web apps 列表中，展开您的 web 应用名称以显示过渡站点槽左侧的标记。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-356">In the list of web apps, expand the mark to the left of your web app name to display the staging site slot.</span></span> <span data-ttu-id="5c2bd-357">它具有名称的 web 应用跟***（过渡）***。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-357">It has the name of your web app followed by ***(staging)***.</span></span> <span data-ttu-id="5c2bd-358">单击要转到管理页的过渡站点。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-358">Click the staging site to go to the management page.</span></span>

    <span data-ttu-id="5c2bd-359">![导航到过渡 web 应用](maintainable-azure-websites-managing-change-and-scale/_static/image36.png "导航到过渡 web 应用")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-359">![Navigating to the staging web app](maintainable-azure-websites-managing-change-and-scale/_static/image36.png "Navigating to the staging web app")</span></span>

    <span data-ttu-id="5c2bd-360">*导航到过渡应用*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-360">*Navigating to the staging app*</span></span>
7. <span data-ttu-id="5c2bd-361">请注意该他管理页与下面类似任何其他 web 应用程序的管理页。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-361">Notice that he management page looks like any other web app's management page.</span></span> <span data-ttu-id="5c2bd-362">导航到**部署**页和复制**Git URL**值。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-362">Navigate to the **Deployments** page and copy the **Git URL** value.</span></span> <span data-ttu-id="5c2bd-363">你将稍后在本练习中使用它。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-363">You will use it later in this exercise.</span></span>

    ![复制 Git URL 值](maintainable-azure-websites-managing-change-and-scale/_static/image37.png)

    <span data-ttu-id="5c2bd-365">*复制 Git URL 值*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-365">*Copying the Git URL value*</span></span>
8. <span data-ttu-id="5c2bd-366">打开一个新**Git Bash**控制台，然后执行以下命令。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-366">Open a new **Git Bash** console and execute the following commands.</span></span> <span data-ttu-id="5c2bd-367">更新*[您的应用程序的路径]*占位符替换为的路径**GeekQuiz**解决方案，位于**Source\Ex1 DeployingWebSiteToStaging\Begin**文件夹此实验室中。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-367">Update the *[YOUR-APPLICATION-PATH]* placeholder with the path to the **GeekQuiz** solution, located in the **Source\Ex1-DeployingWebSiteToStaging\Begin** folder of this lab.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample11.cmd)]

    ![Git 初始化和第一个提交](maintainable-azure-websites-managing-change-and-scale/_static/image38.png)

    <span data-ttu-id="5c2bd-369">*Git 初始化和第一个提交*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-369">*Git initialization and first commit*</span></span>
9. <span data-ttu-id="5c2bd-370">运行以下命令以将你的 web 应用推送到远程**Git**存储库。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-370">Run the following command to push your web app to the remote **Git** repository.</span></span> <span data-ttu-id="5c2bd-371">将占位符替换为你从管理门户获得的 URL。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-371">Replace the placeholder with the URL you obtained from the management portal.</span></span> <span data-ttu-id="5c2bd-372">系统将提示您输入部署密码。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-372">You will be prompted for your deployment password.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample12.cmd)]

    ![将推送到 Windows Azure](maintainable-azure-websites-managing-change-and-scale/_static/image39.png)

    <span data-ttu-id="5c2bd-374">*将推送到 Azure*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-374">*Pushing to Azure*</span></span>

    > [!NOTE]
    > <span data-ttu-id="5c2bd-375">将内容部署到的 FTP 主机或 GIT 存储库的 web 应用，你必须进行身份验证使用**部署凭据**从 web 应用创建**快速启动**或**仪表板**管理页。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-375">When you deploy content to the FTP host or GIT repository of a web app, you must authenticate using the **deployment credentials** that you created from the web app's **Quick Start** or **Dashboard** management pages.</span></span> <span data-ttu-id="5c2bd-376">如果你不知道你的部署凭据你可以轻松地重置它们使用管理门户。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-376">If you do not know your deployment credentials you can easily reset them using the management portal.</span></span> <span data-ttu-id="5c2bd-377">打开 web 应用**仪表板**页上，单击**重置部署凭据**链接。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-377">Open the web app **Dashboard** page and click the **Reset your deployment credentials** link.</span></span> <span data-ttu-id="5c2bd-378">提供新密码并单击**确定**。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-378">Provide a new password and click **OK**.</span></span> <span data-ttu-id="5c2bd-379">部署凭据可用于使用与你的订阅相关联的所有 web 应用。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-379">Deployment credentials are valid for use with all web apps associated with your subscription.</span></span>
10. <span data-ttu-id="5c2bd-380">为了验证 web 应用已成功推送到 Azure，请返回到管理门户并单击**网站**。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-380">In order to verify the web app was successfully pushed to Azure, go back to the management portal and click **Websites**.</span></span>
11. <span data-ttu-id="5c2bd-381">找到你的 web 应用并展开以显示过渡站点槽的条目。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-381">Locate your web app and expand the entry to display the staging site slot.</span></span> <span data-ttu-id="5c2bd-382">单击其**名称**转到管理页。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-382">Click its **Name** to go to the management page.</span></span>
12. <span data-ttu-id="5c2bd-383">单击**部署**若要查看**部署历史记录**。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-383">Click **Deployments** to see the **deployment history**.</span></span> <span data-ttu-id="5c2bd-384">验证是否有**活动部署**与你*&quot;初始提交&quot;*。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-384">Verify that there is an **Active Deployment** with your *&quot;Initial Commit&quot;*.</span></span>

    ![活动的部署](maintainable-azure-websites-managing-change-and-scale/_static/image40.png)

    <span data-ttu-id="5c2bd-386">*活动的部署*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-386">*Active deployment*</span></span>
13. <span data-ttu-id="5c2bd-387">最后，单击**浏览**以转到 web 应用程序的命令栏中。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-387">Finally, click **Browse** in the command bar to go to the web app.</span></span>

    ![浏览 web 应用](maintainable-azure-websites-managing-change-and-scale/_static/image41.png)

    <span data-ttu-id="5c2bd-389">*浏览 web 应用*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-389">*Browse web app*</span></span>
14. <span data-ttu-id="5c2bd-390">如果已成功部署的应用，你将看到专家 Quiz 登录页。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-390">If the application is successfully deployed, you will see the Geek Quiz login page.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5c2bd-391">部署的应用程序的地址 URL 包含名称的 web 应用跟*-暂存*。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-391">The address URL of the deployed application contains the name of your web app followed by *-staging*.</span></span>

    ![在过渡环境中运行的应用程序](maintainable-azure-websites-managing-change-and-scale/_static/image42.png)

    <span data-ttu-id="5c2bd-393">*在过渡环境中运行的应用程序*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-393">*Application running in the staging environment*</span></span>
15. <span data-ttu-id="5c2bd-394">如果你想要使用此应用程序，请单击**注册**注册新的用户。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-394">If you wish to explore the application, click **Register** to register a new user.</span></span> <span data-ttu-id="5c2bd-395">通过输入用户名、 电子邮件地址和密码完成帐户详细信息。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-395">Complete the account details by entering a user name, email address and password.</span></span> <span data-ttu-id="5c2bd-396">接下来，应用程序显示测验的第一个问题。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-396">Next, the application shows the first question of the quiz.</span></span> <span data-ttu-id="5c2bd-397">回答几个问题以确保它按预期工作。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-397">Answer a few questions to make sure it is working as expected.</span></span>

    ![应用程序可供使用](maintainable-azure-websites-managing-change-and-scale/_static/image43.png)

    <span data-ttu-id="5c2bd-399">*应用程序可供使用*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-399">*Application ready to be used*</span></span>

<a id="Ex2Task4"></a>
#### <a name="task-4--promoting-the-web-app-to-production"></a><span data-ttu-id="5c2bd-400">任务 4 – 将提升到生产 Web 应用</span><span class="sxs-lookup"><span data-stu-id="5c2bd-400">Task 4 – Promoting the Web App to Production</span></span>

<span data-ttu-id="5c2bd-401">现在，你已验证 web 应用在过渡环境中正常运行，你就可以将其升级到生产环境。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-401">Now that you have verified that the web app is working correctly in the staging environment, you are ready to promote it to production.</span></span> <span data-ttu-id="5c2bd-402">在此任务中，将交换过渡站点槽与生产站点槽。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-402">In this task, you will swap the staging site slot with the production site slot.</span></span>

1. <span data-ttu-id="5c2bd-403">返回到管理门户并选择过渡站点槽。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-403">Go back to the management portal and select the staging site slot.</span></span> <span data-ttu-id="5c2bd-404">单击**交换**命令栏中。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-404">Click **Swap** in the command bar.</span></span>

    ![切换到生产环境](maintainable-azure-websites-managing-change-and-scale/_static/image44.png)

    <span data-ttu-id="5c2bd-406">*切换到生产环境*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-406">*Swap to production*</span></span>
2. <span data-ttu-id="5c2bd-407">单击**是**在确认对话框中，以继续执行交换操作。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-407">Click **Yes** in the confirmation dialog box to proceed with the swap operation.</span></span> <span data-ttu-id="5c2bd-408">Azure 将立即交换生产站点的暂存站点内容的内容。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-408">Azure will immediately swap the content of the production site with the content of the staging site.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5c2bd-409">某些设置从的过渡版本将自动复制对生产版本 （例如连接字符串替代、 处理程序映射等），但其他设置将不会更改 （例如 DNS 终结点、 SSL 绑定等）。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-409">Some settings from the staged version will automatically be copied to the production version (e.g. connection string overrides, handler mappings, etc.) but other settings will not change (e.g. DNS endpoints, SSL bindings, etc.).</span></span>

    ![确认交换操作](maintainable-azure-websites-managing-change-and-scale/_static/image45.png)

    <span data-ttu-id="5c2bd-411">*确认交换操作*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-411">*Confirming swap operation*</span></span>
3. <span data-ttu-id="5c2bd-412">交换完成后，请选择生产槽，然后单击**浏览**命令栏中打开生产站点中。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-412">Once the swap is complete, select the production slot and click **Browse** in the command bar to open the production site.</span></span> <span data-ttu-id="5c2bd-413">请注意地址栏中的 URL。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-413">Notice the URL in the address bar.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5c2bd-414">你可能需要刷新浏览器以清除缓存。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-414">You might need to refresh your browser to clear cache.</span></span> <span data-ttu-id="5c2bd-415">在 Internet Explorer 中，你可以执行此操作通过按**CTRL + R**。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-415">In Internet Explorer, you can do this by pressing **CTRL+R**.</span></span>

    ![在生产环境中运行的 web 应用](maintainable-azure-websites-managing-change-and-scale/_static/image46.png)
4. <span data-ttu-id="5c2bd-417">在**GitBash**控制台中，更新本地 Git 存储库的远程 URL 以面向生产槽。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-417">In the **GitBash** console, update the remote URL for the local Git repository to target the production slot.</span></span> <span data-ttu-id="5c2bd-418">若要执行此操作，请运行以下命令将占位符替换为你部署的用户名和你的 web 应用的名称。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-418">To do this, run the following command replacing the placeholders with your deployment username and the name of your web app.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5c2bd-419">在以下练习中，会将更改推送到生产站点，而不是只是为了实验室的简单性过渡。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-419">In the following exercises, you will push changes to the production site instead of staging just for the simplicity of the lab.</span></span> <span data-ttu-id="5c2bd-420">在实际方案中，建议在升级到生产环境之前，验证在过渡环境中的更改。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-420">In a real-world scenario, it is recommended to verify the changes in the staging environment before promoting to production.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample13.cmd)]

<a id="Exercise3"></a>
### <a name="exercise-3-performing-deployment-rollback-in-production"></a><span data-ttu-id="5c2bd-421">练习 3： 在生产环境中执行部署回滚</span><span class="sxs-lookup"><span data-stu-id="5c2bd-421">Exercise 3: Performing Deployment Rollback in Production</span></span>

<span data-ttu-id="5c2bd-422">其中你没有过渡槽以热之间执行交换过渡和生产，例如，如果你正在使用的情况下**免费**或**共享**模式。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-422">There are scenarios where you do not have a staging slot to perform hot swap between staging and production, for example, if you are working with **Free** or **Shared** mode.</span></span> <span data-ttu-id="5c2bd-423">在这些情况下，你应测试应用程序在测试环境中的 – 本地或远程站点中 – 部署到生产环境之前。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-423">In those scenarios, you should test your application in a testing environment –either locally or in a remote site– before deploying to production.</span></span> <span data-ttu-id="5c2bd-424">但是，很可能在生产站点中可能出现在测试阶段未检测到的问题。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-424">However, it is possible that an issue not detected during the testing phase may arise in the production site.</span></span> <span data-ttu-id="5c2bd-425">在这种情况下，务必具有一种机制来轻松尽可能快地切换到上一个和更稳定版本的应用程序。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-425">In this case, it is important to have a mechanism to easily switch to a previous and more stable version of the application as quickly as possible.</span></span>

<span data-ttu-id="5c2bd-426">在**Azure App Service**，从源代码管理的连续部署功能可使到此可能谢谢**重新部署**在管理门户中可用的操作。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-426">In **Azure App Service**, continuous deployment from source control makes this possible thanks to the **redeploy** action available in the management portal.</span></span> <span data-ttu-id="5c2bd-427">Azure 跟踪与提交推送到存储库关联的部署，并提供了一个选项以重新部署应用程序使用任何你以前的部署，在任何时间。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-427">Azure keeps track of the deployments associated with the commits pushed to the repository and provides an option to redeploy your application using any of your previous deployments, at any time.</span></span>

<span data-ttu-id="5c2bd-428">在本练习中，你将执行对中的代码的更改**专家 Quiz**应用程序有意将注入*bug*。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-428">In this exercise you will perform a change to the code in the **Geek Quiz** application that intentionally injects a *bug*.</span></span> <span data-ttu-id="5c2bd-429">你将部署到生产应用程序，以查看错误，，然后你将利用重新部署功能，请返回到以前的状态。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-429">You will deploy the application to production to see the error, and then you will take advantage of the redeploy feature to go back to the previous state.</span></span>

<a id="Ex3Task1"></a>
#### <a name="task-1--updating-the-geek-quiz-application"></a><span data-ttu-id="5c2bd-430">任务 1 – 更新专家测验应用程序</span><span class="sxs-lookup"><span data-stu-id="5c2bd-430">Task 1 – Updating the Geek Quiz Application</span></span>

<span data-ttu-id="5c2bd-431">在此任务中，将重构代码的一小部分**TriviaController**类，以提取所选的测验选项从数据库中检索成新方法的逻辑的一部分。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-431">In this task, you will refactor a small piece of code of the **TriviaController** class to extract part of the logic that retrieves the selected quiz option from the database into a new method.</span></span>

1. <span data-ttu-id="5c2bd-432">切换到 Visual Studio 实例与**GeekQuiz**从上一练习的解决方案。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-432">Switch to the Visual Studio instance with the **GeekQuiz** solution from the previous exercise.</span></span>
2. <span data-ttu-id="5c2bd-433">在**解决方案资源管理器**，打开**TriviaController.cs**文件**控制器**文件夹。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-433">In **Solution Explorer**, open the **TriviaController.cs** file inside the **Controllers** folder.</span></span>
3. <span data-ttu-id="5c2bd-434">找到**StoreAsync**方法，然后在下图中突出显示代码的选择。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-434">Locate the **StoreAsync** method and select the code highlighted in the following figure.</span></span>

    ![选择代码](maintainable-azure-websites-managing-change-and-scale/_static/image47.png)

    <span data-ttu-id="5c2bd-436">*选择代码*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-436">*Selecting the code*</span></span>
4. <span data-ttu-id="5c2bd-437">右键单击所选的代码，展开**重构**菜单，然后选择**提取方法...**.</span><span class="sxs-lookup"><span data-stu-id="5c2bd-437">Right-click the selected code, expand the **Refactor** menu and select **Extract Method...**.</span></span>

    ![提取作为新方法的代码](maintainable-azure-websites-managing-change-and-scale/_static/image48.png)

    <span data-ttu-id="5c2bd-439">*选择提取方法*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-439">*Selecting Extract Method*</span></span>
5. <span data-ttu-id="5c2bd-440">在**提取方法**对话框中，命名新方法*MatchesOption*单击**确定**。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-440">In the **Extract Method** dialog box, name the new method *MatchesOption* and click **OK**.</span></span>

    ![指定方法名称](maintainable-azure-websites-managing-change-and-scale/_static/image49.png)

    <span data-ttu-id="5c2bd-442">*指定提取的方法的名称*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-442">*Specifying the name for the extracted method*</span></span>
6. <span data-ttu-id="5c2bd-443">所选的代码然后提取到**MatchesOption**方法。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-443">The selected code is then extracted into the **MatchesOption** method.</span></span> <span data-ttu-id="5c2bd-444">生成的代码将显示在下面的代码段。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-444">The resulting code is shown in the following snippet.</span></span>

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample14.cs)]
7. <span data-ttu-id="5c2bd-445">按**CTRL + S**以保存所做的更改。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-445">Press **CTRL + S** to save the changes.</span></span>

<a id="Ex3Task2"></a>
#### <a name="task-2--redeploying-the-geek-quiz-application"></a><span data-ttu-id="5c2bd-446">任务 2 – 重新部署专家测验应用程序</span><span class="sxs-lookup"><span data-stu-id="5c2bd-446">Task 2 – Redeploying the Geek Quiz Application</span></span>

<span data-ttu-id="5c2bd-447">你现在将推送到存储库，将触发新的部署到生产环境中前一项任务所做的更改。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-447">You will now push the changes you made in the previous task to the repository, which will trigger a new deployment to the production environment.</span></span> <span data-ttu-id="5c2bd-448">然后，你将已进行故障诊断问题使用**F12 开发工具**提供由 Internet Explorer，然后执行回滚到以前的部署从 Azure 管理门户。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-448">Then, you will troubleshot an issue using the **F12 development tools** provided by Internet Explorer, and then perform a rollback to the previous deployment from the Azure management portal.</span></span>

1. <span data-ttu-id="5c2bd-449">打开一个新**Git Bash**控制台部署到 Azure App Service 更新的应用程序。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-449">Open a new **Git Bash** console to deploy the updated application to Azure App Service.</span></span>
2. <span data-ttu-id="5c2bd-450">执行以下命令以将更改推送到 Azure。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-450">Execute the following commands to push the changes to Azure.</span></span> <span data-ttu-id="5c2bd-451">更新*[您的应用程序的路径]*占位符替换为的路径**GeekQuiz**解决方案。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-451">Update the *[YOUR-APPLICATION-PATH]* placeholder with the path to the **GeekQuiz** solution.</span></span> <span data-ttu-id="5c2bd-452">系统将提示您输入部署密码。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-452">You will be prompted for your deployment password.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample15.cmd)]

    ![将重构的代码推送到 Azure](maintainable-azure-websites-managing-change-and-scale/_static/image50.png)

    <span data-ttu-id="5c2bd-454">*将重构的代码推送到 Azure*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-454">*Pushing refactored code to Azure*</span></span>
3. <span data-ttu-id="5c2bd-455">打开 Internet Explorer 并导航到你的 web 应用 (例如`http://<your-web-site>.azurewebsites.net`)。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-455">Open Internet Explorer and navigate to your web app (e.g. `http://<your-web-site>.azurewebsites.net`).</span></span> <span data-ttu-id="5c2bd-456">使用以前创建的凭据登录。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-456">Log in using the previously created credentials.</span></span>
4. <span data-ttu-id="5c2bd-457">按**F12**若要启动的开发工具，选择**网络**选项卡，单击**播放**按钮以开始记录。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-457">Press **F12** to launch the development tools, select the **Network** tab and click the **Play** button to start recording.</span></span>

    <span data-ttu-id="5c2bd-458">![启动网络录制](maintainable-azure-websites-managing-change-and-scale/_static/image51.png "启动网络录制")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-458">![Starting network recording](maintainable-azure-websites-managing-change-and-scale/_static/image51.png "Starting network recording")</span></span>

    <span data-ttu-id="5c2bd-459">*启动网络录制*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-459">*Starting network recording*</span></span>
5. <span data-ttu-id="5c2bd-460">选择任何选项测验。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-460">Select any option of the quiz.</span></span> <span data-ttu-id="5c2bd-461">你将看到没有任何反应。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-461">You will see that nothing happens.</span></span>
6. <span data-ttu-id="5c2bd-462">在**F12**窗口中，对应 POST HTTP 请求的项显示 HTTP **500**结果。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-462">In the **F12** window, the entry corresponding to the POST HTTP request shows an HTTP **500** result.</span></span>

    ![HTTP 500 error](maintainable-azure-websites-managing-change-and-scale/_static/image52.png)

    <span data-ttu-id="5c2bd-464">*HTTP 500 error*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-464">*HTTP 500 error*</span></span>
7. <span data-ttu-id="5c2bd-465">选择**控制台**选项卡。可能的原因的详细信息记录错误。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-465">Select the **Console** tab. An error is logged with the details of the cause.</span></span>

    ![记录的错误](maintainable-azure-websites-managing-change-and-scale/_static/image53.png)

    <span data-ttu-id="5c2bd-467">*记录的错误*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-467">*Logged error*</span></span>
8. <span data-ttu-id="5c2bd-468">查找错误的详细信息部分。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-468">Locate the details part of the error.</span></span> <span data-ttu-id="5c2bd-469">显然，重构你在前面的步骤中提交的代码导致此错误。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-469">Clearly, this error is caused by the code refactoring you committed in the previous steps.</span></span>

    <span data-ttu-id="5c2bd-470">`Details: LINQ to Entities does not recognize the method 'Boolean MatchesOption ...`。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-470">`Details: LINQ to Entities does not recognize the method 'Boolean MatchesOption ...`.</span></span>
9. <span data-ttu-id="5c2bd-471">不要关闭浏览器。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-471">Do not close the browser.</span></span>
10. <span data-ttu-id="5c2bd-472">在新浏览器实例中，导航到[Azure 管理门户](https://manage.windowsazure.com)并使用与你的订阅关联的 Microsoft 帐户登录。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-472">In a new browser instance, navigate to the [Azure management portal](https://manage.windowsazure.com) and sign in using the Microsoft account associated with your subscription.</span></span>
11. <span data-ttu-id="5c2bd-473">选择**网站**单击在练习 2 中创建的 web 应用。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-473">Select **Websites** and click the web app you created in Exercise 2.</span></span>
12. <span data-ttu-id="5c2bd-474">导航到**部署**页。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-474">Navigate to the **Deployments** page.</span></span> <span data-ttu-id="5c2bd-475">请注意，在部署历史记录中会列出执行的所有提交。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-475">Notice that all the commits performed are listed in the deployment history.</span></span>

    ![现有的部署列表](maintainable-azure-websites-managing-change-and-scale/_static/image54.png)

    <span data-ttu-id="5c2bd-477">*现有的部署列表*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-477">*List of existing deployments*</span></span>
13. <span data-ttu-id="5c2bd-478">选择上一次提交，然后单击**重新部署**命令栏上。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-478">Select the previous commit and click **Redeploy** on the command bar.</span></span>

    ![重新部署以前的提交](maintainable-azure-websites-managing-change-and-scale/_static/image55.png)

    <span data-ttu-id="5c2bd-480">*重新部署以前的提交*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-480">*Redeploying the previous commit*</span></span>
14. <span data-ttu-id="5c2bd-481">当系统提示确认，请单击**是**。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-481">When prompted to confirm, click **Yes**.</span></span>

    ![确认重新部署](maintainable-azure-websites-managing-change-and-scale/_static/image56.png)
15. <span data-ttu-id="5c2bd-483">部署完成后，切换回浏览器实例替换为你的 web 应用和按**CTRL + F5**。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-483">When the deployment completes, switch back to the browser instance with your web app and press **CTRL + F5**.</span></span>
16. <span data-ttu-id="5c2bd-484">单击任一选项。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-484">Click any of the options.</span></span> <span data-ttu-id="5c2bd-485">翻转动画现在需要位置并将该结果 (*正确/错误*) 将显示。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-485">The flip animation will now take place and the result (*correct/incorrect*) will be displayed.</span></span>
17. <span data-ttu-id="5c2bd-486">（可选）切换到**Git Bash**控制台，然后执行以下命令以恢复为以前的提交。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-486">(Optional) Switch to the **Git Bash** console and execute the following commands to revert to the previous commit.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5c2bd-487">这些命令创建新的提交，取消 Git 存储库中的错误提交所做的所有更改。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-487">These commands create a new commit that undoes all changes in the Git repository that were made in the bad commit.</span></span> <span data-ttu-id="5c2bd-488">然后，azure 将重新部署使用新的提交的应用程序。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-488">Azure will then redeploy the application using the new commit.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample16.cmd)]

<a id="Exercise4"></a>
### <a name="exercise-4-scaling-using-azure-storage"></a><span data-ttu-id="5c2bd-489">使用 Azure 存储缩放练习 4:</span><span class="sxs-lookup"><span data-stu-id="5c2bd-489">Exercise 4: Scaling Using Azure Storage</span></span>

<span data-ttu-id="5c2bd-490">**Blob**是用于存储大量非结构化的文本或二进制数据，如视频、 音频和图像的最简单方式。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-490">**Blobs** are the simplest way to store large amounts of unstructured text or binary data such as video, audio and images.</span></span> <span data-ttu-id="5c2bd-491">移动应用程序以便对存储的静态内容，可帮助你的应用程序的缩放量提供图像或直接向浏览器的文档。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-491">Moving the static content of your application to Storage, helps to scale your application by serving images or documents directly to the browser.</span></span>

<span data-ttu-id="5c2bd-492">在本练习中，你将移动到 Blob 容器应用程序的静态内容。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-492">In this exercise, you will move the static content of your application to a Blob container.</span></span> <span data-ttu-id="5c2bd-493">然后你将配置应用程序以添加**ASP.NET URL 重写规则**中**Web.config**将你的内容重定向到 Blob 容器。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-493">Then you will configure your application to add an **ASP.NET URL rewrite rule** in the **Web.config** to redirect your content to the Blob container.</span></span>

<a id="Ex4Task1"></a>
#### <a name="task-1--creating-an-azure-storage-account"></a><span data-ttu-id="5c2bd-494">任务 1 – 创建 Azure 存储帐户</span><span class="sxs-lookup"><span data-stu-id="5c2bd-494">Task 1 – Creating an Azure Storage Account</span></span>

<span data-ttu-id="5c2bd-495">在此任务中，您将学习如何创建新的存储帐户使用管理门户。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-495">In this task you will learn how to create a new storage account using the management portal.</span></span>

1. <span data-ttu-id="5c2bd-496">导航到[Azure 管理门户](https://manage.windowsazure.com)并使用与你的订阅关联的 Microsoft 帐户登录。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-496">Navigate to the [Azure management portal](https://manage.windowsazure.com) and sign in using the Microsoft account associated with your subscription.</span></span>
2. <span data-ttu-id="5c2bd-497">选择**新 |数据服务 |存储 |快速创建**若要开始创建新的存储帐户。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-497">Select **New | Data Services | Storage | Quick Create** to start creating a new storage account.</span></span> <span data-ttu-id="5c2bd-498">输入该帐户并选择的唯一名称**区域**从列表中。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-498">Enter a unique name for the account and select a **Region** from the list.</span></span> <span data-ttu-id="5c2bd-499">单击**创建存储帐户**以继续。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-499">Click **Create Storage Account** to continue.</span></span>

    <span data-ttu-id="5c2bd-500">![创建新的存储帐户](maintainable-azure-websites-managing-change-and-scale/_static/image57.png "创建新的存储帐户")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-500">![Creating a new Storage Account](maintainable-azure-websites-managing-change-and-scale/_static/image57.png "Creating a new Storage Account")</span></span>

    <span data-ttu-id="5c2bd-501">*创建新的存储帐户*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-501">*Creating a new storage account*</span></span>
3. <span data-ttu-id="5c2bd-502">在**存储**部分中，等到新存储帐户的状态更改为*联机*以便继续以下步骤。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-502">In the **Storage** section, wait until the status of the new storage account changes to *Online* in order to continue with the following step.</span></span>

    <span data-ttu-id="5c2bd-503">![创建存储帐户](maintainable-azure-websites-managing-change-and-scale/_static/image58.png "创建存储帐户")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-503">![Storage Account created](maintainable-azure-websites-managing-change-and-scale/_static/image58.png "Storage Account created")</span></span>

    <span data-ttu-id="5c2bd-504">*创建存储帐户*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-504">*Storage Account created*</span></span>
4. <span data-ttu-id="5c2bd-505">单击存储帐户名，然后单击**仪表板**页顶部的链接。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-505">Click on the storage account name and then click the **Dashboard** link at the top of the page.</span></span> <span data-ttu-id="5c2bd-506">**仪表板**页为您提供的帐户和可以在你的应用程序一起使用的服务终结点的状态信息。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-506">The **Dashboard** page provides you with information about the status of the account and the service endpoints that can be used within your applications.</span></span>

    <span data-ttu-id="5c2bd-507">![显示存储帐户仪表板](maintainable-azure-websites-managing-change-and-scale/_static/image59.png "显示存储帐户仪表板")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-507">![Displaying the Storage Account Dashboard](maintainable-azure-websites-managing-change-and-scale/_static/image59.png "Displaying the Storage Account Dashboard")</span></span>

    <span data-ttu-id="5c2bd-508">*显示存储帐户仪表板*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-508">*Displaying the Storage Account Dashboard*</span></span>
5. <span data-ttu-id="5c2bd-509">单击**管理访问密钥**导航栏中的按钮。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-509">Click the **Manage Access Keys** button in the navigation bar.</span></span>

    <span data-ttu-id="5c2bd-510">![管理访问密钥按钮](maintainable-azure-websites-managing-change-and-scale/_static/image60.png "管理访问密钥按钮")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-510">![Manage Access Keys button](maintainable-azure-websites-managing-change-and-scale/_static/image60.png "Manage Access Keys button")</span></span>

    <span data-ttu-id="5c2bd-511">*管理访问密钥按钮*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-511">*Manage Access Keys button*</span></span>
6. <span data-ttu-id="5c2bd-512">在**管理访问密钥**对话框中，复制**存储帐户名称**和**主访问密钥**因为您将在以下练习中需要它们。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-512">In the **Manage Access Keys** dialog box, copy the **Storage Account Name** and **Primary Access Key** as you will need them in the following exercise.</span></span> <span data-ttu-id="5c2bd-513">然后，关闭对话框。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-513">Then, close the dialog box.</span></span>

    <span data-ttu-id="5c2bd-514">![管理访问密钥对话框](maintainable-azure-websites-managing-change-and-scale/_static/image61.png "管理访问密钥对话框")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-514">![Manage Access Key dialog box](maintainable-azure-websites-managing-change-and-scale/_static/image61.png "Manage Access Key dialog box")</span></span>

    <span data-ttu-id="5c2bd-515">*管理访问密钥对话框*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-515">*Manage Access Key dialog box*</span></span>

<a id="Ex4Task2"></a>
#### <a name="task-2--uploading-an-asset-to-azure-blob-storage"></a><span data-ttu-id="5c2bd-516">任务 2 – 将资产上载到 Azure Blob 存储</span><span class="sxs-lookup"><span data-stu-id="5c2bd-516">Task 2 – Uploading an Asset to Azure Blob Storage</span></span>

<span data-ttu-id="5c2bd-517">在此任务中，你将使用 Visual Studio 中的服务器资源管理器窗口连接到你的存储帐户。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-517">In this task, you will use the Server Explorer window from Visual Studio to connect to your storage account.</span></span> <span data-ttu-id="5c2bd-518">然后，将创建 blob 容器，并将具有极客 Quiz 徽标的文件上载到容器。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-518">You will then create a blob container and upload a file with the Geek Quiz logo to the container.</span></span>

1. <span data-ttu-id="5c2bd-519">切换到 Visual Studio 实例与**GeekQuiz**从上一练习的解决方案。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-519">Switch to the Visual Studio instance with the **GeekQuiz** solution from the previous exercise.</span></span>
2. <span data-ttu-id="5c2bd-520">从菜单栏中，选择**视图**，然后单击**服务器资源管理器**。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-520">From the menu bar, select **View** and then click **Server Explorer**.</span></span>
3. <span data-ttu-id="5c2bd-521">在**服务器资源管理器**，右键单击**Azure**节点，然后选择**连接到 Azure...**.使用与你的订阅关联的 Microsoft 帐户登录。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-521">In **Server Explorer**, right-click the **Azure** node and select **Connect to Azure...**. Sign in using the Microsoft account associated with your subscription.</span></span>

    ![连接到 Windows Azure](maintainable-azure-websites-managing-change-and-scale/_static/image62.png)

    <span data-ttu-id="5c2bd-523">*连接到 Azure*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-523">*Connect to Azure*</span></span>
4. <span data-ttu-id="5c2bd-524">展开**Azure**节点，右键单击**存储**和选择**附加外部存储...**.</span><span class="sxs-lookup"><span data-stu-id="5c2bd-524">Expand the **Azure** node, right-click **Storage** and select **Attach External Storage...**.</span></span>
5. <span data-ttu-id="5c2bd-525">在**添加新的存储帐户**对话框框中，输入**帐户名称**和**帐户密钥**获取在上一任务和单击**确定**.</span><span class="sxs-lookup"><span data-stu-id="5c2bd-525">In the **Add New Storage Account** dialog box, enter the **Account name** and **Account key** you obtained in the previous task and click **OK**.</span></span>

    ![添加新的存储帐户对话框](maintainable-azure-websites-managing-change-and-scale/_static/image63.png)

    <span data-ttu-id="5c2bd-527">*添加新的存储帐户对话框*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-527">*Add New Storage Account dialog box*</span></span>
6. <span data-ttu-id="5c2bd-528">你的存储帐户应显示在**存储**节点。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-528">Your storage account should appear under the **Storage** node.</span></span> <span data-ttu-id="5c2bd-529">展开你的存储帐户，右键单击**Blob**和选择**创建 Blob 容器...**.</span><span class="sxs-lookup"><span data-stu-id="5c2bd-529">Expand your storage account, right-click **Blobs** and select **Create Blob Container...**.</span></span>

    <span data-ttu-id="5c2bd-530">![创建 Blob 容器](maintainable-azure-websites-managing-change-and-scale/_static/image64.png "创建 Blob 容器")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-530">![Create Blob Container](maintainable-azure-websites-managing-change-and-scale/_static/image64.png "Create Blob Container")</span></span>

    <span data-ttu-id="5c2bd-531">*创建 Blob 容器*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-531">*Create Blob Container*</span></span>
7. <span data-ttu-id="5c2bd-532">在**创建 Blob 容器**对话框框中，输入 blob 容器的名称，单击**确定**。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-532">In the **Create Blob Container** dialog box, enter a name for the blob container and click **OK**.</span></span>

    <span data-ttu-id="5c2bd-533">![创建 Blob 容器对话框](maintainable-azure-websites-managing-change-and-scale/_static/image65.png "创建 Blob 容器对话框")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-533">![Create Blob Container dialog box](maintainable-azure-websites-managing-change-and-scale/_static/image65.png "Create Blob Container dialog box")</span></span>

    <span data-ttu-id="5c2bd-534">*创建 Blob 容器对话框*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-534">*Create Blob Container dialog box*</span></span>
8. <span data-ttu-id="5c2bd-535">应将新的 blob 容器添加到**Blob**节点。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-535">The new blob container should be added to the **Blobs** node.</span></span> <span data-ttu-id="5c2bd-536">更改在容器访问权限，以使容器公开。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-536">Change the access permissions in the container to make the container public.</span></span> <span data-ttu-id="5c2bd-537">要执行此操作，请右键单击**映像**容器，然后选择**属性**。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-537">To do this, right-click the **images** container and select **Properties**.</span></span>

    <span data-ttu-id="5c2bd-538">![映像容器属性](maintainable-azure-websites-managing-change-and-scale/_static/image66.png "映像容器属性")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-538">![images container properties](maintainable-azure-websites-managing-change-and-scale/_static/image66.png "images container properties")</span></span>

    <span data-ttu-id="5c2bd-539">*映像容器属性*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-539">*Images container properties*</span></span>
9. <span data-ttu-id="5c2bd-540">在**属性**窗口中，设置**公共读取访问权限**到**容器**。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-540">In the **Properties** window, set the **Public Read Access** to **Container**.</span></span>

    <span data-ttu-id="5c2bd-541">![更改公共读取访问权限属性](maintainable-azure-websites-managing-change-and-scale/_static/image67.png "更改公共读取访问权限属性")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-541">![Changing public read access property](maintainable-azure-websites-managing-change-and-scale/_static/image67.png "Changing public read access property")</span></span>

    <span data-ttu-id="5c2bd-542">*更改公共读取访问权限属性*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-542">*Changing public read access property*</span></span>
10. <span data-ttu-id="5c2bd-543">当系统提示你是否确实要更改的公共访问属性，请单击**是**。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-543">When prompted if you are sure you want to change the public access property, click **Yes**.</span></span>

    <span data-ttu-id="5c2bd-544">![Microsoft Visual Studio 警告](maintainable-azure-websites-managing-change-and-scale/_static/image68.png "Microsoft Visual Studio 警告")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-544">![Microsoft Visual Studio warning](maintainable-azure-websites-managing-change-and-scale/_static/image68.png "Microsoft Visual Studio warning")</span></span>

    <span data-ttu-id="5c2bd-545">*Microsoft Visual Studio 警告*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-545">*Microsoft Visual Studio warning*</span></span>
11. <span data-ttu-id="5c2bd-546">在**服务器资源管理器**，右键单击**映像**blob 容器，然后选择**查看 Blob 容器**。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-546">In **Server Explorer**, right-click in the **images** blob container and select **View Blob Container**.</span></span>

    <span data-ttu-id="5c2bd-547">![查看 Blob 容器](maintainable-azure-websites-managing-change-and-scale/_static/image69.png "查看 Blob 容器")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-547">![View Blob Container](maintainable-azure-websites-managing-change-and-scale/_static/image69.png "View Blob Container")</span></span>

    <span data-ttu-id="5c2bd-548">*查看 Blob 容器*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-548">*View Blob Container*</span></span>
12. <span data-ttu-id="5c2bd-549">对图像容器应在新窗口中打开，并且没有任何项图例应会显示。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-549">The images container should open in a new window and a legend with no entries should be shown.</span></span> <span data-ttu-id="5c2bd-550">单击**上载**图标以将文件上载到 blob 容器。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-550">Click the **upload** icon to upload a file to the blob container.</span></span>

    <span data-ttu-id="5c2bd-551">![没有任何项的图像容器](maintainable-azure-websites-managing-change-and-scale/_static/image70.png "没有任何项的图像容器")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-551">![Images container with no entries](maintainable-azure-websites-managing-change-and-scale/_static/image70.png "Images container with no entries")</span></span>

    <span data-ttu-id="5c2bd-552">*没有任何项的图像容器*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-552">*Images container with no entries*</span></span>
13. <span data-ttu-id="5c2bd-553">在**上载 Blob**对话框框中，导航到**资产**实验室的文件夹。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-553">In the **Upload Blob** dialog box, navigate to the **Assets** folder of the lab.</span></span> <span data-ttu-id="5c2bd-554">选择**徽标 big.png**文件并单击**打开**。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-554">Select the **logo-big.png** file and click **Open**.</span></span>
14. <span data-ttu-id="5c2bd-555">等待，直到将文件上载。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-555">Wait until the file is uploaded.</span></span> <span data-ttu-id="5c2bd-556">上载完成后，应在图像容器中列出该文件。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-556">When the upload completes, the file should be listed in the images container.</span></span> <span data-ttu-id="5c2bd-557">右键单击文件条目并选择**将 URL 复制**。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-557">Right-click the file entry and select **Copy URL**.</span></span>

    <span data-ttu-id="5c2bd-558">![复制 blob URL](maintainable-azure-websites-managing-change-and-scale/_static/image71.png "复制 blob 文件 URL")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-558">![Copy blob URL](maintainable-azure-websites-managing-change-and-scale/_static/image71.png "Copy blob file URL")</span></span>

    <span data-ttu-id="5c2bd-559">*复制 blob URL*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-559">*Copy blob URL*</span></span>
15. <span data-ttu-id="5c2bd-560">打开 Internet Explorer 并粘贴该 URL。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-560">Open Internet Explorer and paste the URL.</span></span> <span data-ttu-id="5c2bd-561">下图应显示在浏览器。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-561">The following image should be shown in the browser.</span></span>

    <span data-ttu-id="5c2bd-562">![从 Windows Blob 存储的徽标 big.png 映像](maintainable-azure-websites-managing-change-and-scale/_static/image72.png "徽标 big.png 图像从存储")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-562">![logo-big.png image from Windows Blob Storage](maintainable-azure-websites-managing-change-and-scale/_static/image72.png "logo-big.png image from storage")</span></span>

    <span data-ttu-id="5c2bd-563">*从 Azure Blob 存储的徽标 big.png 映像*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-563">*logo-big.png image from Azure Blob Storage*</span></span>

<a id="Ex4Task3"></a>
#### <a name="task-3--updating-the-solution-to-consume-static-content-from-azure-blob-storage"></a><span data-ttu-id="5c2bd-564">任务 3 – 更新解决方案以使用 Azure Blob 存储中的静态内容</span><span class="sxs-lookup"><span data-stu-id="5c2bd-564">Task 3 – Updating the Solution to Consume Static Content from Azure Blob Storage</span></span>

<span data-ttu-id="5c2bd-565">在此任务中，您将配置**GeekQuiz**解决方案来使用该映像上载到 Azure Blob 存储 （而不是位于在 web 应用中的映像） 通过添加在 ASP.NET URL 重写规则**web.config**文件。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-565">In this task, you will configure the **GeekQuiz** solution to consume the image uploaded to Azure Blob Storage (instead of the image located in the web app) by adding an ASP.NET URL rewrite rule in the **web.config** file.</span></span>

1. <span data-ttu-id="5c2bd-566">在 Visual Studio 中，打开**Web.config**文件**GeekQuiz**项目，然后找到 **&lt;system.webServer&gt;** 元素。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-566">In Visual Studio, open the **Web.config** file inside the **GeekQuiz** project and locate the **&lt;system.webServer&gt;** element.</span></span>
2. <span data-ttu-id="5c2bd-567">添加以下代码以添加一个 URL 重写规则，更新将占位符替换为你的存储帐户名称。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-567">Add the following code to add an URL rewrite rule, updating the placeholder with your storage account name.</span></span>

    <span data-ttu-id="5c2bd-568">(代码段- *WebSitesInProduction-Ex4-UrlRewriteRule*)</span><span class="sxs-lookup"><span data-stu-id="5c2bd-568">(Code Snippet - *WebSitesInProduction - Ex4 - UrlRewriteRule*)</span></span>

    [!code-xml[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample17.xml)]

    > [!NOTE]
    > <span data-ttu-id="5c2bd-569">URL 重写是拦截传入 Web 请求，并将请求重定向到不同的资源的过程。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-569">URL rewriting is the process of intercepting an incoming Web request and redirecting the request to a different resource.</span></span> <span data-ttu-id="5c2bd-570">URL 重写规则告知重写引擎请求时需要重定向，并在其中应它们重定向。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-570">The URL rewriting rules tells the rewriting engine when a request needs to be redirected, and where should they be redirected.</span></span> <span data-ttu-id="5c2bd-571">重写规则两个字符串组成： 要在请求 URL 中查找的模式 （通常情况下，使用正则表达式），并找到要替换的模式，如果的字符串。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-571">A rewriting rule is composed of two strings: the pattern to look for in the requested URL (usually, using regular expressions), and the string to replace the pattern with, if found.</span></span> <span data-ttu-id="5c2bd-572">有关详细信息，请参阅[URL 重写在 ASP.NET 中](https://msdn.microsoft.com/library/ms972974.aspx)。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-572">For more information, see [URL Rewriting in ASP.NET](https://msdn.microsoft.com/library/ms972974.aspx).</span></span>
3. <span data-ttu-id="5c2bd-573">按**CTRL + S**以保存所做的更改。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-573">Press **CTRL + S** to save the changes.</span></span>
4. <span data-ttu-id="5c2bd-574">打开一个新**Git Bash**控制台部署到 Azure App Service 更新的应用程序。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-574">Open a new **Git Bash** console to deploy the updated application to Azure App Service.</span></span>
5. <span data-ttu-id="5c2bd-575">执行以下命令以将更改推送到 Azure。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-575">Execute the following commands to push the changes to Azure.</span></span> <span data-ttu-id="5c2bd-576">更新*[您的应用程序的路径]*占位符替换为的路径**GeekQuiz**解决方案。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-576">Update the *[YOUR-APPLICATION-PATH]* placeholder with the path to the **GeekQuiz** solution.</span></span> <span data-ttu-id="5c2bd-577">系统将提示您输入部署密码。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-577">You will be prompted for your deployment password.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample18.cmd)]

    ![将更新部署到 Azure](maintainable-azure-websites-managing-change-and-scale/_static/image73.png)

    <span data-ttu-id="5c2bd-579">*将更新部署到 Azure*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-579">*Deploying update to Azure*</span></span>

<a id="Ex4Task4"></a>
#### <a name="task-4--verification"></a><span data-ttu-id="5c2bd-580">任务 4 – 验证</span><span class="sxs-lookup"><span data-stu-id="5c2bd-580">Task 4 – Verification</span></span>

<span data-ttu-id="5c2bd-581">在此任务将使用**Internet Explorer**浏览**专家 Quiz**应用程序并检查 URL 重写规则映像有效并且你将重定向到在上托管的映像**Azure Blob存储**。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-581">In this task you will use **Internet Explorer** to browse the **Geek Quiz** application and check that the URL rewrite rule for images works and you are redirected to the image hosted on **Azure Blob Storage**.</span></span>

1. <span data-ttu-id="5c2bd-582">打开 Internet Explorer 并导航到你的 web 应用 (例如`http://<your-web-site>.azurewebsites.net`)。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-582">Open Internet Explorer and navigate to your web app (e.g. `http://<your-web-site>.azurewebsites.net`).</span></span> <span data-ttu-id="5c2bd-583">使用以前创建的凭据登录。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-583">Log in using the previously created credentials.</span></span>

    <span data-ttu-id="5c2bd-584">![显示与映像专家 Quiz web 应用](maintainable-azure-websites-managing-change-and-scale/_static/image74.png "显示与映像专家 Quiz web 应用")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-584">![Showing the Geek Quiz web app with the image](maintainable-azure-websites-managing-change-and-scale/_static/image74.png "Showing the Geek Quiz web app with the image")</span></span>

    <span data-ttu-id="5c2bd-585">*显示与映像专家 Quiz web 应用*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-585">*Showing the Geek Quiz web app with the image*</span></span>
2. <span data-ttu-id="5c2bd-586">按**F12**若要启动的开发工具，选择**网络**选项卡上，并开始记录。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-586">Press **F12** to launch the development tools, select the **Network** tab and start recording.</span></span>

    <span data-ttu-id="5c2bd-587">![启动网络录制](maintainable-azure-websites-managing-change-and-scale/_static/image75.png "启动网络录制")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-587">![Starting network recording](maintainable-azure-websites-managing-change-and-scale/_static/image75.png "Starting network recording")</span></span>

    <span data-ttu-id="5c2bd-588">*启动网络录制*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-588">*Starting network recording*</span></span>
3. <span data-ttu-id="5c2bd-589">按**CTRL + F5**刷新网页。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-589">Press **CTRL + F5** to refresh the web page.</span></span>
4. <span data-ttu-id="5c2bd-590">一旦页面完成加载，你应看到的 HTTP 请求**/img/logo-big.png**带有 HTTP URL **301**结果 （重定向） 和另一请求`http://[YOUR-STORAGE-ACCOUNT].blob.core.windows.net/images/logo-big.png`URL 使用 HTTP **200**结果。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-590">Once the page has finished loading, you should see an HTTP request for the **/img/logo-big.png** URL with an HTTP **301** result (redirect) and another request for `http://[YOUR-STORAGE-ACCOUNT].blob.core.windows.net/images/logo-big.png` URL with a HTTP **200** result.</span></span>

    <span data-ttu-id="5c2bd-591">![验证 URL 重定向](maintainable-azure-websites-managing-change-and-scale/_static/image76.png "开发人员工具中显示重定向")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-591">![Verifying the URL redirect](maintainable-azure-websites-managing-change-and-scale/_static/image76.png "Showing the redirect in Dev Tools")</span></span>

    <span data-ttu-id="5c2bd-592">*验证 URL 重定向*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-592">*Verifying the URL redirect*</span></span>

<a id="Exercise5"></a>
### <a name="exercise-5-using-autoscale-for-web-apps"></a><span data-ttu-id="5c2bd-593">练习 5： 使用自动缩放 Web Apps</span><span class="sxs-lookup"><span data-stu-id="5c2bd-593">Exercise 5: Using Autoscale for Web Apps</span></span>

> [!NOTE]
> <span data-ttu-id="5c2bd-594">本练习中是可选的因为它需要支持用于 Web 负载&amp;性能测试该功能仅适用于**Visual Studio 2013 Ultimate Edition**。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-594">This exercise is optional, since it requires support for Web Load &amp; Performance Testing which is only available for **Visual Studio 2013 Ultimate Edition**.</span></span> <span data-ttu-id="5c2bd-595">有关特定的 Visual Studio 2013 功能的详细信息，比较版本[此处](https://www.microsoft.com/visualstudio/eng/products/compare)。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-595">For more information on specific Visual Studio 2013 features, compare versions [here](https://www.microsoft.com/visualstudio/eng/products/compare).</span></span>


<span data-ttu-id="5c2bd-596">**Azure App Service Web Apps** web 运行的应用程序提供的自动缩放功能**标准模式**。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-596">**Azure App Service Web Apps** provides the Autoscale feature for web apps running in **Standard Mode**.</span></span> <span data-ttu-id="5c2bd-597">自动缩放会让 Azure 自动缩放 web 应用程序根据负载的实例计数。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-597">Autoscale lets Azure automatically scale the instance count of your web app depending on the load.</span></span> <span data-ttu-id="5c2bd-598">启用自动缩放后，Azure 将检查一次每隔五分钟的 web 应用的 CPU，并根据需要在该点及时添加实例。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-598">When Autoscale is enabled, Azure checks the CPU of your web app once every five minutes and adds instances as needed at that point in time.</span></span> <span data-ttu-id="5c2bd-599">如果 CPU 使用率较低，Azure 将删除实例一次每两小时以确保不降低你的 web 应用的性能。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-599">If the CPU usage is low, Azure will remove instances once every two hours to ensure that the performance of your web app is not degraded.</span></span>

<span data-ttu-id="5c2bd-600">在本练习中你将经历配置所需的步骤**自动缩放**的功能**专家 Quiz** web 应用。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-600">In this exercise you will go through the steps required to configure the **Autoscale** feature for the **Geek Quiz** web app.</span></span> <span data-ttu-id="5c2bd-601">通过运行 Visual Studio 负载测试，以生成应用程序以触发实例升级足够的 CPU 负载，你将验证此功能。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-601">You will verify this feature by running a Visual Studio load test to generate enough CPU load on the application to trigger an instance upgrade.</span></span>

<a id="Ex5Task1"></a>
#### <a name="task-1--configuring-autoscale-based-on-the-cpu-metric"></a><span data-ttu-id="5c2bd-602">任务 1 – 基于 CPU 指标配置自动缩放</span><span class="sxs-lookup"><span data-stu-id="5c2bd-602">Task 1 – Configuring Autoscale Based on the CPU Metric</span></span>

<span data-ttu-id="5c2bd-603">在此任务将使用 Azure 管理门户启用在练习 2 中创建的 web 应用的自动缩放功能。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-603">In this task you will use the Azure management portal to enable the Autoscale feature for the web app you created in Exercise 2.</span></span>

1. <span data-ttu-id="5c2bd-604">在[Azure 管理门户](https://manage.windowsazure.com/)，选择**网站**单击在练习 2 中创建的 web 应用。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-604">In the [Azure management portal](https://manage.windowsazure.com/), select **Websites** and click the web app you created in Exercise 2.</span></span>
2. <span data-ttu-id="5c2bd-605">导航到**缩放**页。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-605">Navigate to the **Scale** page.</span></span> <span data-ttu-id="5c2bd-606">下**容量**部分中，选择**CPU**为**按指标进行缩放**配置。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-606">Under the **capacity** section, select **CPU** for the **Scale by Metric** configuration.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5c2bd-607">时按 CPU 进行缩放，Azure 将动态调整应用程序使用如果 CPU 使用率发生更改的实例数。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-607">When scaling by CPU, Azure dynamically adjusts the number of instances that the app uses if the CPU usage changes.</span></span>

    <span data-ttu-id="5c2bd-608">![选择按 CPU 缩放到](maintainable-azure-websites-managing-change-and-scale/_static/image77.png "选择自动缩放的 CPU 指标")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-608">![Selecting to scale by CPU](maintainable-azure-websites-managing-change-and-scale/_static/image77.png "Selecting the CPU metric for auto scaling")</span></span>

    <span data-ttu-id="5c2bd-609">*选择按 CPU 进行缩放*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-609">*Selecting to scale by CPU*</span></span>
3. <span data-ttu-id="5c2bd-610">更改**目标 CPU**配置到**20**-**40**百分比。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-610">Change the **Target CPU** configuration to **20**-**40** percent.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5c2bd-611">此范围表示你的 web 应用的平均 CPU 使用率。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-611">This range represents the average CPU usage for your web app.</span></span> <span data-ttu-id="5c2bd-612">Azure 将添加或删除实例以使你的 web 应用保持在此范围内。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-612">Azure will add or remove instances to keep your web app in this range.</span></span> <span data-ttu-id="5c2bd-613">中指定用于缩放的实例的最小和最大数**实例计数**配置。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-613">The minimum and maximum number of instances used for scaling is specified in the **Instance Count** configuration.</span></span> <span data-ttu-id="5c2bd-614">高于或超过该限制，azure 将永远不会。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-614">Azure will never go above or beyond that limit.</span></span>
    > 
    > <span data-ttu-id="5c2bd-615">默认值**目标 CPU**此实验室为了修改值。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-615">The default **Target CPU** values are modified just for the purposes of this lab.</span></span> <span data-ttu-id="5c2bd-616">通过使用小的值配置 CPU 范围，你将要增加到触发器自动缩放几率中等负载放置上应用程序时。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-616">By configuring the CPU range with small values, you are increasing the chances to trigger Autoscale when a moderate load is placed on the application.</span></span>

    <span data-ttu-id="5c2bd-617">![更改目标必须介于 20 和 40%的 CPU](maintainable-azure-websites-managing-change-and-scale/_static/image78.png "更改介于 20%到 40%的目标 CPU")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-617">![Changing the target CPU to be between 20 and 40 percent](maintainable-azure-websites-managing-change-and-scale/_static/image78.png "Changing the target CPU to be between 20 and 40 percent")</span></span>

    <span data-ttu-id="5c2bd-618">*更改目标 CPU 必须介于 20 和 40%*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-618">*Changing the Target CPU to be between 20 and 40 percent*</span></span>
4. <span data-ttu-id="5c2bd-619">单击**保存**以保存所做的更改的命令栏中。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-619">Click **Save** in the command bar to save the changes.</span></span>

<a id="Ex5Task2"></a>
#### <a name="task-2--load-testing-with-visual-studio"></a><span data-ttu-id="5c2bd-620">任务 2 – 负载测试使用 Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5c2bd-620">Task 2 – Load Testing with Visual Studio</span></span>

<span data-ttu-id="5c2bd-621">现在，**自动缩放**已配置，你将创建**Web 性能测试和负载测试项目**在 Visual Studio 生成你的 web 应用一些 CPU 负载。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-621">Now that **Autoscale** has been configured, you will create a **Web Performance and Load Test Project** in Visual Studio to generate some CPU load on your web app.</span></span>

1. <span data-ttu-id="5c2bd-622">打开**Visual Studio Ultimate 2013**和选择**文件 |新 |项目...**启动一个新的解决方案。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-622">Open **Visual Studio Ultimate 2013** and select **File | New | Project...** to start a new solution.</span></span>

    <span data-ttu-id="5c2bd-623">![创建新的项目](maintainable-azure-websites-managing-change-and-scale/_static/image79.png "创建新项目")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-623">![Creating a new project](maintainable-azure-websites-managing-change-and-scale/_static/image79.png "Creating a new project")</span></span>

    <span data-ttu-id="5c2bd-624">*创建新项目*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-624">*Creating a new project*</span></span>
2. <span data-ttu-id="5c2bd-625">在**新项目**对话框中，选择**Web 性能测试和负载测试项目**下**Visual C# |测试**选项卡。请确保**.NET Framework 4.5**是选择，将该项目命名*WebAndLoadTestProject*，选择**位置**单击**确定**。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-625">In the **New Project** dialog box, select **Web Performance and Load Test Project** under the **Visual C# | Test** tab. Make sure **.NET Framework 4.5** is selected, name the project *WebAndLoadTestProject*, choose a **Location** and click **OK**.</span></span>

    <span data-ttu-id="5c2bd-626">![创建新的 Web 和负载测试项目](maintainable-azure-websites-managing-change-and-scale/_static/image80.png "创建新的 Web 和负载测试项目")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-626">![Creating a new Web and Load Test project](maintainable-azure-websites-managing-change-and-scale/_static/image80.png "Creating a new Web and Load Test project")</span></span>

    <span data-ttu-id="5c2bd-627">*创建新的 Web 和负载测试项目*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-627">*Creating a new Web and Load Test project*</span></span>
3. <span data-ttu-id="5c2bd-628">在**WebTest1.webtest**右键单击**WebTest1**节点，然后单击**添加请求**。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-628">In the **WebTest1.webtest** Right-click the **WebTest1** node and click **Add Request**.</span></span>

    <span data-ttu-id="5c2bd-629">![将请求添加到 WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image81.png "将请求添加到 WebTest1")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-629">![Adding a request to WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image81.png "Adding a request to WebTest1")</span></span>

    <span data-ttu-id="5c2bd-630">*将请求添加到 WebTest1*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-630">*Adding a request to WebTest1*</span></span>
4. <span data-ttu-id="5c2bd-631">在**属性**窗口的新的请求节点中，更新**Url**属性以指向你的 web 应用的 URL (例如 *[http://geek-quiz.azurewebsites.net/](http://geek-quiz.azurewebsites.net/)*).</span><span class="sxs-lookup"><span data-stu-id="5c2bd-631">In the **Properties** window of the new request node, update the **Url** property to point to the URL of your web app (e.g. *[http://geek-quiz.azurewebsites.net/](http://geek-quiz.azurewebsites.net/)*).</span></span>

    <span data-ttu-id="5c2bd-632">![更改 Url 属性](maintainable-azure-websites-managing-change-and-scale/_static/image82.png "更改 Url 属性")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-632">![Changing the Url property](maintainable-azure-websites-managing-change-and-scale/_static/image82.png "Changing the Url property")</span></span>

    <span data-ttu-id="5c2bd-633">*更改 Url 属性*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-633">*Changing the Url property*</span></span>
5. <span data-ttu-id="5c2bd-634">在**WebTest1.webtest**窗口中，右键单击**WebTest1**单击**添加循环...**.</span><span class="sxs-lookup"><span data-stu-id="5c2bd-634">In the **WebTest1.webtest** window, right-click **WebTest1** and click **Add Loop...**.</span></span>

    <span data-ttu-id="5c2bd-635">![向 WebTest1 中添加循环](maintainable-azure-websites-managing-change-and-scale/_static/image83.png "WebTest1 向中添加循环")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-635">![Adding a loop to WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image83.png "Adding a loop to WebTest1")</span></span>

    <span data-ttu-id="5c2bd-636">*向 WebTest1 中添加循环*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-636">*Adding a loop to WebTest1*</span></span>
6. <span data-ttu-id="5c2bd-637">在**添加条件规则和项循环**对话框中，选择**For 循环**规则和修改以下属性。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-637">In the **Add Conditional Rule and Items to Loop** dialog box, select the **For Loop** rule and modify the following properties.</span></span>

    1. <span data-ttu-id="5c2bd-638">**终止值：** 1000年</span><span class="sxs-lookup"><span data-stu-id="5c2bd-638">**Terminating value:** 1000</span></span>
    2. <span data-ttu-id="5c2bd-639">**上下文参数名称：**迭代器</span><span class="sxs-lookup"><span data-stu-id="5c2bd-639">**Context Parameter Name:** Iterator</span></span>
    3. <span data-ttu-id="5c2bd-640">**增量值：** 1</span><span class="sxs-lookup"><span data-stu-id="5c2bd-640">**Increment Value:** 1</span></span>

    <span data-ttu-id="5c2bd-641">![选择 For 循环规则并更新属性](maintainable-azure-websites-managing-change-and-scale/_static/image84.png "选择 For 循环规则并更新属性")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-641">![Selecting the For Loop rule and updating the properties](maintainable-azure-websites-managing-change-and-scale/_static/image84.png "Selecting the For Loop rule and updating the properties")</span></span>

    <span data-ttu-id="5c2bd-642">*选择 For 循环规则并更新属性*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-642">*Selecting the For Loop rule and updating the properties*</span></span>
7. <span data-ttu-id="5c2bd-643">下**循环中的项**部分中，选择你以前创建为 for 循环的第一个和最后一个项的请求。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-643">Under the **Items in loop** section, select the request you created previously to be the first and last item for the loop.</span></span> <span data-ttu-id="5c2bd-644">单击“确定”  继续。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-644">Click **OK** to continue.</span></span>

    <span data-ttu-id="5c2bd-645">![选择 for 循环的第一个和最后一个项](maintainable-azure-websites-managing-change-and-scale/_static/image85.png "选择 for 循环的第一个和最后一个项目")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-645">![Selecting the first and last items for the loop](maintainable-azure-websites-managing-change-and-scale/_static/image85.png "Selecting the first and last items for the loop")</span></span>

    <span data-ttu-id="5c2bd-646">*选择 for 循环的第一个和最后一个项目*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-646">*Selecting the first and last items for the loop*</span></span>
8. <span data-ttu-id="5c2bd-647">在**解决方案资源管理器**，右键单击**WebAndLoadTestProject**项目中，展开**添加**菜单，然后选择**负载测试...**.</span><span class="sxs-lookup"><span data-stu-id="5c2bd-647">In **Solution Explorer**, right-click the **WebAndLoadTestProject** project, expand the **Add** menu and select **Load Test...**.</span></span>

    <span data-ttu-id="5c2bd-648">![将负载测试添加到 WebAndLoadTestProject 项目](maintainable-azure-websites-managing-change-and-scale/_static/image86.png "将负载测试添加到 WebAndLoadTestProject 项目")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-648">![Adding a Load Test to the WebAndLoadTestProject project](maintainable-azure-websites-managing-change-and-scale/_static/image86.png "Adding a Load Test to the WebAndLoadTestProject project")</span></span>

    <span data-ttu-id="5c2bd-649">*将负载测试添加到 WebAndLoadTestProject 项目*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-649">*Adding a Load Test to the WebAndLoadTestProject project*</span></span>
9. <span data-ttu-id="5c2bd-650">在**新建负载测试向导**对话框中，单击**下一步**。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-650">In the **New Load Test Wizard** dialog box, click **Next**.</span></span>

    <span data-ttu-id="5c2bd-651">![新建负载测试向导](maintainable-azure-websites-managing-change-and-scale/_static/image87.png "新建负载测试向导")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-651">![New Load Test Wizard](maintainable-azure-websites-managing-change-and-scale/_static/image87.png "New Load Test Wizard")</span></span>

    <span data-ttu-id="5c2bd-652">*新建负载测试向导*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-652">*New Load Test Wizard*</span></span>
10. <span data-ttu-id="5c2bd-653">在**方案**页上，选择**不使用思考时间**单击**下一步**。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-653">In the **Scenario** page, select **Do not use think times** and click **Next**.</span></span>

    <span data-ttu-id="5c2bd-654">![选择不是使用思考时间](maintainable-azure-websites-managing-change-and-scale/_static/image88.png "选择不是使用思考时间")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-654">![Selecting not to use think times](maintainable-azure-websites-managing-change-and-scale/_static/image88.png "Selecting not to use think times")</span></span>

    <span data-ttu-id="5c2bd-655">*选择不使用思考时间*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-655">*Selecting not to use think times*</span></span>
11. <span data-ttu-id="5c2bd-656">在**负载模式**页上，请确保**常量负载**选项。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-656">In the **Load Pattern** page, make sure that the **Constant Load** option is selected.</span></span> <span data-ttu-id="5c2bd-657">更改**用户计数**将设置为**250**用户和单击**下一步**。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-657">Change the **User Count** setting to **250** users and click **Next**.</span></span>

    <span data-ttu-id="5c2bd-658">![更改用户数为 250](maintainable-azure-websites-managing-change-and-scale/_static/image89.png "为 250 更改的用户计数")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-658">![Changing the user count to 250](maintainable-azure-websites-managing-change-and-scale/_static/image89.png "Changing the user count to 250")</span></span>

    <span data-ttu-id="5c2bd-659">*为 250 更改的用户计数*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-659">*Changing the user count to 250*</span></span>
12. <span data-ttu-id="5c2bd-660">在**测试组合模型**页上，选择**基于顺序测试顺序**单击**下一步**。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-660">In the **Test Mix Model** page, select **Based on sequential test order** and click **Next**.</span></span>

    <span data-ttu-id="5c2bd-661">![选择测试组合模型](maintainable-azure-websites-managing-change-and-scale/_static/image90.png "选择测试组合模型")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-661">![Selecting the test mix model](maintainable-azure-websites-managing-change-and-scale/_static/image90.png "Selecting the test mix model")</span></span>

    <span data-ttu-id="5c2bd-662">*选择测试组合模型*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-662">*Selecting the test mix model*</span></span>
13. <span data-ttu-id="5c2bd-663">在**测试组合模型**页上，单击**添加...**要添加到组合的测试。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-663">In the **Test Mix Model** page, click **Add...** to add a test to the mix.</span></span>

    <span data-ttu-id="5c2bd-664">![向测试组合中添加测试](maintainable-azure-websites-managing-change-and-scale/_static/image91.png "向测试组合中添加测试")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-664">![Adding a test to the test mix](maintainable-azure-websites-managing-change-and-scale/_static/image91.png "Adding a test to the test mix")</span></span>

    <span data-ttu-id="5c2bd-665">*向测试组合中添加测试*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-665">*Adding a test to the test mix*</span></span>
14. <span data-ttu-id="5c2bd-666">在**添加测试**对话框中，双击**WebTest1**若要添加到测试**选定的测试**列表。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-666">In the **Add Tests** dialog box, double-click **WebTest1** to add the test to the **Selected tests** list.</span></span> <span data-ttu-id="5c2bd-667">单击“确定”  继续。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-667">Click **OK** to continue.</span></span>

    <span data-ttu-id="5c2bd-668">![添加 WebTest1 测试](maintainable-azure-websites-managing-change-and-scale/_static/image92.png "添加 WebTest1 测试")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-668">![Adding the WebTest1 test](maintainable-azure-websites-managing-change-and-scale/_static/image92.png "Adding the WebTest1 test")</span></span>

    <span data-ttu-id="5c2bd-669">*添加 WebTest1 测试*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-669">*Adding the WebTest1 test*</span></span>
15. <span data-ttu-id="5c2bd-670">返回**测试组合**页上，单击**下一步**。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-670">Back in the **Test Mix** page, click **Next**.</span></span>

    <span data-ttu-id="5c2bd-671">![完成测试组合页](maintainable-azure-websites-managing-change-and-scale/_static/image93.png "完成测试组合页")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-671">![Completing the Test Mix page](maintainable-azure-websites-managing-change-and-scale/_static/image93.png "Completing the Test Mix page")</span></span>

    <span data-ttu-id="5c2bd-672">*完成测试组合页*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-672">*Completing the Test Mix page*</span></span>
16. <span data-ttu-id="5c2bd-673">在**网络组合**页上，单击**下一步**。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-673">In the **Network Mix** page, click **Next**.</span></span>

    <span data-ttu-id="5c2bd-674">![单击网络组合页中的下一个](maintainable-azure-websites-managing-change-and-scale/_static/image94.png "接着在网络组合页中的单击")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-674">![Clicking next in the Network Mix page](maintainable-azure-websites-managing-change-and-scale/_static/image94.png "Clicking next in the Network Mix page")</span></span>

    <span data-ttu-id="5c2bd-675">*单击网络组合页中的下一步*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-675">*Clicking next in the Network Mix page*</span></span>
17. <span data-ttu-id="5c2bd-676">在**浏览器组合**页上，选择**Internet 资源管理器 10.0**作为浏览器类型，然后单击**下一步**。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-676">In the **Browser Mix** page, select **Internet Explorer 10.0** as the browser type and click **Next**.</span></span>

    <span data-ttu-id="5c2bd-677">![选择的浏览器类型](maintainable-azure-websites-managing-change-and-scale/_static/image95.png "选择浏览器类型")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-677">![Selecting the browser type](maintainable-azure-websites-managing-change-and-scale/_static/image95.png "Selecting the browser type")</span></span>

    <span data-ttu-id="5c2bd-678">*选择的浏览器类型*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-678">*Selecting the browser type*</span></span>
18. <span data-ttu-id="5c2bd-679">在**计数器集**页上，单击**下一步**。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-679">In the **Counter Sets** page, click **Next**.</span></span>

    <span data-ttu-id="5c2bd-680">![在计数器集页中单击下一步](maintainable-azure-websites-managing-change-and-scale/_static/image96.png "单击下一步中计数器集页")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-680">![Clicking Next in the Counter Sets page](maintainable-azure-websites-managing-change-and-scale/_static/image96.png "Clicking Next in the Counter Sets page")</span></span>

    <span data-ttu-id="5c2bd-681">*在计数器集页中单击下一步*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-681">*Clicking Next in the Counter Sets page*</span></span>
19. <span data-ttu-id="5c2bd-682">在**运行设置**页上，设置**负载测试持续时间**到**5 分钟**单击**完成**。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-682">In the **Run Settings** page, set the **Load test duration** to **5 minutes** and click **Finish**.</span></span>

    <span data-ttu-id="5c2bd-683">![将负载测试持续时间设置为 5 分钟](maintainable-azure-websites-managing-change-and-scale/_static/image97.png "将负载测试持续时间设置为 5 分钟")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-683">![Setting the load test duration to 5 minutes](maintainable-azure-websites-managing-change-and-scale/_static/image97.png "Setting the load test duration to 5 minutes")</span></span>

    <span data-ttu-id="5c2bd-684">*将负载测试持续时间设置为 5 分钟*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-684">*Setting the load test duration to 5 minutes*</span></span>
20. <span data-ttu-id="5c2bd-685">在**解决方案资源管理器**，双击**Local.settings**要浏览的测试设置文件。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-685">In **Solution Explorer**, double-click the **Local.settings** file to explore the test settings.</span></span> <span data-ttu-id="5c2bd-686">默认情况下，Visual Studio 使用本地计算机以运行测试。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-686">By default, Visual Studio uses your local computer to run the tests.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5c2bd-687">或者，可以配置测试项目中以运行在云使用的负载测试**Visual Studio Online (VSO)**。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-687">Alternatively, you can configure your test project to run the load tests in the cloud using **Visual Studio Online (VSO)**.</span></span> <span data-ttu-id="5c2bd-688">VSO 提供基于云的负载测试模拟更加真实地模拟负载的服务、 避免如 CPU 容量、 可用内存和网络带宽的本地环境约束。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-688">VSO provides a cloud-based load testing service that simulates a more realistic load, avoiding local environment constraints like CPU capacity, available memory and network bandwidth.</span></span> <span data-ttu-id="5c2bd-689">有关使用 VSO 运行负载测试的详细信息，请参阅[本文](https://www.visualstudio.com/get-started/load-test-your-app-vs)。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-689">For more information about using VSO to run load tests, see [this article](https://www.visualstudio.com/get-started/load-test-your-app-vs).</span></span>

    ![测试设置](maintainable-azure-websites-managing-change-and-scale/_static/image98.png)

<a id="Ex5Task3"></a>
#### <a name="task-3--autoscale-verification"></a><span data-ttu-id="5c2bd-691">任务 3 – 自动缩放验证</span><span class="sxs-lookup"><span data-stu-id="5c2bd-691">Task 3 – Autoscale Verification</span></span>

<span data-ttu-id="5c2bd-692">现在，你将执行你在上一任务中创建负载测试，并请参阅你的 web 应用在负载下的行为方式。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-692">You will now execute the load test you created in the previous task and see how your web app behaves under load.</span></span>

1. <span data-ttu-id="5c2bd-693">在**解决方案资源管理器**，双击**LoadTest1.loadtest**以打开负载测试。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-693">In **Solution Explorer**, double-click **LoadTest1.loadtest** to open the load test.</span></span>

    <span data-ttu-id="5c2bd-694">![打开 LoadTest1.loadtest](maintainable-azure-websites-managing-change-and-scale/_static/image99.png "打开 LoadTest1.loadtest")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-694">![Opening LoadTest1.loadtest](maintainable-azure-websites-managing-change-and-scale/_static/image99.png "Opening LoadTest1.loadtest")</span></span>

    <span data-ttu-id="5c2bd-695">*打开 LoadTest1.loadtest*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-695">*Opening LoadTest1.loadtest*</span></span>
2. <span data-ttu-id="5c2bd-696">在**LoadTest1.loadtest**窗口中，单击工具箱以运行负载测试中的第一个按钮。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-696">In the **LoadTest1.loadtest** window, click the first button in the toolbox to run the load test.</span></span>

    <span data-ttu-id="5c2bd-697">![运行负载测试](maintainable-azure-websites-managing-change-and-scale/_static/image100.png "运行负载测试")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-697">![Running the load test](maintainable-azure-websites-managing-change-and-scale/_static/image100.png "Running the load test")</span></span>

    <span data-ttu-id="5c2bd-698">*运行负载测试*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-698">*Running the load test*</span></span>
3. <span data-ttu-id="5c2bd-699">等待，直到负载测试完成。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-699">Wait until the load test completes.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5c2bd-700">负载测试模拟多个同时将请求发送到 web 应用的用户。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-700">The load test simulates multiple users that send requests to the web app simultaneously.</span></span> <span data-ttu-id="5c2bd-701">当运行测试时，你可以监视可用的计数器，以检测任何错误、 警告或向负载测试运行相关的其他信息。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-701">When the test is running, you can monitor the available counters to detect any errors, warnings or other information related to your load test run.</span></span>

    <span data-ttu-id="5c2bd-702">![负载测试运行](maintainable-azure-websites-managing-change-and-scale/_static/image101.png "等待在完成负载测试")</span><span class="sxs-lookup"><span data-stu-id="5c2bd-702">![Load test running](maintainable-azure-websites-managing-change-and-scale/_static/image101.png "Waiting until the load test completes")</span></span>

    <span data-ttu-id="5c2bd-703">*负载测试运行*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-703">*Load test running*</span></span>
4. <span data-ttu-id="5c2bd-704">测试完成后，请返回到管理门户并导航到**缩放**的 web 应用程序页。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-704">Once the test completes, go back to the management portal and navigate to the **Scale** page of your web app.</span></span> <span data-ttu-id="5c2bd-705">下**容量**部分中，你应在图中查看自动部署的新实例。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-705">Under the **capacity** section, you should see in the graph that a new instance was automatically deployed.</span></span>

    ![自动部署的新实例](maintainable-azure-websites-managing-change-and-scale/_static/image102.png)

    <span data-ttu-id="5c2bd-707">*自动部署的新实例*</span><span class="sxs-lookup"><span data-stu-id="5c2bd-707">*New instance automatically deployed*</span></span>

    > [!NOTE]
    > <span data-ttu-id="5c2bd-708">它可能需要几分钟才会显示在关系图的更改 (按**CTRL + F5**定期以刷新页面)。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-708">It may take several minutes for the changes to appear in the graph (press **CTRL + F5** periodically to refresh the page).</span></span> <span data-ttu-id="5c2bd-709">如果看不到任何更改，你可以尝试以下操作：</span><span class="sxs-lookup"><span data-stu-id="5c2bd-709">If you do not see any changes, you can try the following:</span></span>
    > 
    > - <span data-ttu-id="5c2bd-710">增加的负载测试的持续时间 (例如到**10 分钟**)</span><span class="sxs-lookup"><span data-stu-id="5c2bd-710">Increase the duration of the load test (e.g. to **10 minutes**)</span></span>
    > - <span data-ttu-id="5c2bd-711">减少的最大和最小值**目标 CPU**你的 web 应用的自动缩放配置中的范围</span><span class="sxs-lookup"><span data-stu-id="5c2bd-711">Reduce the maximum and minimum values of the **Target CPU** range in the Autoscale configuration of your web app</span></span>
    > - <span data-ttu-id="5c2bd-712">在与云中运行负载测试**Visual Studio Online**。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-712">Run the load test in the cloud with **Visual Studio Online**.</span></span> <span data-ttu-id="5c2bd-713">详细信息[此处](https://www.visualstudio.com/get-started/load-test-your-app-vs.aspx)</span><span class="sxs-lookup"><span data-stu-id="5c2bd-713">More information [here](https://www.visualstudio.com/get-started/load-test-your-app-vs.aspx)</span></span>

* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="5c2bd-714">摘要</span><span class="sxs-lookup"><span data-stu-id="5c2bd-714">Summary</span></span>

<span data-ttu-id="5c2bd-715">在本动手实验，您学习了如何设置和应用程序部署到 Azure 中的生产 web 应用。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-715">In this hands-on lab, you learned how to set up and deploy your application to production web apps in Azure.</span></span> <span data-ttu-id="5c2bd-716">通过检测并更新数据库使用启动**Entity Framework Code First 迁移**，然后继续通过部署你的站点使用的新版本**Git**和执行回退到你的站点的最新稳定版本。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-716">You started by detecting and updating your databases using **Entity Framework Code First Migrations**, then continued by deploying new versions of your site using **Git** and performing rollbacks to the latest stable version of your site.</span></span> <span data-ttu-id="5c2bd-717">此外，您学习了如何缩放应用程序使用存储将静态内容移到 Blob 容器。</span><span class="sxs-lookup"><span data-stu-id="5c2bd-717">Additionally, you learned how to scale your app using Storage to move your static content to a Blob container.</span></span>
