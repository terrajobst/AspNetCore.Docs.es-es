---
title: "Páginas de ayuda de ASP.NET Core Web API mediante Swagger"
author: spboyer
description: "En este tutorial se proporciona una guía sobre cómo incorporar Swagger para generar documentación y páginas de ayuda para una aplicación de API web."
manager: wpickett
ms.author: spboyer
ms.date: 09/01/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/web-api-help-pages-using-swagger
ms.openlocfilehash: 911504d9472ae78a0d1d002f1feb57f3a160d5bf
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="aspnet-core-web-api-help-pages-using-swagger"></a><span data-ttu-id="592c5-103">Páginas de ayuda de ASP.NET Core Web API mediante Swagger</span><span class="sxs-lookup"><span data-stu-id="592c5-103">ASP.NET Core Web API Help Pages using Swagger</span></span>

<a name="web-api-help-pages-using-swagger"></a>

<span data-ttu-id="592c5-104">Por [Shayne Boyer](https://twitter.com/spboyer) y [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="592c5-104">By [Shayne Boyer](https://twitter.com/spboyer) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="592c5-105">Comprender los distintos métodos de una API puede representar un reto para un desarrollador a la hora de compilar una aplicación de consumo.</span><span class="sxs-lookup"><span data-stu-id="592c5-105">Understanding the various methods of an API can be a challenge for a developer when building a consuming application.</span></span>

<span data-ttu-id="592c5-106">Para generar páginas de ayuda y una documentación de calidad para la API web, por medio de [Swagger](https://swagger.io/) con la implementación de .NET Core [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore), lo único que tiene que hacer es agregar un par de paquetes de NuGet y modificar el archivo *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="592c5-106">Generating good documentation and help pages for your Web API, using [Swagger](https://swagger.io/) with the .NET Core implementation [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore), is as easy as adding a couple of NuGet packages and modifying the *Startup.cs*.</span></span>

* <span data-ttu-id="592c5-107">[Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) es un proyecto de código abierto para generar documentos de Swagger para las API web de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="592c5-107">[Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) is an open source project for generating Swagger documents for ASP.NET Core Web APIs.</span></span>

* <span data-ttu-id="592c5-108">[Swagger](https://swagger.io/) es una representación de lectura mecánica de una API de REST que admite la compatibilidad con la documentación interactiva, la generación de SDK de cliente y la detectabilidad.</span><span class="sxs-lookup"><span data-stu-id="592c5-108">[Swagger](https://swagger.io/) is a machine-readable representation of a RESTful API that enables support for interactive documentation, client SDK generation, and discoverability.</span></span>

<span data-ttu-id="592c5-109">Este tutorial se basa en el ejemplo de [Cree su primera API web con ASP.NET Core MVC y Visual Studio](xref:tutorials/first-web-api).</span><span class="sxs-lookup"><span data-stu-id="592c5-109">This tutorial builds on the sample on [Building Your First Web API with ASP.NET Core MVC and Visual Studio](xref:tutorials/first-web-api).</span></span> <span data-ttu-id="592c5-110">Si quiere seguir, descargue el ejemplo en [https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample).</span><span class="sxs-lookup"><span data-stu-id="592c5-110">If you'd like to follow along, download the sample at [https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample).</span></span>

## <a name="getting-started"></a><span data-ttu-id="592c5-111">Introducción</span><span class="sxs-lookup"><span data-stu-id="592c5-111">Getting Started</span></span>

<span data-ttu-id="592c5-112">Hay tres componentes principales de Swashbuckle:</span><span class="sxs-lookup"><span data-stu-id="592c5-112">There are three main components to Swashbuckle:</span></span>

* <span data-ttu-id="592c5-113">`Swashbuckle.AspNetCore.Swagger`: un modelo de objetos de Swagger y middleware para exponer objetos `SwaggerDocument` como puntos de conexión JSON.</span><span class="sxs-lookup"><span data-stu-id="592c5-113">`Swashbuckle.AspNetCore.Swagger`: a Swagger object model and middleware to expose `SwaggerDocument` objects as JSON endpoints.</span></span>

* <span data-ttu-id="592c5-114">`Swashbuckle.AspNetCore.SwaggerGen`: un generador de Swagger que genera objetos `SwaggerDocument` directamente de sus rutas, controladores y modelos.</span><span class="sxs-lookup"><span data-stu-id="592c5-114">`Swashbuckle.AspNetCore.SwaggerGen`: a Swagger generator that builds `SwaggerDocument` objects directly from your routes, controllers, and models.</span></span> <span data-ttu-id="592c5-115">Se suele combinar con el middleware de punto de conexión de Swagger para exponer automáticamente el JSON de Swagger.</span><span class="sxs-lookup"><span data-stu-id="592c5-115">It's typically combined with the Swagger endpoint middleware to automatically expose Swagger JSON.</span></span>

* <span data-ttu-id="592c5-116">`Swashbuckle.AspNetCore.SwaggerUI`: una versión insertada de la herramienta de interfaz de usuario de Swagger, que interpreta el JSON de Swagger para crear una experiencia enriquecida y personalizable para describir la funcionalidad de la API web.</span><span class="sxs-lookup"><span data-stu-id="592c5-116">`Swashbuckle.AspNetCore.SwaggerUI`: an embedded version of the Swagger UI tool which interprets Swagger JSON to build a rich, customizable experience for describing the Web API functionality.</span></span> <span data-ttu-id="592c5-117">Incluye herramientas de ejecución de pruebas integradas para los métodos públicos.</span><span class="sxs-lookup"><span data-stu-id="592c5-117">It includes built-in test harnesses for the public methods.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="592c5-118">Paquetes NuGet</span><span class="sxs-lookup"><span data-stu-id="592c5-118">NuGet Packages</span></span>

<span data-ttu-id="592c5-119">Se puede agregar Swashbuckle con los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="592c5-119">Swashbuckle can be added with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="592c5-120">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="592c5-120">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="592c5-121">En la ventana **Consola del Administrador de paquetes**:</span><span class="sxs-lookup"><span data-stu-id="592c5-121">From the **Package Manager Console** window:</span></span>

    ```powershell
    Install-Package Swashbuckle.AspNetCore
    ```

* <span data-ttu-id="592c5-122">En el cuadro de diálogo **Administrar paquetes NuGet**:</span><span class="sxs-lookup"><span data-stu-id="592c5-122">From the **Manage NuGet Packages** dialog:</span></span>

     * <span data-ttu-id="592c5-123">Haga clic con el botón derecho en el proyecto en el **Explorador de soluciones** > **Administrar paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="592c5-123">Right-click your project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
     * <span data-ttu-id="592c5-124">Establezca el **origen del paquete** en "nuget.org".</span><span class="sxs-lookup"><span data-stu-id="592c5-124">Set the **Package source** to "nuget.org"</span></span>
     * <span data-ttu-id="592c5-125">Escriba "Swashbuckle.AspNetCore" en el cuadro de búsqueda.</span><span class="sxs-lookup"><span data-stu-id="592c5-125">Enter "Swashbuckle.AspNetCore" in the search box</span></span>
     * <span data-ttu-id="592c5-126">Seleccione el paquete "Swashbuckle.AspNetCore" en la pestaña **Examinar** y haga clic en **Instalar**.</span><span class="sxs-lookup"><span data-stu-id="592c5-126">Select the "Swashbuckle.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="592c5-127">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="592c5-127">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="592c5-128">Haga clic con el botón derecho en la carpeta *Paquetes* en **Panel de solución** > **Agregar paquetes...**</span><span class="sxs-lookup"><span data-stu-id="592c5-128">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="592c5-129">Establezca el menú desplegable **Origen** de la ventana **Agregar paquetes** en "nuget.org".</span><span class="sxs-lookup"><span data-stu-id="592c5-129">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="592c5-130">Escriba Swashbuckle.AspNetCore en el cuadro de búsqueda.</span><span class="sxs-lookup"><span data-stu-id="592c5-130">Enter Swashbuckle.AspNetCore in the search box</span></span>
* <span data-ttu-id="592c5-131">Seleccione el paquete Swashbuckle.AspNetCore en el panel de resultados y haga clic en **Agregar paquete**.</span><span class="sxs-lookup"><span data-stu-id="592c5-131">Select the Swashbuckle.AspNetCore package from the results pane and click **Add Package**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="592c5-132">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="592c5-132">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="592c5-133">Ejecute el siguiente comando en el **terminal integrado**:</span><span class="sxs-lookup"><span data-stu-id="592c5-133">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="592c5-134">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="592c5-134">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="592c5-135">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="592c5-135">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

---

## <a name="add-and-configure-swagger-to-the-middleware"></a><span data-ttu-id="592c5-136">Agregar y configurar Swagger en el middleware</span><span class="sxs-lookup"><span data-stu-id="592c5-136">Add and configure Swagger to the middleware</span></span>

<span data-ttu-id="592c5-137">Agregue el generador de Swagger a la colección de servicios en el método `ConfigureServices` de *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="592c5-137">Add the Swagger generator to the services collection in the `ConfigureServices` method of *Startup.cs*:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup2.cs?name=snippet_ConfigureServices&highlight=7-10)]

<span data-ttu-id="592c5-138">Agregue la instrucción using siguiente para la clase `Info`:</span><span class="sxs-lookup"><span data-stu-id="592c5-138">Add the following using statement for the `Info` class:</span></span>

```csharp
using Swashbuckle.AspNetCore.Swagger;
```

<span data-ttu-id="592c5-139">En el método `Configure` de *Startup.cs*, habilite el middleware para servir el documento JSON generado y la IU de Swagger:</span><span class="sxs-lookup"><span data-stu-id="592c5-139">In the `Configure` method of *Startup.cs*, enable the middleware for serving the generated JSON document and the SwaggerUI:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup2.cs?name=snippet_Configure&highlight=4,7-10)]

