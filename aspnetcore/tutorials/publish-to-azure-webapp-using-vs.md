---
title: "使用 Visual Studio 将 ASP.NET Core 应用发布到 Azure"
author: rick-anderson
description: "了解如何使用 Visual Studio 将 ASP.NET Core 应用发布到 Azure 应用服务。"
ms.author: riande
manager: wpickett
ms.date: 12/16/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: d0e64c967ff332365981338809a47faf35d499ab
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a><span data-ttu-id="3be6f-103">使用 Visual Studio 将 ASP.NET Core Web 应用发布到 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="3be6f-103">Publish an ASP.NET Core web app to Azure App Service using Visual Studio</span></span>

<span data-ttu-id="3be6f-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT) 、[Cesar Blum Silveira](https://github.com/cesarbs) 和 [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="3be6f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs), and [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="3be6f-105">如果你在 Mac 上工作，请参阅[从 Visual Studio for Mac 发布到 Azure](https://blog.xamarin.com/publish-azure-visual-studio-mac/)。</span><span class="sxs-lookup"><span data-stu-id="3be6f-105">See [Publish to Azure from Visual Studio for Mac](https://blog.xamarin.com/publish-azure-visual-studio-mac/) if you are working on a Mac.</span></span>

## <a name="set-up"></a><span data-ttu-id="3be6f-106">设置</span><span class="sxs-lookup"><span data-stu-id="3be6f-106">Set up</span></span>

* <span data-ttu-id="3be6f-107">如果没有[免费 Azure 帐户](https://aka.ms/K5y5yh)，请开设一个。</span><span class="sxs-lookup"><span data-stu-id="3be6f-107">Open a [free Azure account](https://aka.ms/K5y5yh) if you don't have one.</span></span> 

## <a name="create-a-web-app"></a><span data-ttu-id="3be6f-108">创建 Web 应用</span><span class="sxs-lookup"><span data-stu-id="3be6f-108">Create a web app</span></span>

<span data-ttu-id="3be6f-109">在 Visual Studio 起始页中，选择“文件”>“新建”>“项目...”</span><span class="sxs-lookup"><span data-stu-id="3be6f-109">In the Visual Studio Start Page, select **File > New > Project...**</span></span>

![“文件”菜单](publish-to-azure-webapp-using-vs/_static/file_new_project.png)

<span data-ttu-id="3be6f-111">完成“新建项目”对话框：</span><span class="sxs-lookup"><span data-stu-id="3be6f-111">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="3be6f-112">在左侧窗格中，选择“.NET Core”。</span><span class="sxs-lookup"><span data-stu-id="3be6f-112">In the left pane, select **.NET Core**.</span></span>
* <span data-ttu-id="3be6f-113">在中间窗格中，选择“ASP.NET Core Web 应用程序”。</span><span class="sxs-lookup"><span data-stu-id="3be6f-113">In the center pane, select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="3be6f-114">选择“确定”。</span><span class="sxs-lookup"><span data-stu-id="3be6f-114">Select **OK**.</span></span>

![“新建项目”对话框](publish-to-azure-webapp-using-vs/_static/new_prj.png)

<span data-ttu-id="3be6f-116">在“新建 ASP.NET Core Web 应用程序”对话框中：</span><span class="sxs-lookup"><span data-stu-id="3be6f-116">In the **New ASP.NET Core Web Application** dialog:</span></span>

* <span data-ttu-id="3be6f-117">选择“Web 应用程序”。</span><span class="sxs-lookup"><span data-stu-id="3be6f-117">Select **Web Application**.</span></span>
* <span data-ttu-id="3be6f-118">选择“更改身份验证”。</span><span class="sxs-lookup"><span data-stu-id="3be6f-118">Select **Change Authentication**.</span></span>

![“新建项目”对话框](publish-to-azure-webapp-using-vs/_static/new_prj_2.png)

<span data-ttu-id="3be6f-120">“更改身份验证”对话框随即出现。</span><span class="sxs-lookup"><span data-stu-id="3be6f-120">The **Change Authentication** dialog appears.</span></span> 

* <span data-ttu-id="3be6f-121">选择“个人用户帐户”。</span><span class="sxs-lookup"><span data-stu-id="3be6f-121">Select **Individual User Accounts**.</span></span>
* <span data-ttu-id="3be6f-122">选择“确定”返回到“新建 ASP.NET Core Web 应用程序”，然后再次选择“确定”。</span><span class="sxs-lookup"><span data-stu-id="3be6f-122">Select **OK** to return to the **New ASP.NET Core Web Application**, then select **OK** again.</span></span>

![“新建 ASP.NET Core Web 身份验证”对话框](publish-to-azure-webapp-using-vs/_static/new_prj_auth.png) 

<span data-ttu-id="3be6f-124">Visual Studio 随即创建解决方案。</span><span class="sxs-lookup"><span data-stu-id="3be6f-124">Visual Studio creates the solution.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="3be6f-125">运行应用</span><span class="sxs-lookup"><span data-stu-id="3be6f-125">Run the app</span></span>

* <span data-ttu-id="3be6f-126">按 Ctrl+F5 运行项目。</span><span class="sxs-lookup"><span data-stu-id="3be6f-126">Press CTRL+F5 to run the project.</span></span>
* <span data-ttu-id="3be6f-127">测试“关于”和“联系”链接。</span><span class="sxs-lookup"><span data-stu-id="3be6f-127">Test the **About** and **Contact** links.</span></span>

![localhost 上的 Microsoft Edge 中打开的 Web 应用程序](publish-to-azure-webapp-using-vs/_static/show.png)

### <a name="register-a-user"></a><span data-ttu-id="3be6f-129">注册用户</span><span class="sxs-lookup"><span data-stu-id="3be6f-129">Register a user</span></span>

* <span data-ttu-id="3be6f-130">选择“注册”并注册新用户。</span><span class="sxs-lookup"><span data-stu-id="3be6f-130">Select **Register** and register a new user.</span></span> <span data-ttu-id="3be6f-131">可使用虚构电子邮件地址。</span><span class="sxs-lookup"><span data-stu-id="3be6f-131">You can use a fictitious email address.</span></span> <span data-ttu-id="3be6f-132">提交时，页面上会显示以下错误：</span><span class="sxs-lookup"><span data-stu-id="3be6f-132">When you submit, the page displays the following error:</span></span>

    <span data-ttu-id="3be6f-133">“内部服务器错误: 处理请求时，数据库操作失败。SQL 异常: 无法打开数据库。可通过向应用程序数据库上下文应用现有迁移解决此问题。”</span><span class="sxs-lookup"><span data-stu-id="3be6f-133">*"Internal Server Error: A database operation failed while processing the request. SQL exception: Cannot open the database. Applying existing migrations for Application DB context may resolve this issue."*</span></span>
* <span data-ttu-id="3be6f-134">选择“应用迁移”，并在页面更新后刷新页面。</span><span class="sxs-lookup"><span data-stu-id="3be6f-134">Select **Apply Migrations** and, once the page updates, refresh the page.</span></span>

![内部服务器错误: 处理请求时，数据库操作失败。](publish-to-azure-webapp-using-vs/_static/mig.png)

<span data-ttu-id="3be6f-138">应用将显示用于注册新用户的电子邮件和一个“注销”链接。</span><span class="sxs-lookup"><span data-stu-id="3be6f-138">The app displays the email used to register the new user and a **Log out** link.</span></span>

![Microsoft Edge 中打开的 Web 应用程序。](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="3be6f-141">将应用部署到 Azure</span><span class="sxs-lookup"><span data-stu-id="3be6f-141">Deploy the app to Azure</span></span>

<span data-ttu-id="3be6f-142">在解决方案资源管理器中右键单击该项目，然后选择“发布...”。</span><span class="sxs-lookup"><span data-stu-id="3be6f-142">Right-click on the project in Solution Explorer and select **Publish...**.</span></span>

![打开且突出显示“发布”链接的上下文菜单](publish-to-azure-webapp-using-vs/_static/pub.png)

<span data-ttu-id="3be6f-144">在“发布”对话框中：</span><span class="sxs-lookup"><span data-stu-id="3be6f-144">In the **Publish** dialog:</span></span>

* <span data-ttu-id="3be6f-145">选择“Microsoft Azure 应用服务”。</span><span class="sxs-lookup"><span data-stu-id="3be6f-145">Select **Microsoft Azure App Service**.</span></span>
* <span data-ttu-id="3be6f-146">选择齿轮图标，然后选择“创建配置文件”。</span><span class="sxs-lookup"><span data-stu-id="3be6f-146">Select the gear icon and then select **Create Profile**.</span></span>
* <span data-ttu-id="3be6f-147">选择“创建配置文件”。</span><span class="sxs-lookup"><span data-stu-id="3be6f-147">Select **Create Profile**.</span></span>

![“发布”对话框](publish-to-azure-webapp-using-vs/_static/maas1.png)

### <a name="create-azure-resources"></a><span data-ttu-id="3be6f-149">创建 Azure 资源</span><span class="sxs-lookup"><span data-stu-id="3be6f-149">Create Azure resources</span></span>

<span data-ttu-id="3be6f-150">“创建应用服务”对话框随即显示：</span><span class="sxs-lookup"><span data-stu-id="3be6f-150">The **Create App Service** dialog appears:</span></span>

* <span data-ttu-id="3be6f-151">输入订阅。</span><span class="sxs-lookup"><span data-stu-id="3be6f-151">Enter your subscription.</span></span>
* <span data-ttu-id="3be6f-152">“应用名称”、“资源组”和“应用服务计划”输入字段已填充。</span><span class="sxs-lookup"><span data-stu-id="3be6f-152">The **App Name**, **Resource Group**, and **App Service Plan** entry fields are populated.</span></span> <span data-ttu-id="3be6f-153">可以保留这些名称，也可以进行更改。</span><span class="sxs-lookup"><span data-stu-id="3be6f-153">You can keep these names or change them.</span></span>

![“应用服务”对话框](publish-to-azure-webapp-using-vs/_static/newrg1.png)

* <span data-ttu-id="3be6f-155">选择“服务”选项卡以创建新的数据库。</span><span class="sxs-lookup"><span data-stu-id="3be6f-155">Select the **Services** tab to create a new database.</span></span>

* <span data-ttu-id="3be6f-156">选择绿色的 + 图标以创建新的 SQL 数据库</span><span class="sxs-lookup"><span data-stu-id="3be6f-156">Select the green **+** icon to create a new SQL Database</span></span>

![新建 SQL 数据库](publish-to-azure-webapp-using-vs/_static/sql.png)

* <span data-ttu-id="3be6f-158">在“配置 SQL 数据库”对话框中选择“新建...”以创建新的数据库。</span><span class="sxs-lookup"><span data-stu-id="3be6f-158">Select **New...** on the **Configure SQL Database** dialog to create a new database.</span></span>

![新建 SQL 数据库和服务器](publish-to-azure-webapp-using-vs/_static/conf.png)

<span data-ttu-id="3be6f-160">“配置 SQL Server”对话框随即出现。</span><span class="sxs-lookup"><span data-stu-id="3be6f-160">The **Configure SQL Server** dialog appears.</span></span>

* <span data-ttu-id="3be6f-161">输入管理员用户名和密码，然后选择“确定”。</span><span class="sxs-lookup"><span data-stu-id="3be6f-161">Enter an administrator user name and password, and then select **OK**.</span></span> <span data-ttu-id="3be6f-162">可保留默认的“服务器名称”。</span><span class="sxs-lookup"><span data-stu-id="3be6f-162">You can keep the default **Server Name**.</span></span> 

> [!NOTE]
> <span data-ttu-id="3be6f-163">不可使用“admin”作为管理员用户名。</span><span class="sxs-lookup"><span data-stu-id="3be6f-163">"admin" isn't allowed as the administrator user name.</span></span>

![“配置 SQL Server”对话框](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

* <span data-ttu-id="3be6f-165">选择“确定”。</span><span class="sxs-lookup"><span data-stu-id="3be6f-165">Select **OK**.</span></span>

<span data-ttu-id="3be6f-166">Visual Studio 将返回到“创建应用服务”对话框。</span><span class="sxs-lookup"><span data-stu-id="3be6f-166">Visual Studio returns to the **Create App Service** dialog.</span></span>

* <span data-ttu-id="3be6f-167">选择“创建应用服务”对话框上的“创建”。</span><span class="sxs-lookup"><span data-stu-id="3be6f-167">Select **Create** on the **Create App Service** dialog.</span></span>

![“配置 SQL 数据库”对话框](publish-to-azure-webapp-using-vs/_static/conf_final.png)

<span data-ttu-id="3be6f-169">Visual Studio 在 Azure 上创建 Web 应用和 SQL Server。</span><span class="sxs-lookup"><span data-stu-id="3be6f-169">Visual Studio creates the Web app and SQL Server on Azure.</span></span> <span data-ttu-id="3be6f-170">此步骤可能需要几分钟。</span><span class="sxs-lookup"><span data-stu-id="3be6f-170">This step can take a few minutes.</span></span> <span data-ttu-id="3be6f-171">有关创建的资源的信息，请参阅[其他资源](#additonal-resources)。</span><span class="sxs-lookup"><span data-stu-id="3be6f-171">For information on the resources created, see [Additonal resources](#additonal-resources).</span></span>

<span data-ttu-id="3be6f-172">部署完成时，选择“设置”：</span><span class="sxs-lookup"><span data-stu-id="3be6f-172">When deployment completes, select **Settings**:</span></span>

![“配置 SQL Server”对话框](publish-to-azure-webapp-using-vs/_static/set.png)

<span data-ttu-id="3be6f-174">在“发布”对话框的“设置”页面上：</span><span class="sxs-lookup"><span data-stu-id="3be6f-174">On the **Settings** page of the **Publish** dialog:</span></span>

  * <span data-ttu-id="3be6f-175">展开“数据库”并选中“在运行时使用此连接字符串”。</span><span class="sxs-lookup"><span data-stu-id="3be6f-175">Expand **Databases** and check **Use this connection string at runtime**.</span></span>
  * <span data-ttu-id="3be6f-176">展开“Entity Framework 迁移”并选中“在发布时应用此迁移”。</span><span class="sxs-lookup"><span data-stu-id="3be6f-176">Expand **Entity Framework Migrations** and check **Apply this migration on publish**.</span></span>

* <span data-ttu-id="3be6f-177">选择“保存”。</span><span class="sxs-lookup"><span data-stu-id="3be6f-177">Select **Save**.</span></span> <span data-ttu-id="3be6f-178">Visual Studio 将返回到“发布”对话框。</span><span class="sxs-lookup"><span data-stu-id="3be6f-178">Visual Studio returns to the **Publish** dialog.</span></span> 

![“发布”对话框：设置面板](publish-to-azure-webapp-using-vs/_static/pubs.png)

<span data-ttu-id="3be6f-180">单击“发布” 。</span><span class="sxs-lookup"><span data-stu-id="3be6f-180">Click **Publish**.</span></span> <span data-ttu-id="3be6f-181">Visual Studio 将应用发布到 Azure。</span><span class="sxs-lookup"><span data-stu-id="3be6f-181">Visual Studio publishs your app to Azure.</span></span> <span data-ttu-id="3be6f-182">部署完成时，应用在浏览器中打开。</span><span class="sxs-lookup"><span data-stu-id="3be6f-182">When the depoyment completes, the app is opened in a browser.</span></span>

### <a name="test-your-app-in-azure"></a><span data-ttu-id="3be6f-183">在 Azure 中测试应用</span><span class="sxs-lookup"><span data-stu-id="3be6f-183">Test your app in Azure</span></span>

* <span data-ttu-id="3be6f-184">测试“关于”和“联系”链接</span><span class="sxs-lookup"><span data-stu-id="3be6f-184">Test the **About** and **Contact** links</span></span>

* <span data-ttu-id="3be6f-185">注册新用户</span><span class="sxs-lookup"><span data-stu-id="3be6f-185">Register a new user</span></span>

![Microsoft Edge 中 Azure App Service 上打开的 Web 应用程序](publish-to-azure-webapp-using-vs/_static/register.png)

### <a name="update-the-app"></a><span data-ttu-id="3be6f-187">更新应用</span><span class="sxs-lookup"><span data-stu-id="3be6f-187">Update the app</span></span>

* <span data-ttu-id="3be6f-188">编辑“Pages/About.cshtml”Razor 页面并更改其内容。</span><span class="sxs-lookup"><span data-stu-id="3be6f-188">Edit the *Pages/About.cshtml* Razor page and change its contents.</span></span> <span data-ttu-id="3be6f-189">例如，可以将段落修改为显示“Hello ASP.NET Core!”：[!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]</span><span class="sxs-lookup"><span data-stu-id="3be6f-189">For example, you can modify the paragraph to say "Hello ASP.NET Core!": [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]</span></span>

* <span data-ttu-id="3be6f-190">右键单击项目，然后再次选择“发布...”。</span><span class="sxs-lookup"><span data-stu-id="3be6f-190">Right-click on the project and select **Publish...** again.</span></span>

![打开且突出显示“发布”链接的上下文菜单](publish-to-azure-webapp-using-vs/_static/pub.png)

* <span data-ttu-id="3be6f-192">应用发布后，验证所做的更改在 Azure 上是否可用。</span><span class="sxs-lookup"><span data-stu-id="3be6f-192">After the app is published, verify the changes you made are available on Azure.</span></span>

![验证任务已完成](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="clean-up"></a><span data-ttu-id="3be6f-194">清理</span><span class="sxs-lookup"><span data-stu-id="3be6f-194">Clean up</span></span>

<span data-ttu-id="3be6f-195">完成应用测试后，转到 [Azure 门户](https://portal.azure.com/)并删除该应用。</span><span class="sxs-lookup"><span data-stu-id="3be6f-195">When you have finished testing the app, go to the [Azure portal](https://portal.azure.com/) and delete the app.</span></span>

* <span data-ttu-id="3be6f-196">选择“资源组”，然后选择所创建的资源组。</span><span class="sxs-lookup"><span data-stu-id="3be6f-196">Select **Resource groups**, then select the resource group you created.</span></span>

![Azure 门户：侧边栏菜单中的资源组](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* <span data-ttu-id="3be6f-198">在“资源组”页面中，选择“删除”。</span><span class="sxs-lookup"><span data-stu-id="3be6f-198">In the **Resource groups** page, select **Delete**.</span></span>

![Azure 门户：“资源组”页面](publish-to-azure-webapp-using-vs/_static/rgd.png)

* <span data-ttu-id="3be6f-200">输入资源组的名称并选择“删除”。</span><span class="sxs-lookup"><span data-stu-id="3be6f-200">Enter the name of the resource group and select **Delete**.</span></span> <span data-ttu-id="3be6f-201">现已从 Azure 中删除了本教程中创建的应用和其他所有资源。</span><span class="sxs-lookup"><span data-stu-id="3be6f-201">Your app and all other resources created in this tutorial are now deleted from Azure.</span></span>

### <a name="next-steps"></a><span data-ttu-id="3be6f-202">后续步骤</span><span class="sxs-lookup"><span data-stu-id="3be6f-202">Next steps</span></span>

* [<span data-ttu-id="3be6f-203">使用 Visual Studio 和 Git 持续部署到 Azure</span><span class="sxs-lookup"><span data-stu-id="3be6f-203">Continuous Deployment to Azure with Visual Studio and Git</span></span>](xref:host-and-deploy/azure-apps/azure-continuous-deployment)

## <a name="additonal-resources"></a><span data-ttu-id="3be6f-204">其他资源</span><span class="sxs-lookup"><span data-stu-id="3be6f-204">Additonal resources</span></span>

* [<span data-ttu-id="3be6f-205">Azure 应用服务</span><span class="sxs-lookup"><span data-stu-id="3be6f-205">Azure App Service</span></span>](https://docs.microsoft.com/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="3be6f-206">Azure 资源组</span><span class="sxs-lookup"><span data-stu-id="3be6f-206">Azure resource groups</span></span>](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups)
* [<span data-ttu-id="3be6f-207">Azure SQL 数据库</span><span class="sxs-lookup"><span data-stu-id="3be6f-207">Azure SQL Database</span></span>](https://docs.microsoft.com/azure/sql-database/)
