---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
title: 在 TFS 中创建团队项目 |Microsoft 文档
author: jrjlee
description: 本主题介绍如何创建在 Team Foundation Server (TFS) 2010年的新团队项目。
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: b28d3e2d-0bb4-4e29-a780-af810b964722
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
msc.type: authoredcontent
ms.openlocfilehash: 96e0ee5fd0b74e7b22b8e346aa8462f7558a3ddc
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/26/2018
ms.locfileid: "36960689"
---
<a name="creating-a-team-project-in-tfs"></a><span data-ttu-id="9d186-103">在 TFS 中创建团队项目</span><span class="sxs-lookup"><span data-stu-id="9d186-103">Creating a Team Project in TFS</span></span>
====================
<span data-ttu-id="9d186-104">通过[Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="9d186-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="9d186-105">下载 PDF</span><span class="sxs-lookup"><span data-stu-id="9d186-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="9d186-106">本主题介绍如何创建在 Team Foundation Server (TFS) 2010年的新团队项目。</span><span class="sxs-lookup"><span data-stu-id="9d186-106">This topic describes how to create a new team project in Team Foundation Server (TFS) 2010.</span></span>


<span data-ttu-id="9d186-107">本主题窗体的基于名为 Fabrikam，Inc.的虚构公司的企业部署要求的教程系列中的一部分本系列教程使用的示例解决方案&#x2014;[联系人管理器解决方案](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;来表示具有现实级别的复杂性，包括 ASP.NET MVC 3 应用程序，Windows 通信的 web 应用程序Foundation (WCF) 服务和数据库项目。</span><span class="sxs-lookup"><span data-stu-id="9d186-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

## <a name="task-overview"></a><span data-ttu-id="9d186-108">任务概述</span><span class="sxs-lookup"><span data-stu-id="9d186-108">Task Overview</span></span>

<span data-ttu-id="9d186-109">若要设置并在 TFS 中使用新的团队项目，你将需要完成以下高级步骤：</span><span class="sxs-lookup"><span data-stu-id="9d186-109">To provision and use a new team project in TFS, you'll need to complete these high-level steps:</span></span>

- <span data-ttu-id="9d186-110">向将创建新的团队项目的用户授予权限。</span><span class="sxs-lookup"><span data-stu-id="9d186-110">Grant permissions to the user who will create the new team project.</span></span>
- <span data-ttu-id="9d186-111">创建团队项目。</span><span class="sxs-lookup"><span data-stu-id="9d186-111">Create the team project.</span></span>
- <span data-ttu-id="9d186-112">向将在项目工作的团队成员授予权限。</span><span class="sxs-lookup"><span data-stu-id="9d186-112">Grant permissions to the team members who will work on the project.</span></span>
- <span data-ttu-id="9d186-113">签入一些内容。</span><span class="sxs-lookup"><span data-stu-id="9d186-113">Check in some content.</span></span>

<span data-ttu-id="9d186-114">本主题将演示如何执行这些过程，以及它将确定用户和作业很可能是负责针对每个步骤的角色。</span><span class="sxs-lookup"><span data-stu-id="9d186-114">This topic will show you how to perform these procedures, and it will identify the users and job roles that are likely to be responsible for each procedure.</span></span> <span data-ttu-id="9d186-115">请注意，具体取决于你的组织的结构，其中每个任务可能是不同的人员的责任。</span><span class="sxs-lookup"><span data-stu-id="9d186-115">Be aware that, depending on the structure of your organization, each of these tasks may be the responsibility of a different person.</span></span>

<span data-ttu-id="9d186-116">任务和本主题中的演练假定你已安装并配置 TFS，，你已创建团队项目集合配置过程的一部分。</span><span class="sxs-lookup"><span data-stu-id="9d186-116">The tasks and walkthroughs in this topic assume that you've installed and configured TFS, and that you've created a team project collection as part of the configuration process.</span></span> <span data-ttu-id="9d186-117">有关这些假设，详细信息和方案的更多常规背景信息，请参阅[用于 Web 部署配置 TFS 生成服务器](configuring-a-tfs-build-server-for-web-deployment.md)。</span><span class="sxs-lookup"><span data-stu-id="9d186-117">For more information on these assumptions, and for more general background information on the scenario, see [Configure a TFS Build Server for Web Deployment](configuring-a-tfs-build-server-for-web-deployment.md).</span></span>

## <a name="grant-permissions-to-the-team-project-creator"></a><span data-ttu-id="9d186-118">向团队项目创建者授予权限</span><span class="sxs-lookup"><span data-stu-id="9d186-118">Grant Permissions to the Team Project Creator</span></span>

<span data-ttu-id="9d186-119">若要创建新的团队项目，你需要这些权限：</span><span class="sxs-lookup"><span data-stu-id="9d186-119">In order to create a new team project, you need these permissions:</span></span>

- <span data-ttu-id="9d186-120">你必须具有**创建新项目**TFS 应用层上的权限。</span><span class="sxs-lookup"><span data-stu-id="9d186-120">You must have the **Create new projects** permission on the TFS application tier.</span></span> <span data-ttu-id="9d186-121">通常通过将用户添加到授予此权限**项目集合管理员**TFS 组。</span><span class="sxs-lookup"><span data-stu-id="9d186-121">You typically grant this permission by adding users to the **Project Collection Administrators** TFS group.</span></span> <span data-ttu-id="9d186-122">**Team Foundation Administrators**全局组还包括此权限。</span><span class="sxs-lookup"><span data-stu-id="9d186-122">The **Team Foundation Administrators** global group also includes this permission.</span></span>
- <span data-ttu-id="9d186-123">你必须有权创建 SharePoint 站点集合对应的 TFS 团队项目集合中的新团队站点。</span><span class="sxs-lookup"><span data-stu-id="9d186-123">You must have permission to create new team sites within the SharePoint site collection that corresponds to the TFS team project collection.</span></span> <span data-ttu-id="9d186-124">通常通过将用户添加到与 SharePoint 组授予此权限**完全控制**权限在 sharepoint 站点集合。</span><span class="sxs-lookup"><span data-stu-id="9d186-124">You typically grant this permission by adding the user to a SharePoint group with **Full Control** rights on the SharePoint site collection.</span></span>
- <span data-ttu-id="9d186-125">如果你使用的 SQL Server Reporting Services 功能，你必须属于**Team Foundation 内容管理器**Reporting Services 中的角色。</span><span class="sxs-lookup"><span data-stu-id="9d186-125">If you're using SQL Server Reporting Services features, you must be a member of the **Team Foundation Content Manager** role in Reporting Services.</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="9d186-126">这些过程的执行者是谁？</span><span class="sxs-lookup"><span data-stu-id="9d186-126">Who Performs These Procedures?</span></span>

<span data-ttu-id="9d186-127">通常情况下，用户或组的管理 TFS 部署还执行这些过程。</span><span class="sxs-lookup"><span data-stu-id="9d186-127">Typically, the person or group who administers the TFS deployment also performs these procedures.</span></span>

<span data-ttu-id="9d186-128">由于这是一个高特权的权限集，新团队项目，通常由创建用户一小部分与负责管理 TFS 部署。</span><span class="sxs-lookup"><span data-stu-id="9d186-128">Because this is a highly privileged set of permissions, new team projects are typically created by a small subset of users with responsibility for administering a TFS deployment.</span></span> <span data-ttu-id="9d186-129">开发人员将不通常被授予创建新的团队项目所需的权限。</span><span class="sxs-lookup"><span data-stu-id="9d186-129">Developers will not usually be granted the permissions required to create new team projects.</span></span>

### <a name="grant-permissions-in-tfs"></a><span data-ttu-id="9d186-130">授予在 TFS 中的权限</span><span class="sxs-lookup"><span data-stu-id="9d186-130">Grant Permissions in TFS</span></span>

<span data-ttu-id="9d186-131">如果你想要使用户能够创建新的团队项目，第一项高级任务是将用户添加到**项目集合管理员**组为团队项目集合。</span><span class="sxs-lookup"><span data-stu-id="9d186-131">If you want to enable a user to create new team projects, the first high-level task is to add the user to the **Project Collection Administrators** group for the team project collection.</span></span>

<span data-ttu-id="9d186-132">**若要将用户添加到项目集合管理员组**</span><span class="sxs-lookup"><span data-stu-id="9d186-132">**To add a user to the Project Collection Administrators group**</span></span>

1. <span data-ttu-id="9d186-133">在 TFS 服务器上，在**启动**菜单上，指向**所有程序**，单击**Microsoft Team Foundation Server 2010**，然后单击**Team Foundation管理控制台**。</span><span class="sxs-lookup"><span data-stu-id="9d186-133">On the TFS server, on the **Start** menu, point to **All Programs**, click **Microsoft Team Foundation Server 2010**, and then click **Team Foundation Administration Console**.</span></span>
2. <span data-ttu-id="9d186-134">在导航树视图中，展开**应用程序层**，然后单击**团队项目集合**。</span><span class="sxs-lookup"><span data-stu-id="9d186-134">In the navigation tree view, expand **Application Tier**, and then click **Team Project Collections**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image1.png)
3. <span data-ttu-id="9d186-135">在**团队项目集合**窗格中，选择团队项目你想要管理的集合。</span><span class="sxs-lookup"><span data-stu-id="9d186-135">In the **Team Project Collections** pane, select the team project collection you want to manage.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image2.png)
4. <span data-ttu-id="9d186-136">上**常规**选项卡上，单击**组成员身份**。</span><span class="sxs-lookup"><span data-stu-id="9d186-136">On the **General** tab, click **Group Membership**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image3.png)
5. <span data-ttu-id="9d186-137">在**全局组**对话框中，选择**项目集合管理员**分组，并依次**属性**。</span><span class="sxs-lookup"><span data-stu-id="9d186-137">In the **Global Groups** dialog box, select the **Project Collection Administrators** group, and then click **Properties**.</span></span>
6. <span data-ttu-id="9d186-138">在**Team Foundation Server 组属性**对话框中，选择**Windows 用户或组**，然后单击**添加**。</span><span class="sxs-lookup"><span data-stu-id="9d186-138">In the **Team Foundation Server Group Properties** dialog box, select **Windows User or Group**, and then click **Add**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image4.png)
7. <span data-ttu-id="9d186-139">在**选择用户、 计算机或组**对话框中，键入你想要能够创建新的团队项目，用户的用户名单击**检查名称**，然后单击**确定**.</span><span class="sxs-lookup"><span data-stu-id="9d186-139">In the **Select Users, Computers, or Groups** dialog box, type the user name of the user you want to be able to create new team projects, click **Check Names**, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image5.png)
8. <span data-ttu-id="9d186-140">在**Team Foundation Server 组属性**对话框中，单击**确定**。</span><span class="sxs-lookup"><span data-stu-id="9d186-140">In the **Team Foundation Server Group Properties** dialog box, click **OK**.</span></span>
9. <span data-ttu-id="9d186-141">在**全局组**对话框中，单击**关闭**。</span><span class="sxs-lookup"><span data-stu-id="9d186-141">In the **Global Groups** dialog box, click **Close**.</span></span>

