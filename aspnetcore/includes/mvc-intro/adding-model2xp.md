<a name="cli"></a>
## <a name="perform-initial-migration"></a>Realizar la migración inicial

Desde la línea de comandos, ejecute los comandos CLI de .NET Core:

```console
dotnet ef migrations add InitialCreate
dotnet ef database update
```

El comando `dotnet ef migrations add InitialCreate` genera el código para crear el esquema de base de datos inicial. El esquema se basa en el modelo especificado en `DbContext` (en el archivo *Models/MovieContext.cs*). El argumento `Initial` se usa para asignar un nombre a las migraciones. Puede usar cualquier nombre, pero se suele elegir un nombre que describa la migración. Consulte [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) (Introducción a las migraciones) para obtener más información.

El comando `dotnet ef database update` ejecuta el método `Up` en el archivo *Migrations/\<time-stamp>_InitialCreate.cs*, que crea la base de datos.