---
title: "在 ASP.NET 核心中使用 Gulp"
author: rick-anderson
description: "了解如何在 ASP.NET 核心中使用 Gulp。"
keywords: "ASP.NET 核心 Gulp"
ms.author: riande
manager: wpickett
ms.date: 02/28/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/using-gulp
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 68f6838889cfb830f2c5a1976b3140ae5d94ac25
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
# <a name="introduction-to-using-gulp-in-aspnet-core"></a><span data-ttu-id="c04d6-104">在 ASP.NET 核心中使用 Gulp 简介</span><span class="sxs-lookup"><span data-stu-id="c04d6-104">Introduction to using Gulp in ASP.NET Core</span></span> 

<span data-ttu-id="c04d6-105">通过[艾力克 Reitan](https://github.com/Erikre)， [Scott Addie](https://scottaddie.com)， [Daniel Roth](https://github.com/danroth27)，和[Shayne 贝叶](https://twitter.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="c04d6-105">By [Erik Reitan](https://github.com/Erikre), [Scott Addie](https://scottaddie.com), [Daniel Roth](https://github.com/danroth27), and [Shayne Boyer](https://twitter.com/spboyer)</span></span>

<span data-ttu-id="c04d6-106">在典型的现代 web 应用中，可能会生成过程：</span><span class="sxs-lookup"><span data-stu-id="c04d6-106">In a typical modern web app, the build process might:</span></span>

* <span data-ttu-id="c04d6-107">捆绑和 minify JavaScript 和 CSS 文件。</span><span class="sxs-lookup"><span data-stu-id="c04d6-107">Bundle and minify JavaScript and CSS files.</span></span>
* <span data-ttu-id="c04d6-108">运行工具以调用之前每个生成的绑定和缩减任务。</span><span class="sxs-lookup"><span data-stu-id="c04d6-108">Run tools to call the bundling and minification tasks before each build.</span></span>
* <span data-ttu-id="c04d6-109">小于编译或 SASS 文件复制到 CSS。</span><span class="sxs-lookup"><span data-stu-id="c04d6-109">Compile LESS or SASS files to CSS.</span></span>
* <span data-ttu-id="c04d6-110">编译 CoffeeScript 或 TypeScript 文件添加到 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="c04d6-110">Compile CoffeeScript or TypeScript files to JavaScript.</span></span>

<span data-ttu-id="c04d6-111">A*任务运行程序*是一种工具，可以自动进行这些例程开发任务和的详细信息。</span><span class="sxs-lookup"><span data-stu-id="c04d6-111">A *task runner* is a tool which automates these routine development tasks and more.</span></span> <span data-ttu-id="c04d6-112">Visual Studio 为两个流行的基于 JavaScript 的任务流道提供内置支持： [Gulp](https://gulpjs.com/)和[Grunt](using-grunt.md)。</span><span class="sxs-lookup"><span data-stu-id="c04d6-112">Visual Studio provides built-in support for two popular JavaScript-based task runners: [Gulp](https://gulpjs.com/) and [Grunt](using-grunt.md).</span></span>

## <a name="gulp"></a><span data-ttu-id="c04d6-113">gulp</span><span class="sxs-lookup"><span data-stu-id="c04d6-113">Gulp</span></span>

<span data-ttu-id="c04d6-114">Gulp 是一个基于 JavaScript 的流式处理生成工具包，客户端代码。</span><span class="sxs-lookup"><span data-stu-id="c04d6-114">Gulp is a JavaScript-based streaming build toolkit for client-side code.</span></span> <span data-ttu-id="c04d6-115">它通常用于在生成环境中触发特定事件时流通过一系列的进程的客户端文件。</span><span class="sxs-lookup"><span data-stu-id="c04d6-115">It is commonly used to stream client-side files through a series of processes when a specific event is triggered in a build environment.</span></span> <span data-ttu-id="c04d6-116">例如，使用 Gulp 来自动执行[绑定和缩减](bundling-and-minification.md)或清理新生成前的开发环境。</span><span class="sxs-lookup"><span data-stu-id="c04d6-116">For instance, Gulp can be used to automate [bundling and minification](bundling-and-minification.md) or the cleansing of a development environment before a new build.</span></span>

<span data-ttu-id="c04d6-117">在中定义一组 Gulp 任务*gulpfile.js*。</span><span class="sxs-lookup"><span data-stu-id="c04d6-117">A set of Gulp tasks is defined in *gulpfile.js*.</span></span> <span data-ttu-id="c04d6-118">以下 JavaScript 包括 Gulp 模块，并指定文件路径，以在即将推出的任务中引用：</span><span class="sxs-lookup"><span data-stu-id="c04d6-118">The following JavaScript includes Gulp modules and specifies file paths to be referenced within the forthcoming tasks:</span></span>

```javascript
/// <binding Clean='clean' />
"use strict";

var gulp = require("gulp"),
  rimraf = require("rimraf"),
  concat = require("gulp-concat"),
  cssmin = require("gulp-cssmin"),
  uglify = require("gulp-uglify");

var paths = {
  webroot: "./wwwroot/"
};

paths.js = paths.webroot + "js/**/*.js";
paths.minJs = paths.webroot + "js/**/*.min.js";
paths.css = paths.webroot + "css/**/*.css";
paths.minCss = paths.webroot + "css/**/*.min.css";
paths.concatJsDest = paths.webroot + "js/site.min.js";
paths.concatCssDest = paths.webroot + "css/site.min.css";
```

<span data-ttu-id="c04d6-119">上面的代码中指定的节点模块所需。</span><span class="sxs-lookup"><span data-stu-id="c04d6-119">The above code specifies which Node modules are required.</span></span> <span data-ttu-id="c04d6-120">`require`函数导入每个模块，以便依赖任务可以使用其功能。</span><span class="sxs-lookup"><span data-stu-id="c04d6-120">The `require` function imports each module so that the dependent tasks can utilize their features.</span></span> <span data-ttu-id="c04d6-121">每个导入的模块被分配给变量。</span><span class="sxs-lookup"><span data-stu-id="c04d6-121">Each of the imported modules is assigned to a variable.</span></span> <span data-ttu-id="c04d6-122">模块可以位于按名称或路径。</span><span class="sxs-lookup"><span data-stu-id="c04d6-122">The modules can be located either by name or path.</span></span> <span data-ttu-id="c04d6-123">在此示例中，模块名为`gulp`， `rimraf`， `gulp-concat`， `gulp-cssmin`，和`gulp-uglify`按名称检索。</span><span class="sxs-lookup"><span data-stu-id="c04d6-123">In this example, the modules named `gulp`, `rimraf`, `gulp-concat`, `gulp-cssmin`, and `gulp-uglify` are retrieved by name.</span></span> <span data-ttu-id="c04d6-124">此外，创建一系列的路径，以便可以重复使用并在任务中引用的 CSS 和 JavaScript 文件的位置。</span><span class="sxs-lookup"><span data-stu-id="c04d6-124">Additionally, a series of paths are created so that the locations of CSS and JavaScript files can be reused and referenced within the tasks.</span></span> <span data-ttu-id="c04d6-125">下表提供了的模块中包含的描述*gulpfile.js*。</span><span class="sxs-lookup"><span data-stu-id="c04d6-125">The following table provides descriptions of the modules included in *gulpfile.js*.</span></span>

|<span data-ttu-id="c04d6-126">模块名</span><span class="sxs-lookup"><span data-stu-id="c04d6-126">Module Name</span></span>|<span data-ttu-id="c04d6-127">描述</span><span class="sxs-lookup"><span data-stu-id="c04d6-127">Description</span></span>|
|---|---|
|<span data-ttu-id="c04d6-128">gulp</span><span class="sxs-lookup"><span data-stu-id="c04d6-128">gulp</span></span>|<span data-ttu-id="c04d6-129">Gulp 流式处理生成系统中。</span><span class="sxs-lookup"><span data-stu-id="c04d6-129">The Gulp streaming build system.</span></span> <span data-ttu-id="c04d6-130">有关详细信息，请参阅[gulp](https://www.npmjs.com/package/gulp)。</span><span class="sxs-lookup"><span data-stu-id="c04d6-130">For more information, see [gulp](https://www.npmjs.com/package/gulp).</span></span>|
|<span data-ttu-id="c04d6-131">rimraf</span><span class="sxs-lookup"><span data-stu-id="c04d6-131">rimraf</span></span>|<span data-ttu-id="c04d6-132">节点删除模块。</span><span class="sxs-lookup"><span data-stu-id="c04d6-132">A Node deletion module.</span></span> <span data-ttu-id="c04d6-133">有关详细信息，请参阅[rimraf](https://www.npmjs.com/package/rimraf)。</span><span class="sxs-lookup"><span data-stu-id="c04d6-133">For more information, see [rimraf](https://www.npmjs.com/package/rimraf).</span></span>|
|<span data-ttu-id="c04d6-134">gulp concat</span><span class="sxs-lookup"><span data-stu-id="c04d6-134">gulp-concat</span></span>|<span data-ttu-id="c04d6-135">一个连接基于操作系统的换行字符的文件的模块。</span><span class="sxs-lookup"><span data-stu-id="c04d6-135">A module that concatenates files based on the operating system’s newline character.</span></span> <span data-ttu-id="c04d6-136">有关详细信息，请参阅[gulp concat](https://www.npmjs.com/package/gulp-concat)。</span><span class="sxs-lookup"><span data-stu-id="c04d6-136">For more information, see [gulp-concat](https://www.npmjs.com/package/gulp-concat).</span></span>|
|<span data-ttu-id="c04d6-137">gulp cssmin</span><span class="sxs-lookup"><span data-stu-id="c04d6-137">gulp-cssmin</span></span>|<span data-ttu-id="c04d6-138">Minifies CSS 文件模块。</span><span class="sxs-lookup"><span data-stu-id="c04d6-138">A module that minifies CSS files.</span></span> <span data-ttu-id="c04d6-139">有关详细信息，请参阅[gulp cssmin](https://www.npmjs.com/package/gulp-cssmin)。</span><span class="sxs-lookup"><span data-stu-id="c04d6-139">For more information, see [gulp-cssmin](https://www.npmjs.com/package/gulp-cssmin).</span></span>|
|<span data-ttu-id="c04d6-140">gulp uglify</span><span class="sxs-lookup"><span data-stu-id="c04d6-140">gulp-uglify</span></span>|<span data-ttu-id="c04d6-141">Minifies 模块*.js*文件。</span><span class="sxs-lookup"><span data-stu-id="c04d6-141">A module that minifies *.js* files.</span></span> <span data-ttu-id="c04d6-142">有关详细信息，请参阅[gulp uglify](https://www.npmjs.com/package/gulp-uglify)。</span><span class="sxs-lookup"><span data-stu-id="c04d6-142">For more information, see [gulp-uglify](https://www.npmjs.com/package/gulp-uglify).</span></span>|

<span data-ttu-id="c04d6-143">必备项的模块将导入后，可以指定任务。</span><span class="sxs-lookup"><span data-stu-id="c04d6-143">Once the requisite modules are imported, the tasks can be specified.</span></span> <span data-ttu-id="c04d6-144">此处有六项任务注册，表示通过以下代码：</span><span class="sxs-lookup"><span data-stu-id="c04d6-144">Here there are six tasks registered, represented by the following code:</span></span>

```javascript
gulp.task("clean:js", function (cb) {
  rimraf(paths.concatJsDest, cb);
});

gulp.task("clean:css", function (cb) {
  rimraf(paths.concatCssDest, cb);
});

gulp.task("clean", ["clean:js", "clean:css"]);

gulp.task("min:js", function () {
  return gulp.src([paths.js, "!" + paths.minJs], { base: "." })
    .pipe(concat(paths.concatJsDest))
    .pipe(uglify())
    .pipe(gulp.dest("."));
});

gulp.task("min:css", function () {
  return gulp.src([paths.css, "!" + paths.minCss])
    .pipe(concat(paths.concatCssDest))
    .pipe(cssmin())
    .pipe(gulp.dest("."));
});

gulp.task("min", ["min:js", "min:css"]);
```

<span data-ttu-id="c04d6-145">下表提供了在上面的代码中指定的任务的说明：</span><span class="sxs-lookup"><span data-stu-id="c04d6-145">The following table provides an explanation of the tasks specified in the code above:</span></span>

|<span data-ttu-id="c04d6-146">任务名称</span><span class="sxs-lookup"><span data-stu-id="c04d6-146">Task Name</span></span>|<span data-ttu-id="c04d6-147">描述</span><span class="sxs-lookup"><span data-stu-id="c04d6-147">Description</span></span>|
|--- |--- |
|<span data-ttu-id="c04d6-148">干净： js</span><span class="sxs-lookup"><span data-stu-id="c04d6-148">clean:js</span></span>|<span data-ttu-id="c04d6-149">使用 rimraf 节点删除模块删除 site.js 文件的缩减的版本的任务。</span><span class="sxs-lookup"><span data-stu-id="c04d6-149">A task that uses the rimraf Node deletion module to remove the minified version of the site.js file.</span></span>|
|<span data-ttu-id="c04d6-150">干净： css</span><span class="sxs-lookup"><span data-stu-id="c04d6-150">clean:css</span></span>|<span data-ttu-id="c04d6-151">使用 rimraf 节点删除模块删除 site.css 文件的缩减的版本的任务。</span><span class="sxs-lookup"><span data-stu-id="c04d6-151">A task that uses the rimraf Node deletion module to remove the minified version of the site.css file.</span></span>|
|<span data-ttu-id="c04d6-152">清理</span><span class="sxs-lookup"><span data-stu-id="c04d6-152">clean</span></span>|<span data-ttu-id="c04d6-153">一个任务，它调用`clean:js`任务后, 跟`clean:css`任务。</span><span class="sxs-lookup"><span data-stu-id="c04d6-153">A task that calls the `clean:js` task, followed by the `clean:css` task.</span></span>|
|<span data-ttu-id="c04d6-154">min:js</span><span class="sxs-lookup"><span data-stu-id="c04d6-154">min:js</span></span>|<span data-ttu-id="c04d6-155">Minifies 和串联的 js 文件夹中的所有.js 文件的任务。</span><span class="sxs-lookup"><span data-stu-id="c04d6-155">A task that minifies and concatenates all .js files within the js folder.</span></span> <span data-ttu-id="c04d6-156">。 排除 min.js 文件。</span><span class="sxs-lookup"><span data-stu-id="c04d6-156">The .min.js files are excluded.</span></span>|
|<span data-ttu-id="c04d6-157">min:css</span><span class="sxs-lookup"><span data-stu-id="c04d6-157">min:css</span></span>|<span data-ttu-id="c04d6-158">Minifies 和串联的 css 文件夹中的所有.css 文件的任务。</span><span class="sxs-lookup"><span data-stu-id="c04d6-158">A task that minifies and concatenates all .css files within the css folder.</span></span> <span data-ttu-id="c04d6-159">。 排除 min.css 文件。</span><span class="sxs-lookup"><span data-stu-id="c04d6-159">The .min.css files are excluded.</span></span>|
|<span data-ttu-id="c04d6-160">min</span><span class="sxs-lookup"><span data-stu-id="c04d6-160">min</span></span>|<span data-ttu-id="c04d6-161">一个任务，它调用`min:js`任务后, 跟`min:css`任务。</span><span class="sxs-lookup"><span data-stu-id="c04d6-161">A task that calls the `min:js` task, followed by the `min:css` task.</span></span>|

## <a name="running-default-tasks"></a><span data-ttu-id="c04d6-162">正在运行的默认任务</span><span class="sxs-lookup"><span data-stu-id="c04d6-162">Running default tasks</span></span>

<span data-ttu-id="c04d6-163">如果你尚未创建新的 Web 应用程序，请在 Visual Studio 中创建新的 ASP.NET Web 应用程序项目。</span><span class="sxs-lookup"><span data-stu-id="c04d6-163">If you haven’t already created a new Web app, create a new ASP.NET Web Application project in Visual Studio.</span></span>

1.  <span data-ttu-id="c04d6-164">将新的 JavaScript 文件添加到你的项目并将其命名*gulpfile.js*，然后将以下代码复制。</span><span class="sxs-lookup"><span data-stu-id="c04d6-164">Add a new JavaScript file to your project and name it *gulpfile.js*, then copy the following code.</span></span>

    ```javascript
    /// <binding Clean='clean' />
    "use strict";
    
    var gulp = require("gulp"),
      rimraf = require("rimraf"),
      concat = require("gulp-concat"),
      cssmin = require("gulp-cssmin"),
      uglify = require("gulp-uglify");
    
    var paths = {
      webroot: "./wwwroot/"
    };
    
    paths.js = paths.webroot + "js/**/*.js";
    paths.minJs = paths.webroot + "js/**/*.min.js";
    paths.css = paths.webroot + "css/**/*.css";
    paths.minCss = paths.webroot + "css/**/*.min.css";
    paths.concatJsDest = paths.webroot + "js/site.min.js";
    paths.concatCssDest = paths.webroot + "css/site.min.css";
    
    gulp.task("clean:js", function (cb) {
      rimraf(paths.concatJsDest, cb);
    });
    
    gulp.task("clean:css", function (cb) {
      rimraf(paths.concatCssDest, cb);
    });
    
    gulp.task("clean", ["clean:js", "clean:css"]);
    
    gulp.task("min:js", function () {
      return gulp.src([paths.js, "!" + paths.minJs], { base: "." })
        .pipe(concat(paths.concatJsDest))
        .pipe(uglify())
        .pipe(gulp.dest("."));
    });
    
    gulp.task("min:css", function () {
      return gulp.src([paths.css, "!" + paths.minCss])
        .pipe(concat(paths.concatCssDest))
        .pipe(cssmin())
        .pipe(gulp.dest("."));
    });
    
    gulp.task("min", ["min:js", "min:css"]);
    ```

2.  <span data-ttu-id="c04d6-165">打开*package.json*文件 (添加如果不是存在) 并添加以下。</span><span class="sxs-lookup"><span data-stu-id="c04d6-165">Open the *package.json* file (add if not there) and add the following.</span></span>

    ```json
    {
      "devDependencies": {
        "gulp": "3.9.1",
        "gulp-concat": "2.6.1",
        "gulp-cssmin": "0.1.7",
        "gulp-uglify": "2.0.1",
        "rimraf": "2.6.1"
      }
    }
    ```

3.  <span data-ttu-id="c04d6-166">在**解决方案资源管理器**，右键单击*gulpfile.js*，然后选择**任务运行程序资源管理器**。</span><span class="sxs-lookup"><span data-stu-id="c04d6-166">In **Solution Explorer**, right-click *gulpfile.js*, and select **Task Runner Explorer**.</span></span>
    
    ![从解决方案资源管理器中打开任务运行程序资源管理器](using-gulp/_static/02-SolutionExplorer-TaskRunnerExplorer.png)
    
    <span data-ttu-id="c04d6-168">**任务运行程序资源管理器**显示 Gulp 任务的列表。</span><span class="sxs-lookup"><span data-stu-id="c04d6-168">**Task Runner Explorer** shows the list of Gulp tasks.</span></span> <span data-ttu-id="c04d6-169">(你可能需要单击**刷新**显示项目名称左侧的按钮。)</span><span class="sxs-lookup"><span data-stu-id="c04d6-169">(You might have to click the **Refresh** button that appears to the left of the project name.)</span></span>
    
    ![任务运行程序资源管理器](using-gulp/_static/03-TaskRunnerExplorer.png)

4.  <span data-ttu-id="c04d6-171">在此之下**任务**中**任务运行程序资源管理器**，右键单击**干净**，然后选择**运行**从弹出菜单。</span><span class="sxs-lookup"><span data-stu-id="c04d6-171">Underneath **Tasks** in **Task Runner Explorer**, right-click **clean**, and select **Run** from the pop-up menu.</span></span>

    ![任务运行程序资源管理器清理任务](using-gulp/_static/04-TaskRunner-clean.png)

    <span data-ttu-id="c04d6-173">**任务运行程序资源管理器**将创建名为的新选项卡**干净**和执行清理任务，如中定义*gulpfile.js*。</span><span class="sxs-lookup"><span data-stu-id="c04d6-173">**Task Runner Explorer** will create a new tab named **clean** and execute the clean task as it is defined in *gulpfile.js*.</span></span>

5.  <span data-ttu-id="c04d6-174">右键单击**干净**任务，然后选择**绑定** > **之前生成**。</span><span class="sxs-lookup"><span data-stu-id="c04d6-174">Right-click the **clean** task, then select **Bindings** > **Before Build**.</span></span>

    ![任务运行程序资源管理器绑定 BeforeBuild](using-gulp/_static/05-TaskRunner-BeforeBuild.png)

    <span data-ttu-id="c04d6-176">**之前生成**绑定将配置 clean 任务，以在每次生成项目之前自动运行。</span><span class="sxs-lookup"><span data-stu-id="c04d6-176">The **Before Build** binding configures the clean task to run automatically before each build of the project.</span></span>

<span data-ttu-id="c04d6-177">绑定您设置**任务运行程序资源管理器**顶部的注释的形式存储在你*gulpfile.js*和仅在 Visual Studio 中有效。</span><span class="sxs-lookup"><span data-stu-id="c04d6-177">The bindings you set up with **Task Runner Explorer** are stored in the form of a comment at the top of your *gulpfile.js* and are effective only in Visual Studio.</span></span> <span data-ttu-id="c04d6-178">不需要 Visual Studio 的替代方法是配置 gulp 中的任务自动执行你*.csproj*文件。</span><span class="sxs-lookup"><span data-stu-id="c04d6-178">An alternative that doesn't require Visual Studio is to configure automatic execution of gulp tasks in your *.csproj* file.</span></span> <span data-ttu-id="c04d6-179">例如，将这个放你*.csproj*文件：</span><span class="sxs-lookup"><span data-stu-id="c04d6-179">For example, put this in your *.csproj* file:</span></span>

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
  <Exec Command="gulp clean" />
</Target>
```

<span data-ttu-id="c04d6-180">现在 Visual Studio 中或从命令提示符处使用运行项目时执行清理任务`dotnet run`命令 (运行`npm install`第一个)。</span><span class="sxs-lookup"><span data-stu-id="c04d6-180">Now the clean task is executed when you run the project in Visual Studio or from a command prompt using the `dotnet run` command (run `npm install` first).</span></span>

## <a name="defining-and-running-a-new-task"></a><span data-ttu-id="c04d6-181">定义和运行新任务</span><span class="sxs-lookup"><span data-stu-id="c04d6-181">Defining and running a new task</span></span>

<span data-ttu-id="c04d6-182">若要定义新的 Gulp 任务，修改*gulpfile.js*。</span><span class="sxs-lookup"><span data-stu-id="c04d6-182">To define a new Gulp task, modify *gulpfile.js*.</span></span>

1.  <span data-ttu-id="c04d6-183">末尾添加以下 JavaScript *gulpfile.js*:</span><span class="sxs-lookup"><span data-stu-id="c04d6-183">Add the following JavaScript to the end of *gulpfile.js*:</span></span>

    ```javascript
    gulp.task("first", function () {
      console.log('first task! <-----');
    });
    ```

    <span data-ttu-id="c04d6-184">此任务名为`first`，和它只需显示的字符串。</span><span class="sxs-lookup"><span data-stu-id="c04d6-184">This task is named `first`, and it simply displays a string.</span></span>

2.  <span data-ttu-id="c04d6-185">保存*gulpfile.js*。</span><span class="sxs-lookup"><span data-stu-id="c04d6-185">Save *gulpfile.js*.</span></span>

3.  <span data-ttu-id="c04d6-186">在**解决方案资源管理器**，右键单击*gulpfile.js*，然后选择*任务运行程序资源管理器*。</span><span class="sxs-lookup"><span data-stu-id="c04d6-186">In **Solution Explorer**, right-click *gulpfile.js*, and select *Task Runner Explorer*.</span></span>

4.  <span data-ttu-id="c04d6-187">在**任务运行程序资源管理器**，右键单击**第一个**，然后选择**运行**。</span><span class="sxs-lookup"><span data-stu-id="c04d6-187">In **Task Runner Explorer**, right-click **first**, and select **Run**.</span></span>

    ![任务运行程序资源管理器运行第一个任务](using-gulp/_static/06-TaskRunner-First.png)

    <span data-ttu-id="c04d6-189">你将看到，输出文本已显示。</span><span class="sxs-lookup"><span data-stu-id="c04d6-189">You’ll see that the output text is displayed.</span></span> <span data-ttu-id="c04d6-190">如果你有兴趣基于一种常见方案的示例，请参阅的 Gulp 配方。</span><span class="sxs-lookup"><span data-stu-id="c04d6-190">If you are interested in examples based on a common scenario, see Gulp Recipes.</span></span>

## <a name="defining-and-running-tasks-in-a-series"></a><span data-ttu-id="c04d6-191">定义和一系列中运行任务</span><span class="sxs-lookup"><span data-stu-id="c04d6-191">Defining and running tasks in a series</span></span>

<span data-ttu-id="c04d6-192">当你运行多个任务时，任务将默认情况下同时运行。</span><span class="sxs-lookup"><span data-stu-id="c04d6-192">When you run multiple tasks, the tasks run concurrently by default.</span></span> <span data-ttu-id="c04d6-193">但是，如果你需要以特定顺序运行任务，你必须指定每个任务过程何时完成，以及为哪些任务依赖于另一个任务完成。</span><span class="sxs-lookup"><span data-stu-id="c04d6-193">However, if you need to run tasks in a specific order, you must specify when each task is complete, as well as which tasks depend on the completion of another task.</span></span>

1.  <span data-ttu-id="c04d6-194">若要定义要按顺序运行的任务的一系列，替换`first`中前面添加的任务*gulpfile.js*替换为以下：</span><span class="sxs-lookup"><span data-stu-id="c04d6-194">To define a series of tasks to run in order, replace the `first` task that you added above in *gulpfile.js* with the following:</span></span>

    ```javascript
    gulp.task("series:first", function () {
      console.log('first task! <-----');
    });
 
    gulp.task("series:second", ["series:first"], function () {
      console.log('second task! <-----');
    });
 
    gulp.task("series", ["series:first", "series:second"], function () {});
    ```
 
    <span data-ttu-id="c04d6-195">你现在具有三个任务： `series:first`， `series:second`，和`series`。</span><span class="sxs-lookup"><span data-stu-id="c04d6-195">You now have three tasks: `series:first`, `series:second`, and `series`.</span></span> <span data-ttu-id="c04d6-196">`series:second`任务包括指定任务要运行和完成之前的数组的第二个参数`series:second`任务将运行。</span><span class="sxs-lookup"><span data-stu-id="c04d6-196">The `series:second` task includes a second parameter which specifies an array of tasks to be run and completed before the `series:second` task will run.</span></span>  <span data-ttu-id="c04d6-197">根据上面，唯一的代码中的指定`series:first`任务必须完成之前`series:second`任务将运行。</span><span class="sxs-lookup"><span data-stu-id="c04d6-197">As specified in the code above, only the `series:first` task must be completed before the `series:second` task will run.</span></span>

2.  <span data-ttu-id="c04d6-198">保存*gulpfile.js*。</span><span class="sxs-lookup"><span data-stu-id="c04d6-198">Save *gulpfile.js*.</span></span>

3.  <span data-ttu-id="c04d6-199">在**解决方案资源管理器**，右键单击*gulpfile.js*和选择**任务运行程序资源管理器**如果尚未打开。</span><span class="sxs-lookup"><span data-stu-id="c04d6-199">In **Solution Explorer**, right-click *gulpfile.js* and select **Task Runner Explorer** if it isn’t already open.</span></span>

4.  <span data-ttu-id="c04d6-200">在**任务运行程序资源管理器**，右键单击**系列**和选择**运行**。</span><span class="sxs-lookup"><span data-stu-id="c04d6-200">In **Task Runner Explorer**, right-click **series** and select **Run**.</span></span>

    ![任务运行程序资源管理器运行系列任务](using-gulp/_static/07-TaskRunner-Series.png)

## <a name="intellisense"></a><span data-ttu-id="c04d6-202">IntelliSense</span><span class="sxs-lookup"><span data-stu-id="c04d6-202">IntelliSense</span></span>

<span data-ttu-id="c04d6-203">IntelliSense 提供了代码完成、 参数说明和其他功能来提高工作效率，并减少错误。</span><span class="sxs-lookup"><span data-stu-id="c04d6-203">IntelliSense provides code completion, parameter descriptions, and other features to boost productivity and to decrease errors.</span></span> <span data-ttu-id="c04d6-204">用 JavaScript; 编写 gulp 任务因此，IntelliSense 可以提供开发时的协助。</span><span class="sxs-lookup"><span data-stu-id="c04d6-204">Gulp tasks are written in JavaScript; therefore, IntelliSense can provide assistance while developing.</span></span> <span data-ttu-id="c04d6-205">当你使用 JavaScript，IntelliSense 也会列出对象、 函数、 属性和可用的参数基于当前的上下文。</span><span class="sxs-lookup"><span data-stu-id="c04d6-205">As you work with JavaScript, IntelliSense lists the objects, functions, properties, and parameters that are available based on your current context.</span></span> <span data-ttu-id="c04d6-206">从 intellisense 来完成代码提供的弹出列表中选择编码选项。</span><span class="sxs-lookup"><span data-stu-id="c04d6-206">Select a coding option from the pop-up list provided by IntelliSense to complete the code.</span></span>

![IntelliSense 的 gulp](using-gulp/_static/08-IntelliSense.png)

<span data-ttu-id="c04d6-208">Intellisense 的详细信息，请参阅[JavaScript IntelliSense](https://docs.microsoft.com/visualstudio/ide/javascript-intellisense)。</span><span class="sxs-lookup"><span data-stu-id="c04d6-208">For more information about IntelliSense, see [JavaScript IntelliSense](https://docs.microsoft.com/visualstudio/ide/javascript-intellisense).</span></span>

## <a name="development-staging-and-production-environments"></a><span data-ttu-id="c04d6-209">开发、 过渡和生产环境</span><span class="sxs-lookup"><span data-stu-id="c04d6-209">Development, staging, and production environments</span></span>

<span data-ttu-id="c04d6-210">当 Gulp 用于优化过渡和生产客户端文件时，处理的文件将保存到本地的过渡和生产位置。</span><span class="sxs-lookup"><span data-stu-id="c04d6-210">When Gulp is used to optimize client-side files for staging and production, the processed files are saved to a local staging and production location.</span></span> <span data-ttu-id="c04d6-211">*_Layout.cshtml*文件使用**环境**标记帮助器提供两个不同版本的 CSS 文件。</span><span class="sxs-lookup"><span data-stu-id="c04d6-211">The *_Layout.cshtml* file uses the **environment** tag helper to provide two different versions of CSS files.</span></span> <span data-ttu-id="c04d6-212">CSS 文件的一个版本是用于开发和另一个版本进行了优化的过渡和生产。</span><span class="sxs-lookup"><span data-stu-id="c04d6-212">One version of CSS files is for development and the other version is optimized for both staging and production.</span></span> <span data-ttu-id="c04d6-213">在 Visual Studio 2017，当你更改**ASPNETCORE_ENVIRONMENT**环境变量`Production`，Visual Studio 将生成 Web 应用程序和链接到的最小化 CSS 文件。</span><span class="sxs-lookup"><span data-stu-id="c04d6-213">In Visual Studio 2017, when you change the **ASPNETCORE_ENVIRONMENT** environment variable to `Production`, Visual Studio will build the Web app and link to the minimized CSS files.</span></span> <span data-ttu-id="c04d6-214">下面的标记演示**环境**标记帮助程序包含将标记与`Development`CSS 文件和缩减`Staging, Production`CSS 文件。</span><span class="sxs-lookup"><span data-stu-id="c04d6-214">The following markup shows the **environment** tag helpers containing link tags to the `Development` CSS files and the minified `Staging, Production` CSS files.</span></span>

```html
<environment names="Development">
    <script src="~/lib/jquery/dist/jquery.js"></script>
    <script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
    <script src="~/js/site.js" asp-append-version="true"></script>
</environment>
<environment names="Staging,Production">
    <script src="https://ajax.aspnetcdn.com/ajax/jquery/jquery-2.2.0.min.js"
            asp-fallback-src="~/lib/jquery/dist/jquery.min.js"
            asp-fallback-test="window.jQuery"
            crossorigin="anonymous"
            integrity="sha384-K+ctZQ+LL8q6tP7I94W+qzQsfRV2a+AfHIi9k8z8l9ggpc8X+Ytst4yBo/hH+8Fk">
    </script>
    <script src="https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js"
            asp-fallback-src="~/lib/bootstrap/dist/js/bootstrap.min.js"
            asp-fallback-test="window.jQuery && window.jQuery.fn && window.jQuery.fn.modal"
            crossorigin="anonymous"
            integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa">
    </script>
    <script src="~/js/site.min.js" asp-append-version="true"></script>
</environment>
```

## <a name="switching-between-environments"></a><span data-ttu-id="c04d6-215">环境之间切换</span><span class="sxs-lookup"><span data-stu-id="c04d6-215">Switching between environments</span></span>

<span data-ttu-id="c04d6-216">若要针对不同的环境编译之间切换，请修改**ASPNETCORE_ENVIRONMENT**环境变量的值。</span><span class="sxs-lookup"><span data-stu-id="c04d6-216">To switch between compiling for different environments, modify the **ASPNETCORE_ENVIRONMENT** environment variable's value.</span></span>

1.  <span data-ttu-id="c04d6-217">在**任务运行程序资源管理器**，验证**min**任务已设置为运行**之前生成**。</span><span class="sxs-lookup"><span data-stu-id="c04d6-217">In **Task Runner Explorer**, verify that the **min** task has been set to run **Before Build**.</span></span>

2.  <span data-ttu-id="c04d6-218">在**解决方案资源管理器**，右键单击项目名称并选择**属性**。</span><span class="sxs-lookup"><span data-stu-id="c04d6-218">In **Solution Explorer**, right-click the project name and select **Properties**.</span></span>

    <span data-ttu-id="c04d6-219">在显示属性表为 Web 应用。</span><span class="sxs-lookup"><span data-stu-id="c04d6-219">The property sheet for the Web app is displayed.</span></span>

3.  <span data-ttu-id="c04d6-220">单击“调试”选项卡。</span><span class="sxs-lookup"><span data-stu-id="c04d6-220">Click the **Debug** tab.</span></span>

4.  <span data-ttu-id="c04d6-221">设置的值**宿主： 环境**环境变量`Production`。</span><span class="sxs-lookup"><span data-stu-id="c04d6-221">Set the value of the **Hosting:Environment** environment variable to `Production`.</span></span>

5.  <span data-ttu-id="c04d6-222">按**F5**在浏览器中运行该应用程序。</span><span class="sxs-lookup"><span data-stu-id="c04d6-222">Press **F5** to run the application in a browser.</span></span>

6.  <span data-ttu-id="c04d6-223">在浏览器窗口中，右击该页并选择**查看源**若要查看的 HTML 页。</span><span class="sxs-lookup"><span data-stu-id="c04d6-223">In the browser window, right-click the page and select **View Source** to view the HTML for the page.</span></span>

    <span data-ttu-id="c04d6-224">请注意，样式表链接指向的缩减的 CSS 文件。</span><span class="sxs-lookup"><span data-stu-id="c04d6-224">Notice that the stylesheet links point to the minified CSS files.</span></span>

7.  <span data-ttu-id="c04d6-225">关闭浏览器来停止 Web 应用。</span><span class="sxs-lookup"><span data-stu-id="c04d6-225">Close the browser to stop the Web app.</span></span>

8.  <span data-ttu-id="c04d6-226">在 Visual Studio 中，返回到 Web 应用的属性表，并更改**宿主： 环境**环境变量回`Development`。</span><span class="sxs-lookup"><span data-stu-id="c04d6-226">In Visual Studio, return to the property sheet for the Web app and change the **Hosting:Environment** environment variable back to `Development`.</span></span>

9.  <span data-ttu-id="c04d6-227">按**F5**再次在浏览器中运行该应用程序。</span><span class="sxs-lookup"><span data-stu-id="c04d6-227">Press **F5** to run the application in a browser again.</span></span>

10. <span data-ttu-id="c04d6-228">在浏览器窗口中，右击该页并选择**查看源**若要查看的 HTML 页。</span><span class="sxs-lookup"><span data-stu-id="c04d6-228">In the browser window, right-click the page and select **View Source** to see the HTML for the page.</span></span>

    <span data-ttu-id="c04d6-229">请注意，样式表链接指向 CSS 文件的 unminified 版本。</span><span class="sxs-lookup"><span data-stu-id="c04d6-229">Notice that the stylesheet links point to the unminified versions of the CSS files.</span></span>

<span data-ttu-id="c04d6-230">有关 ASP.NET 核心中的环境的详细信息，请参阅[使用多个环境](../fundamentals/environments.md)。</span><span class="sxs-lookup"><span data-stu-id="c04d6-230">For more information related to environments in ASP.NET Core, see [Working with Multiple Environments](../fundamentals/environments.md).</span></span>

## <a name="task-and-module-details"></a><span data-ttu-id="c04d6-231">任务和模块的详细信息</span><span class="sxs-lookup"><span data-stu-id="c04d6-231">Task and module details</span></span>

<span data-ttu-id="c04d6-232">Gulp 任务已注册到的函数名称。</span><span class="sxs-lookup"><span data-stu-id="c04d6-232">A Gulp task is registered with a function name.</span></span>  <span data-ttu-id="c04d6-233">如果其他任务都必须运行在当前任务之前，你可以指定依赖关系。</span><span class="sxs-lookup"><span data-stu-id="c04d6-233">You can specify dependencies if other tasks must run before the current task.</span></span> <span data-ttu-id="c04d6-234">其他函数，您可以运行和监视 Gulp 任务，以及将源设置 (*src*) 和目标 (*dest*) 正在修改的文件。</span><span class="sxs-lookup"><span data-stu-id="c04d6-234">Additional functions allow you to run and watch the Gulp tasks, as well as set the source (*src*) and destination (*dest*) of the files being modified.</span></span> <span data-ttu-id="c04d6-235">以下是主 Gulp API 函数：</span><span class="sxs-lookup"><span data-stu-id="c04d6-235">The following are the primary Gulp API functions:</span></span>

|<span data-ttu-id="c04d6-236">Gulp 函数</span><span class="sxs-lookup"><span data-stu-id="c04d6-236">Gulp Function</span></span>|<span data-ttu-id="c04d6-237">语法</span><span class="sxs-lookup"><span data-stu-id="c04d6-237">Syntax</span></span>|<span data-ttu-id="c04d6-238">描述</span><span class="sxs-lookup"><span data-stu-id="c04d6-238">Description</span></span>|
|---   |--- |--- |
|<span data-ttu-id="c04d6-239">任务</span><span class="sxs-lookup"><span data-stu-id="c04d6-239">task</span></span>  |`gulp.task(name[, deps], fn) { }`|<span data-ttu-id="c04d6-240">`task`函数创建的任务。</span><span class="sxs-lookup"><span data-stu-id="c04d6-240">The `task` function creates a task.</span></span> <span data-ttu-id="c04d6-241">`name`参数定义任务的名称。</span><span class="sxs-lookup"><span data-stu-id="c04d6-241">The `name` parameter defines the name of the task.</span></span> <span data-ttu-id="c04d6-242">`deps`参数包含要运行此任务之前完成的任务的数组。</span><span class="sxs-lookup"><span data-stu-id="c04d6-242">The `deps` parameter contains an array of tasks to be completed before this task runs.</span></span> <span data-ttu-id="c04d6-243">`fn`参数表示执行任务的操作的回调函数。</span><span class="sxs-lookup"><span data-stu-id="c04d6-243">The `fn` parameter represents a callback function which performs the operations of the task.</span></span>|
|<span data-ttu-id="c04d6-244">监视</span><span class="sxs-lookup"><span data-stu-id="c04d6-244">watch</span></span> |`gulp.watch(glob [, opts], tasks) { }`|<span data-ttu-id="c04d6-245">`watch`发生文件更改时，函数会监视文件和运行任务。</span><span class="sxs-lookup"><span data-stu-id="c04d6-245">The `watch` function monitors files and runs tasks when a file change occurs.</span></span> <span data-ttu-id="c04d6-246">`glob`参数是`string`或`array`，它确定要监视哪些文件。</span><span class="sxs-lookup"><span data-stu-id="c04d6-246">The `glob` parameter is a `string` or `array` that determines which files to watch.</span></span> <span data-ttu-id="c04d6-247">`opts`参数提供了监视选项的其他文件。</span><span class="sxs-lookup"><span data-stu-id="c04d6-247">The `opts` parameter provides additional file watching options.</span></span>|
|<span data-ttu-id="c04d6-248">src</span><span class="sxs-lookup"><span data-stu-id="c04d6-248">src</span></span>   |`gulp.src(globs[, options]) { }`|<span data-ttu-id="c04d6-249">`src`函数提供了 glob 值匹配的文件。</span><span class="sxs-lookup"><span data-stu-id="c04d6-249">The `src` function provides files that match the glob value(s).</span></span> <span data-ttu-id="c04d6-250">`glob`参数是`string`或`array`，它确定哪些文件读取。</span><span class="sxs-lookup"><span data-stu-id="c04d6-250">The `glob` parameter is a `string` or `array` that determines which files to read.</span></span> <span data-ttu-id="c04d6-251">`options`参数提供附加文件选项。</span><span class="sxs-lookup"><span data-stu-id="c04d6-251">The `options` parameter provides additional file options.</span></span>|
|<span data-ttu-id="c04d6-252">目标</span><span class="sxs-lookup"><span data-stu-id="c04d6-252">dest</span></span>  |`gulp.dest(path[, options]) { }`|<span data-ttu-id="c04d6-253">`dest`函数定义可以向其写入文件的位置。</span><span class="sxs-lookup"><span data-stu-id="c04d6-253">The `dest` function defines a location to which files can be written.</span></span> <span data-ttu-id="c04d6-254">`path`参数是字符串或确定目标文件夹的函数。</span><span class="sxs-lookup"><span data-stu-id="c04d6-254">The `path` parameter is a string or function that determines the destination folder.</span></span> <span data-ttu-id="c04d6-255">`options`参数是一个对象，指定输出文件夹的选项。</span><span class="sxs-lookup"><span data-stu-id="c04d6-255">The `options` parameter is an object that specifies output folder options.</span></span>|

<span data-ttu-id="c04d6-256">有关其他的 Gulp API 参考信息，请参阅[Gulp 文档 API](https://github.com/gulpjs/gulp/blob/master/docs/API.md)。</span><span class="sxs-lookup"><span data-stu-id="c04d6-256">For additional Gulp API reference information, see [Gulp Docs API](https://github.com/gulpjs/gulp/blob/master/docs/API.md).</span></span>

## <a name="gulp-recipes"></a><span data-ttu-id="c04d6-257">Gulp 配方</span><span class="sxs-lookup"><span data-stu-id="c04d6-257">Gulp recipes</span></span>

<span data-ttu-id="c04d6-258">Gulp 社区提供 Gulp[配方](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md)。</span><span class="sxs-lookup"><span data-stu-id="c04d6-258">The Gulp community provides Gulp [recipes](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md).</span></span> <span data-ttu-id="c04d6-259">这些配方包含 Gulp 任务用于针对常见情况。</span><span class="sxs-lookup"><span data-stu-id="c04d6-259">These recipes consist of Gulp tasks to address common scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c04d6-260">其他资源</span><span class="sxs-lookup"><span data-stu-id="c04d6-260">Additional resources</span></span>

* [<span data-ttu-id="c04d6-261">Gulp 文档</span><span class="sxs-lookup"><span data-stu-id="c04d6-261">Gulp documentation</span></span>](https://github.com/gulpjs/gulp/blob/master/docs/README.md)
* [<span data-ttu-id="c04d6-262">绑定和缩减中 ASP.NET 核心</span><span class="sxs-lookup"><span data-stu-id="c04d6-262">Bundling and minification in ASP.NET Core</span></span>](bundling-and-minification.md)
* [<span data-ttu-id="c04d6-263">在 ASP.NET 核心中使用 Grunt</span><span class="sxs-lookup"><span data-stu-id="c04d6-263">Using Grunt in ASP.NET Core</span></span>](using-grunt.md)
