---
title: "启用 ASP.NET Core 中的身份验证器应用的 QR 代码生成"
author: rick-anderson
description: "启用 ASP.NET Core 中的身份验证器应用的 QR 代码生成"
manager: wpickett
ms.author: riande
ms.date: 09/24/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity-enable-qrcodes
ms.openlocfilehash: ae2d8eb938c00a26cf7ffb5f2fff0b9e0d22148b
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="enabling-qr-code-generation-for-authenticator-apps-in-aspnet-core"></a><span data-ttu-id="b1c5c-103">启用 ASP.NET Core 中的身份验证器应用的 QR 代码生成</span><span class="sxs-lookup"><span data-stu-id="b1c5c-103">Enabling QR Code generation for authenticator apps in ASP.NET Core</span></span>

<span data-ttu-id="b1c5c-104">注意： 本主题适用于 ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b1c5c-104">Note: This topic applies to ASP.NET Core 2.x</span></span>

<span data-ttu-id="b1c5c-105">ASP.NET 核心附带的单个身份验证的身份验证器应用程序的支持。</span><span class="sxs-lookup"><span data-stu-id="b1c5c-105">ASP.NET Core ships with support for authenticator applications for individual authentication.</span></span> <span data-ttu-id="b1c5c-106">两个因素身份验证 (2FA) 身份验证器应用，使用基于时间的一次性密码算法 (TOTP)，是推荐的方法为 2FA 行业。</span><span class="sxs-lookup"><span data-stu-id="b1c5c-106">Two factor authentication (2FA) authenticator apps, using a Time-based One-time Password Algorithm (TOTP), are the industry recommended approach for 2FA.</span></span> <span data-ttu-id="b1c5c-107">2FA 使用 TOTP 优于 SMS 2FA。</span><span class="sxs-lookup"><span data-stu-id="b1c5c-107">2FA using TOTP is preferred to SMS 2FA.</span></span> <span data-ttu-id="b1c5c-108">验证器应用提供哪些用户确认其用户名和密码后，必须输入一个 6 到 8 位代码。</span><span class="sxs-lookup"><span data-stu-id="b1c5c-108">An authenticator app provides a 6 to 8 digit code which users must enter after confirming their username and password.</span></span> <span data-ttu-id="b1c5c-109">通常在智能手机上安装验证器应用。</span><span class="sxs-lookup"><span data-stu-id="b1c5c-109">Typically an authenticator app is installed on a smart phone.</span></span>

<span data-ttu-id="b1c5c-110">ASP.NET 核心 web 应用程序模板支持身份验证器，但不提供对 QRCode 生成的支持。</span><span class="sxs-lookup"><span data-stu-id="b1c5c-110">The ASP.NET Core web app templates support authenticators, but don't provide support for QRCode generation.</span></span> <span data-ttu-id="b1c5c-111">QRCode 生成器轻松地 2FA 的安装程序。</span><span class="sxs-lookup"><span data-stu-id="b1c5c-111">QRCode generators ease the setup of 2FA.</span></span> <span data-ttu-id="b1c5c-112">本文档将指导你完成添加[QR 代码](https://wikipedia.org/wiki/QR_code)生成到 2FA 配置页。</span><span class="sxs-lookup"><span data-stu-id="b1c5c-112">This document will guide you through adding [QR Code](https://wikipedia.org/wiki/QR_code) generation to the 2FA configuration page.</span></span>

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a><span data-ttu-id="b1c5c-113">将 QR 代码添加到 2FA 配置页</span><span class="sxs-lookup"><span data-stu-id="b1c5c-113">Adding QR Codes to the 2FA configuration page</span></span>

<span data-ttu-id="b1c5c-114">这些说明使用*qrcode.js*从 https://davidshimjs.github.io/qrcodejs/ 存储库。</span><span class="sxs-lookup"><span data-stu-id="b1c5c-114">These instructions use *qrcode.js* from the https://davidshimjs.github.io/qrcodejs/ repo.</span></span>

