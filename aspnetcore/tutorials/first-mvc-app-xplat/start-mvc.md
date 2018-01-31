---
title: "Introducción a ASP.NET Core MVC en Mac, Linux o Windows"
author: rick-anderson
description: "Introducción a ASP.NET Core MVC y Visual Studio Code en Mac, Linux y Windows"
manager: wpickett
ms.author: riande
ms.date: 07/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-xplat/start-mvc
ms.openlocfilehash: 4771555b66f328a819f17a32eb3959f9ecf33d44
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="getting-started-with-aspnet-core-mvc--on-mac-linux-or-windows"></a><span data-ttu-id="c1539-103">Introducción a ASP.NET Core MVC en Mac, Linux o Windows</span><span class="sxs-lookup"><span data-stu-id="c1539-103">Getting started with ASP.NET Core MVC  on Mac, Linux, or Windows</span></span>

<span data-ttu-id="c1539-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c1539-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c1539-105">En este tutorial aprenderá los aspectos básicos de la creación de una aplicación web de ASP.NET Core MVC con [Visual Studio Code](https://code.visualstudio.com) (VS Code).</span><span class="sxs-lookup"><span data-stu-id="c1539-105">This tutorial will teach you the basics of building an ASP.NET Core MVC web app using [Visual Studio Code](https://code.visualstudio.com) (VS Code).</span></span> <span data-ttu-id="c1539-106">Para realizar el tutorial debe estar familiarizado con VS Code.</span><span class="sxs-lookup"><span data-stu-id="c1539-106">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="c1539-107">Para más información, vea [Getting started with VS Code](https://code.visualstudio.com/docs) (Introducción a VS Code) y [Visual Studio Code help](#visual-studio-code-help) (Ayuda de Visual Studio Code).</span><span class="sxs-lookup"><span data-stu-id="c1539-107">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span> 

[!INCLUDE[consider RP](../../includes/razor.md)]

<span data-ttu-id="c1539-108">Hay tres versiones de este tutorial:</span><span class="sxs-lookup"><span data-stu-id="c1539-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="c1539-109">macOS: [Creación de una aplicación de ASP.NET Core MVC con Visual Studio para Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="c1539-109">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="c1539-110">Windows: [Creación de una aplicación de ASP.NET Core MVC con Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="c1539-110">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="c1539-111">macOS, Linux y Windows: [Creación de una aplicación de ASP.NET Core MVC con Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="c1539-111">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span> 

## <a name="install-vs-code-and-net-core"></a><span data-ttu-id="c1539-112">Instalación de VS Code y .NET Core</span><span class="sxs-lookup"><span data-stu-id="c1539-112">Install VS Code and .NET Core</span></span>

<span data-ttu-id="c1539-113">Para realizar este tutorial se necesita el [SDK de .NET Core 2.0.0](https://www.microsoft.com/net/core) o una versión posterior.</span><span class="sxs-lookup"><span data-stu-id="c1539-113">This tutorial requires the [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span> <span data-ttu-id="c1539-114">Vea [este PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) para la versión 1.1 de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c1539-114">See [the pdf](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) for the ASP.NET Core 1.1 version.</span></span>

<span data-ttu-id="c1539-115">Instale el software siguiente:</span><span class="sxs-lookup"><span data-stu-id="c1539-115">Install the following:</span></span>

* <span data-ttu-id="c1539-116">[SDK de .NET Core 2.0.0](https://www.microsoft.com/net/core) o versiones posteriores</span><span class="sxs-lookup"><span data-stu-id="c1539-116">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>
* [<span data-ttu-id="c1539-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c1539-117">Visual Studio Code</span></span>](https://code.visualstudio.com)
* <span data-ttu-id="c1539-118">[Extensión de C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) de VS Code</span><span class="sxs-lookup"><span data-stu-id="c1539-118">VS Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span> 

## <a name="create-a-web-app-with-dotnet"></a><span data-ttu-id="c1539-119">Creación de una aplicación web con dotnet</span><span class="sxs-lookup"><span data-stu-id="c1539-119">Create a web app with dotnet</span></span>

<span data-ttu-id="c1539-120">Desde un terminal, ejecute estos comandos:</span><span class="sxs-lookup"><span data-stu-id="c1539-120">From a terminal, run the following commands:</span></span>

```console
mkdir MvcMovie
cd MvcMovie
dotnet new mvc
```

<span data-ttu-id="c1539-121">Abra la carpeta *MvcMovie* en Visual Studio Code (VS Code) y seleccione el archivo *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="c1539-121">Open the *MvcMovie* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="c1539-122">Seleccione **Sí** en el mensaje de **advertencia** "Required assets to build and debug are missing from 'MvcMovie'. Add them?" (Faltan los activos necesarios para compilar y depurar en 'MvcMovie'.</span><span class="sxs-lookup"><span data-stu-id="c1539-122">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'MvcMovie'.</span></span> <span data-ttu-id="c1539-123">¿Desea agregarlos?).</span><span class="sxs-lookup"><span data-stu-id="c1539-123">Add them?"</span></span>
- <span data-ttu-id="c1539-124">Seleccione **Restaurar** en el mensaje de **información** "There are unresolved dependencies" (Hay dependencias no resueltas).</span><span class="sxs-lookup"><span data-stu-id="c1539-124">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

![VS Code con la advertencia "Required assets to build and debug are missing from 'MvcMovie'. Add them?" (Faltan los activos necesarios para compilar y depurar en 'MvcMovie'.](../web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="c1539-128">Presione **Depurar** (F5) para compilar y ejecutar el programa.</span><span class="sxs-lookup"><span data-stu-id="c1539-128">Press **Debug** (F5) to build and run the program.</span></span>

![aplicación en ejecución](../first-mvc-app/start-mvc/_static/1.png)

<span data-ttu-id="c1539-130">VS Code inicia el servidor web [Kestrel](xref:fundamentals/servers/kestrel) y ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c1539-130">VS Code starts the [Kestrel](xref:fundamentals/servers/kestrel) web server and runs your app.</span></span> <span data-ttu-id="c1539-131">Tenga en cuenta que la barra de direcciones muestra `localhost:5000` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="c1539-131">Notice that the address bar shows `localhost:5000` and not something like `example.com`.</span></span> <span data-ttu-id="c1539-132">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="c1539-132">That's because `localhost` is the standard hostname for your local computer.</span></span>

<span data-ttu-id="c1539-133">La plantilla predeterminada proporciona los vínculos **Inicio, Acerca de** y **Contacto**, totalmente funcionales.</span><span class="sxs-lookup"><span data-stu-id="c1539-133">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="c1539-134">En la imagen del explorador anterior no se muestran estos vínculos.</span><span class="sxs-lookup"><span data-stu-id="c1539-134">The browser image above doesn't show these links.</span></span> <span data-ttu-id="c1539-135">Según el tamaño del explorador, tendrá que hacer clic en el icono de navegación para que se muestren.</span><span class="sxs-lookup"><span data-stu-id="c1539-135">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![icono de navegación en la esquina superior derecha](../first-mvc-app/start-mvc/_static/2.png)

<span data-ttu-id="c1539-137">En la siguiente sección de este tutorial obtendrá información sobre MVC y empezará a escribir código.</span><span class="sxs-lookup"><span data-stu-id="c1539-137">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

## <a name="visual-studio-code-help"></a><span data-ttu-id="c1539-138">Ayuda de Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c1539-138">Visual Studio Code help</span></span>

- [<span data-ttu-id="c1539-139">Introducción</span><span class="sxs-lookup"><span data-stu-id="c1539-139">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="c1539-140">Depuración</span><span class="sxs-lookup"><span data-stu-id="c1539-140">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="c1539-141">Terminal integrado</span><span class="sxs-lookup"><span data-stu-id="c1539-141">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="c1539-142">Métodos abreviados de teclado</span><span class="sxs-lookup"><span data-stu-id="c1539-142">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="c1539-143">Métodos abreviados de teclado de Mac</span><span class="sxs-lookup"><span data-stu-id="c1539-143">Mac keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="c1539-144">Métodos abreviados de teclado de Linux</span><span class="sxs-lookup"><span data-stu-id="c1539-144">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - <span data-ttu-id="c1539-145">[Windows keyboard shortcuts](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf) (Métodos abreviados de teclado de Windows)</span><span class="sxs-lookup"><span data-stu-id="c1539-145">[Windows keyboard shortcuts](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="c1539-146">[Next - Add a controller](adding-controller.md) (Siguiente - Agregar un controlador)</span><span class="sxs-lookup"><span data-stu-id="c1539-146">[Next - Add a controller](adding-controller.md)</span></span>
