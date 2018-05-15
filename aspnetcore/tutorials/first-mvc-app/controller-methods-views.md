---
title: ASP.NET Core 中的控制器方法和视图
author: rick-anderson
description: 了解如何在 ASP.NET Core 中使用控制器方法、视图和 DataAnnotations。
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/controller-methods-views
ms.openlocfilehash: 6fe0a0e71079bebcbd3a76abee0f2917f562e766
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2018
---
# <a name="controller-methods-and-views-in-aspnet-core"></a><span data-ttu-id="48520-103">ASP.NET Core 中的控制器方法和视图</span><span class="sxs-lookup"><span data-stu-id="48520-103">Controller methods and views in ASP.NET Core</span></span>

<span data-ttu-id="48520-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="48520-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="48520-105">我们的电影应用有个不错的开始，但是展示效果还不够理想。</span><span class="sxs-lookup"><span data-stu-id="48520-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="48520-106">我们不希望看到时间（如下图所示的 12:00:00 AM），并且“ReleaseDate”应为两个词。</span><span class="sxs-lookup"><span data-stu-id="48520-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be two words.</span></span>

![索引视图：Release Date 为一个词（没有空格），每个电影发行日期都显示时间中午 12 点](working-with-sql/_static/m55.png)

<span data-ttu-id="48520-108">打开 Models/Movie.cs 文件，并添加以下代码中突出显示的行：</span><span class="sxs-lookup"><span data-stu-id="48520-108">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]

<span data-ttu-id="48520-109">右键单击红色的波浪线，然后选择“快速操作和重构”。</span><span class="sxs-lookup"><span data-stu-id="48520-109">Right click on a red squiggly line **> Quick Actions and Refactorings**.</span></span>

  ![上下文菜单中显示“快速操作和重构”。](controller-methods-views/_static/qa.png)


<span data-ttu-id="48520-111">点击 `using System.ComponentModel.DataAnnotations;`</span><span class="sxs-lookup"><span data-stu-id="48520-111">Tap `using System.ComponentModel.DataAnnotations;`</span></span>

  ![使用列表顶部的 System.ComponentModel.DataAnnotations](controller-methods-views/_static/da.png)

  <span data-ttu-id="48520-113">Visual studio 添加 `using System.ComponentModel.DataAnnotations;`。</span><span class="sxs-lookup"><span data-stu-id="48520-113">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

<span data-ttu-id="48520-114">删除不需要的 `using` 语句。</span><span class="sxs-lookup"><span data-stu-id="48520-114">Let's remove the `using` statements that are not needed.</span></span> <span data-ttu-id="48520-115">默认情况下，它们显示为浅灰色字体。</span><span class="sxs-lookup"><span data-stu-id="48520-115">They show up by default in a light grey font.</span></span> <span data-ttu-id="48520-116">右键单击 Movie.cs 文件中的任意位置，然后选择“对 Using 进行删除和排序”。</span><span class="sxs-lookup"><span data-stu-id="48520-116">Right click anywhere in the *Movie.cs* file **> Remove and Sort Usings**.</span></span>

![对 Using 进行删除和排序](controller-methods-views/_static/rm.png)

<span data-ttu-id="48520-118">更新的代码：</span><span class="sxs-lookup"><span data-stu-id="48520-118">The updated code:</span></span>

[!code-csharp[](./start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1)]

<!-- include start -->

[!INCLUDE [adding-model](../../includes/mvc-intro/controller-methods-views.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="48520-119">[上一页](working-with-sql.md)
> [下一页](search.md)</span><span class="sxs-lookup"><span data-stu-id="48520-119">[Previous](working-with-sql.md)
[Next](search.md)</span></span>  