<span data-ttu-id="592c5-140">Inicie la aplicación y vaya a `http://localhost:<random_port>/swagger/v1/swagger.json`.</span><span class="sxs-lookup"><span data-stu-id="592c5-140">Launch the app, and navigate to `http://localhost:<random_port>/swagger/v1/swagger.json`.</span></span> <span data-ttu-id="592c5-141">Aparecerá el documento generado en el que se describen los puntos de conexión.</span><span class="sxs-lookup"><span data-stu-id="592c5-141">The generated document describing the endpoints appears.</span></span>

<span data-ttu-id="592c5-142">**Nota:** En Microsoft Edge, Google Chrome y Firefox se muestran los documentos JSON de forma nativa.</span><span class="sxs-lookup"><span data-stu-id="592c5-142">**Note:** Microsoft Edge, Google Chrome, and Firefox display JSON documents natively.</span></span> <span data-ttu-id="592c5-143">Existen extensiones de Chrome que dan formato al documento para facilitar la lectura.</span><span class="sxs-lookup"><span data-stu-id="592c5-143">There are extensions for Chrome that format the document for easier reading.</span></span> <span data-ttu-id="592c5-144">*El siguiente ejemplo se ha reducido por motivos de brevedad.*</span><span class="sxs-lookup"><span data-stu-id="592c5-144">*The following example is reduced for brevity.*</span></span>

