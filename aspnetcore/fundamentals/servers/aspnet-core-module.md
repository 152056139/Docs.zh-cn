---
title: ASP.NET Core 模块
author: tdykstra
description: 了解 ASP.NET Core 模块如何使 Kestrel Web 服务器将 IIS 或 IIS Express 用作反向代理服务器。
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/23/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/aspnet-core-module
ms.openlocfilehash: 4e842544f861c3704ba7798fa693b36435d54731
ms.sourcegitcommit: 01db73f2f7ac22b11ea48a947131d6176b0fe9ad
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/26/2018
---
# <a name="aspnet-core-module"></a>ASP.NET Core 模块

作者：[Tom Dykstra](https://github.com/tdykstra)、[Rick Strahl](https://github.com/RickStrahl) 和 [Chris Ross](https://github.com/Tratcher) 

ASP.NET Core 模块允许 ASP.NET Core 应用在采用反向代理配置的 IIS 后运行。 IIS 具有高级 Web 应用安全性和易管理性。

受支持的 Windows 版本：

* Windows 7 或更高版本
* Windows Server 2008 R2 或更高版本

ASP.NET Core 模块仅适用于 Kestrel。 该模块与 [HTTP.sys](xref:fundamentals/servers/httpsys)（以前称为 [WebListener](xref:fundamentals/servers/weblistener)）不兼容。

## <a name="aspnet-core-module-description"></a>ASP.NET Core 模块说明

ASP.NET Core 模块是插入 IIS 管道的本机 IIS 模块，用于将 Web 请求重定向到后端 ASP.NET Core 应用。 许多本机模块（如 Windows 身份验证）仍处于活动状态。 要详细了解随该模块活动的 IIS 模块，请参阅 [IIS 模块](xref:host-and-deploy/iis/modules)。

由于 ASP.NET Core 应用在独立于 IIS 辅助进程的进程中运行，因此该模块还处理进程管理。 该模块在第一个请求到达时启动 ASP.NET Core 应用的进程，并在应用崩溃时重新启动该应用。 这基本上与在 [Windows 进程激活服务 (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) 托管的 IIS 中在进程内运行的 ASP.NET 4.x 应用中出现的行为相同。

下图说明了 IIS、ASP.NET Core 模块和 ASP.NET Core 应用之间的关系：

![ASP.NET Core 模块](aspnet-core-module/_static/ancm.png)

请求从 Web 到达内核模式 HTTP.sys 驱动程序。 驱动程序将请求路由到网站的配置端口上的 IIS，通常为 80 (HTTP) 或 443 (HTTPS)。 该模块将该请求转发到应用的随机端口（非端口 80/443）上的 Kestrel。

该模块在启动时通过环境变量指定端口，IIS 集成中间件将服务器配置为侦听 `http://localhost:{port}`。 执行其他检查，拒绝不是来自该模块的请求。 该模块不支持 HTTPS 转发，因此即使请求由 IIS 通过 HTTPS 接收，它们还是通过 HTTP 转发。

Kestrel 从模块获取请求后，请求会被推送到 ASP.NET Core 中间件管道中。 中间件管道处理该请求并将其作为 `HttpContext` 实例传递给应用的逻辑。 应用的响应传递回 IIS，IIS 将响应推送回发起请求的 HTTP 客户端。

ASP.NET Core 模块具有一些其他功能。 该模块可以：

* 为工作进程设置环境变量。
* 将 stdout 输出记录到文件存储器，以解决启动问题。
* 转发 Windows 身份验证令牌。

## <a name="how-to-install-and-use-the-aspnet-core-module"></a>如何安装和使用 ASP.NET Core 模块

有关如何安装和使用 ASP.NET Core 模块的详细说明，请参阅[使用 IIS 在 Windows 上进行托管](xref:host-and-deploy/iis/index)。 有关配置模块的信息，请参阅 [ASP.NET Core 模块配置参考](xref:host-and-deploy/aspnet-core-module)。

## <a name="additional-resources"></a>其他资源

* [使用 IIS 在 Windows 上进行托管](xref:host-and-deploy/iis/index)
* [ASP.NET Core 模块配置参考](xref:host-and-deploy/aspnet-core-module)
* [ASP.NET Core 模块 GitHub 存储库（源代码）](https://github.com/aspnet/AspNetCoreModule)
