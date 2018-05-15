---
title: Introducción a Swashbuckle y ASP.NET Core
author: zuckerthoben
description: Obtenga información sobre cómo agregar Swashbuckle a un proyecto de ASP.NET Core para integrar la interfaz de usuario de Swagger.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 03/26/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/get-started-with-swashbuckle
ms.openlocfilehash: e90339f2884dd9b20cf135f879c9cab6110efecf
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-swashbuckle-and-aspnet-core"></a><span data-ttu-id="8d289-103">Introducción a Swashbuckle y ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8d289-103">Get started with Swashbuckle and ASP.NET Core</span></span>

<span data-ttu-id="8d289-104">Por [Shayne Boyer](https://twitter.com/spboyer) y [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="8d289-104">By [Shayne Boyer](https://twitter.com/spboyer) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="8d289-105">Hay tres componentes principales de Swashbuckle:</span><span class="sxs-lookup"><span data-stu-id="8d289-105">There are three main components to Swashbuckle:</span></span>

* <span data-ttu-id="8d289-106">[Swashbuckle.AspNetCore.Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/): modelo de objetos de Swagger y middleware para exponer objetos `SwaggerDocument` como puntos de conexión JSON.</span><span class="sxs-lookup"><span data-stu-id="8d289-106">[Swashbuckle.AspNetCore.Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/): a Swagger object model and middleware to expose `SwaggerDocument` objects as JSON endpoints.</span></span>

* <span data-ttu-id="8d289-107">[Swashbuckle.AspNetCore.SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/): generador de Swagger que genera objetos `SwaggerDocument` directamente desde las rutas, controladores y modelos.</span><span class="sxs-lookup"><span data-stu-id="8d289-107">[Swashbuckle.AspNetCore.SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/): a Swagger generator that builds `SwaggerDocument` objects directly from your routes, controllers, and models.</span></span> <span data-ttu-id="8d289-108">Se suele combinar con el middleware de punto de conexión de Swagger para exponer automáticamente el JSON de Swagger.</span><span class="sxs-lookup"><span data-stu-id="8d289-108">It's typically combined with the Swagger endpoint middleware to automatically expose Swagger JSON.</span></span>

* <span data-ttu-id="8d289-109">[Swashbuckle.AspNetCore.SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/): versión insertada de la herramienta de interfaz de usuario de Swagger.</span><span class="sxs-lookup"><span data-stu-id="8d289-109">[Swashbuckle.AspNetCore.SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/): an embedded version of the Swagger UI tool.</span></span> <span data-ttu-id="8d289-110">Interpreta el JSON de Swagger para crear una experiencia enriquecida y personalizable para describir la funcionalidad de la Web API.</span><span class="sxs-lookup"><span data-stu-id="8d289-110">It interprets Swagger JSON to build a rich, customizable experience for describing the Web API functionality.</span></span> <span data-ttu-id="8d289-111">Incluye herramientas de ejecución de pruebas integradas para los métodos públicos.</span><span class="sxs-lookup"><span data-stu-id="8d289-111">It includes built-in test harnesses for the public methods.</span></span>

## <a name="package-installation"></a><span data-ttu-id="8d289-112">Instalación del paquete</span><span class="sxs-lookup"><span data-stu-id="8d289-112">Package installation</span></span>

<span data-ttu-id="8d289-113">Se puede agregar Swashbuckle con los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="8d289-113">Swashbuckle can be added with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8d289-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8d289-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="8d289-115">En la ventana **Consola del Administrador de paquetes**:</span><span class="sxs-lookup"><span data-stu-id="8d289-115">From the **Package Manager Console** window:</span></span>

    ```powershell
    Install-Package Swashbuckle.AspNetCore
    ```

* <span data-ttu-id="8d289-116">En el cuadro de diálogo **Administrar paquetes NuGet**:</span><span class="sxs-lookup"><span data-stu-id="8d289-116">From the **Manage NuGet Packages** dialog:</span></span>

  * <span data-ttu-id="8d289-117">Haga clic con el botón derecho en el proyecto en el **Explorador de soluciones** > **Administrar paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="8d289-117">Right-click your project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="8d289-118">Establezca el **origen del paquete** en "nuget.org".</span><span class="sxs-lookup"><span data-stu-id="8d289-118">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="8d289-119">Escriba "Swashbuckle.AspNetCore" en el cuadro de búsqueda.</span><span class="sxs-lookup"><span data-stu-id="8d289-119">Enter "Swashbuckle.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="8d289-120">Seleccione el paquete "Swashbuckle.AspNetCore" en la pestaña **Examinar** y haga clic en **Instalar**.</span><span class="sxs-lookup"><span data-stu-id="8d289-120">Select the "Swashbuckle.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="8d289-121">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="8d289-121">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="8d289-122">Haga clic con el botón derecho en la carpeta *Paquetes* en **Panel de solución** > **Agregar paquetes...**</span><span class="sxs-lookup"><span data-stu-id="8d289-122">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="8d289-123">Establezca el menú desplegable **Origen** de la ventana **Agregar paquetes** en "nuget.org".</span><span class="sxs-lookup"><span data-stu-id="8d289-123">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="8d289-124">Escriba Swashbuckle.AspNetCore en el cuadro de búsqueda.</span><span class="sxs-lookup"><span data-stu-id="8d289-124">Enter Swashbuckle.AspNetCore in the search box</span></span>
* <span data-ttu-id="8d289-125">Seleccione el paquete Swashbuckle.AspNetCore en el panel de resultados y haga clic en **Agregar paquete**.</span><span class="sxs-lookup"><span data-stu-id="8d289-125">Select the Swashbuckle.AspNetCore package from the results pane and click **Add Package**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8d289-126">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8d289-126">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="8d289-127">Ejecute el siguiente comando en el **terminal integrado**:</span><span class="sxs-lookup"><span data-stu-id="8d289-127">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="8d289-128">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="8d289-128">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="8d289-129">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="8d289-129">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="8d289-130">Agregar y configurar el middleware de Swagger</span><span class="sxs-lookup"><span data-stu-id="8d289-130">Add and configure Swagger middleware</span></span>

<span data-ttu-id="8d289-131">Agregue el generador de Swagger a la colección de servicios en el método `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="8d289-131">Add the Swagger generator to the services collection in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=7-10)]

<span data-ttu-id="8d289-132">Importe el siguiente espacio de nombres para que use la clase `Info`:</span><span class="sxs-lookup"><span data-stu-id="8d289-132">Import the following namespace to use the `Info` class:</span></span>

```csharp
using Swashbuckle.AspNetCore.Swagger;
```

<span data-ttu-id="8d289-133">En el método `Startup.Configure`, habilite el middleware para servir el documento JSON generado y la interfaz de usuario de Swagger:</span><span class="sxs-lookup"><span data-stu-id="8d289-133">In the `Startup.Configure` method, enable the middleware for serving the generated JSON document and the Swagger UI:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Startup2.cs?name=snippet_Configure&highlight=4,7-10)]

<span data-ttu-id="8d289-134">Inicie la aplicación y vaya a `http://localhost:<random_port>/swagger/v1/swagger.json`.</span><span class="sxs-lookup"><span data-stu-id="8d289-134">Launch the app, and navigate to `http://localhost:<random_port>/swagger/v1/swagger.json`.</span></span> <span data-ttu-id="8d289-135">El documento generado en el que se describen los puntos de conexión aparecerá según se muestra en la [especificación de Swagger (swagger.json)](xref:tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson).</span><span class="sxs-lookup"><span data-stu-id="8d289-135">The generated document describing the endpoints appears as shown in [Swagger specification (swagger.json)](xref:tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson).</span></span>

<span data-ttu-id="8d289-136">La interfaz de usuario de Swagger se encuentra en `http://localhost:<random_port>/swagger`.</span><span class="sxs-lookup"><span data-stu-id="8d289-136">The Swagger UI can be found at `http://localhost:<random_port>/swagger`.</span></span> <span data-ttu-id="8d289-137">Explore la API a través de la interfaz de usuario de Swagger e incorpórela a otros programas.</span><span class="sxs-lookup"><span data-stu-id="8d289-137">Explore the API via Swagger UI and incorporate it in other programs.</span></span>

> [!TIP]
> <span data-ttu-id="8d289-138">Para servir la interfaz de usuario de Swagger en la raíz de la aplicación (`http://localhost:<random_port>/`), establezca la propiedad `RoutePrefix` en una cadena vacía:</span><span class="sxs-lookup"><span data-stu-id="8d289-138">To serve the Swagger UI at the app's root (`http://localhost:<random_port>/`), set the `RoutePrefix` property to an empty string:</span></span>
> 
> [!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Startup3.cs?name=snippet_UseSwaggerUI&highlight=4)]

