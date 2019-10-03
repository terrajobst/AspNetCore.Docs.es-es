---
title: Introducción a NSwag y ASP.NET Core
author: zuckerthoben
description: Obtenga información sobre cómo usar NSwag para generar documentación y páginas de ayuda de una ASP.NET Web API.
ms.author: scaddie
ms.custom: mvc
ms.date: 06/21/2019
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: 5e62a8cc50947969d42981350b65a24781929d62
ms.sourcegitcommit: fe88748b762525cb490f7e39089a4760f6a73a24
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/30/2019
ms.locfileid: "71691180"
---
# <a name="get-started-with-nswag-and-aspnet-core"></a><span data-ttu-id="c934a-103">Introducción a NSwag y ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c934a-103">Get started with NSwag and ASP.NET Core</span></span>

<span data-ttu-id="c934a-104">Por [Christoph Nienaber](https://twitter.com/zuckerthoben), [Rico Suter](https://rsuter.com) y [Dave Brock](https://twitter.com/daveabrock)</span><span class="sxs-lookup"><span data-stu-id="c934a-104">By [Christoph Nienaber](https://twitter.com/zuckerthoben), [Rico Suter](https://rsuter.com), and [Dave Brock](https://twitter.com/daveabrock)</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="c934a-105">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c934a-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="c934a-106">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c934a-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker-end

<span data-ttu-id="c934a-107">NSwag ofrece las siguientes capacidades:</span><span class="sxs-lookup"><span data-stu-id="c934a-107">NSwag offers the following capabilities:</span></span>

* <span data-ttu-id="c934a-108">Capacidad de usar la interfaz de usuario y el generador de Swagger.</span><span class="sxs-lookup"><span data-stu-id="c934a-108">The ability to utilize the Swagger UI and Swagger generator.</span></span>
* <span data-ttu-id="c934a-109">Capacidad de generar código con flexibilidad.</span><span class="sxs-lookup"><span data-stu-id="c934a-109">Flexible code generation capabilities.</span></span>

<span data-ttu-id="c934a-110">Con NSwag, no es necesario que exista una API; se pueden usar API de terceros que incluyan Swagger y que generen una implementación de cliente.</span><span class="sxs-lookup"><span data-stu-id="c934a-110">With NSwag, you don't need an existing API&mdash;you can use third-party APIs that incorporate Swagger and generate a client implementation.</span></span> <span data-ttu-id="c934a-111">NSwag permite acelerar el ciclo de desarrollo y adaptarse fácilmente a los cambios de API.</span><span class="sxs-lookup"><span data-stu-id="c934a-111">NSwag allows you to expedite the development cycle and easily adapt to API changes.</span></span>

## <a name="register-the-nswag-middleware"></a><span data-ttu-id="c934a-112">Registro del middleware de NSwag</span><span class="sxs-lookup"><span data-stu-id="c934a-112">Register the NSwag middleware</span></span>

<span data-ttu-id="c934a-113">Registre el middleware de NSwag para:</span><span class="sxs-lookup"><span data-stu-id="c934a-113">Register the NSwag middleware to:</span></span>

* <span data-ttu-id="c934a-114">Generar la especificación de Swagger para la API web implementada.</span><span class="sxs-lookup"><span data-stu-id="c934a-114">Generate the Swagger specification for the implemented web API.</span></span>
* <span data-ttu-id="c934a-115">Proporcionar la interfaz de usuario de Swagger para examinar y probar la API web.</span><span class="sxs-lookup"><span data-stu-id="c934a-115">Serve the Swagger UI to browse and test the web API.</span></span>

<span data-ttu-id="c934a-116">Para usar [NSwag](https://github.com/RicoSuter/NSwag) con middleware de ASP.NET Core, instale el paquete NuGet [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/).</span><span class="sxs-lookup"><span data-stu-id="c934a-116">To use the [NSwag](https://github.com/RicoSuter/NSwag) ASP.NET Core middleware, install the [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet package.</span></span> <span data-ttu-id="c934a-117">Este paquete contiene el middleware para generar y proporcionar la especificación de Swagger, la interfaz de usuario de Swagger (v2 y v3) y la [interfaz de usuario de ReDoc](https://github.com/Rebilly/ReDoc).</span><span class="sxs-lookup"><span data-stu-id="c934a-117">This package contains the middleware to generate and serve the Swagger specification, Swagger UI (v2 and v3), and [ReDoc UI](https://github.com/Rebilly/ReDoc).</span></span>

<span data-ttu-id="c934a-118">Use uno de los siguientes métodos para instalar el paquete NuGet de NSwag:</span><span class="sxs-lookup"><span data-stu-id="c934a-118">Use one of the following approaches to install the NSwag NuGet package:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c934a-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c934a-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c934a-120">En la ventana **Consola del Administrador de paquetes**:</span><span class="sxs-lookup"><span data-stu-id="c934a-120">From the **Package Manager Console** window:</span></span>
  * <span data-ttu-id="c934a-121">Vaya a **Vista** > **Otras ventanas** > **Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="c934a-121">Go to **View** > **Other Windows** > **Package Manager Console**</span></span>
  * <span data-ttu-id="c934a-122">Vaya al directorio en el que está *TodoApi.csproj*.</span><span class="sxs-lookup"><span data-stu-id="c934a-122">Navigate to the directory in which the *TodoApi.csproj* file exists</span></span>
  * <span data-ttu-id="c934a-123">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="c934a-123">Execute the following command:</span></span>

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* <span data-ttu-id="c934a-124">En el cuadro de diálogo **Administrar paquetes NuGet**:</span><span class="sxs-lookup"><span data-stu-id="c934a-124">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="c934a-125">Haga clic con el botón derecho en el proyecto en el **Explorador de soluciones** > **Administrar paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="c934a-125">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="c934a-126">Establezca el **origen del paquete** en "nuget.org".</span><span class="sxs-lookup"><span data-stu-id="c934a-126">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="c934a-127">Escriba "NSwag.AspNetCore" en el cuadro de búsqueda.</span><span class="sxs-lookup"><span data-stu-id="c934a-127">Enter "NSwag.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="c934a-128">Seleccione el paquete "NSwag.AspNetCore" en la pestaña **Examinar** y haga clic en **Instalar**.</span><span class="sxs-lookup"><span data-stu-id="c934a-128">Select the "NSwag.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c934a-129">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="c934a-129">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="c934a-130">Haga clic con el botón derecho en la carpeta *Paquetes* en **Panel de solución** > **Agregar paquetes...**</span><span class="sxs-lookup"><span data-stu-id="c934a-130">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="c934a-131">Establezca el menú desplegable **Origen** de la ventana **Agregar paquetes** en "nuget.org".</span><span class="sxs-lookup"><span data-stu-id="c934a-131">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="c934a-132">Escriba "NSwag.AspNetCore" en el cuadro de búsqueda.</span><span class="sxs-lookup"><span data-stu-id="c934a-132">Enter "NSwag.AspNetCore" in the search box</span></span>
* <span data-ttu-id="c934a-133">Seleccione el paquete "NSwag.AspNetCore" en el panel de resultados y haga clic en **Agregar paquete**.</span><span class="sxs-lookup"><span data-stu-id="c934a-133">Select the "NSwag.AspNetCore" package from the results pane and click **Add Package**</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="c934a-134">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="c934a-134">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="c934a-135">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="c934a-135">Run the following command:</span></span>

```dotnetcli
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="c934a-136">Agregar y configurar el middleware de Swagger</span><span class="sxs-lookup"><span data-stu-id="c934a-136">Add and configure Swagger middleware</span></span>

<span data-ttu-id="c934a-137">Para agregar y configurar Swagger en su aplicación de ASP.NET Core, realice los siguientes pasos:</span><span class="sxs-lookup"><span data-stu-id="c934a-137">Add and configure Swagger in your ASP.NET Core app by performing the following steps:</span></span>

* <span data-ttu-id="c934a-138">En el método `Startup.ConfigureServices`, registre los servicios necesarios de Swagger:</span><span class="sxs-lookup"><span data-stu-id="c934a-138">In the `Startup.ConfigureServices` method, register the required Swagger services:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_ConfigureServices&highlight=8)]

* <span data-ttu-id="c934a-139">En el método `Startup.Configure`, habilite el middleware para servir la especificación y la interfaz de usuario de Swagger:</span><span class="sxs-lookup"><span data-stu-id="c934a-139">In the `Startup.Configure` method, enable the middleware for serving the generated Swagger specification and the Swagger UI:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=6-7)]

* <span data-ttu-id="c934a-140">Inicie la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c934a-140">Launch the app.</span></span> <span data-ttu-id="c934a-141">Vaya a:</span><span class="sxs-lookup"><span data-stu-id="c934a-141">Navigate to:</span></span>
  * <span data-ttu-id="c934a-142">`http://localhost:<port>/swagger` para ver la interfaz de usuario de Swagger.</span><span class="sxs-lookup"><span data-stu-id="c934a-142">`http://localhost:<port>/swagger` to view the Swagger UI.</span></span>
  * <span data-ttu-id="c934a-143">`http://localhost:<port>/swagger/v1/swagger.json` para ver la especificación de Swagger.</span><span class="sxs-lookup"><span data-stu-id="c934a-143">`http://localhost:<port>/swagger/v1/swagger.json` to view the Swagger specification.</span></span>

## <a name="code-generation"></a><span data-ttu-id="c934a-144">Generación de código</span><span class="sxs-lookup"><span data-stu-id="c934a-144">Code generation</span></span>

<span data-ttu-id="c934a-145">Para aprovechar las capacidades de generación de código de NSwag, elija una de las siguientes opciones:</span><span class="sxs-lookup"><span data-stu-id="c934a-145">You can take advantage of NSwag's code generation capabilities by choosing one of the following options:</span></span>

* <span data-ttu-id="c934a-146">[NSwagStudio](https://github.com/RicoSuter/NSwag/wiki/NSwagStudio), una aplicación de escritorio de Windows que permite generar código de cliente de API de C# o TypeScript.</span><span class="sxs-lookup"><span data-stu-id="c934a-146">[NSwagStudio](https://github.com/RicoSuter/NSwag/wiki/NSwagStudio) &ndash; a Windows desktop app for generating API client code in C# or TypeScript.</span></span>
* <span data-ttu-id="c934a-147">Paquetes NuGet [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) o [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) para generar código dentro del proyecto.</span><span class="sxs-lookup"><span data-stu-id="c934a-147">The [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) or [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) NuGet packages for code generation inside your project.</span></span>
* <span data-ttu-id="c934a-148">NSwag desde la [línea de comandos](https://github.com/RicoSuter/NSwag/wiki/CommandLine).</span><span class="sxs-lookup"><span data-stu-id="c934a-148">NSwag from the [command line](https://github.com/RicoSuter/NSwag/wiki/CommandLine).</span></span>
* <span data-ttu-id="c934a-149">Paquete NuGet [NSwag.MSBuild](https://github.com/RicoSuter/NSwag/wiki/NSwag.MSBuild).</span><span class="sxs-lookup"><span data-stu-id="c934a-149">The [NSwag.MSBuild](https://github.com/RicoSuter/NSwag/wiki/NSwag.MSBuild) NuGet package.</span></span>
* <span data-ttu-id="c934a-150">[Servicio conectado de Unchase OpenAPI (Swagger)](https://marketplace.visualstudio.com/items?itemName=Unchase.unchaseopenapiconnectedservice), un servicio conectado de Visual Studio para generar código de cliente de API en C# o TypeScript.</span><span class="sxs-lookup"><span data-stu-id="c934a-150">The [Unchase OpenAPI (Swagger) Connected Service](https://marketplace.visualstudio.com/items?itemName=Unchase.unchaseopenapiconnectedservice) &ndash; a Visual Studio Connected Service for generating API client code in C# or TypeScript.</span></span> <span data-ttu-id="c934a-151">También genera controladores de C# para los servicios de OpenAPI con NSwag.</span><span class="sxs-lookup"><span data-stu-id="c934a-151">Also generates C# controllers for OpenAPI services with NSwag.</span></span>

### <a name="generate-code-with-nswagstudio"></a><span data-ttu-id="c934a-152">Generación de código con NSwagStudio</span><span class="sxs-lookup"><span data-stu-id="c934a-152">Generate code with NSwagStudio</span></span>

* <span data-ttu-id="c934a-153">Instale NSwagStudio siguiendo las instrucciones del [repositorio de GitHub de NSwagStudio](https://github.com/RicoSuter/NSwag/wiki/NSwagStudio).</span><span class="sxs-lookup"><span data-stu-id="c934a-153">Install NSwagStudio by following the instructions at the [NSwagStudio GitHub repository](https://github.com/RicoSuter/NSwag/wiki/NSwagStudio).</span></span>
* <span data-ttu-id="c934a-154">Inicie NSwagStudio y escriba la URL del archivo *swagger.json* en el cuadro de texto **Swagger Specification URL** (URL de especificación de Swagger).</span><span class="sxs-lookup"><span data-stu-id="c934a-154">Launch NSwagStudio and enter the *swagger.json* file URL in the **Swagger Specification URL** text box.</span></span> <span data-ttu-id="c934a-155">Por ejemplo, *http://localhost:44354/swagger/v1/swagger.json* .</span><span class="sxs-lookup"><span data-stu-id="c934a-155">For example, *http://localhost:44354/swagger/v1/swagger.json*.</span></span>
* <span data-ttu-id="c934a-156">Haga clic en el botón **Create local Copy** (Crear copia local) para generar una representación JSON de la especificación de Swagger.</span><span class="sxs-lookup"><span data-stu-id="c934a-156">Click the **Create local Copy** button to generate a JSON representation of your Swagger specification.</span></span>

  ![Creación de una copia local de la especificación de Swagger](web-api-help-pages-using-swagger/_static/CreateLocalCopy-NSwagStudio.PNG)

* <span data-ttu-id="c934a-158">En el área **Outputs** (Salidas), haga clic en la casilla **CSharp Client** (Cliente de CSharp).</span><span class="sxs-lookup"><span data-stu-id="c934a-158">In the **Outputs** area, click the **CSharp Client** check box.</span></span> <span data-ttu-id="c934a-159">En función del proyecto, también puede elegir **TypeScript Client** (Cliente de TypeScript) o **CSharp Web API Controller** (Controlador de API web de CSharp).</span><span class="sxs-lookup"><span data-stu-id="c934a-159">Depending on your project, you can also choose **TypeScript Client** or **CSharp Web API Controller**.</span></span> <span data-ttu-id="c934a-160">Si selecciona **CSharp Web API Controller** (Controlador de API web de CSharp), una especificación de servicio volverá a generar el servicio a modo de generación inversa.</span><span class="sxs-lookup"><span data-stu-id="c934a-160">If you select **CSharp Web API Controller**, a service specification rebuilds the service, serving as a reverse generation.</span></span>
* <span data-ttu-id="c934a-161">Haga clic en **Generate Outputs** (Generar salidas) para producir una implementación completa del cliente de C# del proyecto *TodoApi.NSwag*.</span><span class="sxs-lookup"><span data-stu-id="c934a-161">Click **Generate Outputs** to produce a complete C# client implementation of the *TodoApi.NSwag* project.</span></span> <span data-ttu-id="c934a-162">Para ver el código de cliente generado, haga clic en la pestaña **CSharp Client** (Cliente de CSharp):</span><span class="sxs-lookup"><span data-stu-id="c934a-162">To see the generated client code, click the **CSharp Client** tab:</span></span>

```csharp
//----------------------
// <auto-generated>
//     Generated using the NSwag toolchain v12.0.9.0 (NJsonSchema v9.13.10.0 (Newtonsoft.Json v11.0.0.0)) (http://NSwag.org)
// </auto-generated>
//----------------------

namespace MyNamespace
{
    #pragma warning disable

    [System.CodeDom.Compiler.GeneratedCode("NSwag", "12.0.9.0 (NJsonSchema v9.13.10.0 (Newtonsoft.Json v11.0.0.0))")]
    public partial class TodoClient
    {
        private string _baseUrl = "https://localhost:44354";
        private System.Net.Http.HttpClient _httpClient;
        private System.Lazy<Newtonsoft.Json.JsonSerializerSettings> _settings;

        public TodoClient(System.Net.Http.HttpClient httpClient)
        {
            _httpClient = httpClient;
            _settings = new System.Lazy<Newtonsoft.Json.JsonSerializerSettings>(() =>
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
> <span data-ttu-id="c934a-163">El código de cliente de C# se genera en función de las selecciones en la pestaña **Settings** (Configuración). Modifique esta configuración para realizar tareas como cambiar el nombre del espacio de nombres predeterminado y generar un método sincrónico.</span><span class="sxs-lookup"><span data-stu-id="c934a-163">The C# client code is generated based on selections in the **Settings** tab. Modify the settings to perform tasks such as default namespace renaming and synchronous method generation.</span></span>

* <span data-ttu-id="c934a-164">Copie el código de C# generado en un archivo del proyecto de cliente que usará la API.</span><span class="sxs-lookup"><span data-stu-id="c934a-164">Copy the generated C# code into a file in the client project that will consume the API.</span></span>
* <span data-ttu-id="c934a-165">Empiece a consumir la Web API:</span><span class="sxs-lookup"><span data-stu-id="c934a-165">Start consuming the web API:</span></span>

```csharp
 var todoClient = new TodoClient();

// Gets all to-dos from the API
 var allTodos = await todoClient.GetAllAsync();

 // Create a new TodoItem, and save it via the API.
var createdTodo = await todoClient.CreateAsync(new TodoItem());

// Get a single to-do by ID
var foundTodo = await todoClient.GetByIdAsync(1);
```

## <a name="customize-api-documentation"></a><span data-ttu-id="c934a-166">Personalización de la documentación de API</span><span class="sxs-lookup"><span data-stu-id="c934a-166">Customize API documentation</span></span>

<span data-ttu-id="c934a-167">Swagger proporciona opciones para documentar el modelo de objetos de cara a facilitar el consumo de Web API.</span><span class="sxs-lookup"><span data-stu-id="c934a-167">Swagger provides options for documenting the object model to ease consumption of the web API.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="c934a-168">Información y descripción de la API</span><span class="sxs-lookup"><span data-stu-id="c934a-168">API info and description</span></span>

<span data-ttu-id="c934a-169">En el método `Startup.ConfigureServices`, la acción de configuración que se pasa al método `AddSwaggerDocument` agrega información, como el autor, la licencia y la descripción:</span><span class="sxs-lookup"><span data-stu-id="c934a-169">In the `Startup.ConfigureServices` method, a configuration action passed to the `AddSwaggerDocument` method adds information such as the author, license, and description:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup2.cs?name=snippet_AddSwaggerDocument)]

<span data-ttu-id="c934a-170">La interfaz de usuario de Swagger muestra información de la versión:</span><span class="sxs-lookup"><span data-stu-id="c934a-170">The Swagger UI displays the version's information:</span></span>

![Interfaz de usuario de Swagger con información de la versión](web-api-help-pages-using-swagger/_static/custom-info-nswag.png)

### <a name="xml-comments"></a><span data-ttu-id="c934a-172">comentarios XML</span><span class="sxs-lookup"><span data-stu-id="c934a-172">XML comments</span></span>

<span data-ttu-id="c934a-173">Para habilitar los comentarios XML, realice los siguientes pasos:</span><span class="sxs-lookup"><span data-stu-id="c934a-173">To enable XML comments, perform the following steps:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c934a-174">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c934a-174">Visual Studio</span></span>](#tab/visual-studio)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="c934a-175">Haga clic con el botón derecho en el **Explorador de soluciones** y seleccione **Editar <nombre_de_proyecto>.csproj**.</span><span class="sxs-lookup"><span data-stu-id="c934a-175">Right-click the project in **Solution Explorer** and select **Edit <project_name>.csproj**.</span></span>
* <span data-ttu-id="c934a-176">Agregue manualmente las líneas resaltadas al archivo *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="c934a-176">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="c934a-177">En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Propiedades**.</span><span class="sxs-lookup"><span data-stu-id="c934a-177">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="c934a-178">Active la casilla **Archivo de documentación XML** en la sección **Salida** de la pestaña **Compilar**.</span><span class="sxs-lookup"><span data-stu-id="c934a-178">Check the **XML documentation file** box under the **Output** section of the **Build** tab</span></span>

::: moniker-end

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c934a-179">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="c934a-179">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="c934a-180">Desde el *Panel de solución*, presione **Control** y haga clic en el nombre del proyecto.</span><span class="sxs-lookup"><span data-stu-id="c934a-180">From the *Solution Pad*, press **control** and click the project name.</span></span> <span data-ttu-id="c934a-181">Vaya a **Herramientas** > **Editar archivo**.</span><span class="sxs-lookup"><span data-stu-id="c934a-181">Navigate to **Tools** > **Edit File**.</span></span>
* <span data-ttu-id="c934a-182">Agregue manualmente las líneas resaltadas al archivo *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="c934a-182">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="c934a-183">Abra el cuadro de diálogo **Opciones del proyecto** > **Compilar** > **Compilador**.</span><span class="sxs-lookup"><span data-stu-id="c934a-183">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="c934a-184">Active la casilla **Generar documentación XML** en la sección **Opciones generales**.</span><span class="sxs-lookup"><span data-stu-id="c934a-184">Check the **Generate xml documentation** box under the **General Options** section</span></span>

::: moniker-end

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="c934a-185">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="c934a-185">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="c934a-186">Agregue manualmente las líneas resaltadas al archivo *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="c934a-186">Manually add the highlighted lines to the *.csproj* file:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

---

### <a name="data-annotations"></a><span data-ttu-id="c934a-187">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="c934a-187">Data annotations</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="c934a-188">Dado que NSwag usa [Reflection](/dotnet/csharp/programming-guide/concepts/reflection) y el tipo de valor devuelto recomendado para acciones de API web es [IActionResult](xref:Microsoft.AspNetCore.Mvc.IActionResult), no puede inferir su acción ni el valor que devuelve.</span><span class="sxs-lookup"><span data-stu-id="c934a-188">Because NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the recommended return type for web API actions is [IActionResult](xref:Microsoft.AspNetCore.Mvc.IActionResult), it can't infer what your action is doing and what it returns.</span></span>

<span data-ttu-id="c934a-189">Considere el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="c934a-189">Consider the following example:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

<span data-ttu-id="c934a-190">La acción anterior devuelve `IActionResult` pero, dentro de la acción, devuelve [CreatedAtRoute](xref:System.Web.Http.ApiController.CreatedAtRoute*) o [BadRequest](xref:System.Web.Http.ApiController.BadRequest*).</span><span class="sxs-lookup"><span data-stu-id="c934a-190">The preceding action returns `IActionResult`, but inside the action it's returning either [CreatedAtRoute](xref:System.Web.Http.ApiController.CreatedAtRoute*) or [BadRequest](xref:System.Web.Http.ApiController.BadRequest*).</span></span> <span data-ttu-id="c934a-191">Use las anotaciones de datos para indicar a los clientes qué códigos de estado HTTP se sabe que esta acción devuelve.</span><span class="sxs-lookup"><span data-stu-id="c934a-191">Use data annotations to tell clients which HTTP status codes this action is known to return.</span></span> <span data-ttu-id="c934a-192">Complete la acción con los siguientes atributos:</span><span class="sxs-lookup"><span data-stu-id="c934a-192">Decorate the action with the following attributes:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

 <span data-ttu-id="c934a-193">Dado que NSwag usa [Reflection](/dotnet/csharp/programming-guide/concepts/reflection) y el tipo de valor devuelto recomendado para acciones de API web es [ActionResult\<T>](xref:Microsoft.AspNetCore.Mvc.ActionResult%601), solo puede inferir el tipo de valor devuelto definido por `T`.</span><span class="sxs-lookup"><span data-stu-id="c934a-193">Because NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the recommended return type for web API actions is [ActionResult\<T>](xref:Microsoft.AspNetCore.Mvc.ActionResult%601), it can only infer the return type defined by `T`.</span></span> <span data-ttu-id="c934a-194">No puede inferir automáticamente ningún otro tipo de valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="c934a-194">You can't automatically infer other possible return types.</span></span>

<span data-ttu-id="c934a-195">Considere el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="c934a-195">Consider the following example:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

<span data-ttu-id="c934a-196">La acción anterior devuelve `ActionResult<T>`.</span><span class="sxs-lookup"><span data-stu-id="c934a-196">The preceding action returns `ActionResult<T>`.</span></span> <span data-ttu-id="c934a-197">Dentro de la acción, se devuelve [CreatedAtRoute](xref:System.Web.Http.ApiController.CreatedAtRoute*).</span><span class="sxs-lookup"><span data-stu-id="c934a-197">Inside the action, it's returning [CreatedAtRoute](xref:System.Web.Http.ApiController.CreatedAtRoute*).</span></span> <span data-ttu-id="c934a-198">Como el controlador se representa con el atributo [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute), también es posible una respuesta [BadRequest](xref:System.Web.Http.ApiController.BadRequest*).</span><span class="sxs-lookup"><span data-stu-id="c934a-198">Since the controller is decorated with the [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute, a [BadRequest](xref:System.Web.Http.ApiController.BadRequest*) response is possible, too.</span></span> <span data-ttu-id="c934a-199">Para obtener más información, consulte [Respuestas HTTP 400 automáticas](xref:web-api/index#automatic-http-400-responses).</span><span class="sxs-lookup"><span data-stu-id="c934a-199">For more information, see [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses).</span></span> <span data-ttu-id="c934a-200">Use las anotaciones de datos para indicar a los clientes qué códigos de estado HTTP se sabe que esta acción devuelve.</span><span class="sxs-lookup"><span data-stu-id="c934a-200">Use data annotations to tell clients which HTTP status codes this action is known to return.</span></span> <span data-ttu-id="c934a-201">Complete la acción con los siguientes atributos:</span><span class="sxs-lookup"><span data-stu-id="c934a-201">Decorate the action with the following attributes:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]

<span data-ttu-id="c934a-202">En ASP.NET Core 2.2 o versiones posteriores, puede usar las convenciones en lugar de decorar explícitamente acciones individuales con `[ProducesResponseType]`.</span><span class="sxs-lookup"><span data-stu-id="c934a-202">In ASP.NET Core 2.2 or later, you can use conventions instead of explicitly decorating individual actions with `[ProducesResponseType]`.</span></span> <span data-ttu-id="c934a-203">Para más información, consulte <xref:web-api/advanced/conventions>.</span><span class="sxs-lookup"><span data-stu-id="c934a-203">For more information, see <xref:web-api/advanced/conventions>.</span></span>

::: moniker-end

<span data-ttu-id="c934a-204">Ahora, el generador de Swagger puede describir con precisión esta acción y los clientes generados sabrán lo que reciben cuando llamen al punto de conexión.</span><span class="sxs-lookup"><span data-stu-id="c934a-204">The Swagger generator can now accurately describe this action, and generated clients know what they receive when calling the endpoint.</span></span> <span data-ttu-id="c934a-205">Le recomendamos que decore todas las acciones con estos atributos.</span><span class="sxs-lookup"><span data-stu-id="c934a-205">As a recommendation, decorate all actions with these attributes.</span></span>

<span data-ttu-id="c934a-206">Para obtener instrucciones sobre qué respuestas HTTP deben devolver las acciones de API, vea la [especificación RFC 7231](https://tools.ietf.org/html/rfc7231#section-4.3).</span><span class="sxs-lookup"><span data-stu-id="c934a-206">For guidelines on what HTTP responses your API actions should return, see the [RFC 7231 specification](https://tools.ietf.org/html/rfc7231#section-4.3).</span></span>