### <a name="grant-permissions-in-sharepoint-services"></a><span data-ttu-id="9d186-142">授予 SharePoint Services 中的权限</span><span class="sxs-lookup"><span data-stu-id="9d186-142">Grant Permissions in SharePoint Services</span></span>

<span data-ttu-id="9d186-143">接下来，你需要为用户授予 SharePoint 站点集合对应于 TFS 团队项目集合中创建新的团队网站的权限。</span><span class="sxs-lookup"><span data-stu-id="9d186-143">Next, you need to give the user permission to create new team sites in the SharePoint site collection that corresponds to your TFS team project collection.</span></span>

<span data-ttu-id="9d186-144">**若要授予对 SharePoint 站点集合的完全控制权限**</span><span class="sxs-lookup"><span data-stu-id="9d186-144">**To grant Full Control permissions on the SharePoint site collection**</span></span>

1. <span data-ttu-id="9d186-145">在 Team Foundation Server 的管理控制台中，在**团队项目集合**页上，选择你想要管理的团队项目集合。</span><span class="sxs-lookup"><span data-stu-id="9d186-145">In the Team Foundation Server Administration Console, on the **Team Project Collections** page, select the team project collection you want to manage.</span></span>
2. <span data-ttu-id="9d186-146">上**SharePoint 站点**选项卡上，记下的值**当前默认站点位置**URL。</span><span class="sxs-lookup"><span data-stu-id="9d186-146">On the **SharePoint Site** tab, note the value of the **Current Default Site Location** URL.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image6.png)
3. <span data-ttu-id="9d186-147">打开 Internet Explorer，然后转到步骤 2 中记下的 URL。</span><span class="sxs-lookup"><span data-stu-id="9d186-147">Open Internet Explorer, and then go to the URL you noted in step 2.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9d186-148">如果不作为创建团队项目集合的用户登录到 Windows，你将需要以登录到 SharePoint 此用户才能继续。</span><span class="sxs-lookup"><span data-stu-id="9d186-148">If you're not logged on to Windows as the user who created the team project collection, you'll need to sign in to SharePoint as this user in order to continue.</span></span>
4. <span data-ttu-id="9d186-149">上**站点操作**菜单上，单击**站点设置**。</span><span class="sxs-lookup"><span data-stu-id="9d186-149">On the **Site Actions** menu, click **Site Settings**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image7.png)
5. <span data-ttu-id="9d186-150">上**站点设置**页上，在**用户和权限**，单击**人员和组**。</span><span class="sxs-lookup"><span data-stu-id="9d186-150">On the **Site Settings** page, under **Users and Permissions**, click **People and groups**.</span></span>
6. <span data-ttu-id="9d186-151">在左侧的导航面板中，单击**组**。</span><span class="sxs-lookup"><span data-stu-id="9d186-151">In the left navigation panel, click **Groups**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image8.png)
7. <span data-ttu-id="9d186-152">上**人员和组： 所有组**页上，单击**为此站点设置用户组**。</span><span class="sxs-lookup"><span data-stu-id="9d186-152">On the **People and Groups: All Groups** page, click **Set Up Groups for this Site**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image9.png)

   > [!NOTE]
   > <span data-ttu-id="9d186-153">你可能会收到<strong>HTTP 404 未找到</strong>由于 double 的 HTTP 编码 bug 的错误。</span><span class="sxs-lookup"><span data-stu-id="9d186-153">You may receive an <strong>HTTP 404 Not Found</strong> error due to a double HTTP encoding bug.</span></span> <span data-ttu-id="9d186-154">如果发生这种情况，将替换此 URL:</span><span class="sxs-lookup"><span data-stu-id="9d186-154">If this occurs, replace the URL with this:</span></span>   
   > <span data-ttu-id="9d186-155">`[site_collection_URL]/_layouts/permsetup.aspx` 例如：</span><span class="sxs-lookup"><span data-stu-id="9d186-155">`[site_collection_URL]/_layouts/permsetup.aspx` For example:</span></span>  
   > `http://tfs/sites/Fabrikam%20Web%20Projects/_layouts/permsetup.aspx` 