## <a name="customize--extend"></a><span data-ttu-id="8d289-139">Personalizar y ampliar</span><span class="sxs-lookup"><span data-stu-id="8d289-139">Customize & extend</span></span>

<span data-ttu-id="8d289-140">Swagger proporciona opciones para documentar el modelo de objetos y personalizar la interfaz de usuario para que coincida con el tema.</span><span class="sxs-lookup"><span data-stu-id="8d289-140">Swagger provides options for documenting the object model and customizing the UI to match your theme.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="8d289-141">Información y descripción de la API</span><span class="sxs-lookup"><span data-stu-id="8d289-141">API info and description</span></span>

<span data-ttu-id="8d289-142">La acción de configuración que se pasa al método `AddSwaggerGen` agrega información, como el autor, la licencia y la descripción:</span><span class="sxs-lookup"><span data-stu-id="8d289-142">The configuration action passed to the `AddSwaggerGen` method adds information such as the author, license, and description:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Startup.cs?range=21-40,46)]

<span data-ttu-id="8d289-143">La interfaz de usuario de Swagger muestra información de la versión:</span><span class="sxs-lookup"><span data-stu-id="8d289-143">The Swagger UI displays the version's information:</span></span>

![Interfaz de usuario de Swagger con información de la versión: descripción, autor y el vínculo See more (Ver más)](web-api-help-pages-using-swagger/_static/custom-info.png)

