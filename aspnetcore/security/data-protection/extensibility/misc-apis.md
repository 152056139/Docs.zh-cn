---
title: "各种 API"
author: rick-anderson
description: "本文档概述了 ASP.NET 核心数据保护 ISecret 接口。"
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 26474de3a1c681a8c7f8b7812e9ef859f5d448bb
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="miscellaneous-apis"></a><span data-ttu-id="c458d-103">各种 API</span><span class="sxs-lookup"><span data-stu-id="c458d-103">Miscellaneous APIs</span></span>

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> <span data-ttu-id="c458d-104">实现的任意以下接口的类型应该是线程安全的多个调用方。</span><span class="sxs-lookup"><span data-stu-id="c458d-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="isecret"></a><span data-ttu-id="c458d-105">ISecret</span><span class="sxs-lookup"><span data-stu-id="c458d-105">ISecret</span></span>

<span data-ttu-id="c458d-106">`ISecret`接口表示一个机密的值，如加密的密钥材料。</span><span class="sxs-lookup"><span data-stu-id="c458d-106">The `ISecret` interface represents a secret value, such as cryptographic key material.</span></span> <span data-ttu-id="c458d-107">它包含以下 API 图面：</span><span class="sxs-lookup"><span data-stu-id="c458d-107">It contains the following API surface:</span></span>

* <span data-ttu-id="c458d-108">`Length`: `int`</span><span class="sxs-lookup"><span data-stu-id="c458d-108">`Length`: `int`</span></span>

* <span data-ttu-id="c458d-109">`Dispose()`: `void`</span><span class="sxs-lookup"><span data-stu-id="c458d-109">`Dispose()`: `void`</span></span>

* <span data-ttu-id="c458d-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span><span class="sxs-lookup"><span data-stu-id="c458d-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span></span>

<span data-ttu-id="c458d-111">`WriteSecretIntoBuffer`方法填充原始机密值与提供的缓冲区。</span><span class="sxs-lookup"><span data-stu-id="c458d-111">The `WriteSecretIntoBuffer` method populates the supplied buffer with the raw secret value.</span></span> <span data-ttu-id="c458d-112">此 API 将作为参数的缓冲区的原因而不返回`byte[]`直接是，这使调用方机会固定限制对托管的垃圾回收器的机密暴露该缓冲区对象。</span><span class="sxs-lookup"><span data-stu-id="c458d-112">The reason this API takes the buffer as a parameter rather than returning a `byte[]` directly is that this gives the caller the opportunity to pin the buffer object, limiting secret exposure to the managed garbage collector.</span></span>

<span data-ttu-id="c458d-113">`Secret`类型是的具体实现`ISecret`机密的值在进程中内存中的存储位置。</span><span class="sxs-lookup"><span data-stu-id="c458d-113">The `Secret` type is a concrete implementation of `ISecret` where the secret value is stored in in-process memory.</span></span> <span data-ttu-id="c458d-114">在 Windows 平台上的机密的值加密通过[CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx)。</span><span class="sxs-lookup"><span data-stu-id="c458d-114">On Windows platforms, the secret value is encrypted via [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span></span>
