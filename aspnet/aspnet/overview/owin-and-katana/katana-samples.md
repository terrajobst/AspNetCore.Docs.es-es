---
uid: aspnet/overview/owin-and-katana/katana-samples
title: Ejemplos de Katana | Documentos de Microsoft
author: microsoft
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2014
ms.topic: article
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: 815355c00c9c15cfefa5f98dc89da676743b0390
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="katana-samples"></a><span data-ttu-id="b80a7-102">Ejemplos de Katana</span><span class="sxs-lookup"><span data-stu-id="b80a7-102">Katana Samples</span></span>
====================
<span data-ttu-id="b80a7-103">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="b80a7-103">by [Microsoft](https://github.com/microsoft)</span></span>

## <a name="katana-samples"></a><span data-ttu-id="b80a7-104">Ejemplos de Katana</span><span class="sxs-lookup"><span data-stu-id="b80a7-104">Katana Samples</span></span>

<span data-ttu-id="b80a7-105">**ASP.NET enruta ejemplo** | [código fuente](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/AspNetRoutes/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="b80a7-105">**ASP.NET Routes Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/AspNetRoutes/ReadMe.txt)</span></span>  
<span data-ttu-id="b80a7-106">En algunas aplicaciones desea enlazar componentes OWIN en la tabla de rutas de Asp.Net paralelo con componentes no OWIN.</span><span class="sxs-lookup"><span data-stu-id="b80a7-106">In some applications you will want to hook up OWIN components in the Asp.Net route table side by side with non-OWIN components.</span></span> <span data-ttu-id="b80a7-107">Este ejemplo muestra cómo utilizar los métodos de extensión RouteCollection MapOwinPath y MapOwinRoute proporcionada por Microsoft.Owin.Host.SystemWeb.</span><span class="sxs-lookup"><span data-stu-id="b80a7-107">This sample shows how to use the RouteCollection extension methods MapOwinPath and MapOwinRoute provided by Microsoft.Owin.Host.SystemWeb.</span></span>

<span data-ttu-id="b80a7-108">**Bifurcación canalizaciones ejemplo** | [código fuente](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/BranchingPipelines/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="b80a7-108">**Branching Pipelines Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/BranchingPipelines/ReadMe.txt)</span></span>  
<span data-ttu-id="b80a7-109">Las canalizaciones de procesamiento de la solicitud OWIN no debe ser lineal, puede ser creó una bifurcación para procesar las solicitudes de maneras diferentes.</span><span class="sxs-lookup"><span data-stu-id="b80a7-109">OWIN request processing pipelines do not need to be linear, they can be branched to process requests in different ways.</span></span> <span data-ttu-id="b80a7-110">Este ejemplo muestra cómo construir una canalización de bifurcación en función de las rutas de acceso de solicitud u otros datos de solicitud como encabezados.</span><span class="sxs-lookup"><span data-stu-id="b80a7-110">This sample shows how to construct a branching pipeline based on request paths or other request data such as headers.</span></span> <span data-ttu-id="b80a7-111">Estos componentes están disponibles en el paquete de nuget Microsoft.Owin.Mapping.</span><span class="sxs-lookup"><span data-stu-id="b80a7-111">These components are available in the Microsoft.Owin.Mapping nuget package.</span></span>

<span data-ttu-id="b80a7-112">**Ejemplo de servidor personalizado** | [código fuente](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/CustomServer/MyCustomServer/CustomServer.cs) </span><span class="sxs-lookup"><span data-stu-id="b80a7-112">**Custom Server Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/CustomServer/MyCustomServer/CustomServer.cs) </span></span>  
<span data-ttu-id="b80a7-113">Muestra cómo utilizar un servidor OWIN personalizado cuando se hospeda a sí mismo OWIN.</span><span class="sxs-lookup"><span data-stu-id="b80a7-113">Shows how to use a custom OWIN server when self-hosting OWIN.</span></span>

<span data-ttu-id="b80a7-114">**Incrusta ejemplo** | [código fuente](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/Embedded/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="b80a7-114">**Embedded Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/Embedded/ReadMe.txt)</span></span>  
<span data-ttu-id="b80a7-115">Algunos servidores OWIN que se pueden ejecutar dentro de su propio proceso (&quot;hospeda a sí mismo&quot;).</span><span class="sxs-lookup"><span data-stu-id="b80a7-115">Some OWIN servers can be run inside of your own process (&quot;self-hosted&quot;).</span></span> <span data-ttu-id="b80a7-116">Este ejemplo muestra cómo iniciar una aplicación OWIN mediante las herramientas proporcionadas por el paquete de nuget Microsoft.Owin.Hosting.</span><span class="sxs-lookup"><span data-stu-id="b80a7-116">This sample shows how to start an OWIN application using the tools provided by the Microsoft.Owin.Hosting nuget package.</span></span>

