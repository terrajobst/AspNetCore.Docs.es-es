---
uid: signalr/overview/deployment/using-signalr-with-azure-web-sites
title: Con SignalR con las aplicaciones Web en el servicio de aplicación de Azure | Documentos de Microsoft
author: pfletcher
description: Este documento describe cómo configurar una aplicación de SignalR que se ejecuta en Microsoft Azure. Versiones de software usan en el tutorial de Visual Studio 2013 o vis...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/01/2015
ms.topic: article
ms.assetid: 2a7517a0-b88c-4162-ade3-9bf6ca7062fd
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/deployment/using-signalr-with-azure-web-sites
msc.type: authoredcontent
ms.openlocfilehash: 8386441690a3fb479ffb941ebd7c0b2f83870781
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="using-signalr-with-web-apps-in-azure-app-service"></a><span data-ttu-id="883aa-104">Uso de SignalR con las aplicaciones Web en el servicio de aplicación de Azure</span><span class="sxs-lookup"><span data-stu-id="883aa-104">Using SignalR with Web Apps in Azure App Service</span></span>
====================
<span data-ttu-id="883aa-105">por [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="883aa-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="883aa-106">Este documento describe cómo configurar una aplicación de SignalR que se ejecuta en Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="883aa-106">This document describes how to configure a SignalR application that runs on Microsoft Azure.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="883aa-107">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="883aa-107">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="883aa-108">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) o Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="883aa-108">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) or Visual Studio 2012</span></span>
> - <span data-ttu-id="883aa-109">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="883aa-109">.NET 4.5</span></span>
> - <span data-ttu-id="883aa-110">SignalR versión 2</span><span class="sxs-lookup"><span data-stu-id="883aa-110">SignalR version 2</span></span>
> - <span data-ttu-id="883aa-111">Azure SDK 2.3 para Visual Studio 2013 o 2012</span><span class="sxs-lookup"><span data-stu-id="883aa-111">Azure SDK 2.3 for Visual Studio 2013 or 2012</span></span>
>   
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="883aa-112">Preguntas y comentarios</span><span class="sxs-lookup"><span data-stu-id="883aa-112">Questions and comments</span></span>
> 
> <span data-ttu-id="883aa-113">Vota sobre cómo le gustó este tutorial y lo que podemos mejorar en los comentarios en la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="883aa-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="883aa-114">Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicar para la [foro de ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), [StackOverflow.com](http://stackoverflow.com/), o [foros de Microsoft Azure](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).</span><span class="sxs-lookup"><span data-stu-id="883aa-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), [StackOverflow.com](http://stackoverflow.com/), or the [Microsoft Azure forums](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).</span></span>


## <a name="table-of-contents"></a><span data-ttu-id="883aa-115">Tabla de contenido</span><span class="sxs-lookup"><span data-stu-id="883aa-115">Table of Contents</span></span>

- [<span data-ttu-id="883aa-116">Introducción</span><span class="sxs-lookup"><span data-stu-id="883aa-116">Introduction</span></span>](#introduction)
- [<span data-ttu-id="883aa-117">Implementar una aplicación Web de SignalR para el servicio de aplicaciones de Azure</span><span class="sxs-lookup"><span data-stu-id="883aa-117">Deploying a SignalR Web App to Azure App Service</span></span>](#deploying)
- [<span data-ttu-id="883aa-118">Habilitar WebSockets en el servicio de aplicaciones de Azure</span><span class="sxs-lookup"><span data-stu-id="883aa-118">Enabling WebSockets on Azure App Service</span></span>](#websocket)
- [<span data-ttu-id="883aa-119">Usar el Backplane de caché de Redis de Azure</span><span class="sxs-lookup"><span data-stu-id="883aa-119">Using the Azure Redis Cache Backplane</span></span>](#backplane)
- [<span data-ttu-id="883aa-120">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="883aa-120">Next Steps</span></span>](#nextsteps)

<a id="introduction"></a>
## <a name="introduction"></a><span data-ttu-id="883aa-121">Introducción</span><span class="sxs-lookup"><span data-stu-id="883aa-121">Introduction</span></span>

<span data-ttu-id="883aa-122">ASP.NET SignalR puede utilizarse para abrir un nuevo nivel de interactividad entre servidores y web o clientes de. NET.</span><span class="sxs-lookup"><span data-stu-id="883aa-122">ASP.NET SignalR can be used to bring a new level of interactivity between servers and web or .NET clients.</span></span> <span data-ttu-id="883aa-123">Cuando se hospeda en Azure, las aplicaciones de SignalR pueden aprovechar la alta disponibilidad, escalable y entorno de rendimiento que se ejecutan en la nube proporciona.</span><span class="sxs-lookup"><span data-stu-id="883aa-123">When hosted in Azure, SignalR applications can take advantage of the highly available, scalable, and performant environment that running in the cloud provides.</span></span>

<a id="deploying"></a>
## <a name="deploying-a-signalr-web-app-to-azure-app-service"></a><span data-ttu-id="883aa-124">Implementar una aplicación Web de SignalR para el servicio de aplicaciones de Azure</span><span class="sxs-lookup"><span data-stu-id="883aa-124">Deploying a SignalR Web App to Azure App Service</span></span>

<span data-ttu-id="883aa-125">SignalR no agrega complicaciones determinadas para implementar una aplicación en Azure frente a implementar en un servidor local.</span><span class="sxs-lookup"><span data-stu-id="883aa-125">SignalR doesn't add any particular complications to deploying an application to Azure versus deploying to an on-premises server.</span></span> <span data-ttu-id="883aa-126">Una aplicación que usa SignalR puede estar hospedada en Azure sin realizar ningún cambio en la configuración o de otros valores de configuración (aunque por compatibilidad con WebSockets, consulte [habilitar WebSockets en el servicio de aplicaciones de Azure](#websocket) a continuación.) En este tutorial, va a implementar la aplicación creada en el [Tutorial de introducción](../getting-started/tutorial-getting-started-with-signalr.md) en Azure.</span><span class="sxs-lookup"><span data-stu-id="883aa-126">An application that uses SignalR can be hosted in Azure without any changes in configuration or other settings (though for WebSockets support, see [Enabling WebSockets on Azure App Service](#websocket) below.) For this tutorial, you'll deploy the application created in the [Getting Started Tutorial](../getting-started/tutorial-getting-started-with-signalr.md) to Azure.</span></span>

<span data-ttu-id="883aa-127">**Requisitos previos**</span><span class="sxs-lookup"><span data-stu-id="883aa-127">**Prerequisites**</span></span>

- <span data-ttu-id="883aa-128">Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="883aa-128">Visual Studio 2013.</span></span> <span data-ttu-id="883aa-129">Si no tiene Visual Studio, Visual Studio 2013 Express para Web se incluye en la instalación del SDK de Azure.</span><span class="sxs-lookup"><span data-stu-id="883aa-129">If you don't have Visual Studio, Visual Studio 2013 Express for Web is included in the install of the Azure SDK.</span></span>
- <span data-ttu-id="883aa-130">[Azure SDK 2.3 para Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) o [SDK 2.3 de Azure para Visual Studio 2012](https://go.microsoft.com/fwlink/p/?linkid=323511).</span><span class="sxs-lookup"><span data-stu-id="883aa-130">[Azure SDK 2.3 for Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) or [Azure SDK 2.3 for Visual Studio 2012](https://go.microsoft.com/fwlink/p/?linkid=323511).</span></span>
- <span data-ttu-id="883aa-131">Para completar este tutorial, necesitará una suscripción de Azure.</span><span class="sxs-lookup"><span data-stu-id="883aa-131">To complete this tutorial, you will need an Azure subscription.</span></span> <span data-ttu-id="883aa-132">También puede [activar las ventajas de suscriptor MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), o [registrarse para obtener una suscripción de prueba](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="883aa-132">You can [activate your MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), or [sign up for a trial subscription](https://azure.microsoft.com/pricing/free-trial/).</span></span>

### <a name="deploying-a-signalr-web-app-to-azure"></a><span data-ttu-id="883aa-133">Implementar una aplicación web de SignalR en Azure</span><span class="sxs-lookup"><span data-stu-id="883aa-133">Deploying a SignalR web app to Azure</span></span>

1. <span data-ttu-id="883aa-134">Completar la [Tutorial de introducción](../getting-started/tutorial-getting-started-with-signalr.md), o descargar el proyecto terminado de [Galería de código](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span><span class="sxs-lookup"><span data-stu-id="883aa-134">Complete the [Getting Started Tutorial](../getting-started/tutorial-getting-started-with-signalr.md), or download the finished project from [Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span></span>
2. <span data-ttu-id="883aa-135">En Visual Studio, seleccione **generar**, **publicar SignalR Chat**.</span><span class="sxs-lookup"><span data-stu-id="883aa-135">In Visual Studio, select **Build**, **Publish SignalR Chat**.</span></span>
3. <span data-ttu-id="883aa-136">En el cuadro de diálogo "Publicar Web", seleccione "sitios Web de Windows Azure".</span><span class="sxs-lookup"><span data-stu-id="883aa-136">In the "Publish Web" dialog, select "Windows Azure Web Sites".</span></span>

    ![Seleccione los sitios Web de Azure](using-signalr-with-azure-web-sites/_static/image1.png)
4. <span data-ttu-id="883aa-138">Si no has iniciado sesión en su cuenta de Microsoft, haga clic en **inicio de sesión...**  en el cuadro de diálogo "Seleccionar existente sitios Web" y de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="883aa-138">If you aren't signed in to your Microsoft account, click **Sign In...** in the "Select Existing Web Site" dialog, and sign in.</span></span>

    ![Seleccione el sitio Web existente](using-signalr-with-azure-web-sites/_static/image2.png)    ![Inicie sesión en Azure](using-signalr-with-azure-web-sites/_static/image3.png)
5. <span data-ttu-id="883aa-141">En el cuadro de diálogo "Seleccionar existente Web Site", haga clic en **nuevo**.</span><span class="sxs-lookup"><span data-stu-id="883aa-141">In the "Select Existing Web Site" dialog, click **New**.</span></span>

    ![Nuevo sitio web](using-signalr-with-azure-web-sites/_static/image4.png)
6. <span data-ttu-id="883aa-143">En el cuadro de diálogo "Crear sitio en Windows Azure", escriba un nombre de aplicación único.</span><span class="sxs-lookup"><span data-stu-id="883aa-143">In the "Create site on Windows Azure" dialog, enter a unique app name.</span></span> <span data-ttu-id="883aa-144">Seleccione la región más cercana a usted en la lista desplegable de la región.</span><span class="sxs-lookup"><span data-stu-id="883aa-144">Select the region closest to you in the Region dropdown.</span></span> <span data-ttu-id="883aa-145">Haga clic en **Crear**.</span><span class="sxs-lookup"><span data-stu-id="883aa-145">Click **Create**.</span></span>

    ![Crear sitio en Azure](using-signalr-with-azure-web-sites/_static/image5.png)
7. <span data-ttu-id="883aa-147">En el cuadro de diálogo "Publicar Web", haga clic en **publicar**.</span><span class="sxs-lookup"><span data-stu-id="883aa-147">In the "Publish Web" dialog, click **Publish**.</span></span>

    ![Publicar sitio](using-signalr-with-azure-web-sites/_static/image6.png)
8. <span data-ttu-id="883aa-149">Cuando la aplicación se haya completado la publicación, la aplicación de SignalR Chat alojada en aplicaciones de Web del servicio de aplicación de Azure se abrirá en un explorador.</span><span class="sxs-lookup"><span data-stu-id="883aa-149">When the app has completed publishing, the SignalR Chat application hosted in Azure App Service Web Apps will open in a browser.</span></span>

    ![Sitio abrir en un explorador](using-signalr-with-azure-web-sites/_static/image7.png)

<a id="websocket"></a>
### <a name="enabling-websockets-on-azure-app-service-web-apps"></a><span data-ttu-id="883aa-151">Habilitar WebSockets en aplicaciones Web del servicio de aplicación de Azure</span><span class="sxs-lookup"><span data-stu-id="883aa-151">Enabling WebSockets on Azure App Service Web Apps</span></span>

<span data-ttu-id="883aa-152">WebSockets debe habilitarse explícitamente en la aplicación web que se usará en una aplicación de SignalR; en caso contrario, se usará otros protocolos (vea [transportes y reservas](../getting-started/introduction-to-signalr.md#transports) para obtener más información).</span><span class="sxs-lookup"><span data-stu-id="883aa-152">WebSockets needs to be explicitly enabled in your web app to be used in a SignalR application; otherwise, other protocols will be used (See [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports) for details).</span></span>

<span data-ttu-id="883aa-153">Para poder usar WebSockets en aplicaciones de Web del servicio de aplicación de Azure, debe habilitarla en la sección de configuración de la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="883aa-153">In order to use WebSockets on Azure App Service Web Apps, enable it in the configuration section of the web app.</span></span> <span data-ttu-id="883aa-154">Para ello, abra la aplicación web en el [Portal de administración de Azure](https://manage.windowsazure.com/)y haga clic en configurar.</span><span class="sxs-lookup"><span data-stu-id="883aa-154">To do this, open your web app in the [Azure Management Portal](https://manage.windowsazure.com/), and select Configure.</span></span>

![Pestaña Configurar](using-signalr-with-azure-web-sites/_static/image8.png)

<span data-ttu-id="883aa-156">En la parte superior de la página de configuración, asegúrese de que .NET 4.5 se usa para la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="883aa-156">At the top of the configuration page, ensure that .NET 4.5 is used for your web app.</span></span>

![Configuración de la versión 4.5 de .NET framework](using-signalr-with-azure-web-sites/_static/image9.png)

<span data-ttu-id="883aa-158">En la página de configuración, en la **WebSockets** configuración, seleccione **en**.</span><span class="sxs-lookup"><span data-stu-id="883aa-158">On the configuration page, in the **WebSockets** setting, select **On**.</span></span>

![Configuración de WebSockets: en](using-signalr-with-azure-web-sites/_static/image10.png)

<span data-ttu-id="883aa-160">En la parte inferior de la página de configuración, seleccione **guardar** para guardar los cambios.</span><span class="sxs-lookup"><span data-stu-id="883aa-160">At the bottom of the Configuration page, select **Save** to save your changes.</span></span>

![Guardar la configuración](using-signalr-with-azure-web-sites/_static/image11.png)

<a id="backplane"></a>
## <a name="using-the-azure-redis-cache-backplane"></a><span data-ttu-id="883aa-162">Usar el Backplane de caché de Redis de Azure</span><span class="sxs-lookup"><span data-stu-id="883aa-162">Using the Azure Redis Cache Backplane</span></span>

<span data-ttu-id="883aa-163">Si se usan varias instancias de la aplicación web y los usuarios de esas instancias deben interactuar entre sí (de modo que, por ejemplo, mensajes de chat creados en una instancia pueden llegar a los usuarios conectados a otras instancias), el [caché en Redis de Azure backplane](../performance/scaleout-with-redis.md) deben implementarse en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="883aa-163">If you use multiple instances for your web app, and the users of those instances need to interact with one another (so that, for instance, chat messages created in one instance can reach the users connected to other instances), the [Azure Redis Cache backplane](../performance/scaleout-with-redis.md) must be implemented in your application.</span></span>

<a id="nextsteps"></a>
## <a name="next-steps"></a><span data-ttu-id="883aa-164">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="883aa-164">Next Steps</span></span>

<span data-ttu-id="883aa-165">Para obtener más información sobre las aplicaciones Web en el servicio de aplicación de Azure, consulte [información general de aplicaciones Web](https://azure.microsoft.com/documentation/articles/app-service-web-overview/).</span><span class="sxs-lookup"><span data-stu-id="883aa-165">For more information on Web Apps in Azure App Service, see [Web Apps overview](https://azure.microsoft.com/documentation/articles/app-service-web-overview/).</span></span>
