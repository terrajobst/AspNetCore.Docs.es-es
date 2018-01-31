---
title: Crear una API Web con ASP.NET Core y Visual Studio Code
description: Compilar una API web con ASP.NET Core MVC y Visual Studio Code en macOS, Linux o Windows
author: rick-anderson
ms.author: riande
ms.date: 09/22/2017
ms.topic: get-started-article
ms.prod: asp.net-core
ms.technology: aspnet
manager: wpickett
uid: tutorials/web-api-vsc
ms.openlocfilehash: 9fec8904cf05fc486160c0641731c6336fe2766a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-code-on-linux-macos-and-windows"></a><span data-ttu-id="61092-103">Crear una Web API con ASP.NET Core MVC y Visual Studio Code en Linux, macOS y Windows</span><span class="sxs-lookup"><span data-stu-id="61092-103">Create a Web API with ASP.NET Core MVC and Visual Studio Code on Linux, macOS, and Windows</span></span>

<span data-ttu-id="61092-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="61092-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="61092-105">En este tutorial compilará una API Web para administrar una lista de tareas pendientes.</span><span class="sxs-lookup"><span data-stu-id="61092-105">In this tutorial, you’ll build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="61092-106">No compilará ninguna interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="61092-106">You won’t build a UI.</span></span>

