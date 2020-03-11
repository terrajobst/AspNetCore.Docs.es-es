Ejecute el scaffolding de identidad:

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto > **Agregar** > **nuevo elemento con scaffolding**.
* En el panel izquierdo del cuadro de diálogo **Agregar scaffold** , seleccione **Identity** > **Agregar**.
* En el cuadro de diálogo **Agregar identidad** , seleccione las opciones que desee.
  * Seleccione la página de diseño existente o el archivo de diseño se sobrescribirá con un marcado incorrecto. Por ejemplo `~/Pages/Shared/_Layout.cshtml` para los proyectos de Razor Pages `~/Views/Shared/_Layout.cshtml` para MVC
  * Seleccione el botón **+** para crear una nueva **clase de contexto de datos**.
* Seleccione **Agregar**.

# <a name="net-core-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

Si no ha instalado previamente el proveedor de scaffolding de ASP.NET Core, instalar ahora:

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

Agregue las referencias de paquete de NuGet necesarias al archivo de proyecto (\*. csproj). Ejecute el siguiente comando en el directorio del proyecto:

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.AspNetCore.Identity.EntityFrameworkCore
dotnet add package Microsoft.AspNetCore.Identity.UI
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet add package Microsoft.EntityFrameworkCore.Tools
```

Ejecute el siguiente comando para enumerar las opciones de proveedor de scaffolding de identidad:

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

[!INCLUDE[](~/includes/scaffoldTFM.md)]

En la carpeta del proyecto, ejecute el scaffolding de identidad con las opciones que desee. Por ejemplo, para configurar la identidad con la interfaz de usuario predeterminada y el número mínimo de archivos, ejecute el siguiente comando:

```dotnetcli
dotnet aspnet-codegenerator identity --useDefaultUI
```

---
