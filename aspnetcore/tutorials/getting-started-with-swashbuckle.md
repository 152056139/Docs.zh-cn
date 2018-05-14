---
title: Swashbuckle 和 ASP.NET Core 入门
author: zuckerthoben
description: 了解如何将 Swashbuckle 添加到 ASP.NET Core 项目中以集成 Swagger UI。
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 03/26/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/get-started-with-swashbuckle
ms.openlocfilehash: e90339f2884dd9b20cf135f879c9cab6110efecf
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-swashbuckle-and-aspnet-core"></a>Swashbuckle 和 ASP.NET Core 入门

作者：[Shayne Boyer](https://twitter.com/spboyer) 和 [Scott Addie](https://twitter.com/Scott_Addie)

Swashbuckle 有三个主要组成部分：

* [Swashbuckle.AspNetCore.Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/)：将 `SwaggerDocument` 对象公开为 JSON 终结点的 Swagger 对象模型和中间件。

* [Swashbuckle.AspNetCore.SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/)：从路由、控制器和模型直接生成 `SwaggerDocument` 对象的 Swagger 生成器。 它通常与 Swagger 终结点中间件结合，以自动公开 Swagger JSON。

* [Swashbuckle.AspNetCore.SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/)：Swagger UI 工具的嵌入式版本。 它解释 Swagger JSON 以构建描述 Web API 功能的可自定义的丰富体验。 它包括针对公共方法的内置测试工具。

## <a name="package-installation"></a>包安装

可以使用以下方法来添加 Swashbuckle：

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 从“程序包管理器控制台”窗口：

    ```powershell
    Install-Package Swashbuckle.AspNetCore
    ```

* 从“管理 NuGet 程序包”对话框中：

  * 右键单击“解决方案资源管理器” > “管理 NuGet 包”中的项目
  * 将“包源”设置为“nuget.org”
  * 在搜索框中输入“Swashbuckle.AspNetCore”
  * 从“浏览”选项卡中选择“Swashbuckle.AspNetCore”包，然后单击“安装”

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* 右键单击“Solution Pad” > “添加包...”中的“包”文件夹
* 将“添加包”窗口的“源”下拉列表设置为“nuget.org”
* 在搜索框中输入 Swashbuckle.AspNetCore
* 从结果窗格中选择“Swashbuckle.AspNetCore”包，然后单击“添加包”

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

从“集成终端”中运行以下命令：

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

运行下面的命令：

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a>添加并配置 Swagger 中间件

将 Swagger 生成器添加到 `Startup.ConfigureServices` 方法中的服务集合中：

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=7-10)]

导入以下命名空间以使用 `Info` 类：

```csharp
using Swashbuckle.AspNetCore.Swagger;
```

在 `Startup.Configure` 方法中，启用中间件为生成的 JSON 文档和 Swagger UI 提供服务：

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Startup2.cs?name=snippet_Configure&highlight=4,7-10)]

启动应用，并导航到 `http://localhost:<random_port>/swagger/v1/swagger.json`。 生成的描述终结点的文档显示在 [Swagger 规范 (swagger.json)](xref:tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson) 中。

可在 `http://localhost:<random_port>/swagger` 找到 Swagger UI。 通过 Swagger UI 浏览 API，并将其合并其他计划中。

> [!TIP]
> 要在应用的根 (`http://localhost:<random_port>/`) 处提供 Swagger UI，请将 `RoutePrefix` 属性设置为空字符串：
> 
> [!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Startup3.cs?name=snippet_UseSwaggerUI&highlight=4)]

## <a name="customize--extend"></a>自定义和扩展

Swagger 提供了为对象模型进行归档和自定义 UI 以匹配你的主题的选项。

### <a name="api-info-and-description"></a>API 信息和说明

传递给 `AddSwaggerGen` 方法的配置操作会添加诸如作者、许可证和说明的信息：

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Startup.cs?range=21-40,46)]

Swagger UI 显示版本的信息：

![包含版本信息的 Swagger UI：说明、作者以及查看更多链接](web-api-help-pages-using-swagger/_static/custom-info.png)

### <a name="xml-comments"></a>XML 注释

可使用以下方法启用 XML 注释：

#### <a name="visual-studiotabvisual-studio-xml"></a>[Visual Studio](#tab/visual-studio-xml/)
* 右键单击“解决方案资源管理器”中的项目，然后选择“属性”
* 查看“生成”选项卡的“输出”部分下的“XML 文档文件”框：

![项目属性的“生成”选项卡](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

#### <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[Visual Studio for Mac](#tab/visual-studio-mac-xml/)
* 打开“项目选项”对话框 >“生成” > “编译器”
* 查看“常规选项”部分下的“生成 xml 文档”框：

![项目选项的“常规选项”部分](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

#### <a name="visual-studio-codetabvisual-studio-code-xml"></a>[Visual Studio Code](#tab/visual-studio-code-xml/)
将以下代码片段手动添加到 .csproj 文件：

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=2)]

* * *
启用 XML 注释，为未记录的公共类型和成员提供调试信息。 警告消息指示未记录的类型和成员。 例如，以下消息指示违反警告代码 1591：

```text
warning CS1591: Missing XML comment for publicly visible type or member 'TodoController.GetAll()'
```

定义要在 .csproj 文件中忽略的以分号分隔的警告代码列表，以取消警告：

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=3)]

配置 Swagger 以使用生成的 XML 文件。 对于 Linux 或非 Windows 操作系统，文件名和路径区分大小写。 例如，“TodoApi.XML”文件在 Windows 上有效，但在 CentOS 上无效。

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=29-31)]

