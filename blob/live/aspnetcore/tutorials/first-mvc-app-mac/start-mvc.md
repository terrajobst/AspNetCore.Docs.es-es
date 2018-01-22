---
title: "Introducción a ASP.NET Core MVC y Visual Studio para Mac"
author: rick-anderson
description: "Introducción a ASP.NET Core MVC y Visual Studio"
ms.author: riande
manager: wpickett
ms.date: 8/23/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-mac/start-mvc
ms.openlocfilehash: 51c0477c40885d3aaaa7562f8baba0a94cb4f920
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
# <a name="getting-started-with-aspnet-core-mvc-and-visual-studio-for-mac"></a><span data-ttu-id="147fa-103">Introducción a ASP.NET Core MVC y Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="147fa-103">Getting started with ASP.NET Core MVC and Visual Studio for Mac</span></span>

<span data-ttu-id="147fa-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="147fa-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="147fa-105">En este tutorial aprenderá los aspectos básicos de la creación de una aplicación web de ASP.NET Core MVC con [Visual Studio para Mac](https://www.visualstudio.com/vs/visual-studio-mac/).</span><span class="sxs-lookup"><span data-stu-id="147fa-105">This tutorial teaches you the basics of building an ASP.NET Core MVC web app using [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/).</span></span> 

[!INCLUDE[consider RP](../../includes/razor.md)]

<span data-ttu-id="147fa-106">Hay tres versiones de este tutorial:</span><span class="sxs-lookup"><span data-stu-id="147fa-106">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="147fa-107">macOS: [Creación de una aplicación de ASP.NET Core MVC con Visual Studio para Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="147fa-107">macOS: [Build an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="147fa-108">Windows: [Creación de una aplicación de ASP.NET Core MVC con Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="147fa-108">Windows: [Build an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="147fa-109">Linux, macOS y Windows: [Creación de una aplicación de ASP.NET Core MVC con Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="147fa-109">Linux, macOS, and Windows: [Build an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="147fa-110">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="147fa-110">Prerequisites</span></span>

<span data-ttu-id="147fa-111">Para realizar este tutorial se necesita el [SDK de .NET Core 2.0.0](https://www.microsoft.com/net/core) o una versión posterior.</span><span class="sxs-lookup"><span data-stu-id="147fa-111">This tutorial requires the [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>

<span data-ttu-id="147fa-112">Instale el software siguiente:</span><span class="sxs-lookup"><span data-stu-id="147fa-112">Install the following:</span></span>

- <span data-ttu-id="147fa-113">[SDK de .NET Core 2.0.0](https://www.microsoft.com/net/core) o versiones posteriores</span><span class="sxs-lookup"><span data-stu-id="147fa-113">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later</span></span>
- [<span data-ttu-id="147fa-114">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="147fa-114">Visual Studio for Mac</span></span>](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-a-web-app"></a><span data-ttu-id="147fa-115">Creación de una aplicación web</span><span class="sxs-lookup"><span data-stu-id="147fa-115">Create a web app</span></span>

<span data-ttu-id="147fa-116">Desde Visual Studio, seleccione **Archivo > Nueva solución**.</span><span class="sxs-lookup"><span data-stu-id="147fa-116">From Visual Studio, select **File > New Solution**.</span></span>

![macOS: Nueva solución](../first-web-api-mac/_static/sln.png)

<span data-ttu-id="147fa-118">Seleccione **Aplicación .NET Core > ASP.NET Core > Aplicación web > Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="147fa-118">Select **.NET Core App >  ASP.NET Core > Web App > Next**.</span></span>

![macOS: Cuadro de diálogo Nuevo proyecto](start-mvc/1.png)

<span data-ttu-id="147fa-120">Asigne el nombre **MvcMovie** al proyecto y, después, seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="147fa-120">Name the project **MvcMovie**, and then select **Create**.</span></span>

![macOS: Cuadro de diálogo Nuevo proyecto](start-mvc/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="147fa-122">Iniciar la aplicación</span><span class="sxs-lookup"><span data-stu-id="147fa-122">Launch the app</span></span>

<span data-ttu-id="147fa-123">En Visual Studio, seleccione **Ejecutar > Iniciar sin depurar** para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="147fa-123">In Visual Studio, select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="147fa-124">Visual Studio inicia [Kestrel](xref:fundamentals/servers/index#kestrel), inicia un explorador y navega a `http://localhost:port`, donde *port* es un número de puerto elegido aleatoriamente.</span><span class="sxs-lookup"><span data-stu-id="147fa-124">Visual Studio starts [Kestrel](xref:fundamentals/servers/index#kestrel), launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

![Explorador con el nuevo proyecto](start-mvc/b1.png)

* <span data-ttu-id="147fa-126">En la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="147fa-126">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="147fa-127">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="147fa-127">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="147fa-128">Cuando Visual Studio crea un proyecto web, se usa un puerto aleatorio para el servidor web.</span><span class="sxs-lookup"><span data-stu-id="147fa-128">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="147fa-129">Al ejecutar la aplicación verá otro puerto distinto.</span><span class="sxs-lookup"><span data-stu-id="147fa-129">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="147fa-130">Puede iniciar la aplicación en modo de depuración o en modo de no depuración desde el menú **Ejecutar**.</span><span class="sxs-lookup"><span data-stu-id="147fa-130">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

<span data-ttu-id="147fa-131">La plantilla predeterminada proporciona los vínculos **Inicio, Acerca de** y **Contacto**.</span><span class="sxs-lookup"><span data-stu-id="147fa-131">The default template gives you **Home, About** and **Contact** links.</span></span> <span data-ttu-id="147fa-132">En la imagen del explorador anterior no se muestran estos vínculos.</span><span class="sxs-lookup"><span data-stu-id="147fa-132">The browser image above doesn't show these links.</span></span> <span data-ttu-id="147fa-133">Según el tamaño del explorador, tendrá que hacer clic en el icono de navegación para que se muestren.</span><span class="sxs-lookup"><span data-stu-id="147fa-133">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Explorador con el nuevo proyecto](start-mvc/b2.png)

<span data-ttu-id="147fa-135">En la siguiente sección de este tutorial conocerá MVC y empezará a escribir código.</span><span class="sxs-lookup"><span data-stu-id="147fa-135">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="147fa-136">Siguiente</span><span class="sxs-lookup"><span data-stu-id="147fa-136">Next</span></span>](adding-controller.md)  
