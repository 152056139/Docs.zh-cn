---
title: "从 ASP.NET MVC 迁移到 ASP.NET 核心 MVC"
author: ardalis
description: 
keywords: "ASP.NET 核心，MVC，迁移"
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: article
ms.assetid: 3155cc9e-d0c9-424b-886c-35c0ec6f9f4e
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/mvc
ms.openlocfilehash: ccdceed927d90a1f3201be9d9f92ebb4f2f66e66
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/11/2017
---
# <a name="migrating-from-aspnet-mvc-to-aspnet-core-mvc"></a><span data-ttu-id="a219c-103">从 ASP.NET MVC 迁移到 ASP.NET 核心 MVC</span><span class="sxs-lookup"><span data-stu-id="a219c-103">Migrating From ASP.NET MVC to ASP.NET Core MVC</span></span>

<span data-ttu-id="a219c-104">通过[Rick Anderson](https://twitter.com/RickAndMSFT)， [Daniel Roth](https://github.com/danroth27)， [Steve Smith](http://ardalis.com)，和[Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="a219c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](http://ardalis.com), and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="a219c-105">这篇文章演示如何开始迁移到 ASP.NET MVC 项目[ASP.NET 核心 MVC](../mvc/overview.md)。</span><span class="sxs-lookup"><span data-stu-id="a219c-105">This article shows how to get started migrating an ASP.NET MVC project to [ASP.NET Core MVC](../mvc/overview.md).</span></span> <span data-ttu-id="a219c-106">在过程中，会突出显示许多已更改利用 ASP.NET MVC 的内容。</span><span class="sxs-lookup"><span data-stu-id="a219c-106">In the process, it highlights many of the things that have changed from ASP.NET MVC.</span></span> <span data-ttu-id="a219c-107">从 ASP.NET MVC 迁移是一个多步骤过程，本文涵盖初始安装程序、 基本控制器和视图、 静态内容和客户端依赖关系。</span><span class="sxs-lookup"><span data-stu-id="a219c-107">Migrating from ASP.NET MVC is a multiple step process and this article covers the initial setup, basic controllers and views, static content, and client-side dependencies.</span></span> <span data-ttu-id="a219c-108">其他文章涵盖迁移配置和标识代码在许多 ASP.NET MVC 项目中找到。</span><span class="sxs-lookup"><span data-stu-id="a219c-108">Additional articles cover migrating configuration and identity code found in many ASP.NET MVC projects.</span></span>

> [!NOTE]
> <span data-ttu-id="a219c-109">这些示例中的版本号可能不是最新。</span><span class="sxs-lookup"><span data-stu-id="a219c-109">The version numbers in the samples might not be current.</span></span> <span data-ttu-id="a219c-110">你可能需要相应地更新你的项目。</span><span class="sxs-lookup"><span data-stu-id="a219c-110">You may need to update your projects accordingly.</span></span>

## <a name="create-the-starter-aspnet-mvc-project"></a><span data-ttu-id="a219c-111">创建初学者 ASP.NET MVC 项目</span><span class="sxs-lookup"><span data-stu-id="a219c-111">Create the starter ASP.NET MVC project</span></span>

<span data-ttu-id="a219c-112">为了演示升级，我们将开始通过创建 ASP.NET MVC 应用程序。</span><span class="sxs-lookup"><span data-stu-id="a219c-112">To demonstrate the upgrade, we'll start by creating a ASP.NET MVC app.</span></span> <span data-ttu-id="a219c-113">创建同名*WebApp1*使命名空间将与我们在下一步中创建 ASP.NET Core 项目相匹配。</span><span class="sxs-lookup"><span data-stu-id="a219c-113">Create it with the name *WebApp1* so the namespace will match the ASP.NET Core project we create in the next step.</span></span>

![Visual Studio 的新项目对话框](mvc/_static/new-project.png)

![新建 Web 应用程序对话框： 在 ASP.NET 模板面板中选择 MVC 项目模板](mvc/_static/new-project-select-mvc-template.png)

<span data-ttu-id="a219c-116">*可选：*更改从解决方案的名称*WebApp1*到*Mvc5*。</span><span class="sxs-lookup"><span data-stu-id="a219c-116">*Optional:* Change the name of the Solution from *WebApp1* to *Mvc5*.</span></span> <span data-ttu-id="a219c-117">Visual Studio 将显示新的解决方案名称 (*Mvc5*)，这会使它更轻松地判断此项目从下一步的项目。</span><span class="sxs-lookup"><span data-stu-id="a219c-117">Visual Studio will display the new solution name (*Mvc5*), which will make it easier to tell this project from the next project.</span></span>

## <a name="create-the-aspnet-core-project"></a><span data-ttu-id="a219c-118">创建 ASP.NET Core 项目</span><span class="sxs-lookup"><span data-stu-id="a219c-118">Create the ASP.NET Core project</span></span>

<span data-ttu-id="a219c-119">创建一个新*空*ASP.NET Core 与以前的项目同名的 web 应用 (*WebApp1*) 以便将两个项目中的命名空间匹配。</span><span class="sxs-lookup"><span data-stu-id="a219c-119">Create a new *empty* ASP.NET Core web app with the same name as the previous project (*WebApp1*) so the namespaces in the two projects match.</span></span> <span data-ttu-id="a219c-120">使用相同的命名空间，可更轻松地复制两个项目之间的代码。</span><span class="sxs-lookup"><span data-stu-id="a219c-120">Having the same namespace makes it easier to copy code between the two projects.</span></span> <span data-ttu-id="a219c-121">你将需要在比以前的项目以使用相同的名称不同的目录中创建此项目。</span><span class="sxs-lookup"><span data-stu-id="a219c-121">You'll have to create this project in a different directory than the previous project to use the same name.</span></span>

![“新建项目”对话框](mvc/_static/new_core.png)

![新建 ASP.NET Web 应用程序对话框： 在 ASP.NET 核心模板面板中选择的空项目模板](mvc/_static/new-project-select-empty-aspnet5-template.png)

* <span data-ttu-id="a219c-124">*可选：*创建新的 ASP.NET Core 应用使用*Web 应用程序*项目模板。</span><span class="sxs-lookup"><span data-stu-id="a219c-124">*Optional:* Create a new ASP.NET Core app using the *Web Application* project template.</span></span> <span data-ttu-id="a219c-125">将项目*WebApp1*，然后选择的一个身份验证选项**单个用户帐户**。</span><span class="sxs-lookup"><span data-stu-id="a219c-125">Name the project *WebApp1*, and select an authentication option of **Individual User Accounts**.</span></span> <span data-ttu-id="a219c-126">重命名此应用程序到*FullAspNetCore*。</span><span class="sxs-lookup"><span data-stu-id="a219c-126">Rename this app to *FullAspNetCore*.</span></span> <span data-ttu-id="a219c-127">在转换过程中创建此项目将保存您的时间。</span><span class="sxs-lookup"><span data-stu-id="a219c-127">Creating this project will save you time in the conversion.</span></span> <span data-ttu-id="a219c-128">你可以查看若要查看的最终结果或将代码复制到转换项目模板生成的代码。</span><span class="sxs-lookup"><span data-stu-id="a219c-128">You can look at the template-generated code to see the end result or to copy code to the conversion project.</span></span> <span data-ttu-id="a219c-129">它也是很有帮助时遇到困难在转换步骤中，要与模板生成项目进行比较。</span><span class="sxs-lookup"><span data-stu-id="a219c-129">It's also helpful when you get stuck on a conversion step to compare with the template-generated project.</span></span>

## <a name="configure-the-site-to-use-mvc"></a><span data-ttu-id="a219c-130">配置站点以使用 MVC</span><span class="sxs-lookup"><span data-stu-id="a219c-130">Configure the site to use MVC</span></span>

* <span data-ttu-id="a219c-131">安装`Microsoft.AspNetCore.Mvc`和`Microsoft.AspNetCore.StaticFiles`NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="a219c-131">Install the `Microsoft.AspNetCore.Mvc` and `Microsoft.AspNetCore.StaticFiles` NuGet packages.</span></span>

  <span data-ttu-id="a219c-132">`Microsoft.AspNetCore.Mvc`是 ASP.NET 核心 MVC 框架。</span><span class="sxs-lookup"><span data-stu-id="a219c-132">`Microsoft.AspNetCore.Mvc` is the ASP.NET Core MVC framework.</span></span> <span data-ttu-id="a219c-133">`Microsoft.AspNetCore.StaticFiles`是静态文件处理程序。</span><span class="sxs-lookup"><span data-stu-id="a219c-133">`Microsoft.AspNetCore.StaticFiles` is the static file handler.</span></span> <span data-ttu-id="a219c-134">是一个模块化，ASP.NET 运行时，你必须显式选择中提供静态文件 (请参阅[使用静态文件](../fundamentals/static-files.md))。</span><span class="sxs-lookup"><span data-stu-id="a219c-134">The ASP.NET runtime is modular, and you must explicitly opt in to serve static files (see [Working with Static Files](../fundamentals/static-files.md)).</span></span>

* <span data-ttu-id="a219c-135">打开*.csproj*文件 (右键单击中的项目**解决方案资源管理器**和选择**编辑 WebApp1.csproj**) 并添加`PrepareForPublish`目标：</span><span class="sxs-lookup"><span data-stu-id="a219c-135">Open the *.csproj* file (right-click the project in **Solution Explorer** and select **Edit WebApp1.csproj**) and add a `PrepareForPublish` target:</span></span>

  <span data-ttu-id="a219c-136">[!code-xml[Main](mvc/sample/WebApp1.csproj?range=21-23)]</span><span class="sxs-lookup"><span data-stu-id="a219c-136">[!code-xml[Main](mvc/sample/WebApp1.csproj?range=21-23)]</span></span>

  <span data-ttu-id="a219c-137">`PrepareForPublish`目标所需的获取通过 Bower 的客户端库。</span><span class="sxs-lookup"><span data-stu-id="a219c-137">The `PrepareForPublish` target is needed for acquiring client-side libraries via Bower.</span></span> <span data-ttu-id="a219c-138">我们将讨论的更高版本。</span><span class="sxs-lookup"><span data-stu-id="a219c-138">We'll talk about that later.</span></span>

* <span data-ttu-id="a219c-139">打开*Startup.cs*文件并将更改代码以匹配以下内容：</span><span class="sxs-lookup"><span data-stu-id="a219c-139">Open the *Startup.cs* file and change the code to match the following:</span></span>

  <span data-ttu-id="a219c-140">[!code-csharp[Main](mvc/sample/Startup.cs?highlight=14,27-34)]</span><span class="sxs-lookup"><span data-stu-id="a219c-140">[!code-csharp[Main](mvc/sample/Startup.cs?highlight=14,27-34)]</span></span>

  <span data-ttu-id="a219c-141">`UseStaticFiles`扩展方法将添加静态文件处理程序。</span><span class="sxs-lookup"><span data-stu-id="a219c-141">The `UseStaticFiles` extension method adds the static file handler.</span></span> <span data-ttu-id="a219c-142">如前所述，ASP.NET 运行时是一个模块化，和中，你必须显式选择要为静态文件服务。</span><span class="sxs-lookup"><span data-stu-id="a219c-142">As mentioned previously, the ASP.NET runtime is modular, and you must explicitly opt in to serve static files.</span></span> <span data-ttu-id="a219c-143">`UseMvc`扩展方法将添加路由。</span><span class="sxs-lookup"><span data-stu-id="a219c-143">The `UseMvc` extension method adds routing.</span></span> <span data-ttu-id="a219c-144">有关详细信息，请参阅[应用程序启动](../fundamentals/startup.md)和[路由](../fundamentals/routing.md)。</span><span class="sxs-lookup"><span data-stu-id="a219c-144">For more information, see [Application Startup](../fundamentals/startup.md) and [Routing](../fundamentals/routing.md).</span></span>

## <a name="add-a-controller-and-view"></a><span data-ttu-id="a219c-145">添加控制器和视图</span><span class="sxs-lookup"><span data-stu-id="a219c-145">Add a controller and view</span></span>

<span data-ttu-id="a219c-146">在本部分中，你将添加一个最小控制器和视图，以用作占位符的 ASP.NET MVC 控制器和视图将在下一部分中迁移。</span><span class="sxs-lookup"><span data-stu-id="a219c-146">In this section, you'll add a minimal controller and view to serve as placeholders for the ASP.NET MVC controller and views you'll migrate in the next section.</span></span>

* <span data-ttu-id="a219c-147">添加*控制器*文件夹。</span><span class="sxs-lookup"><span data-stu-id="a219c-147">Add a *Controllers* folder.</span></span>

* <span data-ttu-id="a219c-148">添加**MVC 控制器类**同名*HomeController.cs*到*控制器*文件夹。</span><span class="sxs-lookup"><span data-stu-id="a219c-148">Add an **MVC controller class** with the name *HomeController.cs* to the *Controllers* folder.</span></span>

![添加新项对话框](mvc/_static/add_mvc_ctl.png)

* <span data-ttu-id="a219c-150">添加*视图*文件夹。</span><span class="sxs-lookup"><span data-stu-id="a219c-150">Add a *Views* folder.</span></span>

* <span data-ttu-id="a219c-151">添加*视图/主页*文件夹。</span><span class="sxs-lookup"><span data-stu-id="a219c-151">Add a *Views/Home* folder.</span></span>

* <span data-ttu-id="a219c-152">添加*Index.cshtml*到 MVC 视图页*视图/主页*文件夹。</span><span class="sxs-lookup"><span data-stu-id="a219c-152">Add an *Index.cshtml* MVC view page to the *Views/Home* folder.</span></span>

![添加新项对话框](mvc/_static/view.png)

<span data-ttu-id="a219c-154">项目结构如下所示：</span><span class="sxs-lookup"><span data-stu-id="a219c-154">The project structure is shown below:</span></span>

![显示文件和文件夹的 WebApp1 的解决方案资源管理器](mvc/_static/project-structure-controller-view.png)

<span data-ttu-id="a219c-156">内容替换*Views/Home/Index.cshtml*替换为以下文件：</span><span class="sxs-lookup"><span data-stu-id="a219c-156">Replace the contents of the *Views/Home/Index.cshtml* file with the following:</span></span>

```html
<h1>Hello world!</h1>
```

<span data-ttu-id="a219c-157">运行该应用。</span><span class="sxs-lookup"><span data-stu-id="a219c-157">Run the app.</span></span>

![Web 应用程序在 Microsoft Edge 中打开](mvc/_static/hello-world.png)

<span data-ttu-id="a219c-159">请参阅[控制器](../mvc/controllers/index.md)和[视图](../mvc/views/index.md)有关详细信息。</span><span class="sxs-lookup"><span data-stu-id="a219c-159">See [Controllers](../mvc/controllers/index.md) and [Views](../mvc/views/index.md) for more information.</span></span>

<span data-ttu-id="a219c-160">现在，我们已最小的工作 ASP.NET Core 项目，我们可以开始从 ASP.NET MVC 项目迁移功能。</span><span class="sxs-lookup"><span data-stu-id="a219c-160">Now that we have a minimal working ASP.NET Core project, we can start migrating functionality from the ASP.NET MVC project.</span></span> <span data-ttu-id="a219c-161">我们将需要将以下：</span><span class="sxs-lookup"><span data-stu-id="a219c-161">We will need to move the following:</span></span>

* <span data-ttu-id="a219c-162">客户端内容 （CSS、 字体和脚本）</span><span class="sxs-lookup"><span data-stu-id="a219c-162">client-side content (CSS, fonts, and scripts)</span></span>

* <span data-ttu-id="a219c-163">控制器</span><span class="sxs-lookup"><span data-stu-id="a219c-163">controllers</span></span>

* <span data-ttu-id="a219c-164">视图</span><span class="sxs-lookup"><span data-stu-id="a219c-164">views</span></span>

* <span data-ttu-id="a219c-165">模型</span><span class="sxs-lookup"><span data-stu-id="a219c-165">models</span></span>

* <span data-ttu-id="a219c-166">绑定</span><span class="sxs-lookup"><span data-stu-id="a219c-166">bundling</span></span>

* <span data-ttu-id="a219c-167">筛选器</span><span class="sxs-lookup"><span data-stu-id="a219c-167">filters</span></span>

* <span data-ttu-id="a219c-168">输入/输出的日志，标识 （这将在执行下一步的教程。）</span><span class="sxs-lookup"><span data-stu-id="a219c-168">Log in/out, identity (This will be done in the next tutorial.)</span></span>

## <a name="controllers-and-views"></a><span data-ttu-id="a219c-169">控制器和视图</span><span class="sxs-lookup"><span data-stu-id="a219c-169">Controllers and views</span></span>

* <span data-ttu-id="a219c-170">将每个方法复制利用 ASP.NET MVC`HomeController`对新`HomeController`。</span><span class="sxs-lookup"><span data-stu-id="a219c-170">Copy each of the methods from the ASP.NET MVC `HomeController` to the new `HomeController`.</span></span> <span data-ttu-id="a219c-171">请注意，在 ASP.NET MVC 内置模板的控制器操作方法的返回类型[ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); 在 ASP.NET 核心 MVC，操作方法返回`IActionResult`相反。</span><span class="sxs-lookup"><span data-stu-id="a219c-171">Note that in ASP.NET MVC, the built-in template's controller action method return type is [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); in ASP.NET Core MVC, the action methods return `IActionResult` instead.</span></span> <span data-ttu-id="a219c-172">`ActionResult`实现`IActionResult`，因此无需更改你的操作方法的返回类型。</span><span class="sxs-lookup"><span data-stu-id="a219c-172">`ActionResult` implements `IActionResult`, so there is no need to change the return type of your action methods.</span></span>

* <span data-ttu-id="a219c-173">复制*About.cshtml*， *Contact.cshtml*，和*Index.cshtml* Razor 视图文件从 ASP.NET MVC 项目添加到 ASP.NET 核心项目。</span><span class="sxs-lookup"><span data-stu-id="a219c-173">Copy the *About.cshtml*, *Contact.cshtml*, and *Index.cshtml* Razor view files from the ASP.NET MVC project to the ASP.NET Core project.</span></span>

* <span data-ttu-id="a219c-174">运行 ASP.NET Core 应用和测试每个方法。</span><span class="sxs-lookup"><span data-stu-id="a219c-174">Run the ASP.NET Core app and test each method.</span></span> <span data-ttu-id="a219c-175">我们尚未尚未进行迁移的布局文件或样式，因此呈现的视图将仅包含视图文件中的内容。</span><span class="sxs-lookup"><span data-stu-id="a219c-175">We haven't migrated the layout file or styles yet, so the rendered views will only contain the content in the view files.</span></span> <span data-ttu-id="a219c-176">不会有的布局生成文件链接`About`和`Contact`视图，因此你将需要从浏览器中调用它们 (替换**4492**与你的项目中使用的端口号)。</span><span class="sxs-lookup"><span data-stu-id="a219c-176">You won't have the layout file generated links for the `About` and `Contact` views, so you'll have to invoke them from the browser (replace **4492** with the port number used in your project).</span></span>

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![联系人页面](mvc/_static/contact-page.png)

<span data-ttu-id="a219c-178">请注意在缺乏样式和菜单项。</span><span class="sxs-lookup"><span data-stu-id="a219c-178">Note the lack of styling and menu items.</span></span> <span data-ttu-id="a219c-179">我们将在下一部分中修复此问题。</span><span class="sxs-lookup"><span data-stu-id="a219c-179">We'll fix that in the next section.</span></span>

## <a name="static-content"></a><span data-ttu-id="a219c-180">静态内容</span><span class="sxs-lookup"><span data-stu-id="a219c-180">Static content</span></span>

<span data-ttu-id="a219c-181">在以前版本的 ASP.NET MVC，静态内容已承载 web 项目的根目录中，已使用服务器端文件混合。</span><span class="sxs-lookup"><span data-stu-id="a219c-181">In previous versions of  ASP.NET MVC, static content was hosted from the root of the web project and was intermixed with server-side files.</span></span> <span data-ttu-id="a219c-182">在 ASP.NET Core 静态内容承载于*wwwroot*文件夹。</span><span class="sxs-lookup"><span data-stu-id="a219c-182">In ASP.NET Core, static content is hosted in the *wwwroot* folder.</span></span> <span data-ttu-id="a219c-183">你将想要复制的静态内容从旧 ASP.NET MVC 应用程序到*wwwroot* ASP.NET Core 项目文件夹中的。</span><span class="sxs-lookup"><span data-stu-id="a219c-183">You'll want to copy the static content from your old ASP.NET MVC app to the *wwwroot* folder in your ASP.NET Core project.</span></span> <span data-ttu-id="a219c-184">在此示例转换：</span><span class="sxs-lookup"><span data-stu-id="a219c-184">In this sample conversion:</span></span>

* <span data-ttu-id="a219c-185">复制*favicon.ico*文件从旧的 MVC 项目到*wwwroot* ASP.NET Core 项目文件夹中的。</span><span class="sxs-lookup"><span data-stu-id="a219c-185">Copy the *favicon.ico* file from the old MVC project to the *wwwroot* folder in the ASP.NET Core project.</span></span>

<span data-ttu-id="a219c-186">旧 ASP.NET MVC 项目使用[Bootstrap](http://getbootstrap.com/)其 Bootstrap 文件中的样式和存储*内容*和*脚本*文件夹。</span><span class="sxs-lookup"><span data-stu-id="a219c-186">The old ASP.NET MVC project uses [Bootstrap](http://getbootstrap.com/) for its styling and stores the Bootstrap files in the *Content* and *Scripts* folders.</span></span> <span data-ttu-id="a219c-187">该模板后，生成旧的 ASP.NET MVC 项目，该引用布局文件中的 Bootstrap (*Views/Shared/_Layout.cshtml*)。</span><span class="sxs-lookup"><span data-stu-id="a219c-187">The template, which generated the old ASP.NET MVC project, references Bootstrap in the layout file (*Views/Shared/_Layout.cshtml*).</span></span> <span data-ttu-id="a219c-188">你也可以将复制*bootstrap.js*和*bootstrap.css*文件从 ASP.NET MVC 项目合并为*wwwroot*文件夹中新的项目中，但这种方法不使用用于管理 ASP.NET Core 中的客户端依赖项的改进的机制。</span><span class="sxs-lookup"><span data-stu-id="a219c-188">You could copy the *bootstrap.js* and *bootstrap.css* files from the ASP.NET MVC project to the *wwwroot* folder in the new project, but that approach doesn't use the improved mechanism for managing client-side dependencies in ASP.NET Core.</span></span>

<span data-ttu-id="a219c-189">在新的项目中，我们将添加对 Bootstrap （和其他客户端库），它使用支持[Bower](http://bower.io/):</span><span class="sxs-lookup"><span data-stu-id="a219c-189">In the new project, we'll add support for Bootstrap (and other client-side libraries) using [Bower](http://bower.io/):</span></span>

* <span data-ttu-id="a219c-190">添加[Bower](http://bower.io/)名为配置文件*bower.json*与项目根目录 (在项目中，右键单击，然后**添加 > 新项 > Bower 配置文件**)。</span><span class="sxs-lookup"><span data-stu-id="a219c-190">Add a [Bower](http://bower.io/) configuration file named *bower.json* to the project root (Right-click on the project, and then **Add > New Item > Bower Configuration File**).</span></span> <span data-ttu-id="a219c-191">添加[Bootstrap](http://getbootstrap.com/)和[jQuery](https://jquery.com/)文件 （请参阅下面突出显示的行）。</span><span class="sxs-lookup"><span data-stu-id="a219c-191">Add [Bootstrap](http://getbootstrap.com/) and [jQuery](https://jquery.com/) to the file (see the highlighted lines below).</span></span>

  <span data-ttu-id="a219c-192">[!code-json[Main](mvc/sample/bower.json?highlight=5-6)]</span><span class="sxs-lookup"><span data-stu-id="a219c-192">[!code-json[Main](mvc/sample/bower.json?highlight=5-6)]</span></span>

<span data-ttu-id="a219c-193">在保存文件，Bower 将自动下载到的依赖关系*wwwroot/lib*文件夹。</span><span class="sxs-lookup"><span data-stu-id="a219c-193">Upon saving the file, Bower will automatically download the dependencies to the *wwwroot/lib* folder.</span></span> <span data-ttu-id="a219c-194">你可以使用**搜索解决方案资源管理器**框查找资产的路径：</span><span class="sxs-lookup"><span data-stu-id="a219c-194">You can use the **Search Solution Explorer** box to find the path of the assets:</span></span>

![解决方案资源管理器搜索结果中显示的 jquery 资产](mvc/_static/search.png)

<span data-ttu-id="a219c-196">请参阅[Bower 与管理客户端包](../client-side/bower.md)有关详细信息。</span><span class="sxs-lookup"><span data-stu-id="a219c-196">See [Manage Client-Side Packages with Bower](../client-side/bower.md) for more information.</span></span>

<a name=migrate-layout-file></a>

## <a name="migrate-the-layout-file"></a><span data-ttu-id="a219c-197">迁移布局文件</span><span class="sxs-lookup"><span data-stu-id="a219c-197">Migrate the layout file</span></span>

* <span data-ttu-id="a219c-198">复制*_ViewStart.cshtml*文件从旧的 ASP.NET MVC 项目*视图*文件夹导入到 ASP.NET 核心项目*视图*文件夹。</span><span class="sxs-lookup"><span data-stu-id="a219c-198">Copy the *_ViewStart.cshtml* file from the old ASP.NET MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="a219c-199">*_ViewStart.cshtml*文件未更改 ASP.NET 核心 mvc。</span><span class="sxs-lookup"><span data-stu-id="a219c-199">The *_ViewStart.cshtml* file has not changed in ASP.NET Core MVC.</span></span>

* <span data-ttu-id="a219c-200">创建*视图/共享*文件夹。</span><span class="sxs-lookup"><span data-stu-id="a219c-200">Create a *Views/Shared* folder.</span></span>

* <span data-ttu-id="a219c-201">*可选：*复制*_ViewImports.cshtml*从*FullAspNetCore* MVC 项目*视图*文件夹导入到 ASP.NET 核心项目*视图*文件夹。</span><span class="sxs-lookup"><span data-stu-id="a219c-201">*Optional:* Copy *_ViewImports.cshtml* from the *FullAspNetCore* MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="a219c-202">删除中的任何命名空间声明*_ViewImports.cshtml*文件。</span><span class="sxs-lookup"><span data-stu-id="a219c-202">Remove any namespace declaration in the *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="a219c-203">*_ViewImports.cshtml*文件对于视图的所有文件提供命名空间，并使[标记帮助程序](../mvc/views/tag-helpers/index.md)。</span><span class="sxs-lookup"><span data-stu-id="a219c-203">The *_ViewImports.cshtml* file provides namespaces for all the view files and brings in [Tag Helpers](../mvc/views/tag-helpers/index.md).</span></span> <span data-ttu-id="a219c-204">新的布局文件中使用标记帮助程序。</span><span class="sxs-lookup"><span data-stu-id="a219c-204">Tag Helpers are used in the new layout file.</span></span> <span data-ttu-id="a219c-205">*_ViewImports.cshtml*文件是用于 ASP.NET 核心新功能。</span><span class="sxs-lookup"><span data-stu-id="a219c-205">The *_ViewImports.cshtml* file is new for ASP.NET Core.</span></span>

* <span data-ttu-id="a219c-206">复制*_Layout.cshtml*文件从旧的 ASP.NET MVC 项目*视图/共享*文件夹导入到 ASP.NET 核心项目*视图/共享*文件夹。</span><span class="sxs-lookup"><span data-stu-id="a219c-206">Copy the *_Layout.cshtml* file from the old ASP.NET MVC project's *Views/Shared* folder into the ASP.NET Core project's *Views/Shared* folder.</span></span>

<span data-ttu-id="a219c-207">打开*_Layout.cshtml*文件并进行以下更改 （已完成的代码下面显示）：</span><span class="sxs-lookup"><span data-stu-id="a219c-207">Open *_Layout.cshtml* file and make the following changes (the completed code is shown below):</span></span>

   * <span data-ttu-id="a219c-208">替换`@Styles.Render("~/Content/css")`与`<link>`元素加载*bootstrap.css* （见下文）。</span><span class="sxs-lookup"><span data-stu-id="a219c-208">Replace `@Styles.Render("~/Content/css")` with a `<link>` element to load *bootstrap.css* (see below).</span></span>

   * <span data-ttu-id="a219c-209">删除 `@Scripts.Render("~/bundles/modernizr")`。</span><span class="sxs-lookup"><span data-stu-id="a219c-209">Remove `@Scripts.Render("~/bundles/modernizr")`.</span></span>

   * <span data-ttu-id="a219c-210">注释掉`@Html.Partial("_LoginPartial")`行 (括在一行时与`@*...*@`)。</span><span class="sxs-lookup"><span data-stu-id="a219c-210">Comment out the `@Html.Partial("_LoginPartial")` line (surround the line with `@*...*@`).</span></span> <span data-ttu-id="a219c-211">在将来的教程，我们会返回到它。</span><span class="sxs-lookup"><span data-stu-id="a219c-211">We'll return to it in a future tutorial.</span></span>

   * <span data-ttu-id="a219c-212">替换`@Scripts.Render("~/bundles/jquery")`与`<script>`元素 （见下文）。</span><span class="sxs-lookup"><span data-stu-id="a219c-212">Replace `@Scripts.Render("~/bundles/jquery")` with a `<script>` element (see below).</span></span>

   * <span data-ttu-id="a219c-213">替换`@Scripts.Render("~/bundles/bootstrap")`与`<script>`元素 （见下文）...</span><span class="sxs-lookup"><span data-stu-id="a219c-213">Replace `@Scripts.Render("~/bundles/bootstrap")` with a `<script>` element (see below)..</span></span>

<span data-ttu-id="a219c-214">替换 CSS 链接：</span><span class="sxs-lookup"><span data-stu-id="a219c-214">The replacement CSS link:</span></span>

```html
<link rel="stylesheet" href="~/lib/bootstrap/dist/css/bootstrap.css" />
```

<span data-ttu-id="a219c-215">替换脚本标记：</span><span class="sxs-lookup"><span data-stu-id="a219c-215">The replacement script tags:</span></span>

```html
<script src="~/lib/jquery/dist/jquery.js"></script>
<script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
```

<span data-ttu-id="a219c-216">已更新*_Layout.cshtml*文件如下所示：</span><span class="sxs-lookup"><span data-stu-id="a219c-216">The updated *_Layout.cshtml* file is shown below:</span></span>

<span data-ttu-id="a219c-217">[!code-html[Main](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7,27,39-40)]</span><span class="sxs-lookup"><span data-stu-id="a219c-217">[!code-html[Main](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7,27,39-40)]</span></span>

<span data-ttu-id="a219c-218">在浏览器中查看站点。</span><span class="sxs-lookup"><span data-stu-id="a219c-218">View the site in the browser.</span></span> <span data-ttu-id="a219c-219">它应现在正确加载，以就地预期的样式。</span><span class="sxs-lookup"><span data-stu-id="a219c-219">It should now load correctly, with the expected styles in place.</span></span>

* <span data-ttu-id="a219c-220">*可选：*可能想要尝试使用新的布局文件。</span><span class="sxs-lookup"><span data-stu-id="a219c-220">*Optional:* You might want to try using the new layout file.</span></span> <span data-ttu-id="a219c-221">对于此项目中，你可以复制中的布局文件*FullAspNetCore*项目。</span><span class="sxs-lookup"><span data-stu-id="a219c-221">For this project you can copy the layout file from the *FullAspNetCore* project.</span></span> <span data-ttu-id="a219c-222">新的布局文件使用[标记帮助程序](../mvc/views/tag-helpers/index.md)并且具有其他的改进功能。</span><span class="sxs-lookup"><span data-stu-id="a219c-222">The new layout file uses [Tag Helpers](../mvc/views/tag-helpers/index.md) and has other improvements.</span></span>

## <a name="configure-bundling--minification"></a><span data-ttu-id="a219c-223">配置绑定和缩减</span><span class="sxs-lookup"><span data-stu-id="a219c-223">Configure Bundling & Minification</span></span>

<span data-ttu-id="a219c-224">有关如何配置绑定和缩减的信息，请参阅[捆绑和缩减](../client-side/bundling-and-minification.md)。</span><span class="sxs-lookup"><span data-stu-id="a219c-224">For information about how to configure bundling and minification, see [Bundling and Minification](../client-side/bundling-and-minification.md).</span></span>

## <a name="solving-http-500-errors"></a><span data-ttu-id="a219c-225">解决 HTTP 500 错误</span><span class="sxs-lookup"><span data-stu-id="a219c-225">Solving HTTP 500 errors</span></span>

<span data-ttu-id="a219c-226">有许多问题的包含没有关于此问题的源的信息可能会导致 HTTP 500 错误消息。</span><span class="sxs-lookup"><span data-stu-id="a219c-226">There are many problems that can cause a HTTP 500 error message that contain no information on the source of the problem.</span></span> <span data-ttu-id="a219c-227">例如，如果*Views/_ViewImports.cshtml*文件包含在你的项目中不存在的命名空间，你将收到 HTTP 500 错误。</span><span class="sxs-lookup"><span data-stu-id="a219c-227">For example, if the *Views/_ViewImports.cshtml* file contains a namespace that doesn't exist in your project, you'll get a HTTP 500 error.</span></span> <span data-ttu-id="a219c-228">若要获取详细的错误消息，添加以下代码：</span><span class="sxs-lookup"><span data-stu-id="a219c-228">To get a detailed error message, add the following code:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
{
    if (env.IsDevelopment())
    {
         app.UseDeveloperExceptionPage();
    }

    app.UseStaticFiles();

    app.UseMvc(routes =>
    {
        routes.MapRoute(
            name: "default",
            template: "{controller=Home}/{action=Index}/{id?}");
    });
}
```

<span data-ttu-id="a219c-229">请参阅**使用开发人员异常页**中[错误处理](../fundamentals/error-handling.md)有关详细信息。</span><span class="sxs-lookup"><span data-stu-id="a219c-229">See **Using the Developer Exception Page** in [Error Handling](../fundamentals/error-handling.md) for more information.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a219c-230">其他资源</span><span class="sxs-lookup"><span data-stu-id="a219c-230">Additional Resources</span></span>

* [<span data-ttu-id="a219c-231">客户端开发</span><span class="sxs-lookup"><span data-stu-id="a219c-231">Client-Side Development</span></span>](../client-side/index.md)

* [<span data-ttu-id="a219c-232">标记帮助程序</span><span class="sxs-lookup"><span data-stu-id="a219c-232">Tag Helpers</span></span>](../mvc/views/tag-helpers/index.md)
