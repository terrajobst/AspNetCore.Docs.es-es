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
keywords: ASP.NET Core, WebAPI, API web, REST, mac, macOS, HTTP, servicio, servicio HTTP
manager: wpickett
ms.openlocfilehash: 6bbd5e332e395928d8f79888ecf190f7f59a4bbc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-for-mac"></a><span data-ttu-id="a9bd9-104">Crear una API web con ASP.NET Core MVC y Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="a9bd9-104">Create a Web API with ASP.NET Core MVC and Visual Studio for Mac</span></span>

<span data-ttu-id="a9bd9-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="a9bd9-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="a9bd9-106">En este tutorial compilará una API Web para administrar una lista de tareas pendientes.</span><span class="sxs-lookup"><span data-stu-id="a9bd9-106">In this tutorial, you’ll build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="a9bd9-107">No compilará ninguna interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="a9bd9-107">You won’t build a UI.</span></span>

<span data-ttu-id="a9bd9-108">Hay tres versiones de este tutorial:</span><span class="sxs-lookup"><span data-stu-id="a9bd9-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="a9bd9-109">macOS: API web con Visual Studio para Mac (este tutorial)</span><span class="sxs-lookup"><span data-stu-id="a9bd9-109">macOS: Web API with Visual Studio for Mac (This tutorial)</span></span>
* <span data-ttu-id="a9bd9-110">Windows: [API web con Visual Studio para Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="a9bd9-110">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>
* <span data-ttu-id="a9bd9-111">macOS, Linux y Windows: [API web con Visual Studio Code](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="a9bd9-111">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

* <span data-ttu-id="a9bd9-112">Vea [Introduction to ASP.NET Core MVC on Mac or Linux](xref:tutorials/first-mvc-app-xplat/index) (Introducción a ASP.NET Core MVC en Mac o Linux) para ver un ejemplo en el que se usa una base de datos persistente.</span><span class="sxs-lookup"><span data-stu-id="a9bd9-112">See [Introduction to ASP.NET Core MVC on Mac or Linux](xref:tutorials/first-mvc-app-xplat/index) for an example that uses a persistent database.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a9bd9-113">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="a9bd9-113">Prerequisites</span></span>

<span data-ttu-id="a9bd9-114">Instale el software siguiente:</span><span class="sxs-lookup"><span data-stu-id="a9bd9-114">Install the following:</span></span>

- <span data-ttu-id="a9bd9-115">[SDK de .NET Core 2.0.0](https://www.microsoft.com/net/core) o versiones posteriores</span><span class="sxs-lookup"><span data-stu-id="a9bd9-115">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later</span></span>
- [<span data-ttu-id="a9bd9-116">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="a9bd9-116">Visual Studio for Mac</span></span>](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-the-project"></a><span data-ttu-id="a9bd9-117">Crear el proyecto</span><span class="sxs-lookup"><span data-stu-id="a9bd9-117">Create the project</span></span>

<span data-ttu-id="a9bd9-118">Desde Visual Studio, seleccione **Archivo > Nueva solución**.</span><span class="sxs-lookup"><span data-stu-id="a9bd9-118">From Visual Studio, select **File > New Solution**.</span></span>

![macOS: Nueva solución](first-web-api-mac/_static/sln.png)

<span data-ttu-id="a9bd9-120">Seleccione **Aplicación .NET Core > API web de ASP.NET Core > Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="a9bd9-120">Select **.NET Core App >  ASP.NET Core Web API > Next**.</span></span>

![macOS: Cuadro de diálogo Nuevo proyecto](first-web-api-mac/_static/1.png)

<span data-ttu-id="a9bd9-122">Escriba **TodoApi** en **Nombre del proyecto** y seleccione Crear.</span><span class="sxs-lookup"><span data-stu-id="a9bd9-122">Enter **TodoApi** for the **Project Name**, and then select Create.</span></span>

![cuadro de diálogo de configuración](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="a9bd9-124">Iniciar la aplicación</span><span class="sxs-lookup"><span data-stu-id="a9bd9-124">Launch the app</span></span>

<span data-ttu-id="a9bd9-125">En Visual Studio, seleccione **Ejecutar > Iniciar con depuración** para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a9bd9-125">In Visual Studio, select **Run > Start With Debugging** to launch the app.</span></span> <span data-ttu-id="a9bd9-126">Visual Studio inicia un explorador y se desplaza a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="a9bd9-126">Visual Studio launches a browser and navigates to `http://localhost:5000`.</span></span> <span data-ttu-id="a9bd9-127">Obtendrá un error HTTP 404 (No encontrado).</span><span class="sxs-lookup"><span data-stu-id="a9bd9-127">You get an HTTP 404 (Not Found) error.</span></span>  <span data-ttu-id="a9bd9-128">Cambie la dirección URL a `http://localhost:port/api/values`.</span><span class="sxs-lookup"><span data-stu-id="a9bd9-128">Change the URL to `http://localhost:port/api/values`.</span></span> <span data-ttu-id="a9bd9-129">Se mostrarán los datos de `ValuesController`:</span><span class="sxs-lookup"><span data-stu-id="a9bd9-129">The `ValuesController` data will be displayed:</span></span>

```
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="a9bd9-130">Agregar compatibilidad para Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="a9bd9-130">Add support for Entity Framework Core</span></span>

<span data-ttu-id="a9bd9-131">Instale el proveedor de base de datos [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/).</span><span class="sxs-lookup"><span data-stu-id="a9bd9-131">Install the [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) database provider.</span></span> <span data-ttu-id="a9bd9-132">Este proveedor de base de datos permite usar Entity Framework Core con una base de datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="a9bd9-132">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

* <span data-ttu-id="a9bd9-133">En el menú **Proyecto**, seleccione **Agregar paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="a9bd9-133">From the **Project** menu, select **Add NuGet Packages**.</span></span> 

  *  <span data-ttu-id="a9bd9-134">Como alternativa, puede hacer clic con el botón derecho en **Dependencias** y seleccionar **Agregar paquetes**.</span><span class="sxs-lookup"><span data-stu-id="a9bd9-134">Alternately, you can right-click **Dependencies**, and then select **Add Packages**.</span></span>

* <span data-ttu-id="a9bd9-135">Escriba `EntityFrameworkCore.InMemory` en el cuadro de búsqueda.</span><span class="sxs-lookup"><span data-stu-id="a9bd9-135">Enter `EntityFrameworkCore.InMemory` in the search box.</span></span>
* <span data-ttu-id="a9bd9-136">Seleccione `Microsoft.EntityFrameworkCore.InMemory` y, luego, **Agregar paquete**.</span><span class="sxs-lookup"><span data-stu-id="a9bd9-136">Select `Microsoft.EntityFrameworkCore.InMemory`, and then select **Add Package**.</span></span>

### <a name="add-a-model-class"></a><span data-ttu-id="a9bd9-137">Agregar una clase de modelo</span><span class="sxs-lookup"><span data-stu-id="a9bd9-137">Add a model class</span></span>

<span data-ttu-id="a9bd9-138">Un modelo es un objeto que representa los datos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a9bd9-138">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="a9bd9-139">En este caso, el único modelo es una tarea pendiente.</span><span class="sxs-lookup"><span data-stu-id="a9bd9-139">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="a9bd9-140">Agregue una carpeta denominada *Modelos*.</span><span class="sxs-lookup"><span data-stu-id="a9bd9-140">Add a folder named *Models*.</span></span> <span data-ttu-id="a9bd9-141">En el Explorador de soluciones, haga clic con el botón derecho en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="a9bd9-141">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="a9bd9-142">Seleccione **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="a9bd9-142">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="a9bd9-143">Asigne a la carpeta el nombre *Modelos*.</span><span class="sxs-lookup"><span data-stu-id="a9bd9-143">Name the folder *Models*.</span></span>

![nueva carpeta](first-web-api-mac/_static/folder.png)

<span data-ttu-id="a9bd9-145">Nota: Puede colocar clases de modelo en cualquier lugar del proyecto, pero la carpeta *Modelos* se usa por convención.</span><span class="sxs-lookup"><span data-stu-id="a9bd9-145">Note: You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="a9bd9-146">Agregue una clase `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="a9bd9-146">Add a `TodoItem` class.</span></span> <span data-ttu-id="a9bd9-147">Haga clic con el botón derecho en la carpeta *Modelos* y seleccione **Agregar > Nuevo archivo > General > Clase vacía**.</span><span class="sxs-lookup"><span data-stu-id="a9bd9-147">Right-click the *Models* folder and select **Add > New File > General > Empty Class**.</span></span> <span data-ttu-id="a9bd9-148">Asigne a la clase el nombre `TodoItem` y seleccione **Nuevo**.</span><span class="sxs-lookup"><span data-stu-id="a9bd9-148">Name the class `TodoItem`, and then select **New**.</span></span>

<span data-ttu-id="a9bd9-149">Reemplace el código generado por el siguiente:</span><span class="sxs-lookup"><span data-stu-id="a9bd9-149">Replace the generated code with:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="a9bd9-150">La base de datos genera el `Id` cuando se crea una `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="a9bd9-150">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="a9bd9-151">Crear el contexto de base de datos</span><span class="sxs-lookup"><span data-stu-id="a9bd9-151">Create the database context</span></span>

<span data-ttu-id="a9bd9-152">El *contexto de base de datos* es la clase principal que coordina la funcionalidad de Entity Framework para un modelo de datos determinado.</span><span class="sxs-lookup"><span data-stu-id="a9bd9-152">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="a9bd9-153">Esta clase se crea derivándola de la clase `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="a9bd9-153">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="a9bd9-154">Agregue una clase `TodoContext` a la carpeta *Modelos*.</span><span class="sxs-lookup"><span data-stu-id="a9bd9-154">Add a `TodoContext` class to the *Models* folder.</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="a9bd9-155">Agregar un controlador</span><span class="sxs-lookup"><span data-stu-id="a9bd9-155">Add a controller</span></span>

<span data-ttu-id="a9bd9-156">En el Explorador de soluciones, en la carpeta *Controladores*, agregue la clase `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="a9bd9-156">In Solution Explorer, in the *Controllers* folder, add the class `TodoController`.</span></span>

<span data-ttu-id="a9bd9-157">Reemplace el código generado por el siguiente (y agregue llaves de cierre):</span><span class="sxs-lookup"><span data-stu-id="a9bd9-157">Replace the generated code with the following (and add closing braces):</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="a9bd9-158">Iniciar la aplicación</span><span class="sxs-lookup"><span data-stu-id="a9bd9-158">Launch the app</span></span>

<span data-ttu-id="a9bd9-159">En Visual Studio, seleccione **Ejecutar > Iniciar con depuración** para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a9bd9-159">In Visual Studio, select **Run > Start With Debugging** to launch the app.</span></span> <span data-ttu-id="a9bd9-160">Visual Studio inicia un explorador y navega hasta `http://localhost:port`, donde *port* es un número de puerto elegido aleatoriamente.</span><span class="sxs-lookup"><span data-stu-id="a9bd9-160">Visual Studio launches a browser and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span> <span data-ttu-id="a9bd9-161">Obtendrá un error HTTP 404 (No encontrado).</span><span class="sxs-lookup"><span data-stu-id="a9bd9-161">You get an HTTP 404 (Not Found) error.</span></span>  <span data-ttu-id="a9bd9-162">Cambie la dirección URL a `http://localhost:port/api/values`.</span><span class="sxs-lookup"><span data-stu-id="a9bd9-162">Change the URL to `http://localhost:port/api/values`.</span></span> <span data-ttu-id="a9bd9-163">Se mostrarán los datos de `ValuesController`:</span><span class="sxs-lookup"><span data-stu-id="a9bd9-163">The `ValuesController` data will be displayed:</span></span>

```
["value1","value2"]
```

<span data-ttu-id="a9bd9-164">Vaya al controlador `Todo` en `http://localhost:port/api/todo`:</span><span class="sxs-lookup"><span data-stu-id="a9bd9-164">Navigate to the `Todo` controller at`http://localhost:port/api/todo`:</span></span>

```
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="a9bd9-165">Implementar las otras operaciones CRUD</span><span class="sxs-lookup"><span data-stu-id="a9bd9-165">Implement the other CRUD operations</span></span>

<span data-ttu-id="a9bd9-166">Vamos a agregar los métodos `Create`, `Update` y `Delete` al controlador.</span><span class="sxs-lookup"><span data-stu-id="a9bd9-166">We'll add `Create`, `Update`, and `Delete` methods to the controller.</span></span> <span data-ttu-id="a9bd9-167">Son variaciones de un tema, así que solo mostraré el código y comentaré las diferencias principales.</span><span class="sxs-lookup"><span data-stu-id="a9bd9-167">These are variations on a theme, so I'll just show the code and highlight the main differences.</span></span> <span data-ttu-id="a9bd9-168">Compile el proyecto después de agregar o cambiar el código.</span><span class="sxs-lookup"><span data-stu-id="a9bd9-168">Build the project after adding or changing code.</span></span>

### <a name="create"></a><span data-ttu-id="a9bd9-169">Crear</span><span class="sxs-lookup"><span data-stu-id="a9bd9-169">Create</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="a9bd9-170">Se trata de un método HTTP POST, indicado por el atributo [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="a9bd9-170">This is an HTTP POST method, indicated by the [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="a9bd9-171">El atributo [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) indica a MVC que obtenga el valor de la tarea pendiente del cuerpo de la solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="a9bd9-171">The [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="a9bd9-172">El método `CreatedAtRoute` devuelve una respuesta 201, que es la respuesta estándar para un método HTTP POST que crea un nuevo recurso en el servidor.</span><span class="sxs-lookup"><span data-stu-id="a9bd9-172">The `CreatedAtRoute` method returns a 201 response, which is the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="a9bd9-173">`CreatedAtRoute` también agrega un encabezado de ubicación a la respuesta.</span><span class="sxs-lookup"><span data-stu-id="a9bd9-173">`CreatedAtRoute` also adds a Location header to the response.</span></span> <span data-ttu-id="a9bd9-174">El encabezado de ubicación especifica el URI de la tarea pendiente recién creada.</span><span class="sxs-lookup"><span data-stu-id="a9bd9-174">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="a9bd9-175">Vea [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) (10.2.2 201 creada).</span><span class="sxs-lookup"><span data-stu-id="a9bd9-175">See [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="a9bd9-176">Usar Postman para enviar una solicitud de creación</span><span class="sxs-lookup"><span data-stu-id="a9bd9-176">Use Postman to send a Create request</span></span>

* <span data-ttu-id="a9bd9-177">Inicie la aplicación (**Ejecutar > Iniciar con depuración**).</span><span class="sxs-lookup"><span data-stu-id="a9bd9-177">Start the app (**Run > Start With Debugging**).</span></span>
* <span data-ttu-id="a9bd9-178">Inicie Postman.</span><span class="sxs-lookup"><span data-stu-id="a9bd9-178">Start Postman.</span></span>

![Consola de Postman](first-web-api/_static/pmc.png)

* <span data-ttu-id="a9bd9-180">Establezca el método HTTP en `POST`.</span><span class="sxs-lookup"><span data-stu-id="a9bd9-180">Set the HTTP method to `POST`</span></span>
* <span data-ttu-id="a9bd9-181">Seleccione el botón de radio **Body** (Cuerpo).</span><span class="sxs-lookup"><span data-stu-id="a9bd9-181">Select the **Body** radio button</span></span>
* <span data-ttu-id="a9bd9-182">Seleccione el botón de radio **Raw** (Sin formato).</span><span class="sxs-lookup"><span data-stu-id="a9bd9-182">Select the **raw** radio button</span></span>
* <span data-ttu-id="a9bd9-183">Establezca el tipo en JSON.</span><span class="sxs-lookup"><span data-stu-id="a9bd9-183">Set the type to JSON</span></span>
* <span data-ttu-id="a9bd9-184">En el editor de pares clave-valor, escriba una tarea pendiente como la siguiente:</span><span class="sxs-lookup"><span data-stu-id="a9bd9-184">In the key-value editor, enter a Todo item such as</span></span>

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* <span data-ttu-id="a9bd9-185">Seleccione **Send** (Enviar).</span><span class="sxs-lookup"><span data-stu-id="a9bd9-185">Select **Send**</span></span>

* <span data-ttu-id="a9bd9-186">Seleccione la pestaña Headers (Encabezados) en el panel inferior y copie el encabezado **Location** (Ubicación):</span><span class="sxs-lookup"><span data-stu-id="a9bd9-186">Select the Headers tab in the lower pane and copy the **Location** header:</span></span>

![Pestaña Headers (Encabezados) de la consola de Postman](first-web-api/_static/pmget.png)

<span data-ttu-id="a9bd9-188">Puede usar el URI del encabezado Location (Ubicación) para acceder al recurso que acaba de crear.</span><span class="sxs-lookup"><span data-stu-id="a9bd9-188">You can use the Location header URI to access the resource you just created.</span></span> <span data-ttu-id="a9bd9-189">Recuerde que el método `GetById` creó la ruta denominada `"GetTodo"`:</span><span class="sxs-lookup"><span data-stu-id="a9bd9-189">Recall the `GetById` method created the `"GetTodo"` named route:</span></span>

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(string id)
```

### <a name="update"></a><span data-ttu-id="a9bd9-190">Actualizar</span><span class="sxs-lookup"><span data-stu-id="a9bd9-190">Update</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="a9bd9-191">`Update` es similar a `Create`, pero usa HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="a9bd9-191">`Update` is similar to `Create`, but uses HTTP PUT.</span></span> <span data-ttu-id="a9bd9-192">La respuesta es [204 Sin contenido](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="a9bd9-192">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="a9bd9-193">Según la especificación HTTP, una solicitud PUT requiere que el cliente envíe toda la entidad actualizada, no solo los deltas.</span><span class="sxs-lookup"><span data-stu-id="a9bd9-193">According to the HTTP spec, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="a9bd9-194">Para admitir actualizaciones parciales, use HTTP PATCH.</span><span class="sxs-lookup"><span data-stu-id="a9bd9-194">To support partial updates, use HTTP PATCH.</span></span>

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![Consola de Postman que muestra la respuesta 204 Sin contenido](first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="a9bd9-196">Eliminar</span><span class="sxs-lookup"><span data-stu-id="a9bd9-196">Delete</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="a9bd9-197">La respuesta es [204 Sin contenido](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="a9bd9-197">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

![Consola de Postman que muestra la respuesta 204 Sin contenido](first-web-api/_static/pmd.png)

## <a name="next-steps"></a><span data-ttu-id="a9bd9-199">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="a9bd9-199">Next steps</span></span>

* <span data-ttu-id="a9bd9-200">[Routing to Controller Actions](xref:mvc/controllers/routing) (Enrutamiento a acciones del controlador)</span><span class="sxs-lookup"><span data-stu-id="a9bd9-200">[Routing to Controller Actions](xref:mvc/controllers/routing)</span></span>
* <span data-ttu-id="a9bd9-201">Para saber más sobre la implementación de la API, vea [Publishing and Deployment](../publishing/index.md) (Publicación e implementación).</span><span class="sxs-lookup"><span data-stu-id="a9bd9-201">For information about deploying your API, see [Publishing and Deployment](../publishing/index.md).</span></span>
* <span data-ttu-id="a9bd9-202">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a9bd9-202">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>
* [<span data-ttu-id="a9bd9-203">Postman</span><span class="sxs-lookup"><span data-stu-id="a9bd9-203">Postman</span></span>](https://www.getpostman.com/)
* [<span data-ttu-id="a9bd9-204">Fiddler</span><span class="sxs-lookup"><span data-stu-id="a9bd9-204">Fiddler</span></span>](https://www.telerik.com/download/fiddler)
