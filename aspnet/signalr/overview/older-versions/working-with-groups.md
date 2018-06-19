---
uid: signalr/overview/older-versions/working-with-groups
title: 使用 SignalR 在组 1.x |Microsoft 文档
author: pfletcher
description: 本主题介绍如何保持与中心 API 的组成员身份信息。
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/21/2013
ms.topic: article
ms.assetid: 22929efd-68c9-4609-b76d-f8ba42fda01e
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/working-with-groups
msc.type: authoredcontent
ms.openlocfilehash: 7bc0ff73ade72729cc5e1217b3fe704ac0d8cab8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
ms.locfileid: "28036539"
---
<a name="working-with-groups-in-signalr-1x"></a><span data-ttu-id="1225c-103">使用 SignalR 在组 1.x</span><span class="sxs-lookup"><span data-stu-id="1225c-103">Working with Groups in SignalR 1.x</span></span>
====================
<span data-ttu-id="1225c-104">通过[Patrick Fletcher](https://github.com/pfletcher)， [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="1225c-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="1225c-105">本主题介绍如何将用户添加到组和保留组成员身份信息。</span><span class="sxs-lookup"><span data-stu-id="1225c-105">This topic describes how to add users to groups and persist group membership information.</span></span>


## <a name="overview"></a><span data-ttu-id="1225c-106">概述</span><span class="sxs-lookup"><span data-stu-id="1225c-106">Overview</span></span>

<span data-ttu-id="1225c-107">SignalR 中的组提供一种方法将消息广播到连接的客户端的指定子集。</span><span class="sxs-lookup"><span data-stu-id="1225c-107">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="1225c-108">组可以具有任意数量的客户端，并且客户端可以是任意数量的组的成员。</span><span class="sxs-lookup"><span data-stu-id="1225c-108">A group can have any number of clients, and a client can be a member of any number of groups.</span></span> <span data-ttu-id="1225c-109">你无需显式创建组。</span><span class="sxs-lookup"><span data-stu-id="1225c-109">You don't have to explicitly create groups.</span></span> <span data-ttu-id="1225c-110">实际上，第一次 Groups.Add，对的调用中指定其名称自动创建一个组并将其删除从中它的成员身份中删除最后一次连接时。</span><span class="sxs-lookup"><span data-stu-id="1225c-110">In effect, a group is automatically created the first time you specify its name in a call to Groups.Add, and it is deleted when you remove the last connection from membership in it.</span></span> <span data-ttu-id="1225c-111">有关使用组的简介，请参阅[如何从中心类管理组成员身份](index.md)中心 API-Server 指南中。</span><span class="sxs-lookup"><span data-stu-id="1225c-111">For an introduction to using groups, see [How to manage group membership from the Hub class](index.md) in the Hubs API - Server Guide.</span></span>

<span data-ttu-id="1225c-112">用于获取组成员身份列表或组的列表中没有任何 API。</span><span class="sxs-lookup"><span data-stu-id="1225c-112">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="1225c-113">SignalR 将消息发送到客户端和基于发布/订阅模型的组和服务器不维护组或组成员身份的列表。</span><span class="sxs-lookup"><span data-stu-id="1225c-113">SignalR sends messages to clients and groups based on a pub/sub model, and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="1225c-114">这有助于最大程度提高可伸缩性，因为每当向 web 场添加节点时，SignalR 维护任何状态都将传播到新节点。</span><span class="sxs-lookup"><span data-stu-id="1225c-114">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<span data-ttu-id="1225c-115">当将用户添加到组使用`Groups.Add`方法，用户会收到消息定向到该组的当前连接的持续时间内，但该组中的用户的成员身份不会保留超出当前的连接。</span><span class="sxs-lookup"><span data-stu-id="1225c-115">When you add a user to a group using the `Groups.Add` method, the user receives messages directed to that group for the duration of the current connection, but the user's membership in that group is not persisted beyond the current connection.</span></span> <span data-ttu-id="1225c-116">如果你想要永久保留关于组和组成员身份的信息，你必须如数据库或 Azure 表存储存储库中存储该数据。</span><span class="sxs-lookup"><span data-stu-id="1225c-116">If you want to permanently retain information about groups and group membership, you must store that data in a repository such as a database or Azure table storage.</span></span> <span data-ttu-id="1225c-117">然后，用户连接到你的应用程序，每次你从存储库中检索用户所属的组并手动将该用户添加到这些组。</span><span class="sxs-lookup"><span data-stu-id="1225c-117">Then, each time a user connects to your application, you retrieve from the repository which groups the user belongs to, and manually add that user to those groups.</span></span>

<span data-ttu-id="1225c-118">当重新连接暂时中断后，用户自动重新将加入的以前分配的组。</span><span class="sxs-lookup"><span data-stu-id="1225c-118">When reconnecting after a temporary disruption, the user automatically re-joins the previously-assigned groups.</span></span> <span data-ttu-id="1225c-119">自动重新加入一组仅适用时重新连接后，不是在建立新连接时。</span><span class="sxs-lookup"><span data-stu-id="1225c-119">Automatically rejoining a group only applies when reconnecting, not when establishing a new connection.</span></span> <span data-ttu-id="1225c-120">从包含以前分配的组列表中的客户端传递进行数字签名的令牌。</span><span class="sxs-lookup"><span data-stu-id="1225c-120">A digitally-signed token is passed from the client that contains the list of previously-assigned groups.</span></span> <span data-ttu-id="1225c-121">如果你想要验证用户是否属于请求的组，则可以重写默认行为。</span><span class="sxs-lookup"><span data-stu-id="1225c-121">If you want to verify whether the user belongs to the requested groups, you can override the default behavior.</span></span>

<span data-ttu-id="1225c-122">本主题包括以下部分：</span><span class="sxs-lookup"><span data-stu-id="1225c-122">This topic includes the following sections:</span></span>

- [<span data-ttu-id="1225c-123">添加和删除用户</span><span class="sxs-lookup"><span data-stu-id="1225c-123">Adding and removing users</span></span>](#add)
- [<span data-ttu-id="1225c-124">调用组的成员</span><span class="sxs-lookup"><span data-stu-id="1225c-124">Calling members of a group</span></span>](#call)
- [<span data-ttu-id="1225c-125">在数据库中存储组成员身份</span><span class="sxs-lookup"><span data-stu-id="1225c-125">Storing group membership in a database</span></span>](#storedatabase)
- [<span data-ttu-id="1225c-126">在 Azure 表存储中存储组成员身份</span><span class="sxs-lookup"><span data-stu-id="1225c-126">Storing group membership in Azure table storage</span></span>](#storeazuretable)
- [<span data-ttu-id="1225c-127">当重新连接时验证组成员身份</span><span class="sxs-lookup"><span data-stu-id="1225c-127">Verifying group membership when reconnecting</span></span>](#verify)

<a id="add"></a>

## <a name="adding-and-removing-users"></a><span data-ttu-id="1225c-128">添加和删除用户</span><span class="sxs-lookup"><span data-stu-id="1225c-128">Adding and removing users</span></span>

<span data-ttu-id="1225c-129">若要添加或从组中删除用户，请调用[添加](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx)或[删除](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx)方法，并在用户的连接 id 和组的名称作为参数传递。</span><span class="sxs-lookup"><span data-stu-id="1225c-129">To add or remove users from a group, you call the [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) or [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods, and pass in the user's connection id and group's name as parameters.</span></span> <span data-ttu-id="1225c-130">不需要手动用户从组中删除连接结束时。</span><span class="sxs-lookup"><span data-stu-id="1225c-130">You do not need to manually remove a user from a group when the connection ends.</span></span>

<span data-ttu-id="1225c-131">下面的示例演示`Groups.Add`和`Groups.Remove`中心方法中使用的方法。</span><span class="sxs-lookup"><span data-stu-id="1225c-131">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample1.cs?highlight=5,10)]

<span data-ttu-id="1225c-132">`Groups.Add`和`Groups.Remove`方法异步执行。</span><span class="sxs-lookup"><span data-stu-id="1225c-132">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span>

<span data-ttu-id="1225c-133">如果你想要将客户端添加到组并立即向客户端发送一条消息，通过使用组，你必须确保 Groups.Add 方法先完成。</span><span class="sxs-lookup"><span data-stu-id="1225c-133">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the Groups.Add method finishes first.</span></span> <span data-ttu-id="1225c-134">下面的代码示例演示如何为此，请通过使用在.NET 4.5 起，和一个，方法是使用在.NET 4 中工作的代码中的代码。</span><span class="sxs-lookup"><span data-stu-id="1225c-134">The following code examples show how to do that, one by using code that works in .NET 4.5, and one by using code that works in .NET 4.</span></span>

#### <a name="asynchronous-net-45-example"></a><span data-ttu-id="1225c-135">异步.NET 4.5 示例</span><span class="sxs-lookup"><span data-stu-id="1225c-135">Asynchronous .NET 4.5 Example</span></span>

[!code-csharp[Main](working-with-groups/samples/sample2.cs?highlight=1,3)]

#### <a name="asynchronous-net-4-example"></a><span data-ttu-id="1225c-136">异步.NET 4 示例</span><span class="sxs-lookup"><span data-stu-id="1225c-136">Asynchronous .NET 4 Example</span></span>

[!code-csharp[Main](working-with-groups/samples/sample3.cs?highlight=3-4)]

<span data-ttu-id="1225c-137">一般情况下，不应包含`await`调用时`Groups.Remove`方法因为想要删除的连接 id 可能不再可用。</span><span class="sxs-lookup"><span data-stu-id="1225c-137">In general, you should not include `await` when calling the `Groups.Remove` method because the connection id that you are trying to remove might no longer be available.</span></span> <span data-ttu-id="1225c-138">在这种情况下，`TaskCanceledException`后在请求超时时引发。如果你的应用程序必须确保用户具有已删除从组在将消息发送到组之前，你可以添加`await`Groups.Remove，然后 catch 之前`TaskCanceledException`可能引发的异常。</span><span class="sxs-lookup"><span data-stu-id="1225c-138">In that case, `TaskCanceledException` is thrown after the request times out. If your application must ensure that the user has been removed from the group before sending a message to the group, you can add `await` before Groups.Remove, and then catch the `TaskCanceledException` exception that might be thrown.</span></span>

<a id="call"></a>

## <a name="calling-members-of-a-group"></a><span data-ttu-id="1225c-139">调用组的成员</span><span class="sxs-lookup"><span data-stu-id="1225c-139">Calling members of a group</span></span>

<span data-ttu-id="1225c-140">下面的示例中所示，你可以将消息发送到的所有组的成员或仅指定的组的成员。</span><span class="sxs-lookup"><span data-stu-id="1225c-140">You can send messages to all of the members of a group or only specified members of the group, as shown in the following examples.</span></span>

- <span data-ttu-id="1225c-141">**所有**连接客户端在指定的组。</span><span class="sxs-lookup"><span data-stu-id="1225c-141">**All** connected clients in a specified group.</span></span> 

    [!code-css[Main](working-with-groups/samples/sample4.css)]
- <span data-ttu-id="1225c-142">所有连接指定的组中的客户端**除外指定客户端**，标识由连接 id。</span><span class="sxs-lookup"><span data-stu-id="1225c-142">All connected clients in a specified group **except the specified clients**, identified by connection ID.</span></span> 

    [!code-csharp[Main](working-with-groups/samples/sample5.cs)]
- <span data-ttu-id="1225c-143">所有连接指定的组中的客户端**除外调用的客户端**。</span><span class="sxs-lookup"><span data-stu-id="1225c-143">All connected clients in a specified group **except the calling client**.</span></span> 

    [!code-css[Main](working-with-groups/samples/sample6.css)]

<a id="storedatabase"></a>

## <a name="storing-group-membership-in-a-database"></a><span data-ttu-id="1225c-144">在数据库中存储组成员身份</span><span class="sxs-lookup"><span data-stu-id="1225c-144">Storing group membership in a database</span></span>

<span data-ttu-id="1225c-145">下面的示例演示如何保留在数据库中的组和用户信息。</span><span class="sxs-lookup"><span data-stu-id="1225c-145">The following examples show how to retain group and user information in a database.</span></span> <span data-ttu-id="1225c-146">你可以使用任何数据访问技术;但是，下面的示例演示如何定义使用 Entity Framework 模型。</span><span class="sxs-lookup"><span data-stu-id="1225c-146">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="1225c-147">这些实体模型对应于数据库表和字段。</span><span class="sxs-lookup"><span data-stu-id="1225c-147">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="1225c-148">您的数据结构无法千差万别，具体取决于你的应用程序的要求。</span><span class="sxs-lookup"><span data-stu-id="1225c-148">Your data structure could vary considerably depending on the requirements of your application.</span></span> <span data-ttu-id="1225c-149">该示例包括一个名为类`ConversationRoom`这将是唯一的该应用程序，用户加入有关不同主题，例如体育或园对话。</span><span class="sxs-lookup"><span data-stu-id="1225c-149">This example includes a class named `ConversationRoom` which would be unique to an application that enables users to join conversations about different subjects, such as sports or gardening.</span></span> <span data-ttu-id="1225c-150">此示例还包括为所连接的类。</span><span class="sxs-lookup"><span data-stu-id="1225c-150">This example also includes a class for the connections.</span></span> <span data-ttu-id="1225c-151">连接类不是绝对必需用于跟踪组成员身份，但经常是可靠的解决方案，跟踪用户的一部分。</span><span class="sxs-lookup"><span data-stu-id="1225c-151">The connection class is not absolutely required for tracking group membership but is frequently part of robust solution to tracking users.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample7.cs)]

<span data-ttu-id="1225c-152">然后，在中心，你可以从数据库中检索组和用户信息并手动将用户添加到相应的组。</span><span class="sxs-lookup"><span data-stu-id="1225c-152">Then, in the hub, you can retrieve the group and user information from the database and manually add the user to the appropriate groups.</span></span> <span data-ttu-id="1225c-153">该示例不包括用于跟踪的用户连接的代码。</span><span class="sxs-lookup"><span data-stu-id="1225c-153">The example does not include code for tracking the user connections.</span></span> <span data-ttu-id="1225c-154">在此示例中，`await`关键字不应用之前`Groups.Add`因为到组的成员未立即发送一条消息。</span><span class="sxs-lookup"><span data-stu-id="1225c-154">In this example, the `await` keyword is not applied before `Groups.Add` because a message is not immediately sent to members of the group.</span></span> <span data-ttu-id="1225c-155">如果你想要添加新成员后，立即将消息发送到组的所有成员，你将想要应用`await`关键字，若要确保异步操作已完成。</span><span class="sxs-lookup"><span data-stu-id="1225c-155">If you want to send a message to all members of the group immediately after adding the new member, you would want to apply the `await` keyword to make sure the asynchronous operation has completed.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample8.cs)]

<a id="storeazuretable"></a>

## <a name="storing-group-membership-in-azure-table-storage"></a><span data-ttu-id="1225c-156">在 Azure 表存储中存储组成员身份</span><span class="sxs-lookup"><span data-stu-id="1225c-156">Storing group membership in Azure table storage</span></span>

<span data-ttu-id="1225c-157">使用 Azure 表存储来存储组和用户信息是类似于使用数据库。</span><span class="sxs-lookup"><span data-stu-id="1225c-157">Using Azure table storage to store group and user information is similar to using a database.</span></span> <span data-ttu-id="1225c-158">下面的示例演示将存储的用户名和组名称的表实体。</span><span class="sxs-lookup"><span data-stu-id="1225c-158">The following example shows a table entity that stores the user name and group name.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample9.cs)]