8. <span data-ttu-id="9d186-156">上**为此站点设置用户组**页上，添加将创建到团队项目的用户**所有者**分组，并依次**确定**。</span><span class="sxs-lookup"><span data-stu-id="9d186-156">On the **Set Up Groups for this Site** page, add the user who will create team projects to the **Owners** group, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image10.png)

<span data-ttu-id="9d186-157">允许用户创建团队项目集合中的新团队项目的详细信息，请参阅[为团队项目集合设置管理员权限](https://msdn.microsoft.com/library/dd547204.aspx)。</span><span class="sxs-lookup"><span data-stu-id="9d186-157">For more information on enabling users to create new team projects within a team project collection, see [Set Administrator Permissions for Team Project Collections](https://msdn.microsoft.com/library/dd547204.aspx).</span></span>

## <a name="create-a-new-team-project-and-add-users"></a><span data-ttu-id="9d186-158">创建新的团队项目并添加用户</span><span class="sxs-lookup"><span data-stu-id="9d186-158">Create a New Team Project and Add Users</span></span>

<span data-ttu-id="9d186-159">所需的权限之后，你可以使用**团队资源管理器**Visual Studio 2010 创建新的团队项目中的窗口。</span><span class="sxs-lookup"><span data-stu-id="9d186-159">Once you have the necessary permissions, you can use the **Team Explorer** window in Visual Studio 2010 to create a new team project.</span></span> <span data-ttu-id="9d186-160">此方法提供了收集所有必要的信息并执行必要的任务在 TFS、 SharePoint 和 SQL Server Reporting Services 中的向导。</span><span class="sxs-lookup"><span data-stu-id="9d186-160">This approach provides a wizard that collects all the required information and performs the necessary tasks in TFS, SharePoint, and SQL Server Reporting Services.</span></span> <span data-ttu-id="9d186-161">你还需要新的团队项目的权限授予的开发人员团队的成员，以使他们能够添加和修改内容。</span><span class="sxs-lookup"><span data-stu-id="9d186-161">You'll also need to grant permissions on the new team project to members of the developer team, to enable them to add and modify content.</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="9d186-162">这些过程的执行者是谁？</span><span class="sxs-lookup"><span data-stu-id="9d186-162">Who Performs These Procedures?</span></span>

<span data-ttu-id="9d186-163">通常是 TFS 管理员或开发人员团队主管执行这些过程。</span><span class="sxs-lookup"><span data-stu-id="9d186-163">Usually either a TFS administrator or a developer team leader performs these procedures.</span></span>

### <a name="create-a-new-team-project"></a><span data-ttu-id="9d186-164">创建新的团队项目</span><span class="sxs-lookup"><span data-stu-id="9d186-164">Create a New Team Project</span></span>

<span data-ttu-id="9d186-165">下一步的过程描述如何在 TFS 2010 中创建新的团队项目。</span><span class="sxs-lookup"><span data-stu-id="9d186-165">The next procedure describes how to create a new team project in TFS 2010.</span></span>

<span data-ttu-id="9d186-166">**若要创建新的团队项目**</span><span class="sxs-lookup"><span data-stu-id="9d186-166">**To create a new team project**</span></span>

1. <span data-ttu-id="9d186-167">上**启动**菜单上，指向**所有程序**，单击**Microsoft Visual Studio 2010**，右键单击**Microsoft Visual Studio 2010**，然后单击**以管理员身份运行**。</span><span class="sxs-lookup"><span data-stu-id="9d186-167">On the **Start** menu, point to **All Programs**, click **Microsoft Visual Studio 2010**, right-click **Microsoft Visual Studio 2010**, and then click **Run as administrator**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9d186-168">如果你不以管理员身份运行 Visual Studio 2010，新的团队项目向导将失败的最后一个步骤。</span><span class="sxs-lookup"><span data-stu-id="9d186-168">If you don't run Visual Studio 2010 as an administrator, the New Team Project Wizard will fail on the last step.</span></span>
2. <span data-ttu-id="9d186-169">如果**用户帐户控制**显示对话框，请单击**是**。</span><span class="sxs-lookup"><span data-stu-id="9d186-169">If the **User Account Control** dialog box appears, click **Yes**.</span></span>
3. <span data-ttu-id="9d186-170">在 Visual Studio 中，在**团队**菜单上，单击**连接到 Team Foundation Server**。</span><span class="sxs-lookup"><span data-stu-id="9d186-170">In Visual Studio, on the **Team** menu, click **Connect to Team Foundation Server**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9d186-171">如果你已配置 TFS 服务器的连接，则可以省略步骤 4-7。</span><span class="sxs-lookup"><span data-stu-id="9d186-171">If you have already configured a connection to a TFS server, you can omit steps 4-7.</span></span>
4. <span data-ttu-id="9d186-172">在**连接到团队项目**对话框中，单击**服务器**。</span><span class="sxs-lookup"><span data-stu-id="9d186-172">In the **Connection to Team Project** dialog box, click **Servers**.</span></span>
5. <span data-ttu-id="9d186-173">在**添加/移除 Team Foundation Server**对话框中，单击**添加**。</span><span class="sxs-lookup"><span data-stu-id="9d186-173">In the **Add/Remove Team Foundation Server** dialog box, click **Add**.</span></span>
6. <span data-ttu-id="9d186-174">在**添加 Team Foundation Server**对话框中，提供你的 TFS 实例的详细信息，然后单击**确定**。</span><span class="sxs-lookup"><span data-stu-id="9d186-174">In the **Add Team Foundation Server** dialog box, provide the details of your TFS instance, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image11.png)
7. <span data-ttu-id="9d186-175">在**添加/移除 Team Foundation Server**对话框中，单击**关闭**。</span><span class="sxs-lookup"><span data-stu-id="9d186-175">In the **Add/Remove Team Foundation Server** dialog box, click **Close**.</span></span>
8. <span data-ttu-id="9d186-176">在**连接到团队项目**对话框中，选择你想要连接到，选择团队的 TFS 实例项目的集合你想要添加到，，然后单击**连接**。</span><span class="sxs-lookup"><span data-stu-id="9d186-176">In the **Connect to Team Project** dialog box, select the TFS instance you want to connect to, select the team project collection you want to add to, and then click **Connect**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image12.png)
9. <span data-ttu-id="9d186-177">在**团队资源管理器**窗口中，右键单击团队项目集合，然后单击**新团队项目**。</span><span class="sxs-lookup"><span data-stu-id="9d186-177">In the **Team Explorer** window, right-click the team project collection, and then click **New Team Project**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image13.png)
10. <span data-ttu-id="9d186-178">在**新团队项目**对话框中，提供名称和团队项目的描述，然后单击**下一步**。</span><span class="sxs-lookup"><span data-stu-id="9d186-178">In the **New Team Project** dialog box, provide a name and a description for the team project, and then click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9d186-179">如果你的团队项目包含空格，您可能会遇到一些问题，到达以 Internet 信息服务 (IIS) Web 部署工具 （Web 部署） 用于部署的输出路径中的包。</span><span class="sxs-lookup"><span data-stu-id="9d186-179">If your team project includes spaces, you may face some issues when you come to use the Internet Information Services (IIS) Web Deployment Tool (Web Deploy) to deploy packages from the output path.</span></span> <span data-ttu-id="9d186-180">路径中的空格会使运行 Web 部署命令更加困难。</span><span class="sxs-lookup"><span data-stu-id="9d186-180">Spaces in the path can make it a lot more difficult to run Web Deploy commands.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image14.png)
11. <span data-ttu-id="9d186-181">上**选择过程模板**页上，选择你想要使用以管理开发过程中，然后单击的过程模板**下一步**。</span><span class="sxs-lookup"><span data-stu-id="9d186-181">On the **Select a Process Template** page, select the process template that you want to use to manage the development process, and then click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9d186-182">有关 TFS 过程模板的更多信息，请参阅[过程模板和工具](https://msdn.microsoft.com/vstudio/aa718795)。</span><span class="sxs-lookup"><span data-stu-id="9d186-182">For more information on process templates for TFS, see [Process Templates and Tools](https://msdn.microsoft.com/vstudio/aa718795).</span></span>
12. <span data-ttu-id="9d186-183">上**团队站点设置**页上，保留默认设置保持不变，，然后单击**下一步**。</span><span class="sxs-lookup"><span data-stu-id="9d186-183">On the **Team Site Settings** page, leave the default settings unchanged, and then click **Next**.</span></span>
13. <span data-ttu-id="9d186-184">此设置创建，或标识，与 TFS 团队项目相关联的 SharePoint 团队站点。</span><span class="sxs-lookup"><span data-stu-id="9d186-184">This setting creates, or identifies, a SharePoint team site that is associated with the TFS team project.</span></span> <span data-ttu-id="9d186-185">你的开发团队可以使用此站点来管理文档、 参与讨论、 创建 wiki 页面和执行代码的不相关的各种其他任务。</span><span class="sxs-lookup"><span data-stu-id="9d186-185">Your development team can use this site to manage documentation, participate in discussion threads, create wiki pages, and perform various other tasks that are not related to code.</span></span> <span data-ttu-id="9d186-186">有关详细信息，请参阅[交互之间 SharePoint 产品和 Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx)。</span><span class="sxs-lookup"><span data-stu-id="9d186-186">For more information, see [Interactions Between SharePoint Products and Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx).</span></span>
14. <span data-ttu-id="9d186-187">上**指定源代码管理设置**页上，保留默认设置保持不变，，然后单击**下一步**。</span><span class="sxs-lookup"><span data-stu-id="9d186-187">On the **Specify Source Control Settings** page, leave the default settings unchanged, and then click **Next**.</span></span>
15. <span data-ttu-id="9d186-188">此设置标识，或在 TFS 文件夹层次结构将充当你的内容的根文件夹中创建的位置。</span><span class="sxs-lookup"><span data-stu-id="9d186-188">This setting identifies or creates the location in the TFS folder hierarchy that will act as a root folder for your content.</span></span>
16. <span data-ttu-id="9d186-189">上**确认团队项目设置**页上，单击**完成**。</span><span class="sxs-lookup"><span data-stu-id="9d186-189">On the **Confirm Team Project Settings** page, click **Finish**.</span></span>
17. <span data-ttu-id="9d186-190">在新的团队项目已成功创建，在**创建的团队项目**页上，单击**关闭**。</span><span class="sxs-lookup"><span data-stu-id="9d186-190">When the new team project is successfully created, on the **Team Project Created** page, click **Close**.</span></span>

