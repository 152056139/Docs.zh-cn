---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
title: ASP.NET MVC 4 基础知识 |Microsoft 文档
author: rick-anderson
description: 此动手实验开始算起 MVC （模型视图控制器） 音乐商店的教程应用程序介绍，并说明如何使用 ASP.NET MV 分步...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: b7dba543-73c3-4534-a9a0-ba70fa2c6a8a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
msc.type: authoredcontent
ms.openlocfilehash: 225dff4663e0e556cfb8966f1078848b4c2b47a5
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/18/2018
ms.locfileid: "34306762"
---
# <a name="aspnet-mvc-4-fundamentals"></a><span data-ttu-id="d9fd7-103">ASP.NET MVC 4 基础知识</span><span class="sxs-lookup"><span data-stu-id="d9fd7-103">ASP.NET MVC 4 Fundamentals</span></span>

<span data-ttu-id="d9fd7-104">通过[Web 营地团队](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="d9fd7-104">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="d9fd7-105">下载 Web 营地培训工具包</span><span class="sxs-lookup"><span data-stu-id="d9fd7-105">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="d9fd7-106">此动手实验开始算起 MVC （模型视图控制器） 音乐商店的介绍，并说明如何使用 ASP.NET MVC 和 Visual Studio 分步教程应用程序。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-106">This Hands-On Lab is based on MVC (Model View Controller) Music Store, a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio.</span></span> <span data-ttu-id="d9fd7-107">在实验室中，你将学习简单起见，尚未电源的结合使用这些技术。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-107">Throughout the lab you will learn the simplicity, yet power of using these technologies together.</span></span> <span data-ttu-id="d9fd7-108">你将通过简单的应用程序启动，并且将生成它，直到有一个完全正常运行的 ASP.NET MVC 4 Web 应用。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-108">You will start with a simple application and will build it until you have a fully functional ASP.NET MVC 4 Web Application.</span></span>

<span data-ttu-id="d9fd7-109">此实验室适用于 ASP.NET MVC 4。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-109">This Lab works with ASP.NET MVC 4.</span></span>

<span data-ttu-id="d9fd7-110">如果你想要浏览的教程应用程序的 ASP.NET MVC 3 版本，你可以找到它在[MVC 音乐商店](https://github.com/evilDave/MVC-Music-Store)。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-110">If you wish to explore the ASP.NET MVC 3 version of the tutorial application, you can find it in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>

<span data-ttu-id="d9fd7-111">此动手实验假定开发人员上可以体验可用于 Web 开发技术，如 HTML 和 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-111">This Hands-On Lab assumes that the developer has experience in Web development technologies, such as HTML and JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="d9fd7-112">在 Web 营地培训工具包中，在包括所有的示例代码和代码段[Microsoft 的 Web/WebCampTrainingKit 版本](https://aka.ms/webcamps-training-kit)。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-112">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="d9fd7-113">特定于此实验室项目位于[ASP.NET MVC 4 基础知识](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals)。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-113">The project specific to this lab is available at [ASP.NET MVC 4 Fundamentals](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).</span></span>

<a id="The_Music_Store_application"></a>
### <a name="the-music-store-application"></a><span data-ttu-id="d9fd7-114">音乐商店应用程序</span><span class="sxs-lookup"><span data-stu-id="d9fd7-114">The Music Store application</span></span>

<span data-ttu-id="d9fd7-115">音乐商店 web 应用程序将在本实验整个生成包含三个主要部分： 购物、 结帐和管理。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-115">The Music Store web application that will be built throughout this Lab comprises three main parts: shopping, checkout, and administration.</span></span> <span data-ttu-id="d9fd7-116">访问者将能够浏览按风格的专辑、 将唱片集添加到购物车、 查看它们的选择和最后转到结帐登录并完成该订单。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-116">Visitors will be able to browse albums by genre, add albums to their cart, review their selection and finally proceed to checkout to login and complete the order.</span></span> <span data-ttu-id="d9fd7-117">此外，存储管理员将能够管理可用的专辑，以及它们的主要属性。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-117">Additionally, store administrators will be able to manage the available albums as well as their main properties.</span></span>

<span data-ttu-id="d9fd7-118">![音乐商店屏幕](aspnet-mvc-4-fundamentals/_static/image1.png "音乐商店屏幕")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-118">![Music Store screens](aspnet-mvc-4-fundamentals/_static/image1.png "Music Store screens")</span></span>

<span data-ttu-id="d9fd7-119">*音乐商店屏幕*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-119">*Music Store screens*</span></span>

<a id="ASPNET_MVC_4_Essentials"></a>
### <a name="aspnet-mvc-4-essentials"></a><span data-ttu-id="d9fd7-120">ASP.NET MVC 4 Essentials</span><span class="sxs-lookup"><span data-stu-id="d9fd7-120">ASP.NET MVC 4 Essentials</span></span>

<span data-ttu-id="d9fd7-121">音乐商店应用程序将使用生成**模型视图控制器 (MVC)**，分隔到三个主要组件的应用程序的体系结构模式：</span><span class="sxs-lookup"><span data-stu-id="d9fd7-121">Music Store application will be built using **Model View Controller (MVC)**, an architectural pattern that separates an application into three main components:</span></span>

- <span data-ttu-id="d9fd7-122">**模型**： 模型对象是实现域逻辑的应用程序的部件。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-122">**Models**: Model objects are the parts of the application that implement the domain logic.</span></span> <span data-ttu-id="d9fd7-123">通常情况下，模型对象还检索和存储在数据库模型状态。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-123">Often, model objects also retrieve and store model state in a database.</span></span>
- <span data-ttu-id="d9fd7-124">**视图：** 视图是显示应用程序的用户界面 (UI) 的组件。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-124">**Views:** Views are the components that display the application's user interface (UI).</span></span> <span data-ttu-id="d9fd7-125">通常情况下，此 UI 会从模型数据创建。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-125">Typically, this UI is created from the model data.</span></span> <span data-ttu-id="d9fd7-126">示例将显示文本框和下拉列表基于唱片集对象的当前状态的唱片集的编辑视图。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-126">An example would be the edit view of Albums that displays text boxes and a drop-down list based on the current state of an Album object.</span></span>
- <span data-ttu-id="d9fd7-127">**控制器：** 控制器是处理用户交互，操作模型，并最终选择的视图来呈现的 UI 的组成部分。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-127">**Controllers:** Controllers are the components that handle user interaction, manipulate the model, and ultimately select a view to render the UI.</span></span> <span data-ttu-id="d9fd7-128">在 MVC 应用程序中，视图仅显示信息；控制器处理并响应用户输入和交互。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-128">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span>

<span data-ttu-id="d9fd7-129">MVC 模式可帮助你创建单独的应用程序 （输入的逻辑、 业务逻辑和 UI 逻辑），同时提供这些元素间的松散耦合的不同方面的应用程序。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-129">The MVC pattern helps you to create applications that separate the different aspects of the application (input logic, business logic, and UI logic), while providing a loose coupling between these elements.</span></span> <span data-ttu-id="d9fd7-130">这种隔离可帮助你管理复杂性，在生成应用程序，因为它使你能够专注于一次的实现的一个方面。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-130">This separation helps you manage complexity when you build an application, as it allows you to focus on one aspect of the implementation at a time.</span></span> <span data-ttu-id="d9fd7-131">此外，MVC 模式可以轻松地应用程序，还鼓励测试驱动开发 (TDD) 将用于创建应用程序进行测试。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-131">In addition, the MVC pattern makes it easy to test applications, also encouraging the use of test-driven development (TDD) for creating applications.</span></span>

<span data-ttu-id="d9fd7-132">**ASP.NET MVC** framework 为创建基于 ASP.NET MVC Web 应用程序提供了 ASP.NET Web 窗体模式的替代方法。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-132">The **ASP.NET MVC** framework provides an alternative to the ASP.NET Web Forms pattern for creating ASP.NET MVC-based Web applications.</span></span> <span data-ttu-id="d9fd7-133">**ASP.NET MVC**框架是一种轻型、 高度可测试演示框架的 （与基于 Web 的窗体应用程序） 与现有的 ASP.NET 功能，如母版页和基于成员资格的集成身份验证，以便获取的核心.NET framework 的所有功能。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-133">The **ASP.NET MVC** framework is a lightweight, highly testable presentation framework that (as with Web-forms-based applications) is integrated with existing ASP.NET features, such as master pages and membership-based authentication so you get all the power of the core .NET framework.</span></span> <span data-ttu-id="d9fd7-134">这是因为你已经使用的所有库也可用 ASP.NET MVC 4 中，您还已熟悉 ASP.NET Web 窗体的情况下很有用。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-134">This is useful if you are already familiar with ASP.NET Web Forms because all the libraries that you already use are available in ASP.NET MVC 4 as well.</span></span>

<span data-ttu-id="d9fd7-135">此外，MVC 应用程序的三个主要组件之间的松耦合还可并行开发。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-135">In addition, the loose coupling between the three main components of an MVC application also promotes parallel development.</span></span> <span data-ttu-id="d9fd7-136">例如，一位开发人员可以处理视图、 控制器逻辑，可以处理第二个开发人员和第三个开发人员可以专注于模型中的业务逻辑。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-136">For instance, one developer can work on the view, a second developer can work on the controller logic, and a third developer can focus on the business logic in the model.</span></span>

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="d9fd7-137">目标</span><span class="sxs-lookup"><span data-stu-id="d9fd7-137">Objectives</span></span>

<span data-ttu-id="d9fd7-138">在本动手实验中，你将了解如何：</span><span class="sxs-lookup"><span data-stu-id="d9fd7-138">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="d9fd7-139">基于音乐应用商店应用程序教程从头开始创建 ASP.NET MVC 应用程序</span><span class="sxs-lookup"><span data-stu-id="d9fd7-139">Create an ASP.NET MVC application from scratch based on the Music Store Application tutorial</span></span>
- <span data-ttu-id="d9fd7-140">添加控制器来处理在主页上的站点和浏览其主要功能的 Url</span><span class="sxs-lookup"><span data-stu-id="d9fd7-140">Add Controllers to handle URLs to the Home page of the site and for browsing its main functionality</span></span>
- <span data-ttu-id="d9fd7-141">添加自定义其样式以及显示的内容视图</span><span class="sxs-lookup"><span data-stu-id="d9fd7-141">Add a View to customize the content displayed along with its style</span></span>
- <span data-ttu-id="d9fd7-142">添加模型类来包含和管理数据和域逻辑</span><span class="sxs-lookup"><span data-stu-id="d9fd7-142">Add Model classes to contain and manage data and domain logic</span></span>
- <span data-ttu-id="d9fd7-143">使用视图模型模式以将信息从控制器操作传递到视图模板</span><span class="sxs-lookup"><span data-stu-id="d9fd7-143">Use View Model pattern to pass information from controller actions to the view templates</span></span>
- <span data-ttu-id="d9fd7-144">浏览 internet 应用程序的 ASP.NET MVC 4 新模板</span><span class="sxs-lookup"><span data-stu-id="d9fd7-144">Explore the ASP.NET MVC 4 new template for internet applications</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="d9fd7-145">系统必备</span><span class="sxs-lookup"><span data-stu-id="d9fd7-145">Prerequisites</span></span>

<span data-ttu-id="d9fd7-146">你必须具有要完成本实验的以下项：</span><span class="sxs-lookup"><span data-stu-id="d9fd7-146">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="d9fd7-147">[Visual Studio 2012 Express for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (读取[附录 A](#AppendixA)有关如何安装它的说明)</span><span class="sxs-lookup"><span data-stu-id="d9fd7-147">[Visual Studio 2012 Express for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (read [Appendix A](#AppendixA) for instructions on how to install it)</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="d9fd7-148">安装</span><span class="sxs-lookup"><span data-stu-id="d9fd7-148">Setup</span></span>

<span data-ttu-id="d9fd7-149">**安装代码片段**</span><span class="sxs-lookup"><span data-stu-id="d9fd7-149">**Installing Code Snippets**</span></span>

<span data-ttu-id="d9fd7-150">为方便起见，你将沿此实验室管理大部分都是代码的可用作 Visual Studio 代码段。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-150">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="d9fd7-151">若要安装运行的代码段 **.\Source\Setup\CodeSnippets.vsi**文件。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-151">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="d9fd7-152">如果你不熟悉 Visual Studio 代码段，并想要了解如何使用它们，你可以从该文档引用的附录&quot;[附录 c： 使用代码段](#AppendixC)&quot;。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-152">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="d9fd7-153">练习</span><span class="sxs-lookup"><span data-stu-id="d9fd7-153">Exercises</span></span>

<span data-ttu-id="d9fd7-154">此动手实验包含通过在以下练习：</span><span class="sxs-lookup"><span data-stu-id="d9fd7-154">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="d9fd7-155">练习 1： 创建 MusicStore ASP.NET MVC Web 应用程序项目</span><span class="sxs-lookup"><span data-stu-id="d9fd7-155">Exercise 1: Creating MusicStore ASP.NET MVC Web Application Project</span></span>](#Exercise1)
2. [<span data-ttu-id="d9fd7-156">练习 2： 创建控制器</span><span class="sxs-lookup"><span data-stu-id="d9fd7-156">Exercise 2: Creating a Controller</span></span>](#Exercise2)
3. [<span data-ttu-id="d9fd7-157">练习 3： 将参数传递给控制器</span><span class="sxs-lookup"><span data-stu-id="d9fd7-157">Exercise 3: Passing parameters to a Controller</span></span>](#Exercise3)
4. [<span data-ttu-id="d9fd7-158">练习 4： 创建视图</span><span class="sxs-lookup"><span data-stu-id="d9fd7-158">Exercise 4: Creating a View</span></span>](#Exercise4)
5. [<span data-ttu-id="d9fd7-159">练习 5： 创建视图模型</span><span class="sxs-lookup"><span data-stu-id="d9fd7-159">Exercise 5: Creating a View Model</span></span>](#Exercise5)
6. [<span data-ttu-id="d9fd7-160">练习 6： 视图中使用参数</span><span class="sxs-lookup"><span data-stu-id="d9fd7-160">Exercise 6: Using parameters in View</span></span>](#Exercise6)
7. [<span data-ttu-id="d9fd7-161">练习 7: ASP.NET MVC 4 新模板浏览</span><span class="sxs-lookup"><span data-stu-id="d9fd7-161">Exercise 7: A lap around ASP.NET MVC 4 New Template</span></span>](#Exercise7)

> [!NOTE]
> <span data-ttu-id="d9fd7-162">每个练习均附带由**结束**包含生成您应该完成练习后获得的解决方案文件夹。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-162">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="d9fd7-163">如果你需要通过在练习工作的更多帮助，可以使用此解决方案作为指南。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-163">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="d9fd7-164">估计时间来完成该实验： **60 分钟**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-164">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_MusicStore_ASPNET_MVC_Web_Application_Project"></a>
### <a name="exercise-1-creating-musicstore-aspnet-mvc-web-application-project"></a><span data-ttu-id="d9fd7-165">练习 1： 创建 MusicStore ASP.NET MVC Web 应用程序项目</span><span class="sxs-lookup"><span data-stu-id="d9fd7-165">Exercise 1: Creating MusicStore ASP.NET MVC Web Application Project</span></span>

<span data-ttu-id="d9fd7-166">在此练习中，你将了解如何在 Visual Studio 2012 Express 中创建 ASP.NET MVC 应用程序，用于 Web，以及其主文件夹组织。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-166">In this exercise, you will learn how to create an ASP.NET MVC application in Visual Studio 2012 Express for Web as well as its main folder organization.</span></span> <span data-ttu-id="d9fd7-167">此外，你将学习如何添加新的控制器，并使其在应用程序的主页中显示一个简单的字符串。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-167">Additionally, you will learn how to add a new Controller and make it display a simple string in the application's home page.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_ASPNET_MVC_Web_Application_Project"></a>
#### <a name="task-1---creating-the-aspnet-mvc-web-application-project"></a><span data-ttu-id="d9fd7-168">任务 1-创建 ASP.NET MVC Web 应用程序项目</span><span class="sxs-lookup"><span data-stu-id="d9fd7-168">Task 1 - Creating the ASP.NET MVC Web Application Project</span></span>

1. <span data-ttu-id="d9fd7-169">在此任务中，你将创建一个空的 ASP.NET MVC 应用程序项目，它使用 MVC Visual Studio 模板。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-169">In this task, you will create an empty ASP.NET MVC application project using the MVC Visual Studio template.</span></span> <span data-ttu-id="d9fd7-170">启动**VS Express for Web**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-170">Start **VS Express for Web**.</span></span>
2. <span data-ttu-id="d9fd7-171">在“文件”菜单上，单击“新建项目”。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-171">On the **File** menu, click **New Project**.</span></span>
3. <span data-ttu-id="d9fd7-172">在**新项目**对话框框中，选择**ASP.NET MVC 4 Web 应用程序**项目类型，位于**Visual C#，** **Web**模板列表。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-172">In the **New Project** dialog box select the **ASP.NET MVC 4 Web Application** project type, located under **Visual C#,** **Web** template list.</span></span>
4. <span data-ttu-id="d9fd7-173">更改**名称**到*MvcMusicStore*。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-173">Change the **Name** to *MvcMusicStore*.</span></span>
5. <span data-ttu-id="d9fd7-174">设置在新解决方案的位置**开始**文件夹在本练习的源文件夹中，例如 **[YOUR HOL 路径] \Source\Ex01-CreatingMusicStoreProject\Begin**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-174">Set the location of the solution inside a new **Begin** folder in this Exercise's Source folder, for example **[YOUR-HOL-PATH]\Source\Ex01-CreatingMusicStoreProject\Begin**.</span></span> <span data-ttu-id="d9fd7-175">单击 **“确定”**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-175">Click **OK**.</span></span>

    <span data-ttu-id="d9fd7-176">![创建新项目对话框](aspnet-mvc-4-fundamentals/_static/image2.png "创建新项目对话框")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-176">![Create New Project Dialog Box](aspnet-mvc-4-fundamentals/_static/image2.png "Create New Project Dialog Box")</span></span>

    <span data-ttu-id="d9fd7-177">*创建新项目对话框*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-177">*Create New Project Dialog Box*</span></span>
6. <span data-ttu-id="d9fd7-178">在**新建 ASP.NET MVC 4 项目**对话框框中，选择**基本**模板并确保**视图引擎**所选**Razor**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-178">In the **New ASP.NET MVC 4 Project** dialog box select the **Basic** template and make sure that the **View engine** selected is **Razor**.</span></span> <span data-ttu-id="d9fd7-179">单击 **“确定”**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-179">Click **OK**.</span></span>

    <span data-ttu-id="d9fd7-180">![新的 ASP.NET MVC 4 项目对话框](aspnet-mvc-4-fundamentals/_static/image3.png "新建 ASP.NET MVC 4 项目对话框")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-180">![New ASP.NET MVC 4 Project Dialog Box](aspnet-mvc-4-fundamentals/_static/image3.png "New ASP.NET MVC 4 Project Dialog Box")</span></span>

    <span data-ttu-id="d9fd7-181">*新的 ASP.NET MVC 4 项目对话框*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-181">*New ASP.NET MVC 4 Project Dialog Box*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Exploring_the_Solution_Structure"></a>
#### <a name="task-2---exploring-the-solution-structure"></a><span data-ttu-id="d9fd7-182">任务 2-浏览解决方案结构</span><span class="sxs-lookup"><span data-stu-id="d9fd7-182">Task 2 - Exploring the Solution Structure</span></span>

<span data-ttu-id="d9fd7-183">ASP.NET MVC framework 包含可帮助您创建 Web 应用程序支持的 MVC 模式的 Visual Studio 项目模板。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-183">The ASP.NET MVC framework includes a Visual Studio project template that helps you create Web applications supporting the MVC pattern.</span></span> <span data-ttu-id="d9fd7-184">此模板创建新的 ASP.NET MVC Web 应用程序使用所需的文件夹、 项模板和配置文件条目。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-184">This template creates a new ASP.NET MVC Web application with the required folders, item templates, and configuration-file entries.</span></span>

<span data-ttu-id="d9fd7-185">在此任务中，你将检查该解决方案结构，以了解涉及的元素和它们之间的关系。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-185">In this task, you will examine the solution structure to understand the elements that are involved and their relationships.</span></span> <span data-ttu-id="d9fd7-186">以下文件夹包含在所有 ASP.NET MVC 应用程序，因为默认情况下的 ASP.NET MVC framework 使用&quot;配置约定&quot;方法，而使某些默认假设基于文件夹命名约定。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-186">The following folders are included in all the ASP.NET MVC application because the ASP.NET MVC framework by default uses a &quot;convention over configuration&quot; approach, and makes some default assumptions based on folder naming conventions.</span></span>

1. <span data-ttu-id="d9fd7-187">创建项目后，查看已在右侧的解决方案资源管理器中创建的文件夹结构：</span><span class="sxs-lookup"><span data-stu-id="d9fd7-187">Once the project is created, review the folder structure that has been created in the Solution Explorer on the right side:</span></span>

    <span data-ttu-id="d9fd7-188">![在解决方案资源管理器的 ASP.NET MVC 文件夹结构](aspnet-mvc-4-fundamentals/_static/image4.png "解决方案资源管理器中的 ASP.NET MVC 文件夹结构")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-188">![ASP.NET MVC Folder structure in Solution Explorer](aspnet-mvc-4-fundamentals/_static/image4.png "ASP.NET MVC Folder structure in Solution Explorer")</span></span>

    <span data-ttu-id="d9fd7-189">*在解决方案资源管理器的 ASP.NET MVC 文件夹结构*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-189">*ASP.NET MVC Folder structure in Solution Explorer*</span></span>

   1. <span data-ttu-id="d9fd7-190">**控制器**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-190">**Controllers**.</span></span> <span data-ttu-id="d9fd7-191">此文件夹将包含控制器类。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-191">This folder will contain the controller classes.</span></span> <span data-ttu-id="d9fd7-192">在基于 MVC 的应用中，控制器负责处理最终用户交互、 操作模型，以及最终选择的视图来呈现的 UI。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-192">In an MVC based application, controllers are responsible for handling end user interaction, manipulating the model, and ultimately choosing a view to render the UI.</span></span>

       > [!NOTE]
       > <span data-ttu-id="d9fd7-193">MVC framework 要求以结尾的所有控制器的名称&quot;控制器&quot;-例如，HomeController、 LoginController 或 ProductController。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-193">The MVC framework requires the names of all controllers to end with &quot;Controller&quot;-for example, HomeController, LoginController, or ProductController.</span></span>
   2. <span data-ttu-id="d9fd7-194">**模型**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-194">**Models**.</span></span> <span data-ttu-id="d9fd7-195">表示 MVC Web 应用程序的应用程序模型的类，提供此文件夹。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-195">This folder is provided for classes that represent the application model for the MVC Web application.</span></span> <span data-ttu-id="d9fd7-196">这通常包括定义对象和与数据存储区交互的逻辑的代码。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-196">This usually includes code that defines objects and the logic for interacting with the data store.</span></span> <span data-ttu-id="d9fd7-197">通常情况下，实际的模型对象将在单独的类库。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-197">Typically, the actual model objects will be in separate class libraries.</span></span> <span data-ttu-id="d9fd7-198">但是，在创建新的应用程序时，可能包含的类，并且然后在开发周期中将它们移到单独的类库，请在以后。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-198">However, when you create a new application, you might include classes and then move them into separate class libraries at a later point in the development cycle.</span></span>
   3. <span data-ttu-id="d9fd7-199">**视图**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-199">**Views**.</span></span> <span data-ttu-id="d9fd7-200">此文件夹是视图，负责显示应用程序的用户界面组件的建议的位置。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-200">This folder is the recommended location for views, the components responsible for displaying the application's user interface.</span></span> <span data-ttu-id="d9fd7-201">视图使用.aspx、.ascx、.cshtml 和.master 文件，除了与呈现视图相关的任何其他文件。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-201">Views use .aspx, .ascx, .cshtml and .master files, in addition to any other files that are related to rendering views.</span></span> <span data-ttu-id="d9fd7-202">视图文件夹包含一个用于每个控制器; 文件夹此文件夹称为具有控制器名称前缀。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-202">Views folder contains a folder for each controller; the folder is named with the controller-name prefix.</span></span> <span data-ttu-id="d9fd7-203">例如，如果你有一个名为的控制器**HomeController**，Views 文件夹将包含名为主页的文件夹。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-203">For example, if you have a controller named **HomeController**, the Views folder will contain a folder named Home.</span></span> <span data-ttu-id="d9fd7-204">默认情况下，当 ASP.NET MVC framework 加载视图，它查找具有 Views\controllerName 文件夹中的请求的视图名称的.aspx 文件 (**视图 [ControllerName] [操作].aspx**) 或 (**视图 [ControllerName][操作].cshtml**) Razor 视图。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-204">By default, when the ASP.NET MVC framework loads a view, it looks for an .aspx file with the requested view name in the Views\controllerName folder (**Views[ControllerName][Action].aspx**) or (**Views[ControllerName][Action].cshtml**) for Razor Views.</span></span>

      > [!NOTE]
      > <span data-ttu-id="d9fd7-205">除了前面列出的文件夹，MVC Web 应用程序使用**Global.asax**文件以设置全局 URL 路由默认值，并使用**Web.config**文件来配置应用程序。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-205">In addition to the folders listed previously, an MVC Web application uses the **Global.asax** file to set global URL routing defaults, and it uses the **Web.config** file to configure the application.</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Adding_a_HomeController"></a>
#### <a name="task-3---adding-a-homecontroller"></a><span data-ttu-id="d9fd7-206">任务 3-添加 HomeController</span><span class="sxs-lookup"><span data-stu-id="d9fd7-206">Task 3 - Adding a HomeController</span></span>

<span data-ttu-id="d9fd7-207">在不使用 MVC 框架的 ASP.NET 应用程序，用户交互被组织的页面，周围和围绕引发和处理这些页面中的事件。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-207">In ASP.NET applications that do not use the MVC framework, user interaction is organized around pages, and around raising and handling events from those pages.</span></span> <span data-ttu-id="d9fd7-208">与此相反，对 ASP.NET MVC 应用程序的用户交互是围绕控制器和其操作方法进行组织。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-208">In contrast, user interaction with ASP.NET MVC applications is organized around controllers and their action methods.</span></span>

<span data-ttu-id="d9fd7-209">另一方面，ASP.NET MVC framework 将 Url 映射到称为控制器的类。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-209">On the other hand, ASP.NET MVC framework maps URLs to classes that are referred to as controllers.</span></span> <span data-ttu-id="d9fd7-210">控制器处理传入的请求、 处理用户输入和交互、 执行相应的应用程序逻辑和确定将发送回客户端的响应 （显示 HTML，下载的文件，将重定向到一个不同的 URL，等等。）。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-210">Controllers process incoming requests, handle user input and interactions, execute appropriate application logic and determine the response to send back to the client (display HTML, download a file, redirect to a different URL, etc.).</span></span> <span data-ttu-id="d9fd7-211">对于显示 HTML，了控制器类通常调用要生成请求的 HTML 标记的单独视图组件。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-211">In the case of displaying HTML, a controller class typically calls a separate view component to generate the HTML markup for the request.</span></span> <span data-ttu-id="d9fd7-212">在 MVC 应用程序中，视图仅显示信息；控制器处理并响应用户输入和交互。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-212">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span>

<span data-ttu-id="d9fd7-213">在此任务中，你将添加了控制器类将处理到音乐商店站点主页的 Url。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-213">In this task, you will add a Controller class that will handle URLs to the Home page of the Music Store site.</span></span>

1. <span data-ttu-id="d9fd7-214">右键单击**控制器**在解决方案资源管理器，选择文件夹**添加**然后**控制器**命令：</span><span class="sxs-lookup"><span data-stu-id="d9fd7-214">Right-click **Controllers** folder within the Solution Explorer, select **Add** and then **Controller** command:</span></span>

    <span data-ttu-id="d9fd7-215">![添加控制器命令](aspnet-mvc-4-fundamentals/_static/image5.png "添加控制器命令")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-215">![Add a Controller Command](aspnet-mvc-4-fundamentals/_static/image5.png "Add a Controller Command")</span></span>

    <span data-ttu-id="d9fd7-216">*添加控制器命令*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-216">*Add Controller Command*</span></span>
2. <span data-ttu-id="d9fd7-217">**添加控制器**此时将显示对话框。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-217">The **Add Controller** dialog appears.</span></span> <span data-ttu-id="d9fd7-218">控制器*HomeController*按**添加**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-218">Name the controller *HomeController* and press **Add**.</span></span>

    <span data-ttu-id="d9fd7-219">![添加控制器对话框](aspnet-mvc-4-fundamentals/_static/image6.png "添加控制器对话框")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-219">![Add Controller Dialog](aspnet-mvc-4-fundamentals/_static/image6.png "Add Controller Dialog")</span></span>

    <span data-ttu-id="d9fd7-220">*添加控制器对话框*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-220">*Add Controller Dialog*</span></span>
3. <span data-ttu-id="d9fd7-221">文件**HomeController.cs**中创建**控制器**文件夹。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-221">The file **HomeController.cs** is created in the **Controllers** folder.</span></span> <span data-ttu-id="d9fd7-222">为了具有**HomeController**其索引操作上返回一个字符串，请更换**索引**方法替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="d9fd7-222">In order to have the **HomeController** return a string on its Index action, replace the **Index** method with the following code:</span></span>

    <span data-ttu-id="d9fd7-223">(代码段- *ASP.NET MVC 4 基础-Ex1 HomeController 索引*)</span><span class="sxs-lookup"><span data-stu-id="d9fd7-223">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex1 HomeController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample1.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="d9fd7-224">任务 4-运行应用程序</span><span class="sxs-lookup"><span data-stu-id="d9fd7-224">Task 4 - Running the Application</span></span>

<span data-ttu-id="d9fd7-225">在此任务中，你还将尝试在 web 浏览器应用程序。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-225">In this task, you will try out the Application in a web browser.</span></span>

1. <span data-ttu-id="d9fd7-226">按**F5**运行该应用程序。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-226">Press **F5** to run the Application.</span></span> <span data-ttu-id="d9fd7-227">编译项目，并在本地 IIS Web 服务器将启动。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-227">The project is compiled and the local IIS Web Server starts.</span></span> <span data-ttu-id="d9fd7-228">本地 IIS Web 服务器将自动打开 web 浏览器指向 Web 服务器的 URL。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-228">The local IIS Web Server will automatically open a web browser pointing to the URL of the Web server.</span></span>

    <span data-ttu-id="d9fd7-229">![Web 浏览器中运行的应用程序](aspnet-mvc-4-fundamentals/_static/image7.png "web 浏览器中运行的应用程序")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-229">![Application running in a web browser](aspnet-mvc-4-fundamentals/_static/image7.png "Application running in a web browser")</span></span>

    <span data-ttu-id="d9fd7-230">*Web 浏览器中运行的应用程序*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-230">*Application running in a web browser*</span></span>

    > [!NOTE]
    > <span data-ttu-id="d9fd7-231">本地 IIS Web 服务器将上一个随机免费端口号来运行网站。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-231">The local IIS Web Server will run the website on a random free port number.</span></span> <span data-ttu-id="d9fd7-232">在上图中，站点运行在`http://localhost:50103/`，因此它正在使用端口 50103。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-232">In the figure above, the site is running at `http://localhost:50103/`, so it's using port 50103.</span></span> <span data-ttu-id="d9fd7-233">端口号可能会有所不同。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-233">Your port number may vary.</span></span>
2. <span data-ttu-id="d9fd7-234">关闭浏览器。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-234">Close the browser.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Controller"></a>
### <a name="exercise-2-creating-a-controller"></a><span data-ttu-id="d9fd7-235">练习 2： 创建控制器</span><span class="sxs-lookup"><span data-stu-id="d9fd7-235">Exercise 2: Creating a Controller</span></span>

<span data-ttu-id="d9fd7-236">在此练习中，你将了解如何更新控制器以实现简单的音乐商店应用程序的功能。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-236">In this exercise, you will learn how to update the controller to implement simple functionality of the Music Store application.</span></span> <span data-ttu-id="d9fd7-237">该控制器将定义用于处理每个以下的特定请求的操作方法：</span><span class="sxs-lookup"><span data-stu-id="d9fd7-237">That controller will define action methods to handle each of the following specific requests:</span></span>

- <span data-ttu-id="d9fd7-238">音乐商店中音乐风格列表页面</span><span class="sxs-lookup"><span data-stu-id="d9fd7-238">A listing page of the music genres in the Music Store</span></span>
- <span data-ttu-id="d9fd7-239">列出所有特定流派音乐集浏览页</span><span class="sxs-lookup"><span data-stu-id="d9fd7-239">A browse page that lists all of the music albums for a particular genre</span></span>
- <span data-ttu-id="d9fd7-240">显示有关特定音乐唱片集的信息的详细信息页</span><span class="sxs-lookup"><span data-stu-id="d9fd7-240">A details page that shows information about a specific music album</span></span>

<span data-ttu-id="d9fd7-241">对于本练习的作用域，这些操作将只返回一个字符串到目前为止。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-241">For the scope of this exercise, those actions will simply return a string by now.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Adding_a_New_StoreController_Class"></a>
#### <a name="task-1---adding-a-new-storecontroller-class"></a><span data-ttu-id="d9fd7-242">任务 1-添加新 StoreController 类</span><span class="sxs-lookup"><span data-stu-id="d9fd7-242">Task 1 - Adding a New StoreController Class</span></span>

<span data-ttu-id="d9fd7-243">在此任务中，你将添加新的控制器。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-243">In this task, you will add a new Controller.</span></span>

1. <span data-ttu-id="d9fd7-244">如果尚未打开，启动**VS Express for Web 2012**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-244">If not already open, start **VS Express for Web 2012**.</span></span>
2. <span data-ttu-id="d9fd7-245">在**文件**菜单上，选择**打开项目**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-245">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="d9fd7-246">在打开项目对话框中，浏览到**Source\Ex02 CreatingAController\Begin**，选择**Begin.sln**单击**打开**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-246">In the Open Project dialog, browse to **Source\Ex02-CreatingAController\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="d9fd7-247">或者，你也可以继续使用该解决方案完成上一练习后获得。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-247">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="d9fd7-248">如果你打开提供**开始**解决方案，你将需要下载一些缺少的 NuGet 程序包才能继续。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-248">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="d9fd7-249">若要执行此操作，请单击**项目**菜单，然后选择**管理 NuGet 包**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-249">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="d9fd7-250">在**管理 NuGet 包**对话框中，单击**还原**以便下载缺少的程序包。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-250">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="d9fd7-251">最后，通过单击生成解决方案**生成** | **生成解决方案**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-251">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="d9fd7-252">使用 NuGet 的优点之一是，你无需提供你的项目中的所有库减小项目大小。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-252">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="d9fd7-253">使用 NuGet 增强工具，请通过指定的包版本在 Packages.config 文件中，你将能够下载首次运行该项目的所有所需的库。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-253">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="d9fd7-254">这是你将需要从本实验打开现有的解决方案后运行这些步骤的原因。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-254">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="d9fd7-255">添加新控制器。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-255">Add the new controller.</span></span> <span data-ttu-id="d9fd7-256">要执行此操作，请右键单击**控制器**在解决方案资源管理器，选择文件夹**添加**然后**控制器**命令。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-256">To do this, right-click the **Controllers** folder within the Solution Explorer, select **Add** and then the **Controller** command.</span></span> <span data-ttu-id="d9fd7-257">更改**控制器名称**到*StoreController*，然后单击**添加**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-257">Change the **Controller Name** to *StoreController*, and click **Add**.</span></span>

    <span data-ttu-id="d9fd7-258">![添加控制器对话框](aspnet-mvc-4-fundamentals/_static/image8.png "添加控制器对话框")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-258">![Add Controller Dialog](aspnet-mvc-4-fundamentals/_static/image8.png "Add Controller Dialog")</span></span>

    <span data-ttu-id="d9fd7-259">*添加控制器对话框*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-259">*Add Controller Dialog*</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_Modifying_the_StoreControllers_Actions"></a>
#### <a name="task-2---modifying-the-storecontrollers-actions"></a><span data-ttu-id="d9fd7-260">任务 2-修改 StoreController 的操作</span><span class="sxs-lookup"><span data-stu-id="d9fd7-260">Task 2 - Modifying the StoreController's Actions</span></span>

<span data-ttu-id="d9fd7-261">在此任务中，你将要修改调用控制器方法**操作**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-261">In this task, you will modify the Controller methods that are called **actions**.</span></span> <span data-ttu-id="d9fd7-262">操作负责处理 URL 请求和确定应发送回浏览器或调用 URL 的用户的内容。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-262">Actions are responsible for handling URL requests and determining the content that should be sent back to the browser or user that invoked the URL.</span></span>

1. <span data-ttu-id="d9fd7-263">**StoreController**类已经**索引**方法。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-263">The **StoreController** class already has an **Index** method.</span></span> <span data-ttu-id="d9fd7-264">你将使用它更高版本在本实验室中来实现页，其中列出所有的音乐商店风格。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-264">You will use it later in this Lab to implement the page that lists all genres of the music store.</span></span> <span data-ttu-id="d9fd7-265">现在，只需将**索引**方法替换为以下代码返回字符串的&quot;Hello from Store.Index()&quot;:</span><span class="sxs-lookup"><span data-stu-id="d9fd7-265">For now, just replace the **Index** method with the following code that returns a string &quot;Hello from Store.Index()&quot;:</span></span>

    <span data-ttu-id="d9fd7-266">(代码段- *ASP.NET MVC 4 基础-Ex2 StoreController 索引*)</span><span class="sxs-lookup"><span data-stu-id="d9fd7-266">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex2 StoreController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample2.cs)]
2. <span data-ttu-id="d9fd7-267">添加**浏览**和**详细信息**方法。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-267">Add **Browse** and **Details** methods.</span></span> <span data-ttu-id="d9fd7-268">若要执行此操作，将添加到下面的代码**StoreController**:</span><span class="sxs-lookup"><span data-stu-id="d9fd7-268">To do this, add the following code to the **StoreController**:</span></span>

    <span data-ttu-id="d9fd7-269">(代码段- *ASP.NET MVC 4 基础-Ex2 StoreController BrowseAndDetails*)</span><span class="sxs-lookup"><span data-stu-id="d9fd7-269">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex2 StoreController BrowseAndDetails*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample3.cs)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="d9fd7-270">任务 3-运行应用程序</span><span class="sxs-lookup"><span data-stu-id="d9fd7-270">Task 3 - Running the Application</span></span>

<span data-ttu-id="d9fd7-271">在此任务中，你还将尝试在 web 浏览器应用程序。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-271">In this task, you will try out the Application in a web browser.</span></span>

1. <span data-ttu-id="d9fd7-272">按**F5**运行该应用程序。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-272">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="d9fd7-273">在启动项目**主页**页。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-273">The project starts in the **Home** page.</span></span> <span data-ttu-id="d9fd7-274">更改 URL 以验证每个操作的实现。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-274">Change the URL to verify each action's implementation.</span></span>

    1. <span data-ttu-id="d9fd7-275">**/ 存储**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-275">**/Store**.</span></span> <span data-ttu-id="d9fd7-276">你将看到 **&quot;Hello from Store.Index()&quot;**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-276">You will see **&quot;Hello from Store.Index()&quot;**.</span></span>
    2. <span data-ttu-id="d9fd7-277">**/ 应用商店/浏览**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-277">**/Store/Browse**.</span></span> <span data-ttu-id="d9fd7-278">你将看到 **&quot;Hello from Store.Browse()&quot;**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-278">You will see **&quot;Hello from Store.Browse()&quot;**.</span></span>
    3. <span data-ttu-id="d9fd7-279">**/ 存储/详细信息**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-279">**/Store/Details**.</span></span> <span data-ttu-id="d9fd7-280">你将看到 **&quot;Hello from Store.Details()&quot;**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-280">You will see **&quot;Hello from Store.Details()&quot;**.</span></span>

        <span data-ttu-id="d9fd7-281">![浏览 StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "浏览 StoreBrowse")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-281">![Browsing StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "Browsing StoreBrowse")</span></span>

        <span data-ttu-id="d9fd7-282">*浏览 /Store/Browse*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-282">*Browsing /Store/Browse*</span></span>
3. <span data-ttu-id="d9fd7-283">关闭浏览器。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-283">Close the browser.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Passing_parameters_to_a_Controller"></a>
### <a name="exercise-3-passing-parameters-to-a-controller"></a><span data-ttu-id="d9fd7-284">练习 3： 将参数传递给控制器</span><span class="sxs-lookup"><span data-stu-id="d9fd7-284">Exercise 3: Passing parameters to a Controller</span></span>

<span data-ttu-id="d9fd7-285">到目前为止，你具有已返回的常量字符串从控制器。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-285">Until now, you have been returning constant strings from the Controllers.</span></span> <span data-ttu-id="d9fd7-286">在本练习中，你将了解如何将参数传递到的控制器使用的 URL 和查询字符串，，然后再进行对浏览器响应文本的方法操作。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-286">In this exercise you will learn how to pass parameters to a Controller using the URL and querystring, and then making the method actions respond with text to the browser.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Adding_Genre_Parameter_to_StoreController"></a>
#### <a name="task-1---adding-genre-parameter-to-storecontroller"></a><span data-ttu-id="d9fd7-287">任务 1-添加 StoreController 流派参数</span><span class="sxs-lookup"><span data-stu-id="d9fd7-287">Task 1 - Adding Genre Parameter to StoreController</span></span>

<span data-ttu-id="d9fd7-288">在此任务中，你将使用**querystring**以发送到的参数**浏览**中的操作方法**StoreController**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-288">In this task, you will use the **querystring** to send parameters to the **Browse** action method in the **StoreController**.</span></span>

1. <span data-ttu-id="d9fd7-289">如果尚未打开，启动**VS Express for Web**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-289">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="d9fd7-290">在**文件**菜单上，选择**打开项目**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-290">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="d9fd7-291">在打开项目对话框中，浏览到**Source\Ex03 PassingParametersToAController\Begin**，选择**Begin.sln**单击**打开**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-291">In the Open Project dialog, browse to **Source\Ex03-PassingParametersToAController\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="d9fd7-292">或者，你也可以继续使用该解决方案完成上一练习后获得。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-292">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="d9fd7-293">如果你打开提供**开始**解决方案，你将需要下载一些缺少的 NuGet 程序包才能继续。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-293">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="d9fd7-294">若要执行此操作，请单击**项目**菜单，然后选择**管理 NuGet 包**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-294">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="d9fd7-295">在**管理 NuGet 包**对话框中，单击**还原**以便下载缺少的程序包。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-295">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="d9fd7-296">最后，通过单击生成解决方案**生成** | **生成解决方案**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-296">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="d9fd7-297">使用 NuGet 的优点之一是，你无需提供你的项目中的所有库减小项目大小。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-297">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="d9fd7-298">使用 NuGet 增强工具，请通过指定的包版本在 Packages.config 文件中，你将能够下载首次运行该项目的所有所需的库。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-298">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="d9fd7-299">这是你将需要从本实验打开现有的解决方案后运行这些步骤的原因。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-299">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="d9fd7-300">打开**StoreController**类。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-300">Open **StoreController** class.</span></span> <span data-ttu-id="d9fd7-301">为此，请在**解决方案资源管理器**，展开**控制器**文件夹并双击**StoreController.cs**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-301">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
4. <span data-ttu-id="d9fd7-302">更改**浏览**方法，添加要为特定流派请求的字符串参数。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-302">Change the **Browse** method, adding a string parameter to request for a specific genre.</span></span> <span data-ttu-id="d9fd7-303">ASP.NET MVC 将自动传递任何查询字符串或窗体发布参数名为**流派**给此操作方法调用时。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-303">ASP.NET MVC will automatically pass any querystring or form post parameters named **genre** to this action method when invoked.</span></span> <span data-ttu-id="d9fd7-304">若要执行此操作，请替换**浏览**方法替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="d9fd7-304">To do this, replace the **Browse** method with the following code:</span></span>

    <span data-ttu-id="d9fd7-305">(代码段- *ASP.NET MVC 4 基础-Ex3 StoreController BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="d9fd7-305">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex3 StoreController BrowseMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="d9fd7-306">你使用**HttpUtility.HtmlEncode**到实用程序方法会阻止用户将 Javascript 注入到视图中，使用的链接  **/存储/浏览？流派 =&lt;脚本&gt;window.location=[http://hackersite.com](http://hackersite.com)&lt;/&gt;**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-306">You are using the **HttpUtility.HtmlEncode** utility method to prevents users from injecting Javascript into the View with a link like **/Store/Browse?Genre=&lt;script&gt;window.location='[http://hackersite.com](http://hackersite.com)'&lt;/script&gt;**.</span></span>
> 
> <span data-ttu-id="d9fd7-307">有关更多说明，请访问[此 msdn 文章](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx)。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-307">For further explanation, please visit [this msdn article](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="d9fd7-308">任务 2-运行应用程序</span><span class="sxs-lookup"><span data-stu-id="d9fd7-308">Task 2 - Running the Application</span></span>

<span data-ttu-id="d9fd7-309">在此任务中，将尝试使用 web 浏览器中的应用程序并使用**流派**参数。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-309">In this task, you will try out the Application in a web browser and use the **genre** parameter.</span></span>

1. <span data-ttu-id="d9fd7-310">按**F5**运行该应用程序。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-310">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="d9fd7-311">在启动项目**主页**页。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-311">The project starts in the **Home** page.</span></span> <span data-ttu-id="d9fd7-312">将 URL 更改为  */存储/浏览？流派 = Disco*以验证操作接收流派参数。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-312">Change the URL to */Store/Browse?Genre=Disco* to verify that the action receives the genre parameter.</span></span>

    <span data-ttu-id="d9fd7-313">![浏览 StoreBrowseGenre = Disco](aspnet-mvc-4-fundamentals/_static/image10.png "浏览 StoreBrowseGenre = Disco")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-313">![Browsing StoreBrowseGenre=Disco](aspnet-mvc-4-fundamentals/_static/image10.png "Browsing StoreBrowseGenre=Disco")</span></span>

    <span data-ttu-id="d9fd7-314">*浏览 /Store/Browse？流派 = Disco*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-314">*Browsing /Store/Browse?Genre=Disco*</span></span>
3. <span data-ttu-id="d9fd7-315">关闭浏览器。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-315">Close the browser.</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Adding_an_Id_Parameter_Embedded_in_the_URL"></a>
#### <a name="task-3---adding-an-id-parameter-embedded-in-the-url"></a><span data-ttu-id="d9fd7-316">任务 3-添加在 URL 中嵌入一个 Id 参数</span><span class="sxs-lookup"><span data-stu-id="d9fd7-316">Task 3 - Adding an Id Parameter Embedded in the URL</span></span>

<span data-ttu-id="d9fd7-317">在此任务中，你将使用**URL**传递**Id**参数**详细信息**操作方法**StoreController**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-317">In this task, you will use the **URL** to pass an **Id** parameter to the **Details** action method of the **StoreController**.</span></span> <span data-ttu-id="d9fd7-318">ASP.NET MVC 的默认路由约定是在操作方法名称后面的 url 段视为一个名为参数**Id**。如果你的操作方法具有名为 Id 参数，然后 ASP.NET MVC 将会自动传递的 URL 段向你作为参数。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-318">ASP.NET MVC's default routing convention is to treat the segment of a URL after the action method name as a parameter named **Id**. If your action method has parameter named Id, then ASP.NET MVC will automatically pass the URL segment to you as a parameter.</span></span> <span data-ttu-id="d9fd7-319">在 URL 中**存储/详细信息/5**， **Id**将解释为**5**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-319">In the URL **Store/Details/5**, **Id** will be interpreted as **5**.</span></span>

1. <span data-ttu-id="d9fd7-320">更改**详细信息**方法**StoreController**、 添加**int**参数调用**id**。若要执行此操作，请替换**详细信息**方法替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="d9fd7-320">Change the **Details** method of the **StoreController**, adding an **int** parameter called **id**. To do this, replace the **Details** method with the following code:</span></span>

    <span data-ttu-id="d9fd7-321">(代码段- *ASP.NET MVC 4 基础-Ex3 StoreController DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="d9fd7-321">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex3 StoreController DetailsMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample5.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="d9fd7-322">任务 4-运行应用程序</span><span class="sxs-lookup"><span data-stu-id="d9fd7-322">Task 4 - Running the Application</span></span>

<span data-ttu-id="d9fd7-323">在此任务中，将尝试使用 web 浏览器中的应用程序并使用**Id**参数。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-323">In this task, you will try out the Application in a web browser and use the **Id** parameter.</span></span>

1. <span data-ttu-id="d9fd7-324">按**F5**运行该应用程序。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-324">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="d9fd7-325">在启动项目**主页**页。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-325">The project starts in the **Home** page.</span></span> <span data-ttu-id="d9fd7-326">将 URL 更改为 */Store/Details/5*以验证操作接收 id 参数。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-326">Change the URL to */Store/Details/5* to verify that the action receives the id parameter.</span></span>

    <span data-ttu-id="d9fd7-327">![浏览 StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "浏览 StoreDetails5")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-327">![Browsing StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "Browsing StoreDetails5")</span></span>

    <span data-ttu-id="d9fd7-328">*浏览 /Store/Details/5*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-328">*Browsing /Store/Details/5*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Creating_a_View"></a>
### <a name="exercise-4-creating-a-view"></a><span data-ttu-id="d9fd7-329">练习 4： 创建视图</span><span class="sxs-lookup"><span data-stu-id="d9fd7-329">Exercise 4: Creating a View</span></span>

<span data-ttu-id="d9fd7-330">到目前为止你具有已返回字符串的控制器操作。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-330">So far you have been returning strings from controller actions.</span></span> <span data-ttu-id="d9fd7-331">尽管是了解控制器的工作原理的有效方法，它是不实际的 Web 应用程序生成方式。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-331">Although that is a useful way of understanding how controllers work, it is not how your real Web applications are built.</span></span> <span data-ttu-id="d9fd7-332">视图是为更好的方法，以生成 HTML 回浏览器中使用的模板文件使用的组件。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-332">Views are components that provide a better approach for generating HTML back to the browser with the use of template files.</span></span>

<span data-ttu-id="d9fd7-333">在本练习中，你将了解如何添加布局的母版页设置常见 HTML 内容，以增强的站点和最后一个视图模板来启用 HomeController 返回 HTML 外观的样式表的模板。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-333">In this exercise you will learn how to add a layout master page to setup a template for common HTML content, a StyleSheet to enhance the look and feel of the site and finally a View template to enable HomeController to return HTML.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Modifying_the_file__layoutcshtml"></a>
#### <a name="task-1---modifying-the-file-layoutcshtml"></a><span data-ttu-id="d9fd7-334">任务 1-修改文件\_layout.cshtml</span><span class="sxs-lookup"><span data-stu-id="d9fd7-334">Task 1 - Modifying the file \_layout.cshtml</span></span>

<span data-ttu-id="d9fd7-335">文件 **~/Views/Shared/\_layout.cshtml**允许你设置用于常见 HTML 中的整个网站中使用的模板。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-335">The file **~/Views/Shared/\_layout.cshtml** allows you to setup a template for common HTML to use across the entire website.</span></span> <span data-ttu-id="d9fd7-336">在此任务将向主页和存储区域添加布局母版页与具有链接的常见标头。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-336">In this task you will add a layout master page with a common header with links to the Home page and Store area.</span></span>

1. <span data-ttu-id="d9fd7-337">如果尚未打开，启动**VS Express for Web**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-337">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="d9fd7-338">在**文件**菜单上，选择**打开项目**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-338">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="d9fd7-339">在打开项目对话框中，浏览到**Source\Ex04 CreatingAView\Begin**，选择**Begin.sln**单击**打开**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-339">In the Open Project dialog, browse to **Source\Ex04-CreatingAView\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="d9fd7-340">或者，你也可以继续使用该解决方案完成上一练习后获得。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-340">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="d9fd7-341">如果你打开提供**开始**解决方案，你将需要下载一些缺少的 NuGet 程序包才能继续。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-341">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="d9fd7-342">若要执行此操作，请单击**项目**菜单，然后选择**管理 NuGet 包**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-342">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="d9fd7-343">在**管理 NuGet 包**对话框中，单击**还原**以便下载缺少的程序包。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-343">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="d9fd7-344">最后，通过单击生成解决方案**生成** | **生成解决方案**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-344">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="d9fd7-345">使用 NuGet 的优点之一是，你无需提供你的项目中的所有库减小项目大小。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-345">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="d9fd7-346">使用 NuGet 增强工具，请通过指定的包版本在 Packages.config 文件中，你将能够下载首次运行该项目的所有所需的库。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-346">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="d9fd7-347">这是你将需要从本实验打开现有的解决方案后运行这些步骤的原因。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-347">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="d9fd7-348">文件 <strong>\_layout.cshtml</strong>包含站点上的所有页的 HTML 容器布局。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-348">The file <strong>\_layout.cshtml</strong> contains the HTML container layout for all pages on the site.</span></span> <span data-ttu-id="d9fd7-349">它包括<strong>&lt;html&gt;</strong> HTML 响应中，元素以及<strong>&lt;头&gt;</strong>和<strong>&lt;正文&gt;</strong>元素。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-349">It includes the <strong>&lt;html&gt;</strong> element for the HTML response, as well as the <strong>&lt;head&gt;</strong> and <strong>&lt;body&gt;</strong> elements.</span></span> <span data-ttu-id="d9fd7-350"><strong>@RenderBody（)</strong>在 HTML 内正文指定区域模板将能够使用动态内容填充该视图。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-350"><strong>@RenderBody()</strong> within the HTML body identify regions that view templates will be able to fill in with dynamic content.</span></span>
   <span data-ttu-id="d9fd7-351">(C#)</span><span class="sxs-lookup"><span data-stu-id="d9fd7-351">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample6.cshtml)]
4. <span data-ttu-id="d9fd7-352">将具有链接的常见标头添加到站点中的所有页面上的主页和应用商店区域。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-352">Add a common header with links to the Home page and Store area on all pages in the site.</span></span> <span data-ttu-id="d9fd7-353">若要做到这一点，请添加下面的代码下面&lt;正文&gt;语句。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-353">In order to do that, add the following code below &lt;body&gt; statement.</span></span>
   <span data-ttu-id="d9fd7-354">(C#)</span><span class="sxs-lookup"><span data-stu-id="d9fd7-354">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample7.cshtml)]
5. <span data-ttu-id="d9fd7-355">包括 div 呈现每一页的正文部分。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-355">Include a div to render the body section of each page.</span></span> <span data-ttu-id="d9fd7-356">替换 <strong>@RenderBody（)</strong>替换为以下 higlighted 代码: (C#)</span><span class="sxs-lookup"><span data-stu-id="d9fd7-356">Replace <strong>@RenderBody()</strong> with the following higlighted code: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample8.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="d9fd7-357">你知道吗？</span><span class="sxs-lookup"><span data-stu-id="d9fd7-357">Did you know?</span></span> <span data-ttu-id="d9fd7-358">Visual Studio 2012 具有它可以更方便地将常用的代码添加在 HTML、 代码文件和的详细信息的代码段 ！</span><span class="sxs-lookup"><span data-stu-id="d9fd7-358">Visual Studio 2012 has snippets that make it easy to add commonly used code in HTML, code files and more!</span></span> <span data-ttu-id="d9fd7-359">尝试通过键入**&lt;div&gt;** 按**选项卡**两次以插入的完整**div**标记。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-359">Try it out by typing **&lt;div&gt;** and pressing **TAB** twice to insert a complete **div** tag.</span></span>

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_CSS_Stylesheet"></a>
#### <a name="task-2---adding-css-stylesheet"></a><span data-ttu-id="d9fd7-360">任务 2-添加 CSS 样式表</span><span class="sxs-lookup"><span data-stu-id="d9fd7-360">Task 2 - Adding CSS Stylesheet</span></span>

<span data-ttu-id="d9fd7-361">空项目模板包括非常简化的 CSS 文件，其中只包含用于显示基本窗体和验证消息的样式。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-361">The empty project template includes a very streamlined CSS file which just includes styles used to display basic forms and validation messages.</span></span> <span data-ttu-id="d9fd7-362">你将使用其他 CSS 和映像 （可能由设计器） 来增强网站的外观和感觉。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-362">You will use additional CSS and images (potentially provided by a designer) in order to enhance the look and feel of the site.</span></span>

<span data-ttu-id="d9fd7-363">在此任务中，你将添加的 CSS 样式表定义的站点的样式。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-363">In this task, you will add a CSS stylesheet to define the styles of the site.</span></span>

1. <span data-ttu-id="d9fd7-364">CSS 文件和映像都将纳入**Source\Assets\Content**这个实验室的文件夹。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-364">The CSS file and images are included in the **Source\Assets\Content** folder of this Lab.</span></span> <span data-ttu-id="d9fd7-365">要将它们添加到应用程序，将从其内容**Windows 资源管理器**到窗口**解决方案资源管理器**在 Visual Studio Express for Web，如下所示：</span><span class="sxs-lookup"><span data-stu-id="d9fd7-365">In order to add them to the application, drag their content from a **Windows Explorer** window into the **Solution Explorer** in Visual Studio Express for Web, as shown below:</span></span>

    <span data-ttu-id="d9fd7-366">![拖动样式内容](aspnet-mvc-4-fundamentals/_static/image12.png "拖动样式内容")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-366">![Dragging style contents](aspnet-mvc-4-fundamentals/_static/image12.png "Dragging style contents")</span></span>

    <span data-ttu-id="d9fd7-367">*拖动样式内容*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-367">*Dragging style contents*</span></span>
2. <span data-ttu-id="d9fd7-368">警告将显示一个对话框，询问是否确定要替换**Site.css**文件和某些现有的映像。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-368">A warning dialog will appear, asking for confirmation to replace **Site.css** file and some existing images.</span></span> <span data-ttu-id="d9fd7-369">检查**应用于所有项目**单击**是**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-369">Check **Apply to all items** and click **Yes**.</span></span>

<a id="Ex4Task3"></a>

<a id="Task_3_-_Adding_a_View_Template"></a>
#### <a name="task-3---adding-a-view-template"></a><span data-ttu-id="d9fd7-370">任务 3-添加视图模板</span><span class="sxs-lookup"><span data-stu-id="d9fd7-370">Task 3 - Adding a View Template</span></span>

<span data-ttu-id="d9fd7-371">在此任务中，你将添加一个视图模板以生成将使用布局主控页的 HTML 响应和 CSS 添加在本练习中。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-371">In this task, you will add a View template to generate the HTML response that will use the layout master page and CSS added in this exercise.</span></span>

1. <span data-ttu-id="d9fd7-372">若要浏览该网站的主页时，请使用视图模板，你将首先需要而不是返回一个字符串，则指示**HomeController 索引**方法将返回**视图**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-372">To use a View template when browsing the site's home page, you will first need to indicate that instead of returning a string, the **HomeController Index** method will return a **View**.</span></span> <span data-ttu-id="d9fd7-373">打开**HomeController**类并更改其**索引**方法以返回**ActionResult**，并将其返回**View()**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-373">Open **HomeController** class and change its **Index** method to return an **ActionResult**, and have it return **View()**.</span></span>

    <span data-ttu-id="d9fd7-374">(代码段- *ASP.NET MVC 4 基础-Ex4 HomeController 索引*)</span><span class="sxs-lookup"><span data-stu-id="d9fd7-374">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex4 HomeController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample9.cs)]
2. <span data-ttu-id="d9fd7-375">现在，你需要添加适当的视图模板。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-375">Now, you need to add an appropriate View template.</span></span> <span data-ttu-id="d9fd7-376">若要这样做，**右键单击**内**索引**操作方法，然后选择**添加视图**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-376">To do this, **right-click** inside the **Index** action method and select **Add View**.</span></span> <span data-ttu-id="d9fd7-377">此时会弹出**添加视图**对话框。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-377">This will bring up the **Add View** dialog.</span></span>

    <span data-ttu-id="d9fd7-378">![添加从索引方法内的某个视图](aspnet-mvc-4-fundamentals/_static/image13.png "添加从索引方法内的某个视图")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-378">![Adding a View from within the Index method](aspnet-mvc-4-fundamentals/_static/image13.png "Adding a View from within the Index method")</span></span>

    <span data-ttu-id="d9fd7-379">*添加从索引方法内的某个视图*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-379">*Adding a View from within the Index method*</span></span>
3. <span data-ttu-id="d9fd7-380">**添加视图**对话框将显示生成的视图模板文件。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-380">The **Add View** Dialog will appear to generate a View template file.</span></span> <span data-ttu-id="d9fd7-381">默认情况下，此对话框预填充视图模板的名称，使其匹配将使用它的操作方法。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-381">By default, this dialog pre-populates the name of the View template so that it matches the action method that will use it.</span></span> <span data-ttu-id="d9fd7-382">因为你使用**添加视图**中的上下文菜单**索引**内 HomeController，操作方法**添加视图**对话框有作为默认视图名称的索引。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-382">Because you used the **Add View** context menu within the **Index** action method within the HomeController, the **Add View** dialog has Index as the default view name.</span></span> <span data-ttu-id="d9fd7-383">单击 **添加**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-383">Click **Add**.</span></span>

    <span data-ttu-id="d9fd7-384">![添加视图对话框](aspnet-mvc-4-fundamentals/_static/image14.png "添加视图对话框")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-384">![Add View Dialog](aspnet-mvc-4-fundamentals/_static/image14.png "Add View Dialog")</span></span>

    <span data-ttu-id="d9fd7-385">*添加视图对话框*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-385">*Add View Dialog*</span></span>
4. <span data-ttu-id="d9fd7-386">Visual Studio 将生成**Index.cshtml**中的视图模板**views/home**文件夹，然后打开它。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-386">Visual Studio generates an **Index.cshtml** view template inside the **Views\Home** folder and then opens it.</span></span>

    <span data-ttu-id="d9fd7-387">![主索引视图创建](aspnet-mvc-4-fundamentals/_static/image15.png "主页索引视图创建")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-387">![Home Index view created](aspnet-mvc-4-fundamentals/_static/image15.png "Home Index view created")</span></span>

    <span data-ttu-id="d9fd7-388">*创建的主索引视图*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-388">*Home Index view created*</span></span>

    > [!NOTE]
    > <span data-ttu-id="d9fd7-389">名称和位置**Index.cshtml**文件是相关，并遵循的默认 ASP.NET MVC 命名约定。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-389">name and location of the **Index.cshtml** file is relevant and follows the default ASP.NET MVC naming conventions.</span></span>
    > 
    > <span data-ttu-id="d9fd7-390">文件夹 \Views\**主页** 与控制器名称匹配 (**主页**控制器)。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-390">The folder \Views\**Home** matches the controller name (**Home** Controller).</span></span> <span data-ttu-id="d9fd7-391">视图模板名称 (**索引**)，与匹配的控制器操作方法，这将会显示该视图。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-391">The View template name (**Index**), matches the controller action method which will be displaying the View.</span></span>
    > 
    > <span data-ttu-id="d9fd7-392">这种方式，可避免 ASP.NET MVC，无需显式指定的名称或视图模板的位置时使用此命名约定来返回的视图。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-392">This way, ASP.NET MVC avoids having to explicitly specify the name or location of a View template when using this naming convention to return a View.</span></span>
5. <span data-ttu-id="d9fd7-393">生成的视图模板基于 **\_layout.cshtml**前面定义的模板。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-393">The generated View template is based on the **\_layout.cshtml** template earlier defined.</span></span> <span data-ttu-id="d9fd7-394">更新将 ViewBag.Title 属性设为**主页**，并更改到主要内容**这就是起始页**，下面的代码中所示：</span><span class="sxs-lookup"><span data-stu-id="d9fd7-394">Update the ViewBag.Title property to **Home**, and change the main content to **This is the Home Page**, as shown in the code below:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample10.cshtml)]
6. <span data-ttu-id="d9fd7-395">选择**MvcMusicStore**项目中的解决方案资源管理器和按**F5**运行该应用程序。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-395">Select **MvcMusicStore** project in the Solution Explorer and Press **F5** to run the Application.</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_Verification"></a>
#### <a name="task-4-verification"></a><span data-ttu-id="d9fd7-396">任务 4： 验证</span><span class="sxs-lookup"><span data-stu-id="d9fd7-396">Task 4: Verification</span></span>

<span data-ttu-id="d9fd7-397">若要验证已正确执行所有步骤在前面的练习，请继续执行，如下所示：</span><span class="sxs-lookup"><span data-stu-id="d9fd7-397">In order to verify that you have correctly performed all the steps in the previous exercise, proceed as follows:</span></span>

<span data-ttu-id="d9fd7-398">在浏览器中打开应用程序后，你应该注意:</span><span class="sxs-lookup"><span data-stu-id="d9fd7-398">With the application opened in a browser, you should note that:</span></span>

1. <span data-ttu-id="d9fd7-399">HomeController 的 Index 操作方法找到并显示**\Views\Home\Index.cshtml**查看模板，即使代码调用**返回 View()**，因为视图模板后接标准的命名约定。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-399">The HomeController's Index action method found and displayed the **\Views\Home\Index.cshtml** View template, even though the code called **return View()**, because the View template followed the standard naming convention.</span></span>
2. <span data-ttu-id="d9fd7-400">主页上显示中定义的欢迎消息**\Views\Home\Index.cshtml**查看模板。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-400">The Home Page displays the welcome message defined within the **\Views\Home\Index.cshtml** view template.</span></span>
3. <span data-ttu-id="d9fd7-401">使用主页 **\_layout.cshtml**模板，因此，在欢迎消息包含在标准站点 HTML 布局。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-401">The Home Page is using the **\_layout.cshtml** template, and so the welcome message is contained within the standard site HTML layout.</span></span>

    <span data-ttu-id="d9fd7-402">![主页使用定义 LayoutPage 和样式的索引视图](aspnet-mvc-4-fundamentals/_static/image16.png "主页使用定义 LayoutPage 和样式的索引视图")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-402">![Home Index View using the defined LayoutPage and style](aspnet-mvc-4-fundamentals/_static/image16.png "Home Index View using the defined LayoutPage and style")</span></span>

    <span data-ttu-id="d9fd7-403">*使用定义 LayoutPage 和样式的主索引视图*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-403">*Home Index View using the defined LayoutPage and style*</span></span>

<a id="Exercise5"></a>

<a id="Exercise_5_Creating_a_View_Model"></a>
### <a name="exercise-5-creating-a-view-model"></a><span data-ttu-id="d9fd7-404">练习 5： 创建视图模型</span><span class="sxs-lookup"><span data-stu-id="d9fd7-404">Exercise 5: Creating a View Model</span></span>

<span data-ttu-id="d9fd7-405">目前为止，进行硬编码 HTML，显示您的视图，但若要创建动态 web 应用程序，视图模板应从控制器接收信息。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-405">So far, you made your Views display hardcoded HTML, but, in order to create dynamic web applications, the View template should receive information from the Controller.</span></span> <span data-ttu-id="d9fd7-406">要用于此目的的一个常见方法是**ViewModel**模式，允许要打包生成相应的 HTML 响应所需的所有信息的控制器。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-406">One common technique to be used for that purpose is the **ViewModel** pattern, which allows a Controller to package up all the information needed to generate the appropriate HTML response.</span></span>

<span data-ttu-id="d9fd7-407">在本练习中，您将学习如何创建 ViewModel 类并添加所需的属性： 风格商店和这些风格的列表中的数。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-407">In this exercise, you will learn how to create a ViewModel class and add the required properties: the number of genres in the store and a list of those genres.</span></span> <span data-ttu-id="d9fd7-408">你还将更新 StoreController 用于创建视图模型，并且最后，你将创建一个新的视图模板将在页中显示所述的属性。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-408">You will also update the StoreController to use the created ViewModel, and finally, you will create a new View template that will display the mentioned properties in the page.</span></span>

<a id="Ex5Task1"></a>

<a id="Task_1_-_Creating_a_ViewModel_Class"></a>
#### <a name="task-1---creating-a-viewmodel-class"></a><span data-ttu-id="d9fd7-409">任务 1-创建 ViewModel 类</span><span class="sxs-lookup"><span data-stu-id="d9fd7-409">Task 1 - Creating a ViewModel Class</span></span>

<span data-ttu-id="d9fd7-410">在此任务中，你将创建将实现存储风格列表方案 ViewModel 类。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-410">In this task, you will create a ViewModel class that will implement the Store genre listing scenario.</span></span>

1. <span data-ttu-id="d9fd7-411">如果尚未打开，启动**VS Express for Web**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-411">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="d9fd7-412">在**文件**菜单上，选择**打开项目**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-412">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="d9fd7-413">在打开项目对话框中，浏览到**Source\Ex05 CreatingAViewModel\Begin**，选择**Begin.sln**单击**打开**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-413">In the Open Project dialog, browse to **Source\Ex05-CreatingAViewModel\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="d9fd7-414">或者，你也可以继续使用该解决方案完成上一练习后获得。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-414">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="d9fd7-415">如果你打开提供**开始**解决方案，你将需要下载一些缺少的 NuGet 程序包才能继续。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-415">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="d9fd7-416">若要执行此操作，请单击**项目**菜单，然后选择**管理 NuGet 包**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-416">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="d9fd7-417">在**管理 NuGet 包**对话框中，单击**还原**以便下载缺少的程序包。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-417">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="d9fd7-418">最后，通过单击生成解决方案**生成** | **生成解决方案**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-418">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="d9fd7-419">使用 NuGet 的优点之一是，你无需提供你的项目中的所有库减小项目大小。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-419">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="d9fd7-420">使用 NuGet 增强工具，请通过指定的包版本在 Packages.config 文件中，你将能够下载首次运行该项目的所有所需的库。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-420">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="d9fd7-421">这是你将需要从本实验打开现有的解决方案后运行这些步骤的原因。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-421">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="d9fd7-422">创建**Viewmodel**要用于保存 ViewModel 文件夹。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-422">Create a **ViewModels** folder to hold the ViewModel.</span></span> <span data-ttu-id="d9fd7-423">要执行此操作，请右键单击顶级**MvcMusicStore**项目，依次选择**添加**然后**新文件夹**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-423">To do this, right-click the top-level **MvcMusicStore** project, select **Add** and then **New Folder**.</span></span>

    <span data-ttu-id="d9fd7-424">![添加一个新文件夹](aspnet-mvc-4-fundamentals/_static/image17.png "添加一个新文件夹")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-424">![Adding a new folder](aspnet-mvc-4-fundamentals/_static/image17.png "Adding a new folder")</span></span>

    <span data-ttu-id="d9fd7-425">*添加一个新文件夹*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-425">*Adding a new folder*</span></span>
4. <span data-ttu-id="d9fd7-426">将文件夹命名为*Viewmodel*。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-426">Name the folder *ViewModels*.</span></span>

    <span data-ttu-id="d9fd7-427">![在解决方案资源管理器的 Viewmodel 文件夹](aspnet-mvc-4-fundamentals/_static/image18.png "Viewmodel 文件夹在解决方案资源管理器")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-427">![ViewModels folder in Solution Explorer](aspnet-mvc-4-fundamentals/_static/image18.png "ViewModels folder in Solution Explorer")</span></span>

    <span data-ttu-id="d9fd7-428">*在解决方案资源管理器的 Viewmodel 文件夹*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-428">*ViewModels folder in Solution Explorer*</span></span>
5. <span data-ttu-id="d9fd7-429">创建**ViewModel**类。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-429">Create a **ViewModel** class.</span></span> <span data-ttu-id="d9fd7-430">若要执行此操作，右键单击**Viewmodel**文件夹最近创建的选择**添加**然后**新项**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-430">To do this, right-click on the **ViewModels** folder recently created, select **Add** and then **New Item**.</span></span> <span data-ttu-id="d9fd7-431">下**代码**，选择**类**项并将该文件命名*StoreIndexViewModel.cs*，然后单击**添加**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-431">Under **Code**, choose the **Class** item and name the file *StoreIndexViewModel.cs*, then click **Add**.</span></span>

    <span data-ttu-id="d9fd7-432">![添加新类](aspnet-mvc-4-fundamentals/_static/image19.png "添加新类")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-432">![Adding a new Class](aspnet-mvc-4-fundamentals/_static/image19.png "Adding a new Class")</span></span>

    <span data-ttu-id="d9fd7-433">*添加新类*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-433">*Adding a new Class*</span></span>

    <span data-ttu-id="d9fd7-434">![创建 StoreIndexViewModel 类](aspnet-mvc-4-fundamentals/_static/image20.png "创建 StoreIndexViewModel 类")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-434">![Creating StoreIndexViewModel class](aspnet-mvc-4-fundamentals/_static/image20.png "Creating StoreIndexViewModel class")</span></span>

    <span data-ttu-id="d9fd7-435">*创建 StoreIndexViewModel 类*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-435">*Creating StoreIndexViewModel class*</span></span>

<a id="Ex5Task2"></a>

<a id="Task_2_-_Adding_Properties_to_the_ViewModel_class"></a>
#### <a name="task-2---adding-properties-to-the-viewmodel-class"></a><span data-ttu-id="d9fd7-436">任务 2-将属性添加到 ViewModel 类</span><span class="sxs-lookup"><span data-stu-id="d9fd7-436">Task 2 - Adding Properties to the ViewModel class</span></span>

<span data-ttu-id="d9fd7-437">有两个参数来传递从 StoreController 到视图模板以生成预期的 HTML 响应： 风格商店和这些风格的列表中的数。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-437">There are two parameters to be passed from the StoreController to the View template in order to generate the expected HTML response: the number of genres in the store and a list of those genres.</span></span>

<span data-ttu-id="d9fd7-438">在此任务中，你将添加到这些 2 属性**StoreIndexViewModel**类： **NumberOfGenres** （整数） 和**风格**（的字符串列表）。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-438">In this task, you will add those 2 properties to the **StoreIndexViewModel** class: **NumberOfGenres** (an integer) and **Genres** (a list of strings).</span></span>

1. <span data-ttu-id="d9fd7-439">添加**NumberOfGenres**和**风格**属性设置为**StoreIndexViewModel**类。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-439">Add **NumberOfGenres** and **Genres** properties to the **StoreIndexViewModel** class.</span></span> <span data-ttu-id="d9fd7-440">若要执行此操作，请向类定义添加下面的两行代码：</span><span class="sxs-lookup"><span data-stu-id="d9fd7-440">To do this, add the following 2 lines to the class definition:</span></span>

    <span data-ttu-id="d9fd7-441">(代码段- *ASP.NET MVC 4 基础-Ex5 StoreIndexViewModel 属性*)</span><span class="sxs-lookup"><span data-stu-id="d9fd7-441">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel properties*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample11.cs)]

> [!NOTE]
> <span data-ttu-id="d9fd7-442">**{获取; 设置;}** 表示法将使用 C# 的自动实现的属性功能。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-442">The **{ get; set; }** notation makes use of C#'s auto-implemented properties feature.</span></span> <span data-ttu-id="d9fd7-443">它而无需我们可以声明一个后备字段提供一个属性的优点。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-443">It provides the benefits of a property without requiring us to declare a backing field.</span></span>

<a id="Ex5Task3"></a>

<a id="Task_3_-_Updating_StoreController_to_use_the_StoreIndexViewModel"></a>
#### <a name="task-3---updating-storecontroller-to-use-the-storeindexviewmodel"></a><span data-ttu-id="d9fd7-444">任务 3-更新 StoreController 使用 StoreIndexViewModel</span><span class="sxs-lookup"><span data-stu-id="d9fd7-444">Task 3 - Updating StoreController to use the StoreIndexViewModel</span></span>

<span data-ttu-id="d9fd7-445">**StoreIndexViewModel**类封装从将传递所需的信息**StoreController**的**索引**到视图模板以生成响应的方法.</span><span class="sxs-lookup"><span data-stu-id="d9fd7-445">The **StoreIndexViewModel** class encapsulates the information needed to pass from **StoreController**'s **Index** method to a View template in order to generate a response.</span></span>

<span data-ttu-id="d9fd7-446">在此任务中，你将更新**StoreController**使用**StoreIndexViewModel**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-446">In this task, you will update the **StoreController** to use the **StoreIndexViewModel**.</span></span>

1. <span data-ttu-id="d9fd7-447">打开**StoreController**类。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-447">Open **StoreController** class.</span></span>

    <span data-ttu-id="d9fd7-448">![打开 StoreController 类](aspnet-mvc-4-fundamentals/_static/image21.png "打开 StoreController 类")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-448">![Opening StoreController class](aspnet-mvc-4-fundamentals/_static/image21.png "Opening StoreController class")</span></span>

    <span data-ttu-id="d9fd7-449">*打开 StoreController 类*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-449">*Opening StoreController class*</span></span>
2. <span data-ttu-id="d9fd7-450">若要使用**StoreIndexViewModel**类**StoreController**，在顶部添加以下命名空间**StoreController**代码：</span><span class="sxs-lookup"><span data-stu-id="d9fd7-450">In order to use the **StoreIndexViewModel** class from the **StoreController**, add the following namespace at the top of the **StoreController** code:</span></span>

    <span data-ttu-id="d9fd7-451">(代码段- *ASP.NET MVC 4 基础-使用 Viewmodel Ex5 StoreIndexViewModel*)</span><span class="sxs-lookup"><span data-stu-id="d9fd7-451">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel using ViewModels*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample12.cs)]
3. <span data-ttu-id="d9fd7-452">更改**StoreController**的**索引**操作方法，以便它创建和填充**StoreIndexViewModel**对象，然后将其传递到的视图模板生成它的 HTML 响应。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-452">Change the **StoreController**'s **Index** action method so that it creates and populates a **StoreIndexViewModel** object and then passes it off to a View template to generate an HTML response with it.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d9fd7-453">在实验室中&quot;ASP.NET MVC 模型和数据访问&quot;你将编写从数据库中检索的应用商店风格列表的代码。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-453">In Lab &quot;ASP.NET MVC Models and Data Access&quot; you will write code that retrieves the list of store genres from a database.</span></span> <span data-ttu-id="d9fd7-454">在下面的代码中，你将创建**列表**将填充的虚拟数据风格的**StoreIndexViewModel**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-454">In the following code, you will create a **List** of dummy data genres that will populate the **StoreIndexViewModel**.</span></span>
    > 
    > <span data-ttu-id="d9fd7-455">创建和设置后**StoreIndexViewModel**对象，它将作为自变量传递**视图**方法。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-455">After creating and setting up the **StoreIndexViewModel** object, it will be passed as an argument to the **View** method.</span></span> <span data-ttu-id="d9fd7-456">这表示视图模板将使用该对象来生成它的 HTML 响应。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-456">This indicates that the View template will use that object to generate an HTML response with it.</span></span>
4. <span data-ttu-id="d9fd7-457">替换**索引**方法替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="d9fd7-457">Replace the **Index** method with the following code:</span></span>

    <span data-ttu-id="d9fd7-458">(代码段- *ASP.NET MVC 4 基础-Ex5 StoreController Index 方法*)</span><span class="sxs-lookup"><span data-stu-id="d9fd7-458">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreController Index method*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample13.cs)]

> [!NOTE]
> <span data-ttu-id="d9fd7-459">如果你熟悉 C#，你可能会假定使用**var**意味着**viewModel**变量是后期绑定。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-459">If you're unfamiliar with C#, you may assume that using **var** means that the **viewModel** variable is late-bound.</span></span> <span data-ttu-id="d9fd7-460">不正确-C# 编译器使用类型推理基于分配给变量来确定， **viewModel**属于类型**StoreIndexViewModel**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-460">That's not correct - the C# compiler is using type-inference based on what you assign to the variable to determine that **viewModel** is of type **StoreIndexViewModel**.</span></span> <span data-ttu-id="d9fd7-461">此外，通过编译本地**viewModel**变量作为**StoreIndexViewModel** get 编译时检查和 Visual Studio 代码编辑器支持类型。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-461">Also, by compiling the local **viewModel** variable as a **StoreIndexViewModel** type you get compile-time checking and Visual Studio code-editor support.</span></span>

<a id="Ex5Task4"></a>

<a id="Task_4_-_Creating_a_View_Template_that_Uses_StoreIndexViewModel"></a>
#### <a name="task-4---creating-a-view-template-that-uses-storeindexviewmodel"></a><span data-ttu-id="d9fd7-462">任务 4-创建使用 StoreIndexViewModel 的视图模板</span><span class="sxs-lookup"><span data-stu-id="d9fd7-462">Task 4 - Creating a View Template that Uses StoreIndexViewModel</span></span>

<span data-ttu-id="d9fd7-463">在此任务中，你将创建将用于从控制器传递 StoreIndexViewModel 对象显示的风格列表视图模板。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-463">In this task, you will create a View template that will use a StoreIndexViewModel object passed from the Controller to display a list of genres.</span></span>

1. <span data-ttu-id="d9fd7-464">在创建新的视图模板之前, 让我们来生成项目，以便**添加视图对话框**就会了解有关**StoreIndexViewModel**类。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-464">Before creating the new View template, let's build the project so that the **Add View Dialog** knows about the **StoreIndexViewModel** class.</span></span> <span data-ttu-id="d9fd7-465">通过选择生成项目**生成**菜单项，然后**生成 MvcMusicStore**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-465">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>

    <span data-ttu-id="d9fd7-466">![生成项目](aspnet-mvc-4-fundamentals/_static/image22.png "生成项目")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-466">![Building the project](aspnet-mvc-4-fundamentals/_static/image22.png "Building the project")</span></span>

    <span data-ttu-id="d9fd7-467">*生成项目*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-467">*Building the project*</span></span>
2. <span data-ttu-id="d9fd7-468">创建一个新的视图模板。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-468">Create a new View template.</span></span> <span data-ttu-id="d9fd7-469">为此，请右键单击内**索引**方法，然后选择**添加视图**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-469">To do that, right-click inside the **Index** method and select **Add View**.</span></span>

    <span data-ttu-id="d9fd7-470">![添加视图](aspnet-mvc-4-fundamentals/_static/image23.png "添加视图")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-470">![Adding a View](aspnet-mvc-4-fundamentals/_static/image23.png "Adding a View")</span></span>

    <span data-ttu-id="d9fd7-471">*添加视图*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-471">*Adding a View*</span></span>
3. <span data-ttu-id="d9fd7-472">因为**添加视图对话框**从调用**StoreController**，它将在默认情况下添加的视图模板**\Views\Store\Index.cshtml**文件。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-472">Because the **Add View Dialog** was invoked from the **StoreController**, it will add the View template by default in a **\Views\Store\Index.cshtml** file.</span></span> <span data-ttu-id="d9fd7-473">检查**创建强类型化的视图**复选框，然后选择**StoreIndexViewModel**作为**模型类**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-473">Check the **Create a strongly-typed-view** checkbox and then select **StoreIndexViewModel** as the **Model class**.</span></span> <span data-ttu-id="d9fd7-474">此外，请确保选择的视图引擎是**Razor**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-474">Also, make sure that the View engine selected is **Razor**.</span></span> <span data-ttu-id="d9fd7-475">单击 **添加**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-475">Click **Add**.</span></span>

    <span data-ttu-id="d9fd7-476">![添加视图对话框](aspnet-mvc-4-fundamentals/_static/image24.png "添加视图对话框")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-476">![Add View Dialog](aspnet-mvc-4-fundamentals/_static/image24.png "Add View Dialog")</span></span>

    <span data-ttu-id="d9fd7-477">*添加视图对话框*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-477">*Add View Dialog*</span></span>

    <span data-ttu-id="d9fd7-478">**\Views\Store\Index.cshtml**创建视图模板文件并将其打开。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-478">The **\Views\Store\Index.cshtml** View template file is created and opened.</span></span> <span data-ttu-id="d9fd7-479">基于提供给信息**添加视图**在最后一个步骤中，模板将预期的视图的对话框**StoreIndexViewModel**实例作为要用来生成 HTML 响应数据。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-479">Based on the information provided to the **Add View** dialog in the last step, the View template will expect a **StoreIndexViewModel** instance as the data to use to generate an HTML response.</span></span> <span data-ttu-id="d9fd7-480">你会发现模板继承`ViewPage<musicstore.viewmodels.storeindexviewmodel>`C# 中。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-480">You will notice that the template inherits a `ViewPage<musicstore.viewmodels.storeindexviewmodel>` in C#.</span></span>

<a id="Ex5Task5"></a>

<a id="Task_5_-_Updating_the_View_Template"></a>
#### <a name="task-5---updating-the-view-template"></a><span data-ttu-id="d9fd7-481">任务 5-更新视图模板</span><span class="sxs-lookup"><span data-stu-id="d9fd7-481">Task 5 - Updating the View Template</span></span>

<span data-ttu-id="d9fd7-482">在此任务中，你将更新来检索数量风格的页内的其名称的最后一个任务中创建的视图模板。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-482">In this task, you will update the View template created in the last task to retrieve the number of genres and their names within the page.</span></span>

> [!NOTE]
> <span data-ttu-id="d9fd7-483">你将使用语法 @ (通常称为&quot;代码问题&quot;) 能够查看模板中执行代码。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-483">You will use @ syntax (often referred to as &quot;code nuggets&quot;) to execute code within the View template.</span></span>

1. <span data-ttu-id="d9fd7-484">在**Index.cshtml**文件内,**存储**文件夹，其代码替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="d9fd7-484">In the **Index.cshtml** file, within the **Store** folder, replace its code with the following:</span></span>

[!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample14.cshtml)]

    > [!NOTE]
    > As soon as you finish typing the period after the word **Model**, Visual Studio's Intellisense will show a list of possible properties and methods to choose from.
    > 
    > ![](aspnet-mvc-4-fundamentals/_static/image25.png)
    > 
    > *Getting Model properties and methods with Visual Studio's IntelliSense*
    > 
    > The **Model** property references the **StoreIndexViewModel** object that the Controller passed to the View template. This means that you can access all of the data passed from the Controller to the View template via the **Model** property, and format it into an appropriate HTML response within the View template.
    > 
    > You can just select the **NumberOfGenres** property from the Intellisense list rather than typing it in and then it will auto-complete it by pressing the **tab key**.
2. <span data-ttu-id="d9fd7-485">循环访问中的 genre 列表**StoreIndexViewModel**并创建一个 HTML **&lt;ul&gt;** 列表使用**foreach**循环。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-485">Loop over the genre list in **StoreIndexViewModel** and create an HTML **&lt;ul&gt;** list using a **foreach** loop.</span></span>
   <span data-ttu-id="d9fd7-486">(C#)</span><span class="sxs-lookup"><span data-stu-id="d9fd7-486">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample15.cshtml)]
3. <span data-ttu-id="d9fd7-487">按**F5**运行该应用程序和浏览**应用商店**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-487">Press **F5** to run the Application and browse **/Store**.</span></span> <span data-ttu-id="d9fd7-488">您将看到的传入的风格列表**StoreIndexViewModel**对象**StoreController**到视图模板。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-488">You will see the list of genres passed in the **StoreIndexViewModel** object from the **StoreController** to the View template.</span></span>

    <span data-ttu-id="d9fd7-489">![显示的风格的列表视图](aspnet-mvc-4-fundamentals/_static/image26.png "视图显示的风格的列表")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-489">![View displaying a list of genres](aspnet-mvc-4-fundamentals/_static/image26.png "View displaying a list of genres")</span></span>

    <span data-ttu-id="d9fd7-490">*显示的风格的列表视图*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-490">*View displaying a list of genres*</span></span>
4. <span data-ttu-id="d9fd7-491">关闭浏览器。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-491">Close the browser.</span></span>

<a id="Exercise6"></a>

<a id="Exercise_6_Using_Parameters_in_View"></a>
### <a name="exercise-6-using-parameters-in-view"></a><span data-ttu-id="d9fd7-492">练习 6： 视图中使用参数</span><span class="sxs-lookup"><span data-stu-id="d9fd7-492">Exercise 6: Using Parameters in View</span></span>

<span data-ttu-id="d9fd7-493">在练习 3 中你学习了如何将参数传递到的控制器。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-493">In Exercise 3 you learned how to pass parameters to the Controller.</span></span> <span data-ttu-id="d9fd7-494">在此练习中，你将了解如何在视图模板中使用这些参数。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-494">In this exercise, you will learn how to use those parameters in the View template.</span></span> <span data-ttu-id="d9fd7-495">出于这个目的，将向你介绍第一次将帮助你管理你的数据和域的逻辑的模型类。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-495">For that purpose, you will be introduced first to Model classes that will help you to manage your data and domain logic.</span></span> <span data-ttu-id="d9fd7-496">此外，您将学习如何创建的 ASP.NET MVC 应用程序中的页面链接安心的编码的 URL 路径等。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-496">Additionally, you will learn how to create links to pages inside the ASP.NET MVC application without worrying of things like URL paths encoding.</span></span>

<a id="Ex6Task1"></a>

<a id="Task_1_-_Adding_Model_Classes"></a>
#### <a name="task-1---adding-model-classes"></a><span data-ttu-id="d9fd7-497">任务 1-添加模型类</span><span class="sxs-lookup"><span data-stu-id="d9fd7-497">Task 1 - Adding Model Classes</span></span>

<span data-ttu-id="d9fd7-498">与 Viewmodel 创建只是为了将从控制器的信息传递给视图，不同模型类而生成包含和管理数据和域的逻辑。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-498">Unlike ViewModels, which are created just to pass information from the Controller to the View, Model classes are built to contain and manage data and domain logic.</span></span> <span data-ttu-id="d9fd7-499">在此任务中，你将添加两个模型类来表示这些概念：**流派**和**唱片集**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-499">In this task you will add two model classes to represent these concepts: **Genre** and **Album**.</span></span>

1. <span data-ttu-id="d9fd7-500">如果尚未打开，启动**VS Express for Web**</span><span class="sxs-lookup"><span data-stu-id="d9fd7-500">If not already open, start **VS Express for Web**</span></span>
2. <span data-ttu-id="d9fd7-501">在**文件**菜单上，选择**打开项目**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-501">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="d9fd7-502">在打开项目对话框中，浏览到**Source\Ex06 UsingParametersInView\Begin**，选择**Begin.sln**单击**打开**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-502">In the Open Project dialog, browse to **Source\Ex06-UsingParametersInView\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="d9fd7-503">或者，你也可以继续使用该解决方案完成上一练习后获得。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-503">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="d9fd7-504">如果你打开提供**开始**解决方案，你将需要下载一些缺少的 NuGet 程序包才能继续。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-504">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="d9fd7-505">若要执行此操作，请单击**项目**菜单，然后选择**管理 NuGet 包**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-505">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="d9fd7-506">在**管理 NuGet 包**对话框中，单击**还原**以便下载缺少的程序包。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-506">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="d9fd7-507">最后，通过单击生成解决方案**生成** | **生成解决方案**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-507">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="d9fd7-508">使用 NuGet 的优点之一是，你无需提供你的项目中的所有库减小项目大小。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-508">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="d9fd7-509">使用 NuGet 增强工具，请通过指定的包版本在 Packages.config 文件中，你将能够下载首次运行该项目的所有所需的库。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-509">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="d9fd7-510">这是你将需要从本实验打开现有的解决方案后运行这些步骤的原因。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-510">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="d9fd7-511">添加**流派**模型类。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-511">Add a **Genre** Model class.</span></span> <span data-ttu-id="d9fd7-512">要执行此操作，请右键单击**模型**文件夹中的**解决方案资源管理器**，选择**添加**然后**新项**选项。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-512">To do this, right-click the **Models** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="d9fd7-513">下**代码**，选择**类**项并将该文件命名*Genre.cs*，然后单击**添加**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-513">Under **Code**, choose the **Class** item and name the file *Genre.cs*, then click **Add**.</span></span>

    <span data-ttu-id="d9fd7-514">![添加类](aspnet-mvc-4-fundamentals/_static/image27.png "添加类")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-514">![Adding a class](aspnet-mvc-4-fundamentals/_static/image27.png "Adding a class")</span></span>

    <span data-ttu-id="d9fd7-515">*添加新项*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-515">*Adding a new item*</span></span>

    <span data-ttu-id="d9fd7-516">![添加流派模型类](aspnet-mvc-4-fundamentals/_static/image28.png "添加流派模型类")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-516">![Add Genre Model Class](aspnet-mvc-4-fundamentals/_static/image28.png "Add Genre Model Class")</span></span>

    <span data-ttu-id="d9fd7-517">*添加流派模型类*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-517">*Add Genre Model Class*</span></span>
4. <span data-ttu-id="d9fd7-518">添加**名称**流派类属性。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-518">Add a **Name** property to the Genre class.</span></span> <span data-ttu-id="d9fd7-519">若要执行此操作，添加以下代码：</span><span class="sxs-lookup"><span data-stu-id="d9fd7-519">To do this, add the following code:</span></span>

    <span data-ttu-id="d9fd7-520">(代码段- *ASP.NET MVC 4 基础-Ex6 流派*)</span><span class="sxs-lookup"><span data-stu-id="d9fd7-520">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 Genre*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample16.cs)]
5. <span data-ttu-id="d9fd7-521">按照相同的过程之前，添加**唱片集**类。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-521">Following the same procedure as before, add an **Album** class.</span></span> <span data-ttu-id="d9fd7-522">要执行此操作，请右键单击**模型**文件夹中的**解决方案资源管理器**，选择**添加**然后**新项**选项。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-522">To do this, right-click the **Models** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="d9fd7-523">下**代码**，选择**类**项并将该文件命名*Album.cs*，然后单击**添加**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-523">Under **Code**, choose the **Class** item and name the file *Album.cs*, then click **Add**.</span></span>
6. <span data-ttu-id="d9fd7-524">将两个属性添加到唱片集类：**流派**和**标题**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-524">Add two properties to the Album class: **Genre** and **Title**.</span></span> <span data-ttu-id="d9fd7-525">若要执行此操作，添加以下代码：</span><span class="sxs-lookup"><span data-stu-id="d9fd7-525">To do this, add the following code:</span></span>

    <span data-ttu-id="d9fd7-526">(代码段- *ASP.NET MVC 4 基础-Ex6 唱片集*)</span><span class="sxs-lookup"><span data-stu-id="d9fd7-526">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 Album*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample17.cs)]

<a id="Ex6Task2"></a>

<a id="Task_2_-_Adding_a_StoreBrowseViewModel"></a>
#### <a name="task-2---adding-a-storebrowseviewmodel"></a><span data-ttu-id="d9fd7-527">任务 2-添加 StoreBrowseViewModel</span><span class="sxs-lookup"><span data-stu-id="d9fd7-527">Task 2 - Adding a StoreBrowseViewModel</span></span>

<span data-ttu-id="d9fd7-528">A **StoreBrowseViewModel**将用于在此任务中显示匹配所选的风格唱片集。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-528">A **StoreBrowseViewModel** will be used in this task to show the Albums that match a selected Genre.</span></span> <span data-ttu-id="d9fd7-529">在此任务中，你将创建此类，然后添加两个属性来处理**流派**及其**唱片集**的列表。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-529">In this task, you will create this class and then add two properties to handle the **Genre** and its **Album**'s List.</span></span>

1. <span data-ttu-id="d9fd7-530">添加**StoreBrowseViewModel**类。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-530">Add a **StoreBrowseViewModel** class.</span></span> <span data-ttu-id="d9fd7-531">要执行此操作，请右键单击**Viewmodel**文件夹中的**解决方案资源管理器**，选择**添加**然后**新项**选项。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-531">To do this, right-click the **ViewModels** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="d9fd7-532">下**代码**，选择**类**项并将该文件命名*StoreBrowseViewModel.cs*，然后单击**添加**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-532">Under **Code**, choose the **Class** item and name the file *StoreBrowseViewModel.cs*, then click **Add**.</span></span>
2. <span data-ttu-id="d9fd7-533">添加对中的模型的引用**StoreBrowseViewModel**类。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-533">Add a reference to the Models in **StoreBrowseViewModel** class.</span></span> <span data-ttu-id="d9fd7-534">若要执行此操作，将添加以下使用命名空间：</span><span class="sxs-lookup"><span data-stu-id="d9fd7-534">To do this, add the following using namespace:</span></span>

    <span data-ttu-id="d9fd7-535">(代码段- *ASP.NET MVC 4 基础-Ex6 UsingModel*)</span><span class="sxs-lookup"><span data-stu-id="d9fd7-535">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 UsingModel*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample18.cs)]
