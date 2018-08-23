---
uid: aspnet/overview/owin-and-katana/katana-samples
title: Ejemplos de Katana | Microsoft Docs
author: microsoft
description: ''
ms.author: riande
ms.date: 01/17/2014
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: b8ce2b40a19e0429f1ccedb03b8f829582652d24
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41839146"
---
<a name="katana-samples"></a><span data-ttu-id="0a859-102">Ejemplos de Katana</span><span class="sxs-lookup"><span data-stu-id="0a859-102">Katana Samples</span></span>
====================
<span data-ttu-id="0a859-103">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="0a859-103">by [Microsoft](https://github.com/microsoft)</span></span>

## <a name="katana-samples"></a><span data-ttu-id="0a859-104">Ejemplos de Katana</span><span class="sxs-lookup"><span data-stu-id="0a859-104">Katana Samples</span></span>

<span data-ttu-id="0a859-105">**ASP.NET enruta ejemplo** | [código fuente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/AspNetRoutes)</span><span class="sxs-lookup"><span data-stu-id="0a859-105">**ASP.NET Routes Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/AspNetRoutes)</span></span>  
<span data-ttu-id="0a859-106">En algunas aplicaciones desea enlazar los componentes de OWIN en la tabla de rutas de Asp.Net en paralelo con componentes que no sean OWIN.</span><span class="sxs-lookup"><span data-stu-id="0a859-106">In some applications you will want to hook up OWIN components in the Asp.Net route table side by side with non-OWIN components.</span></span> <span data-ttu-id="0a859-107">En este ejemplo se muestra cómo usar los métodos de extensión RouteCollection MapOwinPath y MapOwinRoute proporcionada por Microsoft.Owin.Host.SystemWeb.</span><span class="sxs-lookup"><span data-stu-id="0a859-107">This sample shows how to use the RouteCollection extension methods MapOwinPath and MapOwinRoute provided by Microsoft.Owin.Host.SystemWeb.</span></span>

<span data-ttu-id="0a859-108">**Bifurcación canalizaciones de ejemplo** | [código fuente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/BranchingPipelines)</span><span class="sxs-lookup"><span data-stu-id="0a859-108">**Branching Pipelines Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/BranchingPipelines)</span></span>  
<span data-ttu-id="0a859-109">Las canalizaciones de procesamiento de solicitudes OWIN no necesita ser lineal, se pueden bifurcar para procesar las solicitudes de maneras diferentes.</span><span class="sxs-lookup"><span data-stu-id="0a859-109">OWIN request processing pipelines do not need to be linear, they can be branched to process requests in different ways.</span></span> <span data-ttu-id="0a859-110">En este ejemplo se muestra cómo construir una canalización de bifurcación en función de las rutas de acceso de solicitud u otros datos de solicitud como los encabezados.</span><span class="sxs-lookup"><span data-stu-id="0a859-110">This sample shows how to construct a branching pipeline based on request paths or other request data such as headers.</span></span> <span data-ttu-id="0a859-111">Estos componentes están disponibles en el paquete de nuget Microsoft.Owin.Mapping.</span><span class="sxs-lookup"><span data-stu-id="0a859-111">These components are available in the Microsoft.Owin.Mapping nuget package.</span></span>

<span data-ttu-id="0a859-112">**Ejemplo de servidor personalizado** | [código fuente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/CustomServer) </span><span class="sxs-lookup"><span data-stu-id="0a859-112">**Custom Server Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/CustomServer) </span></span>  
<span data-ttu-id="0a859-113">Se muestra cómo usar un servidor OWIN personalizado cuando autohospedaje OWIN.</span><span class="sxs-lookup"><span data-stu-id="0a859-113">Shows how to use a custom OWIN server when self-hosting OWIN.</span></span>

<span data-ttu-id="0a859-114">**Embedded ejemplo** | [código fuente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/Embedded)</span><span class="sxs-lookup"><span data-stu-id="0a859-114">**Embedded Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/Embedded)</span></span>  
<span data-ttu-id="0a859-115">Algunos servidores OWIN se pueden ejecutar dentro de su propio proceso (&quot;autohospedado&quot;).</span><span class="sxs-lookup"><span data-stu-id="0a859-115">Some OWIN servers can be run inside of your own process (&quot;self-hosted&quot;).</span></span> <span data-ttu-id="0a859-116">Este ejemplo muestra cómo iniciar una aplicación OWIN mediante las herramientas proporcionadas por el paquete de nuget Microsoft.Owin.Hosting.</span><span class="sxs-lookup"><span data-stu-id="0a859-116">This sample shows how to start an OWIN application using the tools provided by the Microsoft.Owin.Hosting nuget package.</span></span>

