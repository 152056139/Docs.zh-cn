---
title: "启用 ASP.NET Core 中的身份验证器应用的 QR 代码生成"
author: rick-anderson
description: "启用 ASP.NET Core 中的身份验证器应用的 QR 代码生成"
keywords: "ASP.NET 核心、 MVC，QR 代码生成、 身份验证器、 2FA"
ms.author: riande
manager: wpickett
ms.date: 7/24/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-enable-qrcodes
ms.openlocfilehash: 4eb260ecb3d1f1797bed5ba82ef3fc0628f05b6f
ms.sourcegitcommit: bb615e57e80ae496a63a1bf53a3a9cffab2fce9b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/17/2017
---
# <a name="enabling-qr-code-generation-for-authenticator-apps-in-aspnet-core"></a><span data-ttu-id="a6f8a-104">启用 ASP.NET Core 中的身份验证器应用的 QR 代码生成</span><span class="sxs-lookup"><span data-stu-id="a6f8a-104">Enabling QR Code generation for authenticator apps in ASP.NET Core</span></span>

<span data-ttu-id="a6f8a-105">注意： 本主题适用于 ASP.NET Core 2.x 与 Razor 页。</span><span class="sxs-lookup"><span data-stu-id="a6f8a-105">Note: This topic applies to ASP.NET Core 2.x with Razor Pages.</span></span>

<span data-ttu-id="a6f8a-106">ASP.NET 核心附带的单个身份验证的身份验证器应用程序的支持。</span><span class="sxs-lookup"><span data-stu-id="a6f8a-106">ASP.NET Core ships with support for authenticator applications for individual authentication.</span></span> <span data-ttu-id="a6f8a-107">两个因素身份验证 (2FA) 身份验证器应用，使用基于时间的一次性密码算法 (TOTP)，是建议 2FA 的 approch 行业。</span><span class="sxs-lookup"><span data-stu-id="a6f8a-107">Two factor authentication (2FA) authenticator apps, using a Time-based One-time Password Algorithm (TOTP), are the industry recommended approch for 2FA.</span></span> <span data-ttu-id="a6f8a-108">2FA 使用 TOTP 优于 SMS 2FA。</span><span class="sxs-lookup"><span data-stu-id="a6f8a-108">2FA using TOTP is preferred to SMS 2FA.</span></span> <span data-ttu-id="a6f8a-109">验证器应用提供哪些用户确认其用户名和密码后，必须输入一个 6 到 8 位代码。</span><span class="sxs-lookup"><span data-stu-id="a6f8a-109">An authenticator app provides a 6 to 8 digit code which users must enter after confirming their username and password.</span></span> <span data-ttu-id="a6f8a-110">通常在智能手机上安装验证器应用。</span><span class="sxs-lookup"><span data-stu-id="a6f8a-110">Typically an authenticator app is installed on a smart phone.</span></span>

