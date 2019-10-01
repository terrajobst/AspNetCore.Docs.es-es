---
title: 'Tutorial: Creación de una API web con ASP.NET Core'
author: rick-anderson
description: Aprenda a crear de una API web con ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 08/27/2019
uid: tutorials/first-web-api
ms.openlocfilehash: 5e5215f246c6c7a805a4c99f485d51a2fb3c712d
ms.sourcegitcommit: cf9ffcce4fe0b69fe795aae9ae06e99fdb18bdfc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2019
ms.locfileid: "71306666"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core"></a><span data-ttu-id="013e7-103">Tutorial: Creación de una API web con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="013e7-103">Tutorial: Create a web API with ASP.NET Core</span></span>

<span data-ttu-id="013e7-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="013e7-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="013e7-105">En este tutorial se enseñan los conceptos básicos de la compilación de una API web con ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="013e7-105">This tutorial teaches the basics of building a web API with ASP.NET Core.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="013e7-106">En este tutorial aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="013e7-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="013e7-107">Crear un proyecto de API web.</span><span class="sxs-lookup"><span data-stu-id="013e7-107">Create a web API project.</span></span>
> * <span data-ttu-id="013e7-108">Agregar una clase de modelo y un contexto de base de datos.</span><span class="sxs-lookup"><span data-stu-id="013e7-108">Add a model class and a database context.</span></span>
> * <span data-ttu-id="013e7-109">Aplicar scaffolding a un controlador con métodos CRUD.</span><span class="sxs-lookup"><span data-stu-id="013e7-109">Scaffold a controller with CRUD methods.</span></span>
> * <span data-ttu-id="013e7-110">Configurar el enrutamiento, las rutas de acceso URL y los valores devueltos.</span><span class="sxs-lookup"><span data-stu-id="013e7-110">Configure routing, URL paths, and return values.</span></span>
> * <span data-ttu-id="013e7-111">Llamar a la API web con Postman.</span><span class="sxs-lookup"><span data-stu-id="013e7-111">Call the web API with Postman.</span></span>

<span data-ttu-id="013e7-112">Al final, tendrá una API web que pueda administrar los elementos "to-do" almacenados en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="013e7-112">At the end, you have a web API that can manage "to-do" items stored in a database.</span></span>

## <a name="overview"></a><span data-ttu-id="013e7-113">Información general</span><span class="sxs-lookup"><span data-stu-id="013e7-113">Overview</span></span>

<span data-ttu-id="013e7-114">En este tutorial se crea la siguiente API:</span><span class="sxs-lookup"><span data-stu-id="013e7-114">This tutorial creates the following API:</span></span>

|<span data-ttu-id="013e7-115">API</span><span class="sxs-lookup"><span data-stu-id="013e7-115">API</span></span> | <span data-ttu-id="013e7-116">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="013e7-116">Description</span></span> | <span data-ttu-id="013e7-117">Cuerpo de la solicitud</span><span class="sxs-lookup"><span data-stu-id="013e7-117">Request body</span></span> | <span data-ttu-id="013e7-118">Cuerpo de la respuesta</span><span class="sxs-lookup"><span data-stu-id="013e7-118">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="013e7-119">GET /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="013e7-119">GET /api/TodoItems</span></span> | <span data-ttu-id="013e7-120">Obtener todas las tareas pendientes</span><span class="sxs-lookup"><span data-stu-id="013e7-120">Get all to-do items</span></span> | <span data-ttu-id="013e7-121">Ninguna</span><span class="sxs-lookup"><span data-stu-id="013e7-121">None</span></span> | <span data-ttu-id="013e7-122">Matriz de tareas pendientes</span><span class="sxs-lookup"><span data-stu-id="013e7-122">Array of to-do items</span></span>|
|<span data-ttu-id="013e7-123">GET /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="013e7-123">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="013e7-124">Obtener un elemento por identificador</span><span class="sxs-lookup"><span data-stu-id="013e7-124">Get an item by ID</span></span> | <span data-ttu-id="013e7-125">None</span><span class="sxs-lookup"><span data-stu-id="013e7-125">None</span></span> | <span data-ttu-id="013e7-126">Tarea pendiente</span><span class="sxs-lookup"><span data-stu-id="013e7-126">To-do item</span></span>|
|<span data-ttu-id="013e7-127">POST /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="013e7-127">POST /api/TodoItems</span></span> | <span data-ttu-id="013e7-128">Incorporación de un nuevo elemento</span><span class="sxs-lookup"><span data-stu-id="013e7-128">Add a new item</span></span> | <span data-ttu-id="013e7-129">Tarea pendiente</span><span class="sxs-lookup"><span data-stu-id="013e7-129">To-do item</span></span> | <span data-ttu-id="013e7-130">Tarea pendiente</span><span class="sxs-lookup"><span data-stu-id="013e7-130">To-do item</span></span> |
|<span data-ttu-id="013e7-131">PUT /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="013e7-131">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="013e7-132">Actualizar un elemento existente &nbsp;</span><span class="sxs-lookup"><span data-stu-id="013e7-132">Update an existing item &nbsp;</span></span> | <span data-ttu-id="013e7-133">Tarea pendiente</span><span class="sxs-lookup"><span data-stu-id="013e7-133">To-do item</span></span> | <span data-ttu-id="013e7-134">None</span><span class="sxs-lookup"><span data-stu-id="013e7-134">None</span></span> |
|<span data-ttu-id="013e7-135">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="013e7-135">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="013e7-136">Eliminar un elemento &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="013e7-136">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="013e7-137">None</span><span class="sxs-lookup"><span data-stu-id="013e7-137">None</span></span> | <span data-ttu-id="013e7-138">None</span><span class="sxs-lookup"><span data-stu-id="013e7-138">None</span></span>|

<span data-ttu-id="013e7-139">En el diagrama siguiente, se muestra el diseño de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="013e7-139">The following diagram shows the design of the app.</span></span>

