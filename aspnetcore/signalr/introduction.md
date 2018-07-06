---
title: ASP.NET Core SignalR 简介
author: rachelappel
description: 了解如何 ASP.NET Core SignalR 库简化了将实时功能添加到应用。
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 04/25/2018
uid: signalr/introduction
ms.openlocfilehash: 0c833acea139d22883a69a02c2357a71f3ac8db8
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277899"
---
# <a name="introduction-to-aspnet-core-signalr"></a><span data-ttu-id="aa10b-103">ASP.NET Core SignalR 简介</span><span class="sxs-lookup"><span data-stu-id="aa10b-103">Introduction to ASP.NET Core SignalR</span></span>

<span data-ttu-id="aa10b-104">作者：[Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="aa10b-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

## <a name="what-is-signalr"></a><span data-ttu-id="aa10b-105">什么是 SignalR？</span><span class="sxs-lookup"><span data-stu-id="aa10b-105">What is SignalR?</span></span>

<span data-ttu-id="aa10b-106">ASP.NET Core SignalR 是一个库，简化了添加到应用的实时 web 功能。</span><span class="sxs-lookup"><span data-stu-id="aa10b-106">ASP.NET Core SignalR is a library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="aa10b-107">实时 web 功能可立即使服务器端代码能够将内容推送到客户端。</span><span class="sxs-lookup"><span data-stu-id="aa10b-107">Real-time web functionality enables server-side code to push content to clients instantly.</span></span>

<span data-ttu-id="aa10b-108">适用于 SignalR 的良好候选：</span><span class="sxs-lookup"><span data-stu-id="aa10b-108">Good candidates for SignalR:</span></span>

* <span data-ttu-id="aa10b-109">需要从服务器的高频率更新的应用程序。</span><span class="sxs-lookup"><span data-stu-id="aa10b-109">Apps that require high frequency updates from the server.</span></span> <span data-ttu-id="aa10b-110">示例包括游戏、 社交网络、 投票、 拍卖、 映射和 GPS 应用。</span><span class="sxs-lookup"><span data-stu-id="aa10b-110">Examples are gaming, social networks, voting, auction, maps, and GPS apps.</span></span>
* <span data-ttu-id="aa10b-111">仪表板和监视应用程序。</span><span class="sxs-lookup"><span data-stu-id="aa10b-111">Dashboards and monitoring apps.</span></span> <span data-ttu-id="aa10b-112">示例包括公司的仪表板，即时销售更新，或经常出差警报。</span><span class="sxs-lookup"><span data-stu-id="aa10b-112">Examples include company dashboards, instant sales updates, or travel alerts.</span></span>
* <span data-ttu-id="aa10b-113">协作应用程序。</span><span class="sxs-lookup"><span data-stu-id="aa10b-113">Collaborative apps.</span></span> <span data-ttu-id="aa10b-114">白板应用和团队会议软件是协作应用程序的示例。</span><span class="sxs-lookup"><span data-stu-id="aa10b-114">Whiteboard apps and team meeting software are examples of collaborative apps.</span></span>
* <span data-ttu-id="aa10b-115">需要通知的应用程序。</span><span class="sxs-lookup"><span data-stu-id="aa10b-115">Apps that require notifications.</span></span> <span data-ttu-id="aa10b-116">社交网络、 电子邮件、 聊天、 游戏、 旅行警报和很多其他应用程序使用通知。</span><span class="sxs-lookup"><span data-stu-id="aa10b-116">Social networks, email, chat, games, travel alerts, and many other apps use notifications.</span></span>

<span data-ttu-id="aa10b-117">SignalR 提供了一个 API，用于创建服务器到客户端[远程过程调用 (RPC)](https://wikipedia.org/wiki/Remote_procedure_call)。</span><span class="sxs-lookup"><span data-stu-id="aa10b-117">SignalR provides an API for creating server-to-client [remote procedure calls (RPC)](https://wikipedia.org/wiki/Remote_procedure_call).</span></span> <span data-ttu-id="aa10b-118">Rpc 在客户端上从服务器端.NET Core 代码中调用 JavaScript 函数。</span><span class="sxs-lookup"><span data-stu-id="aa10b-118">The RPCs call JavaScript functions on clients from server-side .NET Core code.</span></span>

<span data-ttu-id="aa10b-119">有关 ASP.NET Core SignalR:</span><span class="sxs-lookup"><span data-stu-id="aa10b-119">SignalR for ASP.NET Core:</span></span>

* <span data-ttu-id="aa10b-120">自动处理的连接管理。</span><span class="sxs-lookup"><span data-stu-id="aa10b-120">Handles connection management automatically.</span></span>
* <span data-ttu-id="aa10b-121">允许同时广播到所有连接的客户端的消息。</span><span class="sxs-lookup"><span data-stu-id="aa10b-121">Enables broadcasting messages to all connected clients simultaneously.</span></span> <span data-ttu-id="aa10b-122">例如，聊天室。</span><span class="sxs-lookup"><span data-stu-id="aa10b-122">For example, a chat room.</span></span>
* <span data-ttu-id="aa10b-123">允许将消息发送到特定客户端或客户端组。</span><span class="sxs-lookup"><span data-stu-id="aa10b-123">Enables sending messages to specific clients or groups of clients.</span></span>
* <span data-ttu-id="aa10b-124">为在开放源代码[GitHub](https://github.com/aspnet/signalr)。</span><span class="sxs-lookup"><span data-stu-id="aa10b-124">Is open-sourced at [GitHub](https://github.com/aspnet/signalr).</span></span>
* <span data-ttu-id="aa10b-125">可缩放。</span><span class="sxs-lookup"><span data-stu-id="aa10b-125">Scalable.</span></span>

<span data-ttu-id="aa10b-126">客户端和服务器之间的连接是持久性的与 HTTP 连接不同。</span><span class="sxs-lookup"><span data-stu-id="aa10b-126">The connection between the client and server is persistent, unlike an HTTP connection.</span></span>

## <a name="transports"></a><span data-ttu-id="aa10b-127">传输</span><span class="sxs-lookup"><span data-stu-id="aa10b-127">Transports</span></span>

<span data-ttu-id="aa10b-128">SignalR 通过多种方法用于构建实时 web 应用程序的摘要。</span><span class="sxs-lookup"><span data-stu-id="aa10b-128">SignalR abstracts over a number of techniques for building real-time web applications.</span></span> <span data-ttu-id="aa10b-129">[Websocket](https://tools.ietf.org/html/rfc7118)是最佳的传输，但那些不可用时，可以使用其他技术 Server-Sent 事件和长轮询。</span><span class="sxs-lookup"><span data-stu-id="aa10b-129">[WebSockets](https://tools.ietf.org/html/rfc7118) is the optimal transport, but other techniques like Server-Sent Events and Long Polling can be used when those aren't available.</span></span> <span data-ttu-id="aa10b-130">SignalR 将自动检测并初始化了合适的传输，基于在服务器和客户端上受支持的功能。</span><span class="sxs-lookup"><span data-stu-id="aa10b-130">SignalR will automatically detect and initialize the appropriate transport based on features supported on the server and client.</span></span>

## <a name="hubs"></a><span data-ttu-id="aa10b-131">中心</span><span class="sxs-lookup"><span data-stu-id="aa10b-131">Hubs</span></span>

<span data-ttu-id="aa10b-132">SignalR 使用中心客户端和服务器之间进行通信。</span><span class="sxs-lookup"><span data-stu-id="aa10b-132">SignalR uses hubs to communicate between clients and servers.</span></span>

<span data-ttu-id="aa10b-133">中心是一种高级的管道，允许你的客户端和服务器相互调用方法。</span><span class="sxs-lookup"><span data-stu-id="aa10b-133">A hub is a high-level pipeline that allows your client and server to call methods on each other.</span></span> <span data-ttu-id="aa10b-134">SignalR 处理调度跨计算机界限自动，允许客户端在服务器上调用方法，作为轻松地为本地方法，反之亦然。</span><span class="sxs-lookup"><span data-stu-id="aa10b-134">SignalR handles the dispatching across machine boundaries automatically, allowing clients to call methods on the server as easily as local methods, and vice versa.</span></span> <span data-ttu-id="aa10b-135">中心允许将强类型参数传递给方法，从而使模型绑定。</span><span class="sxs-lookup"><span data-stu-id="aa10b-135">Hubs allow passing strongly-typed parameters to methods, which enables model binding.</span></span> <span data-ttu-id="aa10b-136">SignalR 提供两个内置的中心协议： 文本协议基于 JSON 和基于二进制协议[MessagePack](https://msgpack.org/)。</span><span class="sxs-lookup"><span data-stu-id="aa10b-136">SignalR provides two built-in hub protocols: a text protocol based on JSON and a binary protocol based on [MessagePack](https://msgpack.org/).</span></span>  <span data-ttu-id="aa10b-137">MessagePack 通常会产生较小比时使用 JSON 消息。</span><span class="sxs-lookup"><span data-stu-id="aa10b-137">MessagePack generally creates smaller messages than when using JSON.</span></span> <span data-ttu-id="aa10b-138">较旧的浏览器必须支持[XHR 级别 2](https://caniuse.com/#feat=xhr2)以提供 MessagePack 协议支持。</span><span class="sxs-lookup"><span data-stu-id="aa10b-138">Older browsers must support [XHR level 2](https://caniuse.com/#feat=xhr2) to provide MessagePack protocol support.</span></span>

<span data-ttu-id="aa10b-139">中心调用通过使用活动传输发送消息的客户端代码。</span><span class="sxs-lookup"><span data-stu-id="aa10b-139">Hubs call client-side code by sending messages using the active transport.</span></span> <span data-ttu-id="aa10b-140">消息包含的名称和客户端方法的参数。</span><span class="sxs-lookup"><span data-stu-id="aa10b-140">The messages contain the name and parameters of the client-side method.</span></span> <span data-ttu-id="aa10b-141">为方法参数发送的对象是使用已配置的协议反序列化。</span><span class="sxs-lookup"><span data-stu-id="aa10b-141">Objects sent as method parameters are deserialized using the configured protocol.</span></span> <span data-ttu-id="aa10b-142">客户端尝试匹配到客户端代码中的方法的名称。</span><span class="sxs-lookup"><span data-stu-id="aa10b-142">The client tries to match the name to a method in the client-side code.</span></span> <span data-ttu-id="aa10b-143">如果找到匹配项，使用反序列化的参数数据运行客户端方法。</span><span class="sxs-lookup"><span data-stu-id="aa10b-143">When a match happens, the client method runs using the deserialized parameter data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="aa10b-144">其他资源</span><span class="sxs-lookup"><span data-stu-id="aa10b-144">Additional resources</span></span>

* [<span data-ttu-id="aa10b-145">开始使用 SignalR 为 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="aa10b-145">Get started with SignalR for ASP.NET Core</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="aa10b-146">支持的平台</span><span class="sxs-lookup"><span data-stu-id="aa10b-146">Supported Platforms</span></span>](xref:signalr/supported-platforms)
* [<span data-ttu-id="aa10b-147">中心</span><span class="sxs-lookup"><span data-stu-id="aa10b-147">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="aa10b-148">JavaScript 客户端</span><span class="sxs-lookup"><span data-stu-id="aa10b-148">JavaScript client</span></span>](xref:signalr/javascript-client)
