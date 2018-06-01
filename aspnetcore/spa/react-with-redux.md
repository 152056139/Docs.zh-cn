---
title: 通过 ASP.NET Core 使用带 Redux 的 React 项目模板
author: SteveSandersonMS
description: 了解如何开始使用适用于带 Redux 的 React 和 create-react-app 的 ASP.NET Core 单页应用程序 (SPA) 项目模板。
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/react-with-redux
ms.openlocfilehash: 9abfbfe5be69d3145de453d9d9e56ea35eec64ed
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2018
ms.locfileid: "30076286"
---
# <a name="use-the-react-with-redux-project-template-with-aspnet-core"></a><span data-ttu-id="c2f1b-103">通过 ASP.NET Core 使用带 Redux 的 React 项目模板</span><span class="sxs-lookup"><span data-stu-id="c2f1b-103">Use the React-with-Redux project template with ASP.NET Core</span></span>

> [!NOTE]
> <span data-ttu-id="c2f1b-104">本文档不涉及 ASP.NET Core 2.0 中包含的带 Redux 的 React项目模板。</span><span class="sxs-lookup"><span data-stu-id="c2f1b-104">This documentation isn't about the React-with-Redux project template included in ASP.NET Core 2.0.</span></span> <span data-ttu-id="c2f1b-105">本文介绍你可以手动更新的新版带 Redux 的 React 模板。</span><span class="sxs-lookup"><span data-stu-id="c2f1b-105">It's about the newer React-with-Redux template to which you can update manually.</span></span> <span data-ttu-id="c2f1b-106">该模板默认包含在 ASP.NET Core 2.1 中。</span><span class="sxs-lookup"><span data-stu-id="c2f1b-106">The template is included in ASP.NET Core 2.1 by default.</span></span>

<span data-ttu-id="c2f1b-107">更新的带 Redux 的 React 项目模板为使用 React、Redux 和 [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) 约定实现丰富的客户端用户界面 (UI) 的 ASP.NET Core 应用程序提供了便捷起点。</span><span class="sxs-lookup"><span data-stu-id="c2f1b-107">The updated React-with-Redux project template provides a convenient starting point for ASP.NET Core apps using React, Redux, and [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) conventions to implement a rich, client-side user interface (UI).</span></span>

<span data-ttu-id="c2f1b-108">除了项目创建命令外，关于带 Redux 的 React 模板的所有信息都与 React 模板相同。</span><span class="sxs-lookup"><span data-stu-id="c2f1b-108">With the exception of the project creation command, all information about the React-with-Redux template is the same as the React template.</span></span> <span data-ttu-id="c2f1b-109">要创建此项目类型，请运行 `dotnet new reactredux` 而不是 `dotnet new react`。</span><span class="sxs-lookup"><span data-stu-id="c2f1b-109">To create this project type, run `dotnet new reactredux` instead of `dotnet new react`.</span></span> <span data-ttu-id="c2f1b-110">有关这两个基于 React 的模板的通用功能的详细信息，请参阅 [React 模板文档](xref:spa/react)。</span><span class="sxs-lookup"><span data-stu-id="c2f1b-110">For more information about the functionality common to both React-based templates, see [React template documentation](xref:spa/react).</span></span>
