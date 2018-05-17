---
title: Módulo ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo el módulo de ASP.NET Core permite que el servidor web de Kestrel use IIS o IIS Express como servidor proxy inverso.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/23/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/aspnet-core-module
ms.openlocfilehash: 789b0b2e74405256bb0e247ae5ea9697e3cb28e5
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/14/2018
---
# <a name="aspnet-core-module"></a><span data-ttu-id="e057e-103">Módulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e057e-103">ASP.NET Core Module</span></span>

<span data-ttu-id="e057e-104">Por [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl) y [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="e057e-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), and [Chris Ross](https://github.com/Tratcher)</span></span> 

<span data-ttu-id="e057e-105">El módulo ASP.NET Core permite a las aplicaciones ASP.NET Core ejecutarse tras IIS en una configuración de proxy inverso.</span><span class="sxs-lookup"><span data-stu-id="e057e-105">The ASP.NET Core Module allows ASP.NET Core apps to run behind IIS in a reverse proxy configuration.</span></span> <span data-ttu-id="e057e-106">IIS proporciona características de administración y seguridad de aplicaciones web avanzadas.</span><span class="sxs-lookup"><span data-stu-id="e057e-106">IIS provides advanced web app security and manageability features.</span></span>

<span data-ttu-id="e057e-107">Versiones de Windows compatibles:</span><span class="sxs-lookup"><span data-stu-id="e057e-107">Supported Windows versions:</span></span>

* <span data-ttu-id="e057e-108">Windows 7 o posterior</span><span class="sxs-lookup"><span data-stu-id="e057e-108">Windows 7 or later</span></span>
* <span data-ttu-id="e057e-109">Windows Server 2008 R2 o posterior</span><span class="sxs-lookup"><span data-stu-id="e057e-109">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="e057e-110">El módulo ASP.NET Core funciona únicamente con Kestrel.</span><span class="sxs-lookup"><span data-stu-id="e057e-110">The ASP.NET Core Module only works with Kestrel.</span></span> <span data-ttu-id="e057e-111">El módulo no es compatible con [HTTP.sys](xref:fundamentals/servers/httpsys) (anteriormente denominado [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="e057e-111">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span>

## <a name="aspnet-core-module-description"></a><span data-ttu-id="e057e-112">Descripción del módulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e057e-112">ASP.NET Core Module description</span></span>

<span data-ttu-id="e057e-113">El módulo ASP.NET Core es un módulo de IIS nativo que se conecta a la canalización IIS para redirigir solicitudes web a aplicaciones ASP.NET Core de back-end.</span><span class="sxs-lookup"><span data-stu-id="e057e-113">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to redirect web requests to backend ASP.NET Core apps.</span></span> <span data-ttu-id="e057e-114">Muchos de los módulos nativos, como la autenticación de Windows, permanecen activos.</span><span class="sxs-lookup"><span data-stu-id="e057e-114">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="e057e-115">Para más información sobre los módulos de IIS activos con el módulo, vea [IIS modules](xref:host-and-deploy/iis/modules) (Módulos de IIS).</span><span class="sxs-lookup"><span data-stu-id="e057e-115">To learn more about IIS modules active with the module, see [IIS modules](xref:host-and-deploy/iis/modules).</span></span>

<span data-ttu-id="e057e-116">Dado que las aplicaciones ASP.NET Core se ejecutan en un proceso independiente del proceso de trabajo de IIS, el módulo también se encarga de la administración de procesos.</span><span class="sxs-lookup"><span data-stu-id="e057e-116">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="e057e-117">El módulo inicia el proceso de la aplicación ASP.NET Core cuando entra la primera solicitud, y reinicia la aplicación si esta se bloquea.</span><span class="sxs-lookup"><span data-stu-id="e057e-117">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="e057e-118">Este comportamiento es básicamente el mismo que el de las aplicaciones ASP.NET 4.x que se ejecutan en el proceso en IIS y se administran a través del [Servicio de activación de procesos de Windows (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="e057e-118">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="e057e-119">En el siguiente diagrama se muestra la relación entre las aplicaciones IIS, el módulo ASP.NET Core y las aplicaciones ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="e057e-119">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and ASP.NET Core apps:</span></span>

![Módulo ASP.NET Core](aspnet-core-module/_static/ancm.png)

<span data-ttu-id="e057e-121">Las solicitudes llegan procedentes de Internet al controlador HTTP.sys en modo kernel.</span><span class="sxs-lookup"><span data-stu-id="e057e-121">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="e057e-122">El controlador enruta las solicitudes a IIS en el puerto configurado del sitio web, que suele ser el puerto 80 (HTTP) o 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="e057e-122">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="e057e-123">El módulo reenvía las solicitudes a Kestrel en un puerto aleatorio de la aplicación, que no es ni 80 ni 443.</span><span class="sxs-lookup"><span data-stu-id="e057e-123">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80/443.</span></span>

<span data-ttu-id="e057e-124">El módulo especifica el puerto a través de la variable de entorno en el inicio y el middleware de integración de IIS configura el servidor para que escuche en `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="e057e-124">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="e057e-125">Se realizan más comprobaciones y se rechazan las solicitudes que no se hayan originado en el módulo.</span><span class="sxs-lookup"><span data-stu-id="e057e-125">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="e057e-126">El módulo no admite el reenvío de HTTPS, por lo que las solicitudes se reenvían a través de HTTP, aunque IIS las reciba a través de HTTPS.</span><span class="sxs-lookup"><span data-stu-id="e057e-126">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="e057e-127">Una vez que Kestrel toma una solicitud del módulo, la envía a la canalización de middleware de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e057e-127">After Kestrel picks up a request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="e057e-128">La canalización de middleware controla la solicitud y la pasa como una instancia de `HttpContext` a la lógica de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="e057e-128">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="e057e-129">La respuesta de la aplicación se vuelve a pasar a IIS, que la envía de nuevo al cliente HTTP que inició la solicitud.</span><span class="sxs-lookup"><span data-stu-id="e057e-129">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

<span data-ttu-id="e057e-130">El módulo ASP.NET Core tiene otras funciones.</span><span class="sxs-lookup"><span data-stu-id="e057e-130">The ASP.NET Core Module has a few other functions.</span></span> <span data-ttu-id="e057e-131">Puede:</span><span class="sxs-lookup"><span data-stu-id="e057e-131">The module can:</span></span>

* <span data-ttu-id="e057e-132">Establecer variables de entorno para un proceso de trabajo.</span><span class="sxs-lookup"><span data-stu-id="e057e-132">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="e057e-133">Registrar la salida en un almacenamiento de archivos para solucionar problemas de inicio.</span><span class="sxs-lookup"><span data-stu-id="e057e-133">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="e057e-134">Reenviar tokens de autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="e057e-134">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="e057e-135">Cómo instalar y usar el módulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e057e-135">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="e057e-136">Para obtener instrucciones detalladas sobre cómo instalar y usar el módulo ASP.NET Core, vea [Hospedaje de ASP.NET Core en Windows con IIS](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="e057e-136">For detailed instructions on how to install and use the ASP.NET Core Module, see [Host on Windows with IIS](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="e057e-137">Para más información sobre cómo configurar el módulo, vea [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module) (Referencia de configuración del módulo de ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="e057e-137">For information on configuring the module, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e057e-138">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="e057e-138">Additional resources</span></span>

* [<span data-ttu-id="e057e-139">Hospedaje en Windows con IIS</span><span class="sxs-lookup"><span data-stu-id="e057e-139">Host on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="e057e-140">Referencia de configuración del módulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e057e-140">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="e057e-141">Repositorio GitHub del módulo ASP.NET Core (código fuente)</span><span class="sxs-lookup"><span data-stu-id="e057e-141">ASP.NET Core Module GitHub repository (source code)</span></span>](https://github.com/aspnet/AspNetCoreModule)
