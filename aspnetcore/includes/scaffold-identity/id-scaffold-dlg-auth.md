Ejecute al scaffolder de identidad:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* De **el Explorador de soluciones**, haga doble clic en el proyecto > **agregar** > **nuevo elemento de scaffolding**.
* En el panel izquierdo de la **agregar scaffolding** cuadro de diálogo, seleccione **identidad** > **agregar**.
* En el **Agregar identidad** cuadro de diálogo, seleccione las opciones que desee.
  * Seleccione la página de diseño existente, o se sobrescribirá el archivo de diseño con formato incorrecto. Cuando se selecciona un archivo _Layout.cshtml existente, es **no** sobrescribe.

 Por ejemplo `~/Pages/Shared/_Layout.cshtml` para las páginas de Razor `~/Views/Shared/_Layout.cshtml` para los proyectos MVC
* Para usar el contexto de datos existente, seleccione al menos un archivo para invalidar. Debe seleccionar al menos un archivo para agregar el contexto de datos.
  * Seleccione la clase de contexto de datos.
  * Seleccione **agregar**.
* Para crear un nuevo contexto de usuario y posiblemente crear una clase de usuario personalizado de identidad:
  * Seleccione el **+** botón para crear un nuevo **clase de contexto de datos**.
  * Seleccione **agregar**.

Nota: Si va a crear un nuevo contexto de usuario, no tienes que seleccionar un archivo para invalidar.

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

Si no ha instalado previamente el scaffolder ASP.NET, instalarla ahora:

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

Agregue una referencia de paquete a [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) al proyecto (\*.csproj) archivo. Ejecute el siguiente comando en el directorio del proyecto:

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

Ejecute el siguiente comando para mostrar las opciones de scaffolder de identidad:

```cli
dotnet aspnet-codegenerator identity -h
```

En la carpeta del proyecto, ejecute al scaffolder de identidad con las opciones que desee. Por ejemplo, para configurar la identidad con la interfaz de usuario predeterminada y el número mínimo de archivos, ejecute el siguiente comando. Use el nombre completo correcto para el contexto de base de datos:

```cli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files Account.Register
```

-------------