### <a name="add-users-to-a-team-project"></a><span data-ttu-id="9d186-191">将用户添加到团队项目</span><span class="sxs-lookup"><span data-stu-id="9d186-191">Add Users to a Team Project</span></span>

<span data-ttu-id="9d186-192">现在，你已创建新的团队项目，可以将权限授予用户，使他们可以开始添加和对内容进行协作。</span><span class="sxs-lookup"><span data-stu-id="9d186-192">Now that you've created the new team project, you can grant permissions to users to enable them to start adding and collaborating on content.</span></span>

<span data-ttu-id="9d186-193">**若要将用户添加到团队项目**</span><span class="sxs-lookup"><span data-stu-id="9d186-193">**To add users to a team project**</span></span>

1. <span data-ttu-id="9d186-194">在 Visual Studio 2010 中，在**团队资源管理器**窗口中，右键单击团队项目，指向**团队项目设置**，然后单击**组成员身份**。</span><span class="sxs-lookup"><span data-stu-id="9d186-194">In Visual Studio 2010, in the **Team Explorer** window, right-click the team project, point to **Team Project Settings**, and then click **Group Membership**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image15.png)
2. <span data-ttu-id="9d186-195">若要使用户能够添加、 修改和删除在源代码管理下的代码，你需要添加他或她到**contributors （参与者)** 组。</span><span class="sxs-lookup"><span data-stu-id="9d186-195">To enable a user to add, modify, and remove code under source control, you need to add him or her to the **Contributors** group.</span></span>
3. <span data-ttu-id="9d186-196">在**项目组**对话框中，选择**contributors （参与者)** 分组，并依次**属性**。</span><span class="sxs-lookup"><span data-stu-id="9d186-196">In the **Project Groups** dialog box, select the **Contributors** group, and then click **Properties**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image16.png)
4. <span data-ttu-id="9d186-197">在**Team Foundation Server 组属性**对话框中，选择**Windows 用户或组**，然后单击**添加**。</span><span class="sxs-lookup"><span data-stu-id="9d186-197">In the **Team Foundation Server Group Properties** dialog box, select **Windows User or Group**, and then click **Add**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image17.png)
5. <span data-ttu-id="9d186-198">在**选择用户、 计算机或组**对话框中，键入你想要添加到团队项目中，用户的用户名单击**检查名称**，然后单击**确定**。</span><span class="sxs-lookup"><span data-stu-id="9d186-198">In the **Select Users, Computers, or Groups** dialog box, type the user name of the user you want to add to the team project, click **Check Names**, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image18.png)
6. <span data-ttu-id="9d186-199">在**Team Foundation Server 组属性**对话框中，单击**确定**。</span><span class="sxs-lookup"><span data-stu-id="9d186-199">In the **Team Foundation Server Group Properties** dialog box, click **OK**.</span></span>
7. <span data-ttu-id="9d186-200">在**项目组**对话框中，单击**关闭**。</span><span class="sxs-lookup"><span data-stu-id="9d186-200">In the **Project Groups** dialog box, click **Close**.</span></span>