3. <span data-ttu-id="d9fd7-536">添加到两个属性**StoreBrowseViewModel**类：**流派**和**专辑**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-536">Add two properties to **StoreBrowseViewModel** class: **Genre** and **Albums**.</span></span> <span data-ttu-id="d9fd7-537">若要执行此操作，添加以下代码：</span><span class="sxs-lookup"><span data-stu-id="d9fd7-537">To do this, add the following code:</span></span>

    <span data-ttu-id="d9fd7-538">(代码段- *ASP.NET MVC 4 基础-Ex6 ModelProperties*)</span><span class="sxs-lookup"><span data-stu-id="d9fd7-538">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 ModelProperties*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample19.cs)]

> [!NOTE]
> <span data-ttu-id="d9fd7-539">什么是**列表&lt;唱片集&gt;** ？: 使用此定义**列表&lt;T&gt;** 类型，其中**T**约束类型设置为此哪些元素**列表**属于，在这种情况下**唱片集**（或其任意后代）。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-539">What is **List&lt;Album&gt;** ?: This definition is using the **List&lt;T&gt;** type, where **T** constrains the type to which elements of this **List** belong to, in this case **Album** (or any of its descendants).</span></span>
> 
> <span data-ttu-id="d9fd7-540">这种设计类和方法的操作延迟的一个或多个类型的规范的类或方法声明，并且由客户端代码实例化是一项功能的 C# 语言的能力称为**泛型**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-540">This ability to design classes and methods that defer the specification of one or more types until the class or method is declared and instantiated by client code is a feature of the C# language called **Generics**.</span></span>
> 
> <span data-ttu-id="d9fd7-541">**列表&lt;T&gt;** 泛型等效于**ArrayList**键入和位于**System.Collections.Generic**命名空间。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-541">**List&lt;T&gt;** is the generic equivalent of the **ArrayList** type and is available in the **System.Collections.Generic** namespace.</span></span> <span data-ttu-id="d9fd7-542">使用的好处之一**泛型**是，由于指定的类型，不需要处理的类型检查操作，例如强制转换到的元素**唱片集**需要像一样**ArrayList**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-542">One of the benefits of using **generics** is that since the type is specified, you do not need to take care of type checking operations such as casting the elements into **Album** as you would do with an **ArrayList**.</span></span>

