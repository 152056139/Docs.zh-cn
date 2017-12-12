---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-cs
title: "动态填充控件 (C#) |Microsoft 文档"
author: wenz
description: "ASP.NET AJAX 控件工具包中的 DynamicPopulate 控件调用 web 服务 （或页方法），并将生成的值填充到 t 上的目标控件..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e1fec43e-1daf-49d2-b0c7-7f1b930455cc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: a1868a0e4cec4a95d4175ce255fea2e200692075
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="dynamically-populating-a-control-c"></a><span data-ttu-id="5b5f8-103">动态填充控件 (C#)</span><span class="sxs-lookup"><span data-stu-id="5b5f8-103">Dynamically Populating a Control (C#)</span></span>
====================
<span data-ttu-id="5b5f8-104">通过[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="5b5f8-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="5b5f8-105">[下载代码](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.cs.zip)或[下载 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="5b5f8-105">[Download Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0CS.pdf)</span></span>

> <span data-ttu-id="5b5f8-106">ASP.NET AJAX 控件工具包中的 DynamicPopulate 控件调用 web 服务 （或页方法），并将生成的值填充到目标控件在页上，而无需页面刷新。</span><span class="sxs-lookup"><span data-stu-id="5b5f8-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span>


## <a name="overview"></a><span data-ttu-id="5b5f8-107">概述</span><span class="sxs-lookup"><span data-stu-id="5b5f8-107">Overview</span></span>

<span data-ttu-id="5b5f8-108">`DynamicPopulate` ASP.NET AJAX 控件工具包中的控件调用 web 服务 （或页方法），并将生成的值填充到目标控件在页上，而无需页面刷新。</span><span class="sxs-lookup"><span data-stu-id="5b5f8-108">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="5b5f8-109">本教程演示如何对此进行设置。</span><span class="sxs-lookup"><span data-stu-id="5b5f8-109">This tutorial shows how to set this up.</span></span>

## <a name="steps"></a><span data-ttu-id="5b5f8-110">步骤</span><span class="sxs-lookup"><span data-stu-id="5b5f8-110">Steps</span></span>

<span data-ttu-id="5b5f8-111">首先，你需要 ASP.NET Web 服务实现调用的方法`DynamicPopulate`。</span><span class="sxs-lookup"><span data-stu-id="5b5f8-111">First of all, you need an ASP.NET Web Service which implements the method to be called by `DynamicPopulate`.</span></span> <span data-ttu-id="5b5f8-112">Web 服务类需要`ScriptService`中定义的特性`Microsoft.Web.Script.Services`; 否则 ASP.NET AJAX 无法创建 web 服务，反过来则需要通过客户端 JavaScript 代理`DynamicPopulate`。</span><span class="sxs-lookup"><span data-stu-id="5b5f8-112">The web service class requires the `ScriptService` attribute which is defined within `Microsoft.Web.Script.Services`; otherwise ASP.NET AJAX cannot create the client-side JavaScript proxy for the web service which in turn is required by `DynamicPopulate`.</span></span>

<span data-ttu-id="5b5f8-113">Web 方法就必须预料到一个自变量的类型字符串，调用`contextKey`，因为`DynamicPopulate`控件将发送一条与每个 web 服务调用上下文信息。</span><span class="sxs-lookup"><span data-stu-id="5b5f8-113">The web method must expect one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="5b5f8-114">以下 web 服务所表示的格式返回当前日期`contextKey`自变量：</span><span class="sxs-lookup"><span data-stu-id="5b5f8-114">The following web service returns the current date in a format represented by the `contextKey` argument:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample1.aspx)]

<span data-ttu-id="5b5f8-115">Web 服务然后另存为`DynamicPopulate.cs.asmx`。</span><span class="sxs-lookup"><span data-stu-id="5b5f8-115">The web service is then saved as `DynamicPopulate.cs.asmx`.</span></span> <span data-ttu-id="5b5f8-116">或者，可以实现`getDate()`方法包含实际的 ASP.NET 页中的页方法作为`DynamicPopulate`控件。</span><span class="sxs-lookup"><span data-stu-id="5b5f8-116">Alternatively, you could implement the `getDate()` method as a page method within the actual ASP.NET page with the `DynamicPopulate` control.</span></span>

<span data-ttu-id="5b5f8-117">在下一步的步骤中，创建一个新的 ASP.NET 文件。</span><span class="sxs-lookup"><span data-stu-id="5b5f8-117">In the next step, create a new ASP.NET file.</span></span> <span data-ttu-id="5b5f8-118">如往常一样，第一步是包括`ScriptManager`当前页来加载 ASP.NET AJAX 库并使控件工具包工作中：</span><span class="sxs-lookup"><span data-stu-id="5b5f8-118">As always, the first step is to include the `ScriptManager` in the current page to load the ASP.NET AJAX library and to make the Control Toolkit work:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample2.aspx)]

