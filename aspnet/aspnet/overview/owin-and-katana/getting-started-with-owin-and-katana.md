---
uid: aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
title: "Introducción a OWIN y Katana | Documentos de Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/27/2013
ms.topic: article
ms.assetid: 6dae249f-5ac6-4f6e-bc49-13bcd5a54a70
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
msc.type: authoredcontent
ms.openlocfilehash: 8922aada723da9b149ec111902fcd883c8241dfb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="getting-started-with-owin-and-katana"></a><span data-ttu-id="902cd-102">Introducción a OWIN y Katana</span><span class="sxs-lookup"><span data-stu-id="902cd-102">Getting Started with OWIN and Katana</span></span>
====================
<span data-ttu-id="902cd-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="902cd-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="902cd-104">[Abrir la interfaz Web para .NET (OWIN)](http://owin.org/) define una abstracción entre servidores web de .NET y las aplicaciones web.</span><span class="sxs-lookup"><span data-stu-id="902cd-104">[Open Web Interface for .NET (OWIN)](http://owin.org/) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="902cd-105">Si se desacopla el servidor web de la aplicación, OWIN resulta más fácil crear middleware para el desarrollo web. NET.</span><span class="sxs-lookup"><span data-stu-id="902cd-105">By decoupling the web server from the application, OWIN makes it easier to create middleware for .NET web development.</span></span> <span data-ttu-id="902cd-106">Además, OWIN facilita a las aplicaciones web de puerto a otros hosts &#8212; por ejemplo, hospeda a sí mismo en un servicio de Windows u otro proceso.</span><span class="sxs-lookup"><span data-stu-id="902cd-106">Also, OWIN makes it easier to port web applications to other hosts&#8212;for example, self-hosting in a Windows service or other process.</span></span>

<span data-ttu-id="902cd-107">OWIN es una especificación de propiedad de la Comunidad, no es una implementación.</span><span class="sxs-lookup"><span data-stu-id="902cd-107">OWIN is a community-owned specification, not an implementation.</span></span> <span data-ttu-id="902cd-108">El proyecto de Katana es un conjunto de componentes OWIN de código abierto desarrollado por Microsoft.</span><span class="sxs-lookup"><span data-stu-id="902cd-108">The Katana project is a set of open-source OWIN components developed by Microsoft.</span></span> <span data-ttu-id="902cd-109">Para obtener una descripción general de OWIN y Katana, consulte [una información general del proyecto Katana](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="902cd-109">For a general overview of both OWIN and Katana, see [An Overview of Project Katana](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="902cd-110">En este artículo, saltará derecha en el código para empezar a trabajar.</span><span class="sxs-lookup"><span data-stu-id="902cd-110">In this article, I will jump right into code to get started.</span></span>

<span data-ttu-id="902cd-111">Este tutorial usa [candidato de versión comercial de Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566), pero también puede usar Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="902cd-111">This tutorial uses [Visual Studio 2013 Release Candidate](https://go.microsoft.com/fwlink/?LinkId=306566), but you can also use Visual Studio 2012.</span></span> <span data-ttu-id="902cd-112">Algunos de los pasos son diferentes en Visual Studio 2012, que tenga en cuenta a continuación.</span><span class="sxs-lookup"><span data-stu-id="902cd-112">A few of the steps are different in Visual Studio 2012, which I note below.</span></span>

## <a name="host-owin-in-iis"></a><span data-ttu-id="902cd-113">Host OWIN en IIS</span><span class="sxs-lookup"><span data-stu-id="902cd-113">Host OWIN in IIS</span></span>

<span data-ttu-id="902cd-114">En esta sección, se podrá hospedar OWIN en IIS.</span><span class="sxs-lookup"><span data-stu-id="902cd-114">In this section, we'll host OWIN in IIS.</span></span> <span data-ttu-id="902cd-115">Esta opción proporciona la flexibilidad y facilidad de creación de una canalización OWIN junto con el conjunto de características antiguas de IIS.</span><span class="sxs-lookup"><span data-stu-id="902cd-115">This option gives you the flexibility and composability of an OWIN pipeline together with the mature feature set of IIS.</span></span> <span data-ttu-id="902cd-116">Con esta opción, la aplicación OWIN se ejecuta en la canalización de solicitudes ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="902cd-116">Using this option, the OWIN application runs in the ASP.NET request pipeline.</span></span>

<span data-ttu-id="902cd-117">En primer lugar, cree un nuevo proyecto de aplicación Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="902cd-117">First, create a new ASP.NET Web Application project.</span></span> <span data-ttu-id="902cd-118">(En Visual Studio 2012, utilice el tipo de proyecto de aplicación Web ASP.NET vacía).</span><span class="sxs-lookup"><span data-stu-id="902cd-118">(In Visual Studio 2012, use the ASP.NET Empty Web Application project type.)</span></span>

![](getting-started-with-owin-and-katana/_static/image1.png)

<span data-ttu-id="902cd-119">En el **nuevo proyecto ASP.NET** cuadro de diálogo, seleccione la **vacía** plantilla.</span><span class="sxs-lookup"><span data-stu-id="902cd-119">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span>

![](getting-started-with-owin-and-katana/_static/image2.png)

### <a name="add-nuget-packages"></a><span data-ttu-id="902cd-120">Agregar paquetes de NuGet</span><span class="sxs-lookup"><span data-stu-id="902cd-120">Add NuGet Packages</span></span>

<span data-ttu-id="902cd-121">A continuación, agregue los paquetes de NuGet necesarios.</span><span class="sxs-lookup"><span data-stu-id="902cd-121">Next, add the required NuGet packages.</span></span> <span data-ttu-id="902cd-122">Desde el **herramientas** menú, seleccione **Administrador de paquetes de biblioteca**, a continuación, seleccione **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="902cd-122">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="902cd-123">En la ventana de la consola de administrador de paquetes, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="902cd-123">In the Package Manager Console window, type the following command:</span></span>

`install-package Microsoft.Owin.Host.SystemWeb –Pre`

![](getting-started-with-owin-and-katana/_static/image3.png)

### <a name="add-a-startup-class"></a><span data-ttu-id="902cd-124">Agregar una clase de inicio</span><span class="sxs-lookup"><span data-stu-id="902cd-124">Add a Startup Class</span></span>

<span data-ttu-id="902cd-125">A continuación, agregue una clase de inicio OWIN.</span><span class="sxs-lookup"><span data-stu-id="902cd-125">Next, add an OWIN startup class.</span></span> <span data-ttu-id="902cd-126">En el Explorador de soluciones, haga clic en el proyecto y seleccione **agregar**, a continuación, seleccione **nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="902cd-126">In Solution Explorer, right-click the project and select **Add**, then select **New Item**.</span></span> <span data-ttu-id="902cd-127">En el **Agregar nuevo elemento** cuadro de diálogo, seleccione **clase de inicio de Owin**.</span><span class="sxs-lookup"><span data-stu-id="902cd-127">In the **Add New Item** dialog, select **Owin Startup class**.</span></span> <span data-ttu-id="902cd-128">Para obtener más información acerca de cómo configurar la clase de inicio, consulte [detección de clase de inicio de OWIN](owin-startup-class-detection.md).</span><span class="sxs-lookup"><span data-stu-id="902cd-128">For more info on configuring the startup class, see [OWIN Startup Class Detection](owin-startup-class-detection.md).</span></span>

![](getting-started-with-owin-and-katana/_static/image4.png)

<span data-ttu-id="902cd-129">Agregue el código siguiente al método `Startup1.Configuration`:</span><span class="sxs-lookup"><span data-stu-id="902cd-129">Add the following code to the `Startup1.Configuration` method:</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample1.cs?highlight=3)]

<span data-ttu-id="902cd-130">Este código agrega un elemento simple de middleware a la canalización OWIN, que se implementa como una función que recibe un **Microsoft.Owin.IOwinContext** instancia.</span><span class="sxs-lookup"><span data-stu-id="902cd-130">This code adds a simple piece of middleware to the OWIN pipeline, implemented as a function that receives a **Microsoft.Owin.IOwinContext** instance.</span></span> <span data-ttu-id="902cd-131">Cuando el servidor recibe una solicitud HTTP, la canalización OWIN invoca el middleware.</span><span class="sxs-lookup"><span data-stu-id="902cd-131">When the server receives an HTTP request, the OWIN pipeline invokes the middleware.</span></span> <span data-ttu-id="902cd-132">El middleware establece el tipo de contenido de la respuesta y escribe el cuerpo de respuesta.</span><span class="sxs-lookup"><span data-stu-id="902cd-132">The middleware sets the content type for the response and writes the response body.</span></span>

> [!NOTE]
> <span data-ttu-id="902cd-133">La plantilla de clase de inicio de OWIN está disponible en Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="902cd-133">The OWIN Startup class template is available in Visual Studio 2013.</span></span> <span data-ttu-id="902cd-134">Si usas Visual Studio 2012, basta con agregar una nueva clase vacía denominada `Startup1`y pegue el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="902cd-134">If you are using Visual Studio 2012, just add a new empty class named `Startup1`, and paste in the following code:</span></span>


[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample2.cs)]

### <a name="run-the-application"></a><span data-ttu-id="902cd-135">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="902cd-135">Run the Application</span></span>

<span data-ttu-id="902cd-136">Presione F5 para comenzar la depuración.</span><span class="sxs-lookup"><span data-stu-id="902cd-136">Press F5 to begin debugging.</span></span> <span data-ttu-id="902cd-137">Visual Studio abrirá una ventana del explorador para `http://localhost:*port*/`.</span><span class="sxs-lookup"><span data-stu-id="902cd-137">Visual Studio will open a browser window to `http://localhost:*port*/`.</span></span> <span data-ttu-id="902cd-138">La página debe ser similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="902cd-138">The page should look like the following:</span></span>

![](getting-started-with-owin-and-katana/_static/image5.png)

## <a name="self-host-owin-in-a-console-application"></a><span data-ttu-id="902cd-139">Autohospedaje OWIN en una aplicación de consola</span><span class="sxs-lookup"><span data-stu-id="902cd-139">Self-Host OWIN in a Console Application</span></span>

<span data-ttu-id="902cd-140">Es fácil convertir esta aplicación de hospedaje de IIS que hospeda a sí mismo en un proceso personalizado.</span><span class="sxs-lookup"><span data-stu-id="902cd-140">It's easy to convert this application from IIS hosting to self-hosting in a custom process.</span></span> <span data-ttu-id="902cd-141">Con el hospedaje de IIS, los IIS actúa como el servidor HTTP y que el proceso que hospedan el servidor.</span><span class="sxs-lookup"><span data-stu-id="902cd-141">With IIS hosting, IIS acts as both the HTTP server and as the process that host the sever.</span></span> <span data-ttu-id="902cd-142">Con que se hospeda a sí mismo, la aplicación crea el proceso y usa el **HttpListener** clase como el servidor HTTP.</span><span class="sxs-lookup"><span data-stu-id="902cd-142">With self-hosting, your application creates the process and uses the **HttpListener** class as the HTTP server.</span></span>

<span data-ttu-id="902cd-143">En Visual Studio, cree una nueva aplicación de consola.</span><span class="sxs-lookup"><span data-stu-id="902cd-143">In Visual Studio, create a new console application.</span></span> <span data-ttu-id="902cd-144">En la ventana de la consola de administrador de paquetes, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="902cd-144">In the Package Manager Console window, type the following command:</span></span>

`Install-Package Microsoft.Owin.SelfHost -Pre`

<span data-ttu-id="902cd-145">Agregar un `Startup1` clase a partir de la parte 1 de este tutorial al proyecto.</span><span class="sxs-lookup"><span data-stu-id="902cd-145">Add a `Startup1` class from part 1 of this tutorial to the project.</span></span> <span data-ttu-id="902cd-146">No es necesario modificar esta clase.</span><span class="sxs-lookup"><span data-stu-id="902cd-146">You don't need to modify this class.</span></span>

<span data-ttu-id="902cd-147">Implementar la aplicación `Main` método tal como se indica a continuación.</span><span class="sxs-lookup"><span data-stu-id="902cd-147">Implement the application's `Main` method as follows.</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample3.cs)]

<span data-ttu-id="902cd-148">Al ejecutar la aplicación de consola, el servidor empieza a escuchar a `http://localhost:9000`.</span><span class="sxs-lookup"><span data-stu-id="902cd-148">When you run the console application, the server starts listening to `http://localhost:9000`.</span></span> <span data-ttu-id="902cd-149">Si navega a esta dirección en un explorador web, verá la página "¡Hello world".</span><span class="sxs-lookup"><span data-stu-id="902cd-149">If you navigate to this address in a web browser, you will see the "Hello world" page.</span></span>

![](getting-started-with-owin-and-katana/_static/image6.png)

## <a name="add-owin-diagnostics"></a><span data-ttu-id="902cd-150">Agregar diagnósticos OWIN</span><span class="sxs-lookup"><span data-stu-id="902cd-150">Add OWIN Diagnostics</span></span>

<span data-ttu-id="902cd-151">El paquete Microsoft.Owin.Diagnostics contiene middleware que detecta las excepciones no controladas y muestra una página HTML con los detalles del error.</span><span class="sxs-lookup"><span data-stu-id="902cd-151">The Microsoft.Owin.Diagnostics package contains middleware that catches unhandled exceptions and displays an HTML page with error details.</span></span> <span data-ttu-id="902cd-152">Estas funciones de página mucho al igual que la página de error ASP.NET que a veces se denomina el "[pantalla amarillo de la muerte](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)" (YSOD).</span><span class="sxs-lookup"><span data-stu-id="902cd-152">This page functions much like the ASP.NET error page that is sometimes called the "[yellow screen of death](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)" (YSOD).</span></span> <span data-ttu-id="902cd-153">Como el YSOD, la página de error de Katana es útil durante el desarrollo, pero es una buena práctica para deshabilitarlo en modo de producción.</span><span class="sxs-lookup"><span data-stu-id="902cd-153">Like the YSOD, the Katana error page is useful during development, but it's a good practice to disable it in production mode.</span></span>

<span data-ttu-id="902cd-154">Para instalar el paquete de diagnóstico en el proyecto, escriba el siguiente comando en la ventana de la consola de administrador de paquetes:</span><span class="sxs-lookup"><span data-stu-id="902cd-154">To install the Diagnostics package in your project, type the following command in the Package Manager Console window:</span></span>

`install-package Microsoft.Owin.Diagnostics –Pre`

<span data-ttu-id="902cd-155">Cambie el código en su `Startup1.Configuration` método tal como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="902cd-155">Change the code in your `Startup1.Configuration` method as follows:</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample4.cs?highlight=4,9-12)]

