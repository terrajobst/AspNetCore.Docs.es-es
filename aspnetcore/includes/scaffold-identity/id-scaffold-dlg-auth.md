<span data-ttu-id="18a61-101">Ejecute el scaffolding de identidad:</span><span class="sxs-lookup"><span data-stu-id="18a61-101">Run the Identity scaffolder:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="18a61-102">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="18a61-102">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="18a61-103">En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto > **Agregar** > **nuevo elemento con scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="18a61-103">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="18a61-104">En el panel izquierdo del cuadro de diálogo **Agregar scaffold** , seleccione **Identity** > **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="18a61-104">From the left pane of the **Add Scaffold** dialog, select **Identity** > **Add**.</span></span>
* <span data-ttu-id="18a61-105">En el cuadro de diálogo **Agregar identidad** , seleccione las opciones que desee.</span><span class="sxs-lookup"><span data-stu-id="18a61-105">In the **Add Identity** dialog, select the options you want.</span></span>
  * <span data-ttu-id="18a61-106">Seleccione la página de diseño existente o el archivo de diseño se sobrescribirá con un marcado incorrecto.</span><span class="sxs-lookup"><span data-stu-id="18a61-106">Select your existing layout page, or your layout file will be overwritten with incorrect markup.</span></span> <span data-ttu-id="18a61-107">Cuando se selecciona un archivo *\_layout. cshtml* existente, **no** se sobrescribe.</span><span class="sxs-lookup"><span data-stu-id="18a61-107">When an existing *\_Layout.cshtml* file is selected, it is **not** overwritten.</span></span>

 <span data-ttu-id="18a61-108">Por ejemplo: `~/Pages/Shared/_Layout.cshtml` para los proyectos de Razor Pages `~/Views/Shared/_Layout.cshtml` para MVC</span><span class="sxs-lookup"><span data-stu-id="18a61-108">For example: `~/Pages/Shared/_Layout.cshtml` for Razor Pages `~/Views/Shared/_Layout.cshtml` for MVC projects</span></span>
* <span data-ttu-id="18a61-109">Para usar el contexto de datos existente, seleccione al menos un archivo para invalidar.</span><span class="sxs-lookup"><span data-stu-id="18a61-109">To use your existing data context, select at least one file to override.</span></span> <span data-ttu-id="18a61-110">Debe seleccionar al menos un archivo para agregar el contexto de datos.</span><span class="sxs-lookup"><span data-stu-id="18a61-110">You must select at least one file to add your data context.</span></span>
  * <span data-ttu-id="18a61-111">Seleccione la clase de contexto de datos.</span><span class="sxs-lookup"><span data-stu-id="18a61-111">Select your data context class.</span></span>
  * <span data-ttu-id="18a61-112">Seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="18a61-112">Select **Add**.</span></span>
* <span data-ttu-id="18a61-113">Para crear un nuevo contexto de usuario y, posiblemente, crear una clase de usuario personalizada para la identidad:</span><span class="sxs-lookup"><span data-stu-id="18a61-113">To create a new user context and possibly create a custom user class for Identity:</span></span>
  * <span data-ttu-id="18a61-114">Seleccione el botón **+** para crear una nueva **clase de contexto de datos**.</span><span class="sxs-lookup"><span data-stu-id="18a61-114">Select the **+** button to create a new **Data context class**.</span></span>
  * <span data-ttu-id="18a61-115">Seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="18a61-115">Select **Add**.</span></span>

<span data-ttu-id="18a61-116">Nota: Si va a crear un nuevo contexto de usuario, no tiene que seleccionar un archivo para invalidar.</span><span class="sxs-lookup"><span data-stu-id="18a61-116">Note: If you're creating a new user context, you don't have to select a file to override.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="18a61-117">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="18a61-117">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="18a61-118">Si no ha instalado previamente el scaffolding de ASP.NET Core, instálelo ahora:</span><span class="sxs-lookup"><span data-stu-id="18a61-118">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="18a61-119">Agregue una referencia de paquete a [Microsoft. VisualStudio. Web. CodeGeneration. Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) al archivo de proyecto (\*. csproj).</span><span class="sxs-lookup"><span data-stu-id="18a61-119">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (\*.csproj) file.</span></span> <span data-ttu-id="18a61-120">Ejecute el siguiente comando en el directorio del proyecto:</span><span class="sxs-lookup"><span data-stu-id="18a61-120">Run the following command in the project directory:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="18a61-121">Ejecute el siguiente comando para enumerar las opciones del scaffolding de identidad:</span><span class="sxs-lookup"><span data-stu-id="18a61-121">Run the following command to list the Identity scaffolder options:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="18a61-122">En la carpeta del proyecto, ejecute el scaffolding de identidad con las opciones que desee.</span><span class="sxs-lookup"><span data-stu-id="18a61-122">In the project folder, run the Identity scaffolder with the options you want.</span></span> <span data-ttu-id="18a61-123">Por ejemplo, para configurar la identidad con la interfaz de usuario predeterminada y el número mínimo de archivos, ejecute el siguiente comando.</span><span class="sxs-lookup"><span data-stu-id="18a61-123">For example, to setup identity with the default UI and the minimum number of files, run the following command.</span></span> <span data-ttu-id="18a61-124">Use el nombre completo correcto para el contexto de base de BD:</span><span class="sxs-lookup"><span data-stu-id="18a61-124">Use the correct fully qualified name for your DB context:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login"
```

<span data-ttu-id="18a61-125">PowerShell usa punto y coma como separador de comandos.</span><span class="sxs-lookup"><span data-stu-id="18a61-125">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="18a61-126">Al usar PowerShell, use el carácter de escape de punto y coma en la lista de archivos o ponga la lista de archivos entre comillas dobles.</span><span class="sxs-lookup"><span data-stu-id="18a61-126">When using PowerShell, escape the semi-colons in the file list or put the file list in double quotes.</span></span> <span data-ttu-id="18a61-127">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="18a61-127">For example:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="18a61-128">Si ejecuta el scaffolding de identidad sin especificar la marca `--files` o la marca `--useDefaultUI`, se crearán todas las páginas de la interfaz de usuario de identidad disponibles en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="18a61-128">If you run the Identity scaffolder without specifying the `--files` flag or the `--useDefaultUI` flag, all the available Identity UI pages will be created in your project.</span></span>

---
