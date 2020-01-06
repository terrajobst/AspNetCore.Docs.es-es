::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="0a3bf-101">Ejecute el scaffolding de identidad:</span><span class="sxs-lookup"><span data-stu-id="0a3bf-101">Run the Identity scaffolder:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0a3bf-102">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0a3bf-102">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0a3bf-103">En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto > **Agregar** > **nuevo elemento con scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="0a3bf-103">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="0a3bf-104">En el panel izquierdo del cuadro de diálogo **Agregar scaffold** , seleccione **Identity** > **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="0a3bf-104">From the left pane of the **Add Scaffold** dialog, select **Identity** > **Add**.</span></span>
* <span data-ttu-id="0a3bf-105">En el cuadro de diálogo **Agregar identidad** , seleccione las opciones que desee.</span><span class="sxs-lookup"><span data-stu-id="0a3bf-105">In the **Add Identity** dialog, select the options you want.</span></span>
  * <span data-ttu-id="0a3bf-106">Seleccione la página de diseño existente o el archivo de diseño se sobrescribirá con un marcado incorrecto.</span><span class="sxs-lookup"><span data-stu-id="0a3bf-106">Select your existing layout page, or your layout file will be overwritten with incorrect markup.</span></span> <span data-ttu-id="0a3bf-107">Cuando se selecciona un archivo *\_layout. cshtml* existente, **no** se sobrescribe.</span><span class="sxs-lookup"><span data-stu-id="0a3bf-107">When an existing *\_Layout.cshtml* file is selected, it is **not** overwritten.</span></span>

 <span data-ttu-id="0a3bf-108">Por ejemplo: `~/Pages/Shared/_Layout.cshtml` para los proyectos de Razor Pages `~/Views/Shared/_Layout.cshtml` para MVC</span><span class="sxs-lookup"><span data-stu-id="0a3bf-108">For example: `~/Pages/Shared/_Layout.cshtml` for Razor Pages `~/Views/Shared/_Layout.cshtml` for MVC projects</span></span>
* <span data-ttu-id="0a3bf-109">Para usar el contexto de datos existente, seleccione al menos un archivo para invalidar.</span><span class="sxs-lookup"><span data-stu-id="0a3bf-109">To use your existing data context, select at least one file to override.</span></span> <span data-ttu-id="0a3bf-110">Debe seleccionar al menos un archivo para agregar el contexto de datos.</span><span class="sxs-lookup"><span data-stu-id="0a3bf-110">You must select at least one file to add your data context.</span></span>
  * <span data-ttu-id="0a3bf-111">Seleccione la clase de contexto de datos.</span><span class="sxs-lookup"><span data-stu-id="0a3bf-111">Select your data context class.</span></span>
  * <span data-ttu-id="0a3bf-112">Seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="0a3bf-112">Select **Add**.</span></span>
* <span data-ttu-id="0a3bf-113">Para crear un nuevo contexto de usuario y, posiblemente, crear una clase de usuario personalizada para la identidad:</span><span class="sxs-lookup"><span data-stu-id="0a3bf-113">To create a new user context and possibly create a custom user class for Identity:</span></span>
  * <span data-ttu-id="0a3bf-114">Seleccione el **+** botón para crear un nuevo **clase de contexto de datos**.</span><span class="sxs-lookup"><span data-stu-id="0a3bf-114">Select the **+** button to create a new **Data context class**.</span></span>
  * <span data-ttu-id="0a3bf-115">Seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="0a3bf-115">Select **Add**.</span></span>

<span data-ttu-id="0a3bf-116">Nota: Si va a crear un nuevo contexto de usuario, no tiene que seleccionar un archivo para invalidar.</span><span class="sxs-lookup"><span data-stu-id="0a3bf-116">Note: If you're creating a new user context, you don't have to select a file to override.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="0a3bf-117">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="0a3bf-117">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="0a3bf-118">Si no ha instalado previamente el proveedor de scaffolding de ASP.NET Core, instalar ahora:</span><span class="sxs-lookup"><span data-stu-id="0a3bf-118">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="0a3bf-119">Agregue las referencias de paquete de NuGet necesarias al archivo de proyecto (\*. csproj).</span><span class="sxs-lookup"><span data-stu-id="0a3bf-119">Add required NuGet package references to the project (\*.csproj) file.</span></span> <span data-ttu-id="0a3bf-120">Ejecute el siguiente comando en el directorio del proyecto:</span><span class="sxs-lookup"><span data-stu-id="0a3bf-120">Run the following command in the project directory:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.AspNetCore.Identity.EntityFrameworkCore
dotnet add package Microsoft.AspNetCore.Identity.UI
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet add package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="0a3bf-121">Ejecute el siguiente comando para enumerar las opciones de proveedor de scaffolding de identidad:</span><span class="sxs-lookup"><span data-stu-id="0a3bf-121">Run the following command to list the Identity scaffolder options:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

