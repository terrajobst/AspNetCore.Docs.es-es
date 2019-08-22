---
title: 'Tutorial: Creación de una API web con ASP.NET Core'
author: rick-anderson
description: Aprenda a crear de una API web con ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 08/14/2019
uid: tutorials/first-web-api
ms.openlocfilehash: 99985e9fb1134c2ba808434f8d24c4a768773268
ms.sourcegitcommit: 476ea5ad86a680b7b017c6f32098acd3414c0f6c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/14/2019
ms.locfileid: "69022595"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core"></a><span data-ttu-id="c4385-103">Tutorial: Creación de una API web con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c4385-103">Tutorial: Create a web API with ASP.NET Core</span></span>

<span data-ttu-id="c4385-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="c4385-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="c4385-105">En este tutorial se enseñan los conceptos básicos de la compilación de una API web con ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c4385-105">This tutorial teaches the basics of building a web API with ASP.NET Core.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="c4385-106">En este tutorial aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="c4385-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c4385-107">Crear un proyecto de API web.</span><span class="sxs-lookup"><span data-stu-id="c4385-107">Create a web API project.</span></span>
> * <span data-ttu-id="c4385-108">Agregar una clase de modelo y un contexto de base de datos.</span><span class="sxs-lookup"><span data-stu-id="c4385-108">Add a model class and a database context.</span></span>
> * <span data-ttu-id="c4385-109">Aplicar scaffolding a un controlador con métodos CRUD.</span><span class="sxs-lookup"><span data-stu-id="c4385-109">Scaffold a controller with CRUD methods.</span></span>
> * <span data-ttu-id="c4385-110">Configurar el enrutamiento, las rutas de acceso URL y los valores devueltos.</span><span class="sxs-lookup"><span data-stu-id="c4385-110">Configure routing, URL paths, and return values.</span></span>
> * <span data-ttu-id="c4385-111">Llamar a la API web con Postman.</span><span class="sxs-lookup"><span data-stu-id="c4385-111">Call the web API with Postman.</span></span>

<span data-ttu-id="c4385-112">Al final, tendrá una API web que pueda administrar los elementos "to-do" almacenados en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="c4385-112">At the end, you have a web API that can manage "to-do" items stored in a database.</span></span>

## <a name="overview"></a><span data-ttu-id="c4385-113">Información general</span><span class="sxs-lookup"><span data-stu-id="c4385-113">Overview</span></span>

<span data-ttu-id="c4385-114">En este tutorial se crea la siguiente API:</span><span class="sxs-lookup"><span data-stu-id="c4385-114">This tutorial creates the following API:</span></span>

|<span data-ttu-id="c4385-115">API</span><span class="sxs-lookup"><span data-stu-id="c4385-115">API</span></span> | <span data-ttu-id="c4385-116">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="c4385-116">Description</span></span> | <span data-ttu-id="c4385-117">Cuerpo de la solicitud</span><span class="sxs-lookup"><span data-stu-id="c4385-117">Request body</span></span> | <span data-ttu-id="c4385-118">Cuerpo de la respuesta</span><span class="sxs-lookup"><span data-stu-id="c4385-118">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="c4385-119">GET /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="c4385-119">GET /api/TodoItems</span></span> | <span data-ttu-id="c4385-120">Obtener todas las tareas pendientes</span><span class="sxs-lookup"><span data-stu-id="c4385-120">Get all to-do items</span></span> | <span data-ttu-id="c4385-121">Ninguna</span><span class="sxs-lookup"><span data-stu-id="c4385-121">None</span></span> | <span data-ttu-id="c4385-122">Matriz de tareas pendientes</span><span class="sxs-lookup"><span data-stu-id="c4385-122">Array of to-do items</span></span>|
|<span data-ttu-id="c4385-123">GET /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="c4385-123">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="c4385-124">Obtener un elemento por identificador</span><span class="sxs-lookup"><span data-stu-id="c4385-124">Get an item by ID</span></span> | <span data-ttu-id="c4385-125">None</span><span class="sxs-lookup"><span data-stu-id="c4385-125">None</span></span> | <span data-ttu-id="c4385-126">Tarea pendiente</span><span class="sxs-lookup"><span data-stu-id="c4385-126">To-do item</span></span>|
|<span data-ttu-id="c4385-127">POST /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="c4385-127">POST /api/TodoItems</span></span> | <span data-ttu-id="c4385-128">Incorporación de un nuevo elemento</span><span class="sxs-lookup"><span data-stu-id="c4385-128">Add a new item</span></span> | <span data-ttu-id="c4385-129">Tarea pendiente</span><span class="sxs-lookup"><span data-stu-id="c4385-129">To-do item</span></span> | <span data-ttu-id="c4385-130">Tarea pendiente</span><span class="sxs-lookup"><span data-stu-id="c4385-130">To-do item</span></span> |
|<span data-ttu-id="c4385-131">PUT /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="c4385-131">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="c4385-132">Actualizar un elemento existente &nbsp;</span><span class="sxs-lookup"><span data-stu-id="c4385-132">Update an existing item &nbsp;</span></span> | <span data-ttu-id="c4385-133">Tarea pendiente</span><span class="sxs-lookup"><span data-stu-id="c4385-133">To-do item</span></span> | <span data-ttu-id="c4385-134">None</span><span class="sxs-lookup"><span data-stu-id="c4385-134">None</span></span> |
|<span data-ttu-id="c4385-135">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="c4385-135">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="c4385-136">Eliminar un elemento &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="c4385-136">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="c4385-137">None</span><span class="sxs-lookup"><span data-stu-id="c4385-137">None</span></span> | <span data-ttu-id="c4385-138">None</span><span class="sxs-lookup"><span data-stu-id="c4385-138">None</span></span>|

<span data-ttu-id="c4385-139">En el diagrama siguiente, se muestra el diseño de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c4385-139">The following diagram shows the design of the app.</span></span>

