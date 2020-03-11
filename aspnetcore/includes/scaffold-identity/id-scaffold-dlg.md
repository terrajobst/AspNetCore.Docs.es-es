<span data-ttu-id="f4b01-101">Ejecute el scaffolding de identidad:</span><span class="sxs-lookup"><span data-stu-id="f4b01-101">Run the Identity scaffolder:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="f4b01-102">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f4b01-102">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f4b01-103">En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto > **Agregar** > **nuevo elemento con scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="f4b01-103">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="f4b01-104">En el panel izquierdo del cuadro de diálogo **Agregar scaffold** , seleccione **Identity** > **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="f4b01-104">From the left pane of the **Add Scaffold** dialog, select **Identity** > **ADD**.</span></span>
* <span data-ttu-id="f4b01-105">En el cuadro de diálogo **Agregar identidad** , seleccione las opciones que desee.</span><span class="sxs-lookup"><span data-stu-id="f4b01-105">In the **ADD Identity** dialog, select the options you want.</span></span>
  * <span data-ttu-id="f4b01-106">Seleccione la página de diseño existente o el archivo de diseño se sobrescribirá con un marcado incorrecto.</span><span class="sxs-lookup"><span data-stu-id="f4b01-106">Select your existing layout page, or your layout file will be overwritten with incorrect markup.</span></span> <span data-ttu-id="f4b01-107">Por ejemplo `~/Pages/Shared/_Layout.cshtml` para los proyectos de Razor Pages `~/Views/Shared/_Layout.cshtml` para MVC</span><span class="sxs-lookup"><span data-stu-id="f4b01-107">For example `~/Pages/Shared/_Layout.cshtml` for Razor Pages `~/Views/Shared/_Layout.cshtml` for MVC projects</span></span>
  * <span data-ttu-id="f4b01-108">Seleccione el botón **+** para crear una nueva **clase de contexto de datos**.</span><span class="sxs-lookup"><span data-stu-id="f4b01-108">Select the **+** button to create a new **Data context class**.</span></span>
* <span data-ttu-id="f4b01-109">Seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="f4b01-109">Select **ADD**.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="f4b01-110">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="f4b01-110">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="f4b01-111">Si no ha instalado previamente el proveedor de scaffolding de ASP.NET Core, instalar ahora:</span><span class="sxs-lookup"><span data-stu-id="f4b01-111">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="f4b01-112">Agregue las referencias de paquete de NuGet necesarias al archivo de proyecto (\*. csproj).</span><span class="sxs-lookup"><span data-stu-id="f4b01-112">Add required NuGet package references to the project (\*.csproj) file.</span></span> <span data-ttu-id="f4b01-113">Ejecute el siguiente comando en el directorio del proyecto:</span><span class="sxs-lookup"><span data-stu-id="f4b01-113">Run the following command in the project directory:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.AspNetCore.Identity.EntityFrameworkCore
dotnet add package Microsoft.AspNetCore.Identity.UI
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet add package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="f4b01-114">Ejecute el siguiente comando para enumerar las opciones de proveedor de scaffolding de identidad:</span><span class="sxs-lookup"><span data-stu-id="f4b01-114">Run the following command to list the Identity scaffolder options:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

[!INCLUDE[](~/includes/scaffoldTFM.md)]

<span data-ttu-id="f4b01-115">En la carpeta del proyecto, ejecute el scaffolding de identidad con las opciones que desee.</span><span class="sxs-lookup"><span data-stu-id="f4b01-115">In the project folder, run the Identity scaffolder with the options you want.</span></span> <span data-ttu-id="f4b01-116">Por ejemplo, para configurar la identidad con la interfaz de usuario predeterminada y el número mínimo de archivos, ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="f4b01-116">For example, to setup identity with the default UI and the minimum number of files, run the following command:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity --useDefaultUI
```

---