<span data-ttu-id="a6f8a-111">ASP.NET 核心 web 应用程序模板支持身份验证器，但不是提供对 QRCode 生成的支持。</span><span class="sxs-lookup"><span data-stu-id="a6f8a-111">The ASP.NET Core web app templates support authenticators, but do not provide support for QRCode generation.</span></span> <span data-ttu-id="a6f8a-112">QRCode 生成器轻松地 2FA 的安装程序。</span><span class="sxs-lookup"><span data-stu-id="a6f8a-112">QRCode generators ease the setup of 2FA.</span></span> <span data-ttu-id="a6f8a-113">本文档将指导你完成添加[QR 代码](https://wikipedia.org/wiki/QR_code)生成到 2FA 配置页。</span><span class="sxs-lookup"><span data-stu-id="a6f8a-113">This document will guide you through adding [QR Code](https://wikipedia.org/wiki/QR_code) generation to the 2FA configuration page.</span></span>

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a><span data-ttu-id="a6f8a-114">将 QR 代码添加到 2FA 配置页</span><span class="sxs-lookup"><span data-stu-id="a6f8a-114">Adding QR Codes to the 2FA configuration page</span></span>

<span data-ttu-id="a6f8a-115">这些说明使用*qrcode.js*从 https://davidshimjs.github.io/qrcodejs/ 存储库。</span><span class="sxs-lookup"><span data-stu-id="a6f8a-115">These instructions use *qrcode.js* from the https://davidshimjs.github.io/qrcodejs/ repo.</span></span>

* <span data-ttu-id="a6f8a-116">下载[qrcode.js javascript 库](https://davidshimjs.github.io/qrcodejs/)到`wwwroot\lib`项目文件夹中的。</span><span class="sxs-lookup"><span data-stu-id="a6f8a-116">Download the  [qrcode.js javascript library](https://davidshimjs.github.io/qrcodejs/) to the `wwwroot\lib` folder in your project.</span></span>

* <span data-ttu-id="a6f8a-117">在*Pages\Account\Manage\EnableAuthenticator.cshtml*文件中，找到`Scripts`文件末尾的部分：</span><span class="sxs-lookup"><span data-stu-id="a6f8a-117">In the *Pages\Account\Manage\EnableAuthenticator.cshtml* file, locate the `Scripts` section at the end of the file:</span></span>

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* <span data-ttu-id="a6f8a-118">更新`Scripts`部分添加到引用`qrcodejs`你添加的库和生成的 QR 代码的调用。</span><span class="sxs-lookup"><span data-stu-id="a6f8a-118">Update the `Scripts` section to add a reference to the `qrcodejs` library you added and a call to generate the QR Code.</span></span> <span data-ttu-id="a6f8a-119">其外观应，如下所示：</span><span class="sxs-lookup"><span data-stu-id="a6f8a-119">It should look as follows:</span></span>

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")

    <script type="text/javascript" src="/lib/qrcode.js"></script>
    <script type="text/javascript">
        new QRCode(document.getElementById("qrCode"),
            {
                text: "@Html.Raw(Model.AuthenticatorUri)",
                width: 150,
                height: 150
            });
    </script>
}
```

* <span data-ttu-id="a6f8a-120">删除的段落，您链接到这些说明。</span><span class="sxs-lookup"><span data-stu-id="a6f8a-120">Delete the paragraph which links you to these instructions.</span></span>

<span data-ttu-id="a6f8a-121">运行你的应用，并确保你可以扫描 QR 代码和验证身份验证器证明的代码。</span><span class="sxs-lookup"><span data-stu-id="a6f8a-121">Run your app and ensure that you can scan the QR code and validate the code the authenticator proves.</span></span>

## <a name="change-the-site-name-in-the-qr-code"></a><span data-ttu-id="a6f8a-122">更改 QR 代码中的站点名称</span><span class="sxs-lookup"><span data-stu-id="a6f8a-122">Change the site name in the QR Code</span></span>

<span data-ttu-id="a6f8a-123">最初创建你的项目时选择的项目名称中获取 QR 代码中的站点名称。</span><span class="sxs-lookup"><span data-stu-id="a6f8a-123">The site name in the QR Code is taken from the project name you choose when initially creating your project.</span></span> <span data-ttu-id="a6f8a-124">你可以通过查找对其进行更改`GenerateQrCodeUri(string email, string unformattedKey)`中的方法*EnableAuthenticator.cshtml.cs*文件。</span><span class="sxs-lookup"><span data-stu-id="a6f8a-124">You can change it by looking for the `GenerateQrCodeUri(string email, string unformattedKey)` method in  the *EnableAuthenticator.cshtml.cs* file.</span></span> 

<span data-ttu-id="a6f8a-125">从模板的默认代码将如下所示：</span><span class="sxs-lookup"><span data-stu-id="a6f8a-125">The default code from the template looks as follows:</span></span>

```c#
private string GenerateQrCodeUri(string email, string unformattedKey)
{
    return string.Format(
        AuthenicatorUriFormat,
        _urlEncoder.Encode("Razor Pages"),
        _urlEncoder.Encode(email),
        unformattedKey);
}
```

<span data-ttu-id="a6f8a-126">第二个参数的调用中`string.Format`是你的站点名称，取自解决方案名称。</span><span class="sxs-lookup"><span data-stu-id="a6f8a-126">The second parameter in the call to `string.Format` is your site name, taken from your solution name.</span></span> <span data-ttu-id="a6f8a-127">它可以更改为任何值，但它必须始终为 URL 编码。</span><span class="sxs-lookup"><span data-stu-id="a6f8a-127">It can be changed to any value, but it must always be URL encoded.</span></span>

## <a name="using-a-different-qr-code-library"></a><span data-ttu-id="a6f8a-128">使用不同的 QR 代码库</span><span class="sxs-lookup"><span data-stu-id="a6f8a-128">Using a different QR Code library</span></span>

<span data-ttu-id="a6f8a-129">QR 代码库可以替换你首选的库。</span><span class="sxs-lookup"><span data-stu-id="a6f8a-129">You can replace the QR Code library with your preferred library.</span></span> <span data-ttu-id="a6f8a-130">HTML 包含`qrCode`元素在其中可以通过任何机制将 QR 代码你的库提供。</span><span class="sxs-lookup"><span data-stu-id="a6f8a-130">The HTML contains a `qrCode` element into which you can place a QR Code by whatever mechanism your library provides.</span></span>

<span data-ttu-id="a6f8a-131">QR 代码的格式正确 URL 可用于:</span><span class="sxs-lookup"><span data-stu-id="a6f8a-131">The correctly formatted URL for the QR Code is available in the:</span></span>

* <span data-ttu-id="a6f8a-132">`AuthenticatorUri`模型的属性。</span><span class="sxs-lookup"><span data-stu-id="a6f8a-132">`AuthenticatorUri` property of the model.</span></span>
* <span data-ttu-id="a6f8a-133">`data-url`中的属性`qrCodeData`元素。</span><span class="sxs-lookup"><span data-stu-id="a6f8a-133">`data-url` property in the `qrCodeData` element.</span></span> 

<span data-ttu-id="a6f8a-134">使用`@Html.Raw`访问视图中的模型属性 （否则为 & 符的 url 中将双编码和 QR 代码的标签参数将被忽略）。</span><span class="sxs-lookup"><span data-stu-id="a6f8a-134">Use `@Html.Raw` to access the model property in a view (otherwise the ampersands in the url will be double encoded and the label parameter of the QR Code will be ignored).</span></span>
