---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: Use OWIN para autohospedaje de ASP.NET Web API 2 | Microsoft Docs
author: rick-anderson
description: Este tutorial muestra cómo hospedar ASP.NET Web API en una aplicación de consola, el uso de OWIN para autohospedaje el marco API Web. Interfaz Web abierta para .NET (OWIN) d...
ms.author: riande
ms.date: 07/09/2013
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: 0d16498e94ac0a66c117ed057db398c14080beaa
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41839204"
---
<a name="use-owin-to-self-host-aspnet-web-api-2"></a><span data-ttu-id="c9698-104">Use OWIN para autohospedaje de ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="c9698-104">Use OWIN to Self-Host ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="c9698-105">por [Kanchan Mehrotra](https://twitter.com/kanchanmeh)</span><span class="sxs-lookup"><span data-stu-id="c9698-105">by [Kanchan Mehrotra](https://twitter.com/kanchanmeh)</span></span>

> <span data-ttu-id="c9698-106">Este tutorial muestra cómo hospedar ASP.NET Web API en una aplicación de consola, el uso de OWIN para autohospedaje el marco API Web.</span><span class="sxs-lookup"><span data-stu-id="c9698-106">This tutorial shows how to host ASP.NET Web API in a console application, using OWIN to self-host the Web API framework.</span></span>
> 
> <span data-ttu-id="c9698-107">[Interfaz Web abierta para .NET](http://owin.org) (OWIN) define una abstracción entre los servidores web de .NET y aplicaciones web.</span><span class="sxs-lookup"><span data-stu-id="c9698-107">[Open Web Interface for .NET](http://owin.org) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="c9698-108">OWIN desacopla la aplicación web desde el servidor, lo que hace que OWIN ideal para el autohospedaje de una aplicación web en su propio proceso, fuera de IIS.</span><span class="sxs-lookup"><span data-stu-id="c9698-108">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="c9698-109">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="c9698-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="c9698-110">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (también funciona con Visual Studio 2012)</span><span class="sxs-lookup"><span data-stu-id="c9698-110">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (also works with Visual Studio 2012)</span></span>
> - <span data-ttu-id="c9698-111">Web API 2</span><span class="sxs-lookup"><span data-stu-id="c9698-111">Web API 2</span></span>


> [!NOTE]
> <span data-ttu-id="c9698-112">Puede encontrar el código fuente completo para este tutorial en [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).</span><span class="sxs-lookup"><span data-stu-id="c9698-112">You can find the complete source code for this tutorial at [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).</span></span>


## <a name="create-a-console-application"></a><span data-ttu-id="c9698-113">Crear una aplicación de consola</span><span class="sxs-lookup"><span data-stu-id="c9698-113">Create a Console Application</span></span>

<span data-ttu-id="c9698-114">En el **archivo** menú, haga clic en **New**, a continuación, haga clic en **proyecto**.</span><span class="sxs-lookup"><span data-stu-id="c9698-114">On the **File** menu, click **New**, then click **Project**.</span></span> <span data-ttu-id="c9698-115">Desde **plantillas instaladas**, bajo Visual C#, haga clic en **Windows** y, a continuación, haga clic en **aplicación de consola**.</span><span class="sxs-lookup"><span data-stu-id="c9698-115">From **Installed Templates**, under Visual C#, click **Windows** and then click **Console Application**.</span></span> <span data-ttu-id="c9698-116">Denomine el proyecto "OwinSelfhostSample" y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="c9698-116">Name the project "OwinSelfhostSample" and click **OK**.</span></span>

[![](use-owin-to-self-host-web-api/_static/image2.png)](use-owin-to-self-host-web-api/_static/image1.png)

## <a name="add-the-web-api-and-owin-packages"></a><span data-ttu-id="c9698-117">Agregar la API Web y paquetes OWIN</span><span class="sxs-lookup"><span data-stu-id="c9698-117">Add the Web API and OWIN Packages</span></span>

<span data-ttu-id="c9698-118">Desde el **herramientas** menú, haga clic en **Administrador de paquetes de biblioteca**, a continuación, haga clic en **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="c9698-118">From the **Tools** menu, click **Library Package Manager**, then click **Package Manager Console**.</span></span> <span data-ttu-id="c9698-119">En la ventana de consola de administrador de paquetes, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="c9698-119">In the Package Manager Console window, enter the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

<span data-ttu-id="c9698-120">Esto instalará el paquete de selfhost WebAPI OWIN y todos los paquetes necesarios de OWIN.</span><span class="sxs-lookup"><span data-stu-id="c9698-120">This will install the WebAPI OWIN selfhost package and all the required OWIN packages.</span></span>

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a><span data-ttu-id="c9698-121">Configuración de Web API para autohospedar</span><span class="sxs-lookup"><span data-stu-id="c9698-121">Configure Web API for Self-Host</span></span>

<span data-ttu-id="c9698-122">En el Explorador de soluciones, haga clic en el proyecto y seleccione **agregar** / **clase** para agregar una nueva clase.</span><span class="sxs-lookup"><span data-stu-id="c9698-122">In Solution Explorer, right click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="c9698-123">Asigne a la clase el nombre `Startup`.</span><span class="sxs-lookup"><span data-stu-id="c9698-123">Name the class `Startup`.</span></span>

![](use-owin-to-self-host-web-api/_static/image5.png)

<span data-ttu-id="c9698-124">Reemplace todo el código repetitivo en este archivo por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="c9698-124">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a><span data-ttu-id="c9698-125">Agregar un controlador de API Web</span><span class="sxs-lookup"><span data-stu-id="c9698-125">Add a Web API Controller</span></span>

<span data-ttu-id="c9698-126">A continuación, agregue una clase de controlador Web API.</span><span class="sxs-lookup"><span data-stu-id="c9698-126">Next, add a Web API controller class.</span></span> <span data-ttu-id="c9698-127">En el Explorador de soluciones, haga clic en el proyecto y seleccione **agregar** / **clase** para agregar una nueva clase.</span><span class="sxs-lookup"><span data-stu-id="c9698-127">In Solution Explorer, right click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="c9698-128">Asigne a la clase el nombre `ValuesController`.</span><span class="sxs-lookup"><span data-stu-id="c9698-128">Name the class `ValuesController`.</span></span>

<span data-ttu-id="c9698-129">Reemplace todo el código repetitivo en este archivo por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="c9698-129">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-using-httpclient"></a><span data-ttu-id="c9698-130">Iniciar el Host OWIN y realice una solicitud mediante HttpClient</span><span class="sxs-lookup"><span data-stu-id="c9698-130">Start the OWIN Host and Make a Request Using HttpClient</span></span>

<span data-ttu-id="c9698-131">Reemplace todo el código reutilizable en el archivo Program.cs por el siguiente:</span><span class="sxs-lookup"><span data-stu-id="c9698-131">Replace all of the boilerplate code in the Program.cs file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="running-the-application"></a><span data-ttu-id="c9698-132">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="c9698-132">Running the Application</span></span>

<span data-ttu-id="c9698-133">Para ejecutar la aplicación, presione F5 en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c9698-133">To run the application, press F5 in Visual Studio.</span></span> <span data-ttu-id="c9698-134">La salida debe tener un aspecto parecido al siguiente:</span><span class="sxs-lookup"><span data-stu-id="c9698-134">The output should look like the following:</span></span>

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a><span data-ttu-id="c9698-135">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="c9698-135">Additional Resources</span></span>

[<span data-ttu-id="c9698-136">Información general del proyecto Katana</span><span class="sxs-lookup"><span data-stu-id="c9698-136">An Overview of Project Katana</span></span>](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[<span data-ttu-id="c9698-137">Hospedar ASP.NET Web API en un rol de trabajo de Azure</span><span class="sxs-lookup"><span data-stu-id="c9698-137">Host ASP.NET Web API in an Azure Worker Role</span></span>](host-aspnet-web-api-in-an-azure-worker-role.md)
