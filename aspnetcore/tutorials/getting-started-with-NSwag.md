---
title: Introducción a NSwag y ASP.NET Core
author: zuckerthoben
description: Obtenga información sobre cómo usar NSwag para generar documentación y páginas de ayuda de una ASP.NET Web API.
ms.author: scaddie
ms.custom: mvc
ms.date: 06/29/2018
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: c0811593609b7d1e3529d5253e8b053f180281f3
ms.sourcegitcommit: 2941e24d7f3fd3d5e88d27e5f852aaedd564deda
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/29/2018
ms.locfileid: "37126279"
---
# <a name="get-started-with-nswag-and-aspnet-core"></a><span data-ttu-id="51694-103">Introducción a NSwag y ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="51694-103">Get started with NSwag and ASP.NET Core</span></span>

<span data-ttu-id="51694-104">Por [Christoph Nienaber](https://twitter.com/zuckerthoben) y [Rico Suter](https://rsuter.com)</span><span class="sxs-lookup"><span data-stu-id="51694-104">By [Christoph Nienaber](https://twitter.com/zuckerthoben) and [Rico Suter](https://rsuter.com)</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="51694-105">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="51694-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="51694-106">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="51694-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker-end

<span data-ttu-id="51694-107">Para usar [NSwag](https://github.com/RSuter/NSwag) con middleware de ASP.NET Core, se necesita el paquete NuGet [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/).</span><span class="sxs-lookup"><span data-stu-id="51694-107">Using [NSwag](https://github.com/RSuter/NSwag) with ASP.NET Core middleware requires the [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet package.</span></span> <span data-ttu-id="51694-108">Este paquete consta de un generador de Swagger, la interfaz de usuario de Swagger (versiones 2 y 3) y la [interfaz de usuario de ReDoc](https://github.com/Rebilly/ReDoc).</span><span class="sxs-lookup"><span data-stu-id="51694-108">The package consists of a Swagger generator, Swagger UI (v2 and v3), and [ReDoc UI](https://github.com/Rebilly/ReDoc).</span></span>

<span data-ttu-id="51694-109">Se recomienda encarecidamente usar las capacidades de generación de código de NSwag.</span><span class="sxs-lookup"><span data-stu-id="51694-109">It's highly recommended to make use of NSwag's code generation capabilities.</span></span> <span data-ttu-id="51694-110">Elija una de las siguientes opciones para generar código:</span><span class="sxs-lookup"><span data-stu-id="51694-110">Choose one of the following options for code generation:</span></span>

* <span data-ttu-id="51694-111">Usar [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), una aplicación de escritorio de Windows para generar código de cliente en C# y TypeScript para la API.</span><span class="sxs-lookup"><span data-stu-id="51694-111">Use [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), a Windows desktop app for generating client code in C# and TypeScript for your API.</span></span>
* <span data-ttu-id="51694-112">Usar los paquetes NuGet [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) o [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) para generar código dentro del proyecto.</span><span class="sxs-lookup"><span data-stu-id="51694-112">Use the [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) or [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) NuGet packages to do code generation inside your project.</span></span>
* <span data-ttu-id="51694-113">Usar NSwag desde la [línea de comandos](https://github.com/NSwag/NSwag/wiki/CommandLine).</span><span class="sxs-lookup"><span data-stu-id="51694-113">Use NSwag from the [command line](https://github.com/NSwag/NSwag/wiki/CommandLine).</span></span>
* <span data-ttu-id="51694-114">Usar el paquete NuGet [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild).</span><span class="sxs-lookup"><span data-stu-id="51694-114">Use the [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) NuGet package.</span></span>

## <a name="features"></a><span data-ttu-id="51694-115">Características</span><span class="sxs-lookup"><span data-stu-id="51694-115">Features</span></span>

<span data-ttu-id="51694-116">La razón principal para usar NSwag es la posibilidad no ya de incorporar la interfaz de usuario de Swagger y el generador de Swagger, sino también de usar sus capacidades de generación de código flexibles.</span><span class="sxs-lookup"><span data-stu-id="51694-116">The main reason to use NSwag is the ability to not only introduce the Swagger UI and Swagger generator, but to make use of the flexible code generation capabilities.</span></span> <span data-ttu-id="51694-117">No es necesario que exista una API: se pueden usar API de terceros que incluyan Swagger y que permitan que NSwag genere una implementación de cliente.</span><span class="sxs-lookup"><span data-stu-id="51694-117">You don't need an existing API&mdash;you can use third-party APIs that incorporate Swagger and let NSwag generate a client implementation.</span></span> <span data-ttu-id="51694-118">En cualquier caso, el ciclo de desarrollo se agiliza y es más fácil adaptarse a los cambios de API.</span><span class="sxs-lookup"><span data-stu-id="51694-118">Either way, the development cycle is expedited and you can more easily adapt to API changes.</span></span>

## <a name="package-installation"></a><span data-ttu-id="51694-119">Instalación del paquete</span><span class="sxs-lookup"><span data-stu-id="51694-119">Package installation</span></span>

<span data-ttu-id="51694-120">El paquete NuGet NSwag se puede agregar con uno de los siguientes métodos:</span><span class="sxs-lookup"><span data-stu-id="51694-120">The NSwag NuGet package can be added with the following approaches:</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="51694-121">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="51694-121">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="51694-122">En la ventana **Consola del Administrador de paquetes**:</span><span class="sxs-lookup"><span data-stu-id="51694-122">From the **Package Manager Console** window:</span></span>
  * <span data-ttu-id="51694-123">Vaya a **Vista** > **Otras ventanas** > **Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="51694-123">Go to **View** > **Other Windows** > **Package Manager Console**</span></span>
  * <span data-ttu-id="51694-124">Vaya al directorio en el que está *TodoApi.csproj*.</span><span class="sxs-lookup"><span data-stu-id="51694-124">Navigate to the directory in which the *TodoApi.csproj* file exists</span></span>
  * <span data-ttu-id="51694-125">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="51694-125">Execute the following command:</span></span>

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* <span data-ttu-id="51694-126">En el cuadro de diálogo **Administrar paquetes NuGet**:</span><span class="sxs-lookup"><span data-stu-id="51694-126">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="51694-127">Haga clic con el botón derecho en el proyecto en el **Explorador de soluciones** > **Administrar paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="51694-127">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="51694-128">Establezca el **origen del paquete** en "nuget.org".</span><span class="sxs-lookup"><span data-stu-id="51694-128">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="51694-129">Escriba "NSwag.AspNetCore" en el cuadro de búsqueda.</span><span class="sxs-lookup"><span data-stu-id="51694-129">Enter "NSwag.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="51694-130">Seleccione el paquete "NSwag.AspNetCore" en la pestaña **Examinar** y haga clic en **Instalar**.</span><span class="sxs-lookup"><span data-stu-id="51694-130">Select the "NSwag.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="51694-131">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="51694-131">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="51694-132">Haga clic con el botón derecho en la carpeta *Paquetes* en **Panel de solución** > **Agregar paquetes...**</span><span class="sxs-lookup"><span data-stu-id="51694-132">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="51694-133">Establezca el menú desplegable **Origen** de la ventana **Agregar paquetes** en "nuget.org".</span><span class="sxs-lookup"><span data-stu-id="51694-133">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="51694-134">Escriba "NSwag.AspNetCore" en el cuadro de búsqueda.</span><span class="sxs-lookup"><span data-stu-id="51694-134">Enter "NSwag.AspNetCore" in the search box</span></span>
* <span data-ttu-id="51694-135">Seleccione el paquete "NSwag.AspNetCore" en el panel de resultados y haga clic en **Agregar paquete**.</span><span class="sxs-lookup"><span data-stu-id="51694-135">Select the "NSwag.AspNetCore" package from the results pane and click **Add Package**</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="51694-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="51694-136">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="51694-137">Ejecute el siguiente comando en el **terminal integrado**:</span><span class="sxs-lookup"><span data-stu-id="51694-137">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

### <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="51694-138">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="51694-138">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="51694-139">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="51694-139">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="51694-140">Agregar y configurar el middleware de Swagger</span><span class="sxs-lookup"><span data-stu-id="51694-140">Add and configure Swagger middleware</span></span>

<span data-ttu-id="51694-141">Importe los siguientes espacios de nombres a la clase `Startup`:</span><span class="sxs-lookup"><span data-stu-id="51694-141">Import the following namespaces in the `Startup` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_StartupConfigureImports)]

<span data-ttu-id="51694-142">En el método `Startup.Configure`, habilite el middleware para servir la especificación y la interfaz de usuario de Swagger:</span><span class="sxs-lookup"><span data-stu-id="51694-142">In the `Startup.Configure` method, enable the middleware for serving the generated Swagger specification and the Swagger UI:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=6-10)]

