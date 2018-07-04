---
uid: signalr/overview/deployment/using-signalr-with-azure-web-sites
title: Uso de SignalR con Web Apps en Azure App Service | Microsoft Docs
author: pfletcher
description: Este documento describe cómo configurar una aplicación de SignalR que se ejecuta en Microsoft Azure. Las versiones de software usan en el tutorial de Visual Studio 2013 o vis...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/01/2015
ms.topic: article
ms.assetid: 2a7517a0-b88c-4162-ade3-9bf6ca7062fd
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/deployment/using-signalr-with-azure-web-sites
msc.type: authoredcontent
ms.openlocfilehash: dabf0f6cfed401e10d2c1134c260022d94c3ab92
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389580"
---
<a name="using-signalr-with-web-apps-in-azure-app-service"></a>Uso de SignalR con Web Apps en Azure App Service
====================
por [Patrick Fletcher](https://github.com/pfletcher)

> Este documento describe cómo configurar una aplicación de SignalR que se ejecuta en Microsoft Azure.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) o Visual Studio 2012
> - .NET 4.5
> - Versión 2 de SignalR
> - Azure SDK 2.3 para Visual Studio 2013 o 2012
>   
> 
> 
> ## <a name="questions-and-comments"></a>Preguntas y comentarios
> 
> Deje comentarios sobre cómo le gustó de este tutorial y que podíamos mejorar en los comentarios en la parte inferior de la página. Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicarlos en el [foro de ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), [StackOverflow.com](http://stackoverflow.com/), o el [foros de Microsoft Azure](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).


## <a name="table-of-contents"></a>Tabla de contenido

- [Introducción](#introduction)
- [Implementar una aplicación Web de SignalR en Azure App Service](#deploying)
- [Habilitar WebSockets en Azure App Service](#websocket)
- [Utilizando el Backplane de Azure Redis Cache](#backplane)
- [Pasos siguientes](#nextsteps)

<a id="introduction"></a>
## <a name="introduction"></a>Introducción

SignalR de ASP.NET puede utilizarse para abrir un nuevo nivel de interactividad entre servidores y web o los clientes. NET. Cuando se hospeda en Azure, SignalR las aplicaciones pueden aprovechar la alta disponibilidad, escalable y eficaz entorno que está ejecutando en la nube proporciona.

<a id="deploying"></a>
## <a name="deploying-a-signalr-web-app-to-azure-app-service"></a>Implementar una aplicación Web de SignalR en Azure App Service

SignalR no agrega ninguna complicación concreto para implementar una aplicación en Azure frente a implementar en un servidor local. Una aplicación que usa SignalR se puede hospedar en Azure sin realizar ningún cambio en la configuración o de otros valores (sin embargo, para la compatibilidad con WebSockets, consulte [habilitar WebSockets en Azure App Service](#websocket) a continuación.) Para este tutorial, implementará la aplicación creada en el [Tutorial de introducción](../getting-started/tutorial-getting-started-with-signalr.md) en Azure.

**Requisitos previos**

- Visual Studio 2013. Si no tiene Visual Studio, Visual Studio 2013 Express para Web se incluye en la instalación del SDK de Azure.
- [Azure SDK 2.3 para Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) o [Azure SDK 2.3 para Visual Studio 2012](https://go.microsoft.com/fwlink/p/?linkid=323511).
- Para completar este tutorial, necesitará una suscripción de Azure. También puede [activar las ventajas de suscriptor MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), o [registrarse para obtener una suscripción de prueba](https://azure.microsoft.com/pricing/free-trial/).

### <a name="deploying-a-signalr-web-app-to-azure"></a>Implementar una aplicación web de SignalR en Azure

1. Completar la [Tutorial de introducción](../getting-started/tutorial-getting-started-with-signalr.md), o descargar el proyecto finalizado desde [Galería de código](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).
2. En Visual Studio, seleccione **compilar**, **publicar SignalR Chat**.
3. En el cuadro de diálogo "Publicar Web", seleccione "sitios Web Windows Azure".

    ![Seleccione los sitios Web de Azure](using-signalr-with-azure-web-sites/_static/image1.png)
4. Si no has iniciado sesión con tu cuenta Microsoft, haga clic en **sesión...**  en el cuadro de diálogo "Seleccionar existente sitio Web" e inicie sesión.

    ![Seleccione el sitio Web existente](using-signalr-with-azure-web-sites/_static/image2.png)    ![Iniciar sesión en Azure](using-signalr-with-azure-web-sites/_static/image3.png)
5. En el cuadro de diálogo "Seleccionar sitio de Web existente", haga clic en **New**.

    ![Nuevo sitio web](using-signalr-with-azure-web-sites/_static/image4.png)
6. En el cuadro de diálogo "Crear sitio en Windows Azure", escriba un nombre de aplicación único. Seleccione la región más cercana a usted en la lista desplegable de la región. Haga clic en **Crear**.

    ![Crear sitio en Azure](using-signalr-with-azure-web-sites/_static/image5.png)
7. En el cuadro de diálogo "Publicar Web", haga clic en **publicar**.

    ![Publicar el sitio](using-signalr-with-azure-web-sites/_static/image6.png)
8. Cuando la aplicación se haya completado la publicación, se abrirá la aplicación de SignalR Chat hospedada en Azure App Service Web Apps en un explorador.

    ![Abrir en un explorador de sitio](using-signalr-with-azure-web-sites/_static/image7.png)

<a id="websocket"></a>
### <a name="enabling-websockets-on-azure-app-service-web-apps"></a>Habilitar WebSockets en Azure App Service Web Apps

Debe habilitarse explícitamente en la aplicación web que se usará en una aplicación de SignalR; WebSockets en caso contrario, se usará otros protocolos (consulte [transportes y reservas](../getting-started/introduction-to-signalr.md#transports) para obtener más información).

Para poder usar WebSockets en Azure App Service Web Apps, habilitarla en la sección de configuración de la aplicación web. Para ello, abra la aplicación web en el [Portal de administración de Azure](https://manage.windowsazure.com/)y seleccione Configurar.

![Pestaña Configurar](using-signalr-with-azure-web-sites/_static/image8.png)

En la parte superior de la página de configuración, asegúrese de que se usa .NET 4.5 para la aplicación web.

![Configuración de la versión 4.5 de .NET framework](using-signalr-with-azure-web-sites/_static/image9.png)

En la página de configuración, en el **WebSockets** , seleccione **en**.

![Configuración de WebSockets: en](using-signalr-with-azure-web-sites/_static/image10.png)

En la parte inferior de la página de configuración, seleccione **guardar** para guardar los cambios.

![Guardar configuración](using-signalr-with-azure-web-sites/_static/image11.png)

<a id="backplane"></a>
## <a name="using-the-azure-redis-cache-backplane"></a>Utilizando el Backplane de Azure Redis Cache

Si se usan varias instancias de la aplicación web y los usuarios de esas instancias deben interactuar entre sí (de modo que, por ejemplo, los mensajes de chat creados en una instancia pueden llegar a los usuarios conectados a otras instancias), el [Azure Redis Cache backplane](../performance/scaleout-with-redis.md) debe implementarse en la aplicación.

<a id="nextsteps"></a>
## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre las aplicaciones Web en Azure App Service, consulte [Introducción a Web Apps](https://azure.microsoft.com/documentation/articles/app-service-web-overview/).
