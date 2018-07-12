Ejecute el proveedor de scaffolding de identidad:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Desde **el Explorador de soluciones**, haga doble clic en el proyecto > **agregar** > **nuevo elemento de scaffolding**.
* En el panel izquierdo de la **agregar Scaffold** cuadro de diálogo, seleccione **identidad** > **agregar**.
* En el **identidad de ADD** cuadro de diálogo, seleccione las opciones que desee.
  * Seleccione la página de diseño existente, o se sobrescribirá el archivo de diseño con formato incorrecto. Por ejemplo `~/Pages/Shared/_Layout.cshtml` para las páginas de Razor `~/Views/Shared/_Layout.cshtml` para los proyectos de MVC
  * Seleccione el **+** botón para crear un nuevo **clase de contexto de datos**.
* Seleccione **agregar**.

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

Si no ha instalado previamente el proveedor de scaffolding ASP.NET, lo instale ahora:

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

Agregue una referencia de paquete a [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) al proyecto (\*.csproj) archivo. Ejecute el siguiente comando en el directorio del proyecto:

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

Ejecute el siguiente comando para enumerar las opciones de proveedor de scaffolding de identidad:

```cli
dotnet aspnet-codegenerator identity -h
```

En la carpeta del proyecto, ejecute el proveedor de scaffolding de identidad con las opciones que desee. Por ejemplo, para configurar la identidad con la interfaz de usuario predeterminada y el número mínimo de archivos, ejecute el siguiente comando:

```cli
dotnet aspnet-codegenerator identity --useDefaultUI
```

-------------