<a id="Ex6Task3"></a>

<a id="Task_3_-_Using_the_New_ViewModel_in_the_StoreController"></a>
#### <a name="task-3---using-the-new-viewmodel-in-the-storecontroller"></a><span data-ttu-id="d9fd7-543">任务 3-在 StoreController 中使用新视图模型</span><span class="sxs-lookup"><span data-stu-id="d9fd7-543">Task 3 - Using the New ViewModel in the StoreController</span></span>

<span data-ttu-id="d9fd7-544">在此任务中，你将要修改**StoreController**的**浏览**和**详细信息**操作方法，以使用新**StoreBrowseViewModel**.</span><span class="sxs-lookup"><span data-stu-id="d9fd7-544">In this task, you will modify the **StoreController**'s **Browse** and **Details** action methods to use the new **StoreBrowseViewModel**.</span></span>

1. <span data-ttu-id="d9fd7-545">添加对中的 Models 文件夹的引用**StoreController**类。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-545">Add a reference to the Models folder in **StoreController** class.</span></span> <span data-ttu-id="d9fd7-546">若要执行此操作，展开**控制器**文件夹中的**解决方案资源管理器**并打开**StoreController**类。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-546">To do this, expand the **Controllers** folder in the **Solution Explorer** and open the **StoreController** class.</span></span> <span data-ttu-id="d9fd7-547">然后添加以下代码：</span><span class="sxs-lookup"><span data-stu-id="d9fd7-547">Then add the following code:</span></span>

    <span data-ttu-id="d9fd7-548">(代码段- *ASP.NET MVC 4 基础-Ex6 UsingModelInController*)</span><span class="sxs-lookup"><span data-stu-id="d9fd7-548">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 UsingModelInController*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample20.cs)]
