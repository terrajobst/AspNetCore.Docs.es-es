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
ms.locfileid: "28043212"
---
<a name="using-signalr-with-web-apps-in-azure-app-service"></a>Uso de SignalR con las aplicaciones Web en el servicio de aplicación de Azure
====================
por [Patrick Fletcher](https://github.com/pfletcher)

> Este documento describe cómo configurar una aplicación de SignalR que se ejecuta en Microsoft Azure.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) o Visual Studio 2012
> - .NET 4.5
> - SignalR versión 2
> - Azure SDK 2.3 para Visual Studio 2013 o 2012
>   
> 
> 
> ## <a name="questions-and-comments"></a>Preguntas y comentarios
> 
> Vota sobre cómo le gustó este tutorial y lo que podemos mejorar en los comentarios en la parte inferior de la página. Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicar para la [foro de ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), [StackOverflow.com](http://stackoverflow.com/), o [foros de Microsoft Azure](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).


## <a name="table-of-contents"></a>Tabla de contenido

- [Introducción](#introduction)
- [Implementar una aplicación Web de SignalR para el servicio de aplicaciones de Azure](#deploying)
- [Habilitar WebSockets en el servicio de aplicaciones de Azure](#websocket)
- [Usar el Backplane de caché de Redis de Azure](#backplane)
- [Pasos siguientes](#nextsteps)

<a id="introduction"></a>
## <a name="introduction"></a>Introducción

ASP.NET SignalR puede utilizarse para abrir un nuevo nivel de interactividad entre servidores y web o clientes de. NET. Cuando se hospeda en Azure, las aplicaciones de SignalR pueden aprovechar la alta disponibilidad, escalable y entorno de rendimiento que se ejecutan en la nube proporciona.

<a id="deploying"></a>
## <a name="deploying-a-signalr-web-app-to-azure-app-service"></a>Implementar una aplicación Web de SignalR para el servicio de aplicaciones de Azure

SignalR no agrega complicaciones determinadas para implementar una aplicación en Azure frente a implementar en un servidor local. Una aplicación que usa SignalR puede estar hospedada en Azure sin realizar ningún cambio en la configuración o de otros valores de configuración (aunque por compatibilidad con WebSockets, consulte [habilitar WebSockets en el servicio de aplicaciones de Azure](#websocket) a continuación.) En este tutorial, va a implementar la aplicación creada en el [Tutorial de introducción](../getting-started/tutorial-getting-started-with-signalr.md) en Azure.

**Requisitos previos**

- Visual Studio 2013. Si no tiene Visual Studio, Visual Studio 2013 Express para Web se incluye en la instalación del SDK de Azure.
- [Azure SDK 2.3 para Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) o [SDK 2.3 de Azure para Visual Studio 2012](https://go.microsoft.com/fwlink/p/?linkid=323511).
- Para completar este tutorial, necesitará una suscripción de Azure. También puede [activar las ventajas de suscriptor MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), o [registrarse para obtener una suscripción de prueba](https://azure.microsoft.com/pricing/free-trial/).

### <a name="deploying-a-signalr-web-app-to-azure"></a>Implementar una aplicación web de SignalR en Azure

1. Completar la [Tutorial de introducción](../getting-started/tutorial-getting-started-with-signalr.md), o descargar el proyecto terminado de [Galería de código](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).
2. En Visual Studio, seleccione **generar**, **publicar SignalR Chat**.
3. En el cuadro de diálogo "Publicar Web", seleccione "sitios Web de Windows Azure".

    ![Seleccione los sitios Web de Azure](using-signalr-with-azure-web-sites/_static/image1.png)
4. Si no has iniciado sesión en su cuenta de Microsoft, haga clic en **inicio de sesión...**  en el cuadro de diálogo "Seleccionar existente sitios Web" y de inicio de sesión.

    ![Seleccione el sitio Web existente](using-signalr-with-azure-web-sites/_static/image2.png)    ![Inicie sesión en Azure](using-signalr-with-azure-web-sites/_static/image3.png)
5. En el cuadro de diálogo "Seleccionar existente Web Site", haga clic en **nuevo**.

    ![Nuevo sitio web](using-signalr-with-azure-web-sites/_static/image4.png)
6. En el cuadro de diálogo "Crear sitio en Windows Azure", escriba un nombre de aplicación único. Seleccione la región más cercana a usted en la lista desplegable de la región. Haga clic en **Crear**.

    ![Crear sitio en Azure](using-signalr-with-azure-web-sites/_static/image5.png)
7. En el cuadro de diálogo "Publicar Web", haga clic en **publicar**.

    ![Publicar sitio](using-signalr-with-azure-web-sites/_static/image6.png)
8. Cuando la aplicación se haya completado la publicación, la aplicación de SignalR Chat alojada en aplicaciones de Web del servicio de aplicación de Azure se abrirá en un explorador.

    ![Sitio abrir en un explorador](using-signalr-with-azure-web-sites/_static/image7.png)

<a id="websocket"></a>
### <a name="enabling-websockets-on-azure-app-service-web-apps"></a>Habilitar WebSockets en aplicaciones Web del servicio de aplicación de Azure

WebSockets debe habilitarse explícitamente en la aplicación web que se usará en una aplicación de SignalR; en caso contrario, se usará otros protocolos (vea [transportes y reservas](../getting-started/introduction-to-signalr.md#transports) para obtener más información).

Para poder usar WebSockets en aplicaciones de Web del servicio de aplicación de Azure, debe habilitarla en la sección de configuración de la aplicación web. Para ello, abra la aplicación web en el [Portal de administración de Azure](https://manage.windowsazure.com/)y haga clic en configurar.

![Pestaña Configurar](using-signalr-with-azure-web-sites/_static/image8.png)

En la parte superior de la página de configuración, asegúrese de que .NET 4.5 se usa para la aplicación web.

![Configuración de la versión 4.5 de .NET framework](using-signalr-with-azure-web-sites/_static/image9.png)

En la página de configuración, en la **WebSockets** configuración, seleccione **en**.

![Configuración de WebSockets: en](using-signalr-with-azure-web-sites/_static/image10.png)

En la parte inferior de la página de configuración, seleccione **guardar** para guardar los cambios.

![Guardar la configuración](using-signalr-with-azure-web-sites/_static/image11.png)

<a id="backplane"></a>
## <a name="using-the-azure-redis-cache-backplane"></a>Usar el Backplane de caché de Redis de Azure

Si se usan varias instancias de la aplicación web y los usuarios de esas instancias deben interactuar entre sí (de modo que, por ejemplo, mensajes de chat creados en una instancia pueden llegar a los usuarios conectados a otras instancias), el [caché en Redis de Azure backplane](../performance/scaleout-with-redis.md) deben implementarse en la aplicación.

<a id="nextsteps"></a>
## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre las aplicaciones Web en el servicio de aplicación de Azure, consulte [información general de aplicaciones Web](https://azure.microsoft.com/documentation/articles/app-service-web-overview/).
