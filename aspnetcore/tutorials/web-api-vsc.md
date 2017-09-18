---
title: Crear una API Web con ASP.NET Core y Visual Studio Code
author: rick-anderson
description: Compilar una API web con ASP.NET Core MVC y Visual Studio Code en macOS, Linux o Windows
keywords: ASP.NET Core, WebAPI, Web API, REST, Mac, Linux, HTTP, servicio, servicio HTTP, VS Code
ms.author: riande
manager: wpickett
ms.date: 5/24/2017
ms.topic: get-started-article
ms.assetid: 830b4bf5-dd14-423e-9f59-764a6f13a8f6
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/web-api-vsc
ms.openlocfilehash: abe088f2c9df94135209ce71540e6b345186ee70
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2017
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-code-on-linux-macos-and-windows"></a><span data-ttu-id="25a74-104">Crear una Web API con ASP.NET Core MVC y Visual Studio Code en Linux, macOS y Windows</span><span class="sxs-lookup"><span data-stu-id="25a74-104">Create a Web API with ASP.NET Core MVC and Visual Studio Code on Linux, macOS, and Windows</span></span>

<span data-ttu-id="25a74-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="25a74-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="25a74-106">En este tutorial compilará una API Web para administrar una lista de tareas pendientes.</span><span class="sxs-lookup"><span data-stu-id="25a74-106">In this tutorial, you’ll build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="25a74-107">No compilará ninguna interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="25a74-107">You won’t build a UI.</span></span>