2. <span data-ttu-id="d9fd7-549">替换**浏览**要使用的操作方法**StoreViewBrowseController**类。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-549">Replace the **Browse** action method to use the **StoreViewBrowseController** class.</span></span> <span data-ttu-id="d9fd7-550">你将创建一种风格和两个新的专辑对象具有虚拟数据 （在下一步的动手实验中你将使用数据库中的实际数据）。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-550">You will create a Genre and two new Albums objects with dummy data (in the next Hands-on Lab you will consume real data from a database).</span></span> <span data-ttu-id="d9fd7-551">若要执行此操作，请替换**浏览**方法替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="d9fd7-551">To do this, replace the **Browse** method with the following code:</span></span>

    <span data-ttu-id="d9fd7-552">(代码段- *ASP.NET MVC 4 基础-Ex6 BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="d9fd7-552">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 BrowseMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample21.cs)]
3. <span data-ttu-id="d9fd7-553">替换**详细信息**要使用的操作方法**StoreViewBrowseController**类。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-553">Replace the **Details** action method to use the **StoreViewBrowseController** class.</span></span> <span data-ttu-id="d9fd7-554">你将创建一个新**唱片集**对象返回给**视图**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-554">You will create a new **Album** object to be returned to the **View**.</span></span> <span data-ttu-id="d9fd7-555">若要执行此操作，请替换**详细信息**方法替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="d9fd7-555">To do this, replace the **Details** method with the following code:</span></span>

    <span data-ttu-id="d9fd7-556">(代码段- *ASP.NET MVC 4 基础-Ex6 DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="d9fd7-556">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 DetailsMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample22.cs)]