<span data-ttu-id="1225c-159">在中心，您将检索分配的组，当用户连接。</span><span class="sxs-lookup"><span data-stu-id="1225c-159">In the hub, you retrieve the assigned groups when the user connects.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample10.cs)]

<a id="verify"></a>

## <a name="verifying-group-membership-when-reconnecting"></a><span data-ttu-id="1225c-160">当重新连接时验证组成员身份</span><span class="sxs-lookup"><span data-stu-id="1225c-160">Verifying group membership when reconnecting</span></span>

<span data-ttu-id="1225c-161">默认情况下，SignalR 会自动重新分配用户到相应的组时从临时中断，如连接时删除并重新建立连接超时之前重新连接。用户的组信息时，会传入令牌重新连接，并且该令牌会验证服务器上。</span><span class="sxs-lookup"><span data-stu-id="1225c-161">By default, SignalR automatically re-assigns a user to the appropriate groups when reconnecting from a temporary disruption, such as when a connection is dropped and re-established before the connection times out. The user's group information is passed in a token when reconnecting, and that token is verified on the server.</span></span> <span data-ttu-id="1225c-162">有关重新加入到组的用户的验证过程的信息，请参阅[当重新连接时重新加入组](index.md)。</span><span class="sxs-lookup"><span data-stu-id="1225c-162">For information about the verification process for rejoining users to groups, see [Rejoining groups when reconnecting](index.md).</span></span>

