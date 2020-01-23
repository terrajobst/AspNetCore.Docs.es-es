---
title: 'Tutorial: Creación de una API web con ASP.NET Core'
author: rick-anderson
description: Aprenda a crear de una API web con ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
uid: tutorials/first-web-api
ms.openlocfilehash: 73e547b014d78dcbcbf1c887ebec16e0743d10b9
ms.sourcegitcommit: f259889044d1fc0f0c7e3882df0008157ced4915
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/21/2020
ms.locfileid: "76294749"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core"></a><span data-ttu-id="7f2b0-103">Tutorial: Creación de una API web con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7f2b0-103">Tutorial: Create a web API with ASP.NET Core</span></span>

<span data-ttu-id="7f2b0-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="7f2b0-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="7f2b0-105">En este tutorial se enseñan los conceptos básicos de la compilación de una API web con ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-105">This tutorial teaches the basics of building a web API with ASP.NET Core.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="7f2b0-106">En este tutorial aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="7f2b0-107">Crear un proyecto de API web.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-107">Create a web API project.</span></span>
> * <span data-ttu-id="7f2b0-108">Agregar una clase de modelo y un contexto de base de datos.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-108">Add a model class and a database context.</span></span>
> * <span data-ttu-id="7f2b0-109">Aplicar scaffolding a un controlador con métodos CRUD.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-109">Scaffold a controller with CRUD methods.</span></span>
> * <span data-ttu-id="7f2b0-110">Configurar el enrutamiento, las rutas de acceso URL y los valores devueltos.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-110">Configure routing, URL paths, and return values.</span></span>
> * <span data-ttu-id="7f2b0-111">Llamar a la API web con Postman.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-111">Call the web API with Postman.</span></span>

<span data-ttu-id="7f2b0-112">Al final, tendrá una API web que pueda administrar los elementos "to-do" almacenados en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-112">At the end, you have a web API that can manage "to-do" items stored in a database.</span></span>

## <a name="overview"></a><span data-ttu-id="7f2b0-113">Información general</span><span class="sxs-lookup"><span data-stu-id="7f2b0-113">Overview</span></span>

<span data-ttu-id="7f2b0-114">En este tutorial se crea la siguiente API:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-114">This tutorial creates the following API:</span></span>

|<span data-ttu-id="7f2b0-115">API</span><span class="sxs-lookup"><span data-stu-id="7f2b0-115">API</span></span> | <span data-ttu-id="7f2b0-116">Descripción</span><span class="sxs-lookup"><span data-stu-id="7f2b0-116">Description</span></span> | <span data-ttu-id="7f2b0-117">Cuerpo de la solicitud</span><span class="sxs-lookup"><span data-stu-id="7f2b0-117">Request body</span></span> | <span data-ttu-id="7f2b0-118">Cuerpo de la respuesta</span><span class="sxs-lookup"><span data-stu-id="7f2b0-118">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="7f2b0-119">GET /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="7f2b0-119">GET /api/TodoItems</span></span> | <span data-ttu-id="7f2b0-120">Obtener todas las tareas pendientes</span><span class="sxs-lookup"><span data-stu-id="7f2b0-120">Get all to-do items</span></span> | <span data-ttu-id="7f2b0-121">Ninguna</span><span class="sxs-lookup"><span data-stu-id="7f2b0-121">None</span></span> | <span data-ttu-id="7f2b0-122">Matriz de tareas pendientes</span><span class="sxs-lookup"><span data-stu-id="7f2b0-122">Array of to-do items</span></span>|
|<span data-ttu-id="7f2b0-123">GET /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="7f2b0-123">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="7f2b0-124">Obtener un elemento por identificador</span><span class="sxs-lookup"><span data-stu-id="7f2b0-124">Get an item by ID</span></span> | <span data-ttu-id="7f2b0-125">None</span><span class="sxs-lookup"><span data-stu-id="7f2b0-125">None</span></span> | <span data-ttu-id="7f2b0-126">Tarea pendiente</span><span class="sxs-lookup"><span data-stu-id="7f2b0-126">To-do item</span></span>|
|<span data-ttu-id="7f2b0-127">POST /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="7f2b0-127">POST /api/TodoItems</span></span> | <span data-ttu-id="7f2b0-128">Incorporación de un nuevo elemento</span><span class="sxs-lookup"><span data-stu-id="7f2b0-128">Add a new item</span></span> | <span data-ttu-id="7f2b0-129">Tarea pendiente</span><span class="sxs-lookup"><span data-stu-id="7f2b0-129">To-do item</span></span> | <span data-ttu-id="7f2b0-130">Tarea pendiente</span><span class="sxs-lookup"><span data-stu-id="7f2b0-130">To-do item</span></span> |
|<span data-ttu-id="7f2b0-131">PUT /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="7f2b0-131">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="7f2b0-132">Actualizar un elemento existente &nbsp;</span><span class="sxs-lookup"><span data-stu-id="7f2b0-132">Update an existing item &nbsp;</span></span> | <span data-ttu-id="7f2b0-133">Tarea pendiente</span><span class="sxs-lookup"><span data-stu-id="7f2b0-133">To-do item</span></span> | <span data-ttu-id="7f2b0-134">None</span><span class="sxs-lookup"><span data-stu-id="7f2b0-134">None</span></span> |
|<span data-ttu-id="7f2b0-135">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="7f2b0-135">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="7f2b0-136">Eliminar un elemento &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="7f2b0-136">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="7f2b0-137">None</span><span class="sxs-lookup"><span data-stu-id="7f2b0-137">None</span></span> | <span data-ttu-id="7f2b0-138">None</span><span class="sxs-lookup"><span data-stu-id="7f2b0-138">None</span></span>|

<span data-ttu-id="7f2b0-139">En el diagrama siguiente, se muestra el diseño de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-139">The following diagram shows the design of the app.</span></span>

