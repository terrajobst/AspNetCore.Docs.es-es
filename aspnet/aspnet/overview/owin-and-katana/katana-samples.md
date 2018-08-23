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
<a name="katana-samples"></a>Ejemplos de Katana
====================
por [Microsoft](https://github.com/microsoft)

## <a name="katana-samples"></a>Ejemplos de Katana

**ASP.NET enruta ejemplo** | [código fuente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/AspNetRoutes)  
En algunas aplicaciones desea enlazar los componentes de OWIN en la tabla de rutas de Asp.Net en paralelo con componentes que no sean OWIN. En este ejemplo se muestra cómo usar los métodos de extensión RouteCollection MapOwinPath y MapOwinRoute proporcionada por Microsoft.Owin.Host.SystemWeb.

**Bifurcación canalizaciones de ejemplo** | [código fuente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/BranchingPipelines)  
Las canalizaciones de procesamiento de solicitudes OWIN no necesita ser lineal, se pueden bifurcar para procesar las solicitudes de maneras diferentes. En este ejemplo se muestra cómo construir una canalización de bifurcación en función de las rutas de acceso de solicitud u otros datos de solicitud como los encabezados. Estos componentes están disponibles en el paquete de nuget Microsoft.Owin.Mapping.

**Ejemplo de servidor personalizado** | [código fuente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/CustomServer)   
Se muestra cómo usar un servidor OWIN personalizado cuando autohospedaje OWIN.

**Embedded ejemplo** | [código fuente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/Embedded)  
Algunos servidores OWIN se pueden ejecutar dentro de su propio proceso (&quot;autohospedado&quot;). Este ejemplo muestra cómo iniciar una aplicación OWIN mediante las herramientas proporcionadas por el paquete de nuget Microsoft.Owin.Hosting.

**Ejemplo HelloWorld** | [código fuente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorld)  
OWIN es un servidor HTTP de abstracción de API que permite la portabilidad de aplicaciones entre distintos servidores. Este ejemplo muestra cómo escribir una aplicación Hello World con algunos **contenedores sencillos** en torno a la abstracción de OWIN y ejecute sin procesar en un servidor web como ASP.NET.

**Ejemplo de Hello World Raw OWIN** | [código fuente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorldRawOwin)  
Este ejemplo muestra cómo escribir una aplicación Hello World con la **raw** abstracción OWIN y ejecútelo en un servidor web, como Asp.Net.

**Ejemplo de SignalR** | [código fuente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/SignalR)  
Se muestra cómo el autohospedaje de SignalR con OWIN o Katana. Para obtener más información sobre cómo autoalojamiento SignalR, consulte [Tutorial: interna de SignalR](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

**Ejemplo de archivos estáticos de** | [código fuente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/StaticFilesSample)   
Muestra cómo admitir solicitudes HTTP para los archivos estáticos con OWIN o Katana.

**API Web** | [código fuente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebApi)   
En este ejemplo se muestra cómo hospedar OWIN en IIS y agregar la API Web a la canalización OWIN.

**Ejemplo de Socket de Web** | [código fuente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebSocketSample)   
Muestra cómo se admite Web Sockets en OWIN mediante el uso de la [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) clase.