<span data-ttu-id="902cd-156">Ahora use CTRL + F5 para ejecutar la aplicación sin depuración, para que Visual Studio no se interrumpirá en la excepción.</span><span class="sxs-lookup"><span data-stu-id="902cd-156">Now use CTRL+F5 to run the application without debugging, so that Visual Studio will not break on the exception.</span></span> <span data-ttu-id="902cd-157">La aplicación comporta igual que antes, hasta que vaya a `http://localhost/fail`, momento en que la aplicación produce la excepción.</span><span class="sxs-lookup"><span data-stu-id="902cd-157">The application behaves the same as before, until you navigate to `http://localhost/fail`, at which point the application throws the exception.</span></span> <span data-ttu-id="902cd-158">El middleware de página de error se detecte la excepción y mostrar una página HTML con información sobre el error.</span><span class="sxs-lookup"><span data-stu-id="902cd-158">The error page middleware will catch the exception and display an HTML page with information about the error.</span></span> <span data-ttu-id="902cd-159">Puede hacer clic en las pestañas para ver la pila, cadena de consulta, cookies, encabezado de solicitud y las variables de entorno OWIN.</span><span class="sxs-lookup"><span data-stu-id="902cd-159">You can click the tabs to see the stack, query string, cookies, request header, and OWIN environment variables.</span></span>

![](getting-started-with-owin-and-katana/_static/image7.png)

## <a name="next-steps"></a><span data-ttu-id="902cd-160">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="902cd-160">Next Steps</span></span>

- [<span data-ttu-id="902cd-161">Detección de clase de inicio OWIN</span><span class="sxs-lookup"><span data-stu-id="902cd-161">OWIN Startup Class Detection</span></span>](owin-startup-class-detection.md)
- [<span data-ttu-id="902cd-162">Usar OWIN para probar internamente ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="902cd-162">Use OWIN to Self-Host ASP.NET Web API</span></span>](../../../web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)
- [<span data-ttu-id="902cd-163">Usar OWIN para auto-hospedar SignalR</span><span class="sxs-lookup"><span data-stu-id="902cd-163">Use OWIN to Self-Host SignalR</span></span>](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)