<span data-ttu-id="b80a7-117">**Ejemplo HelloWorld** | [código fuente](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorld/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="b80a7-117">**HelloWorld Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorld/ReadMe.txt)</span></span>  
<span data-ttu-id="b80a7-118">OWIN es un abstracción de API que permite la portabilidad de aplicación entre varios servidores de servidor HTTP.</span><span class="sxs-lookup"><span data-stu-id="b80a7-118">OWIN is a HTTP server API abstraction that enables application portability across various servers.</span></span> <span data-ttu-id="b80a7-119">En este ejemplo se muestra cómo escribir una aplicación Hello World con algún **contenedores sencillos** alrededor de la abstracción de OWIN y ejecutar sin procesar en un servidor web, como ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b80a7-119">This sample demonstrates how to write a Hello World application using some **simple wrappers** around the raw OWIN abstraction and run it on a web server like ASP.NET.</span></span>

<span data-ttu-id="b80a7-120">**Ejemplo de Hello World sin formato OWIN** | [código fuente](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorldRawOwin/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="b80a7-120">**Hello World Raw OWIN Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorldRawOwin/ReadMe.txt)</span></span>  
<span data-ttu-id="b80a7-121">En este ejemplo se muestra cómo escribir una aplicación Hello World mediante el **sin formato** abstracción OWIN y ejecutarlo en un servidor web como Asp.Net.</span><span class="sxs-lookup"><span data-stu-id="b80a7-121">This sample demonstrates how to write a Hello World application using the **raw** OWIN abstraction and run it on a web server like Asp.Net.</span></span>

<span data-ttu-id="b80a7-122">**Ejemplo de SignalR** | [código fuente](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/SignalR/Program.cs)</span><span class="sxs-lookup"><span data-stu-id="b80a7-122">**SignalR Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/SignalR/Program.cs)</span></span>  
<span data-ttu-id="b80a7-123">Muestra cómo autohospedaje SignalR con OWIN / Katana.</span><span class="sxs-lookup"><span data-stu-id="b80a7-123">Shows how to self-host SignalR using OWIN / Katana.</span></span> <span data-ttu-id="b80a7-124">Para obtener más información acerca de autohospedaje SignalR, consulte [Tutorial: SignalR autohospedaje](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span><span class="sxs-lookup"><span data-stu-id="b80a7-124">For more info about self-hosting SignalR, see [Tutorial: SignalR Self-Host](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span></span>

<span data-ttu-id="b80a7-125">**Ejemplo de archivos estáticos** | [código fuente](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/StaticFilesSample/Startup.cs) </span><span class="sxs-lookup"><span data-stu-id="b80a7-125">**Static Files Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/StaticFilesSample/Startup.cs) </span></span>  
<span data-ttu-id="b80a7-126">Muestra cómo admitir las solicitudes HTTP para archivos estáticos con OWIN / Katana.</span><span class="sxs-lookup"><span data-stu-id="b80a7-126">Shows how to support HTTP requests for static files using OWIN / Katana.</span></span>

<span data-ttu-id="b80a7-127">**Web API** | [código fuente](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebApi/ReadMe.txt) </span><span class="sxs-lookup"><span data-stu-id="b80a7-127">**Web API** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebApi/ReadMe.txt) </span></span>  
<span data-ttu-id="b80a7-128">Este ejemplo muestra cómo hospedar OWIN en IIS y agregar la API Web a la canalización OWIN.</span><span class="sxs-lookup"><span data-stu-id="b80a7-128">This sample shows how to host OWIN in IIS and add Web API to the OWIN pipeline.</span></span>

<span data-ttu-id="b80a7-129">**Ejemplo de Socket de Web** | [código fuente](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebSocketSample/WebSocketServer/Startup.cs) </span><span class="sxs-lookup"><span data-stu-id="b80a7-129">**Web Socket Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebSocketSample/WebSocketServer/Startup.cs) </span></span>  
<span data-ttu-id="b80a7-130">Muestra cómo admitir Web Sockets en OWIN mediante el uso de la [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) clase.</span><span class="sxs-lookup"><span data-stu-id="b80a7-130">Shows how to support Web Sockets in OWIN by using the [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) class.</span></span>