```json
{
   "swagger": "2.0",
   "info": {
       "version": "v1",
       "title": "API V1"
   },
   "basePath": "/",
   "paths": {
       "/api/Todo": {
           "get": {
               "tags": [
                   "Todo"
               ],
               "operationId": "ApiTodoGet",
               "consumes": [],
               "produces": [
                   "text/plain",
                   "application/json",
                   "text/json"
               ],
               "responses": {
                   "200": {
                       "description": "Success",
                       "schema": {
                           "type": "array",
                           "items": {
                               "$ref": "#/definitions/TodoItem"
                           }
                       }
                   }
                }
           },
           "post": {
               ...
           }
       },
       "/api/Todo/{id}": {
           "get": {
               ...
           },
           "put": {
               ...
           },
           "delete": {
               ...
   },
   "definitions": {
       "TodoItem": {
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
   "securityDefinitions": {}
}
```

<span data-ttu-id="592c5-145">Este documento le guía por la interfaz de usuario de Swagger, que se puede visualizar yendo a `http://localhost:<random_port>/swagger`:</span><span class="sxs-lookup"><span data-stu-id="592c5-145">This document drives the Swagger UI, which can be viewed by navigating to `http://localhost:<random_port>/swagger`:</span></span>

![Interfaz de usuario de Swagger](web-api-help-pages-using-swagger/_static/swagger-ui.png)

