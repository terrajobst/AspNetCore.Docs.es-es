---
title: Introducción a NSwag y ASP.NET Core
author: zuckerthoben
description: Obtenga información sobre cómo usar NSwag para generar documentación y páginas de ayuda de una ASP.NET Web API.
ms.author: scaddie
ms.custom: mvc
ms.date: 12/18/2018
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: 8af5bed1e042c4f6d83043b05084c51b3064a548
ms.sourcegitcommit: ea215df889e89db44037a6ac2f01baede0450da9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/19/2018
ms.locfileid: "53595365"
---
# <a name="get-started-with-nswag-and-aspnet-core"></a><span data-ttu-id="06ed3-103">Introducción a NSwag y ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="06ed3-103">Get started with NSwag and ASP.NET Core</span></span>

<span data-ttu-id="06ed3-104">Por [Christoph Nienaber](https://twitter.com/zuckerthoben) y [Rico Suter](https://rsuter.com)</span><span class="sxs-lookup"><span data-stu-id="06ed3-104">By [Christoph Nienaber](https://twitter.com/zuckerthoben) and [Rico Suter](https://rsuter.com)</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="06ed3-105">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="06ed3-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="06ed3-106">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="06ed3-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker-end

<span data-ttu-id="06ed3-107">Registre el middleware de NSwag para:</span><span class="sxs-lookup"><span data-stu-id="06ed3-107">Register the NSwag middlewares to:</span></span>

* <span data-ttu-id="06ed3-108">Generar la especificación de Swagger para la API web implementada.</span><span class="sxs-lookup"><span data-stu-id="06ed3-108">Generate the Swagger specification for the implemented web API.</span></span>
* <span data-ttu-id="06ed3-109">Proporcionar la interfaz de usuario de Swagger para examinar y probar la API web.</span><span class="sxs-lookup"><span data-stu-id="06ed3-109">Serve the Swagger UI to browse and test the web API.</span></span>

<span data-ttu-id="06ed3-110">Para usar [NSwag](https://github.com/RSuter/NSwag) con middleware de ASP.NET Core, instale el paquete NuGet [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/).</span><span class="sxs-lookup"><span data-stu-id="06ed3-110">To use the [NSwag](https://github.com/RSuter/NSwag) ASP.NET Core middlewares, install the [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet package.</span></span> <span data-ttu-id="06ed3-111">Este paquete contiene el middleware para generar y proporcionar la especificación de Swagger, la interfaz de usuario de Swagger (v2 y v3) y la [interfaz de usuario de ReDoc](https://github.com/Rebilly/ReDoc).</span><span class="sxs-lookup"><span data-stu-id="06ed3-111">This package contains the middlewares to generate and serve the Swagger specification, Swagger UI (v2 and v3), and [ReDoc UI](https://github.com/Rebilly/ReDoc).</span></span>

<span data-ttu-id="06ed3-112">Además, se recomienda encarecidamente usar las funcionalidades de generación de código de NSwag.</span><span class="sxs-lookup"><span data-stu-id="06ed3-112">Additionally, it's highly recommended to make use of NSwag's code generation capabilities.</span></span> <span data-ttu-id="06ed3-113">Elija una de las siguientes opciones para usar las funcionalidades de generación de código:</span><span class="sxs-lookup"><span data-stu-id="06ed3-113">Choose one of the following options to use the code generation capabilities:</span></span>

* <span data-ttu-id="06ed3-114">Usar [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), una aplicación de escritorio de Windows para generar código de cliente en C# y TypeScript para la API.</span><span class="sxs-lookup"><span data-stu-id="06ed3-114">Use [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), a Windows desktop app for generating client code in C# and TypeScript for your API.</span></span>
* <span data-ttu-id="06ed3-115">Usar los paquetes NuGet [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) o [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) para generar código dentro del proyecto.</span><span class="sxs-lookup"><span data-stu-id="06ed3-115">Use the [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) or [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) NuGet packages to do code generation inside your project.</span></span>
* <span data-ttu-id="06ed3-116">Usar NSwag desde la [línea de comandos](https://github.com/NSwag/NSwag/wiki/CommandLine).</span><span class="sxs-lookup"><span data-stu-id="06ed3-116">Use NSwag from the [command line](https://github.com/NSwag/NSwag/wiki/CommandLine).</span></span>
* <span data-ttu-id="06ed3-117">Usar el paquete NuGet [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild).</span><span class="sxs-lookup"><span data-stu-id="06ed3-117">Use the [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) NuGet package.</span></span>

## <a name="features"></a><span data-ttu-id="06ed3-118">Características</span><span class="sxs-lookup"><span data-stu-id="06ed3-118">Features</span></span>

<span data-ttu-id="06ed3-119">La razón principal para usar NSwag es la posibilidad no ya de incorporar la interfaz de usuario de Swagger y el generador de Swagger, sino también de usar sus funcionalidades de generación de código flexibles.</span><span class="sxs-lookup"><span data-stu-id="06ed3-119">The main reason to use NSwag is the ability to not only introduce the Swagger UI and Swagger generator, but to also make use of the flexible code generation capabilities.</span></span> <span data-ttu-id="06ed3-120">No es necesario que exista una API: se pueden usar API de terceros que incluyan Swagger y que permitan que NSwag genere una implementación de cliente.</span><span class="sxs-lookup"><span data-stu-id="06ed3-120">You don't need an existing API&mdash;you can use third-party APIs that incorporate Swagger and let NSwag generate a client implementation.</span></span> <span data-ttu-id="06ed3-121">En cualquier caso, el ciclo de desarrollo se agiliza y es más fácil adaptarse a los cambios de API.</span><span class="sxs-lookup"><span data-stu-id="06ed3-121">Either way, the development cycle is expedited and you can more easily adapt to API changes.</span></span>

## <a name="package-installation"></a><span data-ttu-id="06ed3-122">Instalación del paquete</span><span class="sxs-lookup"><span data-stu-id="06ed3-122">Package installation</span></span>

<span data-ttu-id="06ed3-123">El paquete NuGet NSwag se puede agregar con uno de los siguientes métodos:</span><span class="sxs-lookup"><span data-stu-id="06ed3-123">The NSwag NuGet package can be added with the following approaches:</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="06ed3-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="06ed3-124">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="06ed3-125">En la ventana **Consola del Administrador de paquetes**:</span><span class="sxs-lookup"><span data-stu-id="06ed3-125">From the **Package Manager Console** window:</span></span>
  * <span data-ttu-id="06ed3-126">Vaya a **Vista** > **Otras ventanas** > **Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="06ed3-126">Go to **View** > **Other Windows** > **Package Manager Console**</span></span>
  * <span data-ttu-id="06ed3-127">Vaya al directorio en el que está *TodoApi.csproj*.</span><span class="sxs-lookup"><span data-stu-id="06ed3-127">Navigate to the directory in which the *TodoApi.csproj* file exists</span></span>
  * <span data-ttu-id="06ed3-128">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="06ed3-128">Execute the following command:</span></span>

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* <span data-ttu-id="06ed3-129">En el cuadro de diálogo **Administrar paquetes NuGet**:</span><span class="sxs-lookup"><span data-stu-id="06ed3-129">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="06ed3-130">Haga clic con el botón derecho en el proyecto en el **Explorador de soluciones** > **Administrar paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="06ed3-130">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="06ed3-131">Establezca el **origen del paquete** en "nuget.org".</span><span class="sxs-lookup"><span data-stu-id="06ed3-131">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="06ed3-132">Escriba "NSwag.AspNetCore" en el cuadro de búsqueda.</span><span class="sxs-lookup"><span data-stu-id="06ed3-132">Enter "NSwag.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="06ed3-133">Seleccione el paquete "NSwag.AspNetCore" en la pestaña **Examinar** y haga clic en **Instalar**.</span><span class="sxs-lookup"><span data-stu-id="06ed3-133">Select the "NSwag.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="06ed3-134">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="06ed3-134">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="06ed3-135">Haga clic con el botón derecho en la carpeta *Paquetes* en **Panel de solución** > **Agregar paquetes...**</span><span class="sxs-lookup"><span data-stu-id="06ed3-135">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="06ed3-136">Establezca el menú desplegable **Origen** de la ventana **Agregar paquetes** en "nuget.org".</span><span class="sxs-lookup"><span data-stu-id="06ed3-136">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="06ed3-137">Escriba "NSwag.AspNetCore" en el cuadro de búsqueda.</span><span class="sxs-lookup"><span data-stu-id="06ed3-137">Enter "NSwag.AspNetCore" in the search box</span></span>
* <span data-ttu-id="06ed3-138">Seleccione el paquete "NSwag.AspNetCore" en el panel de resultados y haga clic en **Agregar paquete**.</span><span class="sxs-lookup"><span data-stu-id="06ed3-138">Select the "NSwag.AspNetCore" package from the results pane and click **Add Package**</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="06ed3-139">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="06ed3-139">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="06ed3-140">Ejecute el siguiente comando en el **terminal integrado**:</span><span class="sxs-lookup"><span data-stu-id="06ed3-140">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

### <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="06ed3-141">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="06ed3-141">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="06ed3-142">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="06ed3-142">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="06ed3-143">Agregar y configurar el middleware de Swagger</span><span class="sxs-lookup"><span data-stu-id="06ed3-143">Add and configure Swagger middleware</span></span>

<span data-ttu-id="06ed3-144">Importe los siguientes espacios de nombres a la clase `Startup`:</span><span class="sxs-lookup"><span data-stu-id="06ed3-144">Import the following namespaces in the `Startup` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_StartupConfigureImports)]

<span data-ttu-id="06ed3-145">En el método `Startup.ConfigureServices`, registre los servicios necesarios de Swagger:</span><span class="sxs-lookup"><span data-stu-id="06ed3-145">In the `Startup.ConfigureServices` method, register the required Swagger services:</span></span> 

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_ConfigureServices&highlight=8)]