![El cliente está representado por un cuadro a la izquierda.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="c4385-145">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="c4385-145">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c4385-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c4385-146">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c4385-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c4385-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c4385-148">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="c4385-148">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="c4385-149">Creación de un proyecto web</span><span class="sxs-lookup"><span data-stu-id="c4385-149">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c4385-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c4385-150">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c4385-151">En el menú **Archivo**, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="c4385-151">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="c4385-152">Seleccione la plantilla **Aplicación web ASP.NET Core** y haga clic en **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="c4385-152">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="c4385-153">Asigne al proyecto el nombre *TodoApi* y haga clic en **Crear**.</span><span class="sxs-lookup"><span data-stu-id="c4385-153">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="c4385-154">En el cuadro de diálogo **Crear una aplicación web ASP.NET Core**, confirme que las opciones **.NET Core** y **ASP.NET Core 3.0** estén seleccionadas.</span><span class="sxs-lookup"><span data-stu-id="c4385-154">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span> <span data-ttu-id="c4385-155">Seleccione la plantilla **API** y haga clic en **Crear**.</span><span class="sxs-lookup"><span data-stu-id="c4385-155">Select the **API** template and click **Create**.</span></span> <span data-ttu-id="c4385-156">**No** seleccione **Habilitar compatibilidad con Docker**.</span><span class="sxs-lookup"><span data-stu-id="c4385-156">**Don't** select **Enable Docker Support**.</span></span>

![Cuadro de diálogo de nuevo proyecto de VS](first-web-api/_static/vs3.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c4385-158">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c4385-158">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="c4385-159">Abra el [terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="c4385-159">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="c4385-160">Cambie los directorios (`cd`) a la carpeta que va a contener la carpeta del proyecto.</span><span class="sxs-lookup"><span data-stu-id="c4385-160">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="c4385-161">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="c4385-161">Run the following commands:</span></span>

   ```console
   dotnet new webapi -o TodoApi
   cd TodoAPI
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer --version 3.0.0-*
   dotnet add package Microsoft.EntityFrameworkCore.InMemory --version 3.0.0-*
   code -r ../TodoApi
   ```

* <span data-ttu-id="c4385-162">Cuando en un cuadro de diálogo se le pregunte si quiere agregar al proyecto los recursos necesarios, seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="c4385-162">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

  <span data-ttu-id="c4385-163">Los comandos anteriores:</span><span class="sxs-lookup"><span data-stu-id="c4385-163">The preceding commands:</span></span>

  * <span data-ttu-id="c4385-164">Crean un nuevo proyecto de API web y lo abren en Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="c4385-164">Creates a new web API project and opens it in Visual Studio Code.</span></span>
  * <span data-ttu-id="c4385-165">Agregan los paquetes de NuGet que se requieren en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="c4385-165">Adds the NuGet packages which are required in the next section.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c4385-166">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="c4385-166">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="c4385-167">Seleccione **Archivo** > **Nueva solución**.</span><span class="sxs-lookup"><span data-stu-id="c4385-167">Select **File** > **New Solution**.</span></span>

  ![macOS: Nueva solución](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="c4385-169">Seleccione **.NET Core** > **Aplicación** > **API** > **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="c4385-169">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![Cuadro de diálogo de nuevo proyecto de macOS](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="c4385-171">En el cuadro de diálogo **Configurar la nueva API web de ASP.NET Core**, seleccione el **Marco de trabajo de destino** de \* *.NET Core 3.0*.</span><span class="sxs-lookup"><span data-stu-id="c4385-171">In the **Configure your new ASP.NET Core Web API** dialog, select **Target Framework** of \**.NET Core 3.0*.</span></span>

* <span data-ttu-id="c4385-172">Escriba *TodoApi* en **Nombre del proyecto** y seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="c4385-172">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![cuadro de diálogo de configuración](first-web-api-mac/_static/2.png)

[!INCLUDE[](~/includes/mac-terminal-access.md)]

<span data-ttu-id="c4385-174">Abra un terminal de comandos en la carpeta del proyecto y ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="c4385-174">Open a command terminal in the project folder and run the following commands:</span></span>

   ```console
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer --version 3.0.0-*
   dotnet add package Microsoft.EntityFrameworkCore.InMemory --version 3.0.0-*
   ```

---

### <a name="test-the-api"></a><span data-ttu-id="c4385-175">Prueba de la API</span><span class="sxs-lookup"><span data-stu-id="c4385-175">Test the API</span></span>

<span data-ttu-id="c4385-176">La plantilla del proyecto crea una API `WeatherForecast`.</span><span class="sxs-lookup"><span data-stu-id="c4385-176">The project template creates a `WeatherForecast` API.</span></span> <span data-ttu-id="c4385-177">Llame al método `Get` desde un explorador para probar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c4385-177">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c4385-178">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c4385-178">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="c4385-179">Presione Ctrl+F5 para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c4385-179">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="c4385-180">Visual Studio inicia un explorador y navega hasta `https://localhost:<port>/WeatherForecast`, donde `<port>` es un número de puerto elegido aleatoriamente.</span><span class="sxs-lookup"><span data-stu-id="c4385-180">Visual Studio launches a browser and navigates to `https://localhost:<port>/WeatherForecast`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="c4385-181">Si aparece un cuadro de diálogo en que se le pregunta si debe confiar en el certificado de IIS Express, seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="c4385-181">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="c4385-182">En el cuadro de diálogo **Advertencia de seguridad** que aparece a continuación, seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="c4385-182">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c4385-183">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c4385-183">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="c4385-184">Presione Ctrl+F5 para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c4385-184">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="c4385-185">En un explorador, vaya a la siguiente dirección URL: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span><span class="sxs-lookup"><span data-stu-id="c4385-185">In a browser, go to following URL: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c4385-186">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="c4385-186">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="c4385-187">Seleccione **Ejecutar** > **Iniciar depuración** para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c4385-187">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="c4385-188">Visual Studio para Mac inicia un explorador y navega hasta `https://localhost:<port>`, donde `<port>` es un número de puerto elegido aleatoriamente.</span><span class="sxs-lookup"><span data-stu-id="c4385-188">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="c4385-189">Se devuelve un error HTTP 404 (No encontrado).</span><span class="sxs-lookup"><span data-stu-id="c4385-189">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="c4385-190">Anexe `/WeatherForecast` a la dirección URL (cambie la dirección URL a `https://localhost:<port>/WeatherForecast`).</span><span class="sxs-lookup"><span data-stu-id="c4385-190">Append `/WeatherForecast` to the URL (change the URL to `https://localhost:<port>/WeatherForecast`).</span></span>

---

<span data-ttu-id="c4385-191">Se devuelve un JSON similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="c4385-191">JSON similar to the following is returned:</span></span>

```json
[
    {
        "date": "2019-07-16T19:04:05.7257911-06:00",
        "temperatureC": 52,
        "temperatureF": 125,
        "summary": "Mild"
    },
    {
        "date": "2019-07-17T19:04:05.7258461-06:00",
        "temperatureC": 36,
        "temperatureF": 96,
        "summary": "Warm"
    },
    {
        "date": "2019-07-18T19:04:05.7258467-06:00",
        "temperatureC": 39,
        "temperatureF": 102,
        "summary": "Cool"
    },
    {
        "date": "2019-07-19T19:04:05.7258471-06:00",
        "temperatureC": 10,
        "temperatureF": 49,
        "summary": "Bracing"
    },
    {
        "date": "2019-07-20T19:04:05.7258474-06:00",
        "temperatureC": -1,
        "temperatureF": 31,
        "summary": "Chilly"
    }
]
```

## <a name="add-a-model-class"></a><span data-ttu-id="c4385-192">Incorporación de una clase de modelo</span><span class="sxs-lookup"><span data-stu-id="c4385-192">Add a model class</span></span>

<span data-ttu-id="c4385-193">Un *modelo* es un conjunto de clases que representan los datos que la aplicación administra.</span><span class="sxs-lookup"><span data-stu-id="c4385-193">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="c4385-194">El modelo para esta aplicación es una clase `TodoItem` única.</span><span class="sxs-lookup"><span data-stu-id="c4385-194">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c4385-195">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c4385-195">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c4385-196">En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="c4385-196">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="c4385-197">Seleccione **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="c4385-197">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="c4385-198">Asigne a la carpeta el nombre *Models*.</span><span class="sxs-lookup"><span data-stu-id="c4385-198">Name the folder *Models*.</span></span>

* <span data-ttu-id="c4385-199">Haga clic con el botón derecho en la carpeta *Models* y seleccione **Agregar** > **Clase**.</span><span class="sxs-lookup"><span data-stu-id="c4385-199">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="c4385-200">Asigne a la clase el nombre *TodoItem* y seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="c4385-200">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="c4385-201">Reemplace el código de plantilla por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="c4385-201">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c4385-202">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c4385-202">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="c4385-203">Agregue una carpeta denominada *Models*.</span><span class="sxs-lookup"><span data-stu-id="c4385-203">Add a folder named *Models*.</span></span>

* <span data-ttu-id="c4385-204">Agregue una clase `TodoItem` a la carpeta *Models* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="c4385-204">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c4385-205">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="c4385-205">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="c4385-206">Haga clic con el botón derecho en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="c4385-206">Right-click the project.</span></span> <span data-ttu-id="c4385-207">Seleccione **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="c4385-207">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="c4385-208">Asigne a la carpeta el nombre *Models*.</span><span class="sxs-lookup"><span data-stu-id="c4385-208">Name the folder *Models*.</span></span>

  ![nueva carpeta](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="c4385-210">Haga clic con el botón derecho en la carpeta *Modelos* y seleccione **Agregar** > **Nuevo archivo** > **General** > **Clase vacía**.</span><span class="sxs-lookup"><span data-stu-id="c4385-210">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="c4385-211">Asigne a la clase el nombre *TodoItem* y haga clic en **Nuevo**.</span><span class="sxs-lookup"><span data-stu-id="c4385-211">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="c4385-212">Reemplace el código de plantilla por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="c4385-212">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="c4385-213">La propiedad `Id` funciona como clave única en una base de datos relacional.</span><span class="sxs-lookup"><span data-stu-id="c4385-213">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="c4385-214">Las clases de modelo pueden ir en cualquier lugar del proyecto, pero convencionalmente e usa la carpeta *Models*.</span><span class="sxs-lookup"><span data-stu-id="c4385-214">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="c4385-215">Incorporación de un contexto de base de datos</span><span class="sxs-lookup"><span data-stu-id="c4385-215">Add a database context</span></span>

<span data-ttu-id="c4385-216">El *contexto de base de datos* es la clase principal que coordina la funcionalidad de Entity Framework para un modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="c4385-216">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="c4385-217">Esta clase se crea derivándola de la clase `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="c4385-217">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c4385-218">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c4385-218">Visual Studio</span></span>](#tab/visual-studio)

### <a name="add-microsoftentityframeworkcoresqlserver"></a><span data-ttu-id="c4385-219">Adición de Microsoft.EntityFrameworkCore.SqlServer</span><span class="sxs-lookup"><span data-stu-id="c4385-219">Add Microsoft.EntityFrameworkCore.SqlServer</span></span>

* <span data-ttu-id="c4385-220">En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet > Administrar paquetes NuGet para la solución**.</span><span class="sxs-lookup"><span data-stu-id="c4385-220">From the **Tools** menu, select **NuGet Package Manager > Manage NuGet Packages for Solution**.</span></span>
* <span data-ttu-id="c4385-221">Active la casilla **Incluir versión preliminar**.</span><span class="sxs-lookup"><span data-stu-id="c4385-221">Select the **Include prerelease** checkbox.</span></span>
* <span data-ttu-id="c4385-222">Seleccione la pestaña **Examinar** y escriba **Microsoft.EntityFrameworkCore.SqlServer** en el cuadro de búsqueda.</span><span class="sxs-lookup"><span data-stu-id="c4385-222">Select the **Browse** tab, and then enter **Microsoft.EntityFrameworkCore.SqlServer** in the search box.</span></span>
* <span data-ttu-id="c4385-223">Seleccione **Microsoft.EntityFrameworkCore.SqlServer V3.0.0-preview** en el panel izquierdo.</span><span class="sxs-lookup"><span data-stu-id="c4385-223">Select  **Microsoft.EntityFrameworkCore.SqlServer V3.0.0-preview** in the left pane.</span></span>
* <span data-ttu-id="c4385-224">Active la casilla **Proyecto** en el panel derecho y, después, seleccione **Instalar**.</span><span class="sxs-lookup"><span data-stu-id="c4385-224">Select the **Project** check box in the right pane and then select **Install**.</span></span>
* <span data-ttu-id="c4385-225">Siga las instrucciones anteriores para agregar el paquete NuGet `Microsoft.EntityFrameworkCore.InMemory`.</span><span class="sxs-lookup"><span data-stu-id="c4385-225">Use the preceding instructions to add the `Microsoft.EntityFrameworkCore.InMemory` NuGet package.</span></span>

![Administrador de paquetes de NuGet](first-web-api/_static/vs3NuGet.png)

## <a name="add-the-todocontext-database-context"></a><span data-ttu-id="c4385-227">Adición del contexto de la base de datos TodoContext</span><span class="sxs-lookup"><span data-stu-id="c4385-227">Add the TodoContext database context</span></span>

* <span data-ttu-id="c4385-228">Haga clic con el botón derecho en la carpeta *Models* y seleccione **Agregar** > **Clase**.</span><span class="sxs-lookup"><span data-stu-id="c4385-228">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="c4385-229">Asigne a la clase el nombre *TodoContext* y haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="c4385-229">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="c4385-230">Visual Studio Code/Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="c4385-230">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="c4385-231">Agregue una clase `TodoContext` a la carpeta *Models*.</span><span class="sxs-lookup"><span data-stu-id="c4385-231">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="c4385-232">Escriba el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="c4385-232">Enter the following code:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="c4385-233">Registro del contexto de base de datos</span><span class="sxs-lookup"><span data-stu-id="c4385-233">Register the database context</span></span>

<span data-ttu-id="c4385-234">En ASP.NET Core, los servicios (como el contexto de la base de datos) deben registrarse con el contenedor de [inserción de dependencias (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="c4385-234">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="c4385-235">El contenedor proporciona el servicio a los controladores.</span><span class="sxs-lookup"><span data-stu-id="c4385-235">The container provides the service to controllers.</span></span>

<span data-ttu-id="c4385-236">Actualice *Startup.cs* con el siguiente código resaltado:</span><span class="sxs-lookup"><span data-stu-id="c4385-236">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Startup.cs?highlight=7-8,23-24&name=snippet_all)]

<span data-ttu-id="c4385-237">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="c4385-237">The preceding code:</span></span>

* <span data-ttu-id="c4385-238">Elimina las declaraciones `using` no utilizadas.</span><span class="sxs-lookup"><span data-stu-id="c4385-238">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="c4385-239">Agrega el contexto de base de datos para el contenedor de DI.</span><span class="sxs-lookup"><span data-stu-id="c4385-239">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="c4385-240">Especifica que el contexto de base de datos usará una base de datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="c4385-240">Specifies that the database context will use an in-memory database.</span></span>

## <a name="scaffold-a-controller"></a><span data-ttu-id="c4385-241">Scaffolding de un controlador</span><span class="sxs-lookup"><span data-stu-id="c4385-241">Scaffold a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c4385-242">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c4385-242">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c4385-243">Haga clic con el botón derecho en la carpeta *Controllers*.</span><span class="sxs-lookup"><span data-stu-id="c4385-243">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="c4385-244">Seleccione **Agregar** > **Nuevo elemento con scaffold**.</span><span class="sxs-lookup"><span data-stu-id="c4385-244">Select **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="c4385-245">Seleccione **Controlador de API con acciones mediante Entity Framework** y, después, seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="c4385-245">Select **API Controller with actions, using Entity Framework**, and then select **Add**.</span></span>
* <span data-ttu-id="c4385-246">En el cuadro de diálogo **Add API Controller with actions, using Entity Framework** (Agregar controlador de API con acciones mediante Entity Framework):</span><span class="sxs-lookup"><span data-stu-id="c4385-246">In the **Add API Controller with actions, using Entity Framework** dialog:</span></span>

  * <span data-ttu-id="c4385-247">Seleccione **TodoItem (TodoAPI.Models)** en **Clase de modelo**.</span><span class="sxs-lookup"><span data-stu-id="c4385-247">Select **TodoItem (TodoAPI.Models)** in the **Model class**.</span></span>
  * <span data-ttu-id="c4385-248">Seleccione **TodoContext (TodoAPI.Models)** en **Clase de contexto de datos**.</span><span class="sxs-lookup"><span data-stu-id="c4385-248">Select **TodoContext (TodoAPI.Models)** in the **Data context class**.</span></span>
  * <span data-ttu-id="c4385-249">Seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="c4385-249">Select **Add**</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="c4385-250">Visual Studio Code/Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="c4385-250">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="c4385-251">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="c4385-251">Run the following commands:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.Design --version 3.0.0-*
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator controller -name TodoItemsController -async -api -m TodoItem -dc TodoContext  -outDir Controllers
```

<span data-ttu-id="c4385-252">Los comandos anteriores:</span><span class="sxs-lookup"><span data-stu-id="c4385-252">The preceding commands:</span></span>

* <span data-ttu-id="c4385-253">Agregan los paquetes NuGet necesarios para realizar scaffolding.</span><span class="sxs-lookup"><span data-stu-id="c4385-253">Add NuGet packages required for scaffolding.</span></span>
* <span data-ttu-id="c4385-254">Instalan el motor de scaffolding (`dotnet-aspnet-codegenerator`).</span><span class="sxs-lookup"><span data-stu-id="c4385-254">Installs the scaffolding engine (`dotnet-aspnet-codegenerator`).</span></span>
* <span data-ttu-id="c4385-255">Aplican scaffolding a `TodoItemsController`.</span><span class="sxs-lookup"><span data-stu-id="c4385-255">Scaffolds the `TodoItemsController`.</span></span>

---

<span data-ttu-id="c4385-256">El código generado:</span><span class="sxs-lookup"><span data-stu-id="c4385-256">The generated code:</span></span>

* <span data-ttu-id="c4385-257">Define una clase de controlador de API sin métodos.</span><span class="sxs-lookup"><span data-stu-id="c4385-257">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="c4385-258">Representa la clase con el atributo [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute).</span><span class="sxs-lookup"><span data-stu-id="c4385-258">Decorates the class with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="c4385-259">Este atributo indica que el controlador responde a las solicitudes de la API web.</span><span class="sxs-lookup"><span data-stu-id="c4385-259">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="c4385-260">Para información sobre comportamientos específicos que permite el atributo, consulte <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="c4385-260">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="c4385-261">Utiliza la inserción de dependencias para insertar el contexto de base de datos (`TodoContext`) en el controlador.</span><span class="sxs-lookup"><span data-stu-id="c4385-261">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="c4385-262">El contexto de base de datos se usa en cada uno de los métodos [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) del controlador.</span><span class="sxs-lookup"><span data-stu-id="c4385-262">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

## <a name="examine-the-posttodoitem-create-method"></a><span data-ttu-id="c4385-263">Examen del método create de PostTodoItem</span><span class="sxs-lookup"><span data-stu-id="c4385-263">Examine the PostTodoItem create method</span></span>

<span data-ttu-id="c4385-264">Reemplace la instrucción return en `PostTodoItem` para usar el operador [nameof](/dotnet/csharp/language-reference/operators/nameof):</span><span class="sxs-lookup"><span data-stu-id="c4385-264">Replace the return statement in the `PostTodoItem` to use the [nameof](/dotnet/csharp/language-reference/operators/nameof) operator:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Create)]

<span data-ttu-id="c4385-265">El código anterior es un método HTTP POST, según indica el atributo [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="c4385-265">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="c4385-266">El método obtiene el valor de tareas pendientes del cuerpo de la solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="c4385-266">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="c4385-267">El método <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> realiza las acciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="c4385-267">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> method:</span></span>

* <span data-ttu-id="c4385-268">Devuelve un código de estado HTTP 201 cuando se ha ejecutado correctamente.</span><span class="sxs-lookup"><span data-stu-id="c4385-268">Returns an HTTP 201 status code if successful.</span></span> <span data-ttu-id="c4385-269">HTTP 201 es la respuesta estándar para un método HTTP POST que crea un recurso en el servidor.</span><span class="sxs-lookup"><span data-stu-id="c4385-269">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="c4385-270">Agrega un encabezado [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) a la respuesta.</span><span class="sxs-lookup"><span data-stu-id="c4385-270">Adds a [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) header to the response.</span></span> <span data-ttu-id="c4385-271">El encabezado `Location` especifica el [URI](https://developer.mozilla.org/docs/Glossary/URI) de la tarea pendiente recién creada.</span><span class="sxs-lookup"><span data-stu-id="c4385-271">The `Location` header specifies the [URI](https://developer.mozilla.org/docs/Glossary/URI) of the newly created to-do item.</span></span> <span data-ttu-id="c4385-272">Para obtener más información, consulte [10.2.2 201 creado](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="c4385-272">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="c4385-273">Hace referencia a la acción `GetTodoItem` para crear el identificador URI del encabezado `Location`.</span><span class="sxs-lookup"><span data-stu-id="c4385-273">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="c4385-274">La palabra clave `nameof` de C# se usa para evitar que se codifique de forma rígida el nombre de acción en la llamada a `CreatedAtAction`.</span><span class="sxs-lookup"><span data-stu-id="c4385-274">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

### <a name="install-postman"></a><span data-ttu-id="c4385-275">Instalación de Postman</span><span class="sxs-lookup"><span data-stu-id="c4385-275">Install Postman</span></span>

<span data-ttu-id="c4385-276">En este tutorial se usa Postman para probar la API web.</span><span class="sxs-lookup"><span data-stu-id="c4385-276">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="c4385-277">Instale [Postman](https://www.getpostman.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="c4385-277">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="c4385-278">Inicie la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="c4385-278">Start the web app.</span></span>
* <span data-ttu-id="c4385-279">Inicie Postman.</span><span class="sxs-lookup"><span data-stu-id="c4385-279">Start Postman.</span></span>
* <span data-ttu-id="c4385-280">Deshabilite **Comprobación del certificado SSL**.</span><span class="sxs-lookup"><span data-stu-id="c4385-280">Disable **SSL certificate verification**</span></span>
* <span data-ttu-id="c4385-281">En **Archivo > Configuración** (pestaña \**General*), deshabilite **Comprobación del certificado SSL**.</span><span class="sxs-lookup"><span data-stu-id="c4385-281">From  **File > Settings** (\**General* tab), disable **SSL certificate verification**.</span></span>
    > [!WARNING]
    > <span data-ttu-id="c4385-282">Vuelva a habilitar la comprobación del certificado SSL tras probar el controlador.</span><span class="sxs-lookup"><span data-stu-id="c4385-282">Re-enable SSL certificate verification after testing the controller.</span></span>

<a name="post"></a>

### <a name="test-posttodoitem-with-postman"></a><span data-ttu-id="c4385-283">Prueba de PostTodoItem con Postman</span><span class="sxs-lookup"><span data-stu-id="c4385-283">Test PostTodoItem with Postman</span></span>

* <span data-ttu-id="c4385-284">Cree una nueva solicitud.</span><span class="sxs-lookup"><span data-stu-id="c4385-284">Create a new request.</span></span>
* <span data-ttu-id="c4385-285">Establezca el método HTTP en `POST`.</span><span class="sxs-lookup"><span data-stu-id="c4385-285">Set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="c4385-286">Seleccione la pestaña **Cuerpo**.</span><span class="sxs-lookup"><span data-stu-id="c4385-286">Select the **Body** tab.</span></span>
* <span data-ttu-id="c4385-287">Seleccione el botón de radio **Raw** (Sin formato).</span><span class="sxs-lookup"><span data-stu-id="c4385-287">Select the **raw** radio button.</span></span>
* <span data-ttu-id="c4385-288">Establezca el tipo en **JSON (application/json)** .</span><span class="sxs-lookup"><span data-stu-id="c4385-288">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="c4385-289">En el cuerpo de la solicitud, introduzca JSON para una tarea pendiente:</span><span class="sxs-lookup"><span data-stu-id="c4385-289">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="c4385-290">Seleccione **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="c4385-290">Select **Send**.</span></span>

  ![Postman con solicitud de creación](first-web-api/_static/3/create.png)

### <a name="test-the-location-header-uri"></a><span data-ttu-id="c4385-292">Prueba del URI del encabezado de ubicación</span><span class="sxs-lookup"><span data-stu-id="c4385-292">Test the location header URI</span></span>

* <span data-ttu-id="c4385-293">Seleccione la pestaña **Encabezados** en el panel **Respuesta**.</span><span class="sxs-lookup"><span data-stu-id="c4385-293">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="c4385-294">Copie el valor de encabezado **Ubicación**:</span><span class="sxs-lookup"><span data-stu-id="c4385-294">Copy the **Location** header value:</span></span>

  ![Pestaña Encabezados de la consola de Postman](first-web-api/_static/3/create.png)

* <span data-ttu-id="c4385-296">Establezca el método en GET.</span><span class="sxs-lookup"><span data-stu-id="c4385-296">Set the method to GET.</span></span>
* <span data-ttu-id="c4385-297">Pegue el URI (por ejemplo, `https://localhost:5001/api/TodoItems/1`).</span><span class="sxs-lookup"><span data-stu-id="c4385-297">Paste the URI (for example, `https://localhost:5001/api/TodoItems/1`)</span></span>
* <span data-ttu-id="c4385-298">Seleccione **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="c4385-298">Select **Send**.</span></span>

## <a name="examine-the-get-methods"></a><span data-ttu-id="c4385-299">Examen de los métodos GET</span><span class="sxs-lookup"><span data-stu-id="c4385-299">Examine the GET methods</span></span>

<span data-ttu-id="c4385-300">Estos métodos implementan dos puntos de conexión GET:</span><span class="sxs-lookup"><span data-stu-id="c4385-300">These methods implement two GET endpoints:</span></span>

* `GET /api/TodoItems`
* `GET /api/TodoItems/{id}`

<span data-ttu-id="c4385-301">Llame a los dos puntos de conexión desde un explorador o Postman para probar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c4385-301">Test the app by calling the two endpoints from a browser or Postman.</span></span> <span data-ttu-id="c4385-302">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="c4385-302">For example:</span></span>

* [https://localhost:5001/api/TodoItems](https://localhost:5001/api/TodoItems)
* [https://localhost:5001/api/TodoItems/1](https://localhost:5001/api/TodoItems/1)

<span data-ttu-id="c4385-303">La llamada a `GetTodoItems` genera una respuesta similar a la siguiente:</span><span class="sxs-lookup"><span data-stu-id="c4385-303">A response similar to the following is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

### <a name="test-get-with-postman"></a><span data-ttu-id="c4385-304">Prueba de Get con Postman</span><span class="sxs-lookup"><span data-stu-id="c4385-304">Test Get with Postman</span></span>

* <span data-ttu-id="c4385-305">Cree una nueva solicitud.</span><span class="sxs-lookup"><span data-stu-id="c4385-305">Create a new request.</span></span>
* <span data-ttu-id="c4385-306">Establezca el método HTTP en **GET**.</span><span class="sxs-lookup"><span data-stu-id="c4385-306">Set the HTTP method to **GET**.</span></span>
* <span data-ttu-id="c4385-307">Establezca la dirección URL de la solicitud en `https://localhost:<port>/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="c4385-307">Set the request URL to `https://localhost:<port>/api/TodoItems`.</span></span> <span data-ttu-id="c4385-308">Por ejemplo: `https://localhost:5001/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="c4385-308">For example, `https://localhost:5001/api/TodoItems`.</span></span>
* <span data-ttu-id="c4385-309">Establezca **Vista de dos paneles** en Postman.</span><span class="sxs-lookup"><span data-stu-id="c4385-309">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="c4385-310">Seleccione **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="c4385-310">Select **Send**.</span></span>

<span data-ttu-id="c4385-311">Esta aplicación utiliza una base de datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="c4385-311">This app uses an in-memory database.</span></span> <span data-ttu-id="c4385-312">Si la aplicación se detiene y se inicia, la solicitud GET precedente no devolverá ningún dato.</span><span class="sxs-lookup"><span data-stu-id="c4385-312">If the app is stopped and started, the preceding GET request will not return any data.</span></span> <span data-ttu-id="c4385-313">Si no se devuelve ningún dato, publique los datos en la aplicación con [POST](#post).</span><span class="sxs-lookup"><span data-stu-id="c4385-313">If no data is returned, [POST](#post) data to the app.</span></span>

## <a name="routing-and-url-paths"></a><span data-ttu-id="c4385-314">Enrutamiento y rutas URL</span><span class="sxs-lookup"><span data-stu-id="c4385-314">Routing and URL paths</span></span>

<span data-ttu-id="c4385-315">El atributo [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) indica un método que responde a una solicitud HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="c4385-315">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="c4385-316">La ruta de dirección URL para cada método se construye como sigue:</span><span class="sxs-lookup"><span data-stu-id="c4385-316">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="c4385-317">Comience por la cadena de plantilla en el atributo `Route` del controlador:</span><span class="sxs-lookup"><span data-stu-id="c4385-317">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=TodoController&highlight=1)]

* <span data-ttu-id="c4385-318">Reemplace `[controller]` por el nombre del controlador, que convencionalmente es el nombre de clase de controlador sin el sufijo "Controller".</span><span class="sxs-lookup"><span data-stu-id="c4385-318">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="c4385-319">En este ejemplo, el nombre de clase de controlador es **TodoItems**Controller; por tanto, el nombre del controlador es "TodoItems".</span><span class="sxs-lookup"><span data-stu-id="c4385-319">For this sample, the controller class name is **TodoItems**Controller, so the controller name is "TodoItems".</span></span> <span data-ttu-id="c4385-320">El [enrutamiento](xref:mvc/controllers/routing) en ASP.NET Core no distingue entre mayúsculas y minúsculas.</span><span class="sxs-lookup"><span data-stu-id="c4385-320">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="c4385-321">Si el atributo `[HttpGet]` tiene una plantilla de ruta (por ejemplo, `[HttpGet("products")]`), anéxela a la ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="c4385-321">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="c4385-322">En este ejemplo no se usa una plantilla.</span><span class="sxs-lookup"><span data-stu-id="c4385-322">This sample doesn't use a template.</span></span> <span data-ttu-id="c4385-323">Para más información, vea [Enrutamiento mediante atributos con atributos Http[Verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="c4385-323">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="c4385-324">En el siguiente método `GetTodoItem`, `"{id}"` es una variable de marcador de posición correspondiente al identificador único de la tarea pendiente.</span><span class="sxs-lookup"><span data-stu-id="c4385-324">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="c4385-325">Al invocar a `GetTodoItem`, el valor `"{id}"` de la dirección URL se proporciona al método en su parámetro `id`.</span><span class="sxs-lookup"><span data-stu-id="c4385-325">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="c4385-326">Valores devueltos</span><span class="sxs-lookup"><span data-stu-id="c4385-326">Return values</span></span>

<span data-ttu-id="c4385-327">El tipo de valor devuelto de los métodos `GetTodoItems` y `GetTodoItem` es [ActionResult\<T > type](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="c4385-327">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="c4385-328">ASP.NET Core serializa automáticamente el objeto a [JSON](https://www.json.org/) y escribe el JSON en el cuerpo del mensaje de respuesta.</span><span class="sxs-lookup"><span data-stu-id="c4385-328">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="c4385-329">El código de respuesta para este tipo de valor devuelto es el 200, suponiendo que no haya ninguna excepción no controlada.</span><span class="sxs-lookup"><span data-stu-id="c4385-329">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="c4385-330">Las excepciones no controladas se convierten en errores 5xx.</span><span class="sxs-lookup"><span data-stu-id="c4385-330">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="c4385-331">Los tipos de valores devueltos `ActionResult` pueden representar una gama amplia de códigos de estado HTTP.</span><span class="sxs-lookup"><span data-stu-id="c4385-331">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="c4385-332">Por ejemplo, `GetTodoItem` puede devolver dos valores de estado diferentes:</span><span class="sxs-lookup"><span data-stu-id="c4385-332">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="c4385-333">Si no hay ningún elemento que coincida con el identificador solicitado, el método devolverá un código de error 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound).</span><span class="sxs-lookup"><span data-stu-id="c4385-333">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="c4385-334">En caso contrario, el método devuelve 200 con un cuerpo de respuesta JSON.</span><span class="sxs-lookup"><span data-stu-id="c4385-334">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="c4385-335">Devolver `item` genera una respuesta HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="c4385-335">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="the-puttodoitem-method"></a><span data-ttu-id="c4385-336">Método PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="c4385-336">The PutTodoItem method</span></span>

<span data-ttu-id="c4385-337">Examine el método `PutTodoItem`:</span><span class="sxs-lookup"><span data-stu-id="c4385-337">Examine the `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Update)]

<span data-ttu-id="c4385-338">`PutTodoItem` es similar a `PostTodoItem`, salvo por el hecho de que usa HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="c4385-338">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="c4385-339">La respuesta es [204 Sin contenido](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="c4385-339">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="c4385-340">Según la especificación HTTP, una solicitud PUT requiere que el cliente envíe toda la entidad actualizada, no solo los cambios.</span><span class="sxs-lookup"><span data-stu-id="c4385-340">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="c4385-341">Para admitir actualizaciones parciales, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="c4385-341">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="c4385-342">Si recibe un error al llamar a `PutTodoItem`, llame a `GET` para asegurarse de que hay un elemento en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c4385-342">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="c4385-343">Prueba del método PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="c4385-343">Test the PutTodoItem method</span></span>

<span data-ttu-id="c4385-344">En este ejemplo se usa una base de datos en memoria que se debe iniciar cada vez que se inicia la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c4385-344">This sample uses an in-memory database that must be initialed each time the app is started.</span></span> <span data-ttu-id="c4385-345">Debe haber un elemento en la base de datos antes de que realice una llamada PUT.</span><span class="sxs-lookup"><span data-stu-id="c4385-345">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="c4385-346">Llame a GET para asegurarse de que hay un elemento en la base de datos antes de realizar una llamada PUT.</span><span class="sxs-lookup"><span data-stu-id="c4385-346">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="c4385-347">Actualice el elemento to-do que tiene el id. = 1 y establezca su nombre en "feed fish":</span><span class="sxs-lookup"><span data-stu-id="c4385-347">Update the to-do item that has ID = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="c4385-348">En la imagen siguiente, se muestra la actualización de Postman:</span><span class="sxs-lookup"><span data-stu-id="c4385-348">The following image shows the Postman update:</span></span>

![Consola de Postman que muestra la respuesta 204 (Sin contenido)](first-web-api/_static/3/pmcput.png)

## <a name="the-deletetodoitem-method"></a><span data-ttu-id="c4385-350">Método DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="c4385-350">The DeleteTodoItem method</span></span>

<span data-ttu-id="c4385-351">Examine el método `DeleteTodoItem`:</span><span class="sxs-lookup"><span data-stu-id="c4385-351">Examine the `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Delete)]

<span data-ttu-id="c4385-352">La respuesta de `DeleteTodoItem` es [204 (Sin contenido)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="c4385-352">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="c4385-353">Prueba del método DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="c4385-353">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="c4385-354">Use Postman para eliminar una tarea pendiente:</span><span class="sxs-lookup"><span data-stu-id="c4385-354">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="c4385-355">Establezca el método en `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="c4385-355">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="c4385-356">Establezca el URI del objeto que quiera eliminar, por ejemplo, `https://localhost:5001/api/TodoItems/1`.</span><span class="sxs-lookup"><span data-stu-id="c4385-356">Set the URI of the object to delete, for example `https://localhost:5001/api/TodoItems/1`</span></span>
* <span data-ttu-id="c4385-357">Seleccione **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="c4385-357">Select **Send**</span></span>

## <a name="call-the-api-from-jquery"></a><span data-ttu-id="c4385-358">Llamada a la API desde jQuery</span><span class="sxs-lookup"><span data-stu-id="c4385-358">Call the API from jQuery</span></span>

<span data-ttu-id="c4385-359">Vea [Tutorial: Llamada a una API web de ASP.NET Core con jQuery](xref:tutorials/web-api-jquery).</span><span class="sxs-lookup"><span data-stu-id="c4385-359">See [Tutorial: Call an ASP.NET Core web API with jQuery](xref:tutorials/web-api-jquery).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="c4385-360">En este tutorial aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="c4385-360">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c4385-361">Crear un proyecto de API web.</span><span class="sxs-lookup"><span data-stu-id="c4385-361">Create a web API project.</span></span>
> * <span data-ttu-id="c4385-362">Agregar una clase de modelo y un contexto de base de datos.</span><span class="sxs-lookup"><span data-stu-id="c4385-362">Add a model class and a database context.</span></span>
> * <span data-ttu-id="c4385-363">Agregar un controlador.</span><span class="sxs-lookup"><span data-stu-id="c4385-363">Add a controller.</span></span>
> * <span data-ttu-id="c4385-364">Agregar métodos CRUD.</span><span class="sxs-lookup"><span data-stu-id="c4385-364">Add CRUD methods.</span></span>
> * <span data-ttu-id="c4385-365">Configurar el enrutamiento y las rutas de dirección URL.</span><span class="sxs-lookup"><span data-stu-id="c4385-365">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="c4385-366">Especificar los valores devueltos.</span><span class="sxs-lookup"><span data-stu-id="c4385-366">Specify return values.</span></span>
> * <span data-ttu-id="c4385-367">Llamar a la API web con Postman.</span><span class="sxs-lookup"><span data-stu-id="c4385-367">Call the web API with Postman.</span></span>
> * <span data-ttu-id="c4385-368">Llamar a la API web con jQuery.</span><span class="sxs-lookup"><span data-stu-id="c4385-368">Call the web API with jQuery.</span></span>

<span data-ttu-id="c4385-369">Al final, tendrá una API web que pueda administrar las tareas "pendientes" almacenadas en una base de datos relacional.</span><span class="sxs-lookup"><span data-stu-id="c4385-369">At the end, you have a web API that can manage "to-do" items stored in a relational database.</span></span>
## <a name="overview"></a><span data-ttu-id="c4385-370">Información general</span><span class="sxs-lookup"><span data-stu-id="c4385-370">Overview</span></span>

<span data-ttu-id="c4385-371">En este tutorial se crea la siguiente API:</span><span class="sxs-lookup"><span data-stu-id="c4385-371">This tutorial creates the following API:</span></span>

|<span data-ttu-id="c4385-372">API</span><span class="sxs-lookup"><span data-stu-id="c4385-372">API</span></span> | <span data-ttu-id="c4385-373">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="c4385-373">Description</span></span> | <span data-ttu-id="c4385-374">Cuerpo de la solicitud</span><span class="sxs-lookup"><span data-stu-id="c4385-374">Request body</span></span> | <span data-ttu-id="c4385-375">Cuerpo de la respuesta</span><span class="sxs-lookup"><span data-stu-id="c4385-375">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="c4385-376">GET /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="c4385-376">GET /api/TodoItems</span></span> | <span data-ttu-id="c4385-377">Obtener todas las tareas pendientes</span><span class="sxs-lookup"><span data-stu-id="c4385-377">Get all to-do items</span></span> | <span data-ttu-id="c4385-378">Ninguna</span><span class="sxs-lookup"><span data-stu-id="c4385-378">None</span></span> | <span data-ttu-id="c4385-379">Matriz de tareas pendientes</span><span class="sxs-lookup"><span data-stu-id="c4385-379">Array of to-do items</span></span>|
|<span data-ttu-id="c4385-380">GET /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="c4385-380">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="c4385-381">Obtener un elemento por identificador</span><span class="sxs-lookup"><span data-stu-id="c4385-381">Get an item by ID</span></span> | <span data-ttu-id="c4385-382">None</span><span class="sxs-lookup"><span data-stu-id="c4385-382">None</span></span> | <span data-ttu-id="c4385-383">Tarea pendiente</span><span class="sxs-lookup"><span data-stu-id="c4385-383">To-do item</span></span>|
|<span data-ttu-id="c4385-384">POST /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="c4385-384">POST /api/TodoItems</span></span> | <span data-ttu-id="c4385-385">Incorporación de un nuevo elemento</span><span class="sxs-lookup"><span data-stu-id="c4385-385">Add a new item</span></span> | <span data-ttu-id="c4385-386">Tarea pendiente</span><span class="sxs-lookup"><span data-stu-id="c4385-386">To-do item</span></span> | <span data-ttu-id="c4385-387">Tarea pendiente</span><span class="sxs-lookup"><span data-stu-id="c4385-387">To-do item</span></span> |
|<span data-ttu-id="c4385-388">PUT /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="c4385-388">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="c4385-389">Actualizar un elemento existente &nbsp;</span><span class="sxs-lookup"><span data-stu-id="c4385-389">Update an existing item &nbsp;</span></span> | <span data-ttu-id="c4385-390">Tarea pendiente</span><span class="sxs-lookup"><span data-stu-id="c4385-390">To-do item</span></span> | <span data-ttu-id="c4385-391">None</span><span class="sxs-lookup"><span data-stu-id="c4385-391">None</span></span> |
|<span data-ttu-id="c4385-392">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="c4385-392">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="c4385-393">Eliminar un elemento &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="c4385-393">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="c4385-394">None</span><span class="sxs-lookup"><span data-stu-id="c4385-394">None</span></span> | <span data-ttu-id="c4385-395">None</span><span class="sxs-lookup"><span data-stu-id="c4385-395">None</span></span>|

<span data-ttu-id="c4385-396">En el diagrama siguiente, se muestra el diseño de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c4385-396">The following diagram shows the design of the app.</span></span>

![El cliente está representado por un cuadro a la izquierda.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="c4385-402">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="c4385-402">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c4385-403">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c4385-403">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c4385-404">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c4385-404">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c4385-405">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="c4385-405">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="c4385-406">Creación de un proyecto web</span><span class="sxs-lookup"><span data-stu-id="c4385-406">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c4385-407">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c4385-407">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c4385-408">En el menú **Archivo**, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="c4385-408">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="c4385-409">Seleccione la plantilla **Aplicación web ASP.NET Core** y haga clic en **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="c4385-409">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="c4385-410">Asigne al proyecto el nombre *TodoApi* y haga clic en **Crear**.</span><span class="sxs-lookup"><span data-stu-id="c4385-410">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="c4385-411">En el cuadro de diálogo **Crear una aplicación web ASP.NET Core**, confirme que las opciones **.NET Core** y **ASP.NET Core 2.2** estén seleccionadas.</span><span class="sxs-lookup"><span data-stu-id="c4385-411">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 2.2** are selected.</span></span> <span data-ttu-id="c4385-412">Seleccione la plantilla **API** y haga clic en **Crear**.</span><span class="sxs-lookup"><span data-stu-id="c4385-412">Select the **API** template and click **Create**.</span></span> <span data-ttu-id="c4385-413">**No** seleccione **Habilitar compatibilidad con Docker**.</span><span class="sxs-lookup"><span data-stu-id="c4385-413">**Don't** select **Enable Docker Support**.</span></span>

![Cuadro de diálogo de nuevo proyecto de VS](first-web-api/_static/vs.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c4385-415">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c4385-415">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="c4385-416">Abra el [terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="c4385-416">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="c4385-417">Cambie los directorios (`cd`) a la carpeta que va a contener la carpeta del proyecto.</span><span class="sxs-lookup"><span data-stu-id="c4385-417">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="c4385-418">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="c4385-418">Run the following commands:</span></span>

   ```console
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  <span data-ttu-id="c4385-419">Estos comandos crean un nuevo proyecto de API web y abren una nueva instancia de Visual Studio Code en la carpeta del proyecto nuevo.</span><span class="sxs-lookup"><span data-stu-id="c4385-419">These commands create a new web API project and open a new instance of Visual Studio Code in the new project folder.</span></span>

* <span data-ttu-id="c4385-420">Cuando en un cuadro de diálogo se le pregunte si quiere agregar al proyecto los recursos necesarios, seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="c4385-420">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c4385-421">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="c4385-421">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="c4385-422">Seleccione **Archivo** > **Nueva solución**.</span><span class="sxs-lookup"><span data-stu-id="c4385-422">Select **File** > **New Solution**.</span></span>

  ![macOS: Nueva solución](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="c4385-424">Seleccione **.NET Core** > **Aplicación** > **API** > **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="c4385-424">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![Cuadro de diálogo de nuevo proyecto de macOS](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="c4385-426">En el cuadro de diálogo **Configurar la nueva API web de ASP.NET Core**, acepte la **plataforma de destino** predeterminada de \* *.NET Core 2.2*.</span><span class="sxs-lookup"><span data-stu-id="c4385-426">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="c4385-427">Escriba *TodoApi* en **Nombre del proyecto** y seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="c4385-427">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![cuadro de diálogo de configuración](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a><span data-ttu-id="c4385-429">Prueba de la API</span><span class="sxs-lookup"><span data-stu-id="c4385-429">Test the API</span></span>

<span data-ttu-id="c4385-430">La plantilla del proyecto crea una API `values`.</span><span class="sxs-lookup"><span data-stu-id="c4385-430">The project template creates a `values` API.</span></span> <span data-ttu-id="c4385-431">Llame al método `Get` desde un explorador para probar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c4385-431">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c4385-432">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c4385-432">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="c4385-433">Presione Ctrl+F5 para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c4385-433">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="c4385-434">Visual Studio inicia un explorador y navega hasta `https://localhost:<port>/api/values`, donde `<port>` es un número de puerto elegido aleatoriamente.</span><span class="sxs-lookup"><span data-stu-id="c4385-434">Visual Studio launches a browser and navigates to `https://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="c4385-435">Si aparece un cuadro de diálogo en que se le pregunta si debe confiar en el certificado de IIS Express, seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="c4385-435">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="c4385-436">En el cuadro de diálogo **Advertencia de seguridad** que aparece a continuación, seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="c4385-436">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c4385-437">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c4385-437">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="c4385-438">Presione Ctrl+F5 para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c4385-438">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="c4385-439">En un explorador, vaya a la siguiente dirección URL: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span><span class="sxs-lookup"><span data-stu-id="c4385-439">In a browser, go to following URL: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c4385-440">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="c4385-440">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="c4385-441">Seleccione **Ejecutar** > **Iniciar depuración** para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c4385-441">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="c4385-442">Visual Studio para Mac inicia un explorador y navega hasta `https://localhost:<port>`, donde `<port>` es un número de puerto elegido aleatoriamente.</span><span class="sxs-lookup"><span data-stu-id="c4385-442">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="c4385-443">Se devuelve un error HTTP 404 (No encontrado).</span><span class="sxs-lookup"><span data-stu-id="c4385-443">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="c4385-444">Anexe `/api/values` a la dirección URL (cambie la dirección URL a `https://localhost:<port>/api/values`).</span><span class="sxs-lookup"><span data-stu-id="c4385-444">Append `/api/values` to the URL (change the URL to `https://localhost:<port>/api/values`).</span></span>

---

<span data-ttu-id="c4385-445">Se devuelve el siguiente JSON:</span><span class="sxs-lookup"><span data-stu-id="c4385-445">The following JSON is returned:</span></span>

```json
["value1","value2"]
```

## <a name="add-a-model-class"></a><span data-ttu-id="c4385-446">Incorporación de una clase de modelo</span><span class="sxs-lookup"><span data-stu-id="c4385-446">Add a model class</span></span>

<span data-ttu-id="c4385-447">Un *modelo* es un conjunto de clases que representan los datos que la aplicación administra.</span><span class="sxs-lookup"><span data-stu-id="c4385-447">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="c4385-448">El modelo para esta aplicación es una clase `TodoItem` única.</span><span class="sxs-lookup"><span data-stu-id="c4385-448">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c4385-449">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c4385-449">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c4385-450">En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="c4385-450">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="c4385-451">Seleccione **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="c4385-451">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="c4385-452">Asigne a la carpeta el nombre *Models*.</span><span class="sxs-lookup"><span data-stu-id="c4385-452">Name the folder *Models*.</span></span>

* <span data-ttu-id="c4385-453">Haga clic con el botón derecho en la carpeta *Models* y seleccione **Agregar** > **Clase**.</span><span class="sxs-lookup"><span data-stu-id="c4385-453">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="c4385-454">Asigne a la clase el nombre *TodoItem* y seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="c4385-454">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="c4385-455">Reemplace el código de plantilla por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="c4385-455">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c4385-456">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c4385-456">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="c4385-457">Agregue una carpeta denominada *Models*.</span><span class="sxs-lookup"><span data-stu-id="c4385-457">Add a folder named *Models*.</span></span>

* <span data-ttu-id="c4385-458">Agregue una clase `TodoItem` a la carpeta *Models* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="c4385-458">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c4385-459">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="c4385-459">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="c4385-460">Haga clic con el botón derecho en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="c4385-460">Right-click the project.</span></span> <span data-ttu-id="c4385-461">Seleccione **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="c4385-461">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="c4385-462">Asigne a la carpeta el nombre *Models*.</span><span class="sxs-lookup"><span data-stu-id="c4385-462">Name the folder *Models*.</span></span>

  ![nueva carpeta](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="c4385-464">Haga clic con el botón derecho en la carpeta *Modelos* y seleccione **Agregar** > **Nuevo archivo** > **General** > **Clase vacía**.</span><span class="sxs-lookup"><span data-stu-id="c4385-464">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="c4385-465">Asigne a la clase el nombre *TodoItem* y haga clic en **Nuevo**.</span><span class="sxs-lookup"><span data-stu-id="c4385-465">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="c4385-466">Reemplace el código de plantilla por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="c4385-466">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="c4385-467">La propiedad `Id` funciona como clave única en una base de datos relacional.</span><span class="sxs-lookup"><span data-stu-id="c4385-467">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="c4385-468">Las clases de modelo pueden ir en cualquier lugar del proyecto, pero convencionalmente e usa la carpeta *Models*.</span><span class="sxs-lookup"><span data-stu-id="c4385-468">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="c4385-469">Incorporación de un contexto de base de datos</span><span class="sxs-lookup"><span data-stu-id="c4385-469">Add a database context</span></span>

<span data-ttu-id="c4385-470">El *contexto de base de datos* es la clase principal que coordina la funcionalidad de Entity Framework para un modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="c4385-470">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="c4385-471">Esta clase se crea derivándola de la clase `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="c4385-471">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c4385-472">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c4385-472">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c4385-473">Haga clic con el botón derecho en la carpeta *Models* y seleccione **Agregar** > **Clase**.</span><span class="sxs-lookup"><span data-stu-id="c4385-473">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="c4385-474">Asigne a la clase el nombre *TodoContext* y haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="c4385-474">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="c4385-475">Visual Studio Code/Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="c4385-475">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="c4385-476">Agregue una clase `TodoContext` a la carpeta *Models*.</span><span class="sxs-lookup"><span data-stu-id="c4385-476">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="c4385-477">Reemplace el código de plantilla por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="c4385-477">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="c4385-478">Registro del contexto de base de datos</span><span class="sxs-lookup"><span data-stu-id="c4385-478">Register the database context</span></span>

<span data-ttu-id="c4385-479">En ASP.NET Core, los servicios (como el contexto de la base de datos) deben registrarse con el contenedor de [inserción de dependencias (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="c4385-479">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="c4385-480">El contenedor proporciona el servicio a los controladores.</span><span class="sxs-lookup"><span data-stu-id="c4385-480">The container provides the service to controllers.</span></span>

<span data-ttu-id="c4385-481">Actualice *Startup.cs* con el siguiente código resaltado:</span><span class="sxs-lookup"><span data-stu-id="c4385-481">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup1.cs?highlight=5,8,25-26&name=snippet_all)]

<span data-ttu-id="c4385-482">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="c4385-482">The preceding code:</span></span>

* <span data-ttu-id="c4385-483">Elimina las declaraciones `using` no utilizadas.</span><span class="sxs-lookup"><span data-stu-id="c4385-483">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="c4385-484">Agrega el contexto de base de datos para el contenedor de DI.</span><span class="sxs-lookup"><span data-stu-id="c4385-484">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="c4385-485">Especifica que el contexto de base de datos usará una base de datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="c4385-485">Specifies that the database context will use an in-memory database.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="c4385-486">Incorporación de un controlador</span><span class="sxs-lookup"><span data-stu-id="c4385-486">Add a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c4385-487">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c4385-487">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c4385-488">Haga clic con el botón derecho en la carpeta *Controllers*.</span><span class="sxs-lookup"><span data-stu-id="c4385-488">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="c4385-489">Seleccione **Agregar** > **Nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="c4385-489">Select **Add** > **New Item**.</span></span>
* <span data-ttu-id="c4385-490">En el cuadro de diálogo **Agregar nuevo elemento**, seleccione la plantilla **Clase de controlador de API**.</span><span class="sxs-lookup"><span data-stu-id="c4385-490">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span>
* <span data-ttu-id="c4385-491">Asigne a la clase el nombre *TodoController* y seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="c4385-491">Name the class *TodoController*, and select **Add**.</span></span>

  ![Cuadro de diálogo Agregar nuevo elemento con la palabra "controller" en el cuadro de búsqueda y la opción Clase de controlador de API web seleccionada](first-web-api/_static/new_controller.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="c4385-493">Visual Studio Code/Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="c4385-493">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="c4385-494">En la carpeta *Controllers*, cree una clase denominada `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="c4385-494">In the *Controllers* folder, create a class named `TodoController`.</span></span>

---

* <span data-ttu-id="c4385-495">Reemplace el código de plantilla por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="c4385-495">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="c4385-496">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="c4385-496">The preceding code:</span></span>

* <span data-ttu-id="c4385-497">Define una clase de controlador de API sin métodos.</span><span class="sxs-lookup"><span data-stu-id="c4385-497">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="c4385-498">Representa la clase con el atributo [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute).</span><span class="sxs-lookup"><span data-stu-id="c4385-498">Decorates the class with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="c4385-499">Este atributo indica que el controlador responde a las solicitudes de la API web.</span><span class="sxs-lookup"><span data-stu-id="c4385-499">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="c4385-500">Para información sobre comportamientos específicos que permite el atributo, consulte <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="c4385-500">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="c4385-501">Utiliza la inserción de dependencias para insertar el contexto de base de datos (`TodoContext`) en el controlador.</span><span class="sxs-lookup"><span data-stu-id="c4385-501">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="c4385-502">El contexto de base de datos se usa en cada uno de los métodos [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) del controlador.</span><span class="sxs-lookup"><span data-stu-id="c4385-502">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="c4385-503">Si la base de datos está vacía, le agrega un elemento denominado `Item1`.</span><span class="sxs-lookup"><span data-stu-id="c4385-503">Adds an item named `Item1` to the database if the database is empty.</span></span> <span data-ttu-id="c4385-504">Este código está en el constructor, de manera que se ejecuta cada vez que hay una nueva solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="c4385-504">This code is in the constructor, so it runs every time there's a new HTTP request.</span></span> <span data-ttu-id="c4385-505">Si elimina todos los elementos, el constructor volverá a crear `Item1` la próxima vez que se llame a un método de API.</span><span class="sxs-lookup"><span data-stu-id="c4385-505">If you delete all items, the constructor creates `Item1` again the next time an API method is called.</span></span> <span data-ttu-id="c4385-506">De este modo, es posible que parezca que la eliminación no ha funcionado, cuando en realidad sí lo ha hecho.</span><span class="sxs-lookup"><span data-stu-id="c4385-506">So it may look like the deletion didn't work when it actually did work.</span></span>

## <a name="add-get-methods"></a><span data-ttu-id="c4385-507">Incorporación de métodos Get</span><span class="sxs-lookup"><span data-stu-id="c4385-507">Add Get methods</span></span>

<span data-ttu-id="c4385-508">Para proporcionar una API que recupere tareas pendientes, agregue estos métodos a la clase `TodoController`:</span><span class="sxs-lookup"><span data-stu-id="c4385-508">To provide an API that retrieves to-do items, add the following methods to the `TodoController` class:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="c4385-509">Estos métodos implementan dos puntos de conexión GET:</span><span class="sxs-lookup"><span data-stu-id="c4385-509">These methods implement two GET endpoints:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="c4385-510">Detenga la aplicación si todavía está en ejecución.</span><span class="sxs-lookup"><span data-stu-id="c4385-510">Stop the app if it's still running.</span></span> <span data-ttu-id="c4385-511">Luego, ejecútela de nuevo para incluir los últimos cambios.</span><span class="sxs-lookup"><span data-stu-id="c4385-511">Then run it again to include the latest changes.</span></span>

<span data-ttu-id="c4385-512">Llame a los dos puntos de conexión desde un explorador para probar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c4385-512">Test the app by calling the two endpoints from a browser.</span></span> <span data-ttu-id="c4385-513">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="c4385-513">For example:</span></span>

* `https://localhost:<port>/api/todo`
* `https://localhost:<port>/api/todo/1`

<span data-ttu-id="c4385-514">La llamada a `GetTodoItems` genera la siguiente respuesta HTTP:</span><span class="sxs-lookup"><span data-stu-id="c4385-514">The following HTTP response is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

## <a name="routing-and-url-paths"></a><span data-ttu-id="c4385-515">Enrutamiento y rutas URL</span><span class="sxs-lookup"><span data-stu-id="c4385-515">Routing and URL paths</span></span>

<span data-ttu-id="c4385-516">El atributo [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) indica un método que responde a una solicitud HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="c4385-516">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="c4385-517">La ruta de dirección URL para cada método se construye como sigue:</span><span class="sxs-lookup"><span data-stu-id="c4385-517">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="c4385-518">Comience por la cadena de plantilla en el atributo `Route` del controlador:</span><span class="sxs-lookup"><span data-stu-id="c4385-518">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="c4385-519">Reemplace `[controller]` por el nombre del controlador, que convencionalmente es el nombre de clase de controlador sin el sufijo "Controller".</span><span class="sxs-lookup"><span data-stu-id="c4385-519">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="c4385-520">En este ejemplo, el nombre de clase de controlador es **Todo**Controller; por tanto, el nombre del controlador es "todo".</span><span class="sxs-lookup"><span data-stu-id="c4385-520">For this sample, the controller class name is **Todo**Controller, so the controller name is "todo".</span></span> <span data-ttu-id="c4385-521">El [enrutamiento](xref:mvc/controllers/routing) en ASP.NET Core no distingue entre mayúsculas y minúsculas.</span><span class="sxs-lookup"><span data-stu-id="c4385-521">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="c4385-522">Si el atributo `[HttpGet]` tiene una plantilla de ruta (por ejemplo, `[HttpGet("products")]`), anéxela a la ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="c4385-522">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="c4385-523">En este ejemplo no se usa una plantilla.</span><span class="sxs-lookup"><span data-stu-id="c4385-523">This sample doesn't use a template.</span></span> <span data-ttu-id="c4385-524">Para más información, vea [Enrutamiento mediante atributos con atributos Http[Verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="c4385-524">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="c4385-525">En el siguiente método `GetTodoItem`, `"{id}"` es una variable de marcador de posición correspondiente al identificador único de la tarea pendiente.</span><span class="sxs-lookup"><span data-stu-id="c4385-525">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="c4385-526">Al invocar a `GetTodoItem`, el valor `"{id}"` de la dirección URL se proporciona al método en su parámetro `id`.</span><span class="sxs-lookup"><span data-stu-id="c4385-526">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="c4385-527">Valores devueltos</span><span class="sxs-lookup"><span data-stu-id="c4385-527">Return values</span></span>

<span data-ttu-id="c4385-528">El tipo de valor devuelto de los métodos `GetTodoItems` y `GetTodoItem` es [ActionResult\<T > type](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="c4385-528">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="c4385-529">ASP.NET Core serializa automáticamente el objeto a [JSON](https://www.json.org/) y escribe el JSON en el cuerpo del mensaje de respuesta.</span><span class="sxs-lookup"><span data-stu-id="c4385-529">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="c4385-530">El código de respuesta para este tipo de valor devuelto es el 200, suponiendo que no haya ninguna excepción no controlada.</span><span class="sxs-lookup"><span data-stu-id="c4385-530">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="c4385-531">Las excepciones no controladas se convierten en errores 5xx.</span><span class="sxs-lookup"><span data-stu-id="c4385-531">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="c4385-532">Los tipos de valores devueltos `ActionResult` pueden representar una gama amplia de códigos de estado HTTP.</span><span class="sxs-lookup"><span data-stu-id="c4385-532">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="c4385-533">Por ejemplo, `GetTodoItem` puede devolver dos valores de estado diferentes:</span><span class="sxs-lookup"><span data-stu-id="c4385-533">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="c4385-534">Si no hay ningún elemento que coincida con el identificador solicitado, el método devolverá un código de error 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound).</span><span class="sxs-lookup"><span data-stu-id="c4385-534">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="c4385-535">En caso contrario, el método devuelve 200 con un cuerpo de respuesta JSON.</span><span class="sxs-lookup"><span data-stu-id="c4385-535">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="c4385-536">Devolver `item` genera una respuesta HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="c4385-536">Returning `item` results in an HTTP 200 response.</span></span>


## <a name="test-the-gettodoitems-method"></a><span data-ttu-id="c4385-537">Prueba del método GetTodoItems</span><span class="sxs-lookup"><span data-stu-id="c4385-537">Test the GetTodoItems method</span></span>

<span data-ttu-id="c4385-538">En este tutorial se usa Postman para probar la API web.</span><span class="sxs-lookup"><span data-stu-id="c4385-538">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="c4385-539">Instale [Postman](https://www.getpostman.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="c4385-539">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="c4385-540">Inicie la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="c4385-540">Start the web app.</span></span>
* <span data-ttu-id="c4385-541">Inicie Postman.</span><span class="sxs-lookup"><span data-stu-id="c4385-541">Start Postman.</span></span>
* <span data-ttu-id="c4385-542">Deshabilite **Comprobación del certificado SSL**.</span><span class="sxs-lookup"><span data-stu-id="c4385-542">Disable **SSL certificate verification**</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c4385-543">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c4385-543">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c4385-544">En **Archivo** > **Configuración** (pestaña **General**), deshabilite **Comprobación del certificado SSL**.</span><span class="sxs-lookup"><span data-stu-id="c4385-544">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="c4385-545">Visual Studio Code/Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="c4385-545">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="c4385-546">En **Postman** > **Preferencias** (pestaña **General**), deshabilite **Comprobación del certificado SSL**.</span><span class="sxs-lookup"><span data-stu-id="c4385-546">From **Postman** > **Preferences** (**General** tab), disable **SSL certificate verification**.</span></span> <span data-ttu-id="c4385-547">También puede seleccionar la llave inglesa, hacer clic en **Configuración** y deshabilitar la comprobación del certificado SSL.</span><span class="sxs-lookup"><span data-stu-id="c4385-547">Alternatively, select the wrench and select **Settings**, then disable the SSL certificate verification.</span></span>

---
  
> [!WARNING]
> <span data-ttu-id="c4385-548">Vuelva a habilitar la comprobación del certificado SSL tras probar el controlador.</span><span class="sxs-lookup"><span data-stu-id="c4385-548">Re-enable SSL certificate verification after testing the controller.</span></span>

* <span data-ttu-id="c4385-549">Cree una nueva solicitud.</span><span class="sxs-lookup"><span data-stu-id="c4385-549">Create a new request.</span></span>
  * <span data-ttu-id="c4385-550">Establezca el método HTTP en **GET**.</span><span class="sxs-lookup"><span data-stu-id="c4385-550">Set the HTTP method to **GET**.</span></span>
  * <span data-ttu-id="c4385-551">Establezca la dirección URL de la solicitud en `https://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="c4385-551">Set the request URL to `https://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="c4385-552">Por ejemplo: `https://localhost:5001/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="c4385-552">For example, `https://localhost:5001/api/todo`.</span></span>
* <span data-ttu-id="c4385-553">Establezca **Vista de dos paneles** en Postman.</span><span class="sxs-lookup"><span data-stu-id="c4385-553">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="c4385-554">Seleccione **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="c4385-554">Select **Send**.</span></span>

![Postman con solicitud Get](first-web-api/_static/2pv.png)

## <a name="add-a-create-method"></a><span data-ttu-id="c4385-556">Incorporación de un método Create</span><span class="sxs-lookup"><span data-stu-id="c4385-556">Add a Create method</span></span>

<span data-ttu-id="c4385-557">Agregue el siguiente método `PostTodoItem` en *Controllers/TodoController.cs*:</span><span class="sxs-lookup"><span data-stu-id="c4385-557">Add the following `PostTodoItem` method inside of *Controllers/TodoController.cs*:</span></span> 

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="c4385-558">El código anterior es un método HTTP POST, según indica el atributo [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="c4385-558">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="c4385-559">El método obtiene el valor de tareas pendientes del cuerpo de la solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="c4385-559">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="c4385-560">El método `CreatedAtAction` realiza las acciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="c4385-560">The `CreatedAtAction` method:</span></span>

* <span data-ttu-id="c4385-561">Devuelve un código de estado HTTP 201 cuando se ha ejecutado correctamente.</span><span class="sxs-lookup"><span data-stu-id="c4385-561">Returns an HTTP 201 status code, if successful.</span></span> <span data-ttu-id="c4385-562">HTTP 201 es la respuesta estándar para un método HTTP POST que crea un recurso en el servidor.</span><span class="sxs-lookup"><span data-stu-id="c4385-562">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="c4385-563">Agrega un encabezado `Location` a la respuesta.</span><span class="sxs-lookup"><span data-stu-id="c4385-563">Adds a `Location` header to the response.</span></span> <span data-ttu-id="c4385-564">El encabezado `Location` especifica el identificador URI de la tarea pendiente recién creada.</span><span class="sxs-lookup"><span data-stu-id="c4385-564">The `Location` header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="c4385-565">Para obtener más información, consulte [10.2.2 201 creado](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="c4385-565">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="c4385-566">Hace referencia a la acción `GetTodoItem` para crear el identificador URI del encabezado `Location`.</span><span class="sxs-lookup"><span data-stu-id="c4385-566">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="c4385-567">La palabra clave `nameof` de C# se usa para evitar que se codifique de forma rígida el nombre de acción en la llamada a `CreatedAtAction`.</span><span class="sxs-lookup"><span data-stu-id="c4385-567">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method"></a><span data-ttu-id="c4385-568">Prueba del método PostTodoItem</span><span class="sxs-lookup"><span data-stu-id="c4385-568">Test the PostTodoItem method</span></span>

* <span data-ttu-id="c4385-569">Compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="c4385-569">Build the project.</span></span>
* <span data-ttu-id="c4385-570">En Postman, establezca el método HTTP en `POST`.</span><span class="sxs-lookup"><span data-stu-id="c4385-570">In Postman, set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="c4385-571">Seleccione la pestaña **Cuerpo**.</span><span class="sxs-lookup"><span data-stu-id="c4385-571">Select the **Body** tab.</span></span>
* <span data-ttu-id="c4385-572">Seleccione el botón de radio **Raw** (Sin formato).</span><span class="sxs-lookup"><span data-stu-id="c4385-572">Select the **raw** radio button.</span></span>
* <span data-ttu-id="c4385-573">Establezca el tipo en **JSON (application/json)** .</span><span class="sxs-lookup"><span data-stu-id="c4385-573">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="c4385-574">En el cuerpo de la solicitud, introduzca JSON para una tarea pendiente:</span><span class="sxs-lookup"><span data-stu-id="c4385-574">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="c4385-575">Seleccione **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="c4385-575">Select **Send**.</span></span>

  ![Postman con solicitud de creación](first-web-api/_static/create.png)

  <span data-ttu-id="c4385-577">Si recibe un error 405 (Método no permitido), probablemente sea el resultado de no haber compilado el proyecto después de agregar el método `PostTodoItem`.</span><span class="sxs-lookup"><span data-stu-id="c4385-577">If you get a 405 Method Not Allowed error, it's probably the result of not compiling the project after adding the `PostTodoItem` method.</span></span>

### <a name="test-the-location-header-uri"></a><span data-ttu-id="c4385-578">Prueba del URI del encabezado de ubicación</span><span class="sxs-lookup"><span data-stu-id="c4385-578">Test the location header URI</span></span>

* <span data-ttu-id="c4385-579">Seleccione la pestaña **Encabezados** en el panel **Respuesta**.</span><span class="sxs-lookup"><span data-stu-id="c4385-579">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="c4385-580">Copie el valor de encabezado **Ubicación**:</span><span class="sxs-lookup"><span data-stu-id="c4385-580">Copy the **Location** header value:</span></span>

  ![Pestaña Encabezados de la consola de Postman](first-web-api/_static/pmc2.png)

* <span data-ttu-id="c4385-582">Establezca el método en GET.</span><span class="sxs-lookup"><span data-stu-id="c4385-582">Set the method to GET.</span></span>
* <span data-ttu-id="c4385-583">Pegue el URI (por ejemplo, `https://localhost:5001/api/Todo/2`).</span><span class="sxs-lookup"><span data-stu-id="c4385-583">Paste the URI (for example, `https://localhost:5001/api/Todo/2`)</span></span>
* <span data-ttu-id="c4385-584">Seleccione **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="c4385-584">Select **Send**.</span></span>

## <a name="add-a-puttodoitem-method"></a><span data-ttu-id="c4385-585">Incorporación de un método PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="c4385-585">Add a PutTodoItem method</span></span>

<span data-ttu-id="c4385-586">Agregue el siguiente método `PutTodoItem`:</span><span class="sxs-lookup"><span data-stu-id="c4385-586">Add the following `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="c4385-587">`PutTodoItem` es similar a `PostTodoItem`, salvo por el hecho de que usa HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="c4385-587">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="c4385-588">La respuesta es [204 Sin contenido](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="c4385-588">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="c4385-589">Según la especificación HTTP, una solicitud PUT requiere que el cliente envíe toda la entidad actualizada, no solo los cambios.</span><span class="sxs-lookup"><span data-stu-id="c4385-589">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="c4385-590">Para admitir actualizaciones parciales, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="c4385-590">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="c4385-591">Si recibe un error al llamar a `PutTodoItem`, llame a `GET` para asegurarse de que hay un elemento en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c4385-591">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="c4385-592">Prueba del método PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="c4385-592">Test the PutTodoItem method</span></span>

<span data-ttu-id="c4385-593">En este ejemplo se usa una base de datos en memoria que se debe iniciar cada vez que se inicia la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c4385-593">This sample uses an in-memory database that must be initialed each time the app is started.</span></span> <span data-ttu-id="c4385-594">Debe haber un elemento en la base de datos antes de que realice una llamada PUT.</span><span class="sxs-lookup"><span data-stu-id="c4385-594">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="c4385-595">Llame a GET para asegurarse de que hay un elemento en la base de datos antes de realizar una llamada PUT.</span><span class="sxs-lookup"><span data-stu-id="c4385-595">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="c4385-596">Actualice la tarea pendiente que tiene el id. = 1 y establezca su nombre en "feed fish":</span><span class="sxs-lookup"><span data-stu-id="c4385-596">Update the to-do item that has id = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="c4385-597">En la imagen siguiente, se muestra la actualización de Postman:</span><span class="sxs-lookup"><span data-stu-id="c4385-597">The following image shows the Postman update:</span></span>

![Consola de Postman que muestra la respuesta 204 (Sin contenido)](first-web-api/_static/pmcput.png)

## <a name="add-a-deletetodoitem-method"></a><span data-ttu-id="c4385-599">Incorporación de un método DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="c4385-599">Add a DeleteTodoItem method</span></span>

<span data-ttu-id="c4385-600">Agregue el siguiente método `DeleteTodoItem`:</span><span class="sxs-lookup"><span data-stu-id="c4385-600">Add the following `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="c4385-601">La respuesta de `DeleteTodoItem` es [204 (Sin contenido)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="c4385-601">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="c4385-602">Prueba del método DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="c4385-602">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="c4385-603">Use Postman para eliminar una tarea pendiente:</span><span class="sxs-lookup"><span data-stu-id="c4385-603">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="c4385-604">Establezca el método en `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="c4385-604">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="c4385-605">Establezca el URI del objeto que quiera eliminar, por ejemplo, `https://localhost:5001/api/todo/1`.</span><span class="sxs-lookup"><span data-stu-id="c4385-605">Set the URI of the object to delete, for example `https://localhost:5001/api/todo/1`</span></span>
* <span data-ttu-id="c4385-606">Seleccione **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="c4385-606">Select **Send**</span></span>

<span data-ttu-id="c4385-607">La aplicación de ejemplo permite eliminar todos los elementos.</span><span class="sxs-lookup"><span data-stu-id="c4385-607">The sample app allows you to delete all the items.</span></span> <span data-ttu-id="c4385-608">Sin embargo, al eliminar el último elemento, se creará uno nuevo en el constructor de clase de modelo la próxima vez que se llame a la API.</span><span class="sxs-lookup"><span data-stu-id="c4385-608">However, when the last item is deleted, a new one is created by the model class constructor the next time the API is called.</span></span>

## <a name="call-the-api-with-jquery"></a><span data-ttu-id="c4385-609">Llamada a la API con jQuery</span><span class="sxs-lookup"><span data-stu-id="c4385-609">Call the API with jQuery</span></span>

<span data-ttu-id="c4385-610">En esta sección, se agrega una página HTML que usa jQuery para llamar a la API web.</span><span class="sxs-lookup"><span data-stu-id="c4385-610">In this section, an HTML page is added that uses jQuery to call the web api.</span></span> <span data-ttu-id="c4385-611">jQuery inicia la solicitud y actualiza la página con los detalles de la respuesta de la API.</span><span class="sxs-lookup"><span data-stu-id="c4385-611">jQuery initiates the request and updates the page with the details from the API's response.</span></span>

<span data-ttu-id="c4385-612">Configure la aplicación para [atender archivos estáticos](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) y [habilitar la asignación de archivos predeterminada](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) mediante la actualización de *Startup.cs* con el siguiente código resaltado:</span><span class="sxs-lookup"><span data-stu-id="c4385-612">Configure the app to [serve static files](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [enable default file mapping](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) by updating *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

<span data-ttu-id="c4385-613">Cree una carpeta *wwwroot* en el directorio del proyecto.</span><span class="sxs-lookup"><span data-stu-id="c4385-613">Create a *wwwroot* folder in the project directory.</span></span>

<span data-ttu-id="c4385-614">Agregue un archivo HTML denominado *index.html* al directorio *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="c4385-614">Add an HTML file named *index.html* to the *wwwroot* directory.</span></span> <span data-ttu-id="c4385-615">Reemplace el contenido por el siguiente marcado:</span><span class="sxs-lookup"><span data-stu-id="c4385-615">Replace its contents with the following markup:</span></span>

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

<span data-ttu-id="c4385-616">Agregue un archivo JavaScript denominado *site.js* al directorio *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="c4385-616">Add a JavaScript file named *site.js* to the *wwwroot* directory.</span></span> <span data-ttu-id="c4385-617">Reemplace el contenido por el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="c4385-617">Replace its contents with the following code:</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

<span data-ttu-id="c4385-618">Puede que sea necesario realizar un cambio en la configuración de inicio del proyecto de ASP.NET Core para probar la página HTML localmente:</span><span class="sxs-lookup"><span data-stu-id="c4385-618">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

* <span data-ttu-id="c4385-619">Abra *Properties\launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="c4385-619">Open *Properties\launchSettings.json*.</span></span>
* <span data-ttu-id="c4385-620">Quite la propiedad `launchUrl` para forzar a la aplicación a abrirse en *index.html*, esto es, el archivo predeterminado del proyecto.</span><span class="sxs-lookup"><span data-stu-id="c4385-620">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="c4385-621">Existen varias formas de obtener jQuery.</span><span class="sxs-lookup"><span data-stu-id="c4385-621">There are several ways to get jQuery.</span></span> <span data-ttu-id="c4385-622">En el fragmento de código anterior, la biblioteca se carga desde una red CDN.</span><span class="sxs-lookup"><span data-stu-id="c4385-622">In the preceding snippet, the library is loaded from a CDN.</span></span>

<span data-ttu-id="c4385-623">En este ejemplo se llama a todos los métodos CRUD de la API.</span><span class="sxs-lookup"><span data-stu-id="c4385-623">This sample calls all of the CRUD methods of the API.</span></span> <span data-ttu-id="c4385-624">A continuación, encontrará algunas explicaciones de las llamadas a la API.</span><span class="sxs-lookup"><span data-stu-id="c4385-624">Following are explanations of the calls to the API.</span></span>

### <a name="get-a-list-of-to-do-items"></a><span data-ttu-id="c4385-625">Obtención de una lista de tareas pendientes</span><span class="sxs-lookup"><span data-stu-id="c4385-625">Get a list of to-do items</span></span>

<span data-ttu-id="c4385-626">La función de JQuery [ajax](https://api.jquery.com/jquery.ajax/) envía una solicitud `GET` a la API, que devuelve código JSON que representa una matriz de tareas pendientes.</span><span class="sxs-lookup"><span data-stu-id="c4385-626">The jQuery [ajax](https://api.jquery.com/jquery.ajax/) function sends a `GET` request to the API, which returns JSON representing an array of to-do items.</span></span> <span data-ttu-id="c4385-627">La función de devolución de llamada `success` se invoca si la solicitud se realiza correctamente.</span><span class="sxs-lookup"><span data-stu-id="c4385-627">The `success` callback function is invoked if the request succeeds.</span></span> <span data-ttu-id="c4385-628">En la devolución de llamada, el DOM se actualiza con la información de la tarea pendiente.</span><span class="sxs-lookup"><span data-stu-id="c4385-628">In the callback, the DOM is updated with the to-do information.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a><span data-ttu-id="c4385-629">Incorporación de una tarea pendiente</span><span class="sxs-lookup"><span data-stu-id="c4385-629">Add a to-do item</span></span>

<span data-ttu-id="c4385-630">La función [ajax](https://api.jquery.com/jquery.ajax/) envía una solicitud `POST` con la tarea pendiente en su cuerpo.</span><span class="sxs-lookup"><span data-stu-id="c4385-630">The [ajax](https://api.jquery.com/jquery.ajax/) function sends a `POST` request with the to-do item in the request body.</span></span> <span data-ttu-id="c4385-631">Las opciones `accepts` y `contentType` se establecen en `application/json` para especificar el tipo de medio que se va a recibir y a enviar.</span><span class="sxs-lookup"><span data-stu-id="c4385-631">The `accepts` and `contentType` options are set to `application/json` to specify the media type being received and sent.</span></span> <span data-ttu-id="c4385-632">La tarea pendiente se convierte en JSON mediante [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span><span class="sxs-lookup"><span data-stu-id="c4385-632">The to-do item is converted to JSON by using [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span></span> <span data-ttu-id="c4385-633">Cuando la API devuelve un código de estado correcto, se invoca la función `getData` para actualizar la tabla HTML.</span><span class="sxs-lookup"><span data-stu-id="c4385-633">When the API returns a successful status code, the `getData` function is invoked to update the HTML table.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a><span data-ttu-id="c4385-634">Actualizar una tarea pendiente</span><span class="sxs-lookup"><span data-stu-id="c4385-634">Update a to-do item</span></span>

<span data-ttu-id="c4385-635">El hecho de actualizar una tarea pendiente es similar al de agregar una.</span><span class="sxs-lookup"><span data-stu-id="c4385-635">Updating a to-do item is similar to adding one.</span></span> <span data-ttu-id="c4385-636">El valor `url` cambia para agregar el identificador único del elemento, y `type` es `PUT`.</span><span class="sxs-lookup"><span data-stu-id="c4385-636">The `url` changes to add the unique identifier of the item, and the `type` is `PUT`.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a><span data-ttu-id="c4385-637">Eliminar una tarea pendiente</span><span class="sxs-lookup"><span data-stu-id="c4385-637">Delete a to-do item</span></span>

<span data-ttu-id="c4385-638">Para eliminar una tarea pendiente, hay que establecer el valor `type` de la llamada de AJAX en `DELETE` y especificar el identificador único de la tarea en la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="c4385-638">Deleting a to-do item is accomplished by setting the `type` on the AJAX call to `DELETE` and specifying the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="c4385-639">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="c4385-639">Additional resources</span></span>

<span data-ttu-id="c4385-640">[Vea o descargue el código de ejemplo para este tutorial](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span><span class="sxs-lookup"><span data-stu-id="c4385-640">[View or download sample code for this tutorial](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span></span> <span data-ttu-id="c4385-641">Vea [cómo descargarlo](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="c4385-641">See [how to download](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="c4385-642">Para obtener más información, vea los siguientes recursos:</span><span class="sxs-lookup"><span data-stu-id="c4385-642">For more information, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/intro>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>
* [<span data-ttu-id="c4385-643">Versión en YouTube de este tutorial</span><span class="sxs-lookup"><span data-stu-id="c4385-643">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=TTkhEyGBfAk)
