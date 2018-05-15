---
title: Introducción a ASP.NET Core MVC y Visual Studio para Mac
author: rick-anderson
description: Obtenga información sobre cómo empezar a usar ASP.NET Core MVC y Visual Studio.
manager: wpickett
ms.author: riande
ms.date: 8/23/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/first-mvc-app-mac/start-mvc
ms.openlocfilehash: ffa620f07251c52c785672d8fbeefacac31ed4c1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio-for-mac"></a><span data-ttu-id="b316f-103">Introducción a ASP.NET Core MVC y Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="b316f-103">Get started with ASP.NET Core MVC and Visual Studio for Mac</span></span>

<span data-ttu-id="b316f-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b316f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b316f-105">En este tutorial aprenderá los aspectos básicos de la creación de una aplicación web de ASP.NET Core MVC con [Visual Studio para Mac](https://www.visualstudio.com/vs/visual-studio-mac/).</span><span class="sxs-lookup"><span data-stu-id="b316f-105">This tutorial teaches you the basics of building an ASP.NET Core MVC web app using [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/).</span></span> 

[!INCLUDE [consider RP](../../includes/razor.md)]

<span data-ttu-id="b316f-106">Hay tres versiones de este tutorial:</span><span class="sxs-lookup"><span data-stu-id="b316f-106">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="b316f-107">macOS: [Creación de una aplicación de ASP.NET Core MVC con Visual Studio para Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="b316f-107">macOS: [Build an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="b316f-108">Windows: [Creación de una aplicación de ASP.NET Core MVC con Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="b316f-108">Windows: [Build an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="b316f-109">Linux, macOS y Windows: [Creación de una aplicación de ASP.NET Core MVC con Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="b316f-109">Linux, macOS, and Windows: [Build an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b316f-110">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="b316f-110">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-macos.md)]

## <a name="create-a-web-app"></a><span data-ttu-id="b316f-111">Creación de una aplicación web</span><span class="sxs-lookup"><span data-stu-id="b316f-111">Create a web app</span></span>

<span data-ttu-id="b316f-112">Desde Visual Studio, seleccione **Archivo > Nueva solución**.</span><span class="sxs-lookup"><span data-stu-id="b316f-112">From Visual Studio, select **File > New Solution**.</span></span>

![macOS: Nueva solución](../first-web-api-mac/_static/sln.png)

<span data-ttu-id="b316f-114">Seleccione **Aplicación .NET Core > ASP.NET Core > Aplicación web > Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="b316f-114">Select **.NET Core App >  ASP.NET Core > Web App > Next**.</span></span>

![macOS: Cuadro de diálogo Nuevo proyecto](start-mvc/1.png)

<span data-ttu-id="b316f-116">Asigne el nombre **MvcMovie** al proyecto y, después, seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="b316f-116">Name the project **MvcMovie**, and then select **Create**.</span></span>

![macOS: Cuadro de diálogo Nuevo proyecto](start-mvc/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="b316f-118">Iniciar la aplicación</span><span class="sxs-lookup"><span data-stu-id="b316f-118">Launch the app</span></span>

<span data-ttu-id="b316f-119">En Visual Studio, seleccione **Ejecutar > Iniciar sin depurar** para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b316f-119">In Visual Studio, select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="b316f-120">Visual Studio inicia [Kestrel](xref:fundamentals/servers/index#kestrel), inicia un explorador y navega a `http://localhost:port`, donde *port* es un número de puerto elegido aleatoriamente.</span><span class="sxs-lookup"><span data-stu-id="b316f-120">Visual Studio starts [Kestrel](xref:fundamentals/servers/index#kestrel), launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

![Explorador con el nuevo proyecto](start-mvc/b1.png)

* <span data-ttu-id="b316f-122">En la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="b316f-122">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="b316f-123">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="b316f-123">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="b316f-124">Cuando Visual Studio crea un proyecto web, se usa un puerto aleatorio para el servidor web.</span><span class="sxs-lookup"><span data-stu-id="b316f-124">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="b316f-125">Al ejecutar la aplicación verá otro puerto distinto.</span><span class="sxs-lookup"><span data-stu-id="b316f-125">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="b316f-126">Puede iniciar la aplicación en modo de depuración o en modo de no depuración desde el menú **Ejecutar**.</span><span class="sxs-lookup"><span data-stu-id="b316f-126">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

<span data-ttu-id="b316f-127">La plantilla predeterminada proporciona los vínculos **Inicio, Acerca de** y **Contacto**.</span><span class="sxs-lookup"><span data-stu-id="b316f-127">The default template gives you **Home, About** and **Contact** links.</span></span> <span data-ttu-id="b316f-128">En la imagen del explorador anterior no se muestran estos vínculos.</span><span class="sxs-lookup"><span data-stu-id="b316f-128">The browser image above doesn't show these links.</span></span> <span data-ttu-id="b316f-129">Según el tamaño del explorador, tendrá que hacer clic en el icono de navegación para que se muestren.</span><span class="sxs-lookup"><span data-stu-id="b316f-129">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Explorador con el nuevo proyecto](start-mvc/b2.png)

<span data-ttu-id="b316f-131">En la siguiente sección de este tutorial conocerá MVC y empezará a escribir código.</span><span class="sxs-lookup"><span data-stu-id="b316f-131">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="b316f-132">Siguiente</span><span class="sxs-lookup"><span data-stu-id="b316f-132">Next</span></span>](adding-controller.md)  