<span data-ttu-id="06ed3-146">En el método `Startup.Configure`, habilite el middleware para servir la especificación y la interfaz de usuario de Swagger v3 generadas:</span><span class="sxs-lookup"><span data-stu-id="06ed3-146">In the `Startup.Configure` method, enable the middleware for serving the generated Swagger specification and the Swagger UI v3:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=6-10)]

<span data-ttu-id="06ed3-147">Inicie la aplicación.</span><span class="sxs-lookup"><span data-stu-id="06ed3-147">Launch the app.</span></span> <span data-ttu-id="06ed3-148">Vaya a `http://localhost:<port>/swagger` para ver la interfaz de usuario de Swagger.</span><span class="sxs-lookup"><span data-stu-id="06ed3-148">Navigate to `http://localhost:<port>/swagger` to view the Swagger UI.</span></span> <span data-ttu-id="06ed3-149">Vaya a `http://localhost:<port>/swagger/v1/swagger.json` para ver la especificación de Swagger.</span><span class="sxs-lookup"><span data-stu-id="06ed3-149">Navigate to `http://localhost:<port>/swagger/v1/swagger.json` to view the Swagger specification.</span></span>

## <a name="code-generation"></a><span data-ttu-id="06ed3-150">Generación de código</span><span class="sxs-lookup"><span data-stu-id="06ed3-150">Code generation</span></span>

