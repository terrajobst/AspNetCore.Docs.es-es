El código de base de datos de identidad generado requiere [migraciones de Entity Framework Core](/ef/core/managing-schemas/migrations/). Cree una migración y actualización de la base de datos. Por ejemplo, ejecute los siguientes comandos:

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

En la consola del **Administrador de paquetes**de Visual Studio:

```powershell
Install-Package Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore
Add-Migration CreateIdentitySchema
Update-Database
```

# <a name="net-core-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

```dotnetcli
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
```

---

El parámetro de nombre "CreateIdentitySchema" para el comando `Add-Migration` es arbitrario. `"CreateIdentitySchema"` describe la migración.