<span data-ttu-id="592c5-147">Todos los métodos de acción pública aplicados a `TodoController` se pueden probar desde la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="592c5-147">Each public action method in `TodoController` can be tested from the UI.</span></span> <span data-ttu-id="592c5-148">Haga clic en un nombre de método para expandir la sección.</span><span class="sxs-lookup"><span data-stu-id="592c5-148">Click a method name to expand the section.</span></span> <span data-ttu-id="592c5-149">Agregue todos los parámetros necesarios y haga clic en "Try it out!" (¡Pruébelo!).</span><span class="sxs-lookup"><span data-stu-id="592c5-149">Add any necessary parameters, and click "Try it out!".</span></span>

![Ejemplo de prueba de acción GET de Swagger](web-api-help-pages-using-swagger/_static/get-try-it-out.png)

## <a name="customization--extensibility"></a><span data-ttu-id="592c5-151">Personalización y extensibilidad</span><span class="sxs-lookup"><span data-stu-id="592c5-151">Customization & Extensibility</span></span>

<span data-ttu-id="592c5-152">Swagger proporciona opciones para documentar el modelo de objetos y personalizar la interfaz de usuario para que coincida con el tema.</span><span class="sxs-lookup"><span data-stu-id="592c5-152">Swagger provides options for documenting the object model and customizing the UI to match your theme.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="592c5-153">Información y descripción de la API</span><span class="sxs-lookup"><span data-stu-id="592c5-153">API Info and Description</span></span>

<span data-ttu-id="592c5-154">La acción de configuración que se pasa al método `AddSwaggerGen` se puede usar para agregar información, como el autor, la licencia y la descripción:</span><span class="sxs-lookup"><span data-stu-id="592c5-154">The configuration action passed to the `AddSwaggerGen` method can be used to add information such as the author, license, and description:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?range=20-30,36)]

<span data-ttu-id="592c5-155">En la siguiente imagen se muestra la interfaz de usuario de Swagger, donde aparece la información de la versión:</span><span class="sxs-lookup"><span data-stu-id="592c5-155">The following image depicts the Swagger UI displaying the version information:</span></span>

![Interfaz de usuario de Swagger con información de la versión: descripción, autor y el vínculo See more (Ver más)](web-api-help-pages-using-swagger/_static/custom-info.png)

### <a name="xml-comments"></a><span data-ttu-id="592c5-157">Comentarios XML</span><span class="sxs-lookup"><span data-stu-id="592c5-157">XML Comments</span></span>

<span data-ttu-id="592c5-158">Los comentarios XML se pueden habilitar con los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="592c5-158">XML comments can be enabled with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="592c5-159">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="592c5-159">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="592c5-160">En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Propiedades**.</span><span class="sxs-lookup"><span data-stu-id="592c5-160">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="592c5-161">Seleccione la casilla **Archivo de documentación XML** en la sección **Salida** de la pestaña **Compilar**:</span><span class="sxs-lookup"><span data-stu-id="592c5-161">Check the **XML documentation file** box under the **Output** section of the **Build** tab:</span></span>

