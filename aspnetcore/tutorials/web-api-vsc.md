---
title: Crear una API web con ASP.NET Core y Visual Studio Code
author: rick-anderson
description: Compilar una API web con ASP.NET Core MVC y Visual Studio Code en macOS, Linux o Windows
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/08/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/web-api-vsc
ms.openlocfilehash: 9fac4d7b3f687881eafbd63ee71f99bff3b27183
ms.sourcegitcommit: c867d7427bd4a88a78b2322e156367733b532730
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/09/2018
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-code"></a><span data-ttu-id="2be8a-103">Crear una API web con ASP.NET Core y Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2be8a-103">Create a Web API with ASP.NET Core and Visual Studio Code</span></span>

<span data-ttu-id="2be8a-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="2be8a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="2be8a-105">En este tutorial se compila una API web para administrar una lista de tareas pendientes.</span><span class="sxs-lookup"><span data-stu-id="2be8a-105">In this tutorial, build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="2be8a-106">No se construye una interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="2be8a-106">A UI isn't constructed.</span></span>

<span data-ttu-id="2be8a-107">Hay tres versiones de este tutorial:</span><span class="sxs-lookup"><span data-stu-id="2be8a-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="2be8a-108">macOS, Linux y Windows: API Web con Visual Studio Code (este tutorial)</span><span class="sxs-lookup"><span data-stu-id="2be8a-108">macOS, Linux, Windows: Web API with Visual Studio Code (This tutorial)</span></span>
* <span data-ttu-id="2be8a-109">macOS: [API Web con Visual Studio para Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="2be8a-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="2be8a-110">Windows: [API Web con Visual Studio para Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="2be8a-110">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="2be8a-111">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="2be8a-111">Prerequisites</span></span>

[!INCLUDE[prerequisites](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-the-project"></a><span data-ttu-id="2be8a-112">Crear el proyecto</span><span class="sxs-lookup"><span data-stu-id="2be8a-112">Create the project</span></span>

<span data-ttu-id="2be8a-113">Desde una consola, ejecute los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="2be8a-113">From a console, run the following commands:</span></span>

```console
dotnet new webapi -o TodoApi
code TodoApi
```

<span data-ttu-id="2be8a-114">La carpeta *TodoApi* se abre en Visual Studio Code (VS Code).</span><span class="sxs-lookup"><span data-stu-id="2be8a-114">The *TodoApi* folder opens in Visual Studio Code (VS Code).</span></span> <span data-ttu-id="2be8a-115">Seleccione el archivo *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="2be8a-115">Select the *Startup.cs* file.</span></span>

* <span data-ttu-id="2be8a-116">Seleccione **Sí** en el mensaje de **advertencia** "Required assets to build and debug are missing from 'TodoApi'.</span><span class="sxs-lookup"><span data-stu-id="2be8a-116">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'TodoApi'.</span></span> <span data-ttu-id="2be8a-117">Add them?" (Faltan los activos necesarios para compilar y depurar en 'TodoApi'. ¿Quiere agregarlos?).</span><span class="sxs-lookup"><span data-stu-id="2be8a-117">Add them?"</span></span>
* <span data-ttu-id="2be8a-118">Seleccione **Restaurar** en el mensaje de **información** "There are unresolved dependencies" (Hay dependencias no resueltas).</span><span class="sxs-lookup"><span data-stu-id="2be8a-118">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![VS Code con la advertencia "Required assets to build and debug are missing from 'TodoApi'.](web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="2be8a-122">Presione **Depurar** (F5) para compilar y ejecutar el programa.</span><span class="sxs-lookup"><span data-stu-id="2be8a-122">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="2be8a-123">En un navegador, vaya a http://localhost:5000/api/values.</span><span class="sxs-lookup"><span data-stu-id="2be8a-123">In a browser, navigate to http://localhost:5000/api/values.</span></span> <span data-ttu-id="2be8a-124">Se muestra el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="2be8a-124">The following output is displayed:</span></span>

```json
["value1","value2"]
```

<span data-ttu-id="2be8a-125">Vea [Ayuda de Visual Studio Code](#visual-studio-code-help) para obtener sugerencias sobre el uso de VS Code.</span><span class="sxs-lookup"><span data-stu-id="2be8a-125">See [Visual Studio Code help](#visual-studio-code-help) for tips on using VS Code.</span></span>

## <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="2be8a-126">Agregar compatibilidad con Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="2be8a-126">Add support for Entity Framework Core</span></span>

:::moniker range="<= aspnetcore-2.0"
<span data-ttu-id="2be8a-127">Al crear un proyecto en ASP.NET Core 2.0, se agrega la referencia de paquete [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) al archivo *TodoApi.csproj*:</span><span class="sxs-lookup"><span data-stu-id="2be8a-127">Creating a new project in ASP.NET Core 2.0 adds the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) package reference to the *TodoApi.csproj* file:</span></span>

<span data-ttu-id="2be8a-128">[!code-xml[](first-web-api/samples/2.0/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="2be8a-128">[!code-xml[](first-web-api/samples/2.0/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]</span></span>
:::moniker-end
:::moniker range=">= aspnetcore-2.1"
<span data-ttu-id="2be8a-129">Al crear un proyecto en ASP.NET Core 2.1 o posterior, se agrega la referencia de paquete [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) al archivo *TodoApi.csproj*:</span><span class="sxs-lookup"><span data-stu-id="2be8a-129">Creating a new project in ASP.NET Core 2.1 or later adds the [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) package reference to the *TodoApi.csproj* file:</span></span>

<span data-ttu-id="2be8a-130">[!code-xml[](first-web-api/samples/2.1/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="2be8a-130">[!code-xml[](first-web-api/samples/2.1/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]</span></span>
:::moniker-end

<span data-ttu-id="2be8a-131">No es necesario instalar el proveedor de base de datos [Entity Framework Core InMemory](/ef/core/providers/in-memory/) por separado.</span><span class="sxs-lookup"><span data-stu-id="2be8a-131">There's no need to install the [Entity Framework Core InMemory](/ef/core/providers/in-memory/) database provider separately.</span></span> <span data-ttu-id="2be8a-132">Este proveedor de base de datos permite usar Entity Framework Core con una base de datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="2be8a-132">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="2be8a-133">Agregar una clase de modelo</span><span class="sxs-lookup"><span data-stu-id="2be8a-133">Add a model class</span></span>

<span data-ttu-id="2be8a-134">Un modelo es un objeto que representa los datos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2be8a-134">A model is an object representing the data in your app.</span></span> <span data-ttu-id="2be8a-135">En este caso, el único modelo es una tarea pendiente.</span><span class="sxs-lookup"><span data-stu-id="2be8a-135">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="2be8a-136">Agregue una carpeta denominada *Models*.</span><span class="sxs-lookup"><span data-stu-id="2be8a-136">Add a folder named *Models*.</span></span> <span data-ttu-id="2be8a-137">Puede colocar clases de modelo en cualquier lugar del proyecto, pero la carpeta *Models* se usa por convención.</span><span class="sxs-lookup"><span data-stu-id="2be8a-137">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="2be8a-138">Agregue una clase `TodoItem` con el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="2be8a-138">Add a `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="2be8a-139">La base de datos genera el `Id` cuando se crea `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="2be8a-139">The database generates the `Id` when a `TodoItem` is created.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="2be8a-140">Crear el contexto de base de datos</span><span class="sxs-lookup"><span data-stu-id="2be8a-140">Create the database context</span></span>

<span data-ttu-id="2be8a-141">El *contexto de base de datos* es la clase principal que coordina la funcionalidad de Entity Framework para un modelo de datos determinado.</span><span class="sxs-lookup"><span data-stu-id="2be8a-141">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="2be8a-142">Esta clase se crea al derivar de la clase `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="2be8a-142">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="2be8a-143">Agregue una clase `TodoContext` a la carpeta *Models*:</span><span class="sxs-lookup"><span data-stu-id="2be8a-143">Add a `TodoContext` class in the *Models* folder:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="2be8a-144">Adición de un controlador</span><span class="sxs-lookup"><span data-stu-id="2be8a-144">Add a controller</span></span>

<span data-ttu-id="2be8a-145">En la carpeta *Controladores*, cree una clase denominada `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="2be8a-145">In the *Controllers* folder, create a class named `TodoController`.</span></span> <span data-ttu-id="2be8a-146">Reemplace el contenido por el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="2be8a-146">Replace its contents with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="2be8a-147">Iniciar la aplicación</span><span class="sxs-lookup"><span data-stu-id="2be8a-147">Launch the app</span></span>

<span data-ttu-id="2be8a-148">En VS Code, presione F5 para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2be8a-148">In VS Code, press F5 to launch the app.</span></span> <span data-ttu-id="2be8a-149">Vaya a http://localhost:5000/api/todo (el controlador `Todo` que se acaba de crear).</span><span class="sxs-lookup"><span data-stu-id="2be8a-149">Navigate to http://localhost:5000/api/todo (the `Todo` controller we created).</span></span>

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a><span data-ttu-id="2be8a-150">Ayuda de Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2be8a-150">Visual Studio Code help</span></span>

* [<span data-ttu-id="2be8a-151">Introducción</span><span class="sxs-lookup"><span data-stu-id="2be8a-151">Getting started</span></span>](https://code.visualstudio.com/docs)
* [<span data-ttu-id="2be8a-152">Depuración</span><span class="sxs-lookup"><span data-stu-id="2be8a-152">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
* [<span data-ttu-id="2be8a-153">Terminal integrado</span><span class="sxs-lookup"><span data-stu-id="2be8a-153">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
* [<span data-ttu-id="2be8a-154">Métodos abreviados de teclado</span><span class="sxs-lookup"><span data-stu-id="2be8a-154">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  * [<span data-ttu-id="2be8a-155">Funciones rápidas de teclado de macOS</span><span class="sxs-lookup"><span data-stu-id="2be8a-155">macOS keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  * [<span data-ttu-id="2be8a-156">Métodos abreviados de teclado de Linux</span><span class="sxs-lookup"><span data-stu-id="2be8a-156">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  * [<span data-ttu-id="2be8a-157">Métodos abreviados de teclado de Windows</span><span class="sxs-lookup"><span data-stu-id="2be8a-157">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]
