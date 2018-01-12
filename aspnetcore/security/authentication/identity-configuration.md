---
title: "配置 ASP.NET 核心标识"
author: AdrienTorris
description: "了解 ASP.NET 核心标识默认值，并配置要使用自定义值的各种标识属性。"
keywords: "ASP.NET 核心，标识、 身份验证安全性"
ms.author: scaddie
manager: wpickett
ms.date: 01/11/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-configuration
ms.openlocfilehash: ac204cb89aac1f90adc64c4f0bec4e946cb8c4d9
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2018
---
# <a name="configure-identity"></a>配置标识

ASP.NET 核心标识等密码策略、 锁定时间和可以在你的应用程序中轻松地重写的 cookie 设置应用程序中具有常见行为`Startup`类。

## <a name="passwords-policy"></a>密码策略

默认情况下，标识要求密码包含大写字符、 小写字符、 数字和非字母数字字符。 此外，还有一些其他限制。 若要简化密码限制，修改`ConfigureServices`方法`Startup`的你的应用程序的类。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

ASP.NET 核心 2.0 增加`RequiredUniqueChars`属性。 否则，选项是从 ASP.NET Core 相同 1.x。

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

`IdentityOptions.Password`具有以下属性：

| 属性                | 描述                       | 默认 |
| ----------------------- | --------------------------------- | ------- |
| `RequireDigit`          | 需要介于 0-9 中的密码。 | true |
| `RequiredLength`        | 密码最小长度。 | 6 |
| `RequireNonAlphanumeric`| 需要密码中的非字母数字字符。 | true |
| `RequireUppercase`      | 需要密码中的大写字符。 | true |
| `RequireLowercase`      | 需要密码中的小写字符。 | true |
| `RequiredUniqueChars`   | 需要密码中的非重复字符数。 | 1 |


## <a name="users-lockout"></a>用户的锁定

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

`IdentityOptions.Lockout`具有以下属性：

| 属性                | 描述                       | 默认 |
| ----------------------- | --------------------------------- | ------- |
| `DefaultLockoutTimeSpan` | 时间量锁定用户锁定发生时。  | 5 分钟  |
| `MaxFailedAccessAttempts` | 失败的访问尝试，直到用户已被锁定，如果启用了锁定次数。  | 5 |
| `AllowedForNewUsers` | 确定是否新用户会被锁定。  | true |

## <a name="sign-in-settings"></a>登录设置

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

`IdentityOptions.SignIn`具有以下属性：

| 属性                | 描述                       | 默认 |
| ----------------------- | --------------------------------- | ------- |
| `RequireConfirmedEmail` | 需要登录的确认电子邮件。 | False  |
| `RequireConfirmedPhoneNumber` |  需要一个确认的电话号码来登录。 | False  |

## <a name="user-validation-settings"></a>用户验证设置

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

`IdentityOptions.User`具有以下属性：

| 属性                | 描述                       | 默认 |
| ----------------------- | --------------------------------- | ------- |
| `RequireUniqueEmail`  | 要求每个用户具有唯一的电子邮件。 | False  |
| `AllowedUserNameCharacters`  | 允许使用的用户名中的字符。 | abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+ |


## <a name="applications-cookie-settings"></a>应用程序的 cookie 设置

密码策略，如应用程序的 cookie 的所有设置可以都更改在`Startup`类。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

下`ConfigureServices`中`Startup`类，你可以配置应用程序的 cookie。

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

---

`CookieAuthenticationOptions`具有以下属性：

| 属性                | 描述                       | 默认 |
| ----------------------- | --------------------------------- | ------- |
| `Cookie.Name`  | Cookie 的名称。  | .AspNetCore.Cookies。  |
| `Cookie.HttpOnly`  | 为 true 时，cookie 不能从客户端脚本访问。  |  true |
| `ExpireTimeSpan`  | 控制在 cookie 中存储身份验证票证的时间就会保持有效从它创建的点。  | 14 天  |
| `LoginPath`  | 未授权用户时，他们将被重定向到登录到此路径。 | / 帐户/登录名  |
| `LogoutPath`  | 当用户已注销时，则将被重定向到此路径。  | / 帐户/注销  |
| `AccessDeniedPath`  | 当用户失败时授权检查时，则将被重定向到此路径。  |   |
| `SlidingExpiration`  | 为 true 时，将使用新的过期时间，当前 cookie 时通过到期窗口的多个中间颁发一个新的 cookie。  | / 帐户/AccessDenied |
| `ReturnUrlParameter`  | 确定其 401 未授权的状态代码更改为 302 重定向到登录名路径时，该中间件会追加查询字符串参数的名称。  |  true |
| `AuthenticationScheme`  | 这是仅适用于 ASP.NET Core 1.x。 特定的身份验证方案逻辑名称。 |  |
| `AutomaticAuthenticate`  | 此标志才适用于 ASP.NET Core 1.x。 为 true 时，cookie 身份验证应在每个请求上运行并尝试验证并重新构造它创建的任何序列化的主体。  |  |