![El cliente está representado por un cuadro a la izquierda.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="7f2b0-145">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="7f2b0-145">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7f2b0-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7f2b0-146">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7f2b0-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7f2b0-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="7f2b0-148">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="7f2b0-148">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="7f2b0-149">Creación de un proyecto web</span><span class="sxs-lookup"><span data-stu-id="7f2b0-149">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7f2b0-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7f2b0-150">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="7f2b0-151">En el menú **Archivo**, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-151">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="7f2b0-152">Seleccione la plantilla **Aplicación web ASP.NET Core** y haga clic en **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-152">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="7f2b0-153">Asigne al proyecto el nombre *TodoApi* y haga clic en **Crear**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-153">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="7f2b0-154">En el cuadro de diálogo **Crear una aplicación web ASP.NET Core**, confirme que las opciones **.NET Core** y **ASP.NET Core 3.1** estén seleccionadas.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-154">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.1** are selected.</span></span> <span data-ttu-id="7f2b0-155">Seleccione la plantilla **API** y haga clic en **Crear**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-155">Select the **API** template and click **Create**.</span></span>

![Cuadro de diálogo de nuevo proyecto de VS](first-web-api/_static/vs3.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7f2b0-157">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7f2b0-157">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="7f2b0-158">Abra el [terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="7f2b0-158">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="7f2b0-159">Cambie los directorios (`cd`) a la carpeta que va a contener la carpeta del proyecto.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-159">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="7f2b0-160">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-160">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   cd TodoApi
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   code -r ../TodoApi
   ```

* <span data-ttu-id="7f2b0-161">Cuando en un cuadro de diálogo se le pregunte si quiere agregar al proyecto los recursos necesarios, seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-161">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

  <span data-ttu-id="7f2b0-162">Los comandos anteriores:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-162">The preceding commands:</span></span>

  * <span data-ttu-id="7f2b0-163">Crean un nuevo proyecto de API web y lo abren en Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-163">Creates a new web API project and opens it in Visual Studio Code.</span></span>
  * <span data-ttu-id="7f2b0-164">Agregan los paquetes de NuGet que se requieren en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-164">Adds the NuGet packages which are required in the next section.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="7f2b0-165">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="7f2b0-165">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="7f2b0-166">Seleccione **Archivo** > **Nueva solución**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-166">Select **File** > **New Solution**.</span></span>

  ![macOS: Nueva solución](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="7f2b0-168">Seleccione **.NET Core** > **Aplicación** > **API** > **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-168">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![Cuadro de diálogo de nuevo proyecto de macOS](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="7f2b0-170">En el cuadro de diálogo **Configure your new ASP.NET Core Web API** (Configurar la nueva API web de ASP.NET Core), seleccione **Plataforma de destino** de \* *.NET Core 3.1*.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-170">In the **Configure your new ASP.NET Core Web API** dialog, select **Target Framework** of \**.NET Core 3.1*.</span></span>

* <span data-ttu-id="7f2b0-171">Escriba *TodoApi* en **Nombre del proyecto** y seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-171">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![cuadro de diálogo de configuración](first-web-api-mac/_static/2.png)

[!INCLUDE[](~/includes/mac-terminal-access.md)]

<span data-ttu-id="7f2b0-173">Abra un terminal de comandos en la carpeta del proyecto y ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-173">Open a command terminal in the project folder and run the following commands:</span></span>

   ```dotnetcli
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   ```

---

### <a name="test-the-api"></a><span data-ttu-id="7f2b0-174">Prueba de la API</span><span class="sxs-lookup"><span data-stu-id="7f2b0-174">Test the API</span></span>

<span data-ttu-id="7f2b0-175">La plantilla del proyecto crea una API `WeatherForecast`.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-175">The project template creates a `WeatherForecast` API.</span></span> <span data-ttu-id="7f2b0-176">Llame al método `Get` desde un explorador para probar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-176">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7f2b0-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7f2b0-177">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="7f2b0-178">Presione Ctrl+F5 para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-178">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="7f2b0-179">Visual Studio inicia un explorador y navega hasta `https://localhost:<port>/WeatherForecast`, donde `<port>` es un número de puerto elegido aleatoriamente.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-179">Visual Studio launches a browser and navigates to `https://localhost:<port>/WeatherForecast`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="7f2b0-180">Si aparece un cuadro de diálogo en que se le pregunta si debe confiar en el certificado de IIS Express, seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-180">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="7f2b0-181">En el cuadro de diálogo **Advertencia de seguridad** que aparece a continuación, seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-181">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7f2b0-182">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7f2b0-182">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="7f2b0-183">Presione Ctrl+F5 para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-183">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="7f2b0-184">En un explorador, vaya a la siguiente dirección URL: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span><span class="sxs-lookup"><span data-stu-id="7f2b0-184">In a browser, go to following URL: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="7f2b0-185">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="7f2b0-185">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="7f2b0-186">Seleccione **Ejecutar** > **Iniciar depuración** para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-186">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="7f2b0-187">Visual Studio para Mac inicia un explorador y navega hasta `https://localhost:<port>`, donde `<port>` es un número de puerto elegido aleatoriamente.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-187">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="7f2b0-188">Se devuelve un error HTTP 404 (No encontrado).</span><span class="sxs-lookup"><span data-stu-id="7f2b0-188">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="7f2b0-189">Anexe `/WeatherForecast` a la dirección URL (cambie la dirección URL a `https://localhost:<port>/WeatherForecast`).</span><span class="sxs-lookup"><span data-stu-id="7f2b0-189">Append `/WeatherForecast` to the URL (change the URL to `https://localhost:<port>/WeatherForecast`).</span></span>

---

<span data-ttu-id="7f2b0-190">Se devuelve un JSON similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-190">JSON similar to the following is returned:</span></span>

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

## <a name="add-a-model-class"></a><span data-ttu-id="7f2b0-191">Incorporación de una clase de modelo</span><span class="sxs-lookup"><span data-stu-id="7f2b0-191">Add a model class</span></span>

<span data-ttu-id="7f2b0-192">Un *modelo* es un conjunto de clases que representan los datos que la aplicación administra.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-192">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="7f2b0-193">El modelo para esta aplicación es una clase `TodoItem` única.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-193">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7f2b0-194">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7f2b0-194">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="7f2b0-195">En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-195">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="7f2b0-196">Seleccione **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-196">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="7f2b0-197">Asigne a la carpeta el nombre *Models*.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-197">Name the folder *Models*.</span></span>

* <span data-ttu-id="7f2b0-198">Haga clic con el botón derecho en la carpeta *Models* y seleccione **Agregar** > **Clase**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-198">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="7f2b0-199">Asigne a la clase el nombre *TodoItem* y seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-199">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="7f2b0-200">Reemplace el código de plantilla por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-200">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7f2b0-201">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7f2b0-201">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="7f2b0-202">Agregue una carpeta denominada *Models*.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-202">Add a folder named *Models*.</span></span>

* <span data-ttu-id="7f2b0-203">Agregue una clase `TodoItem` a la carpeta *Models* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-203">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="7f2b0-204">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="7f2b0-204">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="7f2b0-205">Haga clic con el botón derecho en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-205">Right-click the project.</span></span> <span data-ttu-id="7f2b0-206">Seleccione **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-206">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="7f2b0-207">Asigne a la carpeta el nombre *Models*.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-207">Name the folder *Models*.</span></span>

  ![nueva carpeta](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="7f2b0-209">Haga clic con el botón derecho en la carpeta *Modelos* y seleccione **Agregar** > **Nuevo archivo** > **General** > **Clase vacía**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-209">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="7f2b0-210">Asigne a la clase el nombre *TodoItem* y haga clic en **Nuevo**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-210">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="7f2b0-211">Reemplace el código de plantilla por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-211">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="7f2b0-212">La propiedad `Id` funciona como clave única en una base de datos relacional.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-212">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="7f2b0-213">Las clases de modelo pueden ir en cualquier lugar del proyecto, pero convencionalmente e usa la carpeta *Models*.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-213">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="7f2b0-214">Incorporación de un contexto de base de datos</span><span class="sxs-lookup"><span data-stu-id="7f2b0-214">Add a database context</span></span>

<span data-ttu-id="7f2b0-215">El *contexto de base de datos* es la clase principal que coordina la funcionalidad de Entity Framework para un modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-215">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="7f2b0-216">Esta clase se crea derivándola de la clase `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-216">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7f2b0-217">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7f2b0-217">Visual Studio</span></span>](#tab/visual-studio)

### <a name="add-microsoftentityframeworkcoresqlserver"></a><span data-ttu-id="7f2b0-218">Adición de Microsoft.EntityFrameworkCore.SqlServer</span><span class="sxs-lookup"><span data-stu-id="7f2b0-218">Add Microsoft.EntityFrameworkCore.SqlServer</span></span>

* <span data-ttu-id="7f2b0-219">En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet > Administrar paquetes NuGet para la solución**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-219">From the **Tools** menu, select **NuGet Package Manager > Manage NuGet Packages for Solution**.</span></span>
* <span data-ttu-id="7f2b0-220">Seleccione la pestaña **Examinar** y escriba **Microsoft.EntityFrameworkCore.SqlServer** en el cuadro de búsqueda.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-220">Select the **Browse** tab, and then enter **Microsoft.EntityFrameworkCore.SqlServer** in the search box.</span></span>
* <span data-ttu-id="7f2b0-221">Seleccione **Microsoft.EntityFrameworkCore.SqlServer** en el panel izquierdo.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-221">Select **Microsoft.EntityFrameworkCore.SqlServer** in the left pane.</span></span>
* <span data-ttu-id="7f2b0-222">Active la casilla **Proyecto** en el panel derecho y, después, seleccione **Instalar**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-222">Select the **Project** check box in the right pane and then select **Install**.</span></span>
* <span data-ttu-id="7f2b0-223">Siga las instrucciones anteriores para agregar el paquete NuGet `Microsoft.EntityFrameworkCore.InMemory`.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-223">Use the preceding instructions to add the `Microsoft.EntityFrameworkCore.InMemory` NuGet package.</span></span>

![Administrador de paquetes de NuGet](first-web-api/_static/vs3NuGet.png)

## <a name="add-the-todocontext-database-context"></a><span data-ttu-id="7f2b0-225">Adición del contexto de la base de datos TodoContext</span><span class="sxs-lookup"><span data-stu-id="7f2b0-225">Add the TodoContext database context</span></span>

* <span data-ttu-id="7f2b0-226">Haga clic con el botón derecho en la carpeta *Models* y seleccione **Agregar** > **Clase**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-226">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="7f2b0-227">Asigne a la clase el nombre *TodoContext* y haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-227">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="7f2b0-228">Visual Studio Code/Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="7f2b0-228">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="7f2b0-229">Agregue una clase `TodoContext` a la carpeta *Models*.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-229">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="7f2b0-230">Escriba el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-230">Enter the following code:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="7f2b0-231">Registro del contexto de base de datos</span><span class="sxs-lookup"><span data-stu-id="7f2b0-231">Register the database context</span></span>

<span data-ttu-id="7f2b0-232">En ASP.NET Core, los servicios (como el contexto de la base de datos) deben registrarse con el contenedor de [inserción de dependencias (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="7f2b0-232">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="7f2b0-233">El contenedor proporciona el servicio a los controladores.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-233">The container provides the service to controllers.</span></span>

<span data-ttu-id="7f2b0-234">Actualice *Startup.cs* con el siguiente código resaltado:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-234">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Startup.cs?highlight=7-8,23-24&name=snippet_all)]

<span data-ttu-id="7f2b0-235">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-235">The preceding code:</span></span>

* <span data-ttu-id="7f2b0-236">Elimina las declaraciones `using` no utilizadas.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-236">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="7f2b0-237">Agrega el contexto de base de datos para el contenedor de DI.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-237">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="7f2b0-238">Especifica que el contexto de base de datos usará una base de datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-238">Specifies that the database context will use an in-memory database.</span></span>

## <a name="scaffold-a-controller"></a><span data-ttu-id="7f2b0-239">Scaffolding de un controlador</span><span class="sxs-lookup"><span data-stu-id="7f2b0-239">Scaffold a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7f2b0-240">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7f2b0-240">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="7f2b0-241">Haga clic con el botón derecho en la carpeta *Controllers*.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-241">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="7f2b0-242">Seleccione **Agregar** > **Nuevo elemento con scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-242">Select **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="7f2b0-243">Seleccione **Controlador de API con acciones mediante Entity Framework** y, después, seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-243">Select **API Controller with actions, using Entity Framework**, and then select **Add**.</span></span>
* <span data-ttu-id="7f2b0-244">En el cuadro de diálogo **Add API Controller with actions, using Entity Framework** (Agregar controlador de API con acciones mediante Entity Framework):</span><span class="sxs-lookup"><span data-stu-id="7f2b0-244">In the **Add API Controller with actions, using Entity Framework** dialog:</span></span>

  * <span data-ttu-id="7f2b0-245">Seleccione **TodoItem (TodoApi.Models)** en **Clase de modelo**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-245">Select **TodoItem (TodoApi.Models)** in the **Model class**.</span></span>
  * <span data-ttu-id="7f2b0-246">Seleccione **TodoContext (TodoApi.Models)** en **Clase de contexto de datos**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-246">Select **TodoContext (TodoApi.Models)** in the **Data context class**.</span></span>
  * <span data-ttu-id="7f2b0-247">Seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-247">Select **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="7f2b0-248">Visual Studio Code/Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="7f2b0-248">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="7f2b0-249">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-249">Run the following commands:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator controller -name TodoItemsController -async -api -m TodoItem -dc TodoContext -outDir Controllers
```

<span data-ttu-id="7f2b0-250">Los comandos anteriores:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-250">The preceding commands:</span></span>

* <span data-ttu-id="7f2b0-251">Agregan los paquetes NuGet necesarios para realizar scaffolding.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-251">Add NuGet packages required for scaffolding.</span></span>
* <span data-ttu-id="7f2b0-252">Instalan el motor de scaffolding (`dotnet-aspnet-codegenerator`).</span><span class="sxs-lookup"><span data-stu-id="7f2b0-252">Installs the scaffolding engine (`dotnet-aspnet-codegenerator`).</span></span>
* <span data-ttu-id="7f2b0-253">Aplican scaffolding a `TodoItemsController`.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-253">Scaffolds the `TodoItemsController`.</span></span>

---

<span data-ttu-id="7f2b0-254">El código generado:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-254">The generated code:</span></span>

* <span data-ttu-id="7f2b0-255">Marca la clase con el atributo [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute).</span><span class="sxs-lookup"><span data-stu-id="7f2b0-255">Marks the class with the [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="7f2b0-256">Este atributo indica que el controlador responde a las solicitudes de la API web.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-256">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="7f2b0-257">Para información sobre comportamientos específicos que permite el atributo, consulte <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-257">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="7f2b0-258">Utiliza la inserción de dependencias para insertar el contexto de base de datos (`TodoContext`) en el controlador.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-258">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="7f2b0-259">El contexto de base de datos se usa en cada uno de los métodos [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) del controlador.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-259">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

## <a name="examine-the-posttodoitem-create-method"></a><span data-ttu-id="7f2b0-260">Examen del método create de PostTodoItem</span><span class="sxs-lookup"><span data-stu-id="7f2b0-260">Examine the PostTodoItem create method</span></span>

<span data-ttu-id="7f2b0-261">Reemplace la instrucción return en `PostTodoItem` para usar el operador [nameof](/dotnet/csharp/language-reference/operators/nameof):</span><span class="sxs-lookup"><span data-stu-id="7f2b0-261">Replace the return statement in the `PostTodoItem` to use the [nameof](/dotnet/csharp/language-reference/operators/nameof) operator:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Create)]

<span data-ttu-id="7f2b0-262">El código anterior es un método HTTP POST, indicado por el atributo [`[HttpPost]`](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="7f2b0-262">The preceding code is an HTTP POST method, as indicated by the [`[HttpPost]`](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="7f2b0-263">El método obtiene el valor de tareas pendientes del cuerpo de la solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-263">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="7f2b0-264">El método <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> realiza las acciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-264">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> method:</span></span>

* <span data-ttu-id="7f2b0-265">Devuelve un código de estado HTTP 201 cuando se ha ejecutado correctamente.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-265">Returns an HTTP 201 status code if successful.</span></span> <span data-ttu-id="7f2b0-266">HTTP 201 es la respuesta estándar para un método HTTP POST que crea un recurso en el servidor.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-266">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="7f2b0-267">Agrega un encabezado [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) a la respuesta.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-267">Adds a [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) header to the response.</span></span> <span data-ttu-id="7f2b0-268">El encabezado `Location` especifica el [URI](https://developer.mozilla.org/docs/Glossary/URI) de la tarea pendiente recién creada.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-268">The `Location` header specifies the [URI](https://developer.mozilla.org/docs/Glossary/URI) of the newly created to-do item.</span></span> <span data-ttu-id="7f2b0-269">Para obtener más información, consulte [10.2.2 201 creado](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="7f2b0-269">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="7f2b0-270">Hace referencia a la acción `GetTodoItem` para crear el identificador URI del encabezado `Location`.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-270">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="7f2b0-271">La palabra clave `nameof` de C# se usa para evitar que se codifique de forma rígida el nombre de acción en la llamada a `CreatedAtAction`.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-271">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

### <a name="install-postman"></a><span data-ttu-id="7f2b0-272">Instalación de Postman</span><span class="sxs-lookup"><span data-stu-id="7f2b0-272">Install Postman</span></span>

<span data-ttu-id="7f2b0-273">En este tutorial se usa Postman para probar la API web.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-273">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="7f2b0-274">Instale [Postman](https://www.getpostman.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="7f2b0-274">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="7f2b0-275">Inicie la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-275">Start the web app.</span></span>
* <span data-ttu-id="7f2b0-276">Inicie Postman.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-276">Start Postman.</span></span>
* <span data-ttu-id="7f2b0-277">Deshabilite **Comprobación del certificado SSL**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-277">Disable **SSL certificate verification**</span></span>
  * <span data-ttu-id="7f2b0-278">En **Archivo** > **Configuración** (pestaña **General**), deshabilite **Comprobación del certificado SSL**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-278">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>
    > [!WARNING]
    > <span data-ttu-id="7f2b0-279">Vuelva a habilitar la comprobación del certificado SSL tras probar el controlador.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-279">Re-enable SSL certificate verification after testing the controller.</span></span>

<a name="post"></a>

### <a name="test-posttodoitem-with-postman"></a><span data-ttu-id="7f2b0-280">Prueba de PostTodoItem con Postman</span><span class="sxs-lookup"><span data-stu-id="7f2b0-280">Test PostTodoItem with Postman</span></span>

* <span data-ttu-id="7f2b0-281">Cree una nueva solicitud.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-281">Create a new request.</span></span>
* <span data-ttu-id="7f2b0-282">Establezca el método HTTP en `POST`.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-282">Set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="7f2b0-283">Seleccione la pestaña **Cuerpo**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-283">Select the **Body** tab.</span></span>
* <span data-ttu-id="7f2b0-284">Seleccione el botón de radio **Raw** (Sin formato).</span><span class="sxs-lookup"><span data-stu-id="7f2b0-284">Select the **raw** radio button.</span></span>
* <span data-ttu-id="7f2b0-285">Establezca el tipo en **JSON (application/json)** .</span><span class="sxs-lookup"><span data-stu-id="7f2b0-285">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="7f2b0-286">En el cuerpo de la solicitud, introduzca JSON para una tarea pendiente:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-286">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="7f2b0-287">Seleccione **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-287">Select **Send**.</span></span>

  ![Postman con solicitud de creación](first-web-api/_static/3/create.png)

### <a name="test-the-location-header-uri"></a><span data-ttu-id="7f2b0-289">Prueba del URI del encabezado de ubicación</span><span class="sxs-lookup"><span data-stu-id="7f2b0-289">Test the location header URI</span></span>

* <span data-ttu-id="7f2b0-290">Seleccione la pestaña **Encabezados** en el panel **Respuesta**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-290">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="7f2b0-291">Copie el valor de encabezado **Ubicación**:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-291">Copy the **Location** header value:</span></span>

  ![Pestaña Encabezados de la consola de Postman](first-web-api/_static/3/create.png)

* <span data-ttu-id="7f2b0-293">Establezca el método en GET.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-293">Set the method to GET.</span></span>
* <span data-ttu-id="7f2b0-294">Pegue el URI (por ejemplo, `https://localhost:5001/api/TodoItems/1`).</span><span class="sxs-lookup"><span data-stu-id="7f2b0-294">Paste the URI (for example, `https://localhost:5001/api/TodoItems/1`).</span></span>
* <span data-ttu-id="7f2b0-295">Seleccione **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-295">Select **Send**.</span></span>

## <a name="examine-the-get-methods"></a><span data-ttu-id="7f2b0-296">Examen de los métodos GET</span><span class="sxs-lookup"><span data-stu-id="7f2b0-296">Examine the GET methods</span></span>

<span data-ttu-id="7f2b0-297">Estos métodos implementan dos puntos de conexión GET:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-297">These methods implement two GET endpoints:</span></span>

* `GET /api/TodoItems`
* `GET /api/TodoItems/{id}`

<span data-ttu-id="7f2b0-298">Llame a los dos puntos de conexión desde un explorador o Postman para probar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-298">Test the app by calling the two endpoints from a browser or Postman.</span></span> <span data-ttu-id="7f2b0-299">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-299">For example:</span></span>

* [https://localhost:5001/api/TodoItems](https://localhost:5001/api/TodoItems)
* [https://localhost:5001/api/TodoItems/1](https://localhost:5001/api/TodoItems/1)

<span data-ttu-id="7f2b0-300">La llamada a `GetTodoItems` genera una respuesta similar a la siguiente:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-300">A response similar to the following is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

### <a name="test-get-with-postman"></a><span data-ttu-id="7f2b0-301">Prueba de Get con Postman</span><span class="sxs-lookup"><span data-stu-id="7f2b0-301">Test Get with Postman</span></span>

* <span data-ttu-id="7f2b0-302">Cree una nueva solicitud.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-302">Create a new request.</span></span>
* <span data-ttu-id="7f2b0-303">Establezca el método HTTP en **GET**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-303">Set the HTTP method to **GET**.</span></span>
* <span data-ttu-id="7f2b0-304">Establezca la dirección URL de la solicitud en `https://localhost:<port>/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-304">Set the request URL to `https://localhost:<port>/api/TodoItems`.</span></span> <span data-ttu-id="7f2b0-305">Por ejemplo: `https://localhost:5001/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-305">For example, `https://localhost:5001/api/TodoItems`.</span></span>
* <span data-ttu-id="7f2b0-306">Establezca **Vista de dos paneles** en Postman.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-306">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="7f2b0-307">Seleccione **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-307">Select **Send**.</span></span>

<span data-ttu-id="7f2b0-308">Esta aplicación utiliza una base de datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-308">This app uses an in-memory database.</span></span> <span data-ttu-id="7f2b0-309">Si la aplicación se detiene y se inicia, la solicitud GET precedente no devolverá ningún dato.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-309">If the app is stopped and started, the preceding GET request will not return any data.</span></span> <span data-ttu-id="7f2b0-310">Si no se devuelve ningún dato, publique los datos en la aplicación con [POST](#post).</span><span class="sxs-lookup"><span data-stu-id="7f2b0-310">If no data is returned, [POST](#post) data to the app.</span></span>

## <a name="routing-and-url-paths"></a><span data-ttu-id="7f2b0-311">Enrutamiento y rutas URL</span><span class="sxs-lookup"><span data-stu-id="7f2b0-311">Routing and URL paths</span></span>

<span data-ttu-id="7f2b0-312">El atributo [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) indica un método que responde a una solicitud HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-312">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="7f2b0-313">La ruta de dirección URL para cada método se construye como sigue:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-313">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="7f2b0-314">Comience por la cadena de plantilla en el atributo `Route` del controlador:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-314">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=TodoController&highlight=1)]

* <span data-ttu-id="7f2b0-315">Reemplace `[controller]` por el nombre del controlador, que convencionalmente es el nombre de clase de controlador sin el sufijo "Controller".</span><span class="sxs-lookup"><span data-stu-id="7f2b0-315">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="7f2b0-316">En este ejemplo, el nombre de clase de controlador es **TodoItems**Controller; por tanto, el nombre del controlador es "TodoItems".</span><span class="sxs-lookup"><span data-stu-id="7f2b0-316">For this sample, the controller class name is **TodoItems**Controller, so the controller name is "TodoItems".</span></span> <span data-ttu-id="7f2b0-317">El [enrutamiento](xref:mvc/controllers/routing) en ASP.NET Core no distingue entre mayúsculas y minúsculas.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-317">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="7f2b0-318">Si el atributo `[HttpGet]` tiene una plantilla de ruta (por ejemplo, `[HttpGet("products")]`), anéxela a la ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-318">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="7f2b0-319">En este ejemplo no se usa una plantilla.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-319">This sample doesn't use a template.</span></span> <span data-ttu-id="7f2b0-320">Para más información, vea [Enrutamiento mediante atributos con atributos Http[Verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="7f2b0-320">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="7f2b0-321">En el siguiente método `GetTodoItem`, `"{id}"` es una variable de marcador de posición correspondiente al identificador único de la tarea pendiente.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-321">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="7f2b0-322">Al invocar a `GetTodoItem`, el valor `"{id}"` de la URL se proporciona al método en su parámetro `id`.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-322">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its `id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="7f2b0-323">Valores devueltos</span><span class="sxs-lookup"><span data-stu-id="7f2b0-323">Return values</span></span>

<span data-ttu-id="7f2b0-324">El tipo de valor devuelto de los métodos `GetTodoItems` y `GetTodoItem` es [ActionResult\<T > type](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="7f2b0-324">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="7f2b0-325">ASP.NET Core serializa automáticamente el objeto a [JSON](https://www.json.org/) y escribe el JSON en el cuerpo del mensaje de respuesta.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-325">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="7f2b0-326">El código de respuesta para este tipo de valor devuelto es el 200, suponiendo que no haya ninguna excepción no controlada.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-326">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="7f2b0-327">Las excepciones no controladas se convierten en errores 5xx.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-327">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="7f2b0-328">Los tipos de valores devueltos `ActionResult` pueden representar una gama amplia de códigos de estado HTTP.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-328">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="7f2b0-329">Por ejemplo, `GetTodoItem` puede devolver dos valores de estado diferentes:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-329">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="7f2b0-330">Si no hay ningún elemento que coincida con el identificador solicitado, el método devolverá un código de error 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound).</span><span class="sxs-lookup"><span data-stu-id="7f2b0-330">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="7f2b0-331">En caso contrario, el método devuelve 200 con un cuerpo de respuesta JSON.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-331">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="7f2b0-332">Devolver `item` genera una respuesta HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-332">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="the-puttodoitem-method"></a><span data-ttu-id="7f2b0-333">Método PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="7f2b0-333">The PutTodoItem method</span></span>

<span data-ttu-id="7f2b0-334">Examine el método `PutTodoItem`:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-334">Examine the `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Update)]

<span data-ttu-id="7f2b0-335">`PutTodoItem` es similar a `PostTodoItem`, salvo por el hecho de que usa HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-335">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="7f2b0-336">La respuesta es [204 Sin contenido](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="7f2b0-336">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="7f2b0-337">Según la especificación HTTP, una solicitud PUT requiere que el cliente envíe toda la entidad actualizada, no solo los cambios.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-337">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="7f2b0-338">Para admitir actualizaciones parciales, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="7f2b0-338">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="7f2b0-339">Si recibe un error al llamar a `PutTodoItem`, llame a `GET` para asegurarse de que hay un elemento en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-339">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="7f2b0-340">Prueba del método PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="7f2b0-340">Test the PutTodoItem method</span></span>

<span data-ttu-id="7f2b0-341">En este ejemplo se usa una base de datos en memoria que se debe inicializar cada vez que se inicia la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-341">This sample uses an in-memory database that must be initialized each time the app is started.</span></span> <span data-ttu-id="7f2b0-342">Debe haber un elemento en la base de datos antes de que realice una llamada PUT.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-342">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="7f2b0-343">Llame a GET para asegurarse de que hay un elemento en la base de datos antes de realizar una llamada PUT.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-343">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="7f2b0-344">Actualice el elemento to-do que tiene el id. = 1 y establezca su nombre en "feed fish":</span><span class="sxs-lookup"><span data-stu-id="7f2b0-344">Update the to-do item that has ID = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="7f2b0-345">En la imagen siguiente, se muestra la actualización de Postman:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-345">The following image shows the Postman update:</span></span>

![Consola de Postman que muestra la respuesta 204 (Sin contenido)](first-web-api/_static/3/pmcput.png)

## <a name="the-deletetodoitem-method"></a><span data-ttu-id="7f2b0-347">Método DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="7f2b0-347">The DeleteTodoItem method</span></span>

<span data-ttu-id="7f2b0-348">Examine el método `DeleteTodoItem`:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-348">Examine the `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Delete)]

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="7f2b0-349">Prueba del método DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="7f2b0-349">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="7f2b0-350">Use Postman para eliminar una tarea pendiente:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-350">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="7f2b0-351">Establezca el método en `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-351">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="7f2b0-352">Establezca el URI del objeto que quiera eliminar (por ejemplo, `https://localhost:5001/api/TodoItems/1`).</span><span class="sxs-lookup"><span data-stu-id="7f2b0-352">Set the URI of the object to delete (for example `https://localhost:5001/api/TodoItems/1`).</span></span>
* <span data-ttu-id="7f2b0-353">Seleccione **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-353">Select **Send**.</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="7f2b0-354">Llamar a la API web con JavaScript</span><span class="sxs-lookup"><span data-stu-id="7f2b0-354">Call the web API with JavaScript</span></span>

<span data-ttu-id="7f2b0-355">Consulte [Tutorial: Llamada a una API web de ASP.NET Core con JavaScript](xref:tutorials/web-api-javascript).</span><span class="sxs-lookup"><span data-stu-id="7f2b0-355">See [Tutorial: Call an ASP.NET Core web API with JavaScript](xref:tutorials/web-api-javascript).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="7f2b0-356">En este tutorial aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-356">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="7f2b0-357">Crear un proyecto de API web.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-357">Create a web API project.</span></span>
> * <span data-ttu-id="7f2b0-358">Agregar una clase de modelo y un contexto de base de datos.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-358">Add a model class and a database context.</span></span>
> * <span data-ttu-id="7f2b0-359">Agregar un controlador.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-359">Add a controller.</span></span>
> * <span data-ttu-id="7f2b0-360">Agregar métodos CRUD.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-360">Add CRUD methods.</span></span>
> * <span data-ttu-id="7f2b0-361">Configurar el enrutamiento y las rutas de dirección URL.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-361">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="7f2b0-362">Especificar los valores devueltos.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-362">Specify return values.</span></span>
> * <span data-ttu-id="7f2b0-363">Llamar a la API web con Postman.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-363">Call the web API with Postman.</span></span>
> * <span data-ttu-id="7f2b0-364">Llamar a la API web con JavaScript.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-364">Call the web API with JavaScript.</span></span>

<span data-ttu-id="7f2b0-365">Al final, tendrá una API web que pueda administrar las tareas "pendientes" almacenadas en una base de datos relacional.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-365">At the end, you have a web API that can manage "to-do" items stored in a relational database.</span></span>

## <a name="overview"></a><span data-ttu-id="7f2b0-366">Información general</span><span class="sxs-lookup"><span data-stu-id="7f2b0-366">Overview</span></span>

<span data-ttu-id="7f2b0-367">En este tutorial se crea la siguiente API:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-367">This tutorial creates the following API:</span></span>

|<span data-ttu-id="7f2b0-368">API</span><span class="sxs-lookup"><span data-stu-id="7f2b0-368">API</span></span> | <span data-ttu-id="7f2b0-369">Descripción</span><span class="sxs-lookup"><span data-stu-id="7f2b0-369">Description</span></span> | <span data-ttu-id="7f2b0-370">Cuerpo de la solicitud</span><span class="sxs-lookup"><span data-stu-id="7f2b0-370">Request body</span></span> | <span data-ttu-id="7f2b0-371">Cuerpo de la respuesta</span><span class="sxs-lookup"><span data-stu-id="7f2b0-371">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="7f2b0-372">GET /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="7f2b0-372">GET /api/TodoItems</span></span> | <span data-ttu-id="7f2b0-373">Obtener todas las tareas pendientes</span><span class="sxs-lookup"><span data-stu-id="7f2b0-373">Get all to-do items</span></span> | <span data-ttu-id="7f2b0-374">Ninguna</span><span class="sxs-lookup"><span data-stu-id="7f2b0-374">None</span></span> | <span data-ttu-id="7f2b0-375">Matriz de tareas pendientes</span><span class="sxs-lookup"><span data-stu-id="7f2b0-375">Array of to-do items</span></span>|
|<span data-ttu-id="7f2b0-376">GET /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="7f2b0-376">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="7f2b0-377">Obtener un elemento por identificador</span><span class="sxs-lookup"><span data-stu-id="7f2b0-377">Get an item by ID</span></span> | <span data-ttu-id="7f2b0-378">None</span><span class="sxs-lookup"><span data-stu-id="7f2b0-378">None</span></span> | <span data-ttu-id="7f2b0-379">Tarea pendiente</span><span class="sxs-lookup"><span data-stu-id="7f2b0-379">To-do item</span></span>|
|<span data-ttu-id="7f2b0-380">POST /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="7f2b0-380">POST /api/TodoItems</span></span> | <span data-ttu-id="7f2b0-381">Incorporación de un nuevo elemento</span><span class="sxs-lookup"><span data-stu-id="7f2b0-381">Add a new item</span></span> | <span data-ttu-id="7f2b0-382">Tarea pendiente</span><span class="sxs-lookup"><span data-stu-id="7f2b0-382">To-do item</span></span> | <span data-ttu-id="7f2b0-383">Tarea pendiente</span><span class="sxs-lookup"><span data-stu-id="7f2b0-383">To-do item</span></span> |
|<span data-ttu-id="7f2b0-384">PUT /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="7f2b0-384">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="7f2b0-385">Actualizar un elemento existente &nbsp;</span><span class="sxs-lookup"><span data-stu-id="7f2b0-385">Update an existing item &nbsp;</span></span> | <span data-ttu-id="7f2b0-386">Tarea pendiente</span><span class="sxs-lookup"><span data-stu-id="7f2b0-386">To-do item</span></span> | <span data-ttu-id="7f2b0-387">None</span><span class="sxs-lookup"><span data-stu-id="7f2b0-387">None</span></span> |
|<span data-ttu-id="7f2b0-388">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="7f2b0-388">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="7f2b0-389">Eliminar un elemento &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="7f2b0-389">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="7f2b0-390">None</span><span class="sxs-lookup"><span data-stu-id="7f2b0-390">None</span></span> | <span data-ttu-id="7f2b0-391">None</span><span class="sxs-lookup"><span data-stu-id="7f2b0-391">None</span></span>|

<span data-ttu-id="7f2b0-392">En el diagrama siguiente, se muestra el diseño de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-392">The following diagram shows the design of the app.</span></span>

![El cliente está representado por un cuadro a la izquierda.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="7f2b0-398">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="7f2b0-398">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7f2b0-399">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7f2b0-399">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7f2b0-400">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7f2b0-400">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="7f2b0-401">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="7f2b0-401">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="7f2b0-402">Creación de un proyecto web</span><span class="sxs-lookup"><span data-stu-id="7f2b0-402">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7f2b0-403">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7f2b0-403">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="7f2b0-404">En el menú **Archivo**, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-404">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="7f2b0-405">Seleccione la plantilla **Aplicación web ASP.NET Core** y haga clic en **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-405">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="7f2b0-406">Asigne al proyecto el nombre *TodoApi* y haga clic en **Crear**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-406">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="7f2b0-407">En el cuadro de diálogo **Crear una aplicación web ASP.NET Core**, confirme que las opciones **.NET Core** y **ASP.NET Core 2.2** estén seleccionadas.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-407">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 2.2** are selected.</span></span> <span data-ttu-id="7f2b0-408">Seleccione la plantilla **API** y haga clic en **Crear**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-408">Select the **API** template and click **Create**.</span></span> <span data-ttu-id="7f2b0-409">**No** seleccione **Habilitar compatibilidad con Docker**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-409">**Don't** select **Enable Docker Support**.</span></span>

![Cuadro de diálogo de nuevo proyecto de VS](first-web-api/_static/vs.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7f2b0-411">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7f2b0-411">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="7f2b0-412">Abra el [terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="7f2b0-412">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="7f2b0-413">Cambie los directorios (`cd`) a la carpeta que va a contener la carpeta del proyecto.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-413">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="7f2b0-414">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-414">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  <span data-ttu-id="7f2b0-415">Estos comandos crean un nuevo proyecto de API web y abren una nueva instancia de Visual Studio Code en la carpeta del proyecto nuevo.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-415">These commands create a new web API project and open a new instance of Visual Studio Code in the new project folder.</span></span>

* <span data-ttu-id="7f2b0-416">Cuando en un cuadro de diálogo se le pregunte si quiere agregar al proyecto los recursos necesarios, seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-416">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="7f2b0-417">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="7f2b0-417">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="7f2b0-418">Seleccione **Archivo** > **Nueva solución**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-418">Select **File** > **New Solution**.</span></span>

  ![macOS: Nueva solución](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="7f2b0-420">Seleccione **.NET Core** > **Aplicación** > **API** > **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-420">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![Cuadro de diálogo de nuevo proyecto de macOS](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="7f2b0-422">En el cuadro de diálogo **Configurar la nueva API web de ASP.NET Core**, acepte la **plataforma de destino** predeterminada de \* *.NET Core 2.2*.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-422">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="7f2b0-423">Escriba *TodoApi* en **Nombre del proyecto** y seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-423">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![cuadro de diálogo de configuración](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a><span data-ttu-id="7f2b0-425">Prueba de la API</span><span class="sxs-lookup"><span data-stu-id="7f2b0-425">Test the API</span></span>

<span data-ttu-id="7f2b0-426">La plantilla del proyecto crea una API `values`.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-426">The project template creates a `values` API.</span></span> <span data-ttu-id="7f2b0-427">Llame al método `Get` desde un explorador para probar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-427">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7f2b0-428">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7f2b0-428">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="7f2b0-429">Presione Ctrl+F5 para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-429">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="7f2b0-430">Visual Studio inicia un explorador y navega hasta `https://localhost:<port>/api/values`, donde `<port>` es un número de puerto elegido aleatoriamente.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-430">Visual Studio launches a browser and navigates to `https://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="7f2b0-431">Si aparece un cuadro de diálogo en que se le pregunta si debe confiar en el certificado de IIS Express, seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-431">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="7f2b0-432">En el cuadro de diálogo **Advertencia de seguridad** que aparece a continuación, seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-432">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7f2b0-433">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7f2b0-433">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="7f2b0-434">Presione Ctrl+F5 para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-434">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="7f2b0-435">En un explorador, vaya a la siguiente dirección URL: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span><span class="sxs-lookup"><span data-stu-id="7f2b0-435">In a browser, go to following URL: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="7f2b0-436">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="7f2b0-436">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="7f2b0-437">Seleccione **Ejecutar** > **Iniciar depuración** para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-437">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="7f2b0-438">Visual Studio para Mac inicia un explorador y navega hasta `https://localhost:<port>`, donde `<port>` es un número de puerto elegido aleatoriamente.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-438">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="7f2b0-439">Se devuelve un error HTTP 404 (No encontrado).</span><span class="sxs-lookup"><span data-stu-id="7f2b0-439">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="7f2b0-440">Anexe `/api/values` a la dirección URL (cambie la dirección URL a `https://localhost:<port>/api/values`).</span><span class="sxs-lookup"><span data-stu-id="7f2b0-440">Append `/api/values` to the URL (change the URL to `https://localhost:<port>/api/values`).</span></span>

---

<span data-ttu-id="7f2b0-441">Se devuelve el siguiente JSON:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-441">The following JSON is returned:</span></span>

```json
["value1","value2"]
```

## <a name="add-a-model-class"></a><span data-ttu-id="7f2b0-442">Incorporación de una clase de modelo</span><span class="sxs-lookup"><span data-stu-id="7f2b0-442">Add a model class</span></span>

<span data-ttu-id="7f2b0-443">Un *modelo* es un conjunto de clases que representan los datos que la aplicación administra.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-443">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="7f2b0-444">El modelo para esta aplicación es una clase `TodoItem` única.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-444">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7f2b0-445">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7f2b0-445">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="7f2b0-446">En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-446">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="7f2b0-447">Seleccione **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-447">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="7f2b0-448">Asigne a la carpeta el nombre *Models*.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-448">Name the folder *Models*.</span></span>

* <span data-ttu-id="7f2b0-449">Haga clic con el botón derecho en la carpeta *Models* y seleccione **Agregar** > **Clase**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-449">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="7f2b0-450">Asigne a la clase el nombre *TodoItem* y seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-450">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="7f2b0-451">Reemplace el código de plantilla por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-451">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7f2b0-452">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7f2b0-452">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="7f2b0-453">Agregue una carpeta denominada *Models*.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-453">Add a folder named *Models*.</span></span>

* <span data-ttu-id="7f2b0-454">Agregue una clase `TodoItem` a la carpeta *Models* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-454">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="7f2b0-455">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="7f2b0-455">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="7f2b0-456">Haga clic con el botón derecho en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-456">Right-click the project.</span></span> <span data-ttu-id="7f2b0-457">Seleccione **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-457">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="7f2b0-458">Asigne a la carpeta el nombre *Models*.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-458">Name the folder *Models*.</span></span>

  ![nueva carpeta](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="7f2b0-460">Haga clic con el botón derecho en la carpeta *Modelos* y seleccione **Agregar** > **Nuevo archivo** > **General** > **Clase vacía**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-460">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="7f2b0-461">Asigne a la clase el nombre *TodoItem* y haga clic en **Nuevo**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-461">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="7f2b0-462">Reemplace el código de plantilla por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-462">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="7f2b0-463">La propiedad `Id` funciona como clave única en una base de datos relacional.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-463">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="7f2b0-464">Las clases de modelo pueden ir en cualquier lugar del proyecto, pero convencionalmente e usa la carpeta *Models*.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-464">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="7f2b0-465">Incorporación de un contexto de base de datos</span><span class="sxs-lookup"><span data-stu-id="7f2b0-465">Add a database context</span></span>

<span data-ttu-id="7f2b0-466">El *contexto de base de datos* es la clase principal que coordina la funcionalidad de Entity Framework para un modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-466">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="7f2b0-467">Esta clase se crea derivándola de la clase `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-467">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7f2b0-468">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7f2b0-468">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="7f2b0-469">Haga clic con el botón derecho en la carpeta *Models* y seleccione **Agregar** > **Clase**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-469">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="7f2b0-470">Asigne a la clase el nombre *TodoContext* y haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-470">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="7f2b0-471">Visual Studio Code/Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="7f2b0-471">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="7f2b0-472">Agregue una clase `TodoContext` a la carpeta *Models*.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-472">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="7f2b0-473">Reemplace el código de plantilla por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-473">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="7f2b0-474">Registro del contexto de base de datos</span><span class="sxs-lookup"><span data-stu-id="7f2b0-474">Register the database context</span></span>

<span data-ttu-id="7f2b0-475">En ASP.NET Core, los servicios (como el contexto de la base de datos) deben registrarse con el contenedor de [inserción de dependencias (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="7f2b0-475">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="7f2b0-476">El contenedor proporciona el servicio a los controladores.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-476">The container provides the service to controllers.</span></span>

<span data-ttu-id="7f2b0-477">Actualice *Startup.cs* con el siguiente código resaltado:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-477">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup1.cs?highlight=5,8,25-26&name=snippet_all)]

<span data-ttu-id="7f2b0-478">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-478">The preceding code:</span></span>

* <span data-ttu-id="7f2b0-479">Elimina las declaraciones `using` no utilizadas.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-479">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="7f2b0-480">Agrega el contexto de base de datos para el contenedor de DI.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-480">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="7f2b0-481">Especifica que el contexto de base de datos usará una base de datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-481">Specifies that the database context will use an in-memory database.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="7f2b0-482">Incorporación de un controlador</span><span class="sxs-lookup"><span data-stu-id="7f2b0-482">Add a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7f2b0-483">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7f2b0-483">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="7f2b0-484">Haga clic con el botón derecho en la carpeta *Controllers*.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-484">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="7f2b0-485">Seleccione **Agregar** > **Nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-485">Select **Add** > **New Item**.</span></span>
* <span data-ttu-id="7f2b0-486">En el cuadro de diálogo **Agregar nuevo elemento**, seleccione la plantilla **Clase de controlador de API**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-486">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span>
* <span data-ttu-id="7f2b0-487">Asigne a la clase el nombre *TodoController* y seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-487">Name the class *TodoController*, and select **Add**.</span></span>

  ![Cuadro de diálogo Agregar nuevo elemento con la palabra "controller" en el cuadro de búsqueda y la opción Clase de controlador de API web seleccionada](first-web-api/_static/new_controller.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="7f2b0-489">Visual Studio Code/Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="7f2b0-489">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="7f2b0-490">En la carpeta *Controllers*, cree una clase denominada `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-490">In the *Controllers* folder, create a class named `TodoController`.</span></span>

---

* <span data-ttu-id="7f2b0-491">Reemplace el código de plantilla por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-491">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="7f2b0-492">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-492">The preceding code:</span></span>

* <span data-ttu-id="7f2b0-493">Define una clase de controlador de API sin métodos.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-493">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="7f2b0-494">Marca la clase con el atributo [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute).</span><span class="sxs-lookup"><span data-stu-id="7f2b0-494">Marks the class with the [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="7f2b0-495">Este atributo indica que el controlador responde a las solicitudes de la API web.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-495">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="7f2b0-496">Para información sobre comportamientos específicos que permite el atributo, consulte <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-496">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="7f2b0-497">Utiliza la inserción de dependencias para insertar el contexto de base de datos (`TodoContext`) en el controlador.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-497">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="7f2b0-498">El contexto de base de datos se usa en cada uno de los métodos [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) del controlador.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-498">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="7f2b0-499">Si la base de datos está vacía, le agrega un elemento denominado `Item1`.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-499">Adds an item named `Item1` to the database if the database is empty.</span></span> <span data-ttu-id="7f2b0-500">Este código está en el constructor, de manera que se ejecuta cada vez que hay una nueva solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-500">This code is in the constructor, so it runs every time there's a new HTTP request.</span></span> <span data-ttu-id="7f2b0-501">Si elimina todos los elementos, el constructor volverá a crear `Item1` la próxima vez que se llame a un método de API.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-501">If you delete all items, the constructor creates `Item1` again the next time an API method is called.</span></span> <span data-ttu-id="7f2b0-502">De este modo, es posible que parezca que la eliminación no ha funcionado, cuando en realidad sí lo ha hecho.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-502">So it may look like the deletion didn't work when it actually did work.</span></span>

## <a name="add-get-methods"></a><span data-ttu-id="7f2b0-503">Incorporación de métodos Get</span><span class="sxs-lookup"><span data-stu-id="7f2b0-503">Add Get methods</span></span>

<span data-ttu-id="7f2b0-504">Para proporcionar una API que recupere tareas pendientes, agregue estos métodos a la clase `TodoController`:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-504">To provide an API that retrieves to-do items, add the following methods to the `TodoController` class:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="7f2b0-505">Estos métodos implementan dos puntos de conexión GET:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-505">These methods implement two GET endpoints:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="7f2b0-506">Detenga la aplicación si todavía está en ejecución.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-506">Stop the app if it's still running.</span></span> <span data-ttu-id="7f2b0-507">Luego, ejecútela de nuevo para incluir los últimos cambios.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-507">Then run it again to include the latest changes.</span></span>

<span data-ttu-id="7f2b0-508">Llame a los dos puntos de conexión desde un explorador para probar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-508">Test the app by calling the two endpoints from a browser.</span></span> <span data-ttu-id="7f2b0-509">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-509">For example:</span></span>

* `https://localhost:<port>/api/todo`
* `https://localhost:<port>/api/todo/1`

<span data-ttu-id="7f2b0-510">La llamada a `GetTodoItems` genera la siguiente respuesta HTTP:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-510">The following HTTP response is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

## <a name="routing-and-url-paths"></a><span data-ttu-id="7f2b0-511">Enrutamiento y rutas URL</span><span class="sxs-lookup"><span data-stu-id="7f2b0-511">Routing and URL paths</span></span>

<span data-ttu-id="7f2b0-512">El atributo [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) indica un método que responde a una solicitud HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-512">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="7f2b0-513">La ruta de dirección URL para cada método se construye como sigue:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-513">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="7f2b0-514">Comience por la cadena de plantilla en el atributo `Route` del controlador:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-514">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="7f2b0-515">Reemplace `[controller]` por el nombre del controlador, que convencionalmente es el nombre de clase de controlador sin el sufijo "Controller".</span><span class="sxs-lookup"><span data-stu-id="7f2b0-515">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="7f2b0-516">En este ejemplo, el nombre de clase de controlador es **Todo**Controller; por tanto, el nombre del controlador es "todo".</span><span class="sxs-lookup"><span data-stu-id="7f2b0-516">For this sample, the controller class name is **Todo**Controller, so the controller name is "todo".</span></span> <span data-ttu-id="7f2b0-517">El [enrutamiento](xref:mvc/controllers/routing) en ASP.NET Core no distingue entre mayúsculas y minúsculas.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-517">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="7f2b0-518">Si el atributo `[HttpGet]` tiene una plantilla de ruta (por ejemplo, `[HttpGet("products")]`), anéxela a la ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-518">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="7f2b0-519">En este ejemplo no se usa una plantilla.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-519">This sample doesn't use a template.</span></span> <span data-ttu-id="7f2b0-520">Para más información, vea [Enrutamiento mediante atributos con atributos Http[Verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="7f2b0-520">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="7f2b0-521">En el siguiente método `GetTodoItem`, `"{id}"` es una variable de marcador de posición correspondiente al identificador único de la tarea pendiente.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-521">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="7f2b0-522">Al invocar a `GetTodoItem`, el valor `"{id}"` de la dirección URL se proporciona al método en su parámetro `id`.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-522">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="7f2b0-523">Valores devueltos</span><span class="sxs-lookup"><span data-stu-id="7f2b0-523">Return values</span></span>

<span data-ttu-id="7f2b0-524">El tipo de valor devuelto de los métodos `GetTodoItems` y `GetTodoItem` es [ActionResult\<T > type](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="7f2b0-524">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="7f2b0-525">ASP.NET Core serializa automáticamente el objeto a [JSON](https://www.json.org/) y escribe el JSON en el cuerpo del mensaje de respuesta.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-525">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="7f2b0-526">El código de respuesta para este tipo de valor devuelto es el 200, suponiendo que no haya ninguna excepción no controlada.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-526">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="7f2b0-527">Las excepciones no controladas se convierten en errores 5xx.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-527">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="7f2b0-528">Los tipos de valores devueltos `ActionResult` pueden representar una gama amplia de códigos de estado HTTP.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-528">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="7f2b0-529">Por ejemplo, `GetTodoItem` puede devolver dos valores de estado diferentes:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-529">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="7f2b0-530">Si no hay ningún elemento que coincida con el identificador solicitado, el método devolverá un código de error 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound).</span><span class="sxs-lookup"><span data-stu-id="7f2b0-530">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="7f2b0-531">En caso contrario, el método devuelve 200 con un cuerpo de respuesta JSON.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-531">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="7f2b0-532">Devolver `item` genera una respuesta HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-532">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="test-the-gettodoitems-method"></a><span data-ttu-id="7f2b0-533">Prueba del método GetTodoItems</span><span class="sxs-lookup"><span data-stu-id="7f2b0-533">Test the GetTodoItems method</span></span>

<span data-ttu-id="7f2b0-534">En este tutorial se usa Postman para probar la API web.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-534">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="7f2b0-535">Instale [Postman](https://www.getpostman.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="7f2b0-535">Install [Postman](https://www.getpostman.com/downloads/).</span></span>
* <span data-ttu-id="7f2b0-536">Inicie la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-536">Start the web app.</span></span>
* <span data-ttu-id="7f2b0-537">Inicie Postman.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-537">Start Postman.</span></span>
* <span data-ttu-id="7f2b0-538">Deshabilite **Comprobación del certificado SSL**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-538">Disable **SSL certificate verification**.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7f2b0-539">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7f2b0-539">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="7f2b0-540">En **Archivo** > **Configuración** (pestaña **General**), deshabilite **Comprobación del certificado SSL**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-540">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="7f2b0-541">Visual Studio Code/Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="7f2b0-541">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="7f2b0-542">En **Postman** > **Preferencias** (pestaña **General**), deshabilite **Comprobación del certificado SSL**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-542">From **Postman** > **Preferences** (**General** tab), disable **SSL certificate verification**.</span></span> <span data-ttu-id="7f2b0-543">También puede seleccionar la llave inglesa, hacer clic en **Configuración** y deshabilitar la comprobación del certificado SSL.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-543">Alternatively, select the wrench and select **Settings**, then disable the SSL certificate verification.</span></span>

---
  
> [!WARNING]
> <span data-ttu-id="7f2b0-544">Vuelva a habilitar la comprobación del certificado SSL tras probar el controlador.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-544">Re-enable SSL certificate verification after testing the controller.</span></span>

* <span data-ttu-id="7f2b0-545">Cree una nueva solicitud.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-545">Create a new request.</span></span>
  * <span data-ttu-id="7f2b0-546">Establezca el método HTTP en **GET**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-546">Set the HTTP method to **GET**.</span></span>
  * <span data-ttu-id="7f2b0-547">Establezca la dirección URL de la solicitud en `https://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-547">Set the request URL to `https://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="7f2b0-548">Por ejemplo: `https://localhost:5001/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-548">For example, `https://localhost:5001/api/todo`.</span></span>
* <span data-ttu-id="7f2b0-549">Establezca **Vista de dos paneles** en Postman.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-549">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="7f2b0-550">Seleccione **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-550">Select **Send**.</span></span>

![Postman con solicitud Get](first-web-api/_static/2pv.png)

## <a name="add-a-create-method"></a><span data-ttu-id="7f2b0-552">Incorporación de un método Create</span><span class="sxs-lookup"><span data-stu-id="7f2b0-552">Add a Create method</span></span>

<span data-ttu-id="7f2b0-553">Agregue el siguiente método `PostTodoItem` en *Controllers/TodoController.cs*:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-553">Add the following `PostTodoItem` method inside of *Controllers/TodoController.cs*:</span></span> 

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="7f2b0-554">El código anterior es un método HTTP POST, indicado por el atributo [`[HttpPost]`](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="7f2b0-554">The preceding code is an HTTP POST method, as indicated by the [`[HttpPost]`](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="7f2b0-555">El método obtiene el valor de tareas pendientes del cuerpo de la solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-555">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="7f2b0-556">El método `CreatedAtAction` realiza las acciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-556">The `CreatedAtAction` method:</span></span>

* <span data-ttu-id="7f2b0-557">Devuelve un código de estado HTTP 201 cuando se ha ejecutado correctamente.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-557">Returns an HTTP 201 status code, if successful.</span></span> <span data-ttu-id="7f2b0-558">HTTP 201 es la respuesta estándar para un método HTTP POST que crea un recurso en el servidor.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-558">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="7f2b0-559">Agrega un encabezado `Location` a la respuesta.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-559">Adds a `Location` header to the response.</span></span> <span data-ttu-id="7f2b0-560">El encabezado `Location` especifica el identificador URI de la tarea pendiente recién creada.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-560">The `Location` header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="7f2b0-561">Para obtener más información, consulte [10.2.2 201 creado](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="7f2b0-561">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="7f2b0-562">Hace referencia a la acción `GetTodoItem` para crear el identificador URI del encabezado `Location`.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-562">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="7f2b0-563">La palabra clave `nameof` de C# se usa para evitar que se codifique de forma rígida el nombre de acción en la llamada a `CreatedAtAction`.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-563">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method"></a><span data-ttu-id="7f2b0-564">Prueba del método PostTodoItem</span><span class="sxs-lookup"><span data-stu-id="7f2b0-564">Test the PostTodoItem method</span></span>

* <span data-ttu-id="7f2b0-565">Compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-565">Build the project.</span></span>
* <span data-ttu-id="7f2b0-566">En Postman, establezca el método HTTP en `POST`.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-566">In Postman, set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="7f2b0-567">Seleccione la pestaña **Cuerpo**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-567">Select the **Body** tab.</span></span>
* <span data-ttu-id="7f2b0-568">Seleccione el botón de radio **Raw** (Sin formato).</span><span class="sxs-lookup"><span data-stu-id="7f2b0-568">Select the **raw** radio button.</span></span>
* <span data-ttu-id="7f2b0-569">Establezca el tipo en **JSON (application/json)** .</span><span class="sxs-lookup"><span data-stu-id="7f2b0-569">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="7f2b0-570">En el cuerpo de la solicitud, introduzca JSON para una tarea pendiente:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-570">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="7f2b0-571">Seleccione **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-571">Select **Send**.</span></span>

  ![Postman con solicitud de creación](first-web-api/_static/create.png)

  <span data-ttu-id="7f2b0-573">Si recibe un error 405 (Método no permitido), probablemente sea el resultado de no haber compilado el proyecto después de agregar el método `PostTodoItem`.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-573">If you get a 405 Method Not Allowed error, it's probably the result of not compiling the project after adding the `PostTodoItem` method.</span></span>

### <a name="test-the-location-header-uri"></a><span data-ttu-id="7f2b0-574">Prueba del URI del encabezado de ubicación</span><span class="sxs-lookup"><span data-stu-id="7f2b0-574">Test the location header URI</span></span>

* <span data-ttu-id="7f2b0-575">Seleccione la pestaña **Encabezados** en el panel **Respuesta**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-575">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="7f2b0-576">Copie el valor de encabezado **Ubicación**:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-576">Copy the **Location** header value:</span></span>

  ![Pestaña Encabezados de la consola de Postman](first-web-api/_static/pmc2.png)

* <span data-ttu-id="7f2b0-578">Establezca el método en GET.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-578">Set the method to GET.</span></span>
* <span data-ttu-id="7f2b0-579">Pegue el URI (por ejemplo, `https://localhost:5001/api/Todo/2`).</span><span class="sxs-lookup"><span data-stu-id="7f2b0-579">Paste the URI (for example, `https://localhost:5001/api/Todo/2`).</span></span>
* <span data-ttu-id="7f2b0-580">Seleccione **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-580">Select **Send**.</span></span>

## <a name="add-a-puttodoitem-method"></a><span data-ttu-id="7f2b0-581">Incorporación de un método PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="7f2b0-581">Add a PutTodoItem method</span></span>

<span data-ttu-id="7f2b0-582">Agregue el siguiente método `PutTodoItem`:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-582">Add the following `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="7f2b0-583">`PutTodoItem` es similar a `PostTodoItem`, salvo por el hecho de que usa HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-583">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="7f2b0-584">La respuesta es [204 Sin contenido](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="7f2b0-584">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="7f2b0-585">Según la especificación HTTP, una solicitud PUT requiere que el cliente envíe toda la entidad actualizada, no solo los cambios.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-585">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="7f2b0-586">Para admitir actualizaciones parciales, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="7f2b0-586">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="7f2b0-587">Si recibe un error al llamar a `PutTodoItem`, llame a `GET` para asegurarse de que hay un elemento en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-587">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="7f2b0-588">Prueba del método PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="7f2b0-588">Test the PutTodoItem method</span></span>

<span data-ttu-id="7f2b0-589">En este ejemplo se usa una base de datos en memoria que se debe inicializar cada vez que se inicia la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-589">This sample uses an in-memory database that must be initialized each time the app is started.</span></span> <span data-ttu-id="7f2b0-590">Debe haber un elemento en la base de datos antes de que realice una llamada PUT.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-590">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="7f2b0-591">Llame a GET para asegurarse de que hay un elemento en la base de datos antes de realizar una llamada PUT.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-591">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="7f2b0-592">Actualice la tarea pendiente que tiene el id. = 1 y establezca su nombre en "feed fish":</span><span class="sxs-lookup"><span data-stu-id="7f2b0-592">Update the to-do item that has id = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="7f2b0-593">En la imagen siguiente, se muestra la actualización de Postman:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-593">The following image shows the Postman update:</span></span>

![Consola de Postman que muestra la respuesta 204 (Sin contenido)](first-web-api/_static/pmcput.png)

## <a name="add-a-deletetodoitem-method"></a><span data-ttu-id="7f2b0-595">Incorporación de un método DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="7f2b0-595">Add a DeleteTodoItem method</span></span>

<span data-ttu-id="7f2b0-596">Agregue el siguiente método `DeleteTodoItem`:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-596">Add the following `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="7f2b0-597">La respuesta de `DeleteTodoItem` es [204 (Sin contenido)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="7f2b0-597">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="7f2b0-598">Prueba del método DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="7f2b0-598">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="7f2b0-599">Use Postman para eliminar una tarea pendiente:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-599">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="7f2b0-600">Establezca el método en `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-600">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="7f2b0-601">Establezca el URI del objeto que quiera eliminar (por ejemplo, `https://localhost:5001/api/todo/1`).</span><span class="sxs-lookup"><span data-stu-id="7f2b0-601">Set the URI of the object to delete (for example `https://localhost:5001/api/todo/1`).</span></span>
* <span data-ttu-id="7f2b0-602">Seleccione **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-602">Select **Send**.</span></span>

<span data-ttu-id="7f2b0-603">La aplicación de ejemplo permite eliminar todos los elementos.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-603">The sample app allows you to delete all the items.</span></span> <span data-ttu-id="7f2b0-604">Sin embargo, al eliminar el último elemento, se creará uno nuevo en el constructor de clase de modelo la próxima vez que se llame a la API.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-604">However, when the last item is deleted, a new one is created by the model class constructor the next time the API is called.</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="7f2b0-605">Llamar a la API web con JavaScript</span><span class="sxs-lookup"><span data-stu-id="7f2b0-605">Call the web API with JavaScript</span></span>

<span data-ttu-id="7f2b0-606">En esta sección, se agrega una página HTML que usa JavaScript para llamar a la API web.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-606">In this section, an HTML page is added that uses JavaScript to call the web API.</span></span> <span data-ttu-id="7f2b0-607">JQuery inicia la solicitud.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-607">jQuery initiates the request.</span></span> <span data-ttu-id="7f2b0-608">JavaScript actualiza la página con los detalles de la respuesta de la API web.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-608">JavaScript updates the page with the details from the web API's response.</span></span>

<span data-ttu-id="7f2b0-609">Configure la aplicación para [atender archivos estáticos](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) y [habilitar la asignación de archivos predeterminada](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) mediante la actualización de *Startup.cs* con el siguiente código resaltado:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-609">Configure the app to [serve static files](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [enable default file mapping](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) by updating *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

<span data-ttu-id="7f2b0-610">Cree una carpeta *wwwroot* en el directorio del proyecto.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-610">Create a *wwwroot* folder in the project directory.</span></span>

<span data-ttu-id="7f2b0-611">Agregue un archivo HTML denominado *index.html* al directorio *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-611">Add an HTML file named *index.html* to the *wwwroot* directory.</span></span> <span data-ttu-id="7f2b0-612">Reemplace el contenido por el siguiente marcado:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-612">Replace its contents with the following markup:</span></span>

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

<span data-ttu-id="7f2b0-613">Agregue un archivo JavaScript denominado *site.js* al directorio *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-613">Add a JavaScript file named *site.js* to the *wwwroot* directory.</span></span> <span data-ttu-id="7f2b0-614">Reemplace el contenido por el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-614">Replace its contents with the following code:</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

<span data-ttu-id="7f2b0-615">Puede que sea necesario realizar un cambio en la configuración de inicio del proyecto de ASP.NET Core para probar la página HTML localmente:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-615">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

* <span data-ttu-id="7f2b0-616">Abra *Properties\launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-616">Open *Properties\launchSettings.json*.</span></span>
* <span data-ttu-id="7f2b0-617">Quite la propiedad `launchUrl` para forzar a la aplicación a abrirse en *index.html*, esto es, el archivo predeterminado del proyecto.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-617">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="7f2b0-618">En este ejemplo se llama a todos los métodos CRUD de la API web.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-618">This sample calls all of the CRUD methods of the web API.</span></span> <span data-ttu-id="7f2b0-619">A continuación, encontrará algunas explicaciones de las llamadas a la API.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-619">Following are explanations of the calls to the API.</span></span>

### <a name="get-a-list-of-to-do-items"></a><span data-ttu-id="7f2b0-620">Obtención de una lista de tareas pendientes</span><span class="sxs-lookup"><span data-stu-id="7f2b0-620">Get a list of to-do items</span></span>

<span data-ttu-id="7f2b0-621">jQuery envía una solicitud HTTP GET a la API web, que devuelve código JSON que representa una matriz de tareas pendientes.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-621">jQuery sends an HTTP GET request to the web API, which returns JSON representing an array of to-do items.</span></span> <span data-ttu-id="7f2b0-622">La función de devolución de llamada `success` se invoca si la solicitud se realiza correctamente.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-622">The `success` callback function is invoked if the request succeeds.</span></span> <span data-ttu-id="7f2b0-623">En la devolución de llamada, el DOM se actualiza con la información de la tarea pendiente.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-623">In the callback, the DOM is updated with the to-do information.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a><span data-ttu-id="7f2b0-624">Incorporación de una tarea pendiente</span><span class="sxs-lookup"><span data-stu-id="7f2b0-624">Add a to-do item</span></span>

<span data-ttu-id="7f2b0-625">jQuery envía una solicitud HTTP POST con la tarea pendiente en el cuerpo de dicha solicitud.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-625">jQuery sends an HTTP POST request with the to-do item in the request body.</span></span> <span data-ttu-id="7f2b0-626">Las opciones `accepts` y `contentType` se establecen en `application/json` para especificar el tipo de medio que se va a recibir y a enviar.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-626">The `accepts` and `contentType` options are set to `application/json` to specify the media type being received and sent.</span></span> <span data-ttu-id="7f2b0-627">La tarea pendiente se convierte en JSON mediante [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span><span class="sxs-lookup"><span data-stu-id="7f2b0-627">The to-do item is converted to JSON by using [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span></span> <span data-ttu-id="7f2b0-628">Cuando la API devuelve un código de estado correcto, se invoca la función `getData` para actualizar la tabla HTML.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-628">When the API returns a successful status code, the `getData` function is invoked to update the HTML table.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a><span data-ttu-id="7f2b0-629">Actualizar una tarea pendiente</span><span class="sxs-lookup"><span data-stu-id="7f2b0-629">Update a to-do item</span></span>

<span data-ttu-id="7f2b0-630">El hecho de actualizar una tarea pendiente es similar al de agregar una.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-630">Updating a to-do item is similar to adding one.</span></span> <span data-ttu-id="7f2b0-631">El valor `url` cambia para agregar el identificador único del elemento, y `type` es `PUT`.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-631">The `url` changes to add the unique identifier of the item, and the `type` is `PUT`.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a><span data-ttu-id="7f2b0-632">Eliminar una tarea pendiente</span><span class="sxs-lookup"><span data-stu-id="7f2b0-632">Delete a to-do item</span></span>

<span data-ttu-id="7f2b0-633">Para eliminar una tarea pendiente, hay que establecer el valor `type` de la llamada de AJAX en `DELETE` y especificar el identificador único de la tarea en la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="7f2b0-633">Deleting a to-do item is accomplished by setting the `type` on the AJAX call to `DELETE` and specifying the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

::: moniker-end

<a name="auth"></a>

## <a name="add-authentication-support-to-a-web-api"></a><span data-ttu-id="7f2b0-634">Agregar compatibilidad con la autenticación a una API web</span><span class="sxs-lookup"><span data-stu-id="7f2b0-634">Add authentication support to a web API</span></span>

[!INCLUDE[](~/includes/IdentityServer4.md)]

## <a name="additional-resources"></a><span data-ttu-id="7f2b0-635">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="7f2b0-635">Additional resources</span></span>

<span data-ttu-id="7f2b0-636">[Vea o descargue el código de ejemplo para este tutorial](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span><span class="sxs-lookup"><span data-stu-id="7f2b0-636">[View or download sample code for this tutorial](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span></span> <span data-ttu-id="7f2b0-637">Vea [cómo descargarlo](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="7f2b0-637">See [how to download](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="7f2b0-638">Para obtener más información, vea los siguientes recursos:</span><span class="sxs-lookup"><span data-stu-id="7f2b0-638">For more information, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/intro>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>
* [<span data-ttu-id="7f2b0-639">Versión en YouTube de este tutorial</span><span class="sxs-lookup"><span data-stu-id="7f2b0-639">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=TTkhEyGBfAk)