![Pestaña Compilar de las propiedades del proyecto](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="592c5-163">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="592c5-163">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="592c5-164">Abra el cuadro de diálogo **Opciones del proyecto** > **Compilar** > **Compilador**.</span><span class="sxs-lookup"><span data-stu-id="592c5-164">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="592c5-165">Seleccione la casilla **Generar documentación XML** en la sección **Opciones generales**:</span><span class="sxs-lookup"><span data-stu-id="592c5-165">Check the **Generate xml documentation** box under the **General Options** section:</span></span>

![Sección Opciones generales de las opciones del proyecto](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="592c5-167">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="592c5-167">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="592c5-168">Agregue manualmente el siguiente fragmento de código al archivo *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="592c5-168">Manually add the following snippet to the *.csproj* file:</span></span>

[!code-xml[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/TodoApi.csproj?range=7-9)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="592c5-169">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="592c5-169">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="592c5-170">Vea Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="592c5-170">See Visual Studio Code.</span></span>

---

<span data-ttu-id="592c5-171">Puede habilitar los comentarios XML para proporcionar información de depuración para miembros y tipos públicos sin documentación.</span><span class="sxs-lookup"><span data-stu-id="592c5-171">Enabling XML comments provides debug information for undocumented public types and members.</span></span> <span data-ttu-id="592c5-172">Los miembros y tipos sin documentación se indican mediante el mensaje de advertencia: *Falta el comentario XML para el tipo o miembro visible públicamente*.</span><span class="sxs-lookup"><span data-stu-id="592c5-172">Undocumented types and members are indicated by the warning message: *Missing XML comment for publicly visible type or member*.</span></span>

<span data-ttu-id="592c5-173">Configure Swagger para usar el archivo XML generado.</span><span class="sxs-lookup"><span data-stu-id="592c5-173">Configure Swagger to use the generated XML file.</span></span> <span data-ttu-id="592c5-174">Para Linux o sistemas operativos que no sean Windows, las rutas de acceso y los nombres de archivo pueden distinguir entre mayúsculas y minúsculas.</span><span class="sxs-lookup"><span data-stu-id="592c5-174">For Linux or non-Windows operating systems, file names and paths can be case sensitive.</span></span> <span data-ttu-id="592c5-175">Por ejemplo, un archivo denominado *ToDoApi.XML* podría aparecer en Windows, pero no en CentOS.</span><span class="sxs-lookup"><span data-stu-id="592c5-175">For example, a *ToDoApi.XML* file would be found on Windows but not CentOS.</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?name=snippet_ConfigureServices&highlight=20-22)]

<span data-ttu-id="592c5-176">En el código anterior, `ApplicationBasePath` obtiene la ruta de acceso base de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="592c5-176">In the preceding code, `ApplicationBasePath` gets the base path of the app.</span></span> <span data-ttu-id="592c5-177">La ruta de acceso base se usa para buscar el archivo de comentarios XML.</span><span class="sxs-lookup"><span data-stu-id="592c5-177">The base path is used to locate the XML comments file.</span></span> <span data-ttu-id="592c5-178">*TodoApi.xml* solo funciona en este ejemplo, puesto que el nombre del archivo de comentarios XML generado se basa en el nombre de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="592c5-178">*TodoApi.xml* only works for this example, since the name of the generated XML comments file is based on the application name.</span></span>

<span data-ttu-id="592c5-179">Agregar los comentarios con la triple barra diagonal al método mejora la interfaz de usuario de Swagger mediante la adición de la descripción al encabezado de la sección:</span><span class="sxs-lookup"><span data-stu-id="592c5-179">Adding the triple-slash comments to the method enhances the Swagger UI by adding the description to the section header:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete&highlight=2)]