<a id="Ex6Task4"></a>

<a id="Task_4_-_Adding_a_Browse_View_Template"></a>
#### <a name="task-4---adding-a-browse-view-template"></a><span data-ttu-id="d9fd7-557">任务 4-添加浏览视图模板</span><span class="sxs-lookup"><span data-stu-id="d9fd7-557">Task 4 - Adding a Browse View Template</span></span>

<span data-ttu-id="d9fd7-558">在此任务中，你将添加**浏览**视图，以显示特定流派找到唱片集。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-558">In this task, you will add a **Browse** View to show the Albums found for a specific Genre.</span></span>

1. <span data-ttu-id="d9fd7-559">在创建新的视图模板之前, 应生成项目，以便**添加视图**对话框就会了解有关**ViewModel**要使用的类。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-559">Before creating the new View template, you should build the project so that the **Add View** Dialog knows about the **ViewModel** class to use.</span></span> <span data-ttu-id="d9fd7-560">通过选择生成项目**生成**菜单项，然后**生成 MvcMusicStore**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-560">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>
2. <span data-ttu-id="d9fd7-561">添加**浏览**视图。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-561">Add a **Browse** View.</span></span> <span data-ttu-id="d9fd7-562">要执行此操作，右键单击**浏览**操作方法**StoreController**单击**添加视图**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-562">To do this, right-click in the **Browse** action method of the **StoreController** and click **Add View**.</span></span>
3. <span data-ttu-id="d9fd7-563">在**添加视图**对话框框中，验证是否视图名称**浏览**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-563">In the **Add View** dialog box, verify that the View Name is **Browse**.</span></span> <span data-ttu-id="d9fd7-564">检查**创建强类型化视图**复选框，然后选择**StoreBrowseViewModel**从**模型类**下拉列表。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-564">Check the **Create a strongly-typed view** checkbox and select **StoreBrowseViewModel** from the **Model class** dropdown.</span></span> <span data-ttu-id="d9fd7-565">将其他字段保留其默认值。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-565">Leave the other fields with their default value.</span></span> <span data-ttu-id="d9fd7-566">然后单击 **“添加”**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-566">Then click **Add**.</span></span>

    <span data-ttu-id="d9fd7-567">![添加浏览视图](aspnet-mvc-4-fundamentals/_static/image29.png "添加浏览视图")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-567">![Adding a Browse View](aspnet-mvc-4-fundamentals/_static/image29.png "Adding a Browse View")</span></span>

    <span data-ttu-id="d9fd7-568">*添加浏览视图*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-568">*Adding a Browse View*</span></span>