## <a name="conclusion"></a><span data-ttu-id="9d186-201">结束语</span><span class="sxs-lookup"><span data-stu-id="9d186-201">Conclusion</span></span>

<span data-ttu-id="9d186-202">此时，新的团队项目已准备好使用，并且你的开发人员团队可以开始添加内容和开发过程进行协作。</span><span class="sxs-lookup"><span data-stu-id="9d186-202">At this point, your new team project is ready to use, and your developer team can start adding content and collaborating on the development process.</span></span>

<span data-ttu-id="9d186-203">下一主题[将内容添加到源代码管理](adding-content-to-source-control.md)，介绍如何将内容添加到源代码管理。</span><span class="sxs-lookup"><span data-stu-id="9d186-203">The next topic, [Adding Content to Source Control](adding-content-to-source-control.md), describes how to add content to source control.</span></span>

## <a name="further-reading"></a><span data-ttu-id="9d186-204">其他阅读材料</span><span class="sxs-lookup"><span data-stu-id="9d186-204">Further Reading</span></span>

<span data-ttu-id="9d186-205">有关在 TFS 中创建团队项目的更广泛指南，请参阅[创建团队项目](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx)。</span><span class="sxs-lookup"><span data-stu-id="9d186-205">For broader guidance on creating team projects in TFS, see [Create a Team Project](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx).</span></span> <span data-ttu-id="9d186-206">允许用户创建团队项目集合中的新团队项目的详细信息，请参阅[为团队项目集合设置管理员权限](https://msdn.microsoft.com/library/dd547204.aspx)。</span><span class="sxs-lookup"><span data-stu-id="9d186-206">For more information on enabling users to create new team projects within a team project collection, see [Set Administrator Permissions for Team Project Collections](https://msdn.microsoft.com/library/dd547204.aspx).</span></span> <span data-ttu-id="9d186-207">将用户添加到团队项目的详细信息，请参阅[向团队项目添加用户](https://msdn.microsoft.com/library/bb558971.aspx)。</span><span class="sxs-lookup"><span data-stu-id="9d186-207">For more information on adding users to team projects, see [Add Users to Team Projects](https://msdn.microsoft.com/library/bb558971.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9d186-208">[上一页](configuring-team-foundation-server-for-web-deployment.md)
> [下一页](adding-content-to-source-control.md)</span><span class="sxs-lookup"><span data-stu-id="9d186-208">[Previous](configuring-team-foundation-server-for-web-deployment.md)
[Next](adding-content-to-source-control.md)</span></span>
