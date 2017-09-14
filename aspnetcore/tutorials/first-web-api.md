---
title: Crear una API web con ASP.NET Core y Visual Studio para Windows
author: rick-anderson
description: Crear una API web con ASP.NET Core MVC y Visual Studio para Windows
keywords: ASP.NET Core, WebAPI, API web, REST, HTTP, servicio, servicio HTTP
ms.author: riande
manager: wpickett
ms.date: 8/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-web-api
ms.openlocfilehash: c57c73c6f9c60874ef88749b838ed1cc1d353ead
ms.sourcegitcommit: 7fef13045e98d716c589a2982613dad261694a65
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2017
---
#<a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a><span data-ttu-id="4600e-104">Crear una API web con ASP.NET Core y Visual Studio para Windows</span><span class="sxs-lookup"><span data-stu-id="4600e-104">Create a web API with ASP.NET Core and Visual Studio for Windows</span></span>

<span data-ttu-id="4600e-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="4600e-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="4600e-106">En este tutorial compilará una API web para administrar una lista de tareas pendientes</span><span class="sxs-lookup"><span data-stu-id="4600e-106">In this tutorial, you’ll build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="4600e-107">(no creará ninguna interfaz de usuario).</span><span class="sxs-lookup"><span data-stu-id="4600e-107">You won’t build a UI.</span></span>

