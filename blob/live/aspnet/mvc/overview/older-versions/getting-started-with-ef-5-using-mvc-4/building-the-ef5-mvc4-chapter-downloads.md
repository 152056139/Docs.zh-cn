---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
title: "生成章下载 EF 5 MVC 的 4 教程 |Microsoft 文档"
author: Rick-Anderson
description: "Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 5 Code First 和 Visual Studio 的 ASP.NET MVC 4 应用程序..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: d0a89089-eed8-4f61-a478-c5ffa30186f5
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
msc.type: authoredcontent
ms.openlocfilehash: 912a1383ed170b49782657372abc1801140df8dd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="building-the-chapter-downloads-for-the-ef-5-mvc-4-tutorials"></a><span data-ttu-id="e3b40-103">生成章下载 EF 5 MVC 的 4 教程</span><span class="sxs-lookup"><span data-stu-id="e3b40-103">Building the Chapter Downloads for the EF 5 MVC 4 Tutorials</span></span>
====================
<span data-ttu-id="e3b40-104">通过[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="e3b40-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[<span data-ttu-id="e3b40-105">下载已完成的项目</span><span class="sxs-lookup"><span data-stu-id="e3b40-105">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> <span data-ttu-id="e3b40-106">Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 5 Code First 和 Visual Studio 2012 的 ASP.NET MVC 4 应用程序。</span><span class="sxs-lookup"><span data-stu-id="e3b40-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 4 applications using the Entity Framework 5 Code First and Visual Studio 2012.</span></span> <span data-ttu-id="e3b40-107">有关教程系列的信息，请参阅[序列中的第一个教程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。</span><span class="sxs-lookup"><span data-stu-id="e3b40-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>


## <a name="building-the-chapter-downloads"></a><span data-ttu-id="e3b40-108">生成章下载</span><span class="sxs-lookup"><span data-stu-id="e3b40-108">Building the Chapter Downloads</span></span>

1. <span data-ttu-id="e3b40-109">下载并解压缩项目示例的 zip 文件。</span><span class="sxs-lookup"><span data-stu-id="e3b40-109">Download and unzip the  project sample zip file.</span></span> <span data-ttu-id="e3b40-110">在已解压缩的下载包中，你将找到其他 zip 文件，其中一个为完成的每一章。</span><span class="sxs-lookup"><span data-stu-id="e3b40-110">In the unzipped download package, you will find additional zip files, one for the completion of each chapter.</span></span>
2. <span data-ttu-id="e3b40-111">右键单击所需的 zip 文件中，单击**属性**，然后单击**解除阻止**按钮。</span><span class="sxs-lookup"><span data-stu-id="e3b40-111">Right click on the desired zip file, click **Properties**, and click the **Unblock** button.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image1.png)
3. <span data-ttu-id="e3b40-112">将文件解压缩。</span><span class="sxs-lookup"><span data-stu-id="e3b40-112">Unzip the file.</span></span>
4. <span data-ttu-id="e3b40-113">双击*CUx.sln*文件以启动 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="e3b40-113">Double-click the *CUx.sln* file to launch Visual Studio.</span></span>
5. <span data-ttu-id="e3b40-114">从**工具**菜单上，单击**库程序包管理器**，然后**程序包管理器控制台**。</span><span class="sxs-lookup"><span data-stu-id="e3b40-114">From the **Tools** menu, click **Library Package Manager**, then **Package Manager Console**.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image2.png)
6. <span data-ttu-id="e3b40-115">在包管理器控制台 (PMC) 中，单击**还原**。</span><span class="sxs-lookup"><span data-stu-id="e3b40-115">In the Package Manager Console (PMC), click **Restore**.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image3.png)
7. <span data-ttu-id="e3b40-116">退出 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="e3b40-116">Exit Visual Studio.</span></span>
8. <span data-ttu-id="e3b40-117">重新启动 Visual Studio 中，打开在前面步骤中你关闭的解决方案文件。</span><span class="sxs-lookup"><span data-stu-id="e3b40-117">Restart Visual Studio, opening the solution file you closed in the step above.</span></span>
9. <span data-ttu-id="e3b40-118">在包管理器控制台 (PMC) 中，输入`Update-Database`命令：</span><span class="sxs-lookup"><span data-stu-id="e3b40-118">In the Package Manager Console (PMC), enter the `Update-Database` command:</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image4.png)  

    > [!NOTE]
    > <span data-ttu-id="e3b40-119">如果你收到以下错误：</span><span class="sxs-lookup"><span data-stu-id="e3b40-119">If you get the following error:</span></span>  
    >   
    >  <span data-ttu-id="e3b40-120">*术语更新数据库未被识别为 cmdlet、 函数、 脚本文件或可操作程序的名称。请检查拼写的名称，或如果包括路径，请验证路径正确，然后重试。*</span><span class="sxs-lookup"><span data-stu-id="e3b40-120">*The term 'Update-Database' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is correct and try again.*</span></span>  
    > <span data-ttu-id="e3b40-121">退出并重新启动 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="e3b40-121">Exit and restart Visual Studio.</span></span>

    <span data-ttu-id="e3b40-122">每个迁移要在运行，然后将运行 seed 方法。</span><span class="sxs-lookup"><span data-stu-id="e3b40-122">Each migration will run, then the seed method will run.</span></span> <span data-ttu-id="e3b40-123">你现在可以运行应用程序。</span><span class="sxs-lookup"><span data-stu-id="e3b40-123">You can now run the app.</span></span>

    ![](building-the-ef5-mvc4-chapter-downloads/_static/image5.png)

>[!div class="step-by-step"]
[<span data-ttu-id="e3b40-124">上一篇</span><span class="sxs-lookup"><span data-stu-id="e3b40-124">Previous</span></span>](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
