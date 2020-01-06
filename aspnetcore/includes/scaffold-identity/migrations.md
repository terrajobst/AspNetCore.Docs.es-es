<span data-ttu-id="4577c-101">El código de base de datos de identidad generado requiere [migraciones de Entity Framework Core](/ef/core/managing-schemas/migrations/).</span><span class="sxs-lookup"><span data-stu-id="4577c-101">The generated Identity database code requires [Entity Framework Core Migrations](/ef/core/managing-schemas/migrations/).</span></span> <span data-ttu-id="4577c-102">Cree una migración y actualización de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="4577c-102">Create a migration and update the database.</span></span> <span data-ttu-id="4577c-103">Por ejemplo, ejecute los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="4577c-103">For example, run the following commands:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4577c-104">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4577c-104">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="4577c-105">En Visual Studio **Package Manager Console**:</span><span class="sxs-lookup"><span data-stu-id="4577c-105">In the Visual Studio **Package Manager Console**:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore
Add-Migration CreateIdentitySchema
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="4577c-106">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="4577c-106">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
```

---

<span data-ttu-id="4577c-107">El parámetro de nombre "CreateIdentitySchema" para el comando `Add-Migration` es arbitrario.</span><span class="sxs-lookup"><span data-stu-id="4577c-107">The "CreateIdentitySchema" name parameter for the `Add-Migration` command is arbitrary.</span></span> <span data-ttu-id="4577c-108">`"CreateIdentitySchema"` describe la migración.</span><span class="sxs-lookup"><span data-stu-id="4577c-108">`"CreateIdentitySchema"` describes the migration.</span></span>