4. <span data-ttu-id="d9fd7-569">修改**Browse.cshtml**以显示流派的信息，请访问**StoreBrowseViewModel**传递给视图模板对象。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-569">Modify the **Browse.cshtml** to display the Genre's information, accessing the **StoreBrowseViewModel** object that is passed to the view template.</span></span> <span data-ttu-id="d9fd7-570">若要执行此操作，将内容替换为以下: (C#)</span><span class="sxs-lookup"><span data-stu-id="d9fd7-570">To do this, replace the content with the following: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample23.cshtml)]

<a id="Ex6Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="d9fd7-571">任务 5-运行应用程序</span><span class="sxs-lookup"><span data-stu-id="d9fd7-571">Task 5 - Running the Application</span></span>

<span data-ttu-id="d9fd7-572">在此任务中，你将测试**浏览**方法检索从专辑**浏览**方法操作。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-572">In this task, you will test that the **Browse** method retrieves Albums from the **Browse** method action.</span></span>

1. <span data-ttu-id="d9fd7-573">按**F5**运行该应用程序。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-573">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="d9fd7-574">在主页页面中启动项目。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-574">The project starts in the Home page.</span></span> <span data-ttu-id="d9fd7-575">将 URL 更改为  **/存储/浏览？流派 = Disco**以验证操作，返回两个唱片集。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-575">Change the URL to **/Store/Browse?Genre=Disco** to verify that the action returns two Albums.</span></span>

    <span data-ttu-id="d9fd7-576">![浏览应用商店 Disco 专辑](aspnet-mvc-4-fundamentals/_static/image30.png "浏览应用商店 Disco 专辑")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-576">![Browsing Store Disco Albums](aspnet-mvc-4-fundamentals/_static/image30.png "Browsing Store Disco Albums")</span></span>

    <span data-ttu-id="d9fd7-577">*浏览应用商店 Disco 专辑*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-577">*Browsing Store Disco Albums*</span></span>

<a id="Ex6Task6"></a>

<a id="Task_6_-_Displaying_information_About_a_Specific_Album"></a>
#### <a name="task-6---displaying-information-about-a-specific-album"></a><span data-ttu-id="d9fd7-578">任务 6-显示信息有关的特定唱片集</span><span class="sxs-lookup"><span data-stu-id="d9fd7-578">Task 6 - Displaying information About a Specific Album</span></span>

<span data-ttu-id="d9fd7-579">在此任务中，你将实现**商店/详细信息**视图以显示有关特定唱片集的信息。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-579">In this task, you will implement the **Store/Details** view to display information about a specific album.</span></span> <span data-ttu-id="d9fd7-580">在本动手实验中，将显示有关唱片集的所有内容已包含在**视图**模板。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-580">In this Hands-on Lab, everything you will display about the album is already contained in the **View** template.</span></span> <span data-ttu-id="d9fd7-581">这样，而不是创建**StoreDetailsViewModel**类，你将使用当前**StoreBrowseViewModel**将唱片集传递给它的模板。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-581">So, instead of creating a **StoreDetailsViewModel** class, you will use the current **StoreBrowseViewModel** template passing the Album to it.</span></span>

1. <span data-ttu-id="d9fd7-582">关闭浏览器，如果需要可以返回到 Visual Studio 窗口。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-582">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="d9fd7-583">添加新**详细信息**查看**StoreController**的**详细信息**操作方法。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-583">Add a new **Details** view for the **StoreController**'s **Details** action method.</span></span> <span data-ttu-id="d9fd7-584">要执行此操作，请右键单击**详细信息**中的方法**StoreController**类并单击**添加视图**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-584">To do this, right-click the **Details** method in the **StoreController** class and click **Add View**.</span></span>
2. <span data-ttu-id="d9fd7-585">在**添加视图**对话框中，验证**视图名称**是**详细信息**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-585">In the **Add View** dialog, verify that the **View Name** is **Details**.</span></span> <span data-ttu-id="d9fd7-586">检查**创建强类型化视图**复选框，然后选择**唱片集**从**模型类**下拉列表。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-586">Check the **Create a strongly-typed view** checkbox and select **Album** from the **Model class** drop-down.</span></span> <span data-ttu-id="d9fd7-587">将其他字段保留其默认值。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-587">Leave the other fields with their default value.</span></span> <span data-ttu-id="d9fd7-588">然后单击 **“添加”**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-588">Then click **Add**.</span></span> <span data-ttu-id="d9fd7-589">这将创建并打开**\Views\Store\Details.cshtml**文件。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-589">This will create and open a **\Views\Store\Details.cshtml** file.</span></span>

    <span data-ttu-id="d9fd7-590">![添加详细信息视图](aspnet-mvc-4-fundamentals/_static/image31.png "添加详细信息视图")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-590">![Adding a Details View](aspnet-mvc-4-fundamentals/_static/image31.png "Adding a Details View")</span></span>

    <span data-ttu-id="d9fd7-591">*添加详细信息视图*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-591">*Adding a Details View*</span></span>
3. <span data-ttu-id="d9fd7-592">修改**Details.cshtml**文件以显示唱片集的信息，请访问**唱片集**传递给视图模板对象。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-592">Modify the **Details.cshtml** file to display the Album's information, accessing the **Album** object that is passed to the view template.</span></span> <span data-ttu-id="d9fd7-593">若要执行此操作，将内容替换为以下: (C#)</span><span class="sxs-lookup"><span data-stu-id="d9fd7-593">To do this, replace the content with the following: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample24.cshtml)]

<a id="Ex6Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a><span data-ttu-id="d9fd7-594">任务 7-运行应用程序</span><span class="sxs-lookup"><span data-stu-id="d9fd7-594">Task 7 - Running the Application</span></span>

<span data-ttu-id="d9fd7-595">在此任务中，你将测试**详细信息**视图检索唱片集的信息从**详细说明了操作**方法。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-595">In this task, you will test that the **Details** View retrieves Album's information from the **Details action** method.</span></span>

1. <span data-ttu-id="d9fd7-596">按**F5**运行该应用程序。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-596">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="d9fd7-597">在启动项目**主页**页。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-597">The project starts in the **Home** page.</span></span> <span data-ttu-id="d9fd7-598">将 URL 更改为 **/Store/Details/5**验证唱片集的信息。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-598">Change the URL to **/Store/Details/5** to verify the album's information.</span></span>

    <span data-ttu-id="d9fd7-599">![浏览唱片集详细信息](aspnet-mvc-4-fundamentals/_static/image32.png "浏览唱片集详细信息")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-599">![Browsing Albums Detail](aspnet-mvc-4-fundamentals/_static/image32.png "Browsing Albums Detail")</span></span>

    <span data-ttu-id="d9fd7-600">*浏览唱片集的详细信息*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-600">*Browsing Album's Detail*</span></span>

<a id="Ex6Task8"></a>

<a id="Task_8_-_Adding_Links_Between_Pages"></a>
#### <a name="task-8---adding-links-between-pages"></a><span data-ttu-id="d9fd7-601">任务 8-添加页面之间的链接</span><span class="sxs-lookup"><span data-stu-id="d9fd7-601">Task 8 - Adding Links Between Pages</span></span>

<span data-ttu-id="d9fd7-602">在此任务中，你将添加要为相应的每个风格名称中有一个链接的存储区视图中的链接 **/存储/浏览**URL。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-602">In this task, you will add a link in the Store View to have a link in every Genre name to the appropriate **/Store/Browse** URL.</span></span> <span data-ttu-id="d9fd7-603">这样一来，例如单击一种风格时**Disco**，它将会定位到 **/存储/浏览？ 流派 = Disco** URL。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-603">This way, when you click on a Genre, for instance **Disco**, it will navigate to **/Store/Browse?genre=Disco** URL.</span></span>

1. <span data-ttu-id="d9fd7-604">关闭浏览器，如果需要可以返回到 Visual Studio 窗口。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-604">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="d9fd7-605">更新**索引**页后，可以添加一个链接到**浏览**页。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-605">Update the **Index** page to add a link to the **Browse** page.</span></span> <span data-ttu-id="d9fd7-606">为此，请在**解决方案资源管理器**展开**视图**文件夹，则**存储**文件夹并双击**Index.cshtml**页。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-606">To do this, in the **Solution Explorer** expand the **Views** folder, then the **Store** folder and double-click the **Index.cshtml** page.</span></span>
2. <span data-ttu-id="d9fd7-607">指示所选风格浏览视图中添加的链接。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-607">Add a link to the Browse view indicating the genre selected.</span></span> <span data-ttu-id="d9fd7-608">若要执行此操作，请将以下突出显示的代码中**&lt;li&gt;** 标记: (C#)</span><span class="sxs-lookup"><span data-stu-id="d9fd7-608">To do this, replace the following highlighted code within the **&lt;li&gt;** tags: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample25.cshtml)]

   > [!NOTE]
   > <span data-ttu-id="d9fd7-609">另一种方法将直接链接的页，代码如下所示：</span><span class="sxs-lookup"><span data-stu-id="d9fd7-609">another approach would be linking directly to the page, with a code like the following:</span></span>
   > 
   > <span data-ttu-id="d9fd7-610">&lt;href =&quot;/存储/浏览？ 流派 =@genreName&quot;&gt;@genreName &lt; /a&gt;</span><span class="sxs-lookup"><span data-stu-id="d9fd7-610">&lt;a href=&quot;/Store/Browse?genre=@genreName&quot;&gt;@genreName&lt;/a&gt;</span></span>
   > 
   > <span data-ttu-id="d9fd7-611">尽管此方法适用，则它将依赖于硬编码字符串。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-611">Although this approach works, it depends on a hardcoded string.</span></span> <span data-ttu-id="d9fd7-612">如果你更高版本重命名控制器，你将需要手动更改此指令。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-612">If you later rename the Controller, you will have to change this instruction manually.</span></span> <span data-ttu-id="d9fd7-613">更好的替代方法是使用**的 HTML 帮助器**方法。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-613">A better alternative is to use an **HTML Helper** method.</span></span> <span data-ttu-id="d9fd7-614">ASP.NET MVC 包括一个 HTML 帮助器方法，这是可用于此类任务。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-614">ASP.NET MVC includes an HTML Helper method which is available for such tasks.</span></span> <span data-ttu-id="d9fd7-615">**Html.ActionLink()** 帮助器方法，可以轻松地生成 HTML **&lt;&gt;** 链接，并确保 URL 路径正确进行 URL 编码。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-615">The **Html.ActionLink()** helper method makes it easy to build HTML **&lt;a&gt;** links, making sure URL paths are properly URL encoded.</span></span>
   > 
   > <span data-ttu-id="d9fd7-616">Htlm.ActionLink 具有好几个重载。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-616">Htlm.ActionLink has several overloads.</span></span> <span data-ttu-id="d9fd7-617">在本练习中，你将使用一个采用三个参数：</span><span class="sxs-lookup"><span data-stu-id="d9fd7-617">In this exercise you will use one that takes three parameters:</span></span>
   > 
   > 1. <span data-ttu-id="d9fd7-618">链接文本，将显示流派名称</span><span class="sxs-lookup"><span data-stu-id="d9fd7-618">Link text, which will display the Genre name</span></span>
   > 2. <span data-ttu-id="d9fd7-619">控制器操作名称 (**浏览**)</span><span class="sxs-lookup"><span data-stu-id="d9fd7-619">Controller action name (**Browse**)</span></span>
   > 3. <span data-ttu-id="d9fd7-620">路由参数值，指定这两个名称 (**流派**) 和值 (**流派名称**)</span><span class="sxs-lookup"><span data-stu-id="d9fd7-620">Route parameter values, specifying both the name (**Genre**) and the value (**Genre name**)</span></span>

<a id="Ex6Task9"></a>

<a id="Task_9_-_Running_the_Application"></a>
#### <a name="task-9---running-the-application"></a><span data-ttu-id="d9fd7-621">任务 9-运行应用程序</span><span class="sxs-lookup"><span data-stu-id="d9fd7-621">Task 9 - Running the Application</span></span>

<span data-ttu-id="d9fd7-622">在此任务中，你将测试的相应的链接显示了每个风格 **/存储/浏览**URL。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-622">In this task, you will test that each Genre is displayed with a link to the appropriate **/Store/Browse** URL.</span></span>

1. <span data-ttu-id="d9fd7-623">按**F5**运行该应用程序。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-623">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="d9fd7-624">在主页页面中启动项目。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-624">The project starts in the Home page.</span></span> <span data-ttu-id="d9fd7-625">将 URL 更改为**应用商店**验证每个风格链接到相应 **/存储/浏览**URL。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-625">Change the URL to **/Store** to verify that each Genre links to the appropriate **/Store/Browse** URL.</span></span>

    <span data-ttu-id="d9fd7-626">![其中包含指向浏览页浏览风格](aspnet-mvc-4-fundamentals/_static/image33.png "其中包含指向浏览页浏览风格")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-626">![Browsing Genres with links to Browse page](aspnet-mvc-4-fundamentals/_static/image33.png "Browsing Genres with links to Browse page")</span></span>

    <span data-ttu-id="d9fd7-627">*其中包含指向浏览页浏览风格*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-627">*Browsing Genres with links to Browse page*</span></span>

<a id="Ex6Task10"></a>

<a id="Task_10_-_Using_Dynamic_ViewModel_Collection_to_Pass_Values"></a>
#### <a name="task-10---using-dynamic-viewmodel-collection-to-pass-values"></a><span data-ttu-id="d9fd7-628">任务 10-使用动态 ViewModel 集合来传递值</span><span class="sxs-lookup"><span data-stu-id="d9fd7-628">Task 10 - Using Dynamic ViewModel Collection to Pass Values</span></span>

<span data-ttu-id="d9fd7-629">在此任务中，你将学习的简单且功能强大的方法不在模型中进行任何更改的情况下将控制器和视图之间的值。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-629">In this task, you will learn a simple and powerful method to pass values between the Controller and the View without making any changes in the Model.</span></span> <span data-ttu-id="d9fd7-630">ASP.NET MVC 4 提供的集合&quot;ViewModel&quot;，这可以分配给任何动态值并在控制器和视图以及访问。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-630">ASP.NET MVC 4 provides the collection &quot;ViewModel&quot;, which can be assigned to any dynamic value and accessed inside controllers and views as well.</span></span>

<span data-ttu-id="d9fd7-631">你现在将使用 ViewBag 动态集合的列表传递&quot;**星形风格**&quot;从到视图控制器。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-631">You will now use the ViewBag dynamic collection to pass a list of &quot;**Starred genres**&quot; from the controller to the view.</span></span> <span data-ttu-id="d9fd7-632">将访问存储索引视图**ViewModel** ，并显示信息。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-632">The Store Index view will access to **ViewModel** and display the information.</span></span>