![Interfaz de usuario de Swagger en la que se muestra el comentario XML "Deletes a specific TodoItem" (Elimina una tarea pendiente específica)](web-api-help-pages-using-swagger/_static/triple-slash-comments.png)

<span data-ttu-id="592c5-182">La interfaz de usuario se controla con el archivo JSON generado, que también contiene estos comentarios:</span><span class="sxs-lookup"><span data-stu-id="592c5-182">The UI is driven by the generated JSON file, which also contains these comments:</span></span>

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

<span data-ttu-id="592c5-183">Agregue una etiqueta [<remarks>](https://docs.microsoft.com/dotnet/csharp/programming-guide/xmldoc/remarks) a la documentación del método de acción `Create`.</span><span class="sxs-lookup"><span data-stu-id="592c5-183">Add a [<remarks>](https://docs.microsoft.com/dotnet/csharp/programming-guide/xmldoc/remarks) tag to the `Create` action method documentation.</span></span> <span data-ttu-id="592c5-184">Complementa la información especificada en la etiqueta `<summary>` y proporciona una interfaz de usuario de Swagger más sólida.</span><span class="sxs-lookup"><span data-stu-id="592c5-184">It supplements information specified in the `<summary>` tag and provides a more robust Swagger UI.</span></span> <span data-ttu-id="592c5-185">El contenido de la etiqueta `<remarks>` puede estar formado por texto, JSON o XML.</span><span class="sxs-lookup"><span data-stu-id="592c5-185">The `<remarks>` tag content can consist of text, JSON, or XML.</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

<span data-ttu-id="592c5-186">Fíjese en las mejoras de la interfaz de usuario con estos comentarios adicionales.</span><span class="sxs-lookup"><span data-stu-id="592c5-186">Notice the UI enhancements with these additional comments.</span></span>

![Interfaz de usuario de Swagger con comentarios adicionales](web-api-help-pages-using-swagger/_static/xml-comments-extended.png)

### <a name="data-annotations"></a><span data-ttu-id="592c5-188">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="592c5-188">Data Annotations</span></span>

<span data-ttu-id="592c5-189">Incorpore al modelo atributos que se encuentran en `System.ComponentModel.DataAnnotations` para controlar los componentes de la interfaz de usuario de Swagger.</span><span class="sxs-lookup"><span data-stu-id="592c5-189">Decorate the model with attributes, found in `System.ComponentModel.DataAnnotations`, to help drive the Swagger UI components.</span></span>

<span data-ttu-id="592c5-190">Agregue el atributo `[Required]` a la propiedad `Name` de la clase `TodoItem`:</span><span class="sxs-lookup"><span data-stu-id="592c5-190">Add the `[Required]` attribute to the `Name` property of the `TodoItem` class:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Models/TodoItem.cs?highlight=10)]

<span data-ttu-id="592c5-191">La presencia de este atributo cambia el comportamiento de la interfaz de usuario y modifica el esquema JSON subyacente:</span><span class="sxs-lookup"><span data-stu-id="592c5-191">The presence of this attribute changes the UI behavior and alters the underlying JSON schema:</span></span>

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

<span data-ttu-id="592c5-192">Agregue el atributo `[Produces("application/json")]` al controlador de API.</span><span class="sxs-lookup"><span data-stu-id="592c5-192">Add the `[Produces("application/json")]` attribute to the API controller.</span></span> <span data-ttu-id="592c5-193">Su propósito consiste en declarar que las acciones del controlador admitan un tipo de contenido de *application/json*:</span><span class="sxs-lookup"><span data-stu-id="592c5-193">Its purpose is to declare that the controller's actions support a return a content type of *application/json*:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_TodoController&highlight=3)]

<span data-ttu-id="592c5-194">En el menú desplegable **Response Content Type** (Tipo de contenido de respuesta) se selecciona este tipo de contenido como el valor predeterminado para las acciones GET del controlador:</span><span class="sxs-lookup"><span data-stu-id="592c5-194">The **Response Content Type** drop-down selects this content type as the default for the controller's GET actions:</span></span>