<span data-ttu-id="4600e-108">Hay tres versiones de este tutorial:</span><span class="sxs-lookup"><span data-stu-id="4600e-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="4600e-109">Windows: API web con Visual Studio para Windows (este tutorial)</span><span class="sxs-lookup"><span data-stu-id="4600e-109">Windows: Web API with Visual Studio for Windows (This tutorial)</span></span>
* <span data-ttu-id="4600e-110">macOS: [API web con Visual Studio para Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="4600e-110">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="4600e-111">macOS, Linux y Windows: [API web con Visual Studio Code](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="4600e-111">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="4600e-112">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="4600e-112">Prerequisites</span></span>

[!INCLUDE[install 2.0](../includes/install2.0.md)]

<span data-ttu-id="4600e-113">Vea [este PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/_static/_webAPI.pdf) para la versión 1.1 de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4600e-113">See [this PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/_static/_webAPI.pdf) for the ASP.NET Core 1.1 version.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="4600e-114">Crear el proyecto</span><span class="sxs-lookup"><span data-stu-id="4600e-114">Create the project</span></span>

<span data-ttu-id="4600e-115">En Visual Studio, seleccione el menú **Archivo**, > **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="4600e-115">From Visual Studio, select **File** menu, > **New** > **Project**.</span></span>

<span data-ttu-id="4600e-116">Seleccione la plantilla de proyecto **Aplicación web ASP.NET Core (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="4600e-116">Select the **ASP.NET Core Web Application (.NET Core)** project template.</span></span> <span data-ttu-id="4600e-117">Asigne al proyecto el nombre `TodoApi` y seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="4600e-117">Name the project `TodoApi` and select **OK**.</span></span>

![Cuadro de diálogo Nuevo proyecto](first-web-api/_static/new-project.png)

<span data-ttu-id="4600e-119">En el cuadro de diálogo **Nueva aplicación web ASP.NET Core - TodoApi**, seleccione la plantilla **API web**.</span><span class="sxs-lookup"><span data-stu-id="4600e-119">In the **New ASP.NET Core Web Application - TodoApi** dialog, select the **Web API** template.</span></span> <span data-ttu-id="4600e-120">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="4600e-120">Select **OK**.</span></span> <span data-ttu-id="4600e-121">**No** seleccione **Habilitar compatibilidad con Docker**.</span><span class="sxs-lookup"><span data-stu-id="4600e-121">Do **not** select **Enable Docker Support**.</span></span>

![Cuadro de diálogo Nueva aplicación web ASP.NET con la plantilla de proyecto API web seleccionada en las plantillas de ASP.NET Core](first-web-api/_static/web-api-project.png)

### <a name="launch-the-app"></a><span data-ttu-id="4600e-123">Iniciar la aplicación</span><span class="sxs-lookup"><span data-stu-id="4600e-123">Launch the app</span></span>

<span data-ttu-id="4600e-124">En Visual Studio, presione CTRL+F5 para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4600e-124">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="4600e-125">Visual Studio inicia un explorador y navega hasta `http://localhost:port/api/values`, donde *port* es un número de puerto elegido aleatoriamente.</span><span class="sxs-lookup"><span data-stu-id="4600e-125">Visual Studio launches a browser and navigates to `http://localhost:port/api/values`, where *port* is a randomly-chosen port number.</span></span> <span data-ttu-id="4600e-126">En Chrome, Edge y Firefox se muestra lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="4600e-126">Chrome, Edge, and Firefox display the following:</span></span>

```
["value1","value2"]
``` 

### <a name="add-a-model-class"></a><span data-ttu-id="4600e-127">Agregar una clase de modelo</span><span class="sxs-lookup"><span data-stu-id="4600e-127">Add a model class</span></span>

<span data-ttu-id="4600e-128">Un modelo es un objeto que representa los datos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4600e-128">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="4600e-129">En este caso, el único modelo es una tarea pendiente.</span><span class="sxs-lookup"><span data-stu-id="4600e-129">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="4600e-130">Agregue una carpeta denominada "Modelos".</span><span class="sxs-lookup"><span data-stu-id="4600e-130">Add a folder named "Models".</span></span> <span data-ttu-id="4600e-131">En el Explorador de soluciones, haga clic con el botón derecho en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="4600e-131">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="4600e-132">Seleccione **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="4600e-132">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="4600e-133">Asigne a la carpeta el nombre *Modelos*.</span><span class="sxs-lookup"><span data-stu-id="4600e-133">Name the folder *Models*.</span></span>

<span data-ttu-id="4600e-134">Nota: Las clases de modelo pueden ir en cualquier lugar del proyecto, pero la carpeta *Modelos* se usa por convención.</span><span class="sxs-lookup"><span data-stu-id="4600e-134">Note: The model classes go anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="4600e-135">Agregue una clase `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="4600e-135">Add a `TodoItem` class.</span></span> <span data-ttu-id="4600e-136">Haga clic con el botón derecho en la carpeta *Modelos* y seleccione **Agregar** > **Clase**.</span><span class="sxs-lookup"><span data-stu-id="4600e-136">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="4600e-137">Asigne a la clase el nombre `TodoItem` y seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="4600e-137">Name the class `TodoItem` and select **Add**.</span></span>

<span data-ttu-id="4600e-138">Reemplace el código generado por el siguiente:</span><span class="sxs-lookup"><span data-stu-id="4600e-138">Replace the generated code with the following:</span></span>

<span data-ttu-id="4600e-139">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]</span><span class="sxs-lookup"><span data-stu-id="4600e-139">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]</span></span>

<span data-ttu-id="4600e-140">La base de datos genera el `Id` cuando se crea una `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="4600e-140">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="4600e-141">Crear el contexto de base de datos</span><span class="sxs-lookup"><span data-stu-id="4600e-141">Create the database context</span></span>

<span data-ttu-id="4600e-142">El *contexto de base de datos* es la clase principal que coordina la funcionalidad de Entity Framework para un modelo de datos determinado.</span><span class="sxs-lookup"><span data-stu-id="4600e-142">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="4600e-143">Esta clase se crea derivándola de la clase `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="4600e-143">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="4600e-144">Agregue una clase `TodoContext`.</span><span class="sxs-lookup"><span data-stu-id="4600e-144">Add a `TodoContext` class.</span></span> <span data-ttu-id="4600e-145">Haga clic con el botón derecho en la carpeta *Modelos* y seleccione **Agregar** > **Clase**.</span><span class="sxs-lookup"><span data-stu-id="4600e-145">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="4600e-146">Asigne a la clase el nombre `TodoContext` y seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="4600e-146">Name the class `TodoContext` and select **Add**.</span></span>

<span data-ttu-id="4600e-147">Reemplace el código generado por el siguiente:</span><span class="sxs-lookup"><span data-stu-id="4600e-147">Replace the generated code with the following:</span></span>

<span data-ttu-id="4600e-148">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]</span><span class="sxs-lookup"><span data-stu-id="4600e-148">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]</span></span>

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a><span data-ttu-id="4600e-149">Agregar un controlador</span><span class="sxs-lookup"><span data-stu-id="4600e-149">Add a controller</span></span>

<span data-ttu-id="4600e-150">En el Explorador de soluciones, haga clic con el botón derecho en la carpeta *Controladores*.</span><span class="sxs-lookup"><span data-stu-id="4600e-150">In Solution Explorer, right-click the *Controllers* folder.</span></span> <span data-ttu-id="4600e-151">Seleccione **Agregar** > **Nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="4600e-151">Select **Add** > **New Item**.</span></span> <span data-ttu-id="4600e-152">En el cuadro de diálogo **Agregar nuevo elemento**, seleccione la plantilla **Clase de controlador de API web**.</span><span class="sxs-lookup"><span data-stu-id="4600e-152">In the **Add New Item** dialog, select the **Web  API Controller Class** template.</span></span> <span data-ttu-id="4600e-153">Asigne a la clase el nombre `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="4600e-153">Name the class `TodoController`.</span></span>

![Cuadro de diálogo Agregar nuevo elemento con la palabra "controlador" en el cuadro de búsqueda y la opción Clase de controlador de API web seleccionada](first-web-api/_static/new_controller.png)

<span data-ttu-id="4600e-155">Reemplace el código generado por el siguiente:</span><span class="sxs-lookup"><span data-stu-id="4600e-155">Replace the generated code with the following:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]
  
### <a name="launch-the-app"></a><span data-ttu-id="4600e-156">Iniciar la aplicación</span><span class="sxs-lookup"><span data-stu-id="4600e-156">Launch the app</span></span>

<span data-ttu-id="4600e-157">En Visual Studio, presione CTRL+F5 para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4600e-157">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="4600e-158">Visual Studio inicia un explorador y navega hasta `http://localhost:port/api/values`, donde *port* es un número de puerto elegido aleatoriamente.</span><span class="sxs-lookup"><span data-stu-id="4600e-158">Visual Studio launches a browser and navigates to `http://localhost:port/api/values`, where *port* is a randomly chosen port number.</span></span> <span data-ttu-id="4600e-159">Si usa Chrome, Edge o Firefox, se mostrarán los datos.</span><span class="sxs-lookup"><span data-stu-id="4600e-159">If you're using Chrome, Edge or Firefox, the data will be displayed.</span></span> <span data-ttu-id="4600e-160">Si usa Internet Explorer, le pedirá que abra o guarde el archivo *values.json*.</span><span class="sxs-lookup"><span data-stu-id="4600e-160">If you're using IE, IE will prompt to you open or save the *values.json* file.</span></span> <span data-ttu-id="4600e-161">Vaya al controlador `Todo` que acaba de crear `http://localhost:port/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="4600e-161">Navigate to the `Todo` controller we just created `http://localhost:port/api/todo`.</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]