### <a name="xml-comments"></a><span data-ttu-id="8d289-145">comentarios XML</span><span class="sxs-lookup"><span data-stu-id="8d289-145">XML comments</span></span>

<span data-ttu-id="8d289-146">Los comentarios XML se pueden habilitar con los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="8d289-146">XML comments can be enabled with the following approaches:</span></span>

#### <a name="visual-studiotabvisual-studio-xml"></a>[<span data-ttu-id="8d289-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8d289-147">Visual Studio</span></span>](#tab/visual-studio-xml/)
* <span data-ttu-id="8d289-148">En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Propiedades**.</span><span class="sxs-lookup"><span data-stu-id="8d289-148">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="8d289-149">Seleccione la casilla **Archivo de documentación XML** en la sección **Salida** de la pestaña **Compilar**:</span><span class="sxs-lookup"><span data-stu-id="8d289-149">Check the **XML documentation file** box under the **Output** section of the **Build** tab:</span></span>

![Pestaña Compilar de las propiedades del proyecto](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

#### <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[<span data-ttu-id="8d289-151">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="8d289-151">Visual Studio for Mac</span></span>](#tab/visual-studio-mac-xml/)
* <span data-ttu-id="8d289-152">Abra el cuadro de diálogo **Opciones del proyecto** > **Compilar** > **Compilador**.</span><span class="sxs-lookup"><span data-stu-id="8d289-152">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="8d289-153">Seleccione la casilla **Generar documentación XML** en la sección **Opciones generales**:</span><span class="sxs-lookup"><span data-stu-id="8d289-153">Check the **Generate xml documentation** box under the **General Options** section:</span></span>

![Sección Opciones generales de las opciones del proyecto](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

#### <a name="visual-studio-codetabvisual-studio-code-xml"></a>[<span data-ttu-id="8d289-155">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8d289-155">Visual Studio Code</span></span>](#tab/visual-studio-code-xml/)
<span data-ttu-id="8d289-156">Agregue manualmente el siguiente fragmento de código al archivo *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="8d289-156">Manually add the following snippet to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=2)]

* * *
<span data-ttu-id="8d289-157">Puede habilitar los comentarios XML para proporcionar información de depuración para miembros y tipos públicos sin documentación.</span><span class="sxs-lookup"><span data-stu-id="8d289-157">Enabling XML comments provides debug information for undocumented public types and members.</span></span> <span data-ttu-id="8d289-158">Los miembros y tipos que no estén documentados se indican por medio del mensaje de advertencia.</span><span class="sxs-lookup"><span data-stu-id="8d289-158">Undocumented types and members are indicated by the warning message.</span></span> <span data-ttu-id="8d289-159">Por ejemplo, el siguiente mensaje señala una infracción con el código de advertencia 1591:</span><span class="sxs-lookup"><span data-stu-id="8d289-159">For example, the following message indicates a violation of warning code 1591:</span></span>

