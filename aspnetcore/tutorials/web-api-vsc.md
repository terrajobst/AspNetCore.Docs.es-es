---
title: Crear una API web con ASP.NET Core y Visual Studio Code
author: rick-anderson
description: Compilar una API web con ASP.NET Core MVC y Visual Studio Code en macOS, Linux o Windows
ms.author: riande
ms.custom: mvc
ms.date: 07/30/2018
uid: tutorials/web-api-vsc
ms.openlocfilehash: 740110908358a382f20bc1e54e98056296278acf
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2018
ms.locfileid: "50089669"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-code"></a><span data-ttu-id="f1dfc-103">Crear una API web con ASP.NET Core y Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f1dfc-103">Create a Web API with ASP.NET Core and Visual Studio Code</span></span>

<span data-ttu-id="f1dfc-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="f1dfc-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="f1dfc-105">En este tutorial se compila una API web para administrar una lista de tareas pendientes.</span><span class="sxs-lookup"><span data-stu-id="f1dfc-105">In this tutorial, build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="f1dfc-106">No se construye una interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="f1dfc-106">A UI isn't constructed.</span></span>

<span data-ttu-id="f1dfc-107">Hay tres versiones de este tutorial:</span><span class="sxs-lookup"><span data-stu-id="f1dfc-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="f1dfc-108">macOS, Linux y Windows: API Web con Visual Studio Code (este tutorial)</span><span class="sxs-lookup"><span data-stu-id="f1dfc-108">macOS, Linux, Windows: Web API with Visual Studio Code (This tutorial)</span></span>
* <span data-ttu-id="f1dfc-109">macOS: [API Web con Visual Studio para Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="f1dfc-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="f1dfc-110">Windows: [API Web con Visual Studio para Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="f1dfc-110">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="f1dfc-111">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="f1dfc-111">Prerequisites</span></span>

[!INCLUDE[prerequisites](~/includes/net-core-prereqs-vscode.md)]

<span data-ttu-id="f1dfc-112">Vea [Ayuda de Visual Studio Code](#visual-studio-code-help) para obtener sugerencias sobre el uso de VS Code.</span><span class="sxs-lookup"><span data-stu-id="f1dfc-112">See [Visual Studio Code help](#visual-studio-code-help) for tips on using VS Code.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="f1dfc-113">Crear el proyecto</span><span class="sxs-lookup"><span data-stu-id="f1dfc-113">Create the project</span></span>

<span data-ttu-id="f1dfc-114">Desde una consola, ejecute los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="f1dfc-114">From a console, run the following commands:</span></span>

```console
dotnet new webapi -o TodoApi
code TodoApi
```

<span data-ttu-id="f1dfc-115">La carpeta *TodoApi* se abre en Visual Studio Code (VS Code).</span><span class="sxs-lookup"><span data-stu-id="f1dfc-115">The *TodoApi* folder opens in Visual Studio Code (VS Code).</span></span> <span data-ttu-id="f1dfc-116">Seleccione el archivo *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="f1dfc-116">Select the *Startup.cs* file.</span></span>

* <span data-ttu-id="f1dfc-117">Seleccione **Sí** en el mensaje de **advertencia** "Required assets to build and debug are missing from 'TodoApi'.</span><span class="sxs-lookup"><span data-stu-id="f1dfc-117">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'TodoApi'.</span></span> <span data-ttu-id="f1dfc-118">Add them?" (Faltan los activos necesarios para compilar y depurar en 'TodoApi'. ¿Quiere agregarlos?).</span><span class="sxs-lookup"><span data-stu-id="f1dfc-118">Add them?"</span></span>
* <span data-ttu-id="f1dfc-119">Seleccione **Restaurar** en el mensaje de **información** "There are unresolved dependencies" (Hay dependencias no resueltas).</span><span class="sxs-lookup"><span data-stu-id="f1dfc-119">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![VS Code con la advertencia "Required assets to build and debug are missing from 'TodoApi'.](web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="f1dfc-123">Presione **Depurar** (F5) para compilar y ejecutar el programa.</span><span class="sxs-lookup"><span data-stu-id="f1dfc-123">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="f1dfc-124">En un navegador, vaya a http://localhost:5000/api/values.</span><span class="sxs-lookup"><span data-stu-id="f1dfc-124">In a browser, navigate to http://localhost:5000/api/values.</span></span> <span data-ttu-id="f1dfc-125">Se muestra el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="f1dfc-125">The following output is displayed:</span></span>

```json
["value1","value2"]
```



## <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="f1dfc-126">Agregar compatibilidad con Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="f1dfc-126">Add support for Entity Framework Core</span></span>

:::moniker range=">= aspnetcore-2.1"

<span data-ttu-id="f1dfc-127">Al crear un proyecto en ASP.NET Core 2.1 o una versión posterior, se agrega el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) al archivo del proyecto:</span><span class="sxs-lookup"><span data-stu-id="f1dfc-127">Creating a new project in ASP.NET Core 2.1 or later adds the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) to the project file:</span></span>

[!code-xml[](first-web-api/samples/2.1/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]

:::moniker-end

:::moniker range="<= aspnetcore-2.0"

<span data-ttu-id="f1dfc-128">Al crear un proyecto en ASP.NET Core 2.0, se agrega el [metapaquete Microsoft.AspNetCore.All](xref:fundamentals/metapackage) al archivo del proyecto:</span><span class="sxs-lookup"><span data-stu-id="f1dfc-128">Creating a new project in ASP.NET Core 2.0 adds the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) to the project file:</span></span>

[!code-xml[](first-web-api/samples/2.0/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]

:::moniker-end

<span data-ttu-id="f1dfc-129">No es necesario instalar el proveedor de base de datos [Entity Framework Core InMemory](/ef/core/providers/in-memory/) por separado.</span><span class="sxs-lookup"><span data-stu-id="f1dfc-129">There's no need to install the [Entity Framework Core InMemory](/ef/core/providers/in-memory/) database provider separately.</span></span> <span data-ttu-id="f1dfc-130">Este proveedor de base de datos permite usar Entity Framework Core con una base de datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="f1dfc-130">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="f1dfc-131">Agregar una clase de modelo</span><span class="sxs-lookup"><span data-stu-id="f1dfc-131">Add a model class</span></span>

<span data-ttu-id="f1dfc-132">Un modelo es un objeto que representa los datos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f1dfc-132">A model is an object representing the data in your app.</span></span> <span data-ttu-id="f1dfc-133">En este caso, el único modelo es una tarea pendiente.</span><span class="sxs-lookup"><span data-stu-id="f1dfc-133">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="f1dfc-134">Agregue una carpeta denominada *Models*.</span><span class="sxs-lookup"><span data-stu-id="f1dfc-134">Add a folder named *Models*.</span></span> <span data-ttu-id="f1dfc-135">Puede colocar clases de modelo en cualquier lugar del proyecto, pero la carpeta *Models* se usa por convención.</span><span class="sxs-lookup"><span data-stu-id="f1dfc-135">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="f1dfc-136">Agregue una clase `TodoItem` con el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="f1dfc-136">Add a `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="f1dfc-137">La base de datos genera el `Id` cuando se crea `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="f1dfc-137">The database generates the `Id` when a `TodoItem` is created.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="f1dfc-138">Crear el contexto de base de datos</span><span class="sxs-lookup"><span data-stu-id="f1dfc-138">Create the database context</span></span>

<span data-ttu-id="f1dfc-139">El *contexto de base de datos* es la clase principal que coordina la funcionalidad de Entity Framework para un modelo de datos determinado.</span><span class="sxs-lookup"><span data-stu-id="f1dfc-139">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="f1dfc-140">Esta clase se crea al derivar de la clase `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="f1dfc-140">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="f1dfc-141">Agregue una clase `TodoContext` a la carpeta *Models*:</span><span class="sxs-lookup"><span data-stu-id="f1dfc-141">Add a `TodoContext` class in the *Models* folder:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="f1dfc-142">Adición de un controlador</span><span class="sxs-lookup"><span data-stu-id="f1dfc-142">Add a controller</span></span>

<span data-ttu-id="f1dfc-143">En la carpeta *Controladores*, cree una clase denominada `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="f1dfc-143">In the *Controllers* folder, create a class named `TodoController`.</span></span> <span data-ttu-id="f1dfc-144">Reemplace el contenido por el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="f1dfc-144">Replace its contents with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="f1dfc-145">Iniciar la aplicación</span><span class="sxs-lookup"><span data-stu-id="f1dfc-145">Launch the app</span></span>

<span data-ttu-id="f1dfc-146">En VS Code, presione F5 para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f1dfc-146">In VS Code, press F5 to launch the app.</span></span> <span data-ttu-id="f1dfc-147">Vaya a http://localhost:5000/api/todo (el controlador `Todo` que se acaba de crear).</span><span class="sxs-lookup"><span data-stu-id="f1dfc-147">Navigate to http://localhost:5000/api/todo (the `Todo` controller we created).</span></span>

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a><span data-ttu-id="f1dfc-148">Ayuda de Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f1dfc-148">Visual Studio Code help</span></span>

* [<span data-ttu-id="f1dfc-149">Introducción</span><span class="sxs-lookup"><span data-stu-id="f1dfc-149">Getting started</span></span>](https://code.visualstudio.com/docs)
* [<span data-ttu-id="f1dfc-150">Depuración</span><span class="sxs-lookup"><span data-stu-id="f1dfc-150">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
* [<span data-ttu-id="f1dfc-151">Terminal integrado</span><span class="sxs-lookup"><span data-stu-id="f1dfc-151">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
* [<span data-ttu-id="f1dfc-152">Métodos abreviados de teclado</span><span class="sxs-lookup"><span data-stu-id="f1dfc-152">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  * [<span data-ttu-id="f1dfc-153">Funciones rápidas de teclado de macOS</span><span class="sxs-lookup"><span data-stu-id="f1dfc-153">macOS keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  * [<span data-ttu-id="f1dfc-154">Métodos abreviados de teclado de Linux</span><span class="sxs-lookup"><span data-stu-id="f1dfc-154">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  * [<span data-ttu-id="f1dfc-155">Métodos abreviados de teclado de Windows</span><span class="sxs-lookup"><span data-stu-id="f1dfc-155">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]
