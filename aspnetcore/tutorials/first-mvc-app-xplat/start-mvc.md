---
title: Introducción a ASP.NET Core MVC en macOS, Linux o Windows
author: rick-anderson
description: Obtenga información sobre cómo empezar a usar ASP.NET Core MVC y Visual Studio Code en macOS, Linux y Windows.
manager: wpickett
ms.author: riande
ms.date: 07/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-xplat/start-mvc
ms.openlocfilehash: 50fbd54c6b0cc1146271afda7e45a0dab590dd7d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
# <a name="introduction-to-aspnet-core-mvc-on-macos-linux-or-windows"></a><span data-ttu-id="d5dd0-103">Introducción a ASP.NET Core MVC en macOS, Linux o Windows</span><span class="sxs-lookup"><span data-stu-id="d5dd0-103">Introduction to ASP.NET Core MVC on macOS, Linux, or Windows</span></span>

<span data-ttu-id="d5dd0-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d5dd0-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d5dd0-105">En este tutorial aprenderá los aspectos básicos de la creación de una aplicación web de ASP.NET Core MVC con [Visual Studio Code](https://code.visualstudio.com) (VS Code).</span><span class="sxs-lookup"><span data-stu-id="d5dd0-105">This tutorial will teach you the basics of building an ASP.NET Core MVC web app using [Visual Studio Code](https://code.visualstudio.com) (VS Code).</span></span> <span data-ttu-id="d5dd0-106">Para realizar el tutorial debe estar familiarizado con VS Code.</span><span class="sxs-lookup"><span data-stu-id="d5dd0-106">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="d5dd0-107">Para más información, vea [Getting started with VS Code](https://code.visualstudio.com/docs) (Introducción a VS Code) y [Visual Studio Code help](#visual-studio-code-help) (Ayuda de Visual Studio Code).</span><span class="sxs-lookup"><span data-stu-id="d5dd0-107">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span> 

[!INCLUDE [consider RP](../../includes/razor.md)]

<span data-ttu-id="d5dd0-108">Hay tres versiones de este tutorial:</span><span class="sxs-lookup"><span data-stu-id="d5dd0-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="d5dd0-109">macOS: [Creación de una aplicación de ASP.NET Core MVC con Visual Studio para Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="d5dd0-109">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="d5dd0-110">Windows: [Creación de una aplicación de ASP.NET Core MVC con Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="d5dd0-110">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="d5dd0-111">macOS, Linux y Windows: [Creación de una aplicación de ASP.NET Core MVC con Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="d5dd0-111">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="d5dd0-112">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="d5dd0-112">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-a-web-app-with-dotnet"></a><span data-ttu-id="d5dd0-113">Creación de una aplicación web con dotnet</span><span class="sxs-lookup"><span data-stu-id="d5dd0-113">Create a web app with dotnet</span></span>

<span data-ttu-id="d5dd0-114">Desde un terminal, ejecute estos comandos:</span><span class="sxs-lookup"><span data-stu-id="d5dd0-114">From a terminal, run the following commands:</span></span>

```console
mkdir MvcMovie
cd MvcMovie
dotnet new mvc
```

<span data-ttu-id="d5dd0-115">Abra la carpeta *MvcMovie* en Visual Studio Code (VS Code) y seleccione el archivo *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="d5dd0-115">Open the *MvcMovie* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="d5dd0-116">Seleccione **Sí** en el mensaje de **advertencia** "Required assets to build and debug are missing from 'MvcMovie'. Add them?" (Faltan los activos necesarios para compilar y depurar en 'MvcMovie'.</span><span class="sxs-lookup"><span data-stu-id="d5dd0-116">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'MvcMovie'.</span></span> <span data-ttu-id="d5dd0-117">¿Desea agregarlos?).</span><span class="sxs-lookup"><span data-stu-id="d5dd0-117">Add them?"</span></span>
- <span data-ttu-id="d5dd0-118">Seleccione **Restaurar** en el mensaje de **información** "There are unresolved dependencies" (Hay dependencias no resueltas).</span><span class="sxs-lookup"><span data-stu-id="d5dd0-118">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

![VS Code con la advertencia "Required assets to build and debug are missing from 'MvcMovie'. Add them?" (Faltan los activos necesarios para compilar y depurar en 'MvcMovie'.](../web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="d5dd0-122">Presione **Depurar** (F5) para compilar y ejecutar el programa.</span><span class="sxs-lookup"><span data-stu-id="d5dd0-122">Press **Debug** (F5) to build and run the program.</span></span>

![aplicación en ejecución](../first-mvc-app/start-mvc/_static/1.png)

<span data-ttu-id="d5dd0-124">VS Code inicia el servidor web [Kestrel](xref:fundamentals/servers/kestrel) y ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d5dd0-124">VS Code starts the [Kestrel](xref:fundamentals/servers/kestrel) web server and runs your app.</span></span> <span data-ttu-id="d5dd0-125">Tenga en cuenta que la barra de direcciones muestra `localhost:5000` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="d5dd0-125">Notice that the address bar shows `localhost:5000` and not something like `example.com`.</span></span> <span data-ttu-id="d5dd0-126">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="d5dd0-126">That's because `localhost` is the standard hostname for your local computer.</span></span>

<span data-ttu-id="d5dd0-127">La plantilla predeterminada proporciona los vínculos **Inicio, Acerca de** y **Contacto**, totalmente funcionales.</span><span class="sxs-lookup"><span data-stu-id="d5dd0-127">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="d5dd0-128">En la imagen del explorador anterior no se muestran estos vínculos.</span><span class="sxs-lookup"><span data-stu-id="d5dd0-128">The browser image above doesn't show these links.</span></span> <span data-ttu-id="d5dd0-129">Según el tamaño del explorador, tendrá que hacer clic en el icono de navegación para que se muestren.</span><span class="sxs-lookup"><span data-stu-id="d5dd0-129">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![icono de navegación en la esquina superior derecha](../first-mvc-app/start-mvc/_static/2.png)

<span data-ttu-id="d5dd0-131">En la siguiente sección de este tutorial obtendrá información sobre MVC y empezará a escribir código.</span><span class="sxs-lookup"><span data-stu-id="d5dd0-131">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

## <a name="visual-studio-code-help"></a><span data-ttu-id="d5dd0-132">Ayuda de Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d5dd0-132">Visual Studio Code help</span></span>

- [<span data-ttu-id="d5dd0-133">Introducción</span><span class="sxs-lookup"><span data-stu-id="d5dd0-133">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="d5dd0-134">Depuración</span><span class="sxs-lookup"><span data-stu-id="d5dd0-134">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="d5dd0-135">Terminal integrado</span><span class="sxs-lookup"><span data-stu-id="d5dd0-135">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="d5dd0-136">Métodos abreviados de teclado</span><span class="sxs-lookup"><span data-stu-id="d5dd0-136">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="d5dd0-137">Funciones rápidas de teclado de macOS</span><span class="sxs-lookup"><span data-stu-id="d5dd0-137">macOS keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="d5dd0-138">Métodos abreviados de teclado de Linux</span><span class="sxs-lookup"><span data-stu-id="d5dd0-138">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - <span data-ttu-id="d5dd0-139">[Windows keyboard shortcuts](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf) (Métodos abreviados de teclado de Windows)</span><span class="sxs-lookup"><span data-stu-id="d5dd0-139">[Windows keyboard shortcuts](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d5dd0-140">[Next - Add a controller](adding-controller.md) (Siguiente - Agregar un controlador)</span><span class="sxs-lookup"><span data-stu-id="d5dd0-140">[Next - Add a controller](adding-controller.md)</span></span>