```text
warning CS1591: Missing XML comment for publicly visible type or member 'TodoController.GetAll()'
```

<span data-ttu-id="8d289-160">Para eliminar las advertencias, defina una lista delimitada por punto y coma de códigos de advertencia que deban omitirse el archivo *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="8d289-160">Suppress warnings by defining a semicolon-delimited list of warning codes to ignore in the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=3)]

<span data-ttu-id="8d289-161">Configure Swagger para usar el archivo XML generado.</span><span class="sxs-lookup"><span data-stu-id="8d289-161">Configure Swagger to use the generated XML file.</span></span> <span data-ttu-id="8d289-162">En Linux o sistemas operativos que no sean Windows, las rutas de acceso y los nombres de archivo pueden distinguir entre mayúsculas y minúsculas.</span><span class="sxs-lookup"><span data-stu-id="8d289-162">For Linux or non-Windows operating systems, file names and paths can be case-sensitive.</span></span> <span data-ttu-id="8d289-163">Por ejemplo, un archivo *TodoApi.XML* es válido en Windows, pero no en CentOS.</span><span class="sxs-lookup"><span data-stu-id="8d289-163">For example, a *TodoApi.XML* file is valid on Windows but not CentOS.</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=29-31)]

<span data-ttu-id="8d289-164">En el código anterior, [Reflection](/dotnet/csharp/programming-guide/concepts/reflection) se usa para crear un nombre de archivo XML que coincida con el proyecto de Web API.</span><span class="sxs-lookup"><span data-stu-id="8d289-164">In the preceding code, [Reflection](/dotnet/csharp/programming-guide/concepts/reflection) is used to build an XML file name matching that of the Web API project.</span></span> <span data-ttu-id="8d289-165">Con este método se garantiza que el nombre de archivo XML generado va a ser el mismo que el nombre del proyecto.</span><span class="sxs-lookup"><span data-stu-id="8d289-165">This approach ensures that the generated XML file name matches the project name.</span></span> <span data-ttu-id="8d289-166">La propiedad [AppContext.BaseDirectory](/dotnet/api/system.appcontext.basedirectory#System_AppContext_BaseDirectory) sirve para crear una ruta de acceso al archivo XML.</span><span class="sxs-lookup"><span data-stu-id="8d289-166">The [AppContext.BaseDirectory](/dotnet/api/system.appcontext.basedirectory#System_AppContext_BaseDirectory) property is used to construct a path to the XML file.</span></span>

<span data-ttu-id="8d289-167">Agregar comentarios con la triple barra diagonal a una acción mejora la interfaz de usuario de Swagger en tanto agrega una descripción de la acción al encabezado de la sección.</span><span class="sxs-lookup"><span data-stu-id="8d289-167">Adding triple-slash comments to an action enhances the Swagger UI by adding the description to the section header.</span></span> <span data-ttu-id="8d289-168">Agregue un elemento [\<summary>](/dotnet/csharp/programming-guide/xmldoc/summary) antes de la acción `Delete`:</span><span class="sxs-lookup"><span data-stu-id="8d289-168">Add a [\<summary>](/dotnet/csharp/programming-guide/xmldoc/summary) element above the `Delete` action:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Delete&highlight=1-3)]

<span data-ttu-id="8d289-169">La interfaz de usuario de Swagger muestra el texto interno del elemento `<summary>` del código anterior:</span><span class="sxs-lookup"><span data-stu-id="8d289-169">The Swagger UI displays the inner text of the preceding code's `<summary>` element:</span></span>

![Interfaz de usuario de Swagger en la que se muestra el comentario XML "Deletes a specific TodoItem" (Elimina una tarea pendiente específica)](web-api-help-pages-using-swagger/_static/triple-slash-comments.png)

<span data-ttu-id="8d289-172">La interfaz de usuario se controla por medio del esquema JSON generado:</span><span class="sxs-lookup"><span data-stu-id="8d289-172">The UI is driven by the generated JSON schema:</span></span>

