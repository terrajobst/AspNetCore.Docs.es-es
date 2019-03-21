---
ms.openlocfilehash: 698a127120f7f52672ceb927ff24eb1b02f91521
ms.sourcegitcommit: 088e6744cd67a62f214f25146313a53949b17d35
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/21/2019
ms.locfileid: "58320297"
---
<span data-ttu-id="dcbd0-101">Requiere el código de base de datos de identidad generado [migraciones de Entity Framework Core](/ef/core/managing-schemas/migrations/).</span><span class="sxs-lookup"><span data-stu-id="dcbd0-101">The generated Identity database code requires [Entity Framework Core Migrations](/ef/core/managing-schemas/migrations/).</span></span> <span data-ttu-id="dcbd0-102">Cree una migración y actualización de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="dcbd0-102">Create a migration and update the database.</span></span> <span data-ttu-id="dcbd0-103">Por ejemplo, ejecute los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="dcbd0-103">For example, run the following commands:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dcbd0-104">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dcbd0-104">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="dcbd0-105">En Visual Studio **Package Manager Console**:</span><span class="sxs-lookup"><span data-stu-id="dcbd0-105">In the Visual Studio **Package Manager Console**:</span></span>

```PMC
Add-Migration CreateIdentitySchema
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="dcbd0-106">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="dcbd0-106">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
```

---

<span data-ttu-id="dcbd0-107">El parámetro de nombre "CreateIdentitySchema" para el `Add-Migration` comando es arbitrario.</span><span class="sxs-lookup"><span data-stu-id="dcbd0-107">The "CreateIdentitySchema" name parameter for the `Add-Migration` command is arbitrary.</span></span> <span data-ttu-id="dcbd0-108">`"CreateIdentitySchema"` Describe la migración.</span><span class="sxs-lookup"><span data-stu-id="dcbd0-108">`"CreateIdentitySchema"` describes the migration.</span></span>
