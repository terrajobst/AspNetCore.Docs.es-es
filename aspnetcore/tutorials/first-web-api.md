---
title: Crear una API web con ASP.NET Core y Visual Studio para Windows
author: rick-anderson
description: Crear una API web con ASP.NET Core MVC y Visual Studio para Windows
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/27/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-web-api
ms.openlocfilehash: 962c24a7e654328df7e8893e589e45b19e87b931
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/03/2018
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a><span data-ttu-id="0df4f-103">Crear una API web con ASP.NET Core y Visual Studio para Windows</span><span class="sxs-lookup"><span data-stu-id="0df4f-103">Create a Web API with ASP.NET Core and Visual Studio for Windows</span></span>

<span data-ttu-id="0df4f-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="0df4f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

::: moniker range="= aspnetcore-2.1"
[!INCLUDE[](~/includes/2.1.md)]
::: moniker-end

<span data-ttu-id="0df4f-106">En este tutorial se compila una API web para administrar una lista de tareas pendientes.</span><span class="sxs-lookup"><span data-stu-id="0df4f-106">This tutorial builds a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="0df4f-107">No se crea ninguna interfaz de usuario (UI).</span><span class="sxs-lookup"><span data-stu-id="0df4f-107">A user interface (UI) isn't created.</span></span>

