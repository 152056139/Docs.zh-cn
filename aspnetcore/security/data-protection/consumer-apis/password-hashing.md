---
title: 在 ASP.NET 核心中的哈希密码
author: rick-anderson
description: 了解如何使用 ASP.NET 核心数据保护 Api 的密码执行哈希。
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/consumer-apis/password-hashing
ms.openlocfilehash: f44e66789bf348ef6d99f6d862fb34c2d943a0b2
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/03/2018
---
# <a name="hash-passwords-in-aspnet-core"></a><span data-ttu-id="b2e7e-103">在 ASP.NET 核心中的哈希密码</span><span class="sxs-lookup"><span data-stu-id="b2e7e-103">Hash passwords in ASP.NET Core</span></span>

<span data-ttu-id="b2e7e-104">基本数据保护代码包括包*Microsoft.AspNetCore.Cryptography.KeyDerivation*其中包含加密密钥派生函数。</span><span class="sxs-lookup"><span data-stu-id="b2e7e-104">The data protection code base includes a package *Microsoft.AspNetCore.Cryptography.KeyDerivation* which contains cryptographic key derivation functions.</span></span> <span data-ttu-id="b2e7e-105">此包是一个独立组件，并不依赖于数据保护系统的其余部分。</span><span class="sxs-lookup"><span data-stu-id="b2e7e-105">This package is a standalone component and has no dependencies on the rest of the data protection system.</span></span> <span data-ttu-id="b2e7e-106">可以完全单独使用它。</span><span class="sxs-lookup"><span data-stu-id="b2e7e-106">It can be used completely independently.</span></span> <span data-ttu-id="b2e7e-107">源共存于基本为方便起见数据保护代码。</span><span class="sxs-lookup"><span data-stu-id="b2e7e-107">The source exists alongside the data protection code base as a convenience.</span></span>

<span data-ttu-id="b2e7e-108">包当前提供一种方法`KeyDerivation.Pbkdf2`这样，哈希密码使用[PBKDF2 算法](https://tools.ietf.org/html/rfc2898#section-5.2)。</span><span class="sxs-lookup"><span data-stu-id="b2e7e-108">The package currently offers a method `KeyDerivation.Pbkdf2` which allows hashing a password using the [PBKDF2 algorithm](https://tools.ietf.org/html/rfc2898#section-5.2).</span></span> <span data-ttu-id="b2e7e-109">此 API 已非常类似于.NET Framework 的现有[Rfc2898DeriveBytes 类型](/dotnet/api/system.security.cryptography.rfc2898derivebytes)，但有三个重要区别：</span><span class="sxs-lookup"><span data-stu-id="b2e7e-109">This API is very similar to the .NET Framework's existing [Rfc2898DeriveBytes type](/dotnet/api/system.security.cryptography.rfc2898derivebytes), but there are three important distinctions:</span></span>

1. <span data-ttu-id="b2e7e-110">`KeyDerivation.Pbkdf2`方法支持使用多个 PRFs (当前`HMACSHA1`， `HMACSHA256`，和`HMACSHA512`)，而`Rfc2898DeriveBytes`类型仅支持`HMACSHA1`。</span><span class="sxs-lookup"><span data-stu-id="b2e7e-110">The `KeyDerivation.Pbkdf2` method supports consuming multiple PRFs (currently `HMACSHA1`, `HMACSHA256`, and `HMACSHA512`), whereas the `Rfc2898DeriveBytes` type only supports `HMACSHA1`.</span></span>

2. <span data-ttu-id="b2e7e-111">`KeyDerivation.Pbkdf2`方法检测到当前操作系统，然后尝试选择最优化的实现的例程，提供更好的性能，在某些情况下，选择。</span><span class="sxs-lookup"><span data-stu-id="b2e7e-111">The `KeyDerivation.Pbkdf2` method detects the current operating system and attempts to choose the most optimized implementation of the routine, providing much better performance in certain cases.</span></span> <span data-ttu-id="b2e7e-112">(在 Windows 8 中，它提供约 10 倍的吞吐量`Rfc2898DeriveBytes`。)</span><span class="sxs-lookup"><span data-stu-id="b2e7e-112">(On Windows 8, it offers around 10x the throughput of `Rfc2898DeriveBytes`.)</span></span>

3. <span data-ttu-id="b2e7e-113">`KeyDerivation.Pbkdf2`方法要求调用方指定的所有参数 (salt，PRF 和迭代计数)。</span><span class="sxs-lookup"><span data-stu-id="b2e7e-113">The `KeyDerivation.Pbkdf2` method requires the caller to specify all parameters (salt, PRF, and iteration count).</span></span> <span data-ttu-id="b2e7e-114">`Rfc2898DeriveBytes`类型提供的这些默认值。</span><span class="sxs-lookup"><span data-stu-id="b2e7e-114">The `Rfc2898DeriveBytes` type provides default values for these.</span></span>

[!code-csharp[](password-hashing/samples/passwordhasher.cs)]

<span data-ttu-id="b2e7e-115">ASP.NET 核心标识，请参阅源代码`PasswordHasher`现实世界的类型用例。</span><span class="sxs-lookup"><span data-stu-id="b2e7e-115">See the source code for ASP.NET Core Identity's `PasswordHasher` type for a real-world use case.</span></span>
