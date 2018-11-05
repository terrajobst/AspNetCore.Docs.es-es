<a name="cli"></a>

## <a name="add-scaffold-tooling-and-perform-initial-migration"></a>Agregar herramientas de scaffolding y realizar la migración inicial

Agregue las líneas siguientes al archivo *RazorPagesMovie.csproj*, justo antes de la etiqueta `</Project>` de cierre:

```xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Tools" Version="2.1.0-preview1-final"/>
</ItemGroup>
```
  
Desde la línea de comandos, ejecute los comandos CLI de .NET Core:

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet ef migrations add InitialCreate
dotnet ef database update
```

El elemento `DotNetCliToolReference` y el comando `add package` instalan las herramientas necesarias para ejecutar el motor de scaffolding.

El comando `ef migrations add InitialCreate` genera el código para crear el esquema de base de datos inicial. El esquema se basa en el modelo especificado en `DbContext` (en el archivo *Models/MovieContext.cs*). El argumento `InitialCreate` se usa para asignar un nombre a las migraciones. Puede usar cualquier nombre, pero se suele elegir uno que describa la migración. Vea [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) (Introducción a las migraciones) para obtener más información.

El comando `ef database update` ejecuta el método `Up` en el archivo *Migrations/\<time-stamp>_InitialCreate.cs*, con lo que se crea la base de datos.