<span data-ttu-id="0a859-117">**Ejemplo HelloWorld** | [código fuente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorld)</span><span class="sxs-lookup"><span data-stu-id="0a859-117">**HelloWorld Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorld)</span></span>  
<span data-ttu-id="0a859-118">OWIN es un servidor HTTP de abstracción de API que permite la portabilidad de aplicaciones entre distintos servidores.</span><span class="sxs-lookup"><span data-stu-id="0a859-118">OWIN is a HTTP server API abstraction that enables application portability across various servers.</span></span> <span data-ttu-id="0a859-119">Este ejemplo muestra cómo escribir una aplicación Hello World con algunos **contenedores sencillos** en torno a la abstracción de OWIN y ejecute sin procesar en un servidor web como ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="0a859-119">This sample demonstrates how to write a Hello World application using some **simple wrappers** around the raw OWIN abstraction and run it on a web server like ASP.NET.</span></span>

<span data-ttu-id="0a859-120">**Ejemplo de Hello World Raw OWIN** | [código fuente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorldRawOwin)</span><span class="sxs-lookup"><span data-stu-id="0a859-120">**Hello World Raw OWIN Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorldRawOwin)</span></span>  
<span data-ttu-id="0a859-121">Este ejemplo muestra cómo escribir una aplicación Hello World con la **raw** abstracción OWIN y ejecútelo en un servidor web, como Asp.Net.</span><span class="sxs-lookup"><span data-stu-id="0a859-121">This sample demonstrates how to write a Hello World application using the **raw** OWIN abstraction and run it on a web server like Asp.Net.</span></span>

<span data-ttu-id="0a859-122">**Ejemplo de SignalR** | [código fuente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/SignalR)</span><span class="sxs-lookup"><span data-stu-id="0a859-122">**SignalR Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/SignalR)</span></span>  
<span data-ttu-id="0a859-123">Se muestra cómo el autohospedaje de SignalR con OWIN o Katana.</span><span class="sxs-lookup"><span data-stu-id="0a859-123">Shows how to self-host SignalR using OWIN / Katana.</span></span> <span data-ttu-id="0a859-124">Para obtener más información sobre cómo autoalojamiento SignalR, consulte [Tutorial: interna de SignalR](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span><span class="sxs-lookup"><span data-stu-id="0a859-124">For more info about self-hosting SignalR, see [Tutorial: SignalR Self-Host](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span></span>

<span data-ttu-id="0a859-125">**Ejemplo de archivos estáticos de** | [código fuente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/StaticFilesSample) </span><span class="sxs-lookup"><span data-stu-id="0a859-125">**Static Files Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/StaticFilesSample) </span></span>  
<span data-ttu-id="0a859-126">Muestra cómo admitir solicitudes HTTP para los archivos estáticos con OWIN o Katana.</span><span class="sxs-lookup"><span data-stu-id="0a859-126">Shows how to support HTTP requests for static files using OWIN / Katana.</span></span>

<span data-ttu-id="0a859-127">**API Web** | [código fuente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebApi) </span><span class="sxs-lookup"><span data-stu-id="0a859-127">**Web API** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebApi) </span></span>  
<span data-ttu-id="0a859-128">En este ejemplo se muestra cómo hospedar OWIN en IIS y agregar la API Web a la canalización OWIN.</span><span class="sxs-lookup"><span data-stu-id="0a859-128">This sample shows how to host OWIN in IIS and add Web API to the OWIN pipeline.</span></span>

<span data-ttu-id="0a859-129">**Ejemplo de Socket de Web** | [código fuente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebSocketSample) </span><span class="sxs-lookup"><span data-stu-id="0a859-129">**Web Socket Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebSocketSample) </span></span>  
<span data-ttu-id="0a859-130">Muestra cómo se admite Web Sockets en OWIN mediante el uso de la [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) clase.</span><span class="sxs-lookup"><span data-stu-id="0a859-130">Shows how to support Web Sockets in OWIN by using the [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) class.</span></span>