<span data-ttu-id="5b5f8-119">然后，添加一个标签控件 (例如使用相同的名称，该 HTML 控件或&lt; `asp:Label`  / &gt; web 控件) 的更高版本将显示 web 服务调用的结果。</span><span class="sxs-lookup"><span data-stu-id="5b5f8-119">Then, add a label control (for instance using the HTML control of the same name, or the &lt;`asp:Label` /&gt; web control) which will later show the result of the web service call.</span></span>

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample3.aspx)]

<span data-ttu-id="5b5f8-120">然后将使用 （作为 HTML 控件，因为我们不需要回发到服务器） HTML 按钮来触发动态填充：</span><span class="sxs-lookup"><span data-stu-id="5b5f8-120">An HTML button (as an HTML control, since we do not require a postback to the server) will then be used to trigger the dynamic population:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample4.aspx)]

<span data-ttu-id="5b5f8-121">最后，我们需要`DynamicPopulateExtender`网络操作的控件。</span><span class="sxs-lookup"><span data-stu-id="5b5f8-121">Finally, we need the `DynamicPopulateExtender` control to wire things up.</span></span> <span data-ttu-id="5b5f8-122">将设置以下属性 (除了明显的`ID`和`runat` = `"server"`):</span><span class="sxs-lookup"><span data-stu-id="5b5f8-122">The following attributes will be set (apart from the obvious ones, `ID` and `runat`=`"server"`):</span></span>

- <span data-ttu-id="5b5f8-123">`TargetControlID`从 web 服务调用放置结果</span><span class="sxs-lookup"><span data-stu-id="5b5f8-123">`TargetControlID` where to put the result from the web service call</span></span>
- <span data-ttu-id="5b5f8-124">`ServicePath`web 服务的路径 （如果你想要使用的页方法忽略）</span><span class="sxs-lookup"><span data-stu-id="5b5f8-124">`ServicePath` path to the web service (omit if you want to use a page method)</span></span>
- <span data-ttu-id="5b5f8-125">`ServiceMethod`web 方法或页方法的名称</span><span class="sxs-lookup"><span data-stu-id="5b5f8-125">`ServiceMethod` name of the web method or page method</span></span>
- <span data-ttu-id="5b5f8-126">`ContextKey`上下文信息发送到 web 服务</span><span class="sxs-lookup"><span data-stu-id="5b5f8-126">`ContextKey` context information to be sent to the web service</span></span>
- <span data-ttu-id="5b5f8-127">`PopulateTriggerControlID`随即将会触发 web 服务调用的元素</span><span class="sxs-lookup"><span data-stu-id="5b5f8-127">`PopulateTriggerControlID` element which triggers the web service call</span></span>
- <span data-ttu-id="5b5f8-128">`ClearContentsDuringUpdate`是否要在 web 服务调用期间将空的目标元素</span><span class="sxs-lookup"><span data-stu-id="5b5f8-128">`ClearContentsDuringUpdate` whether to empty the target element during the web service call</span></span>

<span data-ttu-id="5b5f8-129">如你所见，该控件所需的一些信息，但将所有内容放入到位是相当直接。</span><span class="sxs-lookup"><span data-stu-id="5b5f8-129">As you can see, the control requires some information but putting everything into place is quite straight-forward.</span></span> <span data-ttu-id="5b5f8-130">下面是有关标记`DynamicPopulateExtender`当前应用场景中的控件：</span><span class="sxs-lookup"><span data-stu-id="5b5f8-130">Here is the markup for the `DynamicPopulateExtender` control in the current scenario:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample5.aspx)]

<span data-ttu-id="5b5f8-131">在浏览器中运行 ASP.NET 页，然后单击按钮;你将收到月-日-年格式的当前日期。</span><span class="sxs-lookup"><span data-stu-id="5b5f8-131">Run the ASP.NET page in the browser and click on the button; you will receive the current date in month-day-year format.</span></span>


<span data-ttu-id="5b5f8-132">[![单击按钮从服务器中检索日期](dynamically-populating-a-control-cs/_static/image2.png)](dynamically-populating-a-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="5b5f8-132">[![A click on the button retrieves the date from the server](dynamically-populating-a-control-cs/_static/image2.png)](dynamically-populating-a-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="5b5f8-133">单击按钮从服务器中检索日期 ([单击以查看实际尺寸的图像](dynamically-populating-a-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="5b5f8-133">A click on the button retrieves the date from the server ([Click to view full-size image](dynamically-populating-a-control-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="5b5f8-134">下一篇</span><span class="sxs-lookup"><span data-stu-id="5b5f8-134">Next</span></span>](dynamically-populating-a-control-using-javascript-code-cs.md)
