---
title: "更新生成的页面"
author: rick-anderson
description: "更新生成的页面，提供更好的显示效果。"
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/da1
ms.openlocfilehash: a1bb1ab1e4fac9c634f4048947ac3f934af3d625
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="update-the-generated-pages"></a><span data-ttu-id="f49b3-103">更新生成的页面</span><span class="sxs-lookup"><span data-stu-id="f49b3-103">Update the generated pages</span></span>

<span data-ttu-id="f49b3-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f49b3-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f49b3-105">我们的电影应用有个不错的开始，但是展示效果还不够理想。</span><span class="sxs-lookup"><span data-stu-id="f49b3-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="f49b3-106">我们不希望看到时间（如下图所示的 12:00:00 AM），并且“ReleaseDate”应为“Release Date”（两个词）。</span><span class="sxs-lookup"><span data-stu-id="f49b3-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be **Release Date** (two words).</span></span>

![在 Chrome 中打开的显示电影数据的电影应用程序](sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="f49b3-108">更新生成的代码</span><span class="sxs-lookup"><span data-stu-id="f49b3-108">Update the generated code</span></span>

<span data-ttu-id="f49b3-109">打开 Models/Movie.cs 文件，并添加以下代码中突出显示的行：</span><span class="sxs-lookup"><span data-stu-id="f49b3-109">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]

<span data-ttu-id="f49b3-110">右键单击红色波浪线，然后单击“快速操作和重构”。</span><span class="sxs-lookup"><span data-stu-id="f49b3-110">Right click on a red squiggly line > ** Quick Actions and Refactorings**.</span></span>

  ![上下文菜单中显示“快速操作和重构”。](da1/qa.png)

<span data-ttu-id="f49b3-112">选择 `using System.ComponentModel.DataAnnotations;`</span><span class="sxs-lookup"><span data-stu-id="f49b3-112">Select `using System.ComponentModel.DataAnnotations;`</span></span>

  ![使用列表顶部的 System.ComponentModel.DataAnnotations](da1/da.png)

  <span data-ttu-id="f49b3-114">Visual studio 添加 `using System.ComponentModel.DataAnnotations;`。</span><span class="sxs-lookup"><span data-stu-id="f49b3-114">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

[!INCLUDE[model1](../../includes/RP/da2.md)]

>[!div class="step-by-step"]
<span data-ttu-id="f49b3-115">[上一篇：使用 SQL Server LocalDB](xref:tutorials/razor-pages/sql)
[添加搜索](xref:tutorials/razor-pages/search)</span><span class="sxs-lookup"><span data-stu-id="f49b3-115">[Previous: Working with SQL Server LocalDB](xref:tutorials/razor-pages/sql)
[Adding Search](xref:tutorials/razor-pages/search)</span></span>
