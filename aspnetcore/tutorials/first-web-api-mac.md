---
title: Crear una API web con ASP.NET Core y Visual Studio para Mac
description: Crear una API web con ASP.NET Core MVC y Visual Studio para Mac
author: rick-anderson
ms.author: riande
ms.date: 09/15/2017
ms.topic: get-started-article
ms.prod: asp.net-core
uid: tutorials/first-web-api-mac
helpviewer_heywords: ASP.NET Core, WebAPI, Web API, REST, mac, macOS, HTTP, Service, HTTP Service
ms.technology: aspnet
manager: wpickett
ms.openlocfilehash: 4f2643a91e1523008b68df670a9734e3d4dea5a8
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-for-mac"></a><span data-ttu-id="e4890-103">Crear una API web con ASP.NET Core MVC y Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="e4890-103">Create a Web API with ASP.NET Core MVC and Visual Studio for Mac</span></span>

<span data-ttu-id="e4890-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="e4890-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="e4890-105">En este tutorial compilará una API Web para administrar una lista de tareas pendientes.</span><span class="sxs-lookup"><span data-stu-id="e4890-105">In this tutorial, you’ll build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="e4890-106">No compilará ninguna interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="e4890-106">You won’t build a UI.</span></span>

<span data-ttu-id="e4890-107">Hay tres versiones de este tutorial:</span><span class="sxs-lookup"><span data-stu-id="e4890-107">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="e4890-108">macOS: API web con Visual Studio para Mac (este tutorial)</span><span class="sxs-lookup"><span data-stu-id="e4890-108">macOS: Web API with Visual Studio for Mac (This tutorial)</span></span>
* <span data-ttu-id="e4890-109">Windows: [API web con Visual Studio para Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="e4890-109">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>
* <span data-ttu-id="e4890-110">macOS, Linux y Windows: [API web con Visual Studio Code](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="e4890-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

* <span data-ttu-id="e4890-111">Vea [Introduction to ASP.NET Core MVC on Mac or Linux](xref:tutorials/first-mvc-app-xplat/index) (Introducción a ASP.NET Core MVC en Mac o Linux) para ver un ejemplo en el que se usa una base de datos persistente.</span><span class="sxs-lookup"><span data-stu-id="e4890-111">See [Introduction to ASP.NET Core MVC on Mac or Linux](xref:tutorials/first-mvc-app-xplat/index) for an example that uses a persistent database.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e4890-112">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="e4890-112">Prerequisites</span></span>

<span data-ttu-id="e4890-113">Instale el software siguiente:</span><span class="sxs-lookup"><span data-stu-id="e4890-113">Install the following:</span></span>

- <span data-ttu-id="e4890-114">[SDK de .NET Core 2.0.0](https://www.microsoft.com/net/core) o versiones posteriores</span><span class="sxs-lookup"><span data-stu-id="e4890-114">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later</span></span>
- [<span data-ttu-id="e4890-115">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="e4890-115">Visual Studio for Mac</span></span>](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-the-project"></a><span data-ttu-id="e4890-116">Crear el proyecto</span><span class="sxs-lookup"><span data-stu-id="e4890-116">Create the project</span></span>

<span data-ttu-id="e4890-117">Desde Visual Studio, seleccione **Archivo > Nueva solución**.</span><span class="sxs-lookup"><span data-stu-id="e4890-117">From Visual Studio, select **File > New Solution**.</span></span>

![macOS: Nueva solución](first-web-api-mac/_static/sln.png)

<span data-ttu-id="e4890-119">Seleccione **Aplicación .NET Core > API web de ASP.NET Core > Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="e4890-119">Select **.NET Core App >  ASP.NET Core Web API > Next**.</span></span>

![macOS: Cuadro de diálogo Nuevo proyecto](first-web-api-mac/_static/1.png)

<span data-ttu-id="e4890-121">Escriba **TodoApi** en **Nombre del proyecto** y seleccione Crear.</span><span class="sxs-lookup"><span data-stu-id="e4890-121">Enter **TodoApi** for the **Project Name**, and then select Create.</span></span>

![cuadro de diálogo de configuración](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="e4890-123">Iniciar la aplicación</span><span class="sxs-lookup"><span data-stu-id="e4890-123">Launch the app</span></span>

<span data-ttu-id="e4890-124">En Visual Studio, seleccione **Ejecutar > Iniciar con depuración** para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="e4890-124">In Visual Studio, select **Run > Start With Debugging** to launch the app.</span></span> <span data-ttu-id="e4890-125">Visual Studio inicia un explorador y se desplaza a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="e4890-125">Visual Studio launches a browser and navigates to `http://localhost:5000`.</span></span> <span data-ttu-id="e4890-126">Obtendrá un error HTTP 404 (No encontrado).</span><span class="sxs-lookup"><span data-stu-id="e4890-126">You get an HTTP 404 (Not Found) error.</span></span>  <span data-ttu-id="e4890-127">Cambie la dirección URL a `http://localhost:port/api/values`.</span><span class="sxs-lookup"><span data-stu-id="e4890-127">Change the URL to `http://localhost:port/api/values`.</span></span> <span data-ttu-id="e4890-128">Se mostrarán los datos de `ValuesController`:</span><span class="sxs-lookup"><span data-stu-id="e4890-128">The `ValuesController` data will be displayed:</span></span>

```
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="e4890-129">Agregar compatibilidad para Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="e4890-129">Add support for Entity Framework Core</span></span>

<span data-ttu-id="e4890-130">Instale el proveedor de base de datos [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/).</span><span class="sxs-lookup"><span data-stu-id="e4890-130">Install the [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) database provider.</span></span> <span data-ttu-id="e4890-131">Este proveedor de base de datos permite usar Entity Framework Core con una base de datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="e4890-131">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

* <span data-ttu-id="e4890-132">En el menú **Proyecto**, seleccione **Agregar paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="e4890-132">From the **Project** menu, select **Add NuGet Packages**.</span></span> 

  *  <span data-ttu-id="e4890-133">Como alternativa, puede hacer clic con el botón derecho en **Dependencias** y seleccionar **Agregar paquetes**.</span><span class="sxs-lookup"><span data-stu-id="e4890-133">Alternately, you can right-click **Dependencies**, and then select **Add Packages**.</span></span>

* <span data-ttu-id="e4890-134">Escriba `EntityFrameworkCore.InMemory` en el cuadro de búsqueda.</span><span class="sxs-lookup"><span data-stu-id="e4890-134">Enter `EntityFrameworkCore.InMemory` in the search box.</span></span>
* <span data-ttu-id="e4890-135">Seleccione `Microsoft.EntityFrameworkCore.InMemory` y, luego, **Agregar paquete**.</span><span class="sxs-lookup"><span data-stu-id="e4890-135">Select `Microsoft.EntityFrameworkCore.InMemory`, and then select **Add Package**.</span></span>

### <a name="add-a-model-class"></a><span data-ttu-id="e4890-136">Agregar una clase de modelo</span><span class="sxs-lookup"><span data-stu-id="e4890-136">Add a model class</span></span>

<span data-ttu-id="e4890-137">Un modelo es un objeto que representa los datos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="e4890-137">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="e4890-138">En este caso, el único modelo es una tarea pendiente.</span><span class="sxs-lookup"><span data-stu-id="e4890-138">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="e4890-139">Agregue una carpeta denominada *Models*.</span><span class="sxs-lookup"><span data-stu-id="e4890-139">Add a folder named *Models*.</span></span> <span data-ttu-id="e4890-140">En el Explorador de soluciones, haga clic con el botón derecho en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="e4890-140">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="e4890-141">Seleccione **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="e4890-141">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="e4890-142">Asigne a la carpeta el nombre *Modelos*.</span><span class="sxs-lookup"><span data-stu-id="e4890-142">Name the folder *Models*.</span></span>

![nueva carpeta](first-web-api-mac/_static/folder.png)

<span data-ttu-id="e4890-144">Nota: Puede colocar clases de modelo en cualquier lugar del proyecto, pero la carpeta *Modelos* se usa por convención.</span><span class="sxs-lookup"><span data-stu-id="e4890-144">Note: You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="e4890-145">Agregue una clase `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="e4890-145">Add a `TodoItem` class.</span></span> <span data-ttu-id="e4890-146">Haga clic con el botón derecho en la carpeta *Modelos* y seleccione **Agregar > Nuevo archivo > General > Clase vacía**.</span><span class="sxs-lookup"><span data-stu-id="e4890-146">Right-click the *Models* folder and select **Add > New File > General > Empty Class**.</span></span> <span data-ttu-id="e4890-147">Asigne a la clase el nombre `TodoItem` y seleccione **Nuevo**.</span><span class="sxs-lookup"><span data-stu-id="e4890-147">Name the class `TodoItem`, and then select **New**.</span></span>

<span data-ttu-id="e4890-148">Reemplace el código generado por el siguiente:</span><span class="sxs-lookup"><span data-stu-id="e4890-148">Replace the generated code with:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="e4890-149">La base de datos genera el `Id` cuando se crea `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="e4890-149">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="e4890-150">Crear el contexto de base de datos</span><span class="sxs-lookup"><span data-stu-id="e4890-150">Create the database context</span></span>

<span data-ttu-id="e4890-151">El *contexto de base de datos* es la clase principal que coordina la funcionalidad de Entity Framework para un modelo de datos determinado.</span><span class="sxs-lookup"><span data-stu-id="e4890-151">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="e4890-152">Esta clase se crea derivándola de la clase `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="e4890-152">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="e4890-153">Agregue una clase `TodoContext` a la carpeta *Modelos*.</span><span class="sxs-lookup"><span data-stu-id="e4890-153">Add a `TodoContext` class to the *Models* folder.</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="e4890-154">Agregar un controlador</span><span class="sxs-lookup"><span data-stu-id="e4890-154">Add a controller</span></span>

<span data-ttu-id="e4890-155">En el Explorador de soluciones, en la carpeta *Controladores*, agregue la clase `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="e4890-155">In Solution Explorer, in the *Controllers* folder, add the class `TodoController`.</span></span>

<span data-ttu-id="e4890-156">Reemplace el código generado por el siguiente (y agregue llaves de cierre):</span><span class="sxs-lookup"><span data-stu-id="e4890-156">Replace the generated code with the following (and add closing braces):</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="e4890-157">Iniciar la aplicación</span><span class="sxs-lookup"><span data-stu-id="e4890-157">Launch the app</span></span>

<span data-ttu-id="e4890-158">En Visual Studio, seleccione **Ejecutar > Iniciar con depuración** para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="e4890-158">In Visual Studio, select **Run > Start With Debugging** to launch the app.</span></span> <span data-ttu-id="e4890-159">Visual Studio inicia un explorador y navega hasta `http://localhost:port`, donde *port* es un número de puerto elegido aleatoriamente.</span><span class="sxs-lookup"><span data-stu-id="e4890-159">Visual Studio launches a browser and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span> <span data-ttu-id="e4890-160">Obtendrá un error HTTP 404 (No encontrado).</span><span class="sxs-lookup"><span data-stu-id="e4890-160">You get an HTTP 404 (Not Found) error.</span></span>  <span data-ttu-id="e4890-161">Cambie la dirección URL a `http://localhost:port/api/values`.</span><span class="sxs-lookup"><span data-stu-id="e4890-161">Change the URL to `http://localhost:port/api/values`.</span></span> <span data-ttu-id="e4890-162">Se mostrarán los datos de `ValuesController`:</span><span class="sxs-lookup"><span data-stu-id="e4890-162">The `ValuesController` data will be displayed:</span></span>

```
["value1","value2"]
```

<span data-ttu-id="e4890-163">Vaya al controlador `Todo` en `http://localhost:port/api/todo`:</span><span class="sxs-lookup"><span data-stu-id="e4890-163">Navigate to the `Todo` controller at`http://localhost:port/api/todo`:</span></span>

```
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="e4890-164">Implementar las otras operaciones CRUD</span><span class="sxs-lookup"><span data-stu-id="e4890-164">Implement the other CRUD operations</span></span>

<span data-ttu-id="e4890-165">Vamos a agregar los métodos `Create`, `Update` y `Delete` al controlador.</span><span class="sxs-lookup"><span data-stu-id="e4890-165">We'll add `Create`, `Update`, and `Delete` methods to the controller.</span></span> <span data-ttu-id="e4890-166">Son variaciones de un tema, así que solo mostraré el código y comentaré las diferencias principales.</span><span class="sxs-lookup"><span data-stu-id="e4890-166">These are variations on a theme, so I'll just show the code and highlight the main differences.</span></span> <span data-ttu-id="e4890-167">Compile el proyecto después de agregar o cambiar el código.</span><span class="sxs-lookup"><span data-stu-id="e4890-167">Build the project after adding or changing code.</span></span>

### <a name="create"></a><span data-ttu-id="e4890-168">Crear</span><span class="sxs-lookup"><span data-stu-id="e4890-168">Create</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="e4890-169">Se trata de un método HTTP POST, indicado por el atributo [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="e4890-169">This is an HTTP POST method, indicated by the [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="e4890-170">El atributo [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) indica a MVC que obtenga el valor de la tarea pendiente del cuerpo de la solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="e4890-170">The [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="e4890-171">El método `CreatedAtRoute` devuelve una respuesta 201, que es la respuesta estándar para un método HTTP POST que crea un nuevo recurso en el servidor.</span><span class="sxs-lookup"><span data-stu-id="e4890-171">The `CreatedAtRoute` method returns a 201 response, which is the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="e4890-172">`CreatedAtRoute` también agrega un encabezado de ubicación a la respuesta.</span><span class="sxs-lookup"><span data-stu-id="e4890-172">`CreatedAtRoute` also adds a Location header to the response.</span></span> <span data-ttu-id="e4890-173">El encabezado de ubicación especifica el URI de la tarea pendiente recién creada.</span><span class="sxs-lookup"><span data-stu-id="e4890-173">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="e4890-174">Vea [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) (10.2.2 201 creada).</span><span class="sxs-lookup"><span data-stu-id="e4890-174">See [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="e4890-175">Usar Postman para enviar una solicitud de creación</span><span class="sxs-lookup"><span data-stu-id="e4890-175">Use Postman to send a Create request</span></span>

* <span data-ttu-id="e4890-176">Inicie la aplicación (**Ejecutar > Iniciar con depuración**).</span><span class="sxs-lookup"><span data-stu-id="e4890-176">Start the app (**Run > Start With Debugging**).</span></span>
* <span data-ttu-id="e4890-177">Inicie Postman.</span><span class="sxs-lookup"><span data-stu-id="e4890-177">Start Postman.</span></span>

![Consola de Postman](first-web-api/_static/pmc.png)

* <span data-ttu-id="e4890-179">Establezca el método HTTP en `POST`.</span><span class="sxs-lookup"><span data-stu-id="e4890-179">Set the HTTP method to `POST`</span></span>
* <span data-ttu-id="e4890-180">Seleccione el botón de radio **Body** (Cuerpo).</span><span class="sxs-lookup"><span data-stu-id="e4890-180">Select the **Body** radio button</span></span>
* <span data-ttu-id="e4890-181">Seleccione el botón de radio **Raw** (Sin formato).</span><span class="sxs-lookup"><span data-stu-id="e4890-181">Select the **raw** radio button</span></span>
* <span data-ttu-id="e4890-182">Establezca el tipo en JSON.</span><span class="sxs-lookup"><span data-stu-id="e4890-182">Set the type to JSON</span></span>
* <span data-ttu-id="e4890-183">En el editor de pares clave-valor, escriba una tarea pendiente como la siguiente:</span><span class="sxs-lookup"><span data-stu-id="e4890-183">In the key-value editor, enter a Todo item such as</span></span>

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* <span data-ttu-id="e4890-184">Seleccione **Send** (Enviar).</span><span class="sxs-lookup"><span data-stu-id="e4890-184">Select **Send**</span></span>

* <span data-ttu-id="e4890-185">Seleccione la pestaña Headers (Encabezados) en el panel inferior y copie el encabezado **Location** (Ubicación):</span><span class="sxs-lookup"><span data-stu-id="e4890-185">Select the Headers tab in the lower pane and copy the **Location** header:</span></span>

![Pestaña Headers (Encabezados) de la consola de Postman](first-web-api/_static/pmget.png)

<span data-ttu-id="e4890-187">Puede usar el URI del encabezado Location (Ubicación) para acceder al recurso que acaba de crear.</span><span class="sxs-lookup"><span data-stu-id="e4890-187">You can use the Location header URI to access the resource you just created.</span></span> <span data-ttu-id="e4890-188">Recuerde que el método `GetById` creó la ruta denominada `"GetTodo"`:</span><span class="sxs-lookup"><span data-stu-id="e4890-188">Recall the `GetById` method created the `"GetTodo"` named route:</span></span>

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(string id)
```

### <a name="update"></a><span data-ttu-id="e4890-189">Actualizar</span><span class="sxs-lookup"><span data-stu-id="e4890-189">Update</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="e4890-190">`Update` es similar a `Create`, pero usa HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="e4890-190">`Update` is similar to `Create`, but uses HTTP PUT.</span></span> <span data-ttu-id="e4890-191">La respuesta es [204 Sin contenido](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="e4890-191">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="e4890-192">Según la especificación HTTP, una solicitud PUT requiere que el cliente envíe toda la entidad actualizada, no solo los deltas.</span><span class="sxs-lookup"><span data-stu-id="e4890-192">According to the HTTP spec, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="e4890-193">Para admitir actualizaciones parciales, use HTTP PATCH.</span><span class="sxs-lookup"><span data-stu-id="e4890-193">To support partial updates, use HTTP PATCH.</span></span>

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![Consola de Postman que muestra la respuesta 204 Sin contenido](first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="e4890-195">Eliminar</span><span class="sxs-lookup"><span data-stu-id="e4890-195">Delete</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="e4890-196">La respuesta es [204 Sin contenido](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="e4890-196">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

![Consola de Postman que muestra la respuesta 204 Sin contenido](first-web-api/_static/pmd.png)

## <a name="next-steps"></a><span data-ttu-id="e4890-198">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="e4890-198">Next steps</span></span>

* <span data-ttu-id="e4890-199">[Routing to Controller Actions](xref:mvc/controllers/routing) (Enrutamiento a acciones del controlador)</span><span class="sxs-lookup"><span data-stu-id="e4890-199">[Routing to Controller Actions](xref:mvc/controllers/routing)</span></span>
* <span data-ttu-id="e4890-200">Para obtener más información sobre la implementación de la API, vea [Hospedaje e implementación](xref:host-and-deploy/index).</span><span class="sxs-lookup"><span data-stu-id="e4890-200">For information about deploying your API, see [Host and deploy](xref:host-and-deploy/index).</span></span>
* <span data-ttu-id="e4890-201">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e4890-201">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>
* [<span data-ttu-id="e4890-202">Postman</span><span class="sxs-lookup"><span data-stu-id="e4890-202">Postman</span></span>](https://www.getpostman.com/)
* [<span data-ttu-id="e4890-203">Fiddler</span><span class="sxs-lookup"><span data-stu-id="e4890-203">Fiddler</span></span>](https://www.telerik.com/download/fiddler)
