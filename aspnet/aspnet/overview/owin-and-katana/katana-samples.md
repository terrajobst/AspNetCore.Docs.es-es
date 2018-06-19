---
uid: aspnet/overview/owin-and-katana/katana-samples
title: Ejemplos de Katana | Documentos de Microsoft
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2014
ms.topic: article
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: 815355c00c9c15cfefa5f98dc89da676743b0390
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
ms.locfileid: "28034495"
---
<a name="katana-samples"></a>Ejemplos de Katana
====================
por [Microsoft](https://github.com/microsoft)

## <a name="katana-samples"></a>Ejemplos de Katana

**ASP.NET enruta ejemplo** | [código fuente](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/AspNetRoutes/ReadMe.txt)  
En algunas aplicaciones desea enlazar componentes OWIN en la tabla de rutas de Asp.Net paralelo con componentes no OWIN. Este ejemplo muestra cómo utilizar los métodos de extensión RouteCollection MapOwinPath y MapOwinRoute proporcionada por Microsoft.Owin.Host.SystemWeb.

**Bifurcación canalizaciones ejemplo** | [código fuente](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/BranchingPipelines/ReadMe.txt)  
Las canalizaciones de procesamiento de la solicitud OWIN no debe ser lineal, puede ser creó una bifurcación para procesar las solicitudes de maneras diferentes. Este ejemplo muestra cómo construir una canalización de bifurcación en función de las rutas de acceso de solicitud u otros datos de solicitud como encabezados. Estos componentes están disponibles en el paquete de nuget Microsoft.Owin.Mapping.

**Ejemplo de servidor personalizado** | [código fuente](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/CustomServer/MyCustomServer/CustomServer.cs)   
Muestra cómo utilizar un servidor OWIN personalizado cuando se hospeda a sí mismo OWIN.

**Incrusta ejemplo** | [código fuente](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/Embedded/ReadMe.txt)  
Algunos servidores OWIN que se pueden ejecutar dentro de su propio proceso (&quot;hospeda a sí mismo&quot;). Este ejemplo muestra cómo iniciar una aplicación OWIN mediante las herramientas proporcionadas por el paquete de nuget Microsoft.Owin.Hosting.

**Ejemplo HelloWorld** | [código fuente](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorld/ReadMe.txt)  
OWIN es un abstracción de API que permite la portabilidad de aplicación entre varios servidores de servidor HTTP. En este ejemplo se muestra cómo escribir una aplicación Hello World con algún **contenedores sencillos** alrededor de la abstracción de OWIN y ejecutar sin procesar en un servidor web, como ASP.NET.

**Ejemplo de Hello World sin formato OWIN** | [código fuente](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorldRawOwin/ReadMe.txt)  
En este ejemplo se muestra cómo escribir una aplicación Hello World mediante el **sin formato** abstracción OWIN y ejecutarlo en un servidor web como Asp.Net.

**Ejemplo de SignalR** | [código fuente](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/SignalR/Program.cs)  
Muestra cómo autohospedaje SignalR con OWIN / Katana. Para obtener más información acerca de autohospedaje SignalR, consulte [Tutorial: SignalR autohospedaje](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

**Ejemplo de archivos estáticos** | [código fuente](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/StaticFilesSample/Startup.cs)   
Muestra cómo admitir las solicitudes HTTP para archivos estáticos con OWIN / Katana.

**Web API** | [código fuente](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebApi/ReadMe.txt)   
Este ejemplo muestra cómo hospedar OWIN en IIS y agregar la API Web a la canalización OWIN.

**Ejemplo de Socket de Web** | [código fuente](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebSocketSample/WebSocketServer/Startup.cs)   
Muestra cómo admitir Web Sockets en OWIN mediante el uso de la [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) clase.