1. <span data-ttu-id="d9fd7-633">关闭浏览器，如果需要可以返回到 Visual Studio 窗口。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-633">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="d9fd7-634">打开**StoreController.cs**和修改**索引**方法来创建一份到 ViewModel 集合星形风格：</span><span class="sxs-lookup"><span data-stu-id="d9fd7-634">Open **StoreController.cs** and modify **Index** method to create a list of starred genres into ViewModel collection :</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample26.cs)]

    > [!NOTE]
    > <span data-ttu-id="d9fd7-635">你也可以使用语法**ViewBag [&quot;Starred&quot;]** 访问属性。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-635">You could also use the syntax **ViewBag[&quot;Starred&quot;]** to access the properties.</span></span>
2. <span data-ttu-id="d9fd7-636">星形图标**&quot;starred.png&quot;** 纳入**Source\Assets\Images**这个实验室的文件夹。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-636">The star icon **&quot;starred.png&quot;** is included in the **Source\Assets\Images** folder of this lab.</span></span> <span data-ttu-id="d9fd7-637">要将其添加到应用程序，将从其内容**Windows 资源管理器**到窗口**解决方案资源管理器**Visual Web Developer 学习版中，如下所示：</span><span class="sxs-lookup"><span data-stu-id="d9fd7-637">In order to add it to the application, drag their content from a **Windows Explorer** window into the **Solution Explorer** in Visual Web Developer Express, as shown below:</span></span>

    <span data-ttu-id="d9fd7-638">![添加到解决方案的星型映像](aspnet-mvc-4-fundamentals/_static/image34.png "添加到解决方案的星型映像")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-638">![Adding star image to the solution](aspnet-mvc-4-fundamentals/_static/image34.png "Adding star image to the solution")</span></span>

    <span data-ttu-id="d9fd7-639">*将星型映像添加到解决方案*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-639">*Adding star image to the solution*</span></span>
