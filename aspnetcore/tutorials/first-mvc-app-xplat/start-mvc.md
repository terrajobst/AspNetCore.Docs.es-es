---
title: "Introducción a ASP.NET Core MVC en Mac, Linux o Windows"
author: rick-anderson
description: "Introducción a ASP.NET Core MVC y Visual Studio Code en Mac, Linux y Windows"
keywords: ASP.NET Core, MVC, VS Code, Visual Studio Code,Mac,Linux,Windows
ms.author: riande
manager: wpickett
ms.date: 07/07/2017
ms.topic: get-started-article
ms.assetid: 1d18b589-1638-4dc6-1638-fb0f41998d78
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-xplat/start-mvc
ms.openlocfilehash: 87ce5dca011a7bc37d34799db159d933c158cba1
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/22/2017
---
# <a name="getting-started-with-aspnet-core-mvc--on-mac-linux-or-windows"></a><span data-ttu-id="07ece-104">Introducción a ASP.NET Core MVC en Mac, Linux o Windows</span><span class="sxs-lookup"><span data-stu-id="07ece-104">Getting started with ASP.NET Core MVC  on Mac, Linux, or Windows</span></span>

<span data-ttu-id="07ece-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="07ece-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="07ece-106">En este tutorial aprenderá los aspectos básicos de la creación de una aplicación web de ASP.NET Core MVC con [Visual Studio Code](https://code.visualstudio.com) (VS Code).</span><span class="sxs-lookup"><span data-stu-id="07ece-106">This tutorial will teach you the basics of building an ASP.NET Core MVC web app using [Visual Studio Code](https://code.visualstudio.com) (VS Code).</span></span> <span data-ttu-id="07ece-107">Para realizar el tutorial debe estar familiarizado con VS Code.</span><span class="sxs-lookup"><span data-stu-id="07ece-107">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="07ece-108">Para más información, vea [Getting started with VS Code](https://code.visualstudio.com/docs) (Introducción a VS Code) y [Visual Studio Code help](#visual-studio-code-help) (Ayuda de Visual Studio Code).</span><span class="sxs-lookup"><span data-stu-id="07ece-108">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span> [!INCLUDE[consider RP](../../includes/razor.md)]

<span data-ttu-id="07ece-109">Hay tres versiones de este tutorial:</span><span class="sxs-lookup"><span data-stu-id="07ece-109">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="07ece-110">macOS: [Creación de una aplicación de ASP.NET Core MVC con Visual Studio para Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="07ece-110">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="07ece-111">Windows: [Creación de una aplicación de ASP.NET Core MVC con Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="07ece-111">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="07ece-112">macOS, Linux y Windows: [Creación de una aplicación de ASP.NET Core MVC con Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="07ece-112">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span> 

## <a name="install-vs-code-and-net-core"></a><span data-ttu-id="07ece-113">Instalación de VS Code y .NET Core</span><span class="sxs-lookup"><span data-stu-id="07ece-113">Install VS Code and .NET Core</span></span>

<span data-ttu-id="07ece-114">Para realizar este tutorial se necesita el [SDK de .NET Core 2.0.0](https://www.microsoft.com/net/core) o una versión posterior.</span><span class="sxs-lookup"><span data-stu-id="07ece-114">This tutorial requires the [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span> <span data-ttu-id="07ece-115">Vea [este PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) para la versión 1.1 de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="07ece-115">See [the pdf](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) for the ASP.NET Core 1.1 version.</span></span>

<span data-ttu-id="07ece-116">Instale el software siguiente:</span><span class="sxs-lookup"><span data-stu-id="07ece-116">Install the following:</span></span>

* <span data-ttu-id="07ece-117">[SDK de .NET Core 2.0.0](https://www.microsoft.com/net/core) o versiones posteriores</span><span class="sxs-lookup"><span data-stu-id="07ece-117">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>
* [<span data-ttu-id="07ece-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="07ece-118">Visual Studio Code</span></span>](https://code.visualstudio.com)
* <span data-ttu-id="07ece-119">[Extensión de C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) de VS Code</span><span class="sxs-lookup"><span data-stu-id="07ece-119">VS Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span> 

## <a name="create-a-web-app-with-dotnet"></a><span data-ttu-id="07ece-120">Creación de una aplicación web con dotnet</span><span class="sxs-lookup"><span data-stu-id="07ece-120">Create a web app with dotnet</span></span>

<span data-ttu-id="07ece-121">Desde un terminal, ejecute estos comandos:</span><span class="sxs-lookup"><span data-stu-id="07ece-121">From a terminal, run the following commands:</span></span>

```console
mkdir MvcMovie
cd MvcMovie
dotnet new mvc
```

<span data-ttu-id="07ece-122">Abra la carpeta *MvcMovie* en Visual Studio Code (VS Code) y seleccione el archivo *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="07ece-122">Open the *MvcMovie* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="07ece-123">Seleccione **Sí** en el mensaje de **advertencia** "Required assets to build and debug are missing from 'MvcMovie'. Add them?" (Faltan los activos necesarios para compilar y depurar en 'MvcMovie'.</span><span class="sxs-lookup"><span data-stu-id="07ece-123">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'MvcMovie'.</span></span> <span data-ttu-id="07ece-124">¿Desea agregarlos?).</span><span class="sxs-lookup"><span data-stu-id="07ece-124">Add them?"</span></span>
- <span data-ttu-id="07ece-125">Seleccione **Restaurar** en el mensaje de **información** "There are unresolved dependencies" (Hay dependencias no resueltas).</span><span class="sxs-lookup"><span data-stu-id="07ece-125">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

![VS Code con la advertencia "Required assets to build and debug are missing from 'MvcMovie'. Add them?" (Faltan los activos necesarios para compilar y depurar en 'MvcMovie'.](../web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="07ece-129">Presione **Depurar** (F5) para compilar y ejecutar el programa.</span><span class="sxs-lookup"><span data-stu-id="07ece-129">Press **Debug** (F5) to build and run the program.</span></span>

![aplicación en ejecución](../first-mvc-app/start-mvc/_static/1.png)

<span data-ttu-id="07ece-131">VS Code inicia el servidor web [Kestrel](xref:fundamentals/servers/kestrel) y ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="07ece-131">VS Code starts the [Kestrel](xref:fundamentals/servers/kestrel) web server and runs your app.</span></span> <span data-ttu-id="07ece-132">Tenga en cuenta que la barra de direcciones muestra `localhost:5000` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="07ece-132">Notice that the address bar shows `localhost:5000` and not something like `example.com`.</span></span> <span data-ttu-id="07ece-133">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="07ece-133">That's because `localhost` is the standard hostname for your local computer.</span></span>

<span data-ttu-id="07ece-134">La plantilla predeterminada proporciona los vínculos **Inicio, Acerca de** y **Contacto**, totalmente funcionales.</span><span class="sxs-lookup"><span data-stu-id="07ece-134">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="07ece-135">En la imagen del explorador anterior no se muestran estos vínculos.</span><span class="sxs-lookup"><span data-stu-id="07ece-135">The browser image above doesn't show these links.</span></span> <span data-ttu-id="07ece-136">Según el tamaño del explorador, tendrá que hacer clic en el icono de navegación para que se muestren.</span><span class="sxs-lookup"><span data-stu-id="07ece-136">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![icono de navegación en la esquina superior derecha](../first-mvc-app/start-mvc/_static/2.png)

<span data-ttu-id="07ece-138">En la siguiente sección de este tutorial obtendrá información sobre MVC y empezará a escribir código.</span><span class="sxs-lookup"><span data-stu-id="07ece-138">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

## <a name="visual-studio-code-help"></a><span data-ttu-id="07ece-139">Ayuda de Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="07ece-139">Visual Studio Code help</span></span>

- [<span data-ttu-id="07ece-140">Introducción</span><span class="sxs-lookup"><span data-stu-id="07ece-140">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="07ece-141">Depuración</span><span class="sxs-lookup"><span data-stu-id="07ece-141">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="07ece-142">Terminal integrado</span><span class="sxs-lookup"><span data-stu-id="07ece-142">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="07ece-143">Métodos abreviados de teclado</span><span class="sxs-lookup"><span data-stu-id="07ece-143">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="07ece-144">Métodos abreviados de teclado de Mac</span><span class="sxs-lookup"><span data-stu-id="07ece-144">Mac keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="07ece-145">Métodos abreviados de teclado de Linux</span><span class="sxs-lookup"><span data-stu-id="07ece-145">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - <span data-ttu-id="07ece-146">[Windows keyboard shortcuts](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf) (Métodos abreviados de teclado de Windows)</span><span class="sxs-lookup"><span data-stu-id="07ece-146">[Windows keyboard shortcuts](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="07ece-147">[Next - Add a controller](adding-controller.md) (Siguiente - Agregar un controlador)</span><span class="sxs-lookup"><span data-stu-id="07ece-147">[Next - Add a controller](adding-controller.md)</span></span>
