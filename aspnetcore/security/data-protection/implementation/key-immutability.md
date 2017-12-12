---
title: "密钥不可变性和更改设置"
author: rick-anderson
description: "本文档概述了 ASP.NET 核心数据保护密钥可变性 Api 的实现详细信息。"
keywords: "ASP.NET 核心，数据保护、 密钥不可变性"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: fc911ae3-feca-4ffe-8b43-362bc632fe04
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: 96860b44b64f241a1bbff2ac8366e0863b1ac10c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
# <a name="key-immutability-and-changing-settings"></a><span data-ttu-id="713e6-104">密钥不可变性和更改设置</span><span class="sxs-lookup"><span data-stu-id="713e6-104">Key Immutability and changing settings</span></span>

<span data-ttu-id="713e6-105">一旦对象持久保留到后备存储，其表示形式是永久固定的。</span><span class="sxs-lookup"><span data-stu-id="713e6-105">Once an object is persisted to the backing store, its representation is forever fixed.</span></span> <span data-ttu-id="713e6-106">新的数据可以添加到后备存储，但可以永远不会更改现有数据。</span><span class="sxs-lookup"><span data-stu-id="713e6-106">New data can be added to the backing store, but existing data can never be mutated.</span></span> <span data-ttu-id="713e6-107">此行为的主要目的是为了防止数据损坏。</span><span class="sxs-lookup"><span data-stu-id="713e6-107">The primary purpose of this behavior is to prevent data corruption.</span></span>

<span data-ttu-id="713e6-108">此行为的一个后果是，一旦给后备存储编写了一个键，不可变。</span><span class="sxs-lookup"><span data-stu-id="713e6-108">One consequence of this behavior is that once a key is written to the backing store, it is immutable.</span></span> <span data-ttu-id="713e6-109">其创建、 激活和到期日期可以永远不会更改，但它可以通过使用吊销`IKeyManager`。</span><span class="sxs-lookup"><span data-stu-id="713e6-109">Its creation, activation, and expiration dates can never be changed, though it can revoked by using `IKeyManager`.</span></span> <span data-ttu-id="713e6-110">此外，其基础的算法信息、 主密钥材料和加密 rest 属性也是不可变。</span><span class="sxs-lookup"><span data-stu-id="713e6-110">Additionally, its underlying algorithmic information, master keying material, and encryption at rest properties are also immutable.</span></span>

<span data-ttu-id="713e6-111">如果开发人员更改会影响密钥保持任何设置，这些更改将不会影响到只有在下次时都会生成一个密钥，是指在通过显式调用`IKeyManager.CreateNewKey`或通过数据保护系统的自己[自动密钥生成](key-management.md#data-protection-implementation-key-management)行为。</span><span class="sxs-lookup"><span data-stu-id="713e6-111">If the developer changes any setting that affects key persistence, those changes will not go into effect until the next time a key is generated, either via an explicit call to `IKeyManager.CreateNewKey` or via the data protection system's own [automatic key generation](key-management.md#data-protection-implementation-key-management) behavior.</span></span> <span data-ttu-id="713e6-112">影响密钥持久性的设置如下所示：</span><span class="sxs-lookup"><span data-stu-id="713e6-112">The settings that affect key persistence are as follows:</span></span>

* [<span data-ttu-id="713e6-113">默认密钥生存时间</span><span class="sxs-lookup"><span data-stu-id="713e6-113">The default key lifetime</span></span>](key-management.md#data-protection-implementation-key-management)

* [<span data-ttu-id="713e6-114">在 rest 机制密钥加密</span><span class="sxs-lookup"><span data-stu-id="713e6-114">The key encryption at rest mechanism</span></span>](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest)

* [<span data-ttu-id="713e6-115">在项包含的算法信息</span><span class="sxs-lookup"><span data-stu-id="713e6-115">The algorithmic information contained within the key</span></span>](xref:security/data-protection/configuration/overview#changing-algorithms-with-usecryptographicalgorithms)

<span data-ttu-id="713e6-116">如果你需要这些设置，以便早于下一步的自动密钥滚动时间启动，请考虑使显式调用`IKeyManager.CreateNewKey`强制创建新的密钥。</span><span class="sxs-lookup"><span data-stu-id="713e6-116">If you need these settings to kick in earlier than the next automatic key rolling time, consider making an explicit call to `IKeyManager.CreateNewKey` to force the creation of a new key.</span></span> <span data-ttu-id="713e6-117">请记得提供显式激活日期 （{现在 + 2 天} 是一个很好的经验法则以允许更改传播的时间） 和调用中的到期日期。</span><span class="sxs-lookup"><span data-stu-id="713e6-117">Remember to provide an explicit activation date ({ now + 2 days } is a good rule of thumb to allow time for the change to propagate) and expiration date in the call.</span></span>

>[!TIP]
> <span data-ttu-id="713e6-118">点击存储库中的所有应用程序应指定要用的相同设置`IDataProtectionBuilder`扩展方法。</span><span class="sxs-lookup"><span data-stu-id="713e6-118">All applications touching the repository should specify the same settings with the `IDataProtectionBuilder` extension methods.</span></span> <span data-ttu-id="713e6-119">否则，保存的密钥的属性将取决于调用的密钥生成例程的特定应用程序。</span><span class="sxs-lookup"><span data-stu-id="713e6-119">Otherwise, the properties of the persisted key will be dependent on the particular application that invoked the key generation routines.</span></span>
