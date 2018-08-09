---
title: ASP.NET Core SignalR.NET 客户端
author: tdykstra
description: 有关 ASP.NET Core SignalR.NET 客户端信息
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/07/2018
uid: signalr/dotnet-client
ms.openlocfilehash: 970888a410b2486a20f98ce77a8674f8ec357f50
ms.sourcegitcommit: 028ad28c546de706ace98066c76774de33e4ad20
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/08/2018
ms.locfileid: "39655247"
---
# <a name="aspnet-core-signalr-net-client"></a>ASP.NET Core SignalR.NET 客户端

作者：[Rachel Appel](http://twitter.com/rachelappel)

ASP.NET Core SignalR.NET 客户端可以使用 Xamarin、 WPF、 Windows 窗体，控制台中和.NET Core 应用。 像[JavaScript 客户端](xref:signalr/javascript-client)，.NET 客户端，可以接收和发送并实时接收到的集线器的消息。

> [!NOTE]
> Xamarin 有特殊要求 Visual Studio 版本。 有关详细信息，请参阅[在 Xamarin 中的 SignalR 客户端 2.1.1](https://github.com/aspnet/Announcements/issues/305)。

[查看或下载示例代码](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample)（[如何下载](xref:tutorials/index#how-to-download-a-sample)）

这篇文章中的代码示例是使用 ASP.NET Core SignalR.NET 客户端的 WPF 应用。

## <a name="install-the-signalr-net-client-package"></a>安装 SignalR.NET 客户端包

`Microsoft.AspNetCore.SignalR.Client`包所需的.NET 客户端连接到 SignalR 集线器。 若要安装客户端库，请运行以下命令**程序包管理器控制台**窗口：

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a>连接到中心

若要建立连接，创建`HubConnectionBuilder`，并调用`Build`。 生成一个连接时，可以配置中心 URL、 协议、 传输类型、 日志级别、 标头，和其他选项。 配置任何必需的选项的方法是插入的任何`HubConnectionBuilder`方法转换`Build`。 启动与连接`StartAsync`。

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?highlight=15-17,33)]

## <a name="call-hub-methods-from-client"></a>从客户端调用集线器方法

`InvokeAsync` 在该中心将调用方法。 将中心方法名称和到中心方法中定义的任何参数传递`InvokeAsync`。 SignalR 是异步的因此请使用`async`和`await`时进行调用。

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=48-49)]

## <a name="call-client-methods-from-hub"></a>从集线器调用客户端方法

定义使用集线器调用的方法`connection.On`之后生成，但之前启动连接。

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=22-29)]

在前面的代码`connection.On`运行时服务器端代码将调用它使用`SendAsync`方法。

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?range=8-11)]

## <a name="error-handling-and-logging"></a>错误处理和日志记录

处理 try catch 语句的错误。 检查`Exception`对象，以确定要执行后发生错误的适当操作。

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=46-54)]

## <a name="additional-resources"></a>其他资源

* [中心](xref:signalr/hubs)
* [JavaScript 客户端](xref:signalr/javascript-client)
* [发布到 Azure](xref:signalr/publish-to-azure-web-app)