* <span data-ttu-id="b1c5c-115">下载[qrcode.js javascript 库](https://davidshimjs.github.io/qrcodejs/)到`wwwroot\lib`项目文件夹中的。</span><span class="sxs-lookup"><span data-stu-id="b1c5c-115">Download the [qrcode.js javascript library](https://davidshimjs.github.io/qrcodejs/) to the `wwwroot\lib` folder in your project.</span></span>

* <span data-ttu-id="b1c5c-116">在*Pages\Account\Manage\EnableAuthenticator.cshtml* （Razor 页） 或*Views\Manage\EnableAuthenticator.cshtml* (MVC)、 找到`Scripts`文件末尾的部分：</span><span class="sxs-lookup"><span data-stu-id="b1c5c-116">In *Pages\Account\Manage\EnableAuthenticator.cshtml* (Razor Pages) or *Views\Manage\EnableAuthenticator.cshtml* (MVC), locate the `Scripts` section at the end of the file:</span></span>

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* <span data-ttu-id="b1c5c-117">更新`Scripts`部分添加到引用`qrcodejs`你添加的库和生成的 QR 代码的调用。</span><span class="sxs-lookup"><span data-stu-id="b1c5c-117">Update the `Scripts` section to add a reference to the `qrcodejs` library you added and a call to generate the QR Code.</span></span> <span data-ttu-id="b1c5c-118">其外观应，如下所示：</span><span class="sxs-lookup"><span data-stu-id="b1c5c-118">It should look as follows:</span></span>

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")

    <script type="text/javascript" src="~/lib/qrcode.js"></script>
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

* <span data-ttu-id="b1c5c-119">删除的段落，您链接到这些说明。</span><span class="sxs-lookup"><span data-stu-id="b1c5c-119">Delete the paragraph which links you to these instructions.</span></span>

<span data-ttu-id="b1c5c-120">运行你的应用，并确保你可以扫描 QR 代码和验证身份验证器证明的代码。</span><span class="sxs-lookup"><span data-stu-id="b1c5c-120">Run your app and ensure that you can scan the QR code and validate the code the authenticator proves.</span></span>

## <a name="change-the-site-name-in-the-qr-code"></a><span data-ttu-id="b1c5c-121">更改 QR 代码中的站点名称</span><span class="sxs-lookup"><span data-stu-id="b1c5c-121">Change the site name in the QR Code</span></span>

<span data-ttu-id="b1c5c-122">最初创建你的项目时选择的项目名称中获取 QR 代码中的站点名称。</span><span class="sxs-lookup"><span data-stu-id="b1c5c-122">The site name in the QR Code is taken from the project name you choose when initially creating your project.</span></span> <span data-ttu-id="b1c5c-123">你可以通过查找对其进行更改`GenerateQrCodeUri(string email, string unformattedKey)`中的方法*Pages\Account\Manage\EnableAuthenticator.cshtml.cs* （Razor 页） 文件或*Controllers\ManageController.cs* (MVC) 文件。</span><span class="sxs-lookup"><span data-stu-id="b1c5c-123">You can change it by looking for the `GenerateQrCodeUri(string email, string unformattedKey)` method in the *Pages\Account\Manage\EnableAuthenticator.cshtml.cs* (Razor Pages) file or the *Controllers\ManageController.cs* (MVC) file.</span></span> 

<span data-ttu-id="b1c5c-124">从模板的默认代码将如下所示：</span><span class="sxs-lookup"><span data-stu-id="b1c5c-124">The default code from the template looks as follows:</span></span>

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

<span data-ttu-id="b1c5c-125">第二个参数的调用中`string.Format`是你的站点名称，取自解决方案名称。</span><span class="sxs-lookup"><span data-stu-id="b1c5c-125">The second parameter in the call to `string.Format` is your site name, taken from your solution name.</span></span> <span data-ttu-id="b1c5c-126">它可以更改为任何值，但它必须始终为 URL 编码。</span><span class="sxs-lookup"><span data-stu-id="b1c5c-126">It can be changed to any value, but it must always be URL encoded.</span></span>

## <a name="using-a-different-qr-code-library"></a><span data-ttu-id="b1c5c-127">使用不同的 QR 代码库</span><span class="sxs-lookup"><span data-stu-id="b1c5c-127">Using a different QR Code library</span></span>

<span data-ttu-id="b1c5c-128">QR 代码库可以替换你首选的库。</span><span class="sxs-lookup"><span data-stu-id="b1c5c-128">You can replace the QR Code library with your preferred library.</span></span> <span data-ttu-id="b1c5c-129">HTML 包含`qrCode`元素在其中可以通过任何机制将 QR 代码你的库提供。</span><span class="sxs-lookup"><span data-stu-id="b1c5c-129">The HTML contains a `qrCode` element into which you can place a QR Code by whatever mechanism your library provides.</span></span>

<span data-ttu-id="b1c5c-130">QR 代码的格式正确 URL 可用于:</span><span class="sxs-lookup"><span data-stu-id="b1c5c-130">The correctly formatted URL for the QR Code is available in the:</span></span>

* <span data-ttu-id="b1c5c-131">`AuthenticatorUri`模型的属性。</span><span class="sxs-lookup"><span data-stu-id="b1c5c-131">`AuthenticatorUri` property of the model.</span></span>
* <span data-ttu-id="b1c5c-132">`data-url`中的属性`qrCodeData`元素。</span><span class="sxs-lookup"><span data-stu-id="b1c5c-132">`data-url` property in the `qrCodeData` element.</span></span> 

<span data-ttu-id="b1c5c-133">使用`@Html.Raw`访问视图中的模型属性 （否则为 & 符的 url 中将双编码和 QR 代码的标签参数将被忽略）。</span><span class="sxs-lookup"><span data-stu-id="b1c5c-133">Use `@Html.Raw` to access the model property in a view (otherwise the ampersands in the url will be double encoded and the label parameter of the QR Code will be ignored).</span></span>

## <a name="totp-client-and-server-time-skew"></a><span data-ttu-id="b1c5c-134">TOTP 客户端和服务器时间偏差</span><span class="sxs-lookup"><span data-stu-id="b1c5c-134">TOTP client and server time skew</span></span>

<span data-ttu-id="b1c5c-135">TOTP 身份验证依赖于服务器和身份验证器的设备具有精确的时间。</span><span class="sxs-lookup"><span data-stu-id="b1c5c-135">TOTP authentication depends on both the server and authenticator device having an accurate time.</span></span> <span data-ttu-id="b1c5c-136">仅在 30 秒内，最后一个令牌。</span><span class="sxs-lookup"><span data-stu-id="b1c5c-136">Tokens only last for 30 seconds.</span></span> <span data-ttu-id="b1c5c-137">如果未 TOTP 2FA 登录名，请检查服务器的时间比准确，并且最好是同步到准确 NTP 服务。</span><span class="sxs-lookup"><span data-stu-id="b1c5c-137">If TOTP 2FA logins are failing, check that the server time is accurate, and preferably synchronized to an accurate NTP service.</span></span>
