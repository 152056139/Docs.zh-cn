---
title: 将搜索添加到 ASP.NET Core MVC 应用
author: rick-anderson
description: 演示如何将搜索添加到简单的 ASP.NET Core MVC 应用
ms.author: riande
ms.date: 03/07/2017
uid: tutorials/first-mvc-app/search
ms.openlocfilehash: d0581142b505323385feba441b707d29b3f6db5d
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/26/2018
ms.locfileid: "36960835"
---
[!INCLUDE [adding-model](~/includes/mvc-intro/search1.md)]

<span data-ttu-id="ac852-103">可使用“重命名”命令快速将 `searchString` 参数重命名为 `id`。</span><span class="sxs-lookup"><span data-stu-id="ac852-103">You can quickly rename the `searchString` parameter to `id` with the **rename** command.</span></span> <span data-ttu-id="ac852-104">右键单击“`searchString`”，选择“重命名”。</span><span class="sxs-lookup"><span data-stu-id="ac852-104">Right click on `searchString` **> Rename**.</span></span>

![上下文菜单](search/_static/rename.png)

<span data-ttu-id="ac852-106">突出显示重命名目标。</span><span class="sxs-lookup"><span data-stu-id="ac852-106">The rename targets are highlighted.</span></span>

![代码编辑器，突出显示整个 Index ActionResult 方法中的变量](search/_static/rename2.png)

<span data-ttu-id="ac852-108">将参数更改为 `id`，并将出现的所有 `searchString` 更改为 `id`。</span><span class="sxs-lookup"><span data-stu-id="ac852-108">Change the parameter to `id` and all occurrences of `searchString` change to `id`.</span></span>

![代码编辑器，显示已更改为 id 的变量](search/_static/rename3.png)

[!INCLUDE [adding-model](~/includes/mvc-intro/search2.md)]

<span data-ttu-id="ac852-110">请注意 intelliSense 如何帮助更新标记。</span><span class="sxs-lookup"><span data-stu-id="ac852-110">Notice how intelliSense helps us update the markup.</span></span>

![Intellisense 上下文菜单，在 form 元素的属性列表中选中了 method](search/_static/int_m.png)

![Intellisense 上下文菜单，在 method 属性值列表中选中了 get](search/_static/int_get.png)

<span data-ttu-id="ac852-113">请注意 `<form>` 标记中独特的字体。</span><span class="sxs-lookup"><span data-stu-id="ac852-113">Notice the distinctive font in the `<form>` tag.</span></span> <span data-ttu-id="ac852-114">该独特字体表明标记受[标记帮助程序](~/mvc/views/tag-helpers/intro.md)支持。</span><span class="sxs-lookup"><span data-stu-id="ac852-114">That distinctive font indicates the tag is supported by [Tag Helpers](~/mvc/views/tag-helpers/intro.md).</span></span>

![使用紫色文本的 form 标记](search/_static/th_font.png)

[!INCLUDE [adding-model](~/includes/mvc-intro/search3.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="ac852-116">[上一页](controller-methods-views.md)
> [下一页](new-field.md)</span><span class="sxs-lookup"><span data-stu-id="ac852-116">[Previous](controller-methods-views.md)
[Next](new-field.md)</span></span>  
