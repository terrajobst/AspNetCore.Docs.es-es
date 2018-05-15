---
title: Introducción a NSwag y ASP.NET Core
author: zuckerthoben
description: Obtenga información sobre cómo usar NSwag para generar documentación y páginas de ayuda para una aplicación ASP.NET Core Web API.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 03/15/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: 80e6a9e1702d8f68d139d2ff9c3a01a27c40cecb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-nswag-and-aspnet-core"></a><span data-ttu-id="c024f-103">Introducción a NSwag y ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c024f-103">Get started with NSwag and ASP.NET Core</span></span>

<span data-ttu-id="c024f-104">Por [Christoph Nienaber](https://twitter.com/zuckerthoben) y [Rico Suter](https://rsuter.com)</span><span class="sxs-lookup"><span data-stu-id="c024f-104">By [Christoph Nienaber](https://twitter.com/zuckerthoben) and [Rico Suter](https://rsuter.com)</span></span>

<span data-ttu-id="c024f-105">Para usar [NSwag](https://github.com/RSuter/NSwag) con middleware de ASP.NET Core, se necesita el paquete NuGet [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/).</span><span class="sxs-lookup"><span data-stu-id="c024f-105">Using [NSwag](https://github.com/RSuter/NSwag) with ASP.NET Core middleware requires the [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet package.</span></span> <span data-ttu-id="c024f-106">Este paquete consta de un generador de Swagger, la interfaz de usuario de Swagger (versiones 2 y 3) y la [interfaz de usuario de ReDoc](https://github.com/Rebilly/ReDoc).</span><span class="sxs-lookup"><span data-stu-id="c024f-106">The package consists of a Swagger generator, Swagger UI (v2 and v3), and [ReDoc UI](https://github.com/Rebilly/ReDoc).</span></span>

<span data-ttu-id="c024f-107">Se recomienda encarecidamente usar las capacidades de generación de código de NSwag.</span><span class="sxs-lookup"><span data-stu-id="c024f-107">It's highly recommended to make use of NSwag's code generation capabilities.</span></span> <span data-ttu-id="c024f-108">Elija una de las siguientes opciones para generar código:</span><span class="sxs-lookup"><span data-stu-id="c024f-108">Choose one of the following options for code generation:</span></span>

* <span data-ttu-id="c024f-109">Usar [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), una aplicación de escritorio de Windows para generar código de cliente en C# y TypeScript para la API</span><span class="sxs-lookup"><span data-stu-id="c024f-109">Use [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), a Windows desktop app for generating client code in C# and TypeScript for your API</span></span>
* <span data-ttu-id="c024f-110">Usar los paquetes NuGet [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) o [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) para generar código dentro del proyecto</span><span class="sxs-lookup"><span data-stu-id="c024f-110">Use the [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) or [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) NuGet packages to do code generation inside your project</span></span>
* <span data-ttu-id="c024f-111">Usar NSwag desde la [línea de comandos](https://github.com/NSwag/NSwag/wiki/CommandLine)</span><span class="sxs-lookup"><span data-stu-id="c024f-111">Use NSwag from the [command line](https://github.com/NSwag/NSwag/wiki/CommandLine)</span></span>
* <span data-ttu-id="c024f-112">Usar el paquete NuGet [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild)</span><span class="sxs-lookup"><span data-stu-id="c024f-112">Use the [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) NuGet package</span></span>

## <a name="features"></a><span data-ttu-id="c024f-113">Características</span><span class="sxs-lookup"><span data-stu-id="c024f-113">Features</span></span>

<span data-ttu-id="c024f-114">La razón principal para usar NSwag es la posibilidad no ya de incorporar la interfaz de usuario de Swagger y el generador de Swagger, sino también de usar sus capacidades de generación de código flexibles.</span><span class="sxs-lookup"><span data-stu-id="c024f-114">The main reason to use NSwag is the ability to not only introduce the Swagger UI and Swagger generator, but to make use of the flexible code generation capabilities.</span></span> <span data-ttu-id="c024f-115">No es necesario que exista una API: se pueden usar API de terceros que incluyan Swagger y que permitan que NSwag genere una implementación de cliente.</span><span class="sxs-lookup"><span data-stu-id="c024f-115">You don't need an existing API&mdash;you can use third-party APIs that incorporate Swagger and let NSwag generate a client implementation.</span></span> <span data-ttu-id="c024f-116">En cualquier caso, el ciclo de desarrollo se agiliza y es más fácil adaptarse a los cambios de API.</span><span class="sxs-lookup"><span data-stu-id="c024f-116">Either way, the development cycle is expedited and you can more easily adapt to API changes.</span></span>

## <a name="package-installation"></a><span data-ttu-id="c024f-117">Instalación del paquete</span><span class="sxs-lookup"><span data-stu-id="c024f-117">Package installation</span></span>

<span data-ttu-id="c024f-118">El paquete NuGet NSwag se puede agregar con uno de los siguientes métodos:</span><span class="sxs-lookup"><span data-stu-id="c024f-118">The NSwag NuGet package can be added with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c024f-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c024f-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c024f-120">En la ventana **Consola del Administrador de paquetes**:</span><span class="sxs-lookup"><span data-stu-id="c024f-120">From the **Package Manager Console** window:</span></span>

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* <span data-ttu-id="c024f-121">En el cuadro de diálogo **Administrar paquetes NuGet**:</span><span class="sxs-lookup"><span data-stu-id="c024f-121">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="c024f-122">Haga clic con el botón derecho en el proyecto en el **Explorador de soluciones** > **Administrar paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="c024f-122">Right-click your project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="c024f-123">Establezca el **origen del paquete** en "nuget.org".</span><span class="sxs-lookup"><span data-stu-id="c024f-123">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="c024f-124">Escriba "NSwag.AspNetCore" en el cuadro de búsqueda.</span><span class="sxs-lookup"><span data-stu-id="c024f-124">Enter "NSwag.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="c024f-125">Seleccione el paquete "NSwag.AspNetCore" en la pestaña **Examinar** y haga clic en **Instalar**.</span><span class="sxs-lookup"><span data-stu-id="c024f-125">Select the "NSwag.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c024f-126">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="c024f-126">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="c024f-127">Haga clic con el botón derecho en la carpeta *Paquetes* en **Panel de solución** > **Agregar paquetes...**</span><span class="sxs-lookup"><span data-stu-id="c024f-127">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="c024f-128">Establezca el menú desplegable **Origen** de la ventana **Agregar paquetes** en "nuget.org".</span><span class="sxs-lookup"><span data-stu-id="c024f-128">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="c024f-129">Escriba NSwag.AspNetCore en el cuadro de búsqueda.</span><span class="sxs-lookup"><span data-stu-id="c024f-129">Enter NSwag.AspNetCore in the search box</span></span>
* <span data-ttu-id="c024f-130">Seleccione el paquete NSwag.AspNetCore en el panel de resultados y haga clic en **Agregar paquete**.</span><span class="sxs-lookup"><span data-stu-id="c024f-130">Select the NSwag.AspNetCore package from the results pane and click **Add Package**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c024f-131">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c024f-131">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="c024f-132">Ejecute el siguiente comando en el **terminal integrado**:</span><span class="sxs-lookup"><span data-stu-id="c024f-132">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.NSwag.csproj package NSwag.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="c024f-133">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="c024f-133">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="c024f-134">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="c024f-134">Run the following command:</span></span>

```console
dotnet add TodoApi.NSwag.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="c024f-135">Agregar y configurar el middleware de Swagger</span><span class="sxs-lookup"><span data-stu-id="c024f-135">Add and configure Swagger middleware</span></span>

<span data-ttu-id="c024f-136">Importe los siguientes espacios de nombres a la clase `Info`:</span><span class="sxs-lookup"><span data-stu-id="c024f-136">Import the following namespaces in the `Info` class:</span></span>

```csharp
using NSwag.AspNetCore;
using System.Reflection;
using NJsonSchema;
```

<span data-ttu-id="c024f-137">En el método `Startup.Configure`, habilite el middleware para servir la especificación y la interfaz de usuario de Swagger:</span><span class="sxs-lookup"><span data-stu-id="c024f-137">In the `Startup.Configure` method, enable the middleware for serving the generated Swagger specification and the Swagger UI:</span></span>

[!code-cs[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=4,7-10)]

<span data-ttu-id="c024f-138">Inicie la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c024f-138">Launch the app.</span></span> <span data-ttu-id="c024f-139">Vaya a `/swagger` para ver la interfaz de usuario de Swagger.</span><span class="sxs-lookup"><span data-stu-id="c024f-139">Navigate to `/swagger` to view the Swagger UI.</span></span> <span data-ttu-id="c024f-140">Vaya a `/swagger/v1/swagger.json` para ver la especificación de Swagger.</span><span class="sxs-lookup"><span data-stu-id="c024f-140">Navigate to `/swagger/v1/swagger.json` to view the Swagger specification.</span></span>

## <a name="code-generation"></a><span data-ttu-id="c024f-141">Generación de código</span><span class="sxs-lookup"><span data-stu-id="c024f-141">Code generation</span></span>

### <a name="via-nswagstudio"></a><span data-ttu-id="c024f-142">Con NSwagStudio</span><span class="sxs-lookup"><span data-stu-id="c024f-142">Via NSwagStudio</span></span>

* <span data-ttu-id="c024f-143">Instale `NSwagStudio` desde el [repositorio de GitHub](https://github.com/RSuter/NSwag/wiki/NSwagStudio) oficial.</span><span class="sxs-lookup"><span data-stu-id="c024f-143">Install `NSwagStudio` from the official [GitHub repository](https://github.com/RSuter/NSwag/wiki/NSwagStudio).</span></span>

* <span data-ttu-id="c024f-144">Inicie NSwagStudio.</span><span class="sxs-lookup"><span data-stu-id="c024f-144">Launch NSwagStudio.</span></span> <span data-ttu-id="c024f-145">Escriba la ubicación del archivo *swagger.json* o cópielo directamente:</span><span class="sxs-lookup"><span data-stu-id="c024f-145">Enter the location of your *swagger.json* or copy it directly:</span></span>

![NSwagStudio](web-api-help-pages-using-swagger/_static/NSwagStudio.png)

* <span data-ttu-id="c024f-147">Indique el tipo de salida de cliente que prefiera.</span><span class="sxs-lookup"><span data-stu-id="c024f-147">Indicate the desired client output type.</span></span> <span data-ttu-id="c024f-148">Las opciones disponibles son **TypeScript Client** (Cliente de TypeScript), **CSharp Client** (Cliente de CSharp) o **CSharp Web API Controller** (Controlador Web API de CSharp).</span><span class="sxs-lookup"><span data-stu-id="c024f-148">Options include **TypeScript Client**, **CSharp Client**, or **CSharp Web API Controller**.</span></span> <span data-ttu-id="c024f-149">Si se usa un controlador Web API, consistirá básicamente en una generación inversa.</span><span class="sxs-lookup"><span data-stu-id="c024f-149">Using a Web API Controller is basically a reverse generation.</span></span> <span data-ttu-id="c024f-150">Se usa una especificación de un servicio para recompilar dicho servicio.</span><span class="sxs-lookup"><span data-stu-id="c024f-150">It uses a specification of a service to rebuild the service.</span></span>

* <span data-ttu-id="c024f-151">Haga clic en **Generate Outputs** (Generar salidas).</span><span class="sxs-lookup"><span data-stu-id="c024f-151">Click **Generate Outputs**.</span></span>

* <span data-ttu-id="c024f-152">Aquí tiene una implementación de cliente completa del ejemplo *TodoApi.NSwag* en C#:</span><span class="sxs-lookup"><span data-stu-id="c024f-152">Here you see a complete client implementation of the *TodoApi.NSwag* sample in C#:</span></span>

![Salida de NSwagStudio](web-api-help-pages-using-swagger/_static/NSwagStudio-Output.png)

* <span data-ttu-id="c024f-154">Coloque el archivo en un proyecto de cliente (por ejemplo, una aplicación [Xamarin.Forms](/xamarin/xamarin-forms/)).</span><span class="sxs-lookup"><span data-stu-id="c024f-154">Put the file into a client project (for example, a [Xamarin.Forms](/xamarin/xamarin-forms/) app).</span></span> <span data-ttu-id="c024f-155">Empiece a consumir la API:</span><span class="sxs-lookup"><span data-stu-id="c024f-155">Start consuming the API:</span></span>

```csharp
var todoClient = new TodoClient();

// Gets all Todos from the Api
var allTodos = await todoClient.GetAllAsync();

// Create a new TodoItem and save it in the Api
var createdTodo = await todoClient.CreateAsync(new TodoItem);

// Get a single Todo by Id
var foundTodo = await todoClient.GetByIdAsync(1);
```

> [!NOTE]
> <span data-ttu-id="c024f-156">Puede insertar una dirección URL base o un cliente HTTP en el cliente de API.</span><span class="sxs-lookup"><span data-stu-id="c024f-156">You can inject a base URL and/or a HTTP client into the API client.</span></span> <span data-ttu-id="c024f-157">El procedimiento recomendado para ello es siempre [reutilizar HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).</span><span class="sxs-lookup"><span data-stu-id="c024f-157">The best practice is to always [reuse the HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).</span></span>

<span data-ttu-id="c024f-158">Ya puede empezar a implementar fácilmente la API en proyectos de cliente.</span><span class="sxs-lookup"><span data-stu-id="c024f-158">You can now start implementing your API into client projects easily.</span></span>

### <a name="other-ways-to-generate-client-code"></a><span data-ttu-id="c024f-159">Otras formas de generar código de cliente</span><span class="sxs-lookup"><span data-stu-id="c024f-159">Other ways to generate client code</span></span>

<span data-ttu-id="c024f-160">Se puede generar código de otras maneras más acordes con el flujo de trabajo:</span><span class="sxs-lookup"><span data-stu-id="c024f-160">You can generate the code in other ways, more suited to your workflow:</span></span>

* [<span data-ttu-id="c024f-161">MSBuild</span><span class="sxs-lookup"><span data-stu-id="c024f-161">MSBuild</span></span>](https://www.nuget.org/packages/NSwag.MSBuild/)

* [<span data-ttu-id="c024f-162">En código</span><span class="sxs-lookup"><span data-stu-id="c024f-162">In code</span></span>](https://github.com/NSwag/NSwag/wiki/SwaggerToCSharpClientGenerator)

* [<span data-ttu-id="c024f-163">Plantillas T4</span><span class="sxs-lookup"><span data-stu-id="c024f-163">T4 templates</span></span>](https://github.com/NSwag/NSwag/wiki/T4)

## <a name="customization"></a><span data-ttu-id="c024f-164">Personalización</span><span class="sxs-lookup"><span data-stu-id="c024f-164">Customization</span></span>

### <a name="xml-comments"></a><span data-ttu-id="c024f-165">comentarios XML</span><span class="sxs-lookup"><span data-stu-id="c024f-165">XML comments</span></span>

<span data-ttu-id="c024f-166">Los comentarios XML se habilitan con los siguientes métodos:</span><span class="sxs-lookup"><span data-stu-id="c024f-166">XML comments are enabled with the following approaches:</span></span>

#### <a name="visual-studiotabvisual-studio-xml"></a>[<span data-ttu-id="c024f-167">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c024f-167">Visual Studio</span></span>](#tab/visual-studio-xml/)
* <span data-ttu-id="c024f-168">En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Propiedades**.</span><span class="sxs-lookup"><span data-stu-id="c024f-168">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="c024f-169">Seleccione la casilla **Archivo de documentación XML** en la sección **Salida** de la pestaña **Compilar**:</span><span class="sxs-lookup"><span data-stu-id="c024f-169">Check the **XML documentation file** box under the **Output** section of the **Build** tab:</span></span>

![Pestaña Compilar de las propiedades del proyecto](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

#### <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[<span data-ttu-id="c024f-171">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="c024f-171">Visual Studio for Mac</span></span>](#tab/visual-studio-mac-xml/)
* <span data-ttu-id="c024f-172">Abra el cuadro de diálogo **Opciones del proyecto** > **Compilar** > **Compilador**.</span><span class="sxs-lookup"><span data-stu-id="c024f-172">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="c024f-173">Seleccione la casilla **Generar documentación XML** en la sección **Opciones generales**:</span><span class="sxs-lookup"><span data-stu-id="c024f-173">Check the **Generate xml documentation** box under the **General Options** section:</span></span>

![Sección Opciones generales de las opciones del proyecto](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

#### <a name="visual-studio-codetabvisual-studio-code-xml"></a>[<span data-ttu-id="c024f-175">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c024f-175">Visual Studio Code</span></span>](#tab/visual-studio-code-xml/)
<span data-ttu-id="c024f-176">Agregue manualmente el siguiente fragmento de código al archivo *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="c024f-176">Manually add the following snippet to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.NSwag/TodoApiNSwag.csproj?range=7-9)]

* * *
## <a name="data-annotations"></a><span data-ttu-id="c024f-177">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="c024f-177">Data annotations</span></span>

<span data-ttu-id="c024f-178">NSwag usa [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), y el procedimiento recomendado para las acciones de Web API consiste en devolver [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult).</span><span class="sxs-lookup"><span data-stu-id="c024f-178">NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the best practice for Web API actions is to return [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult).</span></span> <span data-ttu-id="c024f-179">Por tanto, NSwag no puede deducir lo que la acción realiza y lo que devuelve.</span><span class="sxs-lookup"><span data-stu-id="c024f-179">Consequently, NSwag can't infer what your action is doing and what it returns.</span></span> <span data-ttu-id="c024f-180">Considere el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="c024f-180">Consider the following example:</span></span>

```csharp
public IActionResult Create([FromBody] TodoItem item)
{
    if (item == null)
    {
        return BadRequest();
    }

    _context.TodoItems.Add(item);
    _context.SaveChanges();

     return CreatedAtRoute("GetTodo", new { id = item.Id }, item);
}
```

<span data-ttu-id="c024f-181">La acción anterior devuelve `IActionResult` pero, dentro de la acción, devuelve [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) o [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest).</span><span class="sxs-lookup"><span data-stu-id="c024f-181">The preceding action returns `IActionResult`, but inside the action it's returning either [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) or [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest).</span></span> <span data-ttu-id="c024f-182">Las anotaciones de datos sirven para indicar a los clientes qué respuesta HTTP devuelve esta acción.</span><span class="sxs-lookup"><span data-stu-id="c024f-182">Data annotations are used to tell clients which HTTP response this action is returning.</span></span> <span data-ttu-id="c024f-183">Complete la acción con los siguientes atributos:</span><span class="sxs-lookup"><span data-stu-id="c024f-183">Decorate the action with the following attributes:</span></span>

```csharp
[HttpPost]
[ProducesResponseType(typeof(TodoItem), 201)] // Created
[ProducesResponseType(typeof(TodoItem), 400)] // BadRequest
public IActionResult Create([FromBody] TodoItem item)
```

<span data-ttu-id="c024f-184">Ahora, el generador de Swagger puede describir con precisión esta acción y los clientes generados sabrán lo que reciben cuando llamen al punto de conexión.</span><span class="sxs-lookup"><span data-stu-id="c024f-184">The Swagger generator can now accurately describe this action, and generated clients know what they receive when calling the endpoint.</span></span> <span data-ttu-id="c024f-185">Completar todas las acciones con estos atributos es tremendamente recomendable.</span><span class="sxs-lookup"><span data-stu-id="c024f-185">Decorating all actions with these attributes is highly recommended.</span></span> <span data-ttu-id="c024f-186">Para obtener instrucciones sobre qué respuestas HTTP deben devolver las acciones de API, vea la [especificación RFC 7231](https://tools.ietf.org/html/rfc7231#section-4.3).</span><span class="sxs-lookup"><span data-stu-id="c024f-186">For guidelines on what HTTP responses your API actions should return, see the [RFC 7231 specification](https://tools.ietf.org/html/rfc7231#section-4.3).</span></span>