![El cliente está representado por un cuadro a la izquierda.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="013e7-145">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="013e7-145">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="013e7-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="013e7-146">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="013e7-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="013e7-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="013e7-148">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="013e7-148">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="013e7-149">Creación de un proyecto web</span><span class="sxs-lookup"><span data-stu-id="013e7-149">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="013e7-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="013e7-150">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="013e7-151">En el menú **Archivo**, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="013e7-151">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="013e7-152">Seleccione la plantilla **Aplicación web ASP.NET Core** y haga clic en **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="013e7-152">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="013e7-153">Asigne al proyecto el nombre *TodoApi* y haga clic en **Crear**.</span><span class="sxs-lookup"><span data-stu-id="013e7-153">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="013e7-154">En el cuadro de diálogo **Crear una aplicación web ASP.NET Core**, confirme que las opciones **.NET Core** y **ASP.NET Core 3.0** estén seleccionadas.</span><span class="sxs-lookup"><span data-stu-id="013e7-154">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span> <span data-ttu-id="013e7-155">Seleccione la plantilla **API** y haga clic en **Crear**.</span><span class="sxs-lookup"><span data-stu-id="013e7-155">Select the **API** template and click **Create**.</span></span>

![Cuadro de diálogo de nuevo proyecto de VS](first-web-api/_static/vs3.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="013e7-157">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="013e7-157">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="013e7-158">Abra el [terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="013e7-158">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="013e7-159">Cambie los directorios (`cd`) a la carpeta que va a contener la carpeta del proyecto.</span><span class="sxs-lookup"><span data-stu-id="013e7-159">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="013e7-160">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="013e7-160">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   cd TodoAPI
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   code -r ../TodoApi
   ```

* <span data-ttu-id="013e7-161">Cuando en un cuadro de diálogo se le pregunte si quiere agregar al proyecto los recursos necesarios, seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="013e7-161">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

  <span data-ttu-id="013e7-162">Los comandos anteriores:</span><span class="sxs-lookup"><span data-stu-id="013e7-162">The preceding commands:</span></span>

  * <span data-ttu-id="013e7-163">Crean un nuevo proyecto de API web y lo abren en Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="013e7-163">Creates a new web API project and opens it in Visual Studio Code.</span></span>
  * <span data-ttu-id="013e7-164">Agregan los paquetes de NuGet que se requieren en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="013e7-164">Adds the NuGet packages which are required in the next section.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="013e7-165">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="013e7-165">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="013e7-166">Seleccione **Archivo** > **Nueva solución**.</span><span class="sxs-lookup"><span data-stu-id="013e7-166">Select **File** > **New Solution**.</span></span>

  ![macOS: Nueva solución](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="013e7-168">Seleccione **.NET Core** > **Aplicación** > **API** > **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="013e7-168">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![Cuadro de diálogo de nuevo proyecto de macOS](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="013e7-170">En el cuadro de diálogo **Configurar la nueva API web de ASP.NET Core**, seleccione el **Marco de trabajo de destino** de \* *.NET Core 3.0*.</span><span class="sxs-lookup"><span data-stu-id="013e7-170">In the **Configure your new ASP.NET Core Web API** dialog, select **Target Framework** of \**.NET Core 3.0*.</span></span>

* <span data-ttu-id="013e7-171">Escriba *TodoApi* en **Nombre del proyecto** y seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="013e7-171">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![cuadro de diálogo de configuración](first-web-api-mac/_static/2.png)

[!INCLUDE[](~/includes/mac-terminal-access.md)]

<span data-ttu-id="013e7-173">Abra un terminal de comandos en la carpeta del proyecto y ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="013e7-173">Open a command terminal in the project folder and run the following commands:</span></span>

   ```dotnetcli
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   ```

---

### <a name="test-the-api"></a><span data-ttu-id="013e7-174">Prueba de la API</span><span class="sxs-lookup"><span data-stu-id="013e7-174">Test the API</span></span>

<span data-ttu-id="013e7-175">La plantilla del proyecto crea una API `WeatherForecast`.</span><span class="sxs-lookup"><span data-stu-id="013e7-175">The project template creates a `WeatherForecast` API.</span></span> <span data-ttu-id="013e7-176">Llame al método `Get` desde un explorador para probar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="013e7-176">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="013e7-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="013e7-177">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="013e7-178">Presione Ctrl+F5 para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="013e7-178">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="013e7-179">Visual Studio inicia un explorador y navega hasta `https://localhost:<port>/WeatherForecast`, donde `<port>` es un número de puerto elegido aleatoriamente.</span><span class="sxs-lookup"><span data-stu-id="013e7-179">Visual Studio launches a browser and navigates to `https://localhost:<port>/WeatherForecast`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="013e7-180">Si aparece un cuadro de diálogo en que se le pregunta si debe confiar en el certificado de IIS Express, seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="013e7-180">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="013e7-181">En el cuadro de diálogo **Advertencia de seguridad** que aparece a continuación, seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="013e7-181">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="013e7-182">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="013e7-182">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="013e7-183">Presione Ctrl+F5 para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="013e7-183">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="013e7-184">En un explorador, vaya a la siguiente dirección URL: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span><span class="sxs-lookup"><span data-stu-id="013e7-184">In a browser, go to following URL: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="013e7-185">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="013e7-185">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="013e7-186">Seleccione **Ejecutar** > **Iniciar depuración** para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="013e7-186">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="013e7-187">Visual Studio para Mac inicia un explorador y navega hasta `https://localhost:<port>`, donde `<port>` es un número de puerto elegido aleatoriamente.</span><span class="sxs-lookup"><span data-stu-id="013e7-187">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="013e7-188">Se devuelve un error HTTP 404 (No encontrado).</span><span class="sxs-lookup"><span data-stu-id="013e7-188">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="013e7-189">Anexe `/WeatherForecast` a la dirección URL (cambie la dirección URL a `https://localhost:<port>/WeatherForecast`).</span><span class="sxs-lookup"><span data-stu-id="013e7-189">Append `/WeatherForecast` to the URL (change the URL to `https://localhost:<port>/WeatherForecast`).</span></span>

---

<span data-ttu-id="013e7-190">Se devuelve un JSON similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="013e7-190">JSON similar to the following is returned:</span></span>

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

## <a name="add-a-model-class"></a><span data-ttu-id="013e7-191">Incorporación de una clase de modelo</span><span class="sxs-lookup"><span data-stu-id="013e7-191">Add a model class</span></span>

<span data-ttu-id="013e7-192">Un *modelo* es un conjunto de clases que representan los datos que la aplicación administra.</span><span class="sxs-lookup"><span data-stu-id="013e7-192">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="013e7-193">El modelo para esta aplicación es una clase `TodoItem` única.</span><span class="sxs-lookup"><span data-stu-id="013e7-193">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="013e7-194">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="013e7-194">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="013e7-195">En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="013e7-195">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="013e7-196">Seleccione **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="013e7-196">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="013e7-197">Asigne a la carpeta el nombre *Models*.</span><span class="sxs-lookup"><span data-stu-id="013e7-197">Name the folder *Models*.</span></span>

* <span data-ttu-id="013e7-198">Haga clic con el botón derecho en la carpeta *Models* y seleccione **Agregar** > **Clase**.</span><span class="sxs-lookup"><span data-stu-id="013e7-198">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="013e7-199">Asigne a la clase el nombre *TodoItem* y seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="013e7-199">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="013e7-200">Reemplace el código de plantilla por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="013e7-200">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="013e7-201">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="013e7-201">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="013e7-202">Agregue una carpeta denominada *Models*.</span><span class="sxs-lookup"><span data-stu-id="013e7-202">Add a folder named *Models*.</span></span>

* <span data-ttu-id="013e7-203">Agregue una clase `TodoItem` a la carpeta *Models* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="013e7-203">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="013e7-204">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="013e7-204">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="013e7-205">Haga clic con el botón derecho en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="013e7-205">Right-click the project.</span></span> <span data-ttu-id="013e7-206">Seleccione **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="013e7-206">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="013e7-207">Asigne a la carpeta el nombre *Models*.</span><span class="sxs-lookup"><span data-stu-id="013e7-207">Name the folder *Models*.</span></span>

  ![nueva carpeta](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="013e7-209">Haga clic con el botón derecho en la carpeta *Modelos* y seleccione **Agregar** > **Nuevo archivo** > **General** > **Clase vacía**.</span><span class="sxs-lookup"><span data-stu-id="013e7-209">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="013e7-210">Asigne a la clase el nombre *TodoItem* y haga clic en **Nuevo**.</span><span class="sxs-lookup"><span data-stu-id="013e7-210">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="013e7-211">Reemplace el código de plantilla por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="013e7-211">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="013e7-212">La propiedad `Id` funciona como clave única en una base de datos relacional.</span><span class="sxs-lookup"><span data-stu-id="013e7-212">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="013e7-213">Las clases de modelo pueden ir en cualquier lugar del proyecto, pero convencionalmente e usa la carpeta *Models*.</span><span class="sxs-lookup"><span data-stu-id="013e7-213">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="013e7-214">Incorporación de un contexto de base de datos</span><span class="sxs-lookup"><span data-stu-id="013e7-214">Add a database context</span></span>

<span data-ttu-id="013e7-215">El *contexto de base de datos* es la clase principal que coordina la funcionalidad de Entity Framework para un modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="013e7-215">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="013e7-216">Esta clase se crea derivándola de la clase `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="013e7-216">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="013e7-217">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="013e7-217">Visual Studio</span></span>](#tab/visual-studio)

### <a name="add-microsoftentityframeworkcoresqlserver"></a><span data-ttu-id="013e7-218">Adición de Microsoft.EntityFrameworkCore.SqlServer</span><span class="sxs-lookup"><span data-stu-id="013e7-218">Add Microsoft.EntityFrameworkCore.SqlServer</span></span>

* <span data-ttu-id="013e7-219">En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet > Administrar paquetes NuGet para la solución**.</span><span class="sxs-lookup"><span data-stu-id="013e7-219">From the **Tools** menu, select **NuGet Package Manager > Manage NuGet Packages for Solution**.</span></span>
* <span data-ttu-id="013e7-220">Seleccione la pestaña **Examinar** y escriba **Microsoft.EntityFrameworkCore.SqlServer** en el cuadro de búsqueda.</span><span class="sxs-lookup"><span data-stu-id="013e7-220">Select the **Browse** tab, and then enter **Microsoft.EntityFrameworkCore.SqlServer** in the search box.</span></span>
* <span data-ttu-id="013e7-221">Seleccione **Microsoft.EntityFrameworkCore.SqlServer** en el panel izquierdo.</span><span class="sxs-lookup"><span data-stu-id="013e7-221">Select  **Microsoft.EntityFrameworkCore.SqlServer** in the left pane.</span></span>
* <span data-ttu-id="013e7-222">Active la casilla **Proyecto** en el panel derecho y, después, seleccione **Instalar**.</span><span class="sxs-lookup"><span data-stu-id="013e7-222">Select the **Project** check box in the right pane and then select **Install**.</span></span>
* <span data-ttu-id="013e7-223">Siga las instrucciones anteriores para agregar el paquete NuGet `Microsoft.EntityFrameworkCore.InMemory`.</span><span class="sxs-lookup"><span data-stu-id="013e7-223">Use the preceding instructions to add the `Microsoft.EntityFrameworkCore.InMemory` NuGet package.</span></span>

![Administrador de paquetes de NuGet](first-web-api/_static/vs3NuGet.png)

## <a name="add-the-todocontext-database-context"></a><span data-ttu-id="013e7-225">Adición del contexto de la base de datos TodoContext</span><span class="sxs-lookup"><span data-stu-id="013e7-225">Add the TodoContext database context</span></span>

* <span data-ttu-id="013e7-226">Haga clic con el botón derecho en la carpeta *Models* y seleccione **Agregar** > **Clase**.</span><span class="sxs-lookup"><span data-stu-id="013e7-226">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="013e7-227">Asigne a la clase el nombre *TodoContext* y haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="013e7-227">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="013e7-228">Visual Studio Code/Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="013e7-228">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="013e7-229">Agregue una clase `TodoContext` a la carpeta *Models*.</span><span class="sxs-lookup"><span data-stu-id="013e7-229">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="013e7-230">Escriba el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="013e7-230">Enter the following code:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="013e7-231">Registro del contexto de base de datos</span><span class="sxs-lookup"><span data-stu-id="013e7-231">Register the database context</span></span>

<span data-ttu-id="013e7-232">En ASP.NET Core, los servicios (como el contexto de la base de datos) deben registrarse con el contenedor de [inserción de dependencias (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="013e7-232">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="013e7-233">El contenedor proporciona el servicio a los controladores.</span><span class="sxs-lookup"><span data-stu-id="013e7-233">The container provides the service to controllers.</span></span>

<span data-ttu-id="013e7-234">Actualice *Startup.cs* con el siguiente código resaltado:</span><span class="sxs-lookup"><span data-stu-id="013e7-234">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Startup.cs?highlight=7-8,23-24&name=snippet_all)]

<span data-ttu-id="013e7-235">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="013e7-235">The preceding code:</span></span>

* <span data-ttu-id="013e7-236">Elimina las declaraciones `using` no utilizadas.</span><span class="sxs-lookup"><span data-stu-id="013e7-236">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="013e7-237">Agrega el contexto de base de datos para el contenedor de DI.</span><span class="sxs-lookup"><span data-stu-id="013e7-237">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="013e7-238">Especifica que el contexto de base de datos usará una base de datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="013e7-238">Specifies that the database context will use an in-memory database.</span></span>

## <a name="scaffold-a-controller"></a><span data-ttu-id="013e7-239">Scaffolding de un controlador</span><span class="sxs-lookup"><span data-stu-id="013e7-239">Scaffold a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="013e7-240">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="013e7-240">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="013e7-241">Haga clic con el botón derecho en la carpeta *Controllers*.</span><span class="sxs-lookup"><span data-stu-id="013e7-241">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="013e7-242">Seleccione **Agregar** > **Nuevo elemento con scaffold**.</span><span class="sxs-lookup"><span data-stu-id="013e7-242">Select **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="013e7-243">Seleccione **Controlador de API con acciones mediante Entity Framework** y, después, seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="013e7-243">Select **API Controller with actions, using Entity Framework**, and then select **Add**.</span></span>
* <span data-ttu-id="013e7-244">En el cuadro de diálogo **Add API Controller with actions, using Entity Framework** (Agregar controlador de API con acciones mediante Entity Framework):</span><span class="sxs-lookup"><span data-stu-id="013e7-244">In the **Add API Controller with actions, using Entity Framework** dialog:</span></span>

  * <span data-ttu-id="013e7-245">Seleccione **TodoItem (TodoAPI.Models)** en **Clase de modelo**.</span><span class="sxs-lookup"><span data-stu-id="013e7-245">Select **TodoItem (TodoAPI.Models)** in the **Model class**.</span></span>
  * <span data-ttu-id="013e7-246">Seleccione **TodoContext (TodoAPI.Models)** en **Clase de contexto de datos**.</span><span class="sxs-lookup"><span data-stu-id="013e7-246">Select **TodoContext (TodoAPI.Models)** in the **Data context class**.</span></span>
  * <span data-ttu-id="013e7-247">Seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="013e7-247">Select **Add**</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="013e7-248">Visual Studio Code/Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="013e7-248">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="013e7-249">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="013e7-249">Run the following commands:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator controller -name TodoItemsController -async -api -m TodoItem -dc TodoContext  -outDir Controllers
```

<span data-ttu-id="013e7-250">Los comandos anteriores:</span><span class="sxs-lookup"><span data-stu-id="013e7-250">The preceding commands:</span></span>

* <span data-ttu-id="013e7-251">Agregan los paquetes NuGet necesarios para realizar scaffolding.</span><span class="sxs-lookup"><span data-stu-id="013e7-251">Add NuGet packages required for scaffolding.</span></span>
* <span data-ttu-id="013e7-252">Instalan el motor de scaffolding (`dotnet-aspnet-codegenerator`).</span><span class="sxs-lookup"><span data-stu-id="013e7-252">Installs the scaffolding engine (`dotnet-aspnet-codegenerator`).</span></span>
* <span data-ttu-id="013e7-253">Aplican scaffolding a `TodoItemsController`.</span><span class="sxs-lookup"><span data-stu-id="013e7-253">Scaffolds the `TodoItemsController`.</span></span>

---

<span data-ttu-id="013e7-254">El código generado:</span><span class="sxs-lookup"><span data-stu-id="013e7-254">The generated code:</span></span>

* <span data-ttu-id="013e7-255">Define una clase de controlador de API sin métodos.</span><span class="sxs-lookup"><span data-stu-id="013e7-255">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="013e7-256">Representa la clase con el atributo [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute).</span><span class="sxs-lookup"><span data-stu-id="013e7-256">Decorates the class with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="013e7-257">Este atributo indica que el controlador responde a las solicitudes de la API web.</span><span class="sxs-lookup"><span data-stu-id="013e7-257">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="013e7-258">Para información sobre comportamientos específicos que permite el atributo, consulte <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="013e7-258">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="013e7-259">Utiliza la inserción de dependencias para insertar el contexto de base de datos (`TodoContext`) en el controlador.</span><span class="sxs-lookup"><span data-stu-id="013e7-259">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="013e7-260">El contexto de base de datos se usa en cada uno de los métodos [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) del controlador.</span><span class="sxs-lookup"><span data-stu-id="013e7-260">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

## <a name="examine-the-posttodoitem-create-method"></a><span data-ttu-id="013e7-261">Examen del método create de PostTodoItem</span><span class="sxs-lookup"><span data-stu-id="013e7-261">Examine the PostTodoItem create method</span></span>

<span data-ttu-id="013e7-262">Reemplace la instrucción return en `PostTodoItem` para usar el operador [nameof](/dotnet/csharp/language-reference/operators/nameof):</span><span class="sxs-lookup"><span data-stu-id="013e7-262">Replace the return statement in the `PostTodoItem` to use the [nameof](/dotnet/csharp/language-reference/operators/nameof) operator:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Create)]

<span data-ttu-id="013e7-263">El código anterior es un método HTTP POST, según indica el atributo [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="013e7-263">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="013e7-264">El método obtiene el valor de tareas pendientes del cuerpo de la solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="013e7-264">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="013e7-265">El método <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> realiza las acciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="013e7-265">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> method:</span></span>

* <span data-ttu-id="013e7-266">Devuelve un código de estado HTTP 201 cuando se ha ejecutado correctamente.</span><span class="sxs-lookup"><span data-stu-id="013e7-266">Returns an HTTP 201 status code if successful.</span></span> <span data-ttu-id="013e7-267">HTTP 201 es la respuesta estándar para un método HTTP POST que crea un recurso en el servidor.</span><span class="sxs-lookup"><span data-stu-id="013e7-267">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="013e7-268">Agrega un encabezado [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) a la respuesta.</span><span class="sxs-lookup"><span data-stu-id="013e7-268">Adds a [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) header to the response.</span></span> <span data-ttu-id="013e7-269">El encabezado `Location` especifica el [URI](https://developer.mozilla.org/docs/Glossary/URI) de la tarea pendiente recién creada.</span><span class="sxs-lookup"><span data-stu-id="013e7-269">The `Location` header specifies the [URI](https://developer.mozilla.org/docs/Glossary/URI) of the newly created to-do item.</span></span> <span data-ttu-id="013e7-270">Para obtener más información, consulte [10.2.2 201 creado](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="013e7-270">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="013e7-271">Hace referencia a la acción `GetTodoItem` para crear el identificador URI del encabezado `Location`.</span><span class="sxs-lookup"><span data-stu-id="013e7-271">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="013e7-272">La palabra clave `nameof` de C# se usa para evitar que se codifique de forma rígida el nombre de acción en la llamada a `CreatedAtAction`.</span><span class="sxs-lookup"><span data-stu-id="013e7-272">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

### <a name="install-postman"></a><span data-ttu-id="013e7-273">Instalación de Postman</span><span class="sxs-lookup"><span data-stu-id="013e7-273">Install Postman</span></span>

<span data-ttu-id="013e7-274">En este tutorial se usa Postman para probar la API web.</span><span class="sxs-lookup"><span data-stu-id="013e7-274">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="013e7-275">Instale [Postman](https://www.getpostman.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="013e7-275">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="013e7-276">Inicie la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="013e7-276">Start the web app.</span></span>
* <span data-ttu-id="013e7-277">Inicie Postman.</span><span class="sxs-lookup"><span data-stu-id="013e7-277">Start Postman.</span></span>
* <span data-ttu-id="013e7-278">Deshabilite **Comprobación del certificado SSL**.</span><span class="sxs-lookup"><span data-stu-id="013e7-278">Disable **SSL certificate verification**</span></span>
* <span data-ttu-id="013e7-279">En **Archivo > Configuración** (pestaña \**General*), deshabilite **Comprobación del certificado SSL**.</span><span class="sxs-lookup"><span data-stu-id="013e7-279">From  **File > Settings** (\**General* tab), disable **SSL certificate verification**.</span></span>
    > [!WARNING]
    > <span data-ttu-id="013e7-280">Vuelva a habilitar la comprobación del certificado SSL tras probar el controlador.</span><span class="sxs-lookup"><span data-stu-id="013e7-280">Re-enable SSL certificate verification after testing the controller.</span></span>

<a name="post"></a>

### <a name="test-posttodoitem-with-postman"></a><span data-ttu-id="013e7-281">Prueba de PostTodoItem con Postman</span><span class="sxs-lookup"><span data-stu-id="013e7-281">Test PostTodoItem with Postman</span></span>

* <span data-ttu-id="013e7-282">Cree una nueva solicitud.</span><span class="sxs-lookup"><span data-stu-id="013e7-282">Create a new request.</span></span>
* <span data-ttu-id="013e7-283">Establezca el método HTTP en `POST`.</span><span class="sxs-lookup"><span data-stu-id="013e7-283">Set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="013e7-284">Seleccione la pestaña **Cuerpo**.</span><span class="sxs-lookup"><span data-stu-id="013e7-284">Select the **Body** tab.</span></span>
* <span data-ttu-id="013e7-285">Seleccione el botón de radio **Raw** (Sin formato).</span><span class="sxs-lookup"><span data-stu-id="013e7-285">Select the **raw** radio button.</span></span>
* <span data-ttu-id="013e7-286">Establezca el tipo en **JSON (application/json)** .</span><span class="sxs-lookup"><span data-stu-id="013e7-286">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="013e7-287">En el cuerpo de la solicitud, introduzca JSON para una tarea pendiente:</span><span class="sxs-lookup"><span data-stu-id="013e7-287">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="013e7-288">Seleccione **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="013e7-288">Select **Send**.</span></span>

  ![Postman con solicitud de creación](first-web-api/_static/3/create.png)

### <a name="test-the-location-header-uri"></a><span data-ttu-id="013e7-290">Prueba del URI del encabezado de ubicación</span><span class="sxs-lookup"><span data-stu-id="013e7-290">Test the location header URI</span></span>

* <span data-ttu-id="013e7-291">Seleccione la pestaña **Encabezados** en el panel **Respuesta**.</span><span class="sxs-lookup"><span data-stu-id="013e7-291">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="013e7-292">Copie el valor de encabezado **Ubicación**:</span><span class="sxs-lookup"><span data-stu-id="013e7-292">Copy the **Location** header value:</span></span>

  ![Pestaña Encabezados de la consola de Postman](first-web-api/_static/3/create.png)

* <span data-ttu-id="013e7-294">Establezca el método en GET.</span><span class="sxs-lookup"><span data-stu-id="013e7-294">Set the method to GET.</span></span>
* <span data-ttu-id="013e7-295">Pegue el URI (por ejemplo, `https://localhost:5001/api/TodoItems/1`).</span><span class="sxs-lookup"><span data-stu-id="013e7-295">Paste the URI (for example, `https://localhost:5001/api/TodoItems/1`)</span></span>
* <span data-ttu-id="013e7-296">Seleccione **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="013e7-296">Select **Send**.</span></span>

## <a name="examine-the-get-methods"></a><span data-ttu-id="013e7-297">Examen de los métodos GET</span><span class="sxs-lookup"><span data-stu-id="013e7-297">Examine the GET methods</span></span>

<span data-ttu-id="013e7-298">Estos métodos implementan dos puntos de conexión GET:</span><span class="sxs-lookup"><span data-stu-id="013e7-298">These methods implement two GET endpoints:</span></span>

* `GET /api/TodoItems`
* `GET /api/TodoItems/{id}`

<span data-ttu-id="013e7-299">Llame a los dos puntos de conexión desde un explorador o Postman para probar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="013e7-299">Test the app by calling the two endpoints from a browser or Postman.</span></span> <span data-ttu-id="013e7-300">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="013e7-300">For example:</span></span>

* [https://localhost:5001/api/TodoItems](https://localhost:5001/api/TodoItems)
* [https://localhost:5001/api/TodoItems/1](https://localhost:5001/api/TodoItems/1)

<span data-ttu-id="013e7-301">La llamada a `GetTodoItems` genera una respuesta similar a la siguiente:</span><span class="sxs-lookup"><span data-stu-id="013e7-301">A response similar to the following is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

### <a name="test-get-with-postman"></a><span data-ttu-id="013e7-302">Prueba de Get con Postman</span><span class="sxs-lookup"><span data-stu-id="013e7-302">Test Get with Postman</span></span>

* <span data-ttu-id="013e7-303">Cree una nueva solicitud.</span><span class="sxs-lookup"><span data-stu-id="013e7-303">Create a new request.</span></span>
* <span data-ttu-id="013e7-304">Establezca el método HTTP en **GET**.</span><span class="sxs-lookup"><span data-stu-id="013e7-304">Set the HTTP method to **GET**.</span></span>
* <span data-ttu-id="013e7-305">Establezca la dirección URL de la solicitud en `https://localhost:<port>/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="013e7-305">Set the request URL to `https://localhost:<port>/api/TodoItems`.</span></span> <span data-ttu-id="013e7-306">Por ejemplo: `https://localhost:5001/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="013e7-306">For example, `https://localhost:5001/api/TodoItems`.</span></span>
* <span data-ttu-id="013e7-307">Establezca **Vista de dos paneles** en Postman.</span><span class="sxs-lookup"><span data-stu-id="013e7-307">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="013e7-308">Seleccione **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="013e7-308">Select **Send**.</span></span>

<span data-ttu-id="013e7-309">Esta aplicación utiliza una base de datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="013e7-309">This app uses an in-memory database.</span></span> <span data-ttu-id="013e7-310">Si la aplicación se detiene y se inicia, la solicitud GET precedente no devolverá ningún dato.</span><span class="sxs-lookup"><span data-stu-id="013e7-310">If the app is stopped and started, the preceding GET request will not return any data.</span></span> <span data-ttu-id="013e7-311">Si no se devuelve ningún dato, publique los datos en la aplicación con [POST](#post).</span><span class="sxs-lookup"><span data-stu-id="013e7-311">If no data is returned, [POST](#post) data to the app.</span></span>

## <a name="routing-and-url-paths"></a><span data-ttu-id="013e7-312">Enrutamiento y rutas URL</span><span class="sxs-lookup"><span data-stu-id="013e7-312">Routing and URL paths</span></span>

<span data-ttu-id="013e7-313">El atributo [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) indica un método que responde a una solicitud HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="013e7-313">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="013e7-314">La ruta de dirección URL para cada método se construye como sigue:</span><span class="sxs-lookup"><span data-stu-id="013e7-314">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="013e7-315">Comience por la cadena de plantilla en el atributo `Route` del controlador:</span><span class="sxs-lookup"><span data-stu-id="013e7-315">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=TodoController&highlight=1)]

* <span data-ttu-id="013e7-316">Reemplace `[controller]` por el nombre del controlador, que convencionalmente es el nombre de clase de controlador sin el sufijo "Controller".</span><span class="sxs-lookup"><span data-stu-id="013e7-316">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="013e7-317">En este ejemplo, el nombre de clase de controlador es **TodoItems**Controller; por tanto, el nombre del controlador es "TodoItems".</span><span class="sxs-lookup"><span data-stu-id="013e7-317">For this sample, the controller class name is **TodoItems**Controller, so the controller name is "TodoItems".</span></span> <span data-ttu-id="013e7-318">El [enrutamiento](xref:mvc/controllers/routing) en ASP.NET Core no distingue entre mayúsculas y minúsculas.</span><span class="sxs-lookup"><span data-stu-id="013e7-318">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="013e7-319">Si el atributo `[HttpGet]` tiene una plantilla de ruta (por ejemplo, `[HttpGet("products")]`), anéxela a la ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="013e7-319">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="013e7-320">En este ejemplo no se usa una plantilla.</span><span class="sxs-lookup"><span data-stu-id="013e7-320">This sample doesn't use a template.</span></span> <span data-ttu-id="013e7-321">Para más información, vea [Enrutamiento mediante atributos con atributos Http[Verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="013e7-321">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="013e7-322">En el siguiente método `GetTodoItem`, `"{id}"` es una variable de marcador de posición correspondiente al identificador único de la tarea pendiente.</span><span class="sxs-lookup"><span data-stu-id="013e7-322">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="013e7-323">Al invocar a `GetTodoItem`, el valor `"{id}"` de la dirección URL se proporciona al método en su parámetro `id`.</span><span class="sxs-lookup"><span data-stu-id="013e7-323">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="013e7-324">Valores devueltos</span><span class="sxs-lookup"><span data-stu-id="013e7-324">Return values</span></span>

<span data-ttu-id="013e7-325">El tipo de valor devuelto de los métodos `GetTodoItems` y `GetTodoItem` es [ActionResult\<T > type](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="013e7-325">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="013e7-326">ASP.NET Core serializa automáticamente el objeto a [JSON](https://www.json.org/) y escribe el JSON en el cuerpo del mensaje de respuesta.</span><span class="sxs-lookup"><span data-stu-id="013e7-326">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="013e7-327">El código de respuesta para este tipo de valor devuelto es el 200, suponiendo que no haya ninguna excepción no controlada.</span><span class="sxs-lookup"><span data-stu-id="013e7-327">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="013e7-328">Las excepciones no controladas se convierten en errores 5xx.</span><span class="sxs-lookup"><span data-stu-id="013e7-328">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="013e7-329">Los tipos de valores devueltos `ActionResult` pueden representar una gama amplia de códigos de estado HTTP.</span><span class="sxs-lookup"><span data-stu-id="013e7-329">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="013e7-330">Por ejemplo, `GetTodoItem` puede devolver dos valores de estado diferentes:</span><span class="sxs-lookup"><span data-stu-id="013e7-330">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="013e7-331">Si no hay ningún elemento que coincida con el identificador solicitado, el método devolverá un código de error 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound).</span><span class="sxs-lookup"><span data-stu-id="013e7-331">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="013e7-332">En caso contrario, el método devuelve 200 con un cuerpo de respuesta JSON.</span><span class="sxs-lookup"><span data-stu-id="013e7-332">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="013e7-333">Devolver `item` genera una respuesta HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="013e7-333">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="the-puttodoitem-method"></a><span data-ttu-id="013e7-334">Método PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="013e7-334">The PutTodoItem method</span></span>

<span data-ttu-id="013e7-335">Examine el método `PutTodoItem`:</span><span class="sxs-lookup"><span data-stu-id="013e7-335">Examine the `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Update)]

<span data-ttu-id="013e7-336">`PutTodoItem` es similar a `PostTodoItem`, salvo por el hecho de que usa HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="013e7-336">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="013e7-337">La respuesta es [204 Sin contenido](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="013e7-337">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="013e7-338">Según la especificación HTTP, una solicitud PUT requiere que el cliente envíe toda la entidad actualizada, no solo los cambios.</span><span class="sxs-lookup"><span data-stu-id="013e7-338">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="013e7-339">Para admitir actualizaciones parciales, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="013e7-339">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="013e7-340">Si recibe un error al llamar a `PutTodoItem`, llame a `GET` para asegurarse de que hay un elemento en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="013e7-340">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="013e7-341">Prueba del método PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="013e7-341">Test the PutTodoItem method</span></span>

<span data-ttu-id="013e7-342">En este ejemplo se usa una base de datos en memoria que se debe iniciar cada vez que se inicia la aplicación.</span><span class="sxs-lookup"><span data-stu-id="013e7-342">This sample uses an in-memory database that must be initialed each time the app is started.</span></span> <span data-ttu-id="013e7-343">Debe haber un elemento en la base de datos antes de que realice una llamada PUT.</span><span class="sxs-lookup"><span data-stu-id="013e7-343">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="013e7-344">Llame a GET para asegurarse de que hay un elemento en la base de datos antes de realizar una llamada PUT.</span><span class="sxs-lookup"><span data-stu-id="013e7-344">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="013e7-345">Actualice el elemento to-do que tiene el id. = 1 y establezca su nombre en "feed fish":</span><span class="sxs-lookup"><span data-stu-id="013e7-345">Update the to-do item that has ID = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="013e7-346">En la imagen siguiente, se muestra la actualización de Postman:</span><span class="sxs-lookup"><span data-stu-id="013e7-346">The following image shows the Postman update:</span></span>

![Consola de Postman que muestra la respuesta 204 (Sin contenido)](first-web-api/_static/3/pmcput.png)

## <a name="the-deletetodoitem-method"></a><span data-ttu-id="013e7-348">Método DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="013e7-348">The DeleteTodoItem method</span></span>

<span data-ttu-id="013e7-349">Examine el método `DeleteTodoItem`:</span><span class="sxs-lookup"><span data-stu-id="013e7-349">Examine the `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Delete)]

<span data-ttu-id="013e7-350">La respuesta de `DeleteTodoItem` es [204 (Sin contenido)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="013e7-350">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="013e7-351">Prueba del método DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="013e7-351">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="013e7-352">Use Postman para eliminar una tarea pendiente:</span><span class="sxs-lookup"><span data-stu-id="013e7-352">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="013e7-353">Establezca el método en `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="013e7-353">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="013e7-354">Establezca el URI del objeto que quiera eliminar, por ejemplo, `https://localhost:5001/api/TodoItems/1`.</span><span class="sxs-lookup"><span data-stu-id="013e7-354">Set the URI of the object to delete, for example `https://localhost:5001/api/TodoItems/1`</span></span>
* <span data-ttu-id="013e7-355">Seleccione **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="013e7-355">Select **Send**</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="013e7-356">Llamar a la API web con JavaScript</span><span class="sxs-lookup"><span data-stu-id="013e7-356">Call the web API with JavaScript</span></span>

<span data-ttu-id="013e7-357">Vea [Tutorial: Llamada a una API web de ASP.NET Core con JavaScript](xref:tutorials/web-api-javascript).</span><span class="sxs-lookup"><span data-stu-id="013e7-357">See [Tutorial: Call an ASP.NET Core web API with JavaScript](xref:tutorials/web-api-javascript).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="013e7-358">En este tutorial aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="013e7-358">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="013e7-359">Crear un proyecto de API web.</span><span class="sxs-lookup"><span data-stu-id="013e7-359">Create a web API project.</span></span>
> * <span data-ttu-id="013e7-360">Agregar una clase de modelo y un contexto de base de datos.</span><span class="sxs-lookup"><span data-stu-id="013e7-360">Add a model class and a database context.</span></span>
> * <span data-ttu-id="013e7-361">Agregar un controlador.</span><span class="sxs-lookup"><span data-stu-id="013e7-361">Add a controller.</span></span>
> * <span data-ttu-id="013e7-362">Agregar métodos CRUD.</span><span class="sxs-lookup"><span data-stu-id="013e7-362">Add CRUD methods.</span></span>
> * <span data-ttu-id="013e7-363">Configurar el enrutamiento y las rutas de dirección URL.</span><span class="sxs-lookup"><span data-stu-id="013e7-363">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="013e7-364">Especificar los valores devueltos.</span><span class="sxs-lookup"><span data-stu-id="013e7-364">Specify return values.</span></span>
> * <span data-ttu-id="013e7-365">Llamar a la API web con Postman.</span><span class="sxs-lookup"><span data-stu-id="013e7-365">Call the web API with Postman.</span></span>
> * <span data-ttu-id="013e7-366">Llamar a la API web con JavaScript.</span><span class="sxs-lookup"><span data-stu-id="013e7-366">Call the web API with JavaScript.</span></span>

<span data-ttu-id="013e7-367">Al final, tendrá una API web que pueda administrar las tareas "pendientes" almacenadas en una base de datos relacional.</span><span class="sxs-lookup"><span data-stu-id="013e7-367">At the end, you have a web API that can manage "to-do" items stored in a relational database.</span></span>

## <a name="overview"></a><span data-ttu-id="013e7-368">Información general</span><span class="sxs-lookup"><span data-stu-id="013e7-368">Overview</span></span>

<span data-ttu-id="013e7-369">En este tutorial se crea la siguiente API:</span><span class="sxs-lookup"><span data-stu-id="013e7-369">This tutorial creates the following API:</span></span>

|<span data-ttu-id="013e7-370">API</span><span class="sxs-lookup"><span data-stu-id="013e7-370">API</span></span> | <span data-ttu-id="013e7-371">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="013e7-371">Description</span></span> | <span data-ttu-id="013e7-372">Cuerpo de la solicitud</span><span class="sxs-lookup"><span data-stu-id="013e7-372">Request body</span></span> | <span data-ttu-id="013e7-373">Cuerpo de la respuesta</span><span class="sxs-lookup"><span data-stu-id="013e7-373">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="013e7-374">GET /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="013e7-374">GET /api/TodoItems</span></span> | <span data-ttu-id="013e7-375">Obtener todas las tareas pendientes</span><span class="sxs-lookup"><span data-stu-id="013e7-375">Get all to-do items</span></span> | <span data-ttu-id="013e7-376">Ninguna</span><span class="sxs-lookup"><span data-stu-id="013e7-376">None</span></span> | <span data-ttu-id="013e7-377">Matriz de tareas pendientes</span><span class="sxs-lookup"><span data-stu-id="013e7-377">Array of to-do items</span></span>|
|<span data-ttu-id="013e7-378">GET /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="013e7-378">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="013e7-379">Obtener un elemento por identificador</span><span class="sxs-lookup"><span data-stu-id="013e7-379">Get an item by ID</span></span> | <span data-ttu-id="013e7-380">None</span><span class="sxs-lookup"><span data-stu-id="013e7-380">None</span></span> | <span data-ttu-id="013e7-381">Tarea pendiente</span><span class="sxs-lookup"><span data-stu-id="013e7-381">To-do item</span></span>|
|<span data-ttu-id="013e7-382">POST /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="013e7-382">POST /api/TodoItems</span></span> | <span data-ttu-id="013e7-383">Incorporación de un nuevo elemento</span><span class="sxs-lookup"><span data-stu-id="013e7-383">Add a new item</span></span> | <span data-ttu-id="013e7-384">Tarea pendiente</span><span class="sxs-lookup"><span data-stu-id="013e7-384">To-do item</span></span> | <span data-ttu-id="013e7-385">Tarea pendiente</span><span class="sxs-lookup"><span data-stu-id="013e7-385">To-do item</span></span> |
|<span data-ttu-id="013e7-386">PUT /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="013e7-386">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="013e7-387">Actualizar un elemento existente &nbsp;</span><span class="sxs-lookup"><span data-stu-id="013e7-387">Update an existing item &nbsp;</span></span> | <span data-ttu-id="013e7-388">Tarea pendiente</span><span class="sxs-lookup"><span data-stu-id="013e7-388">To-do item</span></span> | <span data-ttu-id="013e7-389">None</span><span class="sxs-lookup"><span data-stu-id="013e7-389">None</span></span> |
|<span data-ttu-id="013e7-390">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="013e7-390">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="013e7-391">Eliminar un elemento &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="013e7-391">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="013e7-392">None</span><span class="sxs-lookup"><span data-stu-id="013e7-392">None</span></span> | <span data-ttu-id="013e7-393">None</span><span class="sxs-lookup"><span data-stu-id="013e7-393">None</span></span>|

<span data-ttu-id="013e7-394">En el diagrama siguiente, se muestra el diseño de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="013e7-394">The following diagram shows the design of the app.</span></span>

![El cliente está representado por un cuadro a la izquierda.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="013e7-400">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="013e7-400">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="013e7-401">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="013e7-401">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="013e7-402">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="013e7-402">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="013e7-403">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="013e7-403">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="013e7-404">Creación de un proyecto web</span><span class="sxs-lookup"><span data-stu-id="013e7-404">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="013e7-405">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="013e7-405">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="013e7-406">En el menú **Archivo**, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="013e7-406">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="013e7-407">Seleccione la plantilla **Aplicación web ASP.NET Core** y haga clic en **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="013e7-407">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="013e7-408">Asigne al proyecto el nombre *TodoApi* y haga clic en **Crear**.</span><span class="sxs-lookup"><span data-stu-id="013e7-408">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="013e7-409">En el cuadro de diálogo **Crear una aplicación web ASP.NET Core**, confirme que las opciones **.NET Core** y **ASP.NET Core 2.2** estén seleccionadas.</span><span class="sxs-lookup"><span data-stu-id="013e7-409">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 2.2** are selected.</span></span> <span data-ttu-id="013e7-410">Seleccione la plantilla **API** y haga clic en **Crear**.</span><span class="sxs-lookup"><span data-stu-id="013e7-410">Select the **API** template and click **Create**.</span></span> <span data-ttu-id="013e7-411">**No** seleccione **Habilitar compatibilidad con Docker**.</span><span class="sxs-lookup"><span data-stu-id="013e7-411">**Don't** select **Enable Docker Support**.</span></span>

![Cuadro de diálogo de nuevo proyecto de VS](first-web-api/_static/vs.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="013e7-413">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="013e7-413">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="013e7-414">Abra el [terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="013e7-414">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="013e7-415">Cambie los directorios (`cd`) a la carpeta que va a contener la carpeta del proyecto.</span><span class="sxs-lookup"><span data-stu-id="013e7-415">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="013e7-416">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="013e7-416">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  <span data-ttu-id="013e7-417">Estos comandos crean un nuevo proyecto de API web y abren una nueva instancia de Visual Studio Code en la carpeta del proyecto nuevo.</span><span class="sxs-lookup"><span data-stu-id="013e7-417">These commands create a new web API project and open a new instance of Visual Studio Code in the new project folder.</span></span>

* <span data-ttu-id="013e7-418">Cuando en un cuadro de diálogo se le pregunte si quiere agregar al proyecto los recursos necesarios, seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="013e7-418">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="013e7-419">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="013e7-419">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="013e7-420">Seleccione **Archivo** > **Nueva solución**.</span><span class="sxs-lookup"><span data-stu-id="013e7-420">Select **File** > **New Solution**.</span></span>

  ![macOS: Nueva solución](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="013e7-422">Seleccione **.NET Core** > **Aplicación** > **API** > **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="013e7-422">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![Cuadro de diálogo de nuevo proyecto de macOS](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="013e7-424">En el cuadro de diálogo **Configurar la nueva API web de ASP.NET Core**, acepte la **plataforma de destino** predeterminada de \* *.NET Core 2.2*.</span><span class="sxs-lookup"><span data-stu-id="013e7-424">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="013e7-425">Escriba *TodoApi* en **Nombre del proyecto** y seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="013e7-425">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![cuadro de diálogo de configuración](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a><span data-ttu-id="013e7-427">Prueba de la API</span><span class="sxs-lookup"><span data-stu-id="013e7-427">Test the API</span></span>

<span data-ttu-id="013e7-428">La plantilla del proyecto crea una API `values`.</span><span class="sxs-lookup"><span data-stu-id="013e7-428">The project template creates a `values` API.</span></span> <span data-ttu-id="013e7-429">Llame al método `Get` desde un explorador para probar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="013e7-429">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="013e7-430">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="013e7-430">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="013e7-431">Presione Ctrl+F5 para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="013e7-431">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="013e7-432">Visual Studio inicia un explorador y navega hasta `https://localhost:<port>/api/values`, donde `<port>` es un número de puerto elegido aleatoriamente.</span><span class="sxs-lookup"><span data-stu-id="013e7-432">Visual Studio launches a browser and navigates to `https://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="013e7-433">Si aparece un cuadro de diálogo en que se le pregunta si debe confiar en el certificado de IIS Express, seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="013e7-433">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="013e7-434">En el cuadro de diálogo **Advertencia de seguridad** que aparece a continuación, seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="013e7-434">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="013e7-435">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="013e7-435">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="013e7-436">Presione Ctrl+F5 para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="013e7-436">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="013e7-437">En un explorador, vaya a la siguiente dirección URL: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span><span class="sxs-lookup"><span data-stu-id="013e7-437">In a browser, go to following URL: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="013e7-438">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="013e7-438">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="013e7-439">Seleccione **Ejecutar** > **Iniciar depuración** para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="013e7-439">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="013e7-440">Visual Studio para Mac inicia un explorador y navega hasta `https://localhost:<port>`, donde `<port>` es un número de puerto elegido aleatoriamente.</span><span class="sxs-lookup"><span data-stu-id="013e7-440">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="013e7-441">Se devuelve un error HTTP 404 (No encontrado).</span><span class="sxs-lookup"><span data-stu-id="013e7-441">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="013e7-442">Anexe `/api/values` a la dirección URL (cambie la dirección URL a `https://localhost:<port>/api/values`).</span><span class="sxs-lookup"><span data-stu-id="013e7-442">Append `/api/values` to the URL (change the URL to `https://localhost:<port>/api/values`).</span></span>

---

<span data-ttu-id="013e7-443">Se devuelve el siguiente JSON:</span><span class="sxs-lookup"><span data-stu-id="013e7-443">The following JSON is returned:</span></span>

```json
["value1","value2"]
```

## <a name="add-a-model-class"></a><span data-ttu-id="013e7-444">Incorporación de una clase de modelo</span><span class="sxs-lookup"><span data-stu-id="013e7-444">Add a model class</span></span>

<span data-ttu-id="013e7-445">Un *modelo* es un conjunto de clases que representan los datos que la aplicación administra.</span><span class="sxs-lookup"><span data-stu-id="013e7-445">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="013e7-446">El modelo para esta aplicación es una clase `TodoItem` única.</span><span class="sxs-lookup"><span data-stu-id="013e7-446">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="013e7-447">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="013e7-447">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="013e7-448">En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="013e7-448">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="013e7-449">Seleccione **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="013e7-449">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="013e7-450">Asigne a la carpeta el nombre *Models*.</span><span class="sxs-lookup"><span data-stu-id="013e7-450">Name the folder *Models*.</span></span>

* <span data-ttu-id="013e7-451">Haga clic con el botón derecho en la carpeta *Models* y seleccione **Agregar** > **Clase**.</span><span class="sxs-lookup"><span data-stu-id="013e7-451">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="013e7-452">Asigne a la clase el nombre *TodoItem* y seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="013e7-452">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="013e7-453">Reemplace el código de plantilla por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="013e7-453">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="013e7-454">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="013e7-454">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="013e7-455">Agregue una carpeta denominada *Models*.</span><span class="sxs-lookup"><span data-stu-id="013e7-455">Add a folder named *Models*.</span></span>

* <span data-ttu-id="013e7-456">Agregue una clase `TodoItem` a la carpeta *Models* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="013e7-456">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="013e7-457">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="013e7-457">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="013e7-458">Haga clic con el botón derecho en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="013e7-458">Right-click the project.</span></span> <span data-ttu-id="013e7-459">Seleccione **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="013e7-459">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="013e7-460">Asigne a la carpeta el nombre *Models*.</span><span class="sxs-lookup"><span data-stu-id="013e7-460">Name the folder *Models*.</span></span>

  ![nueva carpeta](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="013e7-462">Haga clic con el botón derecho en la carpeta *Modelos* y seleccione **Agregar** > **Nuevo archivo** > **General** > **Clase vacía**.</span><span class="sxs-lookup"><span data-stu-id="013e7-462">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="013e7-463">Asigne a la clase el nombre *TodoItem* y haga clic en **Nuevo**.</span><span class="sxs-lookup"><span data-stu-id="013e7-463">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="013e7-464">Reemplace el código de plantilla por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="013e7-464">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="013e7-465">La propiedad `Id` funciona como clave única en una base de datos relacional.</span><span class="sxs-lookup"><span data-stu-id="013e7-465">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="013e7-466">Las clases de modelo pueden ir en cualquier lugar del proyecto, pero convencionalmente e usa la carpeta *Models*.</span><span class="sxs-lookup"><span data-stu-id="013e7-466">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="013e7-467">Incorporación de un contexto de base de datos</span><span class="sxs-lookup"><span data-stu-id="013e7-467">Add a database context</span></span>

<span data-ttu-id="013e7-468">El *contexto de base de datos* es la clase principal que coordina la funcionalidad de Entity Framework para un modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="013e7-468">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="013e7-469">Esta clase se crea derivándola de la clase `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="013e7-469">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="013e7-470">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="013e7-470">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="013e7-471">Haga clic con el botón derecho en la carpeta *Models* y seleccione **Agregar** > **Clase**.</span><span class="sxs-lookup"><span data-stu-id="013e7-471">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="013e7-472">Asigne a la clase el nombre *TodoContext* y haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="013e7-472">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="013e7-473">Visual Studio Code/Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="013e7-473">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="013e7-474">Agregue una clase `TodoContext` a la carpeta *Models*.</span><span class="sxs-lookup"><span data-stu-id="013e7-474">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="013e7-475">Reemplace el código de plantilla por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="013e7-475">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="013e7-476">Registro del contexto de base de datos</span><span class="sxs-lookup"><span data-stu-id="013e7-476">Register the database context</span></span>

<span data-ttu-id="013e7-477">En ASP.NET Core, los servicios (como el contexto de la base de datos) deben registrarse con el contenedor de [inserción de dependencias (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="013e7-477">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="013e7-478">El contenedor proporciona el servicio a los controladores.</span><span class="sxs-lookup"><span data-stu-id="013e7-478">The container provides the service to controllers.</span></span>

<span data-ttu-id="013e7-479">Actualice *Startup.cs* con el siguiente código resaltado:</span><span class="sxs-lookup"><span data-stu-id="013e7-479">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup1.cs?highlight=5,8,25-26&name=snippet_all)]

<span data-ttu-id="013e7-480">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="013e7-480">The preceding code:</span></span>

* <span data-ttu-id="013e7-481">Elimina las declaraciones `using` no utilizadas.</span><span class="sxs-lookup"><span data-stu-id="013e7-481">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="013e7-482">Agrega el contexto de base de datos para el contenedor de DI.</span><span class="sxs-lookup"><span data-stu-id="013e7-482">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="013e7-483">Especifica que el contexto de base de datos usará una base de datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="013e7-483">Specifies that the database context will use an in-memory database.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="013e7-484">Incorporación de un controlador</span><span class="sxs-lookup"><span data-stu-id="013e7-484">Add a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="013e7-485">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="013e7-485">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="013e7-486">Haga clic con el botón derecho en la carpeta *Controllers*.</span><span class="sxs-lookup"><span data-stu-id="013e7-486">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="013e7-487">Seleccione **Agregar** > **Nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="013e7-487">Select **Add** > **New Item**.</span></span>
* <span data-ttu-id="013e7-488">En el cuadro de diálogo **Agregar nuevo elemento**, seleccione la plantilla **Clase de controlador de API**.</span><span class="sxs-lookup"><span data-stu-id="013e7-488">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span>
* <span data-ttu-id="013e7-489">Asigne a la clase el nombre *TodoController* y seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="013e7-489">Name the class *TodoController*, and select **Add**.</span></span>

  ![Cuadro de diálogo Agregar nuevo elemento con la palabra "controller" en el cuadro de búsqueda y la opción Clase de controlador de API web seleccionada](first-web-api/_static/new_controller.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="013e7-491">Visual Studio Code/Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="013e7-491">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="013e7-492">En la carpeta *Controllers*, cree una clase denominada `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="013e7-492">In the *Controllers* folder, create a class named `TodoController`.</span></span>

---

* <span data-ttu-id="013e7-493">Reemplace el código de plantilla por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="013e7-493">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="013e7-494">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="013e7-494">The preceding code:</span></span>

* <span data-ttu-id="013e7-495">Define una clase de controlador de API sin métodos.</span><span class="sxs-lookup"><span data-stu-id="013e7-495">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="013e7-496">Representa la clase con el atributo [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute).</span><span class="sxs-lookup"><span data-stu-id="013e7-496">Decorates the class with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="013e7-497">Este atributo indica que el controlador responde a las solicitudes de la API web.</span><span class="sxs-lookup"><span data-stu-id="013e7-497">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="013e7-498">Para información sobre comportamientos específicos que permite el atributo, consulte <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="013e7-498">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="013e7-499">Utiliza la inserción de dependencias para insertar el contexto de base de datos (`TodoContext`) en el controlador.</span><span class="sxs-lookup"><span data-stu-id="013e7-499">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="013e7-500">El contexto de base de datos se usa en cada uno de los métodos [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) del controlador.</span><span class="sxs-lookup"><span data-stu-id="013e7-500">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="013e7-501">Si la base de datos está vacía, le agrega un elemento denominado `Item1`.</span><span class="sxs-lookup"><span data-stu-id="013e7-501">Adds an item named `Item1` to the database if the database is empty.</span></span> <span data-ttu-id="013e7-502">Este código está en el constructor, de manera que se ejecuta cada vez que hay una nueva solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="013e7-502">This code is in the constructor, so it runs every time there's a new HTTP request.</span></span> <span data-ttu-id="013e7-503">Si elimina todos los elementos, el constructor volverá a crear `Item1` la próxima vez que se llame a un método de API.</span><span class="sxs-lookup"><span data-stu-id="013e7-503">If you delete all items, the constructor creates `Item1` again the next time an API method is called.</span></span> <span data-ttu-id="013e7-504">De este modo, es posible que parezca que la eliminación no ha funcionado, cuando en realidad sí lo ha hecho.</span><span class="sxs-lookup"><span data-stu-id="013e7-504">So it may look like the deletion didn't work when it actually did work.</span></span>

## <a name="add-get-methods"></a><span data-ttu-id="013e7-505">Incorporación de métodos Get</span><span class="sxs-lookup"><span data-stu-id="013e7-505">Add Get methods</span></span>

<span data-ttu-id="013e7-506">Para proporcionar una API que recupere tareas pendientes, agregue estos métodos a la clase `TodoController`:</span><span class="sxs-lookup"><span data-stu-id="013e7-506">To provide an API that retrieves to-do items, add the following methods to the `TodoController` class:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="013e7-507">Estos métodos implementan dos puntos de conexión GET:</span><span class="sxs-lookup"><span data-stu-id="013e7-507">These methods implement two GET endpoints:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="013e7-508">Detenga la aplicación si todavía está en ejecución.</span><span class="sxs-lookup"><span data-stu-id="013e7-508">Stop the app if it's still running.</span></span> <span data-ttu-id="013e7-509">Luego, ejecútela de nuevo para incluir los últimos cambios.</span><span class="sxs-lookup"><span data-stu-id="013e7-509">Then run it again to include the latest changes.</span></span>

<span data-ttu-id="013e7-510">Llame a los dos puntos de conexión desde un explorador para probar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="013e7-510">Test the app by calling the two endpoints from a browser.</span></span> <span data-ttu-id="013e7-511">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="013e7-511">For example:</span></span>

* `https://localhost:<port>/api/todo`
* `https://localhost:<port>/api/todo/1`

<span data-ttu-id="013e7-512">La llamada a `GetTodoItems` genera la siguiente respuesta HTTP:</span><span class="sxs-lookup"><span data-stu-id="013e7-512">The following HTTP response is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

## <a name="routing-and-url-paths"></a><span data-ttu-id="013e7-513">Enrutamiento y rutas URL</span><span class="sxs-lookup"><span data-stu-id="013e7-513">Routing and URL paths</span></span>

<span data-ttu-id="013e7-514">El atributo [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) indica un método que responde a una solicitud HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="013e7-514">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="013e7-515">La ruta de dirección URL para cada método se construye como sigue:</span><span class="sxs-lookup"><span data-stu-id="013e7-515">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="013e7-516">Comience por la cadena de plantilla en el atributo `Route` del controlador:</span><span class="sxs-lookup"><span data-stu-id="013e7-516">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="013e7-517">Reemplace `[controller]` por el nombre del controlador, que convencionalmente es el nombre de clase de controlador sin el sufijo "Controller".</span><span class="sxs-lookup"><span data-stu-id="013e7-517">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="013e7-518">En este ejemplo, el nombre de clase de controlador es **Todo**Controller; por tanto, el nombre del controlador es "todo".</span><span class="sxs-lookup"><span data-stu-id="013e7-518">For this sample, the controller class name is **Todo**Controller, so the controller name is "todo".</span></span> <span data-ttu-id="013e7-519">El [enrutamiento](xref:mvc/controllers/routing) en ASP.NET Core no distingue entre mayúsculas y minúsculas.</span><span class="sxs-lookup"><span data-stu-id="013e7-519">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="013e7-520">Si el atributo `[HttpGet]` tiene una plantilla de ruta (por ejemplo, `[HttpGet("products")]`), anéxela a la ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="013e7-520">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="013e7-521">En este ejemplo no se usa una plantilla.</span><span class="sxs-lookup"><span data-stu-id="013e7-521">This sample doesn't use a template.</span></span> <span data-ttu-id="013e7-522">Para más información, vea [Enrutamiento mediante atributos con atributos Http[Verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="013e7-522">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="013e7-523">En el siguiente método `GetTodoItem`, `"{id}"` es una variable de marcador de posición correspondiente al identificador único de la tarea pendiente.</span><span class="sxs-lookup"><span data-stu-id="013e7-523">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="013e7-524">Al invocar a `GetTodoItem`, el valor `"{id}"` de la dirección URL se proporciona al método en su parámetro `id`.</span><span class="sxs-lookup"><span data-stu-id="013e7-524">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="013e7-525">Valores devueltos</span><span class="sxs-lookup"><span data-stu-id="013e7-525">Return values</span></span>

<span data-ttu-id="013e7-526">El tipo de valor devuelto de los métodos `GetTodoItems` y `GetTodoItem` es [ActionResult\<T > type](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="013e7-526">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="013e7-527">ASP.NET Core serializa automáticamente el objeto a [JSON](https://www.json.org/) y escribe el JSON en el cuerpo del mensaje de respuesta.</span><span class="sxs-lookup"><span data-stu-id="013e7-527">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="013e7-528">El código de respuesta para este tipo de valor devuelto es el 200, suponiendo que no haya ninguna excepción no controlada.</span><span class="sxs-lookup"><span data-stu-id="013e7-528">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="013e7-529">Las excepciones no controladas se convierten en errores 5xx.</span><span class="sxs-lookup"><span data-stu-id="013e7-529">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="013e7-530">Los tipos de valores devueltos `ActionResult` pueden representar una gama amplia de códigos de estado HTTP.</span><span class="sxs-lookup"><span data-stu-id="013e7-530">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="013e7-531">Por ejemplo, `GetTodoItem` puede devolver dos valores de estado diferentes:</span><span class="sxs-lookup"><span data-stu-id="013e7-531">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="013e7-532">Si no hay ningún elemento que coincida con el identificador solicitado, el método devolverá un código de error 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound).</span><span class="sxs-lookup"><span data-stu-id="013e7-532">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="013e7-533">En caso contrario, el método devuelve 200 con un cuerpo de respuesta JSON.</span><span class="sxs-lookup"><span data-stu-id="013e7-533">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="013e7-534">Devolver `item` genera una respuesta HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="013e7-534">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="test-the-gettodoitems-method"></a><span data-ttu-id="013e7-535">Prueba del método GetTodoItems</span><span class="sxs-lookup"><span data-stu-id="013e7-535">Test the GetTodoItems method</span></span>

<span data-ttu-id="013e7-536">En este tutorial se usa Postman para probar la API web.</span><span class="sxs-lookup"><span data-stu-id="013e7-536">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="013e7-537">Instale [Postman](https://www.getpostman.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="013e7-537">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="013e7-538">Inicie la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="013e7-538">Start the web app.</span></span>
* <span data-ttu-id="013e7-539">Inicie Postman.</span><span class="sxs-lookup"><span data-stu-id="013e7-539">Start Postman.</span></span>
* <span data-ttu-id="013e7-540">Deshabilite **Comprobación del certificado SSL**.</span><span class="sxs-lookup"><span data-stu-id="013e7-540">Disable **SSL certificate verification**</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="013e7-541">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="013e7-541">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="013e7-542">En **Archivo** > **Configuración** (pestaña **General**), deshabilite **Comprobación del certificado SSL**.</span><span class="sxs-lookup"><span data-stu-id="013e7-542">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="013e7-543">Visual Studio Code/Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="013e7-543">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="013e7-544">En **Postman** > **Preferencias** (pestaña **General**), deshabilite **Comprobación del certificado SSL**.</span><span class="sxs-lookup"><span data-stu-id="013e7-544">From **Postman** > **Preferences** (**General** tab), disable **SSL certificate verification**.</span></span> <span data-ttu-id="013e7-545">También puede seleccionar la llave inglesa, hacer clic en **Configuración** y deshabilitar la comprobación del certificado SSL.</span><span class="sxs-lookup"><span data-stu-id="013e7-545">Alternatively, select the wrench and select **Settings**, then disable the SSL certificate verification.</span></span>

---
  
> [!WARNING]
> <span data-ttu-id="013e7-546">Vuelva a habilitar la comprobación del certificado SSL tras probar el controlador.</span><span class="sxs-lookup"><span data-stu-id="013e7-546">Re-enable SSL certificate verification after testing the controller.</span></span>

* <span data-ttu-id="013e7-547">Cree una nueva solicitud.</span><span class="sxs-lookup"><span data-stu-id="013e7-547">Create a new request.</span></span>
  * <span data-ttu-id="013e7-548">Establezca el método HTTP en **GET**.</span><span class="sxs-lookup"><span data-stu-id="013e7-548">Set the HTTP method to **GET**.</span></span>
  * <span data-ttu-id="013e7-549">Establezca la dirección URL de la solicitud en `https://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="013e7-549">Set the request URL to `https://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="013e7-550">Por ejemplo: `https://localhost:5001/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="013e7-550">For example, `https://localhost:5001/api/todo`.</span></span>
* <span data-ttu-id="013e7-551">Establezca **Vista de dos paneles** en Postman.</span><span class="sxs-lookup"><span data-stu-id="013e7-551">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="013e7-552">Seleccione **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="013e7-552">Select **Send**.</span></span>

![Postman con solicitud Get](first-web-api/_static/2pv.png)

## <a name="add-a-create-method"></a><span data-ttu-id="013e7-554">Incorporación de un método Create</span><span class="sxs-lookup"><span data-stu-id="013e7-554">Add a Create method</span></span>

<span data-ttu-id="013e7-555">Agregue el siguiente método `PostTodoItem` en *Controllers/TodoController.cs*:</span><span class="sxs-lookup"><span data-stu-id="013e7-555">Add the following `PostTodoItem` method inside of *Controllers/TodoController.cs*:</span></span> 

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="013e7-556">El código anterior es un método HTTP POST, según indica el atributo [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="013e7-556">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="013e7-557">El método obtiene el valor de tareas pendientes del cuerpo de la solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="013e7-557">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="013e7-558">El método `CreatedAtAction` realiza las acciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="013e7-558">The `CreatedAtAction` method:</span></span>

* <span data-ttu-id="013e7-559">Devuelve un código de estado HTTP 201 cuando se ha ejecutado correctamente.</span><span class="sxs-lookup"><span data-stu-id="013e7-559">Returns an HTTP 201 status code, if successful.</span></span> <span data-ttu-id="013e7-560">HTTP 201 es la respuesta estándar para un método HTTP POST que crea un recurso en el servidor.</span><span class="sxs-lookup"><span data-stu-id="013e7-560">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="013e7-561">Agrega un encabezado `Location` a la respuesta.</span><span class="sxs-lookup"><span data-stu-id="013e7-561">Adds a `Location` header to the response.</span></span> <span data-ttu-id="013e7-562">El encabezado `Location` especifica el identificador URI de la tarea pendiente recién creada.</span><span class="sxs-lookup"><span data-stu-id="013e7-562">The `Location` header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="013e7-563">Para obtener más información, consulte [10.2.2 201 creado](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="013e7-563">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="013e7-564">Hace referencia a la acción `GetTodoItem` para crear el identificador URI del encabezado `Location`.</span><span class="sxs-lookup"><span data-stu-id="013e7-564">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="013e7-565">La palabra clave `nameof` de C# se usa para evitar que se codifique de forma rígida el nombre de acción en la llamada a `CreatedAtAction`.</span><span class="sxs-lookup"><span data-stu-id="013e7-565">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method"></a><span data-ttu-id="013e7-566">Prueba del método PostTodoItem</span><span class="sxs-lookup"><span data-stu-id="013e7-566">Test the PostTodoItem method</span></span>

* <span data-ttu-id="013e7-567">Compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="013e7-567">Build the project.</span></span>
* <span data-ttu-id="013e7-568">En Postman, establezca el método HTTP en `POST`.</span><span class="sxs-lookup"><span data-stu-id="013e7-568">In Postman, set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="013e7-569">Seleccione la pestaña **Cuerpo**.</span><span class="sxs-lookup"><span data-stu-id="013e7-569">Select the **Body** tab.</span></span>
* <span data-ttu-id="013e7-570">Seleccione el botón de radio **Raw** (Sin formato).</span><span class="sxs-lookup"><span data-stu-id="013e7-570">Select the **raw** radio button.</span></span>
* <span data-ttu-id="013e7-571">Establezca el tipo en **JSON (application/json)** .</span><span class="sxs-lookup"><span data-stu-id="013e7-571">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="013e7-572">En el cuerpo de la solicitud, introduzca JSON para una tarea pendiente:</span><span class="sxs-lookup"><span data-stu-id="013e7-572">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="013e7-573">Seleccione **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="013e7-573">Select **Send**.</span></span>

  ![Postman con solicitud de creación](first-web-api/_static/create.png)

  <span data-ttu-id="013e7-575">Si recibe un error 405 (Método no permitido), probablemente sea el resultado de no haber compilado el proyecto después de agregar el método `PostTodoItem`.</span><span class="sxs-lookup"><span data-stu-id="013e7-575">If you get a 405 Method Not Allowed error, it's probably the result of not compiling the project after adding the `PostTodoItem` method.</span></span>

### <a name="test-the-location-header-uri"></a><span data-ttu-id="013e7-576">Prueba del URI del encabezado de ubicación</span><span class="sxs-lookup"><span data-stu-id="013e7-576">Test the location header URI</span></span>

* <span data-ttu-id="013e7-577">Seleccione la pestaña **Encabezados** en el panel **Respuesta**.</span><span class="sxs-lookup"><span data-stu-id="013e7-577">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="013e7-578">Copie el valor de encabezado **Ubicación**:</span><span class="sxs-lookup"><span data-stu-id="013e7-578">Copy the **Location** header value:</span></span>

  ![Pestaña Encabezados de la consola de Postman](first-web-api/_static/pmc2.png)

* <span data-ttu-id="013e7-580">Establezca el método en GET.</span><span class="sxs-lookup"><span data-stu-id="013e7-580">Set the method to GET.</span></span>
* <span data-ttu-id="013e7-581">Pegue el URI (por ejemplo, `https://localhost:5001/api/Todo/2`).</span><span class="sxs-lookup"><span data-stu-id="013e7-581">Paste the URI (for example, `https://localhost:5001/api/Todo/2`)</span></span>
* <span data-ttu-id="013e7-582">Seleccione **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="013e7-582">Select **Send**.</span></span>

## <a name="add-a-puttodoitem-method"></a><span data-ttu-id="013e7-583">Incorporación de un método PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="013e7-583">Add a PutTodoItem method</span></span>

<span data-ttu-id="013e7-584">Agregue el siguiente método `PutTodoItem`:</span><span class="sxs-lookup"><span data-stu-id="013e7-584">Add the following `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="013e7-585">`PutTodoItem` es similar a `PostTodoItem`, salvo por el hecho de que usa HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="013e7-585">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="013e7-586">La respuesta es [204 Sin contenido](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="013e7-586">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="013e7-587">Según la especificación HTTP, una solicitud PUT requiere que el cliente envíe toda la entidad actualizada, no solo los cambios.</span><span class="sxs-lookup"><span data-stu-id="013e7-587">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="013e7-588">Para admitir actualizaciones parciales, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="013e7-588">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="013e7-589">Si recibe un error al llamar a `PutTodoItem`, llame a `GET` para asegurarse de que hay un elemento en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="013e7-589">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="013e7-590">Prueba del método PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="013e7-590">Test the PutTodoItem method</span></span>

<span data-ttu-id="013e7-591">En este ejemplo se usa una base de datos en memoria que se debe iniciar cada vez que se inicia la aplicación.</span><span class="sxs-lookup"><span data-stu-id="013e7-591">This sample uses an in-memory database that must be initialed each time the app is started.</span></span> <span data-ttu-id="013e7-592">Debe haber un elemento en la base de datos antes de que realice una llamada PUT.</span><span class="sxs-lookup"><span data-stu-id="013e7-592">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="013e7-593">Llame a GET para asegurarse de que hay un elemento en la base de datos antes de realizar una llamada PUT.</span><span class="sxs-lookup"><span data-stu-id="013e7-593">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="013e7-594">Actualice la tarea pendiente que tiene el id. = 1 y establezca su nombre en "feed fish":</span><span class="sxs-lookup"><span data-stu-id="013e7-594">Update the to-do item that has id = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="013e7-595">En la imagen siguiente, se muestra la actualización de Postman:</span><span class="sxs-lookup"><span data-stu-id="013e7-595">The following image shows the Postman update:</span></span>

![Consola de Postman que muestra la respuesta 204 (Sin contenido)](first-web-api/_static/pmcput.png)

## <a name="add-a-deletetodoitem-method"></a><span data-ttu-id="013e7-597">Incorporación de un método DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="013e7-597">Add a DeleteTodoItem method</span></span>

<span data-ttu-id="013e7-598">Agregue el siguiente método `DeleteTodoItem`:</span><span class="sxs-lookup"><span data-stu-id="013e7-598">Add the following `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="013e7-599">La respuesta de `DeleteTodoItem` es [204 (Sin contenido)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="013e7-599">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="013e7-600">Prueba del método DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="013e7-600">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="013e7-601">Use Postman para eliminar una tarea pendiente:</span><span class="sxs-lookup"><span data-stu-id="013e7-601">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="013e7-602">Establezca el método en `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="013e7-602">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="013e7-603">Establezca el URI del objeto que quiera eliminar, por ejemplo, `https://localhost:5001/api/todo/1`.</span><span class="sxs-lookup"><span data-stu-id="013e7-603">Set the URI of the object to delete, for example `https://localhost:5001/api/todo/1`</span></span>
* <span data-ttu-id="013e7-604">Seleccione **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="013e7-604">Select **Send**</span></span>

<span data-ttu-id="013e7-605">La aplicación de ejemplo permite eliminar todos los elementos.</span><span class="sxs-lookup"><span data-stu-id="013e7-605">The sample app allows you to delete all the items.</span></span> <span data-ttu-id="013e7-606">Sin embargo, al eliminar el último elemento, se creará uno nuevo en el constructor de clase de modelo la próxima vez que se llame a la API.</span><span class="sxs-lookup"><span data-stu-id="013e7-606">However, when the last item is deleted, a new one is created by the model class constructor the next time the API is called.</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="013e7-607">Llamar a la API web con JavaScript</span><span class="sxs-lookup"><span data-stu-id="013e7-607">Call the web API with JavaScript</span></span>

<span data-ttu-id="013e7-608">En esta sección, se agrega una página HTML que usa JavaScript para llamar a la API web.</span><span class="sxs-lookup"><span data-stu-id="013e7-608">In this section, an HTML page is added that uses JavaScript to call the web API.</span></span> <span data-ttu-id="013e7-609">La API Fetch inicia la solicitud.</span><span class="sxs-lookup"><span data-stu-id="013e7-609">The Fetch API initiates the request.</span></span> <span data-ttu-id="013e7-610">JavaScript actualiza la página con los detalles de la respuesta de la API web.</span><span class="sxs-lookup"><span data-stu-id="013e7-610">JavaScript updates the page with the details from the web API's response.</span></span>

<span data-ttu-id="013e7-611">Configure la aplicación para [atender archivos estáticos](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) y [habilitar la asignación de archivos predeterminada](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) mediante la actualización de *Startup.cs* con el siguiente código resaltado:</span><span class="sxs-lookup"><span data-stu-id="013e7-611">Configure the app to [serve static files](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [enable default file mapping](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) by updating *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

<span data-ttu-id="013e7-612">Cree una carpeta *wwwroot* en el directorio del proyecto.</span><span class="sxs-lookup"><span data-stu-id="013e7-612">Create a *wwwroot* folder in the project directory.</span></span>

<span data-ttu-id="013e7-613">Agregue un archivo HTML denominado *index.html* al directorio *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="013e7-613">Add an HTML file named *index.html* to the *wwwroot* directory.</span></span> <span data-ttu-id="013e7-614">Reemplace el contenido por el siguiente marcado:</span><span class="sxs-lookup"><span data-stu-id="013e7-614">Replace its contents with the following markup:</span></span>

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

<span data-ttu-id="013e7-615">Agregue un archivo JavaScript denominado *site.js* al directorio *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="013e7-615">Add a JavaScript file named *site.js* to the *wwwroot* directory.</span></span> <span data-ttu-id="013e7-616">Reemplace el contenido por el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="013e7-616">Replace its contents with the following code:</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

<span data-ttu-id="013e7-617">Puede que sea necesario realizar un cambio en la configuración de inicio del proyecto de ASP.NET Core para probar la página HTML localmente:</span><span class="sxs-lookup"><span data-stu-id="013e7-617">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

* <span data-ttu-id="013e7-618">Abra *Properties\launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="013e7-618">Open *Properties\launchSettings.json*.</span></span>
* <span data-ttu-id="013e7-619">Quite la propiedad `launchUrl` para forzar a la aplicación a abrirse en *index.html*, esto es, el archivo predeterminado del proyecto.</span><span class="sxs-lookup"><span data-stu-id="013e7-619">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="013e7-620">En este ejemplo se llama a todos los métodos CRUD de la API web.</span><span class="sxs-lookup"><span data-stu-id="013e7-620">This sample calls all of the CRUD methods of the web API.</span></span> <span data-ttu-id="013e7-621">A continuación, encontrará algunas explicaciones de las llamadas a la API.</span><span class="sxs-lookup"><span data-stu-id="013e7-621">Following are explanations of the calls to the API.</span></span>

### <a name="get-a-list-of-to-do-items"></a><span data-ttu-id="013e7-622">Obtención de una lista de tareas pendientes</span><span class="sxs-lookup"><span data-stu-id="013e7-622">Get a list of to-do items</span></span>

<span data-ttu-id="013e7-623">Fetch envía una solicitud HTTP GET a la API web, que devuelve código JSON que representa una matriz de tareas pendientes.</span><span class="sxs-lookup"><span data-stu-id="013e7-623">Fetch sends an HTTP GET request to the web API, which returns JSON representing an array of to-do items.</span></span> <span data-ttu-id="013e7-624">La función de devolución de llamada `success` se invoca si la solicitud se realiza correctamente.</span><span class="sxs-lookup"><span data-stu-id="013e7-624">The `success` callback function is invoked if the request succeeds.</span></span> <span data-ttu-id="013e7-625">En la devolución de llamada, el DOM se actualiza con la información de la tarea pendiente.</span><span class="sxs-lookup"><span data-stu-id="013e7-625">In the callback, the DOM is updated with the to-do information.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a><span data-ttu-id="013e7-626">Incorporación de una tarea pendiente</span><span class="sxs-lookup"><span data-stu-id="013e7-626">Add a to-do item</span></span>

<span data-ttu-id="013e7-627">Fetch envía una solicitud HTTP POST con la tarea pendiente en el cuerpo de dicha solicitud.</span><span class="sxs-lookup"><span data-stu-id="013e7-627">Fetch sends an HTTP POST request with the to-do item in the request body.</span></span> <span data-ttu-id="013e7-628">Las opciones `accepts` y `contentType` se establecen en `application/json` para especificar el tipo de medio que se va a recibir y a enviar.</span><span class="sxs-lookup"><span data-stu-id="013e7-628">The `accepts` and `contentType` options are set to `application/json` to specify the media type being received and sent.</span></span> <span data-ttu-id="013e7-629">La tarea pendiente se convierte en JSON mediante [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span><span class="sxs-lookup"><span data-stu-id="013e7-629">The to-do item is converted to JSON by using [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span></span> <span data-ttu-id="013e7-630">Cuando la API devuelve un código de estado correcto, se invoca la función `getData` para actualizar la tabla HTML.</span><span class="sxs-lookup"><span data-stu-id="013e7-630">When the API returns a successful status code, the `getData` function is invoked to update the HTML table.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a><span data-ttu-id="013e7-631">Actualizar una tarea pendiente</span><span class="sxs-lookup"><span data-stu-id="013e7-631">Update a to-do item</span></span>

<span data-ttu-id="013e7-632">El hecho de actualizar una tarea pendiente es similar al de agregar una.</span><span class="sxs-lookup"><span data-stu-id="013e7-632">Updating a to-do item is similar to adding one.</span></span> <span data-ttu-id="013e7-633">El valor `url` cambia para agregar el identificador único del elemento, y `type` es `PUT`.</span><span class="sxs-lookup"><span data-stu-id="013e7-633">The `url` changes to add the unique identifier of the item, and the `type` is `PUT`.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a><span data-ttu-id="013e7-634">Eliminar una tarea pendiente</span><span class="sxs-lookup"><span data-stu-id="013e7-634">Delete a to-do item</span></span>

<span data-ttu-id="013e7-635">Para eliminar una tarea pendiente, hay que establecer el valor `type` de la llamada de AJAX en `DELETE` y especificar el identificador único de la tarea en la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="013e7-635">Deleting a to-do item is accomplished by setting the `type` on the AJAX call to `DELETE` and specifying the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

::: moniker-end

<a name="auth"></a>

## <a name="add-authentication-support-to-a-web-api"></a><span data-ttu-id="013e7-636">Agregar compatibilidad con la autenticación a una API web</span><span class="sxs-lookup"><span data-stu-id="013e7-636">Add authentication support to a web API</span></span>

<span data-ttu-id="013e7-637">Consulte el tutorial de [IdentityServer4](https://identityserver4.readthedocs.io/en/latest/quickstarts/0_overview.html).</span><span class="sxs-lookup"><span data-stu-id="013e7-637">See the [IdentityServer4](https://identityserver4.readthedocs.io/en/latest/quickstarts/0_overview.html) tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="013e7-638">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="013e7-638">Additional resources</span></span>

<span data-ttu-id="013e7-639">[Vea o descargue el código de ejemplo para este tutorial](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span><span class="sxs-lookup"><span data-stu-id="013e7-639">[View or download sample code for this tutorial](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span></span> <span data-ttu-id="013e7-640">Vea [cómo descargarlo](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="013e7-640">See [how to download](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="013e7-641">Para obtener más información, vea los siguientes recursos:</span><span class="sxs-lookup"><span data-stu-id="013e7-641">For more information, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/intro>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>
* [<span data-ttu-id="013e7-642">Versión en YouTube de este tutorial</span><span class="sxs-lookup"><span data-stu-id="013e7-642">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=TTkhEyGBfAk)