```json
"delete": {
    "tags": [
        "Todo"
    ],
    "summary": "Deletes a specific TodoItem.",
    "operationId": "ApiTodoByIdDelete",
    "consumes": [],
    "produces": [],
    "parameters": [
        {
            "name": "id",
            "in": "path",
            "description": "",
            "required": true,
            "type": "integer",
            "format": "int64"
        }
    ],
    "responses": {
        "200": {
            "description": "Success"
        }
    }
}
```

<span data-ttu-id="8d289-173">Agregue un elemento [\<remarks>](/dotnet/csharp/programming-guide/xmldoc/remarks) a la documentación del método de acción `Create`.</span><span class="sxs-lookup"><span data-stu-id="8d289-173">Add a [\<remarks>](/dotnet/csharp/programming-guide/xmldoc/remarks) element to the `Create` action method documentation.</span></span> <span data-ttu-id="8d289-174">Este elemento complementa la información especificada en el elemento `<summary>` y proporciona una interfaz de usuario de Swagger más sólida.</span><span class="sxs-lookup"><span data-stu-id="8d289-174">It supplements information specified in the `<summary>` element and provides a more robust Swagger UI.</span></span> <span data-ttu-id="8d289-175">El contenido del elemento `<remarks>` puede estar formado por texto, JSON o XML.</span><span class="sxs-lookup"><span data-stu-id="8d289-175">The `<remarks>` element content can consist of text, JSON, or XML.</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

<span data-ttu-id="8d289-176">Fíjese en las mejoras de la interfaz de usuario con estos comentarios extra:</span><span class="sxs-lookup"><span data-stu-id="8d289-176">Notice the UI enhancements with these additional comments:</span></span>

![Interfaz de usuario de Swagger con comentarios adicionales](web-api-help-pages-using-swagger/_static/xml-comments-extended.png)

### <a name="data-annotations"></a><span data-ttu-id="8d289-178">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="8d289-178">Data annotations</span></span>

<span data-ttu-id="8d289-179">Incorpore al modelo atributos que se encuentren en [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) para controlar los componentes de la interfaz de usuario de Swagger.</span><span class="sxs-lookup"><span data-stu-id="8d289-179">Decorate the model with attributes, found in the [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) namespace, to help drive the Swagger UI components.</span></span>

<span data-ttu-id="8d289-180">Agregue el atributo `[Required]` a la propiedad `Name` de la clase `TodoItem`:</span><span class="sxs-lookup"><span data-stu-id="8d289-180">Add the `[Required]` attribute to the `Name` property of the `TodoItem` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Models/TodoItem.cs?highlight=10)]

<span data-ttu-id="8d289-181">La presencia de este atributo cambia el comportamiento de la interfaz de usuario y modifica el esquema JSON subyacente:</span><span class="sxs-lookup"><span data-stu-id="8d289-181">The presence of this attribute changes the UI behavior and alters the underlying JSON schema:</span></span>

```json
"definitions": {
    "TodoItem": {
        "required": [
            "name"
        ],
        "type": "object",
        "properties": {
            "id": {
                "format": "int64",
                "type": "integer"
            },
            "name": {
                "type": "string"
            },
            "isComplete": {
                "default": false,
                "type": "boolean"
            }
        }
    }
},
```

<span data-ttu-id="8d289-182">Agregue el atributo `[Produces("application/json")]` al controlador de API.</span><span class="sxs-lookup"><span data-stu-id="8d289-182">Add the `[Produces("application/json")]` attribute to the API controller.</span></span> <span data-ttu-id="8d289-183">Su propósito consiste en declarar que las acciones del controlador admiten un contenido de respuesta de tipo *application/json*:</span><span class="sxs-lookup"><span data-stu-id="8d289-183">Its purpose is to declare that the controller's actions support a response content type of *application/json*:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=3)]

<span data-ttu-id="8d289-184">En el menú desplegable **Response Content Type** (Tipo de contenido de respuesta) se selecciona este tipo de contenido como el valor predeterminado para las acciones GET del controlador:</span><span class="sxs-lookup"><span data-stu-id="8d289-184">The **Response Content Type** drop-down selects this content type as the default for the controller's GET actions:</span></span>

