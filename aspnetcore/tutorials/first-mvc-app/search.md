---
title: "添加搜索"
author: rick-anderson
description: "演示如何将搜索添加到简单的 ASP.NET Core MVC 应用"
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/search
ms.openlocfilehash: aaa00a9eedda73a2c66da30b5ace3b5b5a7760b2
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/19/2018
---
[!INCLUDE[adding-model](../../includes/mvc-intro/search1.md)]

可使用“重命名”命令快速将 `searchString` 参数重命名为 `id`。 右键单击“`searchString`”，选择“重命名”。

![上下文菜单](search/_static/rename.png)

突出显示重命名目标。

![代码编辑器，突出显示整个 Index ActionResult 方法中的变量](search/_static/rename2.png)

将参数更改为 `id`，并将出现的所有 `searchString` 更改为 `id`。

![代码编辑器，显示已更改为 id 的变量](search/_static/rename3.png)

[!INCLUDE[adding-model](../../includes/mvc-intro/search2.md)]

请注意 intelliSense 如何帮助更新标记。

![Intellisense 上下文菜单，在 form 元素的属性列表中选中了 method](search/_static/int_m.png)

![Intellisense 上下文菜单，在 method 属性值列表中选中了 get](search/_static/int_get.png)

请注意 `<form>` 标记中独特的字体。 该独特字体表明标记受[标记帮助程序](../../mvc/views/tag-helpers/intro.md)支持。

![使用紫色文本的 form 标记](search/_static/th_font.png)

[!INCLUDE[adding-model](../../includes/mvc-intro/search3.md)]

>[!div class="step-by-step"]
[上一页](controller-methods-views.md)
[下一页](new-field.md)  
