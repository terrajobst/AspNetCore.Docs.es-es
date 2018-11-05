<a name="cli"></a>

## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="799e1-101">Agregar herramientas de scaffolding y realizar la migración inicial</span><span class="sxs-lookup"><span data-stu-id="799e1-101">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="799e1-102">Agregue las líneas siguientes al archivo *RazorPagesMovie.csproj*, justo antes de la etiqueta `</Project>` de cierre:</span><span class="sxs-lookup"><span data-stu-id="799e1-102">Add the following lines to the *RazorPagesMovie.csproj* file, just before the closing `</Project>` tag:</span></span>

```xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Tools" Version="2.1.0-preview1-final"/>
</ItemGroup>
```
  
<span data-ttu-id="799e1-103">Desde la línea de comandos, ejecute los comandos CLI de .NET Core:</span><span class="sxs-lookup"><span data-stu-id="799e1-103">From the command line, run the following .NET Core CLI commands:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet ef migrations add InitialCreate
dotnet ef database update
```

<span data-ttu-id="799e1-104">El elemento `DotNetCliToolReference` y el comando `add package` instalan las herramientas necesarias para ejecutar el motor de scaffolding.</span><span class="sxs-lookup"><span data-stu-id="799e1-104">The `DotNetCliToolReference` element and the `add package` command install the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="799e1-105">El comando `ef migrations add InitialCreate` genera el código para crear el esquema de base de datos inicial.</span><span class="sxs-lookup"><span data-stu-id="799e1-105">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="799e1-106">El esquema se basa en el modelo especificado en `DbContext` (en el archivo *Models/MovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="799e1-106">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="799e1-107">El argumento `InitialCreate` se usa para asignar un nombre a las migraciones.</span><span class="sxs-lookup"><span data-stu-id="799e1-107">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="799e1-108">Puede usar cualquier nombre, pero se suele elegir uno que describa la migración.</span><span class="sxs-lookup"><span data-stu-id="799e1-108">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="799e1-109">Vea [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) (Introducción a las migraciones) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="799e1-109">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="799e1-110">El comando `ef database update` ejecuta el método `Up` en el archivo *Migrations/\<time-stamp>_InitialCreate.cs*, con lo que se crea la base de datos.</span><span class="sxs-lookup"><span data-stu-id="799e1-110">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>
