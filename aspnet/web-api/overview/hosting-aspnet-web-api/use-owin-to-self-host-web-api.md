---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: Use OWIN para autohospedaje de ASP.NET Web API | Microsoft Docs
author: rick-anderson
description: Este tutorial muestra cómo hospedar ASP.NET Web API en una aplicación de consola, el uso de OWIN para autohospedaje el marco API Web. Interfaz Web abierta para .NET (OWIN) d...
ms.author: riande
ms.date: 07/09/2013
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: 59ce24aa47ca590fbe9b617dbbe8bc6b3711849e
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667393"
---
<a name="use-owin-to-self-host-aspnet-web-api"></a><span data-ttu-id="22a27-104">Use OWIN para autohospedaje de ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="22a27-104">Use OWIN to Self-Host ASP.NET Web API</span></span> 
====================

> <span data-ttu-id="22a27-105">Este tutorial muestra cómo hospedar ASP.NET Web API en una aplicación de consola, el uso de OWIN para autohospedaje el marco API Web.</span><span class="sxs-lookup"><span data-stu-id="22a27-105">This tutorial shows how to host ASP.NET Web API in a console application, using OWIN to self-host the Web API framework.</span></span>
>
> <span data-ttu-id="22a27-106">[Interfaz Web abierta para .NET](http://owin.org) (OWIN) define una abstracción entre los servidores web de .NET y aplicaciones web.</span><span class="sxs-lookup"><span data-stu-id="22a27-106">[Open Web Interface for .NET](http://owin.org) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="22a27-107">OWIN desacopla la aplicación web desde el servidor, lo que hace que OWIN ideal para el autohospedaje de una aplicación web en su propio proceso, fuera de IIS.</span><span class="sxs-lookup"><span data-stu-id="22a27-107">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="22a27-108">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="22a27-108">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="22a27-109">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="22a27-109">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/) 
> - <span data-ttu-id="22a27-110">Web API 5.2.7</span><span class="sxs-lookup"><span data-stu-id="22a27-110">Web API 5.2.7</span></span>


> [!NOTE]
> <span data-ttu-id="22a27-111">Puede encontrar el código fuente completo para este tutorial en [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).</span><span class="sxs-lookup"><span data-stu-id="22a27-111">You can find the complete source code for this tutorial at [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).</span></span>


## <a name="create-a-console-application"></a><span data-ttu-id="22a27-112">Creación de una aplicación de consola</span><span class="sxs-lookup"><span data-stu-id="22a27-112">Create a console application</span></span>

<span data-ttu-id="22a27-113">En el **archivo** menú, **New**, a continuación, seleccione **proyecto**.</span><span class="sxs-lookup"><span data-stu-id="22a27-113">On the **File** menu,  **New**, then select **Project**.</span></span> <span data-ttu-id="22a27-114">Desde **instalado**, en **Visual C#** , seleccione **Windows Desktop** y, a continuación, seleccione **aplicación de consola (.Net Framework)**.</span><span class="sxs-lookup"><span data-stu-id="22a27-114">From **Installed**, under **Visual C#**, select **Windows Desktop** and then select **Console App (.Net Framework)**.</span></span> <span data-ttu-id="22a27-115">Denomine el proyecto "OwinSelfhostSample" y seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="22a27-115">Name the project "OwinSelfhostSample" and select **OK**.</span></span>

[![](use-owin-to-self-host-web-api/_static/image7.png)](use-owin-to-self-host-web-api/_static/image7.png)

## <a name="add-the-web-api-and-owin-packages"></a><span data-ttu-id="22a27-116">Agregue los paquetes de Web API y OWIN</span><span class="sxs-lookup"><span data-stu-id="22a27-116">Add the Web API and OWIN packages</span></span>

<span data-ttu-id="22a27-117">Desde el **herramientas** menú, seleccione **Administrador de paquetes de NuGet**, a continuación, seleccione **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="22a27-117">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="22a27-118">En la ventana de consola de administrador de paquetes, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="22a27-118">In the Package Manager Console window, enter the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

<span data-ttu-id="22a27-119">Esto instalará el paquete de selfhost WebAPI OWIN y todos los paquetes necesarios de OWIN.</span><span class="sxs-lookup"><span data-stu-id="22a27-119">This will install the WebAPI OWIN selfhost package and all the required OWIN packages.</span></span>

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a><span data-ttu-id="22a27-120">Configurar Web API para autohospedar</span><span class="sxs-lookup"><span data-stu-id="22a27-120">Configure Web API for self-host</span></span>

<span data-ttu-id="22a27-121">En el Explorador de soluciones, haga clic en el proyecto y seleccione **agregar** / **clase** para agregar una nueva clase.</span><span class="sxs-lookup"><span data-stu-id="22a27-121">In Solution Explorer, right-click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="22a27-122">Asigne a la clase el nombre `Startup`.</span><span class="sxs-lookup"><span data-stu-id="22a27-122">Name the class `Startup`.</span></span>

![](use-owin-to-self-host-web-api/_static/image5.png)

<span data-ttu-id="22a27-123">Reemplace todo el código repetitivo en este archivo por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="22a27-123">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a><span data-ttu-id="22a27-124">Agregar un controlador Web API</span><span class="sxs-lookup"><span data-stu-id="22a27-124">Add a Web API controller</span></span>

<span data-ttu-id="22a27-125">A continuación, agregue una clase de controlador Web API.</span><span class="sxs-lookup"><span data-stu-id="22a27-125">Next, add a Web API controller class.</span></span> <span data-ttu-id="22a27-126">En el Explorador de soluciones, haga clic en el proyecto y seleccione **agregar** / **clase** para agregar una nueva clase.</span><span class="sxs-lookup"><span data-stu-id="22a27-126">In Solution Explorer, right-click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="22a27-127">Asigne a la clase el nombre `ValuesController`.</span><span class="sxs-lookup"><span data-stu-id="22a27-127">Name the class `ValuesController`.</span></span>

<span data-ttu-id="22a27-128">Reemplace todo el código repetitivo en este archivo por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="22a27-128">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-with-httpclient"></a><span data-ttu-id="22a27-129">Iniciar al Host OWIN y realizar una solicitud con HttpClient</span><span class="sxs-lookup"><span data-stu-id="22a27-129">Start the OWIN Host and make a request with HttpClient</span></span>

<span data-ttu-id="22a27-130">Reemplace todo el código reutilizable en el archivo Program.cs por el siguiente:</span><span class="sxs-lookup"><span data-stu-id="22a27-130">Replace all of the boilerplate code in the Program.cs file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="run-the-application"></a><span data-ttu-id="22a27-131">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="22a27-131">Run the application</span></span>

<span data-ttu-id="22a27-132">Para ejecutar la aplicación, presione F5 en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="22a27-132">To run the application, press F5 in Visual Studio.</span></span> <span data-ttu-id="22a27-133">La salida debe tener un aspecto parecido al siguiente:</span><span class="sxs-lookup"><span data-stu-id="22a27-133">The output should look like the following:</span></span>

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a><span data-ttu-id="22a27-134">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="22a27-134">Additional resources</span></span>

[<span data-ttu-id="22a27-135">Información general del proyecto Katana</span><span class="sxs-lookup"><span data-stu-id="22a27-135">An Overview of Project Katana</span></span>](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[<span data-ttu-id="22a27-136">Hospedar ASP.NET Web API en un rol de trabajo de Azure</span><span class="sxs-lookup"><span data-stu-id="22a27-136">Host ASP.NET Web API in an Azure Worker Role</span></span>](host-aspnet-web-api-in-an-azure-worker-role.md)