![Interfaz de usuario de Swagger con el tipo de contenido de respuesta predeterminado](web-api-help-pages-using-swagger/_static/json-response-content-type.png)

<span data-ttu-id="8d289-186">A medida que aumenta el uso de anotaciones de datos en la API web, la interfaz de usuario y las páginas de ayuda de la API pasan a ser más descriptivas y útiles.</span><span class="sxs-lookup"><span data-stu-id="8d289-186">As the usage of data annotations in the Web API increases, the UI and API help pages become more descriptive and useful.</span></span>

### <a name="describe-response-types"></a><span data-ttu-id="8d289-187">Describir los tipos de respuesta</span><span class="sxs-lookup"><span data-stu-id="8d289-187">Describe response types</span></span>

<span data-ttu-id="8d289-188">Lo que más preocupa a los desarrolladores es lo que se devuelve; sobre todo, los tipos de respuesta y los códigos de error (si no son los habituales).</span><span class="sxs-lookup"><span data-stu-id="8d289-188">Consuming developers are most concerned with what's returned&mdash;specifically response types and error codes (if not standard).</span></span> <span data-ttu-id="8d289-189">Los tipos de respuesta y los códigos de error se indican en las anotaciones de datos y los comentarios XML.</span><span class="sxs-lookup"><span data-stu-id="8d289-189">The response types and error codes are denoted in the XML comments and data annotations.</span></span>

<span data-ttu-id="8d289-190">La acción `Create` devuelve un código de estado HTTP 201 cuando se ha ejecutado correctamente.</span><span class="sxs-lookup"><span data-stu-id="8d289-190">The `Create` action returns an HTTP 201 status code on success.</span></span> <span data-ttu-id="8d289-191">Cuando el cuerpo de solicitud enviado es null, se devuelve un código de estado HTTP 400.</span><span class="sxs-lookup"><span data-stu-id="8d289-191">An HTTP 400 status code is returned when the posted request body is null.</span></span> <span data-ttu-id="8d289-192">Sin la documentación correcta en la interfaz de usuario de Swagger, el consumidor no dispone de la información necesaria de estos resultados esperados.</span><span class="sxs-lookup"><span data-stu-id="8d289-192">Without proper documentation in the Swagger UI, the consumer lacks knowledge of these expected outcomes.</span></span> <span data-ttu-id="8d289-193">Corrija este problema agregando las líneas resaltadas en el siguiente ejemplo:</span><span class="sxs-lookup"><span data-stu-id="8d289-193">Fix that problem by adding the highlighted lines in the following example:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

<span data-ttu-id="8d289-194">Ahora, la interfaz de usuario de Swagger documenta de forma clara los códigos de respuesta HTTP esperados:</span><span class="sxs-lookup"><span data-stu-id="8d289-194">The Swagger UI now clearly documents the expected HTTP response codes:</span></span>

![Interfaz de usuario de Swagger, donde se muestra la descripción de la clase de respuesta POST "Returns the newly created item" (Devuelve el elemento recién creado) y "400 - If the item is null" (400 - Si el elemento es nulo) para el código de estado y el motivo en Response Messages (Mensajes de respuesta)](web-api-help-pages-using-swagger/_static/data-annotations-response-types.png)

### <a name="customize-the-ui"></a><span data-ttu-id="8d289-196">Personalizar la interfaz de usuario</span><span class="sxs-lookup"><span data-stu-id="8d289-196">Customize the UI</span></span>

<span data-ttu-id="8d289-197">La interfaz de usuario es funcional y tiene un aspecto adecuado.</span><span class="sxs-lookup"><span data-stu-id="8d289-197">The stock UI is both functional and presentable.</span></span> <span data-ttu-id="8d289-198">Pero las páginas de documentación de la API deben ostentar su marca o tema.</span><span class="sxs-lookup"><span data-stu-id="8d289-198">However, API documentation pages should represent your brand or theme.</span></span> <span data-ttu-id="8d289-199">Para incluir la personalización de marca en los componentes de Swashbuckle, se deben agregar los recursos para servir archivos estáticos y generar la estructura de carpetas que hospedará estos archivos.</span><span class="sxs-lookup"><span data-stu-id="8d289-199">Branding the Swashbuckle components requires adding the resources to serve static files and building the folder structure to host those files.</span></span>

