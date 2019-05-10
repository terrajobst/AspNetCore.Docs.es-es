Requiere el código de base de datos de identidad generado [migraciones de Entity Framework Core](/ef/core/managing-schemas/migrations/). Cree una migración y actualización de la base de datos. Por ejemplo, ejecute los siguientes comandos:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

En Visual Studio **Package Manager Console**:

```PMC
Add-Migration CreateIdentitySchema
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

```cli
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
```

---

El parámetro de nombre "CreateIdentitySchema" para el `Add-Migration` comando es arbitrario. `"CreateIdentitySchema"` Describe la migración.