<span data-ttu-id="61092-107">Hay tres versiones de este tutorial:</span><span class="sxs-lookup"><span data-stu-id="61092-107">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="61092-108">macOS, Linux y Windows: API Web con Visual Studio Code (este tutorial)</span><span class="sxs-lookup"><span data-stu-id="61092-108">macOS, Linux, Windows: Web API with Visual Studio Code (This tutorial)</span></span>
* <span data-ttu-id="61092-109">macOS: [API Web con Visual Studio para Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="61092-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="61092-110">Windows: [API Web con Visual Studio para Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="61092-110">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="set-up-your-development-environment"></a><span data-ttu-id="61092-111">Configuración del entorno de desarrollo</span><span class="sxs-lookup"><span data-stu-id="61092-111">Set up your development environment</span></span>

<span data-ttu-id="61092-112">Descargue e instale:</span><span class="sxs-lookup"><span data-stu-id="61092-112">Download and install:</span></span>
- <span data-ttu-id="61092-113">[SDK de .NET Core 2.0.0](https://www.microsoft.com/net/core) o versiones posteriores</span><span class="sxs-lookup"><span data-stu-id="61092-113">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>
- [<span data-ttu-id="61092-114">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="61092-114">Visual Studio Code</span></span>](https://code.visualstudio.com)
- <span data-ttu-id="61092-115">[Extensión de C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) de Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="61092-115">Visual Studio Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span>

## <a name="create-the-project"></a><span data-ttu-id="61092-116">Crear el proyecto</span><span class="sxs-lookup"><span data-stu-id="61092-116">Create the project</span></span>

<span data-ttu-id="61092-117">Desde una consola, ejecute los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="61092-117">From a console, run the following commands:</span></span>

```console
mkdir TodoApi
cd TodoApi
dotnet new webapi
```

<span data-ttu-id="61092-118">Abra la carpeta *TodoApi* en Visual Studio Code (VS Code) y seleccione el archivo *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="61092-118">Open the *TodoApi* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="61092-119">Seleccione **Sí** en el mensaje de **advertencia** "Required assets to build and debug are missing from 'TodoApi'.</span><span class="sxs-lookup"><span data-stu-id="61092-119">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'TodoApi'.</span></span> <span data-ttu-id="61092-120">Add them?" (Faltan los activos necesarios para compilar y depurar en 'TodoApi'. ¿Quiere agregarlos?).</span><span class="sxs-lookup"><span data-stu-id="61092-120">Add them?"</span></span>
- <span data-ttu-id="61092-121">Seleccione **Restaurar** en el mensaje de **información** "There are unresolved dependencies" (Hay dependencias no resueltas).</span><span class="sxs-lookup"><span data-stu-id="61092-121">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![VS Code con la advertencia "Required assets to build and debug are missing from 'TodoApi'.](web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="61092-125">Presione **Depurar** (F5) para compilar y ejecutar el programa.</span><span class="sxs-lookup"><span data-stu-id="61092-125">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="61092-126">En un explorador, vaya a http://localhost:5000/api/values.</span><span class="sxs-lookup"><span data-stu-id="61092-126">In a browser navigate to http://localhost:5000/api/values .</span></span> <span data-ttu-id="61092-127">Se muestra lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="61092-127">The following is displayed:</span></span>

`["value1","value2"]`

<span data-ttu-id="61092-128">Vea [Ayuda de Visual Studio Code](#visual-studio-code-help) para obtener sugerencias sobre el uso de VS Code.</span><span class="sxs-lookup"><span data-stu-id="61092-128">See [Visual Studio Code help](#visual-studio-code-help) for tips on using VS Code.</span></span>

## <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="61092-129">Agregar compatibilidad con Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="61092-129">Add support for Entity Framework Core</span></span>

<span data-ttu-id="61092-130">Al crear un proyecto nuevo en .NET Core 2.0, se agrega el proveedor "Microsoft.AspNetCore.All" en el archivo *TodoApi.csproj*.</span><span class="sxs-lookup"><span data-stu-id="61092-130">Creating a new project in .NET Core 2.0 adds the 'Microsoft.AspNetCore.All' provider in the *TodoApi.csproj* file.</span></span> <span data-ttu-id="61092-131">No es necesario instalar el proveedor de base de datos [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) por separado.</span><span class="sxs-lookup"><span data-stu-id="61092-131">There's no need to install the [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) database provider separately.</span></span> <span data-ttu-id="61092-132">Este proveedor de base de datos permite usar Entity Framework Core con una base de datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="61092-132">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

[!code-xml[Main](web-api-vsc/sample/TodoApi/TodoApi.csproj?highlight=12)]

## <a name="add-a-model-class"></a><span data-ttu-id="61092-133">Agregar una clase de modelo</span><span class="sxs-lookup"><span data-stu-id="61092-133">Add a model class</span></span>

<span data-ttu-id="61092-134">Un modelo es un objeto que representa los datos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="61092-134">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="61092-135">En este caso, el único modelo es una tarea pendiente.</span><span class="sxs-lookup"><span data-stu-id="61092-135">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="61092-136">Agregue una carpeta denominada *Models*.</span><span class="sxs-lookup"><span data-stu-id="61092-136">Add a folder named *Models*.</span></span> <span data-ttu-id="61092-137">Puede colocar clases de modelo en cualquier lugar del proyecto, pero la carpeta *Models* se usa por convención.</span><span class="sxs-lookup"><span data-stu-id="61092-137">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="61092-138">Agregue una clase `TodoItem` con el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="61092-138">Add a `TodoItem` class with the following code:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="61092-139">La base de datos genera el `Id` cuando se crea `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="61092-139">The database generates the `Id` when a `TodoItem` is created.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="61092-140">Crear el contexto de base de datos</span><span class="sxs-lookup"><span data-stu-id="61092-140">Create the database context</span></span>

<span data-ttu-id="61092-141">El *contexto de base de datos* es la clase principal que coordina la funcionalidad de Entity Framework para un modelo de datos determinado.</span><span class="sxs-lookup"><span data-stu-id="61092-141">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="61092-142">Esta clase se crea al derivar de la clase `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="61092-142">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="61092-143">Agregue una clase `TodoContext` a la carpeta *Models*:</span><span class="sxs-lookup"><span data-stu-id="61092-143">Add a `TodoContext` class in the *Models* folder:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="61092-144">Adición de un controlador</span><span class="sxs-lookup"><span data-stu-id="61092-144">Add a controller</span></span>

<span data-ttu-id="61092-145">En la carpeta *Controladores*, cree una clase denominada `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="61092-145">In the *Controllers* folder, create a class named `TodoController`.</span></span> <span data-ttu-id="61092-146">Agregue el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="61092-146">Add the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="61092-147">Iniciar la aplicación</span><span class="sxs-lookup"><span data-stu-id="61092-147">Launch the app</span></span>

<span data-ttu-id="61092-148">En VS Code, presione F5 para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="61092-148">In VS Code, press F5 to launch the app.</span></span> <span data-ttu-id="61092-149">Vaya a http://localhost:5000/api/todo (el controlador `Todo` que se acaba de crear).</span><span class="sxs-lookup"><span data-stu-id="61092-149">Navigate to  http://localhost:5000/api/todo   (The `Todo` controller we just created).</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a><span data-ttu-id="61092-150">Ayuda de Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="61092-150">Visual Studio Code help</span></span>

- [<span data-ttu-id="61092-151">Introducción</span><span class="sxs-lookup"><span data-stu-id="61092-151">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="61092-152">Depuración</span><span class="sxs-lookup"><span data-stu-id="61092-152">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="61092-153">Terminal integrado</span><span class="sxs-lookup"><span data-stu-id="61092-153">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="61092-154">Métodos abreviados de teclado</span><span class="sxs-lookup"><span data-stu-id="61092-154">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="61092-155">Métodos abreviados de teclado de Mac</span><span class="sxs-lookup"><span data-stu-id="61092-155">Mac keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="61092-156">Métodos abreviados de teclado de Linux</span><span class="sxs-lookup"><span data-stu-id="61092-156">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="61092-157">Métodos abreviados de teclado de Windows</span><span class="sxs-lookup"><span data-stu-id="61092-157">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]