### <a name="via-nswagstudio"></a><span data-ttu-id="06ed3-151">Con NSwagStudio</span><span class="sxs-lookup"><span data-stu-id="06ed3-151">Via NSwagStudio</span></span>

* <span data-ttu-id="06ed3-152">Instale NSwagStudio desde el [repositorio de GitHub](https://github.com/RSuter/NSwag/wiki/NSwagStudio) oficial.</span><span class="sxs-lookup"><span data-stu-id="06ed3-152">Install NSwagStudio from the official [GitHub repository](https://github.com/RSuter/NSwag/wiki/NSwagStudio).</span></span>
* <span data-ttu-id="06ed3-153">Inicie NSwagStudio.</span><span class="sxs-lookup"><span data-stu-id="06ed3-153">Launch NSwagStudio.</span></span> <span data-ttu-id="06ed3-154">Indique la dirección URL del archivo *swagger.json* en el cuadro de texto **Swagger Specification URL** (Dirección URL de la especificación Swagger) y haga clic en el botón **Create local Copy** (Crear copia local).</span><span class="sxs-lookup"><span data-stu-id="06ed3-154">Enter the *swagger.json* file URL in the **Swagger Specification URL** textbox, and click the **Create local Copy** button.</span></span>
* <span data-ttu-id="06ed3-155">Seleccione el tipo de salida de cliente **CSharp**.</span><span class="sxs-lookup"><span data-stu-id="06ed3-155">Select the **CSharp Client** client output type.</span></span> <span data-ttu-id="06ed3-156">Las otras opciones son **TypeScript Client** (Cliente de TypeScript) y **CSharp Web API Controller** (Controlador Web API de CSharp).</span><span class="sxs-lookup"><span data-stu-id="06ed3-156">Other options include **TypeScript Client** and **CSharp Web API Controller**.</span></span> <span data-ttu-id="06ed3-157">Si se usa un controlador Web API, consistirá básicamente en una generación inversa.</span><span class="sxs-lookup"><span data-stu-id="06ed3-157">Using a Web API Controller is basically a reverse generation.</span></span> <span data-ttu-id="06ed3-158">Se usa una especificación de un servicio para recompilar dicho servicio.</span><span class="sxs-lookup"><span data-stu-id="06ed3-158">It uses a specification of a service to rebuild the service.</span></span>
* <span data-ttu-id="06ed3-159">Haga clic en el botón **Generate Outputs** (Generar salidas).</span><span class="sxs-lookup"><span data-stu-id="06ed3-159">Click the **Generate Outputs** button.</span></span> <span data-ttu-id="06ed3-160">Se creará una implementación de cliente de C# completa del proyecto *TodoApi.NSwag*.</span><span class="sxs-lookup"><span data-stu-id="06ed3-160">A complete C# client implementation of the *TodoApi.NSwag* project is produced.</span></span> <span data-ttu-id="06ed3-161">Haga clic en la pestaña **CSharp Client** (Cliente de CSharp) en la sección **Outputs** (Salidas) para ver el código de cliente generado:</span><span class="sxs-lookup"><span data-stu-id="06ed3-161">Click the **CSharp Client** tab of the **Outputs** section to see the generated client code:</span></span>

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
> <span data-ttu-id="06ed3-162">El código de cliente de C# se genera según la configuración que haya definida en la pestaña **Settings** (Configuración) de la pestaña **CSharp Client** (Cliente de CSharp). Modifique esta configuración para realizar tareas como cambiar el nombre del espacio de nombres predeterminado y generar un método sincrónico.</span><span class="sxs-lookup"><span data-stu-id="06ed3-162">The C# client code is generated based on settings defined in the **Settings** tab of the **CSharp Client** tab. Modify the settings to perform tasks such as default namespace renaming and synchronous method generation.</span></span>

* <span data-ttu-id="06ed3-163">Coloque el código de C# generado en un archivo del proyecto de cliente (por ejemplo, una aplicación [Xamarin.Forms](/xamarin/xamarin-forms/)).</span><span class="sxs-lookup"><span data-stu-id="06ed3-163">Copy the generated C# code into a file in a client project (for example, a [Xamarin.Forms](/xamarin/xamarin-forms/) app).</span></span>
* <span data-ttu-id="06ed3-164">Empiece a consumir la Web API:</span><span class="sxs-lookup"><span data-stu-id="06ed3-164">Start consuming the web API:</span></span>

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
> <span data-ttu-id="06ed3-165">Puede insertar una dirección URL base o un cliente HTTP en el cliente de API.</span><span class="sxs-lookup"><span data-stu-id="06ed3-165">You can inject a base URL and/or a HTTP client into the API client.</span></span> <span data-ttu-id="06ed3-166">El procedimiento recomendado para ello es siempre [reutilizar HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).</span><span class="sxs-lookup"><span data-stu-id="06ed3-166">The best practice is to always [reuse the HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).</span></span>

### <a name="other-ways-to-generate-client-code"></a><span data-ttu-id="06ed3-167">Otras formas de generar código de cliente</span><span class="sxs-lookup"><span data-stu-id="06ed3-167">Other ways to generate client code</span></span>

<span data-ttu-id="06ed3-168">Se puede generar código de cliente de otras maneras más acordes con el flujo de trabajo:</span><span class="sxs-lookup"><span data-stu-id="06ed3-168">You can generate the client code in other ways, more suited to your workflow:</span></span>

* [<span data-ttu-id="06ed3-169">MSBuild</span><span class="sxs-lookup"><span data-stu-id="06ed3-169">MSBuild</span></span>](https://www.nuget.org/packages/NSwag.MSBuild/)

* [<span data-ttu-id="06ed3-170">En código</span><span class="sxs-lookup"><span data-stu-id="06ed3-170">In code</span></span>](https://github.com/NSwag/NSwag/wiki/SwaggerToCSharpClientGenerator)

* [<span data-ttu-id="06ed3-171">Plantillas T4</span><span class="sxs-lookup"><span data-stu-id="06ed3-171">T4 templates</span></span>](https://github.com/NSwag/NSwag/wiki/T4)

## <a name="customize"></a><span data-ttu-id="06ed3-172">Personalizar</span><span class="sxs-lookup"><span data-stu-id="06ed3-172">Customize</span></span>

<span data-ttu-id="06ed3-173">Swagger proporciona opciones para documentar el modelo de objetos de cara a facilitar el consumo de Web API.</span><span class="sxs-lookup"><span data-stu-id="06ed3-173">Swagger provides options for documenting the object model to ease consumption of the web API.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="06ed3-174">Información y descripción de la API</span><span class="sxs-lookup"><span data-stu-id="06ed3-174">API info and description</span></span>

<span data-ttu-id="06ed3-175">En el método `Startup.Configure`, la acción de configuración que se pasa al método `UseSwagger` agrega información, como el autor, la licencia y la descripción:</span><span class="sxs-lookup"><span data-stu-id="06ed3-175">In the `Startup.Configure` method, a configuration action passed to the `UseSwagger` method adds information such as the author, license, and description:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup2.cs?name=snippet_UseSwagger)]

<span data-ttu-id="06ed3-176">La interfaz de usuario de Swagger muestra información de la versión:</span><span class="sxs-lookup"><span data-stu-id="06ed3-176">The Swagger UI displays the version's information:</span></span>

![Interfaz de usuario de Swagger con información de la versión](web-api-help-pages-using-swagger/_static/custom-info-nswag.png)

### <a name="xml-comments"></a><span data-ttu-id="06ed3-178">comentarios XML</span><span class="sxs-lookup"><span data-stu-id="06ed3-178">XML comments</span></span>

<span data-ttu-id="06ed3-179">Los comentarios XML se habilitan con los siguientes métodos:</span><span class="sxs-lookup"><span data-stu-id="06ed3-179">XML comments are enabled with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio-xml"></a>[<span data-ttu-id="06ed3-180">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="06ed3-180">Visual Studio</span></span>](#tab/visual-studio-xml/)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="06ed3-181">Haga clic con el botón derecho en el **Explorador de soluciones** y seleccione **Editar <nombre_de_proyecto>.csproj**.</span><span class="sxs-lookup"><span data-stu-id="06ed3-181">Right-click the project in **Solution Explorer** and select **Edit <project_name>.csproj**.</span></span>
* <span data-ttu-id="06ed3-182">Agregue manualmente las líneas resaltadas al archivo *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="06ed3-182">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="06ed3-183">En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Propiedades**.</span><span class="sxs-lookup"><span data-stu-id="06ed3-183">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="06ed3-184">Active la casilla **Archivo de documentación XML** en la sección **Salida** de la pestaña **Compilar**.</span><span class="sxs-lookup"><span data-stu-id="06ed3-184">Check the **XML documentation file** box under the **Output** section of the **Build** tab</span></span>

::: moniker-end

# <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[<span data-ttu-id="06ed3-185">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="06ed3-185">Visual Studio for Mac</span></span>](#tab/visual-studio-mac-xml/)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="06ed3-186">Desde el *Panel de solución*, presione **Control** y haga clic en el nombre del proyecto.</span><span class="sxs-lookup"><span data-stu-id="06ed3-186">From the *Solution Pad*, press **control** and click the project name.</span></span> <span data-ttu-id="06ed3-187">Vaya a **Herramientas** > **Editar archivo**.</span><span class="sxs-lookup"><span data-stu-id="06ed3-187">Navigate to **Tools** > **Edit File**.</span></span>
* <span data-ttu-id="06ed3-188">Agregue manualmente las líneas resaltadas al archivo *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="06ed3-188">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="06ed3-189">Abra el cuadro de diálogo **Opciones del proyecto** > **Compilar** > **Compilador**.</span><span class="sxs-lookup"><span data-stu-id="06ed3-189">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="06ed3-190">Active la casilla **Generar documentación XML** en la sección **Opciones generales**.</span><span class="sxs-lookup"><span data-stu-id="06ed3-190">Check the **Generate xml documentation** box under the **General Options** section</span></span>

::: moniker-end

# <a name="visual-studio-codetabvisual-studio-code-xml"></a>[<span data-ttu-id="06ed3-191">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="06ed3-191">Visual Studio Code</span></span>](#tab/visual-studio-code-xml/)

<span data-ttu-id="06ed3-192">Agregue manualmente las líneas resaltadas al archivo *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="06ed3-192">Manually add the highlighted lines to the *.csproj* file:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

---

### <a name="data-annotations"></a><span data-ttu-id="06ed3-193">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="06ed3-193">Data annotations</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="06ed3-194">NSwag usa [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), y el tipo de valor devuelto recomendado para las acciones de Web API es [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult).</span><span class="sxs-lookup"><span data-stu-id="06ed3-194">NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the recommended return type for web API actions is [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult).</span></span> <span data-ttu-id="06ed3-195">Por tanto, NSwag no puede deducir lo que la acción realiza y lo que devuelve.</span><span class="sxs-lookup"><span data-stu-id="06ed3-195">Consequently, NSwag can't infer what your action is doing and what it returns.</span></span> <span data-ttu-id="06ed3-196">Considere el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="06ed3-196">Consider the following example:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

<span data-ttu-id="06ed3-197">La acción anterior devuelve `IActionResult` pero, dentro de la acción, devuelve [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) o [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest).</span><span class="sxs-lookup"><span data-stu-id="06ed3-197">The preceding action returns `IActionResult`, but inside the action it's returning either [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) or [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest).</span></span> <span data-ttu-id="06ed3-198">Las anotaciones de datos sirven para indicar a los clientes qué códigos de estado HTTP se sabe que esta acción devuelve.</span><span class="sxs-lookup"><span data-stu-id="06ed3-198">Data annotations are used to tell clients which HTTP status codes this action is known to return.</span></span> <span data-ttu-id="06ed3-199">Complete la acción con los siguientes atributos:</span><span class="sxs-lookup"><span data-stu-id="06ed3-199">Decorate the action with the following attributes:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="06ed3-200">NSwag usa [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), y el tipo de valor devuelto recomendado para las acciones de Web API es [ActionResult\<](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1).</span><span class="sxs-lookup"><span data-stu-id="06ed3-200">NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the recommended return type for web API actions is [ActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1).</span></span> <span data-ttu-id="06ed3-201">Por lo tanto, NSwag puede inferir únicamente el tipo de valor devuelto definido por `T`.</span><span class="sxs-lookup"><span data-stu-id="06ed3-201">Consequently, NSwag can only infer the return type defined by `T`.</span></span> <span data-ttu-id="06ed3-202">No podrá inferir otros tipos de valor devuelto que son factibles en la acción.</span><span class="sxs-lookup"><span data-stu-id="06ed3-202">Other possible return types in the action cannot be inferred.</span></span> <span data-ttu-id="06ed3-203">Considere el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="06ed3-203">Consider the following example:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

<span data-ttu-id="06ed3-204">La acción anterior devuelve `ActionResult<T>`.</span><span class="sxs-lookup"><span data-stu-id="06ed3-204">The preceding action returns `ActionResult<T>`.</span></span> <span data-ttu-id="06ed3-205">Dentro de la acción, se devuelve [CreatedAtRoute](xref:System.Web.Http.ApiController.CreatedAtRoute*).</span><span class="sxs-lookup"><span data-stu-id="06ed3-205">Inside the action, it's returning [CreatedAtRoute](xref:System.Web.Http.ApiController.CreatedAtRoute*).</span></span> <span data-ttu-id="06ed3-206">Como el controlador se representa con el atributo [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute), también es posible una respuesta [BadRequest](xref:System.Web.Http.ApiController.BadRequest*).</span><span class="sxs-lookup"><span data-stu-id="06ed3-206">Since the controller is decorated with the [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute, a [BadRequest](xref:System.Web.Http.ApiController.BadRequest*) response is possible, too.</span></span> <span data-ttu-id="06ed3-207">Para obtener más información, consulte [Respuestas HTTP 400 automáticas](xref:web-api/index#automatic-http-400-responses).</span><span class="sxs-lookup"><span data-stu-id="06ed3-207">For more information, see [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses).</span></span> <span data-ttu-id="06ed3-208">Las anotaciones de datos sirven para indicar a los clientes qué códigos de estado HTTP se sabe que esta acción devuelve.</span><span class="sxs-lookup"><span data-stu-id="06ed3-208">Data annotations are used to tell clients which HTTP status codes this action is known to return.</span></span> <span data-ttu-id="06ed3-209">Complete la acción con los siguientes atributos:</span><span class="sxs-lookup"><span data-stu-id="06ed3-209">Decorate the action with the following attributes:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]

<span data-ttu-id="06ed3-210">En ASP.NET Core 2.2 o versiones posteriores, las convenciones se pueden usar como alternativa a la representación explícita de acciones individuales con `[ProducesResponseType]`.</span><span class="sxs-lookup"><span data-stu-id="06ed3-210">In ASP.NET Core 2.2 or later, conventions can be used as an alternative to explicitly decorating individual actions with `[ProducesResponseType]`.</span></span> <span data-ttu-id="06ed3-211">Para obtener más información, vea <xref:web-api/advanced/conventions>.</span><span class="sxs-lookup"><span data-stu-id="06ed3-211">For more information, see <xref:web-api/advanced/conventions>.</span></span>

::: moniker-end

<span data-ttu-id="06ed3-212">Ahora, el generador de Swagger puede describir con precisión esta acción y los clientes generados sabrán lo que reciben cuando llamen al punto de conexión.</span><span class="sxs-lookup"><span data-stu-id="06ed3-212">The Swagger generator can now accurately describe this action, and generated clients know what they receive when calling the endpoint.</span></span> <span data-ttu-id="06ed3-213">Completar todas las acciones con estos atributos es tremendamente recomendable.</span><span class="sxs-lookup"><span data-stu-id="06ed3-213">Decorating all actions with these attributes is highly recommended.</span></span> <span data-ttu-id="06ed3-214">Para obtener instrucciones sobre qué respuestas HTTP deben devolver las acciones de API, vea la [especificación RFC 7231](https://tools.ietf.org/html/rfc7231#section-4.3).</span><span class="sxs-lookup"><span data-stu-id="06ed3-214">For guidelines on what HTTP responses your API actions should return, see the [RFC 7231 specification](https://tools.ietf.org/html/rfc7231#section-4.3).</span></span>
