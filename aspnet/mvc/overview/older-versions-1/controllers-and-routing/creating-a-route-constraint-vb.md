---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
title: 创建路由约束 (VB) |Microsoft Docs
author: StephenWalther
description: 在本教程中，Stephen Walther 演示了如何控制如何浏览器请求的匹配路由通过使用正则表达式中创建路由约束。
ms.author: riande
ms.date: 02/16/2009
ms.assetid: b7cce113-c82c-45bf-b97b-357e5d9f7f56
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 0a8e540a00d852d5b710bfdbf63a68f6e6d280ee
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824730"
---
<a name="creating-a-route-constraint-vb"></a>创建路由约束 (VB)
====================
通过[Stephen Walther](https://github.com/StephenWalther)

> 在本教程中，Stephen Walther 演示了如何控制如何浏览器请求的匹配路由通过使用正则表达式中创建路由约束。


使用路由约束来限制相匹配的特定路由的浏览器请求。 可以使用正则表达式来指定路由约束。

例如，假设您具有路由中定义列表 1 中在 Global.asax 文件中。

**代码清单 1-Global.asax.vb**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample1.vb)]

代码清单 1 包含名为产品的路由。 产品路由可用于将浏览器请求映射到包含在代码清单 2 ProductController。

**代码清单 2-Controllers\ProductController.vb**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample2.vb)]

请注意，产品控制器公开的 Details() 操作接受一个名为产品 id 的单个参数。 此参数是一个整数参数。

在列表 1 中定义的路由将与任何以下 Url:

- / 产品/23
- 日产品/7

遗憾的是，路由也会匹配以下 Url:

- / 产品/被废弃
- / 产品/apple

因为 Details() 操作需要的一个整数参数，则发出请求，其中包含一个整数值以外的内容会导致错误。 例如，如果你的浏览器中键入 URL /Product/apple 然后将图 1 中显示错误页。


[![新建项目对话框](creating-a-route-constraint-vb/_static/image1.jpg)](creating-a-route-constraint-vb/_static/image1.png)

**图 01**： 看到 explode 页面 ([单击以查看实际尺寸的图像](creating-a-route-constraint-vb/_static/image2.png))


您真正想要做什么是仅匹配包含正确的整数 productId 的 Url。 您可以使用约束定义路由时以限制与路由匹配的 Url。 列表 3 中的已修改的产品路由包含仅与整数相匹配的正则表达式约束。

**代码清单 3-Global.asax.vb**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample3.vb)]

正则表达式 \d+ 匹配一个或多个整数。 此约束会导致产品路由，以匹配以下 Url:

- / 产品/3
- / 产品/8999

但不是允许以下 Url:

- / 产品/apple
- / 产品

这些浏览器请求将由另一个路由或如果没有匹配的路由*找不到资源*将返回错误。

> [!div class="step-by-step"]
> [上一页](creating-custom-routes-vb.md)
> [下一页](creating-a-custom-route-constraint-vb.md)