<span data-ttu-id="8d289-200">Si el destino es .NET Framework o .NET Core 1.x, agregue el paquete NuGet [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles) al proyecto:</span><span class="sxs-lookup"><span data-stu-id="8d289-200">If targeting .NET Framework or .NET Core 1.x, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles) NuGet package to the project:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="2.0.0" />
```

<span data-ttu-id="8d289-201">El paquete NuGet anterior ya estará instalado si el destino es .NET Core 2.x y se usa el [metapaquete](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="8d289-201">The preceding NuGet package is already installed if targeting .NET Core 2.x and using the [metapackage](xref:fundamentals/metapackage).</span></span>

<span data-ttu-id="8d289-202">Habilite el middleware de los archivos estáticos:</span><span class="sxs-lookup"><span data-stu-id="8d289-202">Enable the static files middleware:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Startup.cs?name=snippet_Configure&highlight=3)]

<span data-ttu-id="8d289-203">Obtenga el contenido de la carpeta *dist* en el [repositorio de GitHub de la interfaz de usuario de Swagger](https://github.com/swagger-api/swagger-ui/tree/master/dist).</span><span class="sxs-lookup"><span data-stu-id="8d289-203">Acquire the contents of the *dist* folder from the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui/tree/master/dist).</span></span> <span data-ttu-id="8d289-204">Esta carpeta contiene los recursos necesarios para la página de interfaz de usuario de Swagger.</span><span class="sxs-lookup"><span data-stu-id="8d289-204">This folder contains the necessary assets for the Swagger UI page.</span></span>

<span data-ttu-id="8d289-205">Cree una carpeta *wwwroot/swagger/ui* y copie en ella el contenido de la carpeta *dist*.</span><span class="sxs-lookup"><span data-stu-id="8d289-205">Create a *wwwroot/swagger/ui* folder, and copy into it the contents of the *dist* folder.</span></span>

<span data-ttu-id="8d289-206">Cree un archivo *custom.css* en *wwwroot/swagger/ui* con el siguiente código CSS para personalizar el encabezado de página:</span><span class="sxs-lookup"><span data-stu-id="8d289-206">Create a *custom.css* file, in *wwwroot/swagger/ui*, with the following CSS to customize the page header:</span></span>

[!code-css[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/wwwroot/swagger/ui/custom.css)]

<span data-ttu-id="8d289-207">Cree una referencia a *custom.css* en el archivo *index.html* después de cualquier otro archivo CSS:</span><span class="sxs-lookup"><span data-stu-id="8d289-207">Reference *custom.css* in the *index.html* file, after any other CSS files:</span></span>

[!code-html[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/wwwroot/swagger/ui/index.html?name=snippet_SwaggerUiCss&highlight=3)]

<span data-ttu-id="8d289-208">Vaya a la página *index.html* en `http://localhost:<random_port>/swagger/ui/index.html`.</span><span class="sxs-lookup"><span data-stu-id="8d289-208">Browse to the *index.html* page at `http://localhost:<random_port>/swagger/ui/index.html`.</span></span> <span data-ttu-id="8d289-209">Escriba `http://localhost:<random_port>/swagger/v1/swagger.json` en el cuadro de texto del encabezado y haga clic en el botón **Explorar**.</span><span class="sxs-lookup"><span data-stu-id="8d289-209">Enter `http://localhost:<random_port>/swagger/v1/swagger.json` in the header's textbox, and click the **Explore** button.</span></span> <span data-ttu-id="8d289-210">La página resultante tiene el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="8d289-210">The resulting page looks as follows:</span></span>

![Interfaz de usuario de Swagger con el título de encabezado personalizado](web-api-help-pages-using-swagger/_static/custom-header.png)

<span data-ttu-id="8d289-212">Puede hacer muchas más cosas con la página.</span><span class="sxs-lookup"><span data-stu-id="8d289-212">There's much more you can do with the page.</span></span> <span data-ttu-id="8d289-213">Vea todas las capacidades de los recursos de la interfaz de usuario en el [repositorio de GitHub de la interfaz de usuario de Swagger](https://github.com/swagger-api/swagger-ui).</span><span class="sxs-lookup"><span data-stu-id="8d289-213">See the full capabilities for the UI resources at the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui).</span></span>