<span data-ttu-id="0df4f-108">Hay tres versiones de este tutorial:</span><span class="sxs-lookup"><span data-stu-id="0df4f-108">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="0df4f-109">Windows: API web con Visual Studio para Windows (este tutorial)</span><span class="sxs-lookup"><span data-stu-id="0df4f-109">Windows: Web API with Visual Studio for Windows (This tutorial)</span></span>
* <span data-ttu-id="0df4f-110">macOS: [API web con Visual Studio para Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="0df4f-110">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="0df4f-111">macOS, Linux y Windows: [API web con Visual Studio Code](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="0df4f-111">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="0df4f-112">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="0df4f-112">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="create-the-project"></a><span data-ttu-id="0df4f-113">Crear el proyecto</span><span class="sxs-lookup"><span data-stu-id="0df4f-113">Create the project</span></span>

<span data-ttu-id="0df4f-114">Haga lo siguiente para descargar Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="0df4f-114">Follow these steps in Visual Studio:</span></span>

* <span data-ttu-id="0df4f-115">En el menú **Archivo**, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="0df4f-115">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="0df4f-116">Seleccione la plantilla **Aplicación web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="0df4f-116">Select the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="0df4f-117">Denomine el proyecto *TodoApi* y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="0df4f-117">Name the project *TodoApi* and click **OK**.</span></span>
* <span data-ttu-id="0df4f-118">En el cuadro de diálogo **Nueva aplicación web ASP.NET Core - TodoApi**, seleccione la versión ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0df4f-118">In the **New ASP.NET Core Web Application - TodoApi** dialog, choose the ASP.NET Core version.</span></span> <span data-ttu-id="0df4f-119">Seleccione la plantilla **API** y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="0df4f-119">Select the **API** template and click **OK**.</span></span> <span data-ttu-id="0df4f-120">**No** seleccione **Habilitar compatibilidad con Docker**.</span><span class="sxs-lookup"><span data-stu-id="0df4f-120">Do **not** select **Enable Docker Support**.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="0df4f-121">Iniciar la aplicación</span><span class="sxs-lookup"><span data-stu-id="0df4f-121">Launch the app</span></span>

<span data-ttu-id="0df4f-122">En Visual Studio, presione CTRL+F5 para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0df4f-122">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="0df4f-123">Visual Studio inicia un explorador y navega hasta `http://localhost:<port>/api/values`, donde `<port>` es un número de puerto elegido aleatoriamente.</span><span class="sxs-lookup"><span data-stu-id="0df4f-123">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="0df4f-124">En Chrome, Microsoft Edge y Firefox se muestra la salida siguiente:</span><span class="sxs-lookup"><span data-stu-id="0df4f-124">Chrome, Microsoft Edge, and Firefox display the following output:</span></span>

```json
["value1","value2"]
```

### <a name="add-a-model-class"></a><span data-ttu-id="0df4f-125">Agregar una clase de modelo</span><span class="sxs-lookup"><span data-stu-id="0df4f-125">Add a model class</span></span>

<span data-ttu-id="0df4f-126">Un modelo es un objeto que representa los datos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0df4f-126">A model is an object representing the data in the app.</span></span> <span data-ttu-id="0df4f-127">En este caso, el único modelo es una tarea pendiente.</span><span class="sxs-lookup"><span data-stu-id="0df4f-127">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="0df4f-128">En el Explorador de soluciones, haga clic con el botón derecho en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="0df4f-128">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="0df4f-129">Seleccione **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="0df4f-129">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="0df4f-130">Asigne a la carpeta el nombre *Models*.</span><span class="sxs-lookup"><span data-stu-id="0df4f-130">Name the folder *Models*.</span></span>

> [!NOTE]
> <span data-ttu-id="0df4f-131">Las clases del modelo pueden ir en cualquier parte del proyecto.</span><span class="sxs-lookup"><span data-stu-id="0df4f-131">The model classes can go anywhere in the project.</span></span> <span data-ttu-id="0df4f-132">La carpeta *Models* se usa por convención para las clases de modelos.</span><span class="sxs-lookup"><span data-stu-id="0df4f-132">The *Models* folder is used by convention for model classes.</span></span>

<span data-ttu-id="0df4f-133">En el Explorador de soluciones, haga clic con el botón derecho en la carpeta de *modelos* y seleccione **Agregar** > **Clase**.</span><span class="sxs-lookup"><span data-stu-id="0df4f-133">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="0df4f-134">Denomine la clase *TodoItem* y, después, haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="0df4f-134">Name the class *TodoItem* and click **Add**.</span></span>

<span data-ttu-id="0df4f-135">Actualice la clase `TodoItem` por el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="0df4f-135">Update the `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="0df4f-136">La base de datos genera el `Id` cuando se crea `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="0df4f-136">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="0df4f-137">Crear el contexto de base de datos</span><span class="sxs-lookup"><span data-stu-id="0df4f-137">Create the database context</span></span>

<span data-ttu-id="0df4f-138">El *contexto de base de datos* es la clase principal que coordina la funcionalidad de Entity Framework para un modelo de datos determinado.</span><span class="sxs-lookup"><span data-stu-id="0df4f-138">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="0df4f-139">Esta clase se crea derivándola de la clase `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="0df4f-139">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="0df4f-140">En el Explorador de soluciones, haga clic con el botón derecho en la carpeta de *modelos* y seleccione **Agregar** > **Clase**.</span><span class="sxs-lookup"><span data-stu-id="0df4f-140">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="0df4f-141">Denomine la clase *TodoContext* y, después, haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="0df4f-141">Name the class *TodoContext* and click **Add**.</span></span>

<span data-ttu-id="0df4f-142">Reemplace la clase por el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="0df4f-142">Replace the class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE [Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a><span data-ttu-id="0df4f-143">Adición de un controlador</span><span class="sxs-lookup"><span data-stu-id="0df4f-143">Add a controller</span></span>

<span data-ttu-id="0df4f-144">En el Explorador de soluciones, haga clic con el botón derecho en la carpeta *Controladores*.</span><span class="sxs-lookup"><span data-stu-id="0df4f-144">In Solution Explorer, right-click the *Controllers* folder.</span></span> <span data-ttu-id="0df4f-145">Seleccione **Agregar** > **Nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="0df4f-145">Select **Add** > **New Item**.</span></span> <span data-ttu-id="0df4f-146">En el cuadro de diálogo **Agregar nuevo elemento**, seleccione la plantilla **Clase de controlador de API**.</span><span class="sxs-lookup"><span data-stu-id="0df4f-146">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span> <span data-ttu-id="0df4f-147">Denomine la clase *TodoController* y, después, haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="0df4f-147">Name the class *TodoController*, and click **Add**.</span></span>

![Cuadro de diálogo Agregar nuevo elemento con la palabra "controlador" en el cuadro de búsqueda y la opción Clase de controlador de API web seleccionada](first-web-api/_static/new_controller.png)

<span data-ttu-id="0df4f-149">Reemplace la clase por el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="0df4f-149">Replace the class with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="0df4f-150">Iniciar la aplicación</span><span class="sxs-lookup"><span data-stu-id="0df4f-150">Launch the app</span></span>

<span data-ttu-id="0df4f-151">En Visual Studio, presione CTRL+F5 para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0df4f-151">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="0df4f-152">Visual Studio inicia un explorador y navega hasta `http://localhost:<port>/api/values`, donde `<port>` es un número de puerto elegido aleatoriamente.</span><span class="sxs-lookup"><span data-stu-id="0df4f-152">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="0df4f-153">Vaya al controlador `Todo` en `http://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="0df4f-153">Navigate to the `Todo` controller at `http://localhost:<port>/api/todo`.</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