[!INCLUDE[](~/includes/scaffoldTFM.md)]

<span data-ttu-id="0a3bf-122">En la carpeta del proyecto, ejecute el scaffolding de identidad con las opciones que desee.</span><span class="sxs-lookup"><span data-stu-id="0a3bf-122">In the project folder, run the Identity scaffolder with the options you want.</span></span> <span data-ttu-id="0a3bf-123">Por ejemplo, para configurar la identidad con la interfaz de usuario predeterminada y el número mínimo de archivos, ejecute el siguiente comando.</span><span class="sxs-lookup"><span data-stu-id="0a3bf-123">For example, to setup identity with the default UI and the minimum number of files, run the following command.</span></span> <span data-ttu-id="0a3bf-124">Use el nombre completo correcto para el contexto de base de BD:</span><span class="sxs-lookup"><span data-stu-id="0a3bf-124">Use the correct fully qualified name for your DB context:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login"
```

<span data-ttu-id="0a3bf-125">PowerShell usa punto y coma como separador de comandos.</span><span class="sxs-lookup"><span data-stu-id="0a3bf-125">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="0a3bf-126">Al usar PowerShell, use el carácter de escape de punto y coma en la lista de archivos o ponga la lista de archivos entre comillas dobles.</span><span class="sxs-lookup"><span data-stu-id="0a3bf-126">When using PowerShell, escape the semi-colons in the file list or put the file list in double quotes.</span></span> <span data-ttu-id="0a3bf-127">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="0a3bf-127">For example:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="0a3bf-128">Si ejecuta el scaffolding de identidad sin especificar la marca `--files` o la marca `--useDefaultUI`, se crearán todas las páginas de la interfaz de usuario de identidad disponibles en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="0a3bf-128">If you run the Identity scaffolder without specifying the `--files` flag or the `--useDefaultUI` flag, all the available Identity UI pages will be created in your project.</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="0a3bf-129">Ejecute el scaffolding de identidad:</span><span class="sxs-lookup"><span data-stu-id="0a3bf-129">Run the Identity scaffolder:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0a3bf-130">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0a3bf-130">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0a3bf-131">En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto > **Agregar** > **nuevo elemento con scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="0a3bf-131">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="0a3bf-132">En el panel izquierdo del cuadro de diálogo **Agregar scaffold** , seleccione **Identity** > **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="0a3bf-132">From the left pane of the **Add Scaffold** dialog, select **Identity** > **Add**.</span></span>
* <span data-ttu-id="0a3bf-133">En el cuadro de diálogo **Agregar identidad** , seleccione las opciones que desee.</span><span class="sxs-lookup"><span data-stu-id="0a3bf-133">In the **Add Identity** dialog, select the options you want.</span></span>
  * <span data-ttu-id="0a3bf-134">Seleccione la página de diseño existente o el archivo de diseño se sobrescribirá con un marcado incorrecto.</span><span class="sxs-lookup"><span data-stu-id="0a3bf-134">Select your existing layout page, or your layout file will be overwritten with incorrect markup.</span></span> <span data-ttu-id="0a3bf-135">Cuando se selecciona un archivo *\_layout. cshtml* existente, **no** se sobrescribe.</span><span class="sxs-lookup"><span data-stu-id="0a3bf-135">When an existing *\_Layout.cshtml* file is selected, it is **not** overwritten.</span></span>

 <span data-ttu-id="0a3bf-136">Por ejemplo: `~/Pages/Shared/_Layout.cshtml` para los proyectos de Razor Pages `~/Views/Shared/_Layout.cshtml` para MVC</span><span class="sxs-lookup"><span data-stu-id="0a3bf-136">For example: `~/Pages/Shared/_Layout.cshtml` for Razor Pages `~/Views/Shared/_Layout.cshtml` for MVC projects</span></span>
* <span data-ttu-id="0a3bf-137">Para usar el contexto de datos existente, seleccione al menos un archivo para invalidar.</span><span class="sxs-lookup"><span data-stu-id="0a3bf-137">To use your existing data context, select at least one file to override.</span></span> <span data-ttu-id="0a3bf-138">Debe seleccionar al menos un archivo para agregar el contexto de datos.</span><span class="sxs-lookup"><span data-stu-id="0a3bf-138">You must select at least one file to add your data context.</span></span>
  * <span data-ttu-id="0a3bf-139">Seleccione la clase de contexto de datos.</span><span class="sxs-lookup"><span data-stu-id="0a3bf-139">Select your data context class.</span></span>
  * <span data-ttu-id="0a3bf-140">Seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="0a3bf-140">Select **Add**.</span></span>
* <span data-ttu-id="0a3bf-141">Para crear un nuevo contexto de usuario y, posiblemente, crear una clase de usuario personalizada para la identidad:</span><span class="sxs-lookup"><span data-stu-id="0a3bf-141">To create a new user context and possibly create a custom user class for Identity:</span></span>
  * <span data-ttu-id="0a3bf-142">Seleccione el **+** botón para crear un nuevo **clase de contexto de datos**.</span><span class="sxs-lookup"><span data-stu-id="0a3bf-142">Select the **+** button to create a new **Data context class**.</span></span>
  * <span data-ttu-id="0a3bf-143">Seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="0a3bf-143">Select **Add**.</span></span>

<span data-ttu-id="0a3bf-144">Nota: Si va a crear un nuevo contexto de usuario, no tiene que seleccionar un archivo para invalidar.</span><span class="sxs-lookup"><span data-stu-id="0a3bf-144">Note: If you're creating a new user context, you don't have to select a file to override.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="0a3bf-145">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="0a3bf-145">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="0a3bf-146">Si no ha instalado previamente el proveedor de scaffolding de ASP.NET Core, instalar ahora:</span><span class="sxs-lookup"><span data-stu-id="0a3bf-146">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="0a3bf-147">Agregue una referencia de paquete a [Microsoft. VisualStudio. Web. CodeGeneration. Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) al archivo de proyecto (\*. csproj).</span><span class="sxs-lookup"><span data-stu-id="0a3bf-147">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (\*.csproj) file.</span></span> <span data-ttu-id="0a3bf-148">Ejecute el siguiente comando en el directorio del proyecto:</span><span class="sxs-lookup"><span data-stu-id="0a3bf-148">Run the following command in the project directory:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="0a3bf-149">Ejecute el siguiente comando para enumerar las opciones de proveedor de scaffolding de identidad:</span><span class="sxs-lookup"><span data-stu-id="0a3bf-149">Run the following command to list the Identity scaffolder options:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="0a3bf-150">En la carpeta del proyecto, ejecute el scaffolding de identidad con las opciones que desee.</span><span class="sxs-lookup"><span data-stu-id="0a3bf-150">In the project folder, run the Identity scaffolder with the options you want.</span></span> <span data-ttu-id="0a3bf-151">Por ejemplo, para configurar la identidad con la interfaz de usuario predeterminada y el número mínimo de archivos, ejecute el siguiente comando.</span><span class="sxs-lookup"><span data-stu-id="0a3bf-151">For example, to setup identity with the default UI and the minimum number of files, run the following command.</span></span> <span data-ttu-id="0a3bf-152">Use el nombre completo correcto para el contexto de base de BD:</span><span class="sxs-lookup"><span data-stu-id="0a3bf-152">Use the correct fully qualified name for your DB context:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login"
```

<span data-ttu-id="0a3bf-153">PowerShell usa punto y coma como separador de comandos.</span><span class="sxs-lookup"><span data-stu-id="0a3bf-153">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="0a3bf-154">Al usar PowerShell, use el carácter de escape de punto y coma en la lista de archivos o ponga la lista de archivos entre comillas dobles.</span><span class="sxs-lookup"><span data-stu-id="0a3bf-154">When using PowerShell, escape the semi-colons in the file list or put the file list in double quotes.</span></span> <span data-ttu-id="0a3bf-155">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="0a3bf-155">For example:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="0a3bf-156">Si ejecuta el scaffolding de identidad sin especificar la marca `--files` o la marca `--useDefaultUI`, se crearán todas las páginas de la interfaz de usuario de identidad disponibles en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="0a3bf-156">If you run the Identity scaffolder without specifying the `--files` flag or the `--useDefaultUI` flag, all the available Identity UI pages will be created in your project.</span></span>

---

::: moniker-end