---
title: 将控制器添加到 ASP.NET Core MVC 应用
author: rick-anderson
description: 了解如何将控制器添加到简单的 ASP.NET Core MVC 应用。
ms.author: riande
ms.date: 02/28/2017
uid: tutorials/first-mvc-app/adding-controller
ms.openlocfilehash: bf4ac33103d525194524e7578902e6f985dbe7c2
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276586"
---
# <a name="add-a-controller-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="bc504-103">将控制器添加到 ASP.NET Core MVC 应用</span><span class="sxs-lookup"><span data-stu-id="bc504-103">Add a controller to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="bc504-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="bc504-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [adding-controller1](~/includes/mvc-intro/adding-controller1.md)]

* <span data-ttu-id="bc504-105">在“解决方案资源管理器”中，右键单击“控制器”，选择“添加”>“新项”</span><span class="sxs-lookup"><span data-stu-id="bc504-105">In **Solution Explorer**, right-click **Controllers > Add > New Item**</span></span>

![上下文菜单](adding-controller/_static/add_controller.png)

* <span data-ttu-id="bc504-107">选择“控制器类”</span><span class="sxs-lookup"><span data-stu-id="bc504-107">Select **Controller Class**</span></span>
* <span data-ttu-id="bc504-108">在“添加新项”对话框中，输入“HelloWorldController”。</span><span class="sxs-lookup"><span data-stu-id="bc504-108">In the **Add New Item** dialog, enter **HelloWorldController**.</span></span>

![添加 MVC 控制器并为其命名](adding-controller/_static/ac.png)

[!INCLUDE [adding-controller2](~/includes/mvc-intro/adding-controller2.md)]

<span data-ttu-id="bc504-110">在 Visual Studio 的非调试模式下 (Ctrl+F5)，不需要在更改代码后生成应用。</span><span class="sxs-lookup"><span data-stu-id="bc504-110">In Visual Studio, in non-debug mode (Ctrl+F5), you don't need to build the app after changing  code.</span></span> <span data-ttu-id="bc504-111">只需要保存文件并更新浏览器，就可以看到所做的更改。</span><span class="sxs-lookup"><span data-stu-id="bc504-111">Just save the file, refresh your browser and you can see the changes.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="bc504-112">[上一页](start-mvc.md)
> [下一页](adding-view.md)</span><span class="sxs-lookup"><span data-stu-id="bc504-112">[Previous](start-mvc.md)
[Next](adding-view.md)</span></span>