---
title: "使用 ASP.NET Core 中的 Bower"
author: rick-anderson
description: "管理 Bower 的客户端包。"
keywords: ASP.NET Core,bower
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: df7c43da-280e-4df6-86cb-eecec8f12bfc
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/bower
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 183e748cbf87b1973941eacb3fb1008f4041bb2a
ms.sourcegitcommit: 532a323f99a37c4d7894c95cee3f7a04b594dcec
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/01/2017
---
# <a name="manage-client-side-packages-with-bower-in-aspnet-core"></a><span data-ttu-id="c7db9-104">管理 ASP.NET Core 中的 Bower 的客户端包</span><span class="sxs-lookup"><span data-stu-id="c7db9-104">Manage client-side packages with Bower in ASP.NET Core</span></span>

<span data-ttu-id="c7db9-105">通过[Rick Anderson](https://twitter.com/RickAndMSFT)，[了米](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/)，和[Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="c7db9-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Noel Rice](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/), and [Scott Addie](https://scottaddie.com)</span></span> 

<span data-ttu-id="c7db9-106">[Bower](https://bower.io/)调用自身"的程序包管理器 web。"</span><span class="sxs-lookup"><span data-stu-id="c7db9-106">[Bower](https://bower.io/) calls itself "A package manager for the web."</span></span> <span data-ttu-id="c7db9-107">内部.NET 生态系统，它将填入 void 留下的 NuGet 的无法传送静态内容的文件。</span><span class="sxs-lookup"><span data-stu-id="c7db9-107">Within the .NET ecosystem, it fills the void left by NuGet’s inability to deliver static content files.</span></span> <span data-ttu-id="c7db9-108">对于 ASP.NET Core 项目，这些静态文件，则所固有的客户端库，如[jQuery](http://jquery.com/)和[Bootstrap](http://getbootstrap.com/)。</span><span class="sxs-lookup"><span data-stu-id="c7db9-108">For ASP.NET Core projects, these static files are inherent to client-side libraries like [jQuery](http://jquery.com/) and [Bootstrap](http://getbootstrap.com/).</span></span> <span data-ttu-id="c7db9-109">对于.NET 库，你仍然使用[NuGet](https://www.nuget.org/)程序包管理器。</span><span class="sxs-lookup"><span data-stu-id="c7db9-109">For .NET libraries, you still use [NuGet](https://www.nuget.org/) package manager.</span></span>

<span data-ttu-id="c7db9-110">设置客户端的 ASP.NET Core 项目模板创建的新项目生成过程。</span><span class="sxs-lookup"><span data-stu-id="c7db9-110">New projects created with the ASP.NET Core project templates set up the client-side build process.</span></span> <span data-ttu-id="c7db9-111">[jQuery](http://jquery.com/)和[Bootstrap](http://getbootstrap.com/)安装，并支持 Bower。</span><span class="sxs-lookup"><span data-stu-id="c7db9-111">[jQuery](http://jquery.com/) and [Bootstrap](http://getbootstrap.com/) are installed, and Bower is supported.</span></span>

<span data-ttu-id="c7db9-112">在列出客户端包*bower.json*文件。</span><span class="sxs-lookup"><span data-stu-id="c7db9-112">Client-side packages are listed in the *bower.json* file.</span></span> <span data-ttu-id="c7db9-113">ASP.NET 核心项目模板配置*bower.json* jQuery、 jQuery 验证与 Bootstrap。</span><span class="sxs-lookup"><span data-stu-id="c7db9-113">The ASP.NET Core project templates configures *bower.json* with jQuery, jQuery validation, and Bootstrap.</span></span>

<span data-ttu-id="c7db9-114">在本教程中，我们将添加对支持[字体出色](http://fontawesome.io)。</span><span class="sxs-lookup"><span data-stu-id="c7db9-114">In this tutorial, we'll add support for [Font Awesome](http://fontawesome.io).</span></span> <span data-ttu-id="c7db9-115">可以使用安装 bower 包**管理 Bower 包**UI 或手动在*bower.json*文件。</span><span class="sxs-lookup"><span data-stu-id="c7db9-115">Bower packages can be installed with the **Manage Bower Packages** UI or manually in the *bower.json* file.</span></span>

### <a name="installation-via-manage-bower-packages-ui"></a><span data-ttu-id="c7db9-116">通过管理 Bower 包 UI 的安装</span><span class="sxs-lookup"><span data-stu-id="c7db9-116">Installation via Manage Bower Packages UI</span></span>

* <span data-ttu-id="c7db9-117">创建新的 ASP.NET 核心 Web 应用程序与**ASP.NET 核心 Web 应用程序 (.NET Core)**模板。</span><span class="sxs-lookup"><span data-stu-id="c7db9-117">Create a new ASP.NET Core Web app with the **ASP.NET Core Web Application (.NET Core)** template.</span></span> <span data-ttu-id="c7db9-118">选择**Web 应用程序**和**无身份验证**。</span><span class="sxs-lookup"><span data-stu-id="c7db9-118">Select **Web Application** and **No Authentication**.</span></span>

* <span data-ttu-id="c7db9-119">右键单击解决方案资源管理器中的项目并选择**管理 Bower 包**(或者从主菜单中，**项目** > **管理 Bower 包**).</span><span class="sxs-lookup"><span data-stu-id="c7db9-119">Right-click the project in Solution Explorer and select **Manage Bower Packages** (alternatively from the main menu, **Project** > **Manage Bower Packages**).</span></span>

* <span data-ttu-id="c7db9-120">在**Bower:\<项目名称\>**窗口中，单击"浏览"选项卡，并输入，然后筛选包列表`font-awesome`的搜索框中：</span><span class="sxs-lookup"><span data-stu-id="c7db9-120">In the **Bower: \<project name\>** window, click the "Browse" tab, and then filter the packages list by entering `font-awesome` in the search box:</span></span>

 ![管理 bower 包](bower/_static/manage-bower-packages.png)

* <span data-ttu-id="c7db9-122">确认"保存更改为*bower.json*"复选框已选中。</span><span class="sxs-lookup"><span data-stu-id="c7db9-122">Confirm that the "Save changes to *bower.json*" checkbox is checked.</span></span> <span data-ttu-id="c7db9-123">从下拉列表中选择一个版本，然后单击**安装**按钮。</span><span class="sxs-lookup"><span data-stu-id="c7db9-123">Select a version from the drop-down list and click the **Install** button.</span></span> <span data-ttu-id="c7db9-124">**输出**窗口显示的安装详细信息。</span><span class="sxs-lookup"><span data-stu-id="c7db9-124">The **Output** window shows the installation details.</span></span>

### <a name="manual-installation-in-bowerjson"></a><span data-ttu-id="c7db9-125">手动安装在 bower.json</span><span class="sxs-lookup"><span data-stu-id="c7db9-125">Manual installation in bower.json</span></span>

<span data-ttu-id="c7db9-126">打开*bower.json*文件并添加到的依赖项的"字体出色"。</span><span class="sxs-lookup"><span data-stu-id="c7db9-126">Open the *bower.json* file and add "font-awesome" to the dependencies.</span></span> <span data-ttu-id="c7db9-127">IntelliSense 会显示可用的包。</span><span class="sxs-lookup"><span data-stu-id="c7db9-127">IntelliSense shows the available packages.</span></span> <span data-ttu-id="c7db9-128">某个包被选中，将显示可用的版本。</span><span class="sxs-lookup"><span data-stu-id="c7db9-128">When a package is selected, the available versions are displayed.</span></span> <span data-ttu-id="c7db9-129">下面的映像版本旧，以及将与你看到的内容不匹配。</span><span class="sxs-lookup"><span data-stu-id="c7db9-129">The images below are older and will not match what you see.</span></span>

![Bower 包资源管理器的智能感知](bower/_static/add-package.png)

![bower 版本 IntelliSense](bower/_static/version-intelliSense.png)

<span data-ttu-id="c7db9-132">Bower 使用[语义版本控制](http://semver.org/)来组织依赖关系。</span><span class="sxs-lookup"><span data-stu-id="c7db9-132">Bower uses [semantic versioning](http://semver.org/) to organize dependencies.</span></span> <span data-ttu-id="c7db9-133">语义版本控制，也称为 SemVer，标识包的编号方案\<主要 >。\<次要 >。\<修补程序 >。</span><span class="sxs-lookup"><span data-stu-id="c7db9-133">Semantic versioning, also known as SemVer, identifies packages with the numbering scheme \<major>.\<minor>.\<patch>.</span></span> <span data-ttu-id="c7db9-134">IntelliSense 显示仅几个常用的选项，从而简化了语义版本控制。</span><span class="sxs-lookup"><span data-stu-id="c7db9-134">IntelliSense simplifies semantic versioning by showing only a few common choices.</span></span> <span data-ttu-id="c7db9-135">IntelliSense 列表 (在上面的示例 4.6.3) 中的顶级项被视为包的最新稳定版本。</span><span class="sxs-lookup"><span data-stu-id="c7db9-135">The top item in the IntelliSense list (4.6.3 in the example above) is considered the latest stable version of the package.</span></span> <span data-ttu-id="c7db9-136">脱字号 (^) 符号匹配的最新的主版本和波形符 （~） 与最新次要版本匹配。</span><span class="sxs-lookup"><span data-stu-id="c7db9-136">The caret (^) symbol matches the most recent major version and the tilde (~) matches the most recent minor version.</span></span>

<span data-ttu-id="c7db9-137">保存*bower.json*文件。</span><span class="sxs-lookup"><span data-stu-id="c7db9-137">Save the *bower.json* file.</span></span> <span data-ttu-id="c7db9-138">Visual Studio 监视*bower.json*文件的更改。</span><span class="sxs-lookup"><span data-stu-id="c7db9-138">Visual Studio watches the *bower.json* file for changes.</span></span> <span data-ttu-id="c7db9-139">保存后， *bower 安装*执行命令。</span><span class="sxs-lookup"><span data-stu-id="c7db9-139">Upon saving, the *bower install* command is executed.</span></span> <span data-ttu-id="c7db9-140">请参见输出窗口**Bower/npm**视图执行的确切命令。</span><span class="sxs-lookup"><span data-stu-id="c7db9-140">See the Output window's **Bower/npm** view for the exact command executed.</span></span>

<span data-ttu-id="c7db9-141">打开*.bowerrc*文件下*bower.json*。</span><span class="sxs-lookup"><span data-stu-id="c7db9-141">Open the *.bowerrc* file under *bower.json*.</span></span> <span data-ttu-id="c7db9-142">`directory`属性设置为*wwwroot/lib*指示的位置 Bower 将安装包资产。</span><span class="sxs-lookup"><span data-stu-id="c7db9-142">The `directory` property is set to *wwwroot/lib* which indicates the location Bower will install the package assets.</span></span>

```json
{
 "directory": "wwwroot/lib"
}
```

<span data-ttu-id="c7db9-143">在解决方案资源管理器搜索框中可用于查找并显示字体出色的包。</span><span class="sxs-lookup"><span data-stu-id="c7db9-143">You can use the search box in Solution Explorer to find and display the font-awesome package.</span></span>

<span data-ttu-id="c7db9-144">打开*views/shared\_Layout.cshtml*文件并将字体出色的 CSS 文件添加到环境[标记帮助器](xref:mvc/views/tag-helpers/intro)为`Development`。</span><span class="sxs-lookup"><span data-stu-id="c7db9-144">Open the *Views\Shared\_Layout.cshtml* file and add the font-awesome CSS file to the environment [Tag Helper](xref:mvc/views/tag-helpers/intro) for `Development`.</span></span> <span data-ttu-id="c7db9-145">从解决方案资源管理器，将拖*字体 awesome.css*内`<environment names="Development">`元素。</span><span class="sxs-lookup"><span data-stu-id="c7db9-145">From Solution Explorer, drag and drop *font-awesome.css* inside the `<environment names="Development">` element.</span></span>

[!code-html[Main](bower/sample/_Layout.cshtml?highlight=4&range=9-13)]

<span data-ttu-id="c7db9-146">在生产应用程序会添加*字体 awesome.min.css*到环境标记帮助器`Staging,Production`。</span><span class="sxs-lookup"><span data-stu-id="c7db9-146">In a production app you would add *font-awesome.min.css* to the environment tag helper for `Staging,Production`.</span></span>

<span data-ttu-id="c7db9-147">内容替换*Views\Home\About.cshtml* Razor 文件替换为以下标记：</span><span class="sxs-lookup"><span data-stu-id="c7db9-147">Replace the contents of the *Views\Home\About.cshtml* Razor file with the following markup:</span></span>

[!code-html[Main](bower/sample/About.cshtml)]

<span data-ttu-id="c7db9-148">运行应用程序并导航到关于视图，以验证字体出色包正常运行。</span><span class="sxs-lookup"><span data-stu-id="c7db9-148">Run the app and navigate to the About view to verify the font-awesome package works.</span></span>

## <a name="exploring-the-client-side-build-process"></a><span data-ttu-id="c7db9-149">浏览客户端生成过程</span><span class="sxs-lookup"><span data-stu-id="c7db9-149">Exploring the client-side build process</span></span>

<span data-ttu-id="c7db9-150">大多数 ASP.NET Core 项目模板已配置为使用 Bower。</span><span class="sxs-lookup"><span data-stu-id="c7db9-150">Most ASP.NET Core project templates are already configured to use Bower.</span></span> <span data-ttu-id="c7db9-151">此下一个演练中创建空的 ASP.NET Core 项目启动，并手动添加每个部分，以便您可以如何在项目中使用 Bower 获取获得感觉。</span><span class="sxs-lookup"><span data-stu-id="c7db9-151">This next walkthrough starts with an empty ASP.NET Core project and adds each piece manually, so you can get a feel for how Bower is used in a project.</span></span> <span data-ttu-id="c7db9-152">你可以查看到的项目结构和运行时，输出会和每个配置更改会发生什么情况。</span><span class="sxs-lookup"><span data-stu-id="c7db9-152">You can see what happens to the project structure and the runtime output as each configuration change is made.</span></span>

<span data-ttu-id="c7db9-153">将客户端生成过程用于 Bower 的常规步骤如下：</span><span class="sxs-lookup"><span data-stu-id="c7db9-153">The general steps to use the client-side build process with Bower are:</span></span>

* <span data-ttu-id="c7db9-154">定义项目中使用的包。</span><span class="sxs-lookup"><span data-stu-id="c7db9-154">Define packages used in your project.</span></span> <!-- once defined, you don't need to download them, VS does -->
* <span data-ttu-id="c7db9-155">从 web 页面的引用包。</span><span class="sxs-lookup"><span data-stu-id="c7db9-155">Reference packages from your web pages.</span></span>

### <a name="define-packages"></a><span data-ttu-id="c7db9-156">定义包</span><span class="sxs-lookup"><span data-stu-id="c7db9-156">Define packages</span></span>

<span data-ttu-id="c7db9-157">一旦列表中的包*bower.json*文件，Visual Studio 将下载它们。</span><span class="sxs-lookup"><span data-stu-id="c7db9-157">Once you list packages in the *bower.json* file, Visual Studio will download them.</span></span> <span data-ttu-id="c7db9-158">下面的示例使用 Bower 加载 jQuery 和引导定向到*wwwroot*文件夹。</span><span class="sxs-lookup"><span data-stu-id="c7db9-158">The following example uses Bower to load jQuery and Bootstrap to the *wwwroot* folder.</span></span>

* <span data-ttu-id="c7db9-159">创建新的 ASP.NET 核心 Web 应用程序与**ASP.NET 核心 Web 应用程序 (.NET Core)**模板。</span><span class="sxs-lookup"><span data-stu-id="c7db9-159">Create a new ASP.NET Core Web app with the **ASP.NET Core Web Application (.NET Core)** template.</span></span> <span data-ttu-id="c7db9-160">选择**空**项目模板，然后单击**确定**。</span><span class="sxs-lookup"><span data-stu-id="c7db9-160">Select the **Empty** project template and click **OK**.</span></span>

* <span data-ttu-id="c7db9-161">在解决方案资源管理器，右键单击项目 >**添加新项**和选择**Bower 配置文件**。</span><span class="sxs-lookup"><span data-stu-id="c7db9-161">In Solution Explorer, right-click the project > **Add New Item** and select **Bower Configuration File**.</span></span> <span data-ttu-id="c7db9-162">注意： A *.bowerrc*还添加文件。</span><span class="sxs-lookup"><span data-stu-id="c7db9-162">Note: A *.bowerrc* file is also added.</span></span>

* <span data-ttu-id="c7db9-163">打开*bower.json*，并添加 jquery 和引导到`dependencies`部分。</span><span class="sxs-lookup"><span data-stu-id="c7db9-163">Open *bower.json*, and add jquery and bootstrap to the `dependencies` section.</span></span> <span data-ttu-id="c7db9-164">生成*bower.json*文件将如下所示下面的示例。</span><span class="sxs-lookup"><span data-stu-id="c7db9-164">The resulting *bower.json* file will look like the following example.</span></span> <span data-ttu-id="c7db9-165">版本将会发生更改，并且可能不匹配下图所示。</span><span class="sxs-lookup"><span data-stu-id="c7db9-165">The versions will change over time and may not match the image below.</span></span>

[!code-json[Main](bower/sample/bower.json?highlight=5,6)]

* <span data-ttu-id="c7db9-166">保存*bower.json*文件。</span><span class="sxs-lookup"><span data-stu-id="c7db9-166">Save the *bower.json* file.</span></span>

 <span data-ttu-id="c7db9-167">验证该项目包括*bootstrap*和*jQuery*中的目录*wwwroot/lib*。</span><span class="sxs-lookup"><span data-stu-id="c7db9-167">Verify the project includes the *bootstrap* and *jQuery* directories in *wwwroot/lib*.</span></span> <span data-ttu-id="c7db9-168">Bower 使用*.bowerrc*文件以安装中的资产*wwwroot/lib*。</span><span class="sxs-lookup"><span data-stu-id="c7db9-168">Bower uses the *.bowerrc* file to install the assets in *wwwroot/lib*.</span></span>

 <span data-ttu-id="c7db9-169">注意:"管理 Bower 包"UI 提供手动文件编辑的替代方法。</span><span class="sxs-lookup"><span data-stu-id="c7db9-169">Note: The "Manage Bower Packages" UI provides an alternative to manual file editing.</span></span>

### <a name="enable-static-files"></a><span data-ttu-id="c7db9-170">启用静态文件</span><span class="sxs-lookup"><span data-stu-id="c7db9-170">Enable static files</span></span>

* <span data-ttu-id="c7db9-171">添加`Microsoft.AspNetCore.StaticFiles`到项目的 NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="c7db9-171">Add the `Microsoft.AspNetCore.StaticFiles` NuGet package to the project.</span></span>
* <span data-ttu-id="c7db9-172">启用静态文件提供与[静态文件中间件](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions)。</span><span class="sxs-lookup"><span data-stu-id="c7db9-172">Enable static files to be served with the [Static file middleware](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions).</span></span> <span data-ttu-id="c7db9-173">添加对的调用[UseStaticFiles](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions)到`Configure`方法`Startup`。</span><span class="sxs-lookup"><span data-stu-id="c7db9-173">Add a call to [UseStaticFiles](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions) to the `Configure` method of `Startup`.</span></span>

[!code-csharp[Main](bower/sample/Startup.cs?highlight=9)]

### <a name="reference-packages"></a><span data-ttu-id="c7db9-174">引用包</span><span class="sxs-lookup"><span data-stu-id="c7db9-174">Reference packages</span></span>

<span data-ttu-id="c7db9-175">在本部分中，你将创建 HTML 页以验证它可以访问已部署的包。</span><span class="sxs-lookup"><span data-stu-id="c7db9-175">In this section, you will create an HTML page to verify it can access the deployed packages.</span></span>

* <span data-ttu-id="c7db9-176">添加一个名为的新 HTML 页*Index.html*到*wwwroot*文件夹。</span><span class="sxs-lookup"><span data-stu-id="c7db9-176">Add a new HTML page named *Index.html* to the *wwwroot* folder.</span></span> <span data-ttu-id="c7db9-177">注意： 你必须将添加到的 HTML 文件*wwwroot*文件夹。</span><span class="sxs-lookup"><span data-stu-id="c7db9-177">Note: You must add the HTML file to the *wwwroot* folder.</span></span> <span data-ttu-id="c7db9-178">默认情况下，静态内容无法提供外部*wwwroot*。</span><span class="sxs-lookup"><span data-stu-id="c7db9-178">By default, static content cannot be served outside *wwwroot*.</span></span> <span data-ttu-id="c7db9-179">请参阅[使用静态文件](xref:fundamentals/static-files)有关详细信息。</span><span class="sxs-lookup"><span data-stu-id="c7db9-179">See [Working with static files](xref:fundamentals/static-files) for more information.</span></span>

 <span data-ttu-id="c7db9-180">内容替换*Index.html*替换为以下标记：</span><span class="sxs-lookup"><span data-stu-id="c7db9-180">Replace the contents of *Index.html* with the following markup:</span></span>

[!code-html[Main](bower/sample/Index.html)]

* <span data-ttu-id="c7db9-181">运行应用程序并导航到`http://localhost:<port>/Index.html`。</span><span class="sxs-lookup"><span data-stu-id="c7db9-181">Run the app and navigate to `http://localhost:<port>/Index.html`.</span></span> <span data-ttu-id="c7db9-182">或者，使用*Index.html*打开，按`Ctrl+Shift+W`。</span><span class="sxs-lookup"><span data-stu-id="c7db9-182">Alternatively, with *Index.html* opened, press `Ctrl+Shift+W`.</span></span> <span data-ttu-id="c7db9-183">验证应用 jumbotron 样式，jQuery 代码响应时单击该按钮，以及启动按钮更改状态。</span><span class="sxs-lookup"><span data-stu-id="c7db9-183">Verify that the jumbotron styling is applied, the jQuery code responds when the button is clicked, and that the Bootstrap button changes state.</span></span>

 ![应用的 jumbotron 样式](bower/_static/jumbotron.png)