在上述代码中，[反射](/dotnet/csharp/programming-guide/concepts/reflection)用于生成与 Web API 项目相匹配的 XML 文件名。 此方法可确保生成的 XML 文件名与项目名称匹配。 [AppContext.BaseDirectory](/dotnet/api/system.appcontext.basedirectory#System_AppContext_BaseDirectory)属性用于构造 XML 文件的路径。

通过向节标题添加说明，将三斜杠注释添加到操作增强了 Swagger UI。 执行 `Delete` 操作前添加 [\<summary>](/dotnet/csharp/programming-guide/xmldoc/summary) 元素：

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Delete&highlight=1-3)]

Swagger UI 显示上述代码的 `<summary>` 元素的内部文本：

![显示 DELETE 方法的 XML 注释“删除特定 TodoItem” 的 Swagger UI](web-api-help-pages-using-swagger/_static/triple-slash-comments.png)

生成的 JSON 架构驱动 UI：

```json
"delete": {
    "tags": [
        "Todo"
    ],
    "summary": "Deletes a specific TodoItem.",
    "operationId": "ApiTodoByIdDelete",
    "consumes": [],
    "produces": [],
    "parameters": [
        {
            "name": "id",
            "in": "path",
            "description": "",
            "required": true,
            "type": "integer",
            "format": "int64"
        }
    ],
    "responses": {
        "200": {
            "description": "Success"
        }
    }
}
```

将 [\<remarks>](/dotnet/csharp/programming-guide/xmldoc/remarks) 元素添加到 `Create` 操作方法文档。 它可以补充 `<summary>` 元素中指定的信息，并提供更可靠的 Swagger UI。 `<remarks>` 元素内容可包含文本、JSON 或 XML。

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

请注意带这些附加注释的 UI 增强功能：

![显示包含其他注释的 Swagger UI](web-api-help-pages-using-swagger/_static/xml-comments-extended.png)

### <a name="data-annotations"></a>数据注释

使用 [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) 命名空间中的属性来修饰模型，以帮助驱动 Swagger UI 组件。

将 `[Required]` 属性添加到 `TodoItem` 类的 `Name` 属性：

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Models/TodoItem.cs?highlight=10)]

此属性的状态更改 UI 行为并更改基础 JSON 架构：

```json
"definitions": {
    "TodoItem": {
        "required": [
            "name"
        ],
        "type": "object",
        "properties": {
            "id": {
                "format": "int64",
                "type": "integer"
            },
            "name": {
                "type": "string"
            },
            "isComplete": {
                "default": false,
                "type": "boolean"
            }
        }
    }
},
```

将 `[Produces("application/json")]` 属性添加到 API 控制器。 这样做的目的是声明控制器的操作支持 application/json 的响应内容类型：

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=3)]

“响应内容类型”下拉列表选此内容类型作为控制器的默认 GET 操作：

![包含默认响应内容类型的 Swagger UI](web-api-help-pages-using-swagger/_static/json-response-content-type.png)

随着 Web API 中的数据注释的使用越来越多，UI 和 API 帮助页变得更具说明性和更为有用。

### <a name="describe-response-types"></a>描述响应类型

消费应用程序开发人员最关心的问题是返回的内容 &mdash; 具体的响应类型和错误代码（如果不标准）。 在 XML 注释和数据注释中表示响应类型和错误代码。

`Create` 操作成功后返回 HTTP 201 状态代码。 发布的请求正文为 NULL 时，将返回 HTTP 400 状态代码。 如果 Swagger UI 中没有提供合适的文档，那么使用者会缺少对这些预期结果的了解。 在以下示例中，通过添加突出显示的行解决此问题：

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

Swagger UI 现在清楚地记录预期的 HTTP 响应代码：

![Swagger UI 针对“响应消息”下的状态代码和原因显示 POST 响应类描述“返回新建的待办事项”和“400 - 如果该项为 null”](web-api-help-pages-using-swagger/_static/data-annotations-response-types.png)

### <a name="customize-the-ui"></a>自定义 UI

股票 UI 既实用又可呈现。 但是，API 文档页应代表品牌或主题。 将 Swashbuckle 组件标记为需要添加资源以提供静态文件，并构建文件夹结构以托管这些文件。

如果以 .NET Framework 或 .NET Core 1.x 为目标，请将 [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles) NuGet 包添加到项目：

```xml
<PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="2.0.0" />
```

如果以 .NET Core 2.x 为目标并使用[元包](xref:fundamentals/metapackage)，则已安装上述 NuGet 包。

启用静态文件中间件：

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Startup.cs?name=snippet_Configure&highlight=3)]

从 [Swagger UI GitHub 存储库](https://github.com/swagger-api/swagger-ui/tree/master/dist)中获取 dist 文件夹的内容。 此文件夹包含 Swagger UI 页必需的资产。

创建 wwwroot/swagger/ui 文件夹，然后将 dist 文件夹的内容复制到其中。

使用以下 CSS 在 wwwroot/swagger/ui 中创建 custom.css 文件，以自定义页面标题：

[!code-css[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/wwwroot/swagger/ui/custom.css)]

引用其他 CSS 文件后，引用 index.html 文件中的“custom.css”：

[!code-html[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/wwwroot/swagger/ui/index.html?name=snippet_SwaggerUiCss&highlight=3)]

浏览到 `http://localhost:<random_port>/swagger/ui/index.html` 中的 index.html 页。 在标题文本框中输入 `http://localhost:<random_port>/swagger/v1/swagger.json`，然后单击“浏览”按钮。 生成的页面如下所示：

![使用自定义标题的 Swagger UI](web-api-help-pages-using-swagger/_static/custom-header.png)

还可以对页面执行更多操作。 在 [Swagger UI GitHub 存储库](https://github.com/swagger-api/swagger-ui)中查看 UI 资源的完整功能。
