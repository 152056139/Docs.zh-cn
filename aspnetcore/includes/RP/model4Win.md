<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="0a750-101">搭建“电影”模型的基架</span><span class="sxs-lookup"><span data-stu-id="0a750-101">Scaffold the Movie model</span></span>

* <span data-ttu-id="0a750-102">打开项目目录（包含 Program.cs、Startup.cs 和 .csproj 文件的目录）中的命令窗口。</span><span class="sxs-lookup"><span data-stu-id="0a750-102">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="0a750-103">运行下面的命令：</span><span class="sxs-lookup"><span data-stu-id="0a750-103">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```
  
<span data-ttu-id="0a750-104">如果收到错误：</span><span class="sxs-lookup"><span data-stu-id="0a750-104">If you get the error:</span></span>
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

<span data-ttu-id="0a750-105">打开项目目录（包含 Program.cs、Startup.cs 和 .csproj 文件的目录）中的命令窗口。</span><span class="sxs-lookup"><span data-stu-id="0a750-105">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>

<span data-ttu-id="0a750-106">如果收到错误：</span><span class="sxs-lookup"><span data-stu-id="0a750-106">If you get the error:</span></span>
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

<span data-ttu-id="0a750-107">退出 Visual Studio，然后重新运行命令。</span><span class="sxs-lookup"><span data-stu-id="0a750-107">Exit Visual Studio and run the command again.</span></span>