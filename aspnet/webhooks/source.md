---
uid: webhooks/source
title: "ASP.NET Webhook 源代码和 NuGet 包 |Microsoft 文档"
author: rick-anderson
description: "指向 ASP.NET Webhook 源代码和 NuGet 包"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 91a62bfa-ea3a-41f9-a2e1-e90d2c8fc8ca
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: ab5eaaa32a8f678b2aa4e2a0b30b674dbd6d6e9c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a><span data-ttu-id="ecbcf-103">ASP.NET Webhook 源代码和 NuGet 包</span><span class="sxs-lookup"><span data-stu-id="ecbcf-103">ASP.NET WebHooks source code and NuGet packages</span></span>

<span data-ttu-id="ecbcf-104">Microsoft ASP.NET Webhook 是模块的 Microsoft ASP.NET 系列的一部分，作为承载[GitHub 上的开放源项目](https://github.com/aspnet/WebHooks)。</span><span class="sxs-lookup"><span data-stu-id="ecbcf-104">Microsoft ASP.NET WebHooks is part of the Microsoft ASP.NET family of modules and is hosted as an [Open Source Project on GitHub](https://github.com/aspnet/WebHooks).</span></span> <span data-ttu-id="ecbcf-105">这意味着我们接受的贡献，但请查看[贡献准则](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md)之前提交拉取请求。</span><span class="sxs-lookup"><span data-stu-id="ecbcf-105">This means that we accept contributions, but please look at the [Contribution Guidelines](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) before submitting a pull request.</span></span>

<span data-ttu-id="ecbcf-106">你正在阅读此联机文档现在还作为承载[GitHub 上的开放源代码](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide)和还接受的贡献。</span><span class="sxs-lookup"><span data-stu-id="ecbcf-106">This online documentation which you are reading now is also hosted as [Open Source on GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) and also accepts contributions.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="ecbcf-107">Nuget 包</span><span class="sxs-lookup"><span data-stu-id="ecbcf-107">Nuget Packages</span></span>

<span data-ttu-id="ecbcf-108">Microsoft ASP.NET Webhook 也是作为预览 Nuget 包，这意味着你可以选择在 Visual Studio 中的预览标志，若要查看这些提供的。</span><span class="sxs-lookup"><span data-stu-id="ecbcf-108">Microsoft ASP.NET WebHooks is also available as preview Nuget packages which means that you have to select the Preview flag in Visual Studio in order to see them.</span></span>

<span data-ttu-id="ecbcf-109">[Nuget 包](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks)devided 为三个部分：</span><span class="sxs-lookup"><span data-stu-id="ecbcf-109">The [Nuget packages](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) are devided into three parts:</span></span>

* <span data-ttu-id="ecbcf-110">[常见](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common)： 发送者和接收者之间共享的常见包。</span><span class="sxs-lookup"><span data-stu-id="ecbcf-110">[Common](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): A common package that is shared between senders and receivers.</span></span>

* <span data-ttu-id="ecbcf-111">[发件人](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom)： 一组支持发送给他人自己 Webhook 的包。</span><span class="sxs-lookup"><span data-stu-id="ecbcf-111">[Sender](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): A set of packages supporting sending your own WebHooks to others.</span></span> <span data-ttu-id="ecbcf-112">用于发送 Webhook 的功能所述更详细地[发送 Webhook](sending/index.md)。</span><span class="sxs-lookup"><span data-stu-id="ecbcf-112">The functionality for sending WebHooks is described in more detail in [Sending WebHooks](sending/index.md).</span></span>

* <span data-ttu-id="ecbcf-113">[接收方](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers)： 一组支持其他设备接收 Webhook 的包。</span><span class="sxs-lookup"><span data-stu-id="ecbcf-113">[Receivers](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): A set of packages supporting receiving WebHooks from others.</span></span> <span data-ttu-id="ecbcf-114">用于接收 Webhook 的功能中的更多详细信息中所述[接收 Webhook](receiving/index.md)。</span><span class="sxs-lookup"><span data-stu-id="ecbcf-114">The functionality for receiving WebHooks is described in more detail in [Receiving WebHooks](receiving/index.md).</span></span>