![Interfaz de usuario de Swagger con el tipo de contenido de respuesta predeterminado](web-api-help-pages-using-swagger/_static/json-response-content-type.png)

<span data-ttu-id="592c5-196">A medida que aumenta el uso de anotaciones de datos en la API web, la interfaz de usuario y las páginas de ayuda de la API pasan a ser más descriptivas y útiles.</span><span class="sxs-lookup"><span data-stu-id="592c5-196">As the usage of data annotations in the Web API increases, the UI and API help pages become more descriptive and useful.</span></span>

### <a name="describing-response-types"></a><span data-ttu-id="592c5-197">Describir los tipos de respuesta</span><span class="sxs-lookup"><span data-stu-id="592c5-197">Describing Response Types</span></span>

<span data-ttu-id="592c5-198">Lo que más preocupa a los desarrolladores de consumo es lo que se devuelve, sobre todo los tipos de respuesta y los códigos de error (si no son los habituales).</span><span class="sxs-lookup"><span data-stu-id="592c5-198">Consuming developers are most concerned with what is returned &mdash; specifically response types and error codes (if not standard).</span></span> <span data-ttu-id="592c5-199">Estos se administran en las anotaciones de datos y en los comentarios XML.</span><span class="sxs-lookup"><span data-stu-id="592c5-199">These are handled in the XML comments and data annotations.</span></span>

<span data-ttu-id="592c5-200">La acción `Create` devuelve `201 Created` en caso de éxito o `400 Bad Request` si el cuerpo de solicitud publicado es nulo.</span><span class="sxs-lookup"><span data-stu-id="592c5-200">The `Create` action returns `201 Created` on success or `400 Bad Request` when the posted request body is null.</span></span> <span data-ttu-id="592c5-201">Sin la documentación correcta en la interfaz de usuario de Swagger, el consumidor no dispone de la información necesaria de estos resultados esperados.</span><span class="sxs-lookup"><span data-stu-id="592c5-201">Without proper documentation in the Swagger UI, the consumer lacks knowledge of these expected outcomes.</span></span> <span data-ttu-id="592c5-202">El problema se corrige agregando las líneas resaltadas en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="592c5-202">That problem is fixed by adding the highlighted lines in the following example:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

<span data-ttu-id="592c5-203">Ahora, la interfaz de usuario de Swagger documenta de forma clara los códigos de respuesta HTTP esperados:</span><span class="sxs-lookup"><span data-stu-id="592c5-203">The Swagger UI now clearly documents the expected HTTP response codes:</span></span>

![Interfaz de usuario de Swagger, donde se muestra la descripción de la clase de respuesta POST "Returns the newly created item" (Devuelve el elemento recién creado) y "400 - If the item is null" (400 - Si el elemento es nulo) para el código de estado y el motivo en Response Messages (Mensajes de respuesta)](web-api-help-pages-using-swagger/_static/data-annotations-response-types.png)

### <a name="customizing-the-ui"></a><span data-ttu-id="592c5-205">Personalizar la interfaz de usuario</span><span class="sxs-lookup"><span data-stu-id="592c5-205">Customizing the UI</span></span>

<span data-ttu-id="592c5-206">La interfaz de usuario es funcional y clara pero, a la hora de compilar las páginas de documentación de la API, quiere que represente su marca o tema.</span><span class="sxs-lookup"><span data-stu-id="592c5-206">The stock UI is both functional and presentable; however, when building documentation pages for your API, you want it to represent your brand or theme.</span></span> <span data-ttu-id="592c5-207">Para llevar a cabo esta tarea con los componentes de Swashbuckle, se deben agregar los recursos para servir archivos estáticos y, luego, generar la estructura de carpetas que hospedará estos archivos.</span><span class="sxs-lookup"><span data-stu-id="592c5-207">Accomplishing that task with the Swashbuckle components requires adding the resources to serve static files and then building the folder structure to host those files.</span></span>

