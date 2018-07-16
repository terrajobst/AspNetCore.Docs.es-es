---
title: Crear una API web con ASP.NET Core y Visual Studio para Mac
author: rick-anderson
description: Crear una API web con ASP.NET Core MVC y Visual Studio para Mac
ms.author: riande
ms.custom: mvc
ms.date: 05/08/2018
uid: tutorials/first-web-api-mac
ms.openlocfilehash: 4caa6d9057de8d0e821c4abefe22985f43ff95ad
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2018
ms.locfileid: "38156145"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-mac"></a><span data-ttu-id="0d033-103">Crear una API web con ASP.NET Core y Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="0d033-103">Create a Web API with ASP.NET Core and Visual Studio for Mac</span></span>

<span data-ttu-id="0d033-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="0d033-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="0d033-105">En este tutorial se compila una API web para administrar una lista de tareas pendientes.</span><span class="sxs-lookup"><span data-stu-id="0d033-105">In this tutorial, build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="0d033-106">No se construye la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="0d033-106">The UI isn't constructed.</span></span>

<span data-ttu-id="0d033-107">Hay tres versiones de este tutorial:</span><span class="sxs-lookup"><span data-stu-id="0d033-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="0d033-108">macOS: API web con Visual Studio para Mac (este tutorial)</span><span class="sxs-lookup"><span data-stu-id="0d033-108">macOS: Web API with Visual Studio for Mac (This tutorial)</span></span>
* <span data-ttu-id="0d033-109">Windows: [API web con Visual Studio para Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="0d033-109">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>
* <span data-ttu-id="0d033-110">macOS, Linux y Windows: [API web con Visual Studio Code](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="0d033-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

<span data-ttu-id="0d033-111">Vea [Introduction to ASP.NET Core MVC on macOS or Linux](xref:tutorials/first-mvc-app-xplat/index) (Introducción a ASP.NET Core MVC en macOS o Linux) para obtener un ejemplo en el que se usa una base de datos persistente.</span><span class="sxs-lookup"><span data-stu-id="0d033-111">See [Introduction to ASP.NET Core MVC on macOS or Linux](xref:tutorials/first-mvc-app-xplat/index) for an example that uses a persistent database.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0d033-112">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="0d033-112">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-macos.md)]

## <a name="create-the-project"></a><span data-ttu-id="0d033-113">Crear el proyecto</span><span class="sxs-lookup"><span data-stu-id="0d033-113">Create the project</span></span>

<span data-ttu-id="0d033-114">En Visual Studio, seleccione **Archivo** > **Nueva solución**.</span><span class="sxs-lookup"><span data-stu-id="0d033-114">From Visual Studio, select **File** > **New Solution**.</span></span>

![macOS: Nueva solución](first-web-api-mac/_static/sln.png)

<span data-ttu-id="0d033-116">Seleccione **Aplicación .NET Core** > **API web de ASP.NET Core** > **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="0d033-116">Select **.NET Core App** > **ASP.NET Core Web API** > **Next**.</span></span>

![macOS: Cuadro de diálogo Nuevo proyecto](first-web-api-mac/_static/1.png)

<span data-ttu-id="0d033-118">Escriba *TodoApi* en **Nombre del proyecto** y haga clic en **Crear**.</span><span class="sxs-lookup"><span data-stu-id="0d033-118">Enter *TodoApi* for the **Project Name**, and then click **Create**.</span></span>

![cuadro de diálogo de configuración](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="0d033-120">Iniciar la aplicación</span><span class="sxs-lookup"><span data-stu-id="0d033-120">Launch the app</span></span>

<span data-ttu-id="0d033-121">En Visual Studio, seleccione **Ejecutar** > **Iniciar con depuración** para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0d033-121">In Visual Studio, select **Run** > **Start With Debugging** to launch the app.</span></span> <span data-ttu-id="0d033-122">Visual Studio inicia un explorador y se desplaza a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="0d033-122">Visual Studio launches a browser and navigates to `http://localhost:5000`.</span></span> <span data-ttu-id="0d033-123">Obtendrá un error HTTP 404 (No encontrado).</span><span class="sxs-lookup"><span data-stu-id="0d033-123">You get an HTTP 404 (Not Found) error.</span></span> <span data-ttu-id="0d033-124">Cambie la dirección URL a `http://localhost:<port>/api/values`.</span><span class="sxs-lookup"><span data-stu-id="0d033-124">Change the URL to `http://localhost:<port>/api/values`.</span></span> <span data-ttu-id="0d033-125">Se muestran los datos de `ValuesController`:</span><span class="sxs-lookup"><span data-stu-id="0d033-125">The `ValuesController` data is displayed:</span></span>

```json
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="0d033-126">Agregar compatibilidad con Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="0d033-126">Add support for Entity Framework Core</span></span>

<span data-ttu-id="0d033-127">Instale el proveedor de base de datos [Entity Framework Core InMemory](/ef/core/providers/in-memory/).</span><span class="sxs-lookup"><span data-stu-id="0d033-127">Install the [Entity Framework Core InMemory](/ef/core/providers/in-memory/) database provider.</span></span> <span data-ttu-id="0d033-128">Este proveedor de base de datos permite usar Entity Framework Core con una base de datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="0d033-128">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

* <span data-ttu-id="0d033-129">En el menú **Proyecto**, seleccione **Agregar paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="0d033-129">From the **Project** menu, select **Add NuGet Packages**.</span></span>

  * <span data-ttu-id="0d033-130">Como alternativa, puede hacer clic con el botón derecho en **Dependencias** y seleccionar **Agregar paquetes**.</span><span class="sxs-lookup"><span data-stu-id="0d033-130">Alternatively, you can right-click **Dependencies**, and then select **Add Packages**.</span></span>

* <span data-ttu-id="0d033-131">Escriba `EntityFrameworkCore.InMemory` en el cuadro de búsqueda.</span><span class="sxs-lookup"><span data-stu-id="0d033-131">Enter `EntityFrameworkCore.InMemory` in the search box.</span></span>
* <span data-ttu-id="0d033-132">Seleccione `Microsoft.EntityFrameworkCore.InMemory` y, luego, **Agregar paquete**.</span><span class="sxs-lookup"><span data-stu-id="0d033-132">Select `Microsoft.EntityFrameworkCore.InMemory`, and then select **Add Package**.</span></span>

### <a name="add-a-model-class"></a><span data-ttu-id="0d033-133">Agregar una clase de modelo</span><span class="sxs-lookup"><span data-stu-id="0d033-133">Add a model class</span></span>

<span data-ttu-id="0d033-134">Un modelo es un objeto que representa los datos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0d033-134">A model is an object representing the data in your app.</span></span> <span data-ttu-id="0d033-135">En este caso, el único modelo es una tarea pendiente.</span><span class="sxs-lookup"><span data-stu-id="0d033-135">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="0d033-136">En el Explorador de soluciones, haga clic con el botón derecho en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="0d033-136">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="0d033-137">Seleccione **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="0d033-137">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="0d033-138">Asigne a la carpeta el nombre *Modelos*.</span><span class="sxs-lookup"><span data-stu-id="0d033-138">Name the folder *Models*.</span></span>

![nueva carpeta](first-web-api-mac/_static/folder.png)

> [!NOTE]
> <span data-ttu-id="0d033-140">Puede colocar clases de modelo en cualquier lugar del proyecto, pero la carpeta *Models* se usa por convención.</span><span class="sxs-lookup"><span data-stu-id="0d033-140">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="0d033-141">Haga clic con el botón derecho en la carpeta *Modelos* y seleccione **Agregar** > **Nuevo archivo** > **General** > **Clase vacía**.</span><span class="sxs-lookup"><span data-stu-id="0d033-141">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span> <span data-ttu-id="0d033-142">Denomine la clase *TodoItem* y, después, haga clic en **Nuevo**.</span><span class="sxs-lookup"><span data-stu-id="0d033-142">Name the class *TodoItem*, and then click **New**.</span></span>

<span data-ttu-id="0d033-143">Reemplace el código generado por el siguiente:</span><span class="sxs-lookup"><span data-stu-id="0d033-143">Replace the generated code with:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="0d033-144">La base de datos genera el `Id` cuando se crea `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="0d033-144">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="0d033-145">Crear el contexto de base de datos</span><span class="sxs-lookup"><span data-stu-id="0d033-145">Create the database context</span></span>

<span data-ttu-id="0d033-146">El *contexto de base de datos* es la clase principal que coordina la funcionalidad de Entity Framework para un modelo de datos determinado.</span><span class="sxs-lookup"><span data-stu-id="0d033-146">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="0d033-147">Esta clase se crea derivándola de la clase `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="0d033-147">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="0d033-148">Agregue una clase `TodoContext` a la carpeta *Modelos*.</span><span class="sxs-lookup"><span data-stu-id="0d033-148">Add a `TodoContext` class to the *Models* folder.</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="0d033-149">Agregar un controlador</span><span class="sxs-lookup"><span data-stu-id="0d033-149">Add a controller</span></span>

<span data-ttu-id="0d033-150">En el Explorador de soluciones, en la carpeta *Controladores*, agregue la clase `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="0d033-150">In Solution Explorer, in the *Controllers* folder, add the class `TodoController`.</span></span>

<span data-ttu-id="0d033-151">Reemplace el código generado con el siguiente:</span><span class="sxs-lookup"><span data-stu-id="0d033-151">Replace the generated code with the following:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="0d033-152">Iniciar la aplicación</span><span class="sxs-lookup"><span data-stu-id="0d033-152">Launch the app</span></span>

<span data-ttu-id="0d033-153">En Visual Studio, seleccione **Ejecutar** > **Iniciar con depuración** para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0d033-153">In Visual Studio, select **Run** > **Start With Debugging** to launch the app.</span></span> <span data-ttu-id="0d033-154">Visual Studio inicia un explorador y navega hasta `http://localhost:<port>`, donde `<port>` es un número de puerto elegido aleatoriamente.</span><span class="sxs-lookup"><span data-stu-id="0d033-154">Visual Studio launches a browser and navigates to `http://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="0d033-155">Obtendrá un error HTTP 404 (No encontrado).</span><span class="sxs-lookup"><span data-stu-id="0d033-155">You get an HTTP 404 (Not Found) error.</span></span> <span data-ttu-id="0d033-156">Cambie la dirección URL a `http://localhost:<port>/api/values`.</span><span class="sxs-lookup"><span data-stu-id="0d033-156">Change the URL to `http://localhost:<port>/api/values`.</span></span> <span data-ttu-id="0d033-157">Se muestran los datos de `ValuesController`:</span><span class="sxs-lookup"><span data-stu-id="0d033-157">The `ValuesController` data is displayed:</span></span>

```json
["value1","value2"]
```

<span data-ttu-id="0d033-158">Vaya al controlador `Todo` en `http://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="0d033-158">Navigate to the `Todo` controller at `http://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="0d033-159">Se devuelve el siguiente JSON:</span><span class="sxs-lookup"><span data-stu-id="0d033-159">The following JSON is returned:</span></span>

```json
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="0d033-160">Implementar las otras operaciones CRUD</span><span class="sxs-lookup"><span data-stu-id="0d033-160">Implement the other CRUD operations</span></span>

<span data-ttu-id="0d033-161">Vamos a agregar los métodos `Create`, `Update` y `Delete` al controlador.</span><span class="sxs-lookup"><span data-stu-id="0d033-161">We'll add `Create`, `Update`, and `Delete` methods to the controller.</span></span> <span data-ttu-id="0d033-162">Estos métodos son variaciones de un tema, así que solo mostraré el código y comentaré las diferencias principales.</span><span class="sxs-lookup"><span data-stu-id="0d033-162">These methods are variations on a theme, so I'll just show the code and highlight the main differences.</span></span> <span data-ttu-id="0d033-163">Compile el proyecto después de agregar o cambiar el código.</span><span class="sxs-lookup"><span data-stu-id="0d033-163">Build the project after adding or changing code.</span></span>

### <a name="create"></a><span data-ttu-id="0d033-164">Crear</span><span class="sxs-lookup"><span data-stu-id="0d033-164">Create</span></span>

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="0d033-165">El código anterior responde a un método HTTP POST, como se puede apreciar por el atributo [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="0d033-165">The preceding method responds to an HTTP POST, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="0d033-166">El atributo [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) indica a MVC que obtenga el valor de la tarea pendiente del cuerpo de la solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="0d033-166">The [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="0d033-167">El código anterior responde a un método HTTP POST, como se puede apreciar por el atributo [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="0d033-167">The preceding method responds to an HTTP POST, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="0d033-168">MVC obtiene el valor de la tarea pendiente del cuerpo de la solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="0d033-168">MVC gets the value of the to-do item from the body of the HTTP request.</span></span>
::: moniker-end

<span data-ttu-id="0d033-169">El método `CreatedAtRoute` devuelve una respuesta 201.</span><span class="sxs-lookup"><span data-stu-id="0d033-169">The `CreatedAtRoute` method returns a 201 response.</span></span> <span data-ttu-id="0d033-170">Se trata de la respuesta estándar de un método HTTP POST que crea un recurso en el servidor.</span><span class="sxs-lookup"><span data-stu-id="0d033-170">It's the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="0d033-171">`CreatedAtRoute` también agrega un encabezado de ubicación a la respuesta.</span><span class="sxs-lookup"><span data-stu-id="0d033-171">`CreatedAtRoute` also adds a Location header to the response.</span></span> <span data-ttu-id="0d033-172">El encabezado de ubicación especifica el URI de la tarea pendiente recién creada.</span><span class="sxs-lookup"><span data-stu-id="0d033-172">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="0d033-173">Vea [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) (10.2.2 201 creada).</span><span class="sxs-lookup"><span data-stu-id="0d033-173">See [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="0d033-174">Usar Postman para enviar una solicitud de creación</span><span class="sxs-lookup"><span data-stu-id="0d033-174">Use Postman to send a Create request</span></span>

* <span data-ttu-id="0d033-175">Inicie la aplicación (**Ejecutar** > **Iniciar con depuración**).</span><span class="sxs-lookup"><span data-stu-id="0d033-175">Start the app (**Run** > **Start With Debugging**).</span></span>
* <span data-ttu-id="0d033-176">Abra Postman.</span><span class="sxs-lookup"><span data-stu-id="0d033-176">Open Postman.</span></span>

![Consola de Postman](first-web-api/_static/pmc.png)

* <span data-ttu-id="0d033-178">Actualice el número de puerto en la dirección URL de localhost.</span><span class="sxs-lookup"><span data-stu-id="0d033-178">Update the port number in the localhost URL.</span></span>
* <span data-ttu-id="0d033-179">Establezca el método HTTP en *POST*.</span><span class="sxs-lookup"><span data-stu-id="0d033-179">Set the HTTP method to *POST*.</span></span>
* <span data-ttu-id="0d033-180">Haga clic en la pestaña **Body** (Cuerpo).</span><span class="sxs-lookup"><span data-stu-id="0d033-180">Click the **Body** tab.</span></span>
* <span data-ttu-id="0d033-181">Seleccione el botón de radio **Raw** (Sin formato).</span><span class="sxs-lookup"><span data-stu-id="0d033-181">Select the **raw** radio button.</span></span>
* <span data-ttu-id="0d033-182">Establezca el tipo en *JSON (application/json)*.</span><span class="sxs-lookup"><span data-stu-id="0d033-182">Set the type to *JSON (application/json)*.</span></span>
* <span data-ttu-id="0d033-183">Escriba un cuerpo de solicitud con una tarea pendiente parecida al siguiente JSON:</span><span class="sxs-lookup"><span data-stu-id="0d033-183">Enter a request body with a to-do item resembling the following JSON:</span></span>

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* <span data-ttu-id="0d033-184">Haga clic en el botón **Send** (Enviar).</span><span class="sxs-lookup"><span data-stu-id="0d033-184">Click the **Send** button.</span></span>

::: moniker range=">= aspnetcore-2.1"
> [!TIP]
> <span data-ttu-id="0d033-185">Si no aparece ninguna respuesta tras hacer clic en **Send** (Enviar), deshabilite la opción **SSL certification verification** (Comprobación de certificación SSL).</span><span class="sxs-lookup"><span data-stu-id="0d033-185">If no response displays after clicking **Send**, disable the **SSL certification verification** option.</span></span> <span data-ttu-id="0d033-186">La encontrará en **File** > **Settings** (Archivo > Configuración).</span><span class="sxs-lookup"><span data-stu-id="0d033-186">This is found under **File** > **Settings**.</span></span> <span data-ttu-id="0d033-187">Vuelva a hacer clic en el botón **Send** (Enviar) después de deshabilitar la configuración.</span><span class="sxs-lookup"><span data-stu-id="0d033-187">Click the **Send** button again after disabling the setting.</span></span>
::: moniker-end

<span data-ttu-id="0d033-188">Haga clic en la pestaña **Headers** (Encabezados) del panel **Response** (Respuesta) y copie el valor de encabezado de **Location** (Ubicación):</span><span class="sxs-lookup"><span data-stu-id="0d033-188">Click the **Headers** tab in the **Response** pane and copy the **Location** header value:</span></span>

![Pestaña Headers (Encabezados) de la consola de Postman](first-web-api/_static/pmc2.png)

<span data-ttu-id="0d033-190">Puede usar el URI del encabezado Location (Ubicación) para tener acceso al recurso que ha creado.</span><span class="sxs-lookup"><span data-stu-id="0d033-190">You can use the Location header URI to access the resource you created.</span></span> <span data-ttu-id="0d033-191">El método `Create` devuelve [CreatedAtRoute](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdatroute#Microsoft_AspNetCore_Mvc_ControllerBase_CreatedAtRoute_System_String_System_Object_System_Object_).</span><span class="sxs-lookup"><span data-stu-id="0d033-191">The `Create` method returns [CreatedAtRoute](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdatroute#Microsoft_AspNetCore_Mvc_ControllerBase_CreatedAtRoute_System_String_System_Object_System_Object_).</span></span> <span data-ttu-id="0d033-192">El primer parámetro que se pasa a `CreatedAtRoute` representa la ruta con nombre que se usa para generar la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="0d033-192">The first parameter passed to `CreatedAtRoute` represents the named route to use for generating the URL.</span></span> <span data-ttu-id="0d033-193">Recuerde que el método `GetById` creó la ruta con nombre `"GetTodo"`:</span><span class="sxs-lookup"><span data-stu-id="0d033-193">Recall that the `GetById` method created the `"GetTodo"` named route:</span></span>

```csharp
[HttpGet("{id}", Name = "GetTodo")]
```

### <a name="update"></a><span data-ttu-id="0d033-194">Actualizar</span><span class="sxs-lookup"><span data-stu-id="0d033-194">Update</span></span>

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]
::: moniker-end

<span data-ttu-id="0d033-195">`Update` es similar a `Create`, pero usa HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="0d033-195">`Update` is similar to `Create`, but uses HTTP PUT.</span></span> <span data-ttu-id="0d033-196">La respuesta es [204 Sin contenido](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="0d033-196">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="0d033-197">Según la especificación HTTP, una solicitud PUT requiere que el cliente envíe toda la entidad actualizada, no solo los deltas.</span><span class="sxs-lookup"><span data-stu-id="0d033-197">According to the HTTP spec, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="0d033-198">Para admitir actualizaciones parciales, use HTTP PATCH.</span><span class="sxs-lookup"><span data-stu-id="0d033-198">To support partial updates, use HTTP PATCH.</span></span>

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![Consola de Postman que muestra la respuesta 204 Sin contenido](first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="0d033-200">Eliminar</span><span class="sxs-lookup"><span data-stu-id="0d033-200">Delete</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="0d033-201">La respuesta es [204 Sin contenido](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="0d033-201">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

![Consola de Postman que muestra la respuesta 204 Sin contenido](first-web-api/_static/pmd.png)

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