3. <span data-ttu-id="d9fd7-640">打开视图**Store/Index.cshtml**并修改的内容。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-640">Open the view **Store/Index.cshtml** and modify the content.</span></span> <span data-ttu-id="d9fd7-641">将读取&quot;星形&quot;中的属性**ViewBag**集合，并要求当前流派名称是否在列表中。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-641">You will read the &quot;starred&quot; property in the **ViewBag** collection, and ask if the current genre name is in the list.</span></span> <span data-ttu-id="d9fd7-642">在这种情况下将显示从右到流派链接星形图标。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-642">In that case you will show a star icon right to the genre link.</span></span>
   <span data-ttu-id="d9fd7-643">(C#)</span><span class="sxs-lookup"><span data-stu-id="d9fd7-643">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample27.cshtml)]

<a id="Ex6Task11"></a>

<a id="Task_11_-_Running_the_Application"></a>
#### <a name="task-11---running-the-application"></a><span data-ttu-id="d9fd7-644">任务 11-运行应用程序</span><span class="sxs-lookup"><span data-stu-id="d9fd7-644">Task 11 - Running the Application</span></span>

<span data-ttu-id="d9fd7-645">在此任务中，你将测试标有星号的风格显示星形图标。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-645">In this task, you will test that the starred genres display a star icon.</span></span>

1. <span data-ttu-id="d9fd7-646">按**F5**运行该应用程序。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-646">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="d9fd7-647">在启动项目**主页**页。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-647">The project starts in the **Home** page.</span></span> <span data-ttu-id="d9fd7-648">将 URL 更改为**应用商店**以验证每个特色的风格有 respecting 标签：</span><span class="sxs-lookup"><span data-stu-id="d9fd7-648">Change the URL to **/Store** to verify that each featured genre has the respecting label:</span></span>

    <span data-ttu-id="d9fd7-649">![使用标有星号元素浏览风格](aspnet-mvc-4-fundamentals/_static/image35.png "浏览风格标有星号的元素")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-649">![Browsing Genres with starred elements](aspnet-mvc-4-fundamentals/_static/image35.png "Browsing Genres with starred elements")</span></span>

    <span data-ttu-id="d9fd7-650">*使用标有星号元素浏览风格*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-650">*Browsing Genres with starred elements*</span></span>

<a id="Exercise7"></a>

<a id="Exercise_7_A_lap_around_ASPNET_MVC_4_new_template"></a>
### <a name="exercise-7-a-lap-around-aspnet-mvc-4-new-template"></a><span data-ttu-id="d9fd7-651">练习 7: ASP.NET MVC 4 新模板浏览</span><span class="sxs-lookup"><span data-stu-id="d9fd7-651">Exercise 7: A lap around ASP.NET MVC 4 new template</span></span>

<span data-ttu-id="d9fd7-652">在此练习中，您将了解在 ASP.NET MVC 4 项目模板中的增强功能讲到新模板的最相关的功能。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-652">In this exercise, you will explore the enhancements in the ASP.NET MVC 4 project templates, taking a look at the most relevant features of the new template.</span></span>

<a id="Ex7Task1"></a>

<a id="Task_1_Exploring_the_ASPNET_MVC_4_Internet_Application_Template"></a>
#### <a name="task-1-exploring-the-aspnet-mvc-4-internet-application-template"></a><span data-ttu-id="d9fd7-653">任务 1： 浏览 ASP.NET MVC 4 Internet 应用程序模板</span><span class="sxs-lookup"><span data-stu-id="d9fd7-653">Task 1: Exploring the ASP.NET MVC 4 Internet Application Template</span></span>

1. <span data-ttu-id="d9fd7-654">如果尚未打开，启动**VS Express for Web**</span><span class="sxs-lookup"><span data-stu-id="d9fd7-654">If not already open, start **VS Express for Web**</span></span>
2. <span data-ttu-id="d9fd7-655">选择**文件 |新 |项目**菜单命令。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-655">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="d9fd7-656">在**新项目**对话框中，选择**Visual C# |Web**左窗格中的模板树，并选择**ASP.NET MVC 4 Web 应用程序**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-656">In the **New Project** dialog, select the **Visual C#|Web** template on the left pane tree, and choose the **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="d9fd7-657">**名称**项目*MusicStore*和更新**解决方案名称**到*开始*，然后选择一个位置 （或保留默认值），然后单击**确定**.</span><span class="sxs-lookup"><span data-stu-id="d9fd7-657">**Name** the project *MusicStore* and update the **solution name** to *Begin*, then select a location (or leave the default) and click **OK**.</span></span>

    <span data-ttu-id="d9fd7-658">![创建新的 ASP.NET MVC 4 项目](aspnet-mvc-4-fundamentals/_static/image36.png "创建新的 ASP.NET MVC 4 项目")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-658">![Creating a new ASP.NET MVC 4 Project](aspnet-mvc-4-fundamentals/_static/image36.png "Creating a new ASP.NET MVC 4 Project")</span></span>

    <span data-ttu-id="d9fd7-659">*创建新的 ASP.NET MVC 4 项目*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-659">*Creating a new ASP.NET MVC 4 Project*</span></span>
3. <span data-ttu-id="d9fd7-660">在**新建 ASP.NET MVC 4 项目**对话框中，选择**Internet 应用程序**项目模板，然后单击**确定**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-660">In the **New ASP.NET MVC 4 Project** dialog, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="d9fd7-661">请注意你可以选择 Razor 或 ASPX 作为视图引擎。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-661">Notice you can select either Razor or ASPX as the view engine.</span></span>

    <span data-ttu-id="d9fd7-662">![新建 ASP.NET MVC 4 Internet 应用程序](aspnet-mvc-4-fundamentals/_static/image37.png "新建 ASP.NET MVC 4 Internet 应用程序")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-662">![Creating a new ASP.NET MVC 4 Internet Application](aspnet-mvc-4-fundamentals/_static/image37.png "Creating a new ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="d9fd7-663">*新建 ASP.NET MVC 4 Internet 应用程序*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-663">*Creating a new ASP.NET MVC 4 Internet Application*</span></span>

    > [!NOTE]
    > <span data-ttu-id="d9fd7-664">ASP.NET MVC 3 中引入了 razor 语法。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-664">Razor syntax has been introduced in ASP.NET MVC 3.</span></span> <span data-ttu-id="d9fd7-665">其目标是字符和在文件中，需要启用快速和流体编码工作流的击键的数量降至最低。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-665">Its goal is to minimize the number of characters and keystrokes required in a file, enabling a fast and fluid coding workflow.</span></span> <span data-ttu-id="d9fd7-666">Razor 利用现有 C# /VB （或其他） 语言技能和提供使出色的 HTML 构造流的模板标记语法。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-666">Razor leverages existing C#/VB (or other) language skills and delivers a template markup syntax that enables an awesome HTML construction workflow.</span></span>
4. <span data-ttu-id="d9fd7-667">按**F5**若要运行解决方案并查看续订的模板。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-667">Press **F5** to run the solution and see the renewed template.</span></span> <span data-ttu-id="d9fd7-668">你可以查看以下功能：</span><span class="sxs-lookup"><span data-stu-id="d9fd7-668">You can check out the following features:</span></span>

    1. <span data-ttu-id="d9fd7-669">**现代样式模板**</span><span class="sxs-lookup"><span data-stu-id="d9fd7-669">**Modern-style templates**</span></span>

        <span data-ttu-id="d9fd7-670">已续订的模板，提供更多的外观现代的样式。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-670">The templates have been renewed, providing more modern-looking styles.</span></span>

        <span data-ttu-id="d9fd7-671">![ASP.NET MVC 4 其样式模板](aspnet-mvc-4-fundamentals/_static/image38.png "其样式模板的 ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-671">![ASP.NET MVC 4 restyled templates](aspnet-mvc-4-fundamentals/_static/image38.png "ASP.NET MVC 4 restyled templates")</span></span>

        <span data-ttu-id="d9fd7-672">*ASP.NET MVC 4 其样式模板*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-672">*ASP.NET MVC 4 restyled templates*</span></span>
    2. <span data-ttu-id="d9fd7-673">**自适应呈现**</span><span class="sxs-lookup"><span data-stu-id="d9fd7-673">**Adaptive Rendering**</span></span>

        <span data-ttu-id="d9fd7-674">签出调整浏览器窗口的大小，请注意如何页面布局可以动态地适应新的窗口大小。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-674">Check out resizing the browser window and notice how the page layout dynamically adapts to the new window size.</span></span> <span data-ttu-id="d9fd7-675">这些模板使用的自适应呈现技术在没有任何自定义项的桌面和移动平台中正确呈现。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-675">These templates use the adaptive rendering technique to render properly in both desktop and mobile platforms without any customization.</span></span>

        <span data-ttu-id="d9fd7-676">![在不同的浏览器大小中的 ASP.NET MVC 4 项目模板](aspnet-mvc-4-fundamentals/_static/image39.png "中不同的浏览器大小的 ASP.NET MVC 4 项目模板")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-676">![ASP.NET MVC 4 project template in different browser sizes](aspnet-mvc-4-fundamentals/_static/image39.png "ASP.NET MVC 4 project template in different browser sizes")</span></span>

        <span data-ttu-id="d9fd7-677">*在不同的浏览器大小中的 ASP.NET MVC 4 项目模板*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-677">*ASP.NET MVC 4 project template in different browser sizes*</span></span>
5. <span data-ttu-id="d9fd7-678">关闭浏览器以停止调试器并返回到 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-678">Close the browser to stop the debugger and return to Visual Studio.</span></span>
6. <span data-ttu-id="d9fd7-679">现在，你就能够浏览解决方案和签出引入的一些新功能由 ASP.NET MVC 4 项目模板中。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-679">Now you are able to explore the solution and check out some of the new features introduced by ASP.NET MVC 4 in the project template.</span></span>

    <span data-ttu-id="d9fd7-680">![ASP.NET MVC4-internet 的应用程序的项目的模板](aspnet-mvc-4-fundamentals/_static/image40.png "ASP.NET MVC 4 Internet 应用程序项目模板")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-680">![ASP.NET MVC4-internet-application-project-template](aspnet-mvc-4-fundamentals/_static/image40.png "The ASP.NET MVC 4 Internet Application Project Template")</span></span>

    <span data-ttu-id="d9fd7-681">*ASP.NET MVC 4 Internet 应用程序项目模板*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-681">*The ASP.NET MVC 4 Internet Application Project Template*</span></span>

   1. <span data-ttu-id="d9fd7-682">**HTML5 标记**</span><span class="sxs-lookup"><span data-stu-id="d9fd7-682">**HTML5 markup**</span></span>

       <span data-ttu-id="d9fd7-683">浏览模板视图若要了解的新的主题标记，例如打开**About.cshtml**内查看**主页**文件夹。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-683">Browse template views to find out the new theme markup, for example open **About.cshtml** view within **Home** folder.</span></span>

       <span data-ttu-id="d9fd7-684">![使用 Razor 和 HTML5 标记的新模板](aspnet-mvc-4-fundamentals/_static/image41.png "使用 Razor 和 HTML5 标记的新模板")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-684">![New template, using Razor and HTML5 markup](aspnet-mvc-4-fundamentals/_static/image41.png "New template, using Razor and HTML5 markup")</span></span>

       <span data-ttu-id="d9fd7-685">*使用 Razor 和 HTML5 标记的新模板*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-685">*New template, using Razor and HTML5 markup*</span></span>
   2. <span data-ttu-id="d9fd7-686">**包含的 JavaScript 库**</span><span class="sxs-lookup"><span data-stu-id="d9fd7-686">**JavaScript libraries included**</span></span>

      1. <span data-ttu-id="d9fd7-687">**jQuery**: jQuery 简化了 HTML 文档遍历、 事件处理，对进行动画处理，和 Ajax 交互。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-687">**jQuery**: jQuery simplifies HTML document traversing, event handling, animating, and Ajax interactions.</span></span>
      2. <span data-ttu-id="d9fd7-688">**jQuery UI**： 此库提供低级别交互和动画，高级效果和受主题影响小组件，基于 jQuery JavaScript 库的抽象。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-688">**jQuery UI**: This library provides abstractions for low-level interaction and animation, advanced effects and themeable widgets, built on top of the jQuery JavaScript Library.</span></span>

         > [!NOTE]
         > <span data-ttu-id="d9fd7-689">你可以了解有关 jQuery 和 jQuery UI 中[ [ http://docs.jquery.com/ ](http://docs.jquery.com/) ](http://docs.jquery.com/)。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-689">You can learn about jQuery and jQuery UI in [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span></span>
      3. <span data-ttu-id="d9fd7-690">**KnockoutJS**: ASP.NET MVC 4 默认模板现在包括**KnockoutJS**，JavaScript MVVM 框架，它允许你创建使用 JavaScript 和 HTML 的丰富且响应度高的 web 应用程序。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-690">**KnockoutJS**: The ASP.NET MVC 4 default template now includes **KnockoutJS**, a JavaScript MVVM framework that lets you create rich and highly responsive web applications using JavaScript and HTML.</span></span> <span data-ttu-id="d9fd7-691">如在 ASP.NET MVC 3 中，jQuery 和 jQuery UI 库也包括在 ASP.NET MVC 4。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-691">Like in ASP.NET MVC 3, jQuery and jQuery UI libraries are also included in ASP.NET MVC 4.</span></span>

          > [!NOTE]
          > <span data-ttu-id="d9fd7-692">你可以获取有关此链接中的 KnockOutJS 库的详细信息： [ http://learn.knockoutjs.com/ ](http://learn.knockoutjs.com/)。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-692">You can get more information about KnockOutJS library in this link: [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/).</span></span>
      4. <span data-ttu-id="d9fd7-693">**Modernizr**： 此库自动运行，使您的网站与较旧的浏览器兼容，在使用 HTML5 和 CSS3 技术时。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-693">**Modernizr**: This library runs automatically, making your site compatible with older browsers when using HTML5 and CSS3 technologies.</span></span>

          > [!NOTE]
          > <span data-ttu-id="d9fd7-694">你可以获取有关此链接中的 Modernizr 库的详细信息： [ http://www.modernizr.com/ ](http://www.modernizr.com/)。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-694">You can get more information about Modernizr library in this link: [http://www.modernizr.com/](http://www.modernizr.com/).</span></span>
   3. <span data-ttu-id="d9fd7-695">**解决方案中包含的 SimpleMembership**</span><span class="sxs-lookup"><span data-stu-id="d9fd7-695">**SimpleMembership included in the solution**</span></span>

       <span data-ttu-id="d9fd7-696">SimpleMembership 旨在以取代以前的 ASP.NET 角色和成员资格提供程序系统。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-696">SimpleMembership has been designed as a replacement for the previous ASP.NET Role and Membership provider system.</span></span> <span data-ttu-id="d9fd7-697">它具有许多新功能，能够更轻松地开发人员的安全 web 页面以更灵活的方式。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-697">It has many new features that make it easier for the developer to secure web pages in a more flexible way.</span></span>

       <span data-ttu-id="d9fd7-698">Internet 模板已具有设置需要集成 SimpleMembership 的一些事项，例如，AccountController 中已准备好使用 OAuthWebSecurity （适用于 OAuth 帐户注册、 登录、 管理等） 和 Web 安全。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-698">The Internet template already has set up a few things to integrate SimpleMembership, for example, the AccountController is prepared to use OAuthWebSecurity (for OAuth account registration, login, management, etc.) and Web Security.</span></span>

       <span data-ttu-id="d9fd7-699">![解决方案中包含 SimpleMembership](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership 解决方案中包含")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-699">![SimpleMembership Included in the solution](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership Included in the solution")</span></span>

       <span data-ttu-id="d9fd7-700">*解决方案中包含 SimpleMembership*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-700">*SimpleMembership Included in the solution*</span></span>

       > [!NOTE]
       > <span data-ttu-id="d9fd7-701">查找更多有关[OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) MSDN 中。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-701">Find more information about [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) in MSDN.</span></span>

> [!NOTE]
> <span data-ttu-id="d9fd7-702">此外，你可以部署此应用程序对 Windows Azure 网站以下[附录 b： 发布 ASP.NET MVC 4 应用程序使用 Web Deploy](#AppendixB)。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-702">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="d9fd7-703">总结</span><span class="sxs-lookup"><span data-stu-id="d9fd7-703">Summary</span></span>

<span data-ttu-id="d9fd7-704">通过完成本动手实验中，你已经学习了 ASP.NET MVC 的基础知识：</span><span class="sxs-lookup"><span data-stu-id="d9fd7-704">By completing this Hands-On Lab you have learned the fundamentals of ASP.NET MVC:</span></span>

- <span data-ttu-id="d9fd7-705">核心元素的 MVC 应用程序和它们交互的方式</span><span class="sxs-lookup"><span data-stu-id="d9fd7-705">The core elements of an MVC application and how they interact</span></span>
- <span data-ttu-id="d9fd7-706">如何创建 ASP.NET MVC 应用程序</span><span class="sxs-lookup"><span data-stu-id="d9fd7-706">How to create an ASP.NET MVC Application</span></span>
- <span data-ttu-id="d9fd7-707">如何添加和配置控制器来处理参数传递的 URL 和查询字符串</span><span class="sxs-lookup"><span data-stu-id="d9fd7-707">How to add and configure Controllers to handle parameters passed through the URL and querystring</span></span>
- <span data-ttu-id="d9fd7-708">如何添加布局的母版页设置有关常见的 HTML 内容，以增强外观和感觉和查看模板，以显示 HTML 内容的样式表模板</span><span class="sxs-lookup"><span data-stu-id="d9fd7-708">How to add a layout master page to setup a template for common HTML content, a StyleSheet to enhance the look and feel and a View template to display HTML content</span></span>
- <span data-ttu-id="d9fd7-709">如何使用用于将属性传递给视图模板 ViewModel 模式显示动态信息</span><span class="sxs-lookup"><span data-stu-id="d9fd7-709">How to use the ViewModel pattern for passing properties to the View template to display dynamic information</span></span>
- <span data-ttu-id="d9fd7-710">如何使用传递给控制器视图模板中的参数</span><span class="sxs-lookup"><span data-stu-id="d9fd7-710">How to use parameters passed to Controllers in the View template</span></span>
- <span data-ttu-id="d9fd7-711">如何将链接添加到 ASP.NET MVC 应用程序中的页面</span><span class="sxs-lookup"><span data-stu-id="d9fd7-711">How to add links to pages inside the ASP.NET MVC application</span></span>
- <span data-ttu-id="d9fd7-712">如何添加和使用在视图中的动态属性</span><span class="sxs-lookup"><span data-stu-id="d9fd7-712">How to add and use dynamic properties in a View</span></span>
- <span data-ttu-id="d9fd7-713">ASP.NET MVC 4 项目模板中的增强功能</span><span class="sxs-lookup"><span data-stu-id="d9fd7-713">The enhancements in the ASP.NET MVC 4 project templates</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="d9fd7-714">附录 a： 安装 Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="d9fd7-714">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="d9fd7-715">你可以安装**Microsoft Visual Studio Express 2012 for Web**或另一个&quot;Express&quot;版本使用 **[Microsoft Web 平台安装程序](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="d9fd7-715">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="d9fd7-716">以下说明将指导你完成安装所需的步骤*Visual studio Express 2012 for Web*使用*Microsoft Web 平台安装程序*。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-716">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="d9fd7-717">转到[ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169)。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-717">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="d9fd7-718">或者，如果你已安装 Web 平台安装程序，你可以打开它，并搜索产品&quot; <em>Visual Studio Express 2012 for Web 的 Windows Azure SDK</em>&quot;。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-718">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="d9fd7-719">单击**立即安装**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-719">Click on **Install Now**.</span></span> <span data-ttu-id="d9fd7-720">如果你没有**Web 平台安装程序**将重定向以下载并请先安装它。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-720">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="d9fd7-721">一次**Web 平台安装程序**处于打开状态，单击**安装**以启动安装程序。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-721">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="d9fd7-722">![安装 Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "安装 Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-722">![Install Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="d9fd7-723">*安装 Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-723">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="d9fd7-724">阅读所有产品的许可证和条款，然后单击**我接受**以继续。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-724">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![接受许可条款](aspnet-mvc-4-fundamentals/_static/image44.png)

    <span data-ttu-id="d9fd7-726">*接受许可条款*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-726">*Accepting the license terms*</span></span>
5. <span data-ttu-id="d9fd7-727">等待，直到下载和安装过程完成。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-727">Wait until the downloading and installation process completes.</span></span>

    ![安装进度](aspnet-mvc-4-fundamentals/_static/image45.png)

    <span data-ttu-id="d9fd7-729">*安装进度*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-729">*Installation progress*</span></span>
6. <span data-ttu-id="d9fd7-730">当安装完成后时，单击**完成**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-730">When the installation completes, click **Finish**.</span></span>

    ![安装已完成](aspnet-mvc-4-fundamentals/_static/image46.png)

    <span data-ttu-id="d9fd7-732">*安装已完成*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-732">*Installation completed*</span></span>
7. <span data-ttu-id="d9fd7-733">单击**退出**以关闭 Web 平台安装程序。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-733">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="d9fd7-734">若要打开 Visual Studio Express for Web，请转到**启动**屏幕并开始编写&quot; **VS Express**&quot;，然后单击**VS Express for Web**磁贴。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-734">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Web 磁贴的 VS Express](aspnet-mvc-4-fundamentals/_static/image47.png)

    <span data-ttu-id="d9fd7-736">*Web 磁贴的 VS Express*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-736">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="d9fd7-737">附录 b： 发布 ASP.NET MVC 4 应用程序使用 Web 部署</span><span class="sxs-lookup"><span data-stu-id="d9fd7-737">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="d9fd7-738">本附录将演示如何从 Windows Azure 管理门户创建新的网站和发布应用程序获取按照本实验中，利用 Windows Azure 提供的 Web 部署发布功能。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-738">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="d9fd7-739">任务 1-创建新的 Web 站点从 Windows Azure 门户</span><span class="sxs-lookup"><span data-stu-id="d9fd7-739">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="d9fd7-740">转到[Windows Azure 管理门户](https://manage.windowsazure.com/)并使用与你的订阅关联的 Microsoft 凭据登录。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-740">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d9fd7-741">使用 Windows Azure 可以免费承载 10 个 ASP.NET 网站，然后随着流量增长而扩展。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-741">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="d9fd7-742">你可以注册[此处](http://aka.ms/aspnet-hol-azure)。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-742">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="d9fd7-743">![登录到 Windows Azure 门户](aspnet-mvc-4-fundamentals/_static/image48.png "登录到 Windows Azure 门户")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-743">![Log on to Windows Azure portal](aspnet-mvc-4-fundamentals/_static/image48.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="d9fd7-744">*登录到 Windows Azure 管理门户*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-744">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="d9fd7-745">单击**新建**命令栏上。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-745">Click **New** on the command bar.</span></span>

    <span data-ttu-id="d9fd7-746">![创建新网站](aspnet-mvc-4-fundamentals/_static/image49.png "创建新网站")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-746">![Creating a new Web Site](aspnet-mvc-4-fundamentals/_static/image49.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="d9fd7-747">*创建新网站*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-747">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="d9fd7-748">单击**计算** | **网站**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-748">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="d9fd7-749">然后选择**快速创建**选项。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-749">Then select **Quick Create** option.</span></span> <span data-ttu-id="d9fd7-750">为新网站提供可用的 URL，然后单击**创建网站**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-750">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d9fd7-751">Windows Azure 网站是在云中，你可以控制和管理运行的 web 应用程序的宿主。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-751">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="d9fd7-752">快速创建选项，可将已完成的 web 应用程序到 Windows Azure 网站从在门户外部部署。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-752">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="d9fd7-753">它不包括用于设置数据库的步骤。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-753">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="d9fd7-754">![创建新的网站使用快速创建](aspnet-mvc-4-fundamentals/_static/image50.png "创建新的网站使用快速创建")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-754">![Creating a new Web Site using Quick Create](aspnet-mvc-4-fundamentals/_static/image50.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="d9fd7-755">*创建新的网站使用快速创建*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-755">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="d9fd7-756">等到新**网站**创建。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-756">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="d9fd7-757">创建网站后单击下的链接**URL**列。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-757">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="d9fd7-758">检查新的 Web 站点工作。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-758">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="d9fd7-759">![浏览到新的 web 站点](aspnet-mvc-4-fundamentals/_static/image51.png "浏览到新的 web 站点")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-759">![Browsing to the new web site](aspnet-mvc-4-fundamentals/_static/image51.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="d9fd7-760">*浏览到新的 web 站点*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-760">*Browsing to the new web site*</span></span>

    <span data-ttu-id="d9fd7-761">![运行网站](aspnet-mvc-4-fundamentals/_static/image52.png "运行的网站")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-761">![Web site running](aspnet-mvc-4-fundamentals/_static/image52.png "Web site running")</span></span>

    <span data-ttu-id="d9fd7-762">*运行的网站*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-762">*Web site running*</span></span>
6. <span data-ttu-id="d9fd7-763">返回到门户并单击在网站的名称**名称**列以显示的管理页。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-763">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="d9fd7-764">![打开网站管理页](aspnet-mvc-4-fundamentals/_static/image53.png "打开网站管理页")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-764">![Opening the web site management pages](aspnet-mvc-4-fundamentals/_static/image53.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="d9fd7-765">*打开网站管理页*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-765">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="d9fd7-766">在**仪表板**页上，在**速览**部分中，单击**下载发布配置文件**链接。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-766">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d9fd7-767">*发布配置文件*包含所有 web 应用程序发布到 Windows Azure 网站中的每个启用的发布方法所需的信息。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-767">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="d9fd7-768">发布配置文件包含的 Url、 用户凭据和连接到并针对每个发布方法启用的终结点进行身份验证所需的数据库字符串。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-768">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="d9fd7-769">**Microsoft WebMatrix 2**， **Microsoft Visual Studio Express for Web**和**Microsoft Visual Studio 2012**支持读取发布配置文件以自动执行的这些程序，以便配置web 应用程序发布到 Windows Azure 网站。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-769">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="d9fd7-770">![下载网站发布配置文件](aspnet-mvc-4-fundamentals/_static/image54.png "下载网站发布配置文件")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-770">![Downloading the web site publish profile](aspnet-mvc-4-fundamentals/_static/image54.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="d9fd7-771">*下载网站发布配置文件*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-771">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="d9fd7-772">到一个已知位置下载发布配置文件。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-772">Download the publish profile file to a known location.</span></span> <span data-ttu-id="d9fd7-773">进一步在本练习中您将了解如何使用此文件来发布 Visual Studio 中的 web 应用程序到 Windows Azure 网站。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-773">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="d9fd7-774">![保存发布配置文件](aspnet-mvc-4-fundamentals/_static/image55.png "保存发布配置文件")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-774">![Saving the publish profile file](aspnet-mvc-4-fundamentals/_static/image55.png "Saving the publish profile")</span></span>

    <span data-ttu-id="d9fd7-775">*保存发布配置文件*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-775">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="d9fd7-776">任务 2-配置数据库服务器</span><span class="sxs-lookup"><span data-stu-id="d9fd7-776">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="d9fd7-777">如果你的应用程序将使用 SQL Server 数据库将需要创建 SQL 数据库服务器。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-777">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="d9fd7-778">如果你想要部署的简单应用程序不使用 SQL Server 可能会跳过此任务。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-778">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="d9fd7-779">需要将用于存储应用程序数据库的 SQL 数据库服务器。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-779">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="d9fd7-780">你可以从你的订阅在 Windows Azure 管理门户中查看 SQL 数据库服务器**Sql 数据库** | **服务器** | **服务器的仪表板**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-780">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="d9fd7-781">如果你没有创建的服务器，则可以创建一个使用**添加**命令栏上的按钮。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-781">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="d9fd7-782">请记下的**服务器名称和 URL、 管理员登录名和密码**，如你将在接下来的任务中使用它们。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-782">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="d9fd7-783">不创建数据库，它将在后面的阶段中创建。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-783">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="d9fd7-784">![SQL 数据库服务器仪表板](aspnet-mvc-4-fundamentals/_static/image56.png "SQL Database 服务器仪表板")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-784">![SQL Database Server Dashboard](aspnet-mvc-4-fundamentals/_static/image56.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="d9fd7-785">*SQL 数据库服务器仪表板*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-785">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="d9fd7-786">在下一个任务将测试从 Visual Studio 中，数据库连接，因此你需要在的服务器的列表中包括你的本地 IP 地址**允许的 IP 地址**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-786">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="d9fd7-787">若要做到这一点，请单击**配置**，选择的 IP 地址从**当前客户端 IP 地址**并将其粘贴在**起始 IP 地址**和**结束 IP 地址**文本框和单击![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png)按钮。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-787">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) button.</span></span>

    ![添加客户端 IP 地址](aspnet-mvc-4-fundamentals/_static/image58.png)

    <span data-ttu-id="d9fd7-789">*添加客户端 IP 地址*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-789">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="d9fd7-790">一次**客户端 IP 地址**添加到允许的 IP 地址列表中，单击**保存**以确认所做的更改。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-790">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![确认做的更改](aspnet-mvc-4-fundamentals/_static/image59.png)

    <span data-ttu-id="d9fd7-792">*确认做的更改*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-792">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="d9fd7-793">任务 3-发布 ASP.NET MVC 4 应用程序使用 Web 部署</span><span class="sxs-lookup"><span data-stu-id="d9fd7-793">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="d9fd7-794">返回到 ASP.NET MVC 4 解决方案。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-794">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="d9fd7-795">在**解决方案资源管理器**，右键单击网站项目，然后选择**发布**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-795">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="d9fd7-796">![发布应用程序](aspnet-mvc-4-fundamentals/_static/image60.png "发布应用程序")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-796">![Publishing the Application](aspnet-mvc-4-fundamentals/_static/image60.png "Publishing the Application")</span></span>

    <span data-ttu-id="d9fd7-797">*发布此网站*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-797">*Publishing the web site*</span></span>
2. <span data-ttu-id="d9fd7-798">导入发布配置文件保存在第一个任务。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-798">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="d9fd7-799">![导入发布配置文件](aspnet-mvc-4-fundamentals/_static/image61.png "导入发布配置文件")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-799">![Importing the publish profile](aspnet-mvc-4-fundamentals/_static/image61.png "Importing the publish profile")</span></span>

    <span data-ttu-id="d9fd7-800">*导入发布配置文件*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-800">*Importing publish profile*</span></span>
3. <span data-ttu-id="d9fd7-801">单击**验证连接**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-801">Click **Validate Connection**.</span></span> <span data-ttu-id="d9fd7-802">验证完成后单击**下一步**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-802">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d9fd7-803">请参阅验证连接按钮旁边将显示绿色的复选标记后，验证已完成。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-803">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="d9fd7-804">![正在验证连接](aspnet-mvc-4-fundamentals/_static/image62.png "验证连接")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-804">![Validating connection](aspnet-mvc-4-fundamentals/_static/image62.png "Validating connection")</span></span>

    <span data-ttu-id="d9fd7-805">*正在验证连接*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-805">*Validating connection*</span></span>
4. <span data-ttu-id="d9fd7-806">在**设置**页上，在**数据库**部分中，单击你的数据库连接的文本框旁边的按钮 (即**DefaultConnection**)。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-806">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="d9fd7-807">![Web 部署配置](aspnet-mvc-4-fundamentals/_static/image63.png "Web 部署配置")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-807">![Web deploy configuration](aspnet-mvc-4-fundamentals/_static/image63.png "Web deploy configuration")</span></span>

    <span data-ttu-id="d9fd7-808">*Web 部署配置*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-808">*Web deploy configuration*</span></span>
5. <span data-ttu-id="d9fd7-809">配置数据库连接，如下所示：</span><span class="sxs-lookup"><span data-stu-id="d9fd7-809">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="d9fd7-810">在**服务器名称**类型 SQL 数据库服务器 URL 使用*tcp:* 前缀。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-810">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="d9fd7-811">在**用户名**键入您的服务器管理员登录名。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-811">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="d9fd7-812">在**密码**键入服务器管理员登录密码。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-812">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="d9fd7-813">键入新的数据库名称，例如： *MVC4SampleDB*。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-813">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="d9fd7-814">![配置目标连接字符串](aspnet-mvc-4-fundamentals/_static/image64.png "配置目标连接字符串")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-814">![Configuring destination connection string](aspnet-mvc-4-fundamentals/_static/image64.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="d9fd7-815">*配置目标连接字符串*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-815">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="d9fd7-816">然后单击“确定” 。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-816">Then click **OK**.</span></span> <span data-ttu-id="d9fd7-817">当系统提示创建数据库单击**是**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-817">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="d9fd7-818">![创建数据库](aspnet-mvc-4-fundamentals/_static/image65.png "创建数据库字符串")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-818">![Creating the database](aspnet-mvc-4-fundamentals/_static/image65.png "Creating the database string")</span></span>

    <span data-ttu-id="d9fd7-819">*创建数据库*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-819">*Creating the database*</span></span>
7. <span data-ttu-id="d9fd7-820">在默认连接文本框中显示了将用于连接到 Windows Azure 中的 SQL 数据库连接字符串。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-820">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="d9fd7-821">然后，单击 **“下一步”**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-821">Then click **Next**.</span></span>

    <span data-ttu-id="d9fd7-822">![连接字符串指向 SQL 数据库](aspnet-mvc-4-fundamentals/_static/image66.png "指向 SQL 数据库的连接字符串")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-822">![Connection string pointing to SQL Database](aspnet-mvc-4-fundamentals/_static/image66.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="d9fd7-823">*连接字符串指向 SQL 数据库*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-823">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="d9fd7-824">在**预览**页上，单击**发布**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-824">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="d9fd7-825">![Web 应用程序发布](aspnet-mvc-4-fundamentals/_static/image67.png "发布 web 应用程序")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-825">![Publishing the web application](aspnet-mvc-4-fundamentals/_static/image67.png "Publishing the web application")</span></span>

    <span data-ttu-id="d9fd7-826">*发布 web 应用程序*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-826">*Publishing the web application*</span></span>
9. <span data-ttu-id="d9fd7-827">完成发布过程后，默认浏览器将打开已发布的网站。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-827">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="d9fd7-828">![应用程序发布到 Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "应用程序发布到 Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-828">![Application published to Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="d9fd7-829">*应用程序发布到 Windows Azure*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-829">*Application published to Windows Azure*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="d9fd7-830">附录 c： 使用代码段</span><span class="sxs-lookup"><span data-stu-id="d9fd7-830">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="d9fd7-831">和代码片段，必须将你能够轻松获得所需的所有代码所示。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-831">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="d9fd7-832">实验室文档将告诉您完全时你可以使用它们，如下图中所示。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-832">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="d9fd7-833">![使用 Visual Studio 代码段将代码插入到你的项目](aspnet-mvc-4-fundamentals/_static/image69.png "使用 Visual Studio 代码片段，可将代码插入到你的项目")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-833">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-fundamentals/_static/image69.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="d9fd7-834">*使用 Visual Studio 代码段将代码插入到你的项目*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-834">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="d9fd7-835">***若要添加代码片段使用键盘 (仅限 C#)***</span><span class="sxs-lookup"><span data-stu-id="d9fd7-835">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="d9fd7-836">将光标置于想要插入代码。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-836">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="d9fd7-837">开始键入代码段名称 （不带空格或连字符）。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-837">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="d9fd7-838">观看作为 IntelliSense 显示匹配的代码段的名称。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-838">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="d9fd7-839">选择正确的代码段 （或继续键入直到选中整个代码段的名称）。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-839">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="d9fd7-840">按 Tab 键两次以光标位置处插入代码段。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-840">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="d9fd7-841">![开始键入代码段名称](aspnet-mvc-4-fundamentals/_static/image70.png "开始键入代码段名称")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-841">![Start typing the snippet name](aspnet-mvc-4-fundamentals/_static/image70.png "Start typing the snippet name")</span></span>

<span data-ttu-id="d9fd7-842">*开始键入代码段名称*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-842">*Start typing the snippet name*</span></span>

<span data-ttu-id="d9fd7-843">![按 Tab 以选择突出显示代码段](aspnet-mvc-4-fundamentals/_static/image71.png "按选项卡以选择突出显示代码段")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-843">![Press Tab to select the highlighted snippet](aspnet-mvc-4-fundamentals/_static/image71.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="d9fd7-844">*按 Tab 以选择突出显示代码段*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-844">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="d9fd7-845">![再次按 Tab 和代码段将会扩展](aspnet-mvc-4-fundamentals/_static/image72.png "再次按 Tab 和代码段将会扩展")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-845">![Press Tab again and the snippet will expand](aspnet-mvc-4-fundamentals/_static/image72.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="d9fd7-846">*再次按 Tab 和代码段将会扩展*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-846">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="d9fd7-847">***若要添加代码片段使用鼠标 （C#、 Visual Basic 和 XML）*** 1。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-847">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="d9fd7-848">右键单击你想要插入代码段。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-848">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="d9fd7-849">选择**插入代码段**跟**我的代码段**。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-849">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="d9fd7-850">选择相关的代码段从列表中，通过单击它。</span><span class="sxs-lookup"><span data-stu-id="d9fd7-850">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="d9fd7-851">![右键单击你想要插入代码段，并选择插入代码段](aspnet-mvc-4-fundamentals/_static/image73.png "右键单击你想要插入代码段，并选择插入代码段")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-851">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-fundamentals/_static/image73.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="d9fd7-852">*右键单击你想要插入代码段，并选择插入代码段*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-852">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="d9fd7-853">![通过单击它选取列表中中的相关代码片段](aspnet-mvc-4-fundamentals/_static/image74.png "选取相关的代码段从列表中，通过单击它")</span><span class="sxs-lookup"><span data-stu-id="d9fd7-853">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-fundamentals/_static/image74.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="d9fd7-854">*通过单击它选取从列表中，相关代码段*</span><span class="sxs-lookup"><span data-stu-id="d9fd7-854">*Pick the relevant snippet from the list, by clicking on it*</span></span>