<span data-ttu-id="25a74-108">Hay tres versiones de este tutorial:</span><span class="sxs-lookup"><span data-stu-id="25a74-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="25a74-109">macOS, Linux y Windows: API Web con Visual Studio Code (este tutorial)</span><span class="sxs-lookup"><span data-stu-id="25a74-109">macOS, Linux, Windows: Web API with Visual Studio Code (This tutorial)</span></span>
* <span data-ttu-id="25a74-110">macOS: [API Web con Visual Studio para Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="25a74-110">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="25a74-111">Windows: [API Web con Visual Studio para Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="25a74-111">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="set-up-your-development-environment"></a><span data-ttu-id="25a74-112">Configuración del entorno de desarrollo</span><span class="sxs-lookup"><span data-stu-id="25a74-112">Set up your development environment</span></span>

<span data-ttu-id="25a74-113">Descargue e instale:</span><span class="sxs-lookup"><span data-stu-id="25a74-113">Download and install:</span></span>
- [<span data-ttu-id="25a74-114">Núcleo de .NET</span><span class="sxs-lookup"><span data-stu-id="25a74-114">.NET Core</span></span>](https://microsoft.com/net/core)
- [<span data-ttu-id="25a74-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="25a74-115">Visual Studio Code</span></span>](https://code.visualstudio.com)
- <span data-ttu-id="25a74-116">[Extensión de C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) de Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="25a74-116">Visual Studio Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span>

## <a name="create-the-project"></a><span data-ttu-id="25a74-117">Crear el proyecto</span><span class="sxs-lookup"><span data-stu-id="25a74-117">Create the project</span></span>

<span data-ttu-id="25a74-118">Desde una consola, ejecute los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="25a74-118">From a console, run the following commands:</span></span>

```console
mkdir TodoApi
cd TodoApi
dotnet new webapi
```

<span data-ttu-id="25a74-119">Abra la carpeta *TodoApi* en Visual Studio Code (VS Code) y seleccione el archivo *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="25a74-119">Open the *TodoApi* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="25a74-120">Seleccione **Sí** en el mensaje de **advertencia** "Required assets to build and debug are missing from 'TodoApi'.</span><span class="sxs-lookup"><span data-stu-id="25a74-120">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'TodoApi'.</span></span> <span data-ttu-id="25a74-121">Add them?" (Faltan los activos necesarios para compilar y depurar en 'TodoApi'. ¿Quiere agregarlos?).</span><span class="sxs-lookup"><span data-stu-id="25a74-121">Add them?"</span></span>
- <span data-ttu-id="25a74-122">Seleccione **Restaurar** en el mensaje de **información** "There are unresolved dependencies" (Hay dependencias no resueltas).</span><span class="sxs-lookup"><span data-stu-id="25a74-122">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![VS Code con la advertencia "Required assets to build and debug are missing from 'TodoApi'.](web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="25a74-126">Presione **Depurar** (F5) para compilar y ejecutar el programa.</span><span class="sxs-lookup"><span data-stu-id="25a74-126">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="25a74-127">En un explorador, vaya a http://localhost:5000/api/values.</span><span class="sxs-lookup"><span data-stu-id="25a74-127">In a browser navigate to http://localhost:5000/api/values .</span></span> <span data-ttu-id="25a74-128">Se muestra lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="25a74-128">The following is displayed:</span></span>

`["value1","value2"]`

<span data-ttu-id="25a74-129">Vea [Ayuda de Visual Studio Code](#visual-studio-code-help) para obtener sugerencias sobre el uso de VS Code.</span><span class="sxs-lookup"><span data-stu-id="25a74-129">See [Visual Studio Code help](#visual-studio-code-help) for tips on using VS Code.</span></span>

## <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="25a74-130">Agregar compatibilidad con Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="25a74-130">Add support for Entity Framework Core</span></span>

<span data-ttu-id="25a74-131">Edite el archivo *TodoApi.csproj* para instalar el proveedor de base de datos [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/).</span><span class="sxs-lookup"><span data-stu-id="25a74-131">Edit the *TodoApi.csproj* file to install the [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) database provider.</span></span> <span data-ttu-id="25a74-132">Este proveedor de base de datos permite usar Entity Framework Core con una base de datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="25a74-132">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

<span data-ttu-id="25a74-133">[!code-xml[Main](web-api-vsc/sample/TodoApi/TodoApi.csproj?highlight=12)]</span><span class="sxs-lookup"><span data-stu-id="25a74-133">[!code-xml[Main](web-api-vsc/sample/TodoApi/TodoApi.csproj?highlight=12)]</span></span>

<span data-ttu-id="25a74-134">Ejecute `dotnet restore` para descargar e instalar el proveedor de base de datos EF Core InMemory.</span><span class="sxs-lookup"><span data-stu-id="25a74-134">Run `dotnet restore` to download and install the EF Core InMemory DB provider.</span></span> <span data-ttu-id="25a74-135">Puede ejecutar `dotnet restore` desde el terminal o escribir `⌘⇧P` (macOS) o `Ctrl+Shift+P` (Linux) en VS Code y luego escribir **.NET**.</span><span class="sxs-lookup"><span data-stu-id="25a74-135">You can run `dotnet restore` from the terminal or enter `⌘⇧P` (macOS) or `Ctrl+Shift+P` (Linux) in VS Code and then type **.NET**.</span></span> <span data-ttu-id="25a74-136">Seleccione **.NET: Restaurar paquetes**.</span><span class="sxs-lookup"><span data-stu-id="25a74-136">Select **.NET: Restore Packages**.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="25a74-137">Agregar una clase de modelo</span><span class="sxs-lookup"><span data-stu-id="25a74-137">Add a model class</span></span>

<span data-ttu-id="25a74-138">Un modelo es un objeto que representa los datos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="25a74-138">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="25a74-139">En este caso, el único modelo es una tarea pendiente.</span><span class="sxs-lookup"><span data-stu-id="25a74-139">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="25a74-140">Agregue una carpeta denominada *Models*.</span><span class="sxs-lookup"><span data-stu-id="25a74-140">Add a folder named *Models*.</span></span> <span data-ttu-id="25a74-141">Puede colocar clases de modelo en cualquier lugar del proyecto, pero la carpeta *Models* se usa por convención.</span><span class="sxs-lookup"><span data-stu-id="25a74-141">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="25a74-142">Agregue una clase `TodoItem` con el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="25a74-142">Add a `TodoItem` class with the following code:</span></span>

<span data-ttu-id="25a74-143">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]</span><span class="sxs-lookup"><span data-stu-id="25a74-143">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]</span></span>

<span data-ttu-id="25a74-144">La base de datos genera el `Id` cuando se crea `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="25a74-144">The database generates the `Id` when a `TodoItem` is created.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="25a74-145">Crear el contexto de base de datos</span><span class="sxs-lookup"><span data-stu-id="25a74-145">Create the database context</span></span>

<span data-ttu-id="25a74-146">El *contexto de base de datos* es la clase principal que coordina la funcionalidad de Entity Framework para un modelo de datos determinado.</span><span class="sxs-lookup"><span data-stu-id="25a74-146">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="25a74-147">Esta clase se crea al derivar de la clase `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="25a74-147">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="25a74-148">Agregue una clase `TodoContext` a la carpeta *Models*:</span><span class="sxs-lookup"><span data-stu-id="25a74-148">Add a `TodoContext` class in the *Models* folder:</span></span>

<span data-ttu-id="25a74-149">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]</span><span class="sxs-lookup"><span data-stu-id="25a74-149">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]</span></span>

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="25a74-150">Adición de un controlador</span><span class="sxs-lookup"><span data-stu-id="25a74-150">Add a controller</span></span>

<span data-ttu-id="25a74-151">En la carpeta *Controladores*, cree una clase denominada `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="25a74-151">In the *Controllers* folder, create a class named `TodoController`.</span></span> <span data-ttu-id="25a74-152">Agregue el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="25a74-152">Add the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="25a74-153">Iniciar la aplicación</span><span class="sxs-lookup"><span data-stu-id="25a74-153">Launch the app</span></span>

<span data-ttu-id="25a74-154">En VS Code, presione F5 para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="25a74-154">In VS Code, press F5 to launch the app.</span></span> <span data-ttu-id="25a74-155">Vaya a http://localhost:5000/api/todo (el controlador `Todo` que se acaba de crear).</span><span class="sxs-lookup"><span data-stu-id="25a74-155">Navigate to  http://localhost:5000/api/todo   (The `Todo` controller we just created).</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a><span data-ttu-id="25a74-156">Ayuda de Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="25a74-156">Visual Studio Code help</span></span>

- [<span data-ttu-id="25a74-157">Introducción</span><span class="sxs-lookup"><span data-stu-id="25a74-157">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="25a74-158">Depuración</span><span class="sxs-lookup"><span data-stu-id="25a74-158">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="25a74-159">Terminal integrado</span><span class="sxs-lookup"><span data-stu-id="25a74-159">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="25a74-160">Métodos abreviados de teclado</span><span class="sxs-lookup"><span data-stu-id="25a74-160">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="25a74-161">Métodos abreviados de teclado de Mac</span><span class="sxs-lookup"><span data-stu-id="25a74-161">Mac keyboard shortcuts</span></span>](https://go.microsoft.com/fwlink/?linkid=832143)
  - [<span data-ttu-id="25a74-162">Métodos abreviados de teclado de Linux</span><span class="sxs-lookup"><span data-stu-id="25a74-162">Linux keyboard shortcuts</span></span>](https://go.microsoft.com/fwlink/?linkid=832144)
  - [<span data-ttu-id="25a74-163">Métodos abreviados de teclado de Windows</span><span class="sxs-lookup"><span data-stu-id="25a74-163">Windows keyboard shortcuts</span></span>](https://go.microsoft.com/fwlink/?linkid=832145)

[!INCLUDE[next steps](../includes/webApi/next.md)]