<span data-ttu-id="1225c-163">一般情况下，应使用自动重新加入上的组重新连接的默认行为。</span><span class="sxs-lookup"><span data-stu-id="1225c-163">In general, you should use the default behavior of automatically rejoining groups on reconnect.</span></span> <span data-ttu-id="1225c-164">SignalR 组并不作为限制对敏感数据的访问的安全机制。</span><span class="sxs-lookup"><span data-stu-id="1225c-164">SignalR groups are not intended as a security mechanism for restricting access to sensitive data.</span></span> <span data-ttu-id="1225c-165">但是，如果你的应用程序必须仔细检查用户的组成员身份，当重新连接时，你可以重写默认行为。</span><span class="sxs-lookup"><span data-stu-id="1225c-165">However, if your application must double-check a user's group membership when reconnecting, you can override the default behavior.</span></span> <span data-ttu-id="1225c-166">更改默认行为可以添加了负担到你的数据库因为必须针对每个重新连接而不是仅当用户连接检索到用户的组成员身份。</span><span class="sxs-lookup"><span data-stu-id="1225c-166">Changing the default behavior can add a burden to your database because a user's group membership must be retrieved for each reconnection rather than just when the user connects.</span></span>

<span data-ttu-id="1225c-167">如果你必须在验证组成员身份重新连接，请创建一个新的中心管道模块返回的已分配的组列表，如下所示。</span><span class="sxs-lookup"><span data-stu-id="1225c-167">If you must verify group membership on reconnect, create a new hub pipeline module that returns a list of assigned groups, as shown below.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample11.cs)]

<span data-ttu-id="1225c-168">然后，将该模块添加到中心管道，作为下面突出显示。</span><span class="sxs-lookup"><span data-stu-id="1225c-168">Then, add that module to the hub pipeline, as highlighted below.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample12.cs?highlight=10)]
