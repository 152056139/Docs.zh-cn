---
title: "上下文标头"
author: rick-anderson
description: "本文档概述了 ASP.NET 核心数据保护上下文标头的实现详细信息。"
keywords: "ASP.NET 核心，数据保护、 上下文标头"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: d026a58c-67f4-411e-a410-c35f29c2c517
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/context-headers
ms.openlocfilehash: eb8e4c9ad67d3046648aea1b45f4a675b41b3ec0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
# <a name="context-headers"></a><span data-ttu-id="1551a-104">上下文标头</span><span class="sxs-lookup"><span data-stu-id="1551a-104">Context headers</span></span>

<a name="data-protection-implementation-context-headers"></a>

## <a name="background-and-theory"></a><span data-ttu-id="1551a-105">背景和理论上</span><span class="sxs-lookup"><span data-stu-id="1551a-105">Background and theory</span></span>

<span data-ttu-id="1551a-106">在数据保护系统中，"密钥"意味着可以提供的对象进行身份验证加密服务。</span><span class="sxs-lookup"><span data-stu-id="1551a-106">In the data protection system, a "key" means an object that can provide authenticated encryption services.</span></span> <span data-ttu-id="1551a-107">每个键由唯一的 id (GUID) 和它所携带算法信息和 entropic 材料。</span><span class="sxs-lookup"><span data-stu-id="1551a-107">Each key is identified by a unique id (a GUID), and it carries with it algorithmic information and entropic material.</span></span> <span data-ttu-id="1551a-108">其用途是，每个密钥执行唯一平均信息量，但系统不能强制实施的而我们还需要考虑的开发人员可能会通过修改密钥链中的现有密钥的算法信息手动更改密钥链。</span><span class="sxs-lookup"><span data-stu-id="1551a-108">It is intended that each key carry unique entropy, but the system cannot enforce that, and we also need to account for developers who might change the key ring manually by modifying the algorithmic information of an existing key in the key ring.</span></span> <span data-ttu-id="1551a-109">若要实现提供这种情况下我们安全要求数据保护系统具有的概念[的加密灵活性](https://www.microsoft.com/en-us/research/publication/cryptographic-agility-and-its-relation-to-circular-encryption/)，这样，安全地跨多个加密算法使用单个 entropic 值。</span><span class="sxs-lookup"><span data-stu-id="1551a-109">To achieve our security requirements given these cases the data protection system has a concept of [cryptographic agility](https://www.microsoft.com/en-us/research/publication/cryptographic-agility-and-its-relation-to-circular-encryption/), which allows securely using a single entropic value across multiple cryptographic algorithms.</span></span>

<span data-ttu-id="1551a-110">大多数系统支持的加密灵活性做到这一点包括有关负载之内算法某些标识信息。</span><span class="sxs-lookup"><span data-stu-id="1551a-110">Most systems which support cryptographic agility do so by including some identifying information about the algorithm inside the payload.</span></span> <span data-ttu-id="1551a-111">算法的 OID 通常是适合于此。</span><span class="sxs-lookup"><span data-stu-id="1551a-111">The algorithm's OID is generally a good candidate for this.</span></span> <span data-ttu-id="1551a-112">但是，我们遇到的一个问题是有多种方法来指定相同的算法:"AES"(CNG) 托管 Aes、 AesManaged、 AesCryptoServiceProvider、 AesCng 和 （如果有特定的参数） 的 RijndaelManaged 类都确实是相同首先，，我们将需要维护所有这些到正确的 OID 的映射。</span><span class="sxs-lookup"><span data-stu-id="1551a-112">However, one problem that we ran into is that there are multiple ways to specify the same algorithm: "AES" (CNG) and the managed Aes, AesManaged, AesCryptoServiceProvider, AesCng, and RijndaelManaged (given specific parameters) classes are all actually the same thing, and we'd need to maintain a mapping of all of these to the correct OID.</span></span> <span data-ttu-id="1551a-113">如果开发人员想要提供自定义算法 （或甚至另一个实现的 AES ！），他们将需要告诉我们其 OID。</span><span class="sxs-lookup"><span data-stu-id="1551a-113">If a developer wanted to provide a custom algorithm (or even another implementation of AES!), they'd have to tell us its OID.</span></span> <span data-ttu-id="1551a-114">此额外的注册步骤使系统配置特别棘手。</span><span class="sxs-lookup"><span data-stu-id="1551a-114">This extra registration step makes system configuration particularly painful.</span></span>

<span data-ttu-id="1551a-115">回顾一下，我们决定我们已从错误方向接近问题。</span><span class="sxs-lookup"><span data-stu-id="1551a-115">Stepping back, we decided that we were approaching the problem from the wrong direction.</span></span> <span data-ttu-id="1551a-116">OID 提示您的算法是什么，但我们不实际关心这。</span><span class="sxs-lookup"><span data-stu-id="1551a-116">An OID tells you what the algorithm is, but we don't actually care about this.</span></span> <span data-ttu-id="1551a-117">如果我们需要在两个不同的算法中安全地使用单个 entropic 值，它不是我们要知道算法的实际的所必要的。</span><span class="sxs-lookup"><span data-stu-id="1551a-117">If we need to use a single entropic value securely in two different algorithms, it's not necessary for us to know what the algorithms actually are.</span></span> <span data-ttu-id="1551a-118">我们实际上关心是它们的行为方式。</span><span class="sxs-lookup"><span data-stu-id="1551a-118">What we actually care about is how they behave.</span></span> <span data-ttu-id="1551a-119">任何有效的对称块加密算法也是强伪随机排列 (PRP): 修复 （键，链接模式，IV，纯文本） 的输入和已加密文本输出将与急剧的概率为不同于任何其他对称的块密码提供的相同的输入的算法。</span><span class="sxs-lookup"><span data-stu-id="1551a-119">Any decent symmetric block cipher algorithm is also a strong pseudorandom permutation (PRP): fix the inputs (key, chaining mode, IV, plaintext) and the ciphertext output will with overwhelming probability be distinct from any other symmetric block cipher algorithm given the same inputs.</span></span> <span data-ttu-id="1551a-120">同样，任何相当的加密哈希函数也是强伪函数 (PRF)，并且给定的一组固定的输入其输出将极其能不同于任何其他加密哈希函数。</span><span class="sxs-lookup"><span data-stu-id="1551a-120">Similarly, any decent keyed hash function is also a strong pseudorandom function (PRF), and given a fixed input set its output will overwhelmingly be distinct from any other keyed hash function.</span></span>

<span data-ttu-id="1551a-121">我们使用强 PRPs 和 PRFs 的概念构建上下文标头。</span><span class="sxs-lookup"><span data-stu-id="1551a-121">We use this concept of strong PRPs and PRFs to build up a context header.</span></span> <span data-ttu-id="1551a-122">此上下文标头实质上是充当稳定指纹通过对于任何给定的操作，所用的算法，并提供所需的数据保护系统的加密灵活性。</span><span class="sxs-lookup"><span data-stu-id="1551a-122">This context header essentially acts as a stable thumbprint over the algorithms in use for any given operation, and it provides the cryptographic agility needed by the data protection system.</span></span> <span data-ttu-id="1551a-123">此标头是可重现和更高版本用作的一部分[子项派生过程](subkeyderivation.md#data-protection-implementation-subkey-derivation)。</span><span class="sxs-lookup"><span data-stu-id="1551a-123">This header is reproducible and is used later as part of the [subkey derivation process](subkeyderivation.md#data-protection-implementation-subkey-derivation).</span></span> <span data-ttu-id="1551a-124">有两个不同的方法来生成具体的操作模式的基础的算法取决于上下文标头。</span><span class="sxs-lookup"><span data-stu-id="1551a-124">There are two different ways to build the context header depending on the modes of operation of the underlying algorithms.</span></span>

## <a name="cbc-mode-encryption--hmac-authentication"></a><span data-ttu-id="1551a-125">CBC 模式下加密 + HMAC 身份验证</span><span class="sxs-lookup"><span data-stu-id="1551a-125">CBC-mode encryption + HMAC authentication</span></span>

<a name="data-protection-implementation-context-headers-cbc-components"></a>

<span data-ttu-id="1551a-126">上下文标头由以下组件组成：</span><span class="sxs-lookup"><span data-stu-id="1551a-126">The context header consists of the following components:</span></span>

* <span data-ttu-id="1551a-127">[16 位]值 00 00，是一个标记，这意味着"CBC 加密 + HMAC 身份验证"。</span><span class="sxs-lookup"><span data-stu-id="1551a-127">[16 bits] The value 00 00, which is a marker meaning "CBC encryption + HMAC authentication".</span></span>

* <span data-ttu-id="1551a-128">[32 位]对称块加密算法密钥长度 （以字节为单位，big endian）。</span><span class="sxs-lookup"><span data-stu-id="1551a-128">[32 bits] The key length (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span>

* <span data-ttu-id="1551a-129">[32 位]块大小 （以字节为单位，big endian） 的对称块加密算法。</span><span class="sxs-lookup"><span data-stu-id="1551a-129">[32 bits] The block size (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span>

* <span data-ttu-id="1551a-130">[32 位]HMAC 算法的密钥长度 （以字节为单位，big endian）。</span><span class="sxs-lookup"><span data-stu-id="1551a-130">[32 bits] The key length (in bytes, big-endian) of the HMAC algorithm.</span></span> <span data-ttu-id="1551a-131">（当前密钥的大小始终与匹配的摘要大小。）</span><span class="sxs-lookup"><span data-stu-id="1551a-131">(Currently the key size always matches the digest size.)</span></span>

* <span data-ttu-id="1551a-132">[32 位]HMAC 算法 （以字节为单位，big endian） 摘要大小。</span><span class="sxs-lookup"><span data-stu-id="1551a-132">[32 bits] The digest size (in bytes, big-endian) of the HMAC algorithm.</span></span>

* <span data-ttu-id="1551a-133">EncCBC (K_E、 IV，"")，它是提供空字符串输入对称的块密码算法的输出，并且其中 IV 是全为零的向量。</span><span class="sxs-lookup"><span data-stu-id="1551a-133">EncCBC(K_E, IV, ""), which is the output of the symmetric block cipher algorithm given an empty string input and where IV is an all-zero vector.</span></span> <span data-ttu-id="1551a-134">下面描述了 K_E 的构造。</span><span class="sxs-lookup"><span data-stu-id="1551a-134">The construction of K_E is described below.</span></span>

* <span data-ttu-id="1551a-135">MAC (K_H，"")，这是提供空字符串输入 HMAC 算法的输出。</span><span class="sxs-lookup"><span data-stu-id="1551a-135">MAC(K_H, ""), which is the output of the HMAC algorithm given an empty string input.</span></span> <span data-ttu-id="1551a-136">下面描述了 K_H 的构造。</span><span class="sxs-lookup"><span data-stu-id="1551a-136">The construction of K_H is described below.</span></span>

<span data-ttu-id="1551a-137">理想情况下，我们无法 K_E 和 K_H 传递全为零的向量。</span><span class="sxs-lookup"><span data-stu-id="1551a-137">Ideally, we could pass all-zero vectors for K_E and K_H.</span></span> <span data-ttu-id="1551a-138">但是，我们想要避免这种情况，其中基础算法检查存在的共享密钥的安全性执行任何操作 （值得注意的是 DES 和 3DES） 之前，它可以阻止使用如全为零向量的简单或可重复模式。</span><span class="sxs-lookup"><span data-stu-id="1551a-138">However, we want to avoid the situation where the underlying algorithm checks for the existence of weak keys before performing any operations (notably DES and 3DES), which precludes using a simple or repeatable pattern like an all-zero vector.</span></span>

<span data-ttu-id="1551a-139">相反，我们使用 NIST SP800 108 KDF 中计数器模式 (请参阅[NIST SP800-108](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf)，sec。 5.1) 具有零长度键、 标签和上下文和作为基础 PRF HMACSHA512。</span><span class="sxs-lookup"><span data-stu-id="1551a-139">Instead, we use the NIST SP800-108 KDF in Counter Mode (see [NIST SP800-108](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf), Sec. 5.1) with a zero-length key, label, and context and HMACSHA512 as the underlying PRF.</span></span> <span data-ttu-id="1551a-140">我们派生 |K_E |+ |K_H |字节的输出，然后将分解结果为 K_E 和 K_H 本身。</span><span class="sxs-lookup"><span data-stu-id="1551a-140">We derive | K_E | + | K_H | bytes of output, then decompose the result into K_E and K_H themselves.</span></span> <span data-ttu-id="1551a-141">数学上，这表示，如下所示。</span><span class="sxs-lookup"><span data-stu-id="1551a-141">Mathematically, this is represented as follows.</span></span>

<span data-ttu-id="1551a-142">(K_E | |K_H) = SP800_108_CTR (prf = HMACSHA512，键 =""，标签 =""，上下文 ="")</span><span class="sxs-lookup"><span data-stu-id="1551a-142">( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")</span></span>

### <a name="example-aes-192-cbc--hmacsha256"></a><span data-ttu-id="1551a-143">示例： AES 192 CBC + HMACSHA256</span><span class="sxs-lookup"><span data-stu-id="1551a-143">Example: AES-192-CBC + HMACSHA256</span></span>

<span data-ttu-id="1551a-144">作为示例，请考虑对称块加密算法是 AES 192 CBC，其中的验证算法是 HMACSHA256 这种情况。</span><span class="sxs-lookup"><span data-stu-id="1551a-144">As an example, consider the case where the symmetric block cipher algorithm is AES-192-CBC and the validation algorithm is HMACSHA256.</span></span> <span data-ttu-id="1551a-145">系统将生成使用以下步骤的上下文标头。</span><span class="sxs-lookup"><span data-stu-id="1551a-145">The system would generate the context header using the following steps.</span></span>

<span data-ttu-id="1551a-146">首先，让 (K_E | |K_H) = SP800_108_CTR (prf = HMACSHA512，键 =""，标签 =""，上下文 ="")，其中 |K_E |= 192 位和 |K_H |= 每个指定的算法的 256 位。</span><span class="sxs-lookup"><span data-stu-id="1551a-146">First, let ( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = ""), where | K_E | = 192 bits and | K_H | = 256 bits per the specified algorithms.</span></span> <span data-ttu-id="1551a-147">这将导致 K_E = 5BB6...21DD 和 K_H = A04A...在下面的示例 00A9:</span><span class="sxs-lookup"><span data-stu-id="1551a-147">This leads to K_E = 5BB6..21DD and K_H = A04A..00A9 in the example below:</span></span>

```
5B B6 C9 83 13 78 22 1D 8E 10 73 CA CF 65 8E B0
61 62 42 71 CB 83 21 DD A0 4A 05 00 5B AB C0 A2
49 6F A5 61 E3 E2 49 87 AA 63 55 CD 74 0A DA C4
B7 92 3D BF 59 90 00 A9
```

<span data-ttu-id="1551a-148">接下来，计算 Enc_CBC (K_E、 IV，"") 对于给定 IV 的 AES-192-CBC = 0 * 和 K_E 按上面所述。</span><span class="sxs-lookup"><span data-stu-id="1551a-148">Next, compute Enc_CBC (K_E, IV, "") for AES-192-CBC given IV = 0* and K_E as above.</span></span>

<span data-ttu-id="1551a-149">结果: = F474B1872B3B53E4721DE19C0841DB6F</span><span class="sxs-lookup"><span data-stu-id="1551a-149">result := F474B1872B3B53E4721DE19C0841DB6F</span></span>

<span data-ttu-id="1551a-150">接下来，计算 MAC (K_H，"") 为 HMACSHA256 给定 K_H 按上面所述。</span><span class="sxs-lookup"><span data-stu-id="1551a-150">Next, compute MAC(K_H, "") for HMACSHA256 given K_H as above.</span></span>

<span data-ttu-id="1551a-151">结果: = D4791184B996092EE1202F36E8608FA8FBD98ABDFF5402F264B1D7211536220C</span><span class="sxs-lookup"><span data-stu-id="1551a-151">result := D4791184B996092EE1202F36E8608FA8FBD98ABDFF5402F264B1D7211536220C</span></span>

<span data-ttu-id="1551a-152">这将产生下面的完整上下文标头：</span><span class="sxs-lookup"><span data-stu-id="1551a-152">This produces the full context header below:</span></span>

```
00 00 00 00 00 18 00 00 00 10 00 00 00 20 00 00
00 20 F4 74 B1 87 2B 3B 53 E4 72 1D E1 9C 08 41
DB 6F D4 79 11 84 B9 96 09 2E E1 20 2F 36 E8 60
8F A8 FB D9 8A BD FF 54 02 F2 64 B1 D7 21 15 36
22 0C
```

<span data-ttu-id="1551a-153">此上下文标头是经过身份验证的加密算法对 （AES 192 CBC 加密 + HMACSHA256 验证） 的指纹。</span><span class="sxs-lookup"><span data-stu-id="1551a-153">This context header is the thumbprint of the authenticated encryption algorithm pair (AES-192-CBC encryption + HMACSHA256 validation).</span></span> <span data-ttu-id="1551a-154">组件，如所述[上面](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers-cbc-components)是：</span><span class="sxs-lookup"><span data-stu-id="1551a-154">The components, as described [above](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers-cbc-components) are:</span></span>

* <span data-ttu-id="1551a-155">标记 (00 00)</span><span class="sxs-lookup"><span data-stu-id="1551a-155">the marker (00 00)</span></span>

* <span data-ttu-id="1551a-156">块密码密钥长度 (00 00 00 18)</span><span class="sxs-lookup"><span data-stu-id="1551a-156">the block cipher key length (00 00 00 18)</span></span>

* <span data-ttu-id="1551a-157">块密码块大小 (00 00 00 10)</span><span class="sxs-lookup"><span data-stu-id="1551a-157">the block cipher block size (00 00 00 10)</span></span>

* <span data-ttu-id="1551a-158">HMAC 密钥长度 (00 00 00 20)</span><span class="sxs-lookup"><span data-stu-id="1551a-158">the HMAC key length (00 00 00 20)</span></span>

* <span data-ttu-id="1551a-159">HMAC 摘要的大小 (00 00 00 20)</span><span class="sxs-lookup"><span data-stu-id="1551a-159">the HMAC digest size (00 00 00 20)</span></span>

* <span data-ttu-id="1551a-160">块密码 PRP 输出 (F4 74-DB 6F) 和</span><span class="sxs-lookup"><span data-stu-id="1551a-160">the block cipher PRP output (F4 74 - DB 6F) and</span></span>

* <span data-ttu-id="1551a-161">HMAC PRF 输出 (D4 79-结束)。</span><span class="sxs-lookup"><span data-stu-id="1551a-161">the HMAC PRF output (D4 79 - end).</span></span>

> [!NOTE]
> <span data-ttu-id="1551a-162">CBC 模式下加密 + HMAC 身份验证上下文标头生成而不考虑算法实现是否提供通过 Windows CNG 或托管 SymmetricAlgorithm 和 KeyedHashAlgorithm 类型相同的方式。</span><span class="sxs-lookup"><span data-stu-id="1551a-162">The CBC-mode encryption + HMAC authentication context header is built the same way regardless of whether the algorithms implementations are provided by Windows CNG or by managed SymmetricAlgorithm and KeyedHashAlgorithm types.</span></span> <span data-ttu-id="1551a-163">这允许在不同操作系统上运行的应用程序可靠地生成相同的上下文标头，即使之间的 Os 不同算法的实现。</span><span class="sxs-lookup"><span data-stu-id="1551a-163">This allows applications running on different operating systems to reliably produce the same context header even though the implementations of the algorithms differ between OSes.</span></span> <span data-ttu-id="1551a-164">（在实践中，KeyedHashAlgorithm 不必是正确的 HMAC。</span><span class="sxs-lookup"><span data-stu-id="1551a-164">(In practice, the KeyedHashAlgorithm doesn't have to be a proper HMAC.</span></span> <span data-ttu-id="1551a-165">它可以是任何加密哈希算法类型。）</span><span class="sxs-lookup"><span data-stu-id="1551a-165">It can be any keyed hash algorithm type.)</span></span>

### <a name="example-3des-192-cbc--hmacsha1"></a><span data-ttu-id="1551a-166">示例： 3DES 192 CBC + HMACSHA1</span><span class="sxs-lookup"><span data-stu-id="1551a-166">Example: 3DES-192-CBC + HMACSHA1</span></span>

<span data-ttu-id="1551a-167">首先，让 (K_E | |K_H) = SP800_108_CTR (prf = HMACSHA512，键 =""，标签 =""，上下文 ="")，其中 |K_E |= 192 位和 |K_H |= 每个指定的算法的 160 位。</span><span class="sxs-lookup"><span data-stu-id="1551a-167">First, let ( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = ""), where | K_E | = 192 bits and | K_H | = 160 bits per the specified algorithms.</span></span> <span data-ttu-id="1551a-168">这将导致 K_E = A219...E2BB 和 K_H = DC4A...在下面的示例 B464:</span><span class="sxs-lookup"><span data-stu-id="1551a-168">This leads to K_E = A219..E2BB and K_H = DC4A..B464 in the example below:</span></span>

```
A2 19 60 2F 83 A9 13 EA B0 61 3A 39 B8 A6 7E 22
61 D9 F8 6C 10 51 E2 BB DC 4A 00 D7 03 A2 48 3E
D1 F7 5A 34 EB 28 3E D7 D4 67 B4 64
```

<span data-ttu-id="1551a-169">接下来，计算 Enc_CBC (K_E、 IV，"") 对于给定 IV 的 3DES-192-CBC = 0 * 和 K_E 按上面所述。</span><span class="sxs-lookup"><span data-stu-id="1551a-169">Next, compute Enc_CBC (K_E, IV, "") for 3DES-192-CBC given IV = 0* and K_E as above.</span></span>

<span data-ttu-id="1551a-170">结果: = ABB100F81E53E10E</span><span class="sxs-lookup"><span data-stu-id="1551a-170">result := ABB100F81E53E10E</span></span>

<span data-ttu-id="1551a-171">接下来，计算 MAC (K_H，"") 为 HMACSHA1 给定 K_H 按上面所述。</span><span class="sxs-lookup"><span data-stu-id="1551a-171">Next, compute MAC(K_H, "") for HMACSHA1 given K_H as above.</span></span>

<span data-ttu-id="1551a-172">结果: = 76EB189B35CF03461DDF877CD9F4B1B4D63A7555</span><span class="sxs-lookup"><span data-stu-id="1551a-172">result := 76EB189B35CF03461DDF877CD9F4B1B4D63A7555</span></span>

<span data-ttu-id="1551a-173">此示例的完整上下文标头，这是经过身份验证的指纹将产生如下所示的加密算法对 （3DES 192 CBC 加密 + HMACSHA1 验证）：</span><span class="sxs-lookup"><span data-stu-id="1551a-173">This produces the full context header which is a thumbprint of the authenticated encryption algorithm pair (3DES-192-CBC encryption + HMACSHA1 validation), shown below:</span></span>

```
00 00 00 00 00 18 00 00 00 08 00 00 00 14 00 00
00 14 AB B1 00 F8 1E 53 E1 0E 76 EB 18 9B 35 CF
03 46 1D DF 87 7C D9 F4 B1 B4 D6 3A 75 55
```

<span data-ttu-id="1551a-174">组件，如下所示细分：</span><span class="sxs-lookup"><span data-stu-id="1551a-174">The components break down as follows:</span></span>

* <span data-ttu-id="1551a-175">标记 (00 00)</span><span class="sxs-lookup"><span data-stu-id="1551a-175">the marker (00 00)</span></span>

* <span data-ttu-id="1551a-176">块密码密钥长度 (00 00 00 18)</span><span class="sxs-lookup"><span data-stu-id="1551a-176">the block cipher key length (00 00 00 18)</span></span>

* <span data-ttu-id="1551a-177">块密码块大小 (00 00 00 08)</span><span class="sxs-lookup"><span data-stu-id="1551a-177">the block cipher block size (00 00 00 08)</span></span>

* <span data-ttu-id="1551a-178">HMAC 密钥长度 (00 00 00 14)</span><span class="sxs-lookup"><span data-stu-id="1551a-178">the HMAC key length (00 00 00 14)</span></span>

* <span data-ttu-id="1551a-179">HMAC 摘要的大小 (00 00 00 14)</span><span class="sxs-lookup"><span data-stu-id="1551a-179">the HMAC digest size (00 00 00 14)</span></span>

* <span data-ttu-id="1551a-180">块密码 PRP 输出 (AB B1-E1 0E) 和</span><span class="sxs-lookup"><span data-stu-id="1551a-180">the block cipher PRP output (AB B1 - E1 0E) and</span></span>

* <span data-ttu-id="1551a-181">HMAC PRF 输出 (76 EB-结束)。</span><span class="sxs-lookup"><span data-stu-id="1551a-181">the HMAC PRF output (76 EB - end).</span></span>

## <a name="galoiscounter-mode-encryption--authentication"></a><span data-ttu-id="1551a-182">Galois/计数器模式加密 + 身份验证</span><span class="sxs-lookup"><span data-stu-id="1551a-182">Galois/Counter Mode encryption + authentication</span></span>

<span data-ttu-id="1551a-183">上下文标头由以下组件组成：</span><span class="sxs-lookup"><span data-stu-id="1551a-183">The context header consists of the following components:</span></span>

* <span data-ttu-id="1551a-184">[16 位]值 00 01，是一个标记，这意味着"GCM 加密 + 身份验证"。</span><span class="sxs-lookup"><span data-stu-id="1551a-184">[16 bits] The value 00 01, which is a marker meaning "GCM encryption + authentication".</span></span>

* <span data-ttu-id="1551a-185">[32 位]对称块加密算法密钥长度 （以字节为单位，big endian）。</span><span class="sxs-lookup"><span data-stu-id="1551a-185">[32 bits] The key length (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span>

* <span data-ttu-id="1551a-186">[32 位]在经过身份验证的加密操作期间使用的 nonce 大小 （以字节为单位，big endian）。</span><span class="sxs-lookup"><span data-stu-id="1551a-186">[32 bits] The nonce size (in bytes, big-endian) used during authenticated encryption operations.</span></span> <span data-ttu-id="1551a-187">(对于我们的系统，这固定的 nonce 大小 = 96 位。)</span><span class="sxs-lookup"><span data-stu-id="1551a-187">(For our system, this is fixed at nonce size = 96 bits.)</span></span>

* <span data-ttu-id="1551a-188">[32 位]块大小 （以字节为单位，big endian） 的对称块加密算法。</span><span class="sxs-lookup"><span data-stu-id="1551a-188">[32 bits] The block size (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span> <span data-ttu-id="1551a-189">(为 GCM，这固定的块大小 = 128 位。)</span><span class="sxs-lookup"><span data-stu-id="1551a-189">(For GCM, this is fixed at block size = 128 bits.)</span></span>

* <span data-ttu-id="1551a-190">[32 位]身份验证标记大小 （以字节为单位，big endian） 已经过身份验证的加密函数生成。</span><span class="sxs-lookup"><span data-stu-id="1551a-190">[32 bits] The authentication tag size (in bytes, big-endian) produced by the authenticated encryption function.</span></span> <span data-ttu-id="1551a-191">(对于我们的系统，这固定为标记大小 = 128 位。)</span><span class="sxs-lookup"><span data-stu-id="1551a-191">(For our system, this is fixed at tag size = 128 bits.)</span></span>

* <span data-ttu-id="1551a-192">[128 位]Enc_GCM 的标记 (K_E，nonce，"")，它是给定的空字符串输入对称的块密码算法的输出，并且其中 nonce 是 96 位全为零向量。</span><span class="sxs-lookup"><span data-stu-id="1551a-192">[128 bits] The tag of Enc_GCM (K_E, nonce, ""), which is the output of the symmetric block cipher algorithm given an empty string input and where nonce is a 96-bit all-zero vector.</span></span>

<span data-ttu-id="1551a-193">K_E 派生使用相同的机制，如下所示 CBC 加密 + HMAC 身份验证方案。</span><span class="sxs-lookup"><span data-stu-id="1551a-193">K_E is derived using the same mechanism as in the CBC encryption + HMAC authentication scenario.</span></span> <span data-ttu-id="1551a-194">但是，由于在此处 play 中没有任何 K_H，我们实质上具有 |K_H |= 0，且该算法会折叠到下面的表单。</span><span class="sxs-lookup"><span data-stu-id="1551a-194">However, since there is no K_H in play here, we essentially have | K_H | = 0, and the algorithm collapses to the below form.</span></span>

<span data-ttu-id="1551a-195">K_E = SP800_108_CTR (prf = HMACSHA512，键 =""，标签 =""，上下文 ="")</span><span class="sxs-lookup"><span data-stu-id="1551a-195">K_E = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")</span></span>

### <a name="example-aes-256-gcm"></a><span data-ttu-id="1551a-196">示例： AES 256 GCM</span><span class="sxs-lookup"><span data-stu-id="1551a-196">Example: AES-256-GCM</span></span>

<span data-ttu-id="1551a-197">首先，让 K_E = SP800_108_CTR (prf = HMACSHA512，键 =""，标签 =""，上下文 ="")，其中 |K_E |= 256 位。</span><span class="sxs-lookup"><span data-stu-id="1551a-197">First, let K_E = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = ""), where | K_E | = 256 bits.</span></span>

<span data-ttu-id="1551a-198">K_E: = 22BC6F1B171C08C4AE2F27444AF8FC8B3087A90006CAEA91FDCFB47C1B8733B8</span><span class="sxs-lookup"><span data-stu-id="1551a-198">K_E := 22BC6F1B171C08C4AE2F27444AF8FC8B3087A90006CAEA91FDCFB47C1B8733B8</span></span>

<span data-ttu-id="1551a-199">接下来，计算 Enc_GCM 的身份验证标记 (K_E，nonce，"") 适用于给定 nonce 的 AES-256-GCM = 096 和 K_E 按上面所述。</span><span class="sxs-lookup"><span data-stu-id="1551a-199">Next, compute the authentication tag of Enc_GCM (K_E, nonce, "") for AES-256-GCM given nonce = 096 and K_E as above.</span></span>

<span data-ttu-id="1551a-200">结果: = E7DCCE66DF855A323A6BB7BD7A59BE45</span><span class="sxs-lookup"><span data-stu-id="1551a-200">result := E7DCCE66DF855A323A6BB7BD7A59BE45</span></span>

<span data-ttu-id="1551a-201">这将产生下面的完整上下文标头：</span><span class="sxs-lookup"><span data-stu-id="1551a-201">This produces the full context header below:</span></span>

```
00 01 00 00 00 20 00 00 00 0C 00 00 00 10 00 00
00 10 E7 DC CE 66 DF 85 5A 32 3A 6B B7 BD 7A 59
BE 45
```

<span data-ttu-id="1551a-202">组件，如下所示细分：</span><span class="sxs-lookup"><span data-stu-id="1551a-202">The components break down as follows:</span></span>

   * <span data-ttu-id="1551a-203">标记 (00 01)</span><span class="sxs-lookup"><span data-stu-id="1551a-203">the marker (00 01)</span></span>

   * <span data-ttu-id="1551a-204">块密码密钥长度 (00 00 00 20)</span><span class="sxs-lookup"><span data-stu-id="1551a-204">the block cipher key length (00 00 00 20)</span></span>

   * <span data-ttu-id="1551a-205">nonce 的大小 (00 00 00 0 C)</span><span class="sxs-lookup"><span data-stu-id="1551a-205">the nonce size (00 00 00 0C)</span></span>

   * <span data-ttu-id="1551a-206">块密码块大小 (00 00 00 10)</span><span class="sxs-lookup"><span data-stu-id="1551a-206">the block cipher block size (00 00 00 10)</span></span>

   * <span data-ttu-id="1551a-207">身份验证标记大小 (00 00 00 10) 和</span><span class="sxs-lookup"><span data-stu-id="1551a-207">the authentication tag size (00 00 00 10) and</span></span>

   * <span data-ttu-id="1551a-208">从运行块密码的身份验证标记 (E7 DC-结束)。</span><span class="sxs-lookup"><span data-stu-id="1551a-208">the authentication tag from running the block cipher (E7 DC - end).</span></span>
