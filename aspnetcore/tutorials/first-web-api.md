---
title: Crear una API web con ASP.NET Core y Visual Studio
author: rick-anderson
description: Crear una API web con ASP.NET Core MVC y Visual Studio en Windows
ms.author: riande
ms.custom: mvc
ms.date: 05/17/2018
uid: tutorials/first-web-api
ms.openlocfilehash: eb23d5376742e04712f714263815582fddc5d18e
ms.sourcegitcommit: e9b99854b0a8021dafabee0db5e1338067f250a9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2018
ms.locfileid: "52450702"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio"></a><span data-ttu-id="0fb73-103">Crear una API web con ASP.NET Core y Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0fb73-103">Create a Web API with ASP.NET Core and Visual Studio</span></span>

<span data-ttu-id="0fb73-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="0fb73-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="0fb73-105">En este tutorial se compila una API web para administrar una lista de tareas pendientes.</span><span class="sxs-lookup"><span data-stu-id="0fb73-105">This tutorial builds a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="0fb73-106">No se crea ninguna interfaz de usuario (UI).</span><span class="sxs-lookup"><span data-stu-id="0fb73-106">A user interface (UI) isn't created.</span></span>

<span data-ttu-id="0fb73-107">Hay tres versiones de este tutorial:</span><span class="sxs-lookup"><span data-stu-id="0fb73-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="0fb73-108">Windows: API web con Visual Studio en Windows (en este tutorial, vea la [versión en vídeo](https://www.youtube.com/watch?v=TTkhEyGBfAk))</span><span class="sxs-lookup"><span data-stu-id="0fb73-108">Windows: Web API with Visual Studio on Windows (This tutorial, see the [video version](https://www.youtube.com/watch?v=TTkhEyGBfAk))</span></span>
* <span data-ttu-id="0fb73-109">macOS: [API Web con Visual Studio para Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="0fb73-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="0fb73-110">macOS, Linux y Windows: [API web con Visual Studio Code](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="0fb73-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

> [!NOTE]
> <span data-ttu-id="0fb73-111">Vamos a probar la facilidad de uso de una nueva estructura propuesta para la tabla de contenido de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0fb73-111">We’re testing the usability of a proposed new structure for the ASP.NET Core table of contents.</span></span>  <span data-ttu-id="0fb73-112">Si tiene unos minutos para realizar un ejercicio de búsqueda de 7 temas diferentes en la tabla actual o propuesta de contenido, [haga clic aquí para participar en el estudio](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).</span><span class="sxs-lookup"><span data-stu-id="0fb73-112">If you have a few minutes to try an exercise of finding 7 different topics in the current or proposed table of contents, please [click here to participate in the study](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).</span></span>

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="0fb73-113">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="0fb73-113">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="create-the-project"></a><span data-ttu-id="0fb73-114">Crear el proyecto</span><span class="sxs-lookup"><span data-stu-id="0fb73-114">Create the project</span></span>

<span data-ttu-id="0fb73-115">Haga lo siguiente para descargar Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="0fb73-115">Follow these steps in Visual Studio:</span></span>

* <span data-ttu-id="0fb73-116">En el menú **Archivo**, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="0fb73-116">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="0fb73-117">Seleccione la plantilla **Aplicación web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="0fb73-117">Select the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="0fb73-118">Denomine el proyecto *TodoApi* y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="0fb73-118">Name the project *TodoApi* and click **OK**.</span></span>
* <span data-ttu-id="0fb73-119">En el cuadro de diálogo **Nueva aplicación web ASP.NET Core - TodoApi**, seleccione la versión ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0fb73-119">In the **New ASP.NET Core Web Application - TodoApi** dialog, choose the ASP.NET Core version.</span></span> <span data-ttu-id="0fb73-120">Seleccione la plantilla **API** y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="0fb73-120">Select the **API** template and click **OK**.</span></span> <span data-ttu-id="0fb73-121">**No** seleccione **Habilitar compatibilidad con Docker**.</span><span class="sxs-lookup"><span data-stu-id="0fb73-121">Do **not** select **Enable Docker Support**.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="0fb73-122">Iniciar la aplicación</span><span class="sxs-lookup"><span data-stu-id="0fb73-122">Launch the app</span></span>

<span data-ttu-id="0fb73-123">En Visual Studio, presione CTRL+F5 para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0fb73-123">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="0fb73-124">Visual Studio inicia un explorador y navega hasta `http://localhost:<port>/api/values`, donde `<port>` es un número de puerto elegido aleatoriamente.</span><span class="sxs-lookup"><span data-stu-id="0fb73-124">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="0fb73-125">En Chrome, Microsoft Edge y Firefox se muestra la salida siguiente:</span><span class="sxs-lookup"><span data-stu-id="0fb73-125">Chrome, Microsoft Edge, and Firefox display the following output:</span></span>

```json
["value1","value2"]
```

<span data-ttu-id="0fb73-126">Si usa Internet Explorer, se le pedirá que guarde un archivo *values.json*.</span><span class="sxs-lookup"><span data-stu-id="0fb73-126">If using Internet Explorer, you'll be prompted to save a *values.json* file.</span></span>

### <a name="add-a-model-class"></a><span data-ttu-id="0fb73-127">Agregar una clase de modelo</span><span class="sxs-lookup"><span data-stu-id="0fb73-127">Add a model class</span></span>

<span data-ttu-id="0fb73-128">Un modelo es un objeto que representa los datos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0fb73-128">A model is an object representing the data in the app.</span></span> <span data-ttu-id="0fb73-129">En este caso, el único modelo es una tarea pendiente.</span><span class="sxs-lookup"><span data-stu-id="0fb73-129">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="0fb73-130">En el Explorador de soluciones, haga clic con el botón derecho en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="0fb73-130">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="0fb73-131">Seleccione **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="0fb73-131">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="0fb73-132">Asigne a la carpeta el nombre *Models*.</span><span class="sxs-lookup"><span data-stu-id="0fb73-132">Name the folder *Models*.</span></span>

> [!NOTE]
> <span data-ttu-id="0fb73-133">Las clases del modelo pueden ir en cualquier parte del proyecto.</span><span class="sxs-lookup"><span data-stu-id="0fb73-133">The model classes can go anywhere in the project.</span></span> <span data-ttu-id="0fb73-134">La carpeta *Models* se usa por convención para las clases de modelos.</span><span class="sxs-lookup"><span data-stu-id="0fb73-134">The *Models* folder is used by convention for model classes.</span></span>

<span data-ttu-id="0fb73-135">En el Explorador de soluciones, haga clic con el botón derecho en la carpeta de *modelos* y seleccione **Agregar** > **Clase**.</span><span class="sxs-lookup"><span data-stu-id="0fb73-135">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="0fb73-136">Denomine la clase *TodoItem* y, después, haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="0fb73-136">Name the class *TodoItem* and click **Add**.</span></span>

<span data-ttu-id="0fb73-137">Actualice la clase `TodoItem` por el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="0fb73-137">Update the `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="0fb73-138">La base de datos genera el `Id` cuando se crea `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="0fb73-138">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="0fb73-139">Crear el contexto de base de datos</span><span class="sxs-lookup"><span data-stu-id="0fb73-139">Create the database context</span></span>

<span data-ttu-id="0fb73-140">El *contexto de base de datos* es la clase principal que coordina la funcionalidad de Entity Framework para un modelo de datos determinado.</span><span class="sxs-lookup"><span data-stu-id="0fb73-140">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="0fb73-141">Esta clase se crea derivándola de la clase `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="0fb73-141">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="0fb73-142">En el Explorador de soluciones, haga clic con el botón derecho en la carpeta de *modelos* y seleccione **Agregar** > **Clase**.</span><span class="sxs-lookup"><span data-stu-id="0fb73-142">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="0fb73-143">Denomine la clase *TodoContext* y, después, haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="0fb73-143">Name the class *TodoContext* and click **Add**.</span></span>

<span data-ttu-id="0fb73-144">Reemplace la clase por el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="0fb73-144">Replace the class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a><span data-ttu-id="0fb73-145">Adición de un controlador</span><span class="sxs-lookup"><span data-stu-id="0fb73-145">Add a controller</span></span>

<span data-ttu-id="0fb73-146">En el Explorador de soluciones, haga clic con el botón derecho en la carpeta *Controladores*.</span><span class="sxs-lookup"><span data-stu-id="0fb73-146">In Solution Explorer, right-click the *Controllers* folder.</span></span> <span data-ttu-id="0fb73-147">Seleccione **Agregar** > **Nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="0fb73-147">Select **Add** > **New Item**.</span></span> <span data-ttu-id="0fb73-148">En el cuadro de diálogo **Agregar nuevo elemento**, seleccione la plantilla **Clase de controlador de API**.</span><span class="sxs-lookup"><span data-stu-id="0fb73-148">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span> <span data-ttu-id="0fb73-149">Denomine la clase *TodoController* y, después, haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="0fb73-149">Name the class *TodoController*, and click **Add**.</span></span>

![Cuadro de diálogo Agregar nuevo elemento con la palabra "controlador" en el cuadro de búsqueda y la opción Clase de controlador de API web seleccionada](first-web-api/_static/new_controller.png)

<span data-ttu-id="0fb73-151">Reemplace la clase por el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="0fb73-151">Replace the class with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="0fb73-152">Iniciar la aplicación</span><span class="sxs-lookup"><span data-stu-id="0fb73-152">Launch the app</span></span>

<span data-ttu-id="0fb73-153">En Visual Studio, presione CTRL+F5 para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0fb73-153">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="0fb73-154">Visual Studio inicia un explorador y navega hasta `http://localhost:<port>/api/values`, donde `<port>` es un número de puerto elegido aleatoriamente.</span><span class="sxs-lookup"><span data-stu-id="0fb73-154">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="0fb73-155">Vaya al controlador `Todo` en `http://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="0fb73-155">Navigate to the `Todo` controller at `http://localhost:<port>/api/todo`.</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