<span data-ttu-id="51694-143">Inicie la aplicación.</span><span class="sxs-lookup"><span data-stu-id="51694-143">Launch the app.</span></span> <span data-ttu-id="51694-144">Vaya a `http://localhost:<port>/swagger` para ver la interfaz de usuario de Swagger.</span><span class="sxs-lookup"><span data-stu-id="51694-144">Navigate to `http://localhost:<port>/swagger` to view the Swagger UI.</span></span> <span data-ttu-id="51694-145">Vaya a `http://localhost:<port>/swagger/v1/swagger.json` para ver la especificación de Swagger.</span><span class="sxs-lookup"><span data-stu-id="51694-145">Navigate to `http://localhost:<port>/swagger/v1/swagger.json` to view the Swagger specification.</span></span>

## <a name="code-generation"></a><span data-ttu-id="51694-146">Generación de código</span><span class="sxs-lookup"><span data-stu-id="51694-146">Code generation</span></span>

### <a name="via-nswagstudio"></a><span data-ttu-id="51694-147">Con NSwagStudio</span><span class="sxs-lookup"><span data-stu-id="51694-147">Via NSwagStudio</span></span>

* <span data-ttu-id="51694-148">Instale NSwagStudio desde el [repositorio de GitHub](https://github.com/RSuter/NSwag/wiki/NSwagStudio) oficial.</span><span class="sxs-lookup"><span data-stu-id="51694-148">Install NSwagStudio from the official [GitHub repository](https://github.com/RSuter/NSwag/wiki/NSwagStudio).</span></span>
* <span data-ttu-id="51694-149">Inicie NSwagStudio.</span><span class="sxs-lookup"><span data-stu-id="51694-149">Launch NSwagStudio.</span></span> <span data-ttu-id="51694-150">Indique la dirección URL del archivo *swagger.json* en el cuadro de texto **Swagger Specification URL** (Dirección URL de la especificación Swagger) y haga clic en el botón **Create local Copy** (Crear copia local).</span><span class="sxs-lookup"><span data-stu-id="51694-150">Enter the *swagger.json* file URL in the **Swagger Specification URL** textbox, and click the **Create local Copy** button.</span></span>
* <span data-ttu-id="51694-151">Seleccione el tipo de salida de cliente **CSharp**.</span><span class="sxs-lookup"><span data-stu-id="51694-151">Select the **CSharp Client** client output type.</span></span> <span data-ttu-id="51694-152">Las otras opciones son **TypeScript Client** (Cliente de TypeScript) y **CSharp Web API Controller** (Controlador Web API de CSharp).</span><span class="sxs-lookup"><span data-stu-id="51694-152">Other options include **TypeScript Client** and **CSharp Web API Controller**.</span></span> <span data-ttu-id="51694-153">Si se usa un controlador Web API, consistirá básicamente en una generación inversa.</span><span class="sxs-lookup"><span data-stu-id="51694-153">Using a Web API Controller is basically a reverse generation.</span></span> <span data-ttu-id="51694-154">Se usa una especificación de un servicio para recompilar dicho servicio.</span><span class="sxs-lookup"><span data-stu-id="51694-154">It uses a specification of a service to rebuild the service.</span></span>
* <span data-ttu-id="51694-155">Haga clic en el botón **Generate Outputs** (Generar salidas).</span><span class="sxs-lookup"><span data-stu-id="51694-155">Click the **Generate Outputs** button.</span></span> <span data-ttu-id="51694-156">Se creará una implementación de cliente de C# completa del proyecto *TodoApi.NSwag*.</span><span class="sxs-lookup"><span data-stu-id="51694-156">A complete C# client implementation of the *TodoApi.NSwag* project is produced.</span></span> <span data-ttu-id="51694-157">Haga clic en la pestaña **CSharp Client** (Cliente de CSharp) en la sección **Outputs** (Salidas) para ver el código de cliente generado:</span><span class="sxs-lookup"><span data-stu-id="51694-157">Click the **CSharp Client** tab of the **Outputs** section to see the generated client code:</span></span>

```csharp
//----------------------
// <auto-generated>
//     Generated using the NSwag toolchain v11.17.3.0 (NJsonSchema v9.10.46.0 (Newtonsoft.Json v9.0.0.0)) (http://NSwag.org)
// </auto-generated>
//----------------------

namespace MyNamespace
{
    #pragma warning disable // Disable all warnings

    [System.CodeDom.Compiler.GeneratedCode("NSwag",
        "11.17.3.0 (NJsonSchema v9.10.46.0 (Newtonsoft.Json v9.0.0.0))")]
    public partial class TodoClient
    {
        private string _baseUrl = "http://localhost:50499";
        private System.Lazy<Newtonsoft.Json.JsonSerializerSettings> _settings;

        public TodoClient()
        {
            _settings = new System.Lazy
                <Newtonsoft.Json.JsonSerializerSettings>(() =>
            {
                var settings = new Newtonsoft.Json.JsonSerializerSettings();
                UpdateJsonSerializerSettings(settings);
                return settings;
            });
        }

        public string BaseUrl
        {
            get { return _baseUrl; }
            set { _baseUrl = value; }
        }

        // code omitted for brevity
```

> [!TIP]
> <span data-ttu-id="51694-158">El código de cliente de C# se genera según la configuración que haya definida en la pestaña **Settings** (Configuración) de la pestaña **CSharp Client** (Cliente de CSharp). Modifique esta configuración para realizar tareas como cambiar el nombre del espacio de nombres predeterminado y generar un método sincrónico.</span><span class="sxs-lookup"><span data-stu-id="51694-158">The C# client code is generated based on settings defined in the **Settings** tab of the **CSharp Client** tab. Modify the settings to perform tasks such as default namespace renaming and synchronous method generation.</span></span>

* <span data-ttu-id="51694-159">Coloque el código de C# generado en un archivo del proyecto de cliente (por ejemplo, una aplicación [Xamarin.Forms](/xamarin/xamarin-forms/)).</span><span class="sxs-lookup"><span data-stu-id="51694-159">Copy the generated C# code into a file in a client project (for example, a [Xamarin.Forms](/xamarin/xamarin-forms/) app).</span></span>
* <span data-ttu-id="51694-160">Empiece a consumir la Web API:</span><span class="sxs-lookup"><span data-stu-id="51694-160">Start consuming the web API:</span></span>

```csharp
var todoClient = new TodoClient();

// Gets all to-dos from the API
var allTodos = await todoClient.GetAllAsync();

// Create a new TodoItem, and save it in the API
var createdTodo = await todoClient.CreateAsync(new TodoItem());

// Get a single to-do by ID
var foundTodo = await todoClient.GetByIdAsync(1);
```

> [!NOTE]
> <span data-ttu-id="51694-161">Puede insertar una dirección URL base o un cliente HTTP en el cliente de API.</span><span class="sxs-lookup"><span data-stu-id="51694-161">You can inject a base URL and/or a HTTP client into the API client.</span></span> <span data-ttu-id="51694-162">El procedimiento recomendado para ello es siempre [reutilizar HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).</span><span class="sxs-lookup"><span data-stu-id="51694-162">The best practice is to always [reuse the HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).</span></span>

### <a name="other-ways-to-generate-client-code"></a><span data-ttu-id="51694-163">Otras formas de generar código de cliente</span><span class="sxs-lookup"><span data-stu-id="51694-163">Other ways to generate client code</span></span>

<span data-ttu-id="51694-164">Se puede generar código de cliente de otras maneras más acordes con el flujo de trabajo:</span><span class="sxs-lookup"><span data-stu-id="51694-164">You can generate the client code in other ways, more suited to your workflow:</span></span>

* [<span data-ttu-id="51694-165">MSBuild</span><span class="sxs-lookup"><span data-stu-id="51694-165">MSBuild</span></span>](https://www.nuget.org/packages/NSwag.MSBuild/)

* [<span data-ttu-id="51694-166">En código</span><span class="sxs-lookup"><span data-stu-id="51694-166">In code</span></span>](https://github.com/NSwag/NSwag/wiki/SwaggerToCSharpClientGenerator)

* [<span data-ttu-id="51694-167">Plantillas T4</span><span class="sxs-lookup"><span data-stu-id="51694-167">T4 templates</span></span>](https://github.com/NSwag/NSwag/wiki/T4)

## <a name="customize"></a><span data-ttu-id="51694-168">Personalizar</span><span class="sxs-lookup"><span data-stu-id="51694-168">Customize</span></span>

<span data-ttu-id="51694-169">Swagger proporciona opciones para documentar el modelo de objetos de cara a facilitar el consumo de Web API.</span><span class="sxs-lookup"><span data-stu-id="51694-169">Swagger provides options for documenting the object model to ease consumption of the web API.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="51694-170">Información y descripción de la API</span><span class="sxs-lookup"><span data-stu-id="51694-170">API info and description</span></span>

<span data-ttu-id="51694-171">En el método `Startup.Configure`, la acción de configuración que se pasa al método `UseSwagger` agrega información, como el autor, la licencia y la descripción:</span><span class="sxs-lookup"><span data-stu-id="51694-171">In the `Startup.Configure` method, a configuration action passed to the `UseSwagger` method adds information such as the author, license, and description:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup2.cs?name=snippet_UseSwagger)]

<span data-ttu-id="51694-172">La interfaz de usuario de Swagger muestra información de la versión:</span><span class="sxs-lookup"><span data-stu-id="51694-172">The Swagger UI displays the version's information:</span></span>

![Interfaz de usuario de Swagger con información de la versión](web-api-help-pages-using-swagger/_static/custom-info-nswag.png)

### <a name="xml-comments"></a><span data-ttu-id="51694-174">comentarios XML</span><span class="sxs-lookup"><span data-stu-id="51694-174">XML comments</span></span>

<span data-ttu-id="51694-175">Los comentarios XML se habilitan con los siguientes métodos:</span><span class="sxs-lookup"><span data-stu-id="51694-175">XML comments are enabled with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio-xml"></a>[<span data-ttu-id="51694-176">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="51694-176">Visual Studio</span></span>](#tab/visual-studio-xml/)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="51694-177">Haga clic con el botón derecho en el **Explorador de soluciones** y seleccione **Editar <nombre_de_proyecto>.csproj**.</span><span class="sxs-lookup"><span data-stu-id="51694-177">Right-click the project in **Solution Explorer** and select **Edit <project_name>.csproj**.</span></span>
* <span data-ttu-id="51694-178">Agregue manualmente las líneas resaltadas al archivo *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="51694-178">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="51694-179">En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Propiedades**.</span><span class="sxs-lookup"><span data-stu-id="51694-179">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="51694-180">Active la casilla **Archivo de documentación XML** en la sección **Salida** de la pestaña **Compilar**.</span><span class="sxs-lookup"><span data-stu-id="51694-180">Check the **XML documentation file** box under the **Output** section of the **Build** tab</span></span>

::: moniker-end

# <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[<span data-ttu-id="51694-181">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="51694-181">Visual Studio for Mac</span></span>](#tab/visual-studio-mac-xml/)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="51694-182">Desde el *Panel de solución*, presione **Control** y haga clic en el nombre del proyecto.</span><span class="sxs-lookup"><span data-stu-id="51694-182">From the *Solution Pad*, press **control** and click the project name.</span></span> <span data-ttu-id="51694-183">Vaya a **Herramientas** > **Editar archivo**.</span><span class="sxs-lookup"><span data-stu-id="51694-183">Navigate to **Tools** > **Edit File**.</span></span>
* <span data-ttu-id="51694-184">Agregue manualmente las líneas resaltadas al archivo *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="51694-184">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="51694-185">Abra el cuadro de diálogo **Opciones del proyecto** > **Compilar** > **Compilador**.</span><span class="sxs-lookup"><span data-stu-id="51694-185">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="51694-186">Active la casilla **Generar documentación XML** en la sección **Opciones generales**.</span><span class="sxs-lookup"><span data-stu-id="51694-186">Check the **Generate xml documentation** box under the **General Options** section</span></span>

::: moniker-end

# <a name="visual-studio-codetabvisual-studio-code-xml"></a>[<span data-ttu-id="51694-187">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="51694-187">Visual Studio Code</span></span>](#tab/visual-studio-code-xml/)

<span data-ttu-id="51694-188">Agregue manualmente las líneas resaltadas al archivo *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="51694-188">Manually add the highlighted lines to the *.csproj* file:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

---

### <a name="data-annotations"></a><span data-ttu-id="51694-189">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="51694-189">Data annotations</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="51694-190">NSwag usa [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), y el tipo de valor devuelto recomendado para las acciones de Web API es [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult).</span><span class="sxs-lookup"><span data-stu-id="51694-190">NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the recommended return type for web API actions is [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult).</span></span> <span data-ttu-id="51694-191">Por tanto, NSwag no puede deducir lo que la acción realiza y lo que devuelve.</span><span class="sxs-lookup"><span data-stu-id="51694-191">Consequently, NSwag can't infer what your action is doing and what it returns.</span></span> <span data-ttu-id="51694-192">Considere el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="51694-192">Consider the following example:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

<span data-ttu-id="51694-193">La acción anterior devuelve `IActionResult` pero, dentro de la acción, devuelve [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) o [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest).</span><span class="sxs-lookup"><span data-stu-id="51694-193">The preceding action returns `IActionResult`, but inside the action it's returning either [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) or [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest).</span></span> <span data-ttu-id="51694-194">Las anotaciones de datos sirven para indicar a los clientes qué códigos de estado HTTP se sabe que esta acción devuelve.</span><span class="sxs-lookup"><span data-stu-id="51694-194">Data annotations are used to tell clients which HTTP status codes this action is known to return.</span></span> <span data-ttu-id="51694-195">Complete la acción con los siguientes atributos:</span><span class="sxs-lookup"><span data-stu-id="51694-195">Decorate the action with the following attributes:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="51694-196">NSwag usa [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), y el tipo de valor devuelto recomendado para las acciones de Web API es [ActionResult\<](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1).</span><span class="sxs-lookup"><span data-stu-id="51694-196">NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the recommended return type for web API actions is [ActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1).</span></span> <span data-ttu-id="51694-197">Por lo tanto, NSwag puede inferir únicamente el tipo de valor devuelto definido por `T`.</span><span class="sxs-lookup"><span data-stu-id="51694-197">Consequently, NSwag can only infer the return type defined by `T`.</span></span> <span data-ttu-id="51694-198">No podrá inferir otros tipos de valor devuelto que son factibles en la acción.</span><span class="sxs-lookup"><span data-stu-id="51694-198">Other possible return types in the action cannot be inferred.</span></span> <span data-ttu-id="51694-199">Considere el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="51694-199">Consider the following example:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

<span data-ttu-id="51694-200">La acción anterior devuelve `ActionResult<T>` pero, dentro de la acción, devuelve [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute).</span><span class="sxs-lookup"><span data-stu-id="51694-200">The preceding action returns `ActionResult<T>`, but inside the action it's returning either [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute).</span></span> <span data-ttu-id="51694-201">Como el controlador se completa con el atributo [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute), también es posible una respuesta [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest).</span><span class="sxs-lookup"><span data-stu-id="51694-201">Since the controller is decorated with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute, a [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest) response is possible too.</span></span> <span data-ttu-id="51694-202">Vea [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses) (Respuestas HTTP 400 automáticas) para más información.</span><span class="sxs-lookup"><span data-stu-id="51694-202">See [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses) for more info.</span></span> <span data-ttu-id="51694-203">Las anotaciones de datos sirven para indicar a los clientes qué códigos de estado HTTP se sabe que esta acción devuelve.</span><span class="sxs-lookup"><span data-stu-id="51694-203">Data annotations are used to tell clients which HTTP status codes this action is known to return.</span></span> <span data-ttu-id="51694-204">Complete la acción con los siguientes atributos:</span><span class="sxs-lookup"><span data-stu-id="51694-204">Decorate the action with the following attributes:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]
::: moniker-end

<span data-ttu-id="51694-205">Ahora, el generador de Swagger puede describir con precisión esta acción y los clientes generados sabrán lo que reciben cuando llamen al punto de conexión.</span><span class="sxs-lookup"><span data-stu-id="51694-205">The Swagger generator can now accurately describe this action, and generated clients know what they receive when calling the endpoint.</span></span> <span data-ttu-id="51694-206">Completar todas las acciones con estos atributos es tremendamente recomendable.</span><span class="sxs-lookup"><span data-stu-id="51694-206">Decorating all actions with these attributes is highly recommended.</span></span> <span data-ttu-id="51694-207">Para obtener instrucciones sobre qué respuestas HTTP deben devolver las acciones de API, vea la [especificación RFC 7231](https://tools.ietf.org/html/rfc7231#section-4.3).</span><span class="sxs-lookup"><span data-stu-id="51694-207">For guidelines on what HTTP responses your API actions should return, see the [RFC 7231 specification](https://tools.ietf.org/html/rfc7231#section-4.3).</span></span>
