# <a name="how-to-buildrun-secure-user-data-sample"></a><span data-ttu-id="0d1f2-101">如何生成/运行安全的用户数据示例</span><span class="sxs-lookup"><span data-stu-id="0d1f2-101">How to build/run Secure user data sample</span></span>

* <span data-ttu-id="0d1f2-102">设置密码与密码管理器工具：</span><span class="sxs-lookup"><span data-stu-id="0d1f2-102">Set password with the Secret Manager tool:</span></span>

  `dotnet user-secrets set SeedUserPW <pw>`

* <span data-ttu-id="0d1f2-103">更新数据库：</span><span class="sxs-lookup"><span data-stu-id="0d1f2-103">Update the database:</span></span>

    `dotnet ef database update`

* <span data-ttu-id="0d1f2-104">在项目中启用 SSL</span><span class="sxs-lookup"><span data-stu-id="0d1f2-104">Enable SSL in the project</span></span>