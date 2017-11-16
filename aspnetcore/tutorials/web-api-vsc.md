---
title: Crear una API Web con ASP.NET Core y Visual Studio Code
description: Compilar una API web con ASP.NET Core MVC y Visual Studio Code en macOS, Linux o Windows
author: rick-anderson
ms.author: riande
ms.date: 09/22/2017
ms.topic: get-started-article
ms.prod: asp.net-core
ms.technology: aspnet
keywords: ASP.NET Core, WebAPI, API web, REST, Mac, Linux, HTTP, servicio, servicio HTTP, VS Code
manager: wpickett
ms.assetid: 830b4bf5-dd14-423e-9f59-764a6f13a8f6
uid: tutorials/web-api-vsc
ms.openlocfilehash: caf40ee1c2d45d2fbf33b07d707fa4f1be98d31c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-code-on-linux-macos-and-windows"></a><span data-ttu-id="378c2-104">Crear una Web API con ASP.NET Core MVC y Visual Studio Code en Linux, macOS y Windows</span><span class="sxs-lookup"><span data-stu-id="378c2-104">Create a Web API with ASP.NET Core MVC and Visual Studio Code on Linux, macOS, and Windows</span></span>

<span data-ttu-id="378c2-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="378c2-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="378c2-106">En este tutorial compilará una API Web para administrar una lista de tareas pendientes.</span><span class="sxs-lookup"><span data-stu-id="378c2-106">In this tutorial, you’ll build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="378c2-107">No compilará ninguna interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="378c2-107">You won’t build a UI.</span></span>

<span data-ttu-id="378c2-108">Hay tres versiones de este tutorial:</span><span class="sxs-lookup"><span data-stu-id="378c2-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="378c2-109">macOS, Linux y Windows: API Web con Visual Studio Code (este tutorial)</span><span class="sxs-lookup"><span data-stu-id="378c2-109">macOS, Linux, Windows: Web API with Visual Studio Code (This tutorial)</span></span>
* <span data-ttu-id="378c2-110">macOS: [API Web con Visual Studio para Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="378c2-110">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="378c2-111">Windows: [API Web con Visual Studio para Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="378c2-111">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="set-up-your-development-environment"></a><span data-ttu-id="378c2-112">Configuración del entorno de desarrollo</span><span class="sxs-lookup"><span data-stu-id="378c2-112">Set up your development environment</span></span>

<span data-ttu-id="378c2-113">Descargue e instale:</span><span class="sxs-lookup"><span data-stu-id="378c2-113">Download and install:</span></span>
- <span data-ttu-id="378c2-114">[SDK de .NET Core 2.0.0](https://www.microsoft.com/net/core) o versiones posteriores</span><span class="sxs-lookup"><span data-stu-id="378c2-114">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>
- [<span data-ttu-id="378c2-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="378c2-115">Visual Studio Code</span></span>](https://code.visualstudio.com)
- <span data-ttu-id="378c2-116">[Extensión de C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) de Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="378c2-116">Visual Studio Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span>

## <a name="create-the-project"></a><span data-ttu-id="378c2-117">Crear el proyecto</span><span class="sxs-lookup"><span data-stu-id="378c2-117">Create the project</span></span>

<span data-ttu-id="378c2-118">Desde una consola, ejecute los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="378c2-118">From a console, run the following commands:</span></span>

```console
mkdir TodoApi
cd TodoApi
dotnet new webapi
```

<span data-ttu-id="378c2-119">Abra la carpeta *TodoApi* en Visual Studio Code (VS Code) y seleccione el archivo *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="378c2-119">Open the *TodoApi* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="378c2-120">Seleccione **Sí** en el mensaje de **advertencia** "Required assets to build and debug are missing from 'TodoApi'.</span><span class="sxs-lookup"><span data-stu-id="378c2-120">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'TodoApi'.</span></span> <span data-ttu-id="378c2-121">Add them?" (Faltan los activos necesarios para compilar y depurar en 'TodoApi'. ¿Quiere agregarlos?).</span><span class="sxs-lookup"><span data-stu-id="378c2-121">Add them?"</span></span>
- <span data-ttu-id="378c2-122">Seleccione **Restaurar** en el mensaje de **información** "There are unresolved dependencies" (Hay dependencias no resueltas).</span><span class="sxs-lookup"><span data-stu-id="378c2-122">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![VS Code con la advertencia "Required assets to build and debug are missing from 'TodoApi'.](web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="378c2-126">Presione **Depurar** (F5) para compilar y ejecutar el programa.</span><span class="sxs-lookup"><span data-stu-id="378c2-126">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="378c2-127">En un explorador, vaya a http://localhost:5000/api/values.</span><span class="sxs-lookup"><span data-stu-id="378c2-127">In a browser navigate to http://localhost:5000/api/values .</span></span> <span data-ttu-id="378c2-128">Se muestra lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="378c2-128">The following is displayed:</span></span>

`["value1","value2"]`

<span data-ttu-id="378c2-129">Vea [Ayuda de Visual Studio Code](#visual-studio-code-help) para obtener sugerencias sobre el uso de VS Code.</span><span class="sxs-lookup"><span data-stu-id="378c2-129">See [Visual Studio Code help](#visual-studio-code-help) for tips on using VS Code.</span></span>

## <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="378c2-130">Agregar compatibilidad con Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="378c2-130">Add support for Entity Framework Core</span></span>

<span data-ttu-id="378c2-131">Edite el archivo *TodoApi.csproj* para instalar el proveedor de base de datos [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/).</span><span class="sxs-lookup"><span data-stu-id="378c2-131">Edit the *TodoApi.csproj* file to install the [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) database provider.</span></span> <span data-ttu-id="378c2-132">Este proveedor de base de datos permite usar Entity Framework Core con una base de datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="378c2-132">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

[!code-xml[Main](web-api-vsc/sample/TodoApi/TodoApi.csproj?highlight=12)]

<span data-ttu-id="378c2-133">Ejecute `dotnet restore` para descargar e instalar el proveedor de base de datos EF Core InMemory.</span><span class="sxs-lookup"><span data-stu-id="378c2-133">Run `dotnet restore` to download and install the EF Core InMemory DB provider.</span></span> <span data-ttu-id="378c2-134">Puede ejecutar `dotnet restore` desde el terminal o escribir `⌘⇧P` (macOS) o `Ctrl+Shift+P` (Linux) en VS Code y luego escribir **.NET**.</span><span class="sxs-lookup"><span data-stu-id="378c2-134">You can run `dotnet restore` from the terminal or enter `⌘⇧P` (macOS) or `Ctrl+Shift+P` (Linux) in VS Code and then type **.NET**.</span></span> <span data-ttu-id="378c2-135">Seleccione **.NET: Restaurar paquetes**.</span><span class="sxs-lookup"><span data-stu-id="378c2-135">Select **.NET: Restore Packages**.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="378c2-136">Agregar una clase de modelo</span><span class="sxs-lookup"><span data-stu-id="378c2-136">Add a model class</span></span>

<span data-ttu-id="378c2-137">Un modelo es un objeto que representa los datos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="378c2-137">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="378c2-138">En este caso, el único modelo es una tarea pendiente.</span><span class="sxs-lookup"><span data-stu-id="378c2-138">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="378c2-139">Agregue una carpeta denominada *Models*.</span><span class="sxs-lookup"><span data-stu-id="378c2-139">Add a folder named *Models*.</span></span> <span data-ttu-id="378c2-140">Puede colocar clases de modelo en cualquier lugar del proyecto, pero la carpeta *Models* se usa por convención.</span><span class="sxs-lookup"><span data-stu-id="378c2-140">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="378c2-141">Agregue una clase `TodoItem` con el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="378c2-141">Add a `TodoItem` class with the following code:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="378c2-142">La base de datos genera el `Id` cuando se crea `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="378c2-142">The database generates the `Id` when a `TodoItem` is created.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="378c2-143">Crear el contexto de base de datos</span><span class="sxs-lookup"><span data-stu-id="378c2-143">Create the database context</span></span>

<span data-ttu-id="378c2-144">El *contexto de base de datos* es la clase principal que coordina la funcionalidad de Entity Framework para un modelo de datos determinado.</span><span class="sxs-lookup"><span data-stu-id="378c2-144">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="378c2-145">Esta clase se crea al derivar de la clase `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="378c2-145">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="378c2-146">Agregue una clase `TodoContext` a la carpeta *Models*:</span><span class="sxs-lookup"><span data-stu-id="378c2-146">Add a `TodoContext` class in the *Models* folder:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="378c2-147">Adición de un controlador</span><span class="sxs-lookup"><span data-stu-id="378c2-147">Add a controller</span></span>

<span data-ttu-id="378c2-148">En la carpeta *Controladores*, cree una clase denominada `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="378c2-148">In the *Controllers* folder, create a class named `TodoController`.</span></span> <span data-ttu-id="378c2-149">Agregue el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="378c2-149">Add the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="378c2-150">Iniciar la aplicación</span><span class="sxs-lookup"><span data-stu-id="378c2-150">Launch the app</span></span>

<span data-ttu-id="378c2-151">En VS Code, presione F5 para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="378c2-151">In VS Code, press F5 to launch the app.</span></span> <span data-ttu-id="378c2-152">Vaya a http://localhost:5000/api/todo (el controlador `Todo` que se acaba de crear).</span><span class="sxs-lookup"><span data-stu-id="378c2-152">Navigate to  http://localhost:5000/api/todo   (The `Todo` controller we just created).</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a><span data-ttu-id="378c2-153">Ayuda de Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="378c2-153">Visual Studio Code help</span></span>

- [<span data-ttu-id="378c2-154">Introducción</span><span class="sxs-lookup"><span data-stu-id="378c2-154">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="378c2-155">Depuración</span><span class="sxs-lookup"><span data-stu-id="378c2-155">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="378c2-156">Terminal integrado</span><span class="sxs-lookup"><span data-stu-id="378c2-156">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="378c2-157">Métodos abreviados de teclado</span><span class="sxs-lookup"><span data-stu-id="378c2-157">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="378c2-158">Métodos abreviados de teclado de Mac</span><span class="sxs-lookup"><span data-stu-id="378c2-158">Mac keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="378c2-159">Métodos abreviados de teclado de Linux</span><span class="sxs-lookup"><span data-stu-id="378c2-159">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="378c2-160">Métodos abreviados de teclado de Windows</span><span class="sxs-lookup"><span data-stu-id="378c2-160">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]