<span data-ttu-id="592c5-208">Si el destino es .NET Framework, agregue el paquete NuGet `Microsoft.AspNetCore.StaticFiles` al proyecto:</span><span class="sxs-lookup"><span data-stu-id="592c5-208">If targeting .NET Framework, add the `Microsoft.AspNetCore.StaticFiles` NuGet package to the project:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="2.0.0" />
```

<span data-ttu-id="592c5-209">Habilite el middleware de los archivos estáticos:</span><span class="sxs-lookup"><span data-stu-id="592c5-209">Enable the static files middleware:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?name=snippet_Configure&highlight=3)]

<span data-ttu-id="592c5-210">Obtenga el contenido de la carpeta *dist* en el [repositorio de GitHub de la interfaz de usuario de Swagger](https://github.com/swagger-api/swagger-ui/tree/2.x/dist).</span><span class="sxs-lookup"><span data-stu-id="592c5-210">Acquire the contents of the *dist* folder from the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui/tree/2.x/dist).</span></span> <span data-ttu-id="592c5-211">Esta carpeta contiene los recursos necesarios para la página de interfaz de usuario de Swagger.</span><span class="sxs-lookup"><span data-stu-id="592c5-211">This folder contains the necessary assets for the Swagger UI page.</span></span>

<span data-ttu-id="592c5-212">Cree una carpeta *wwwroot/swagger/ui* y copie en ella el contenido de la carpeta *dist*.</span><span class="sxs-lookup"><span data-stu-id="592c5-212">Create a *wwwroot/swagger/ui* folder, and copy into it the contents of the *dist* folder.</span></span>

<span data-ttu-id="592c5-213">Cree un archivo *wwwroot/swagger/ui/css/custom.css* con el siguiente código CSS para personalizar el encabezado de página:</span><span class="sxs-lookup"><span data-stu-id="592c5-213">Create a *wwwroot/swagger/ui/css/custom.css* file with the following CSS to customize the page header:</span></span>

[!code-css[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/wwwroot/swagger/ui/css/custom.css)]

<span data-ttu-id="592c5-214">Cree una referencia a *custom.css* en el archivo *index.html*:</span><span class="sxs-lookup"><span data-stu-id="592c5-214">Reference *custom.css* in the *index.html* file:</span></span>

[!code-html[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/wwwroot/swagger/ui/index.html?range=14)]

<span data-ttu-id="592c5-215">Vaya a la página *index.html* en `http://localhost:<random_port>/swagger/ui/index.html`.</span><span class="sxs-lookup"><span data-stu-id="592c5-215">Browse to the *index.html* page at `http://localhost:<random_port>/swagger/ui/index.html`.</span></span> <span data-ttu-id="592c5-216">Escriba `http://localhost:<random_port>/swagger/v1/swagger.json` en el cuadro de texto del encabezado y haga clic en el botón **Explorar**.</span><span class="sxs-lookup"><span data-stu-id="592c5-216">Enter `http://localhost:<random_port>/swagger/v1/swagger.json` in the header's textbox, and click the **Explore** button.</span></span> <span data-ttu-id="592c5-217">La página resultante tiene el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="592c5-217">The resulting page looks as follows:</span></span>

![Interfaz de usuario de Swagger con el título de encabezado personalizado](web-api-help-pages-using-swagger/_static/custom-header.png)

<span data-ttu-id="592c5-219">Puede hacer muchas más cosas con la página.</span><span class="sxs-lookup"><span data-stu-id="592c5-219">There's much more you can do with the page.</span></span> <span data-ttu-id="592c5-220">Vea todas las capacidades de los recursos de la interfaz de usuario en el [repositorio de GitHub de la interfaz de usuario de Swagger](https://github.com/swagger-api/swagger-ui).</span><span class="sxs-lookup"><span data-stu-id="592c5-220">See the full capabilities for the UI resources at the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui).</span></span>
