---
title: 基于 ASP.NET 核心项目使用单个用户帐户创建项目
author: rick-anderson
description: 发现基于 ASP.NET Core 项目使用单个用户帐户创建的文章。
manager: wpickett
ms.author: riande
ms.date: 11/30/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/individual
ms.openlocfilehash: 699def0133f53b922477ac294f70db41998945ef
ms.sourcegitcommit: a0b6319c36f41cdce76ea334372f6e14fc66507e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/02/2018
ms.locfileid: "34729543"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a>基于 ASP.NET 核心项目使用单个用户帐户创建项目

ASP.NET 核心标识包含在 Visual Studio 中使用"单个用户帐户"选项的项目模板中。

中使用的.NET 核心 CLI 中可用的身份验证模板`-au Individual`:

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new webapp -au Individual
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new razor -au Individual
```

::: moniker-end

以下文章演示了如何使用 ASP.NET Core 使用单个用户帐户的模板中生成的代码：

* [使用 SMS 设置双因素身份验证](xref:security/authentication/2fa)
* [ASP.NET Core 中的帐户确认和密码恢复](xref:security/authentication/accconfirm)
* [使用受授权的用户数据创建 ASP.NET Core 应用](xref:security/authorization/secure-data)
