---
title: Crear una API web con ASP.NET Core y Visual Studio para Windows
author: rick-anderson
description: Crear una API web con ASP.NET Core MVC y Visual Studio para Windows
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/08/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-web-api
ms.openlocfilehash: cb46f8b4013488dbe2bb5ca3d08a8c6e452141dd
ms.sourcegitcommit: c867d7427bd4a88a78b2322e156367733b532730
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/09/2018
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a><span data-ttu-id="8b555-103">Crear una API web con ASP.NET Core y Visual Studio para Windows</span><span class="sxs-lookup"><span data-stu-id="8b555-103">Create a Web API with ASP.NET Core and Visual Studio for Windows</span></span>

<span data-ttu-id="8b555-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="8b555-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="8b555-105">En este tutorial se compila una API web para administrar una lista de tareas pendientes.</span><span class="sxs-lookup"><span data-stu-id="8b555-105">This tutorial builds a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="8b555-106">No se crea ninguna interfaz de usuario (UI).</span><span class="sxs-lookup"><span data-stu-id="8b555-106">A user interface (UI) isn't created.</span></span>

<span data-ttu-id="8b555-107">Hay tres versiones de este tutorial:</span><span class="sxs-lookup"><span data-stu-id="8b555-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="8b555-108">Windows: API web con Visual Studio para Windows (este tutorial)</span><span class="sxs-lookup"><span data-stu-id="8b555-108">Windows: Web API with Visual Studio for Windows (This tutorial)</span></span>
* <span data-ttu-id="8b555-109">macOS: [API web con Visual Studio para Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="8b555-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="8b555-110">macOS, Linux y Windows: [API web con Visual Studio Code](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="8b555-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="8b555-111">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="8b555-111">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="create-the-project"></a><span data-ttu-id="8b555-112">Crear el proyecto</span><span class="sxs-lookup"><span data-stu-id="8b555-112">Create the project</span></span>

<span data-ttu-id="8b555-113">Haga lo siguiente para descargar Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="8b555-113">Follow these steps in Visual Studio:</span></span>

* <span data-ttu-id="8b555-114">En el menú **Archivo**, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="8b555-114">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="8b555-115">Seleccione la plantilla **Aplicación web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="8b555-115">Select the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="8b555-116">Denomine el proyecto *TodoApi* y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="8b555-116">Name the project *TodoApi* and click **OK**.</span></span>
* <span data-ttu-id="8b555-117">En el cuadro de diálogo **Nueva aplicación web ASP.NET Core - TodoApi**, seleccione la versión ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8b555-117">In the **New ASP.NET Core Web Application - TodoApi** dialog, choose the ASP.NET Core version.</span></span> <span data-ttu-id="8b555-118">Seleccione la plantilla **API** y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="8b555-118">Select the **API** template and click **OK**.</span></span> <span data-ttu-id="8b555-119">**No** seleccione **Habilitar compatibilidad con Docker**.</span><span class="sxs-lookup"><span data-stu-id="8b555-119">Do **not** select **Enable Docker Support**.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="8b555-120">Iniciar la aplicación</span><span class="sxs-lookup"><span data-stu-id="8b555-120">Launch the app</span></span>

<span data-ttu-id="8b555-121">En Visual Studio, presione CTRL+F5 para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8b555-121">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="8b555-122">Visual Studio inicia un explorador y navega hasta `http://localhost:<port>/api/values`, donde `<port>` es un número de puerto elegido aleatoriamente.</span><span class="sxs-lookup"><span data-stu-id="8b555-122">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="8b555-123">En Chrome, Microsoft Edge y Firefox se muestra la salida siguiente:</span><span class="sxs-lookup"><span data-stu-id="8b555-123">Chrome, Microsoft Edge, and Firefox display the following output:</span></span>

```json
["value1","value2"]
```

### <a name="add-a-model-class"></a><span data-ttu-id="8b555-124">Agregar una clase de modelo</span><span class="sxs-lookup"><span data-stu-id="8b555-124">Add a model class</span></span>

<span data-ttu-id="8b555-125">Un modelo es un objeto que representa los datos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8b555-125">A model is an object representing the data in the app.</span></span> <span data-ttu-id="8b555-126">En este caso, el único modelo es una tarea pendiente.</span><span class="sxs-lookup"><span data-stu-id="8b555-126">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="8b555-127">En el Explorador de soluciones, haga clic con el botón derecho en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="8b555-127">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="8b555-128">Seleccione **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="8b555-128">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="8b555-129">Asigne a la carpeta el nombre *Models*.</span><span class="sxs-lookup"><span data-stu-id="8b555-129">Name the folder *Models*.</span></span>

> [!NOTE]
> <span data-ttu-id="8b555-130">Las clases del modelo pueden ir en cualquier parte del proyecto.</span><span class="sxs-lookup"><span data-stu-id="8b555-130">The model classes can go anywhere in the project.</span></span> <span data-ttu-id="8b555-131">La carpeta *Models* se usa por convención para las clases de modelos.</span><span class="sxs-lookup"><span data-stu-id="8b555-131">The *Models* folder is used by convention for model classes.</span></span>

<span data-ttu-id="8b555-132">En el Explorador de soluciones, haga clic con el botón derecho en la carpeta de *modelos* y seleccione **Agregar** > **Clase**.</span><span class="sxs-lookup"><span data-stu-id="8b555-132">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="8b555-133">Denomine la clase *TodoItem* y, después, haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="8b555-133">Name the class *TodoItem* and click **Add**.</span></span>

<span data-ttu-id="8b555-134">Actualice la clase `TodoItem` por el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="8b555-134">Update the `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="8b555-135">La base de datos genera el `Id` cuando se crea `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="8b555-135">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="8b555-136">Crear el contexto de base de datos</span><span class="sxs-lookup"><span data-stu-id="8b555-136">Create the database context</span></span>

<span data-ttu-id="8b555-137">El *contexto de base de datos* es la clase principal que coordina la funcionalidad de Entity Framework para un modelo de datos determinado.</span><span class="sxs-lookup"><span data-stu-id="8b555-137">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="8b555-138">Esta clase se crea derivándola de la clase `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="8b555-138">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="8b555-139">En el Explorador de soluciones, haga clic con el botón derecho en la carpeta de *modelos* y seleccione **Agregar** > **Clase**.</span><span class="sxs-lookup"><span data-stu-id="8b555-139">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="8b555-140">Denomine la clase *TodoContext* y, después, haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="8b555-140">Name the class *TodoContext* and click **Add**.</span></span>

<span data-ttu-id="8b555-141">Reemplace la clase por el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="8b555-141">Replace the class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a><span data-ttu-id="8b555-142">Adición de un controlador</span><span class="sxs-lookup"><span data-stu-id="8b555-142">Add a controller</span></span>

<span data-ttu-id="8b555-143">En el Explorador de soluciones, haga clic con el botón derecho en la carpeta *Controladores*.</span><span class="sxs-lookup"><span data-stu-id="8b555-143">In Solution Explorer, right-click the *Controllers* folder.</span></span> <span data-ttu-id="8b555-144">Seleccione **Agregar** > **Nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="8b555-144">Select **Add** > **New Item**.</span></span> <span data-ttu-id="8b555-145">En el cuadro de diálogo **Agregar nuevo elemento**, seleccione la plantilla **Clase de controlador de API**.</span><span class="sxs-lookup"><span data-stu-id="8b555-145">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span> <span data-ttu-id="8b555-146">Denomine la clase *TodoController* y, después, haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="8b555-146">Name the class *TodoController*, and click **Add**.</span></span>

![Cuadro de diálogo Agregar nuevo elemento con la palabra "controlador" en el cuadro de búsqueda y la opción Clase de controlador de API web seleccionada](first-web-api/_static/new_controller.png)

<span data-ttu-id="8b555-148">Reemplace la clase por el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="8b555-148">Replace the class with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="8b555-149">Iniciar la aplicación</span><span class="sxs-lookup"><span data-stu-id="8b555-149">Launch the app</span></span>

<span data-ttu-id="8b555-150">En Visual Studio, presione CTRL+F5 para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8b555-150">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="8b555-151">Visual Studio inicia un explorador y navega hasta `http://localhost:<port>/api/values`, donde `<port>` es un número de puerto elegido aleatoriamente.</span><span class="sxs-lookup"><span data-stu-id="8b555-151">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="8b555-152">Vaya al controlador `Todo` en `http://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="8b555-152">Navigate to the `Todo` controller at `http://localhost:<port>/api/todo`.</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
