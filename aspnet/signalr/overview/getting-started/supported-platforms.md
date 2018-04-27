---
uid: signalr/overview/getting-started/supported-platforms
title: Plataformas compatibles | Documentos de Microsoft
author: pfletcher
description: Este artículo describe qué clientes y servidores son compatibles con SignalR.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/18/2018
ms.topic: article
ms.assetid: eac31beb-0f46-4afa-9def-e80904dea4f0
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/supported-platforms
msc.type: authoredcontent
ms.openlocfilehash: 4d3dc028ff67d0a9cfa03627b5f98f6541ecfff8
ms.sourcegitcommit: 7c8fd9b7445cd77eb7f7d774bfd120c26f3b5d84
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/19/2018
---
<a name="supported-platforms"></a>Plataformas compatibles
====================
por [Patrick Fletcher](https://github.com/pfletcher)

> Este artículo describe qué clientes y servidores son compatibles con SignalR. 
> 
> ## <a name="questions-and-comments"></a>Preguntas y comentarios
> 
> Vota sobre cómo le gustó este tutorial y lo que podemos mejorar en los comentarios en la parte inferior de la página. Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicar para la [foro de ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).


SignalR es compatible con una variedad de configuraciones de cliente y servidor. Además, cada opción de transporte tiene un conjunto de requisitos de su propio; Si los requisitos del sistema para un transporte no están disponibles, SignalR correctamente conmutará por error a otros transportes. Para obtener más información sobre los transportes que es compatible con SignalR, consulte [transportes y reservas](introduction-to-signalr.md#transports).

## <a name="server-system-requirements"></a>Requisitos de sistema de servidor

El componente de servidor de SignalR se puede hospedar en una variedad de configuraciones de servidor. En esta sección se describe las versiones compatibles de sistemas operativos, .NET framework, Internet Information Server y otros componentes.

### <a name="supported-server-operating-systems"></a>Sistemas operativos de servidor admitidos

El componente de servidor de SignalR se puede hospedar en los siguientes sistemas operativos de servidor o cliente. Tenga en cuenta que para que SignalR usar WebSockets, Windows Server 2012, Windows Server 2016 o Windows 8 (WebSocket puede utilizarse en sitios Web de Windows Azure, siempre que se establezca la versión de .NET framework del sitio en 4.5 y Sockets Web está habilitado en el sitio Página de configuración).

- Windows Server 2016
- Windows Server 2012
- Windows Server 2008 r2
- Windows 10
- Windows 8
- Windows 7
- Windows Azure

### <a name="supported-server-net-framework-version"></a>Versión de .NET Framework de servidor admitidos

SignalR 2 solo se admite en .NET Framework 4.5. Consulte la [actualizaciones recomendadas](#updates) sección actualizaciones que mejoran la confiabilidad, compatibilidad, estabilidad y rendimiento.

### <a name="supported-server-iis-versions"></a>Versiones de IIS de servidor admitidos

Cuando SignalR se hospeda en IIS, se admiten las siguientes versiones. Tenga en cuenta que si se utiliza un sistema operativo cliente, como para el desarrollo (Windows 8 o Windows 7), versiones completas de IIS o Cassini no se recomienda, ya que habrá un límite de 10 conexiones simultáneas impuesto, que se alcanzará muy rápidamente dado que las conexiones transitorio, con frecuencia volver a establecer y no se eliminan inmediatamente cuando ya no se usan. IIS Express debe utilizarse en sistemas operativos de cliente.

Tenga en cuenta que para SignalR usar WebSocket, 8 de IIS o IIS Express 8 debe utilizarse, el servidor debe usar Windows 8, Windows Server 2012 o posterior, y WebSocket debe estar habilitada en IIS. Para obtener información sobre cómo habilitar WebSocket en IIS, consulte [IIS 8.0 compatibilidad con el protocolo WebSocket](https://www.iis.net/learn/get-started/whats-new-in-iis-8/iis-80-websocket-protocol-support).

- 8 de IIS o IIS 8 Express.
- IIS 7 y 7.5. Compatibilidad con [direcciones URL sin extensión](https://support.microsoft.com/kb/980368) es necesario.
- Debe ejecutar IIS en el modo integrado; no se admite el modo clásico. Si IIS se ejecuta en modo clásico utiliza el transporte de Server-Sent eventos se pueden producir retrasos de mensaje de hasta 30 segundos.
- La aplicación de hospedaje debe ejecutarse en modo de plena confianza.

## <a name="client-system-requirements"></a>Requisitos del sistema cliente

SignalR puede utilizarse en una gran variedad de plataformas de cliente. Esta sección describen los requisitos del sistema para el uso de SignalR en exploradores web, aplicaciones de escritorio de Windows, las aplicaciones de Silverlight y dispositivos móviles.

### <a name="web-browsers"></a>Exploradores web

SignalR puede utilizarse en una gran variedad de exploradores web, pero por lo general, se admiten sólo las dos últimas versiones.

Las aplicaciones que utilizan SignalR en exploradores deben usar jQuery versión 1.6.4 o versiones posteriores importantes (por ejemplo, 1.7.2, 1.8.2 o 1.9.1).

SignalR puede utilizarse en los siguientes exploradores:

- Versiones de Microsoft Internet Explorer 8, 9, 10 y 11. Moderno, escritorio y móviles versiones son compatibles.
- Mozilla Firefox: versión actual - 1, versiones de Windows y Mac.
- Google Chrome: versión actual - 1, versiones de Windows y Mac.
- Safari: versión actual - 1, versiones de iOS y Mac.
- Opera: versión actual - 1, sólo en Windows.
- Explorador Android

Además de necesitar algunos exploradores, los transportes distintos que usa SignalR tienen requisitos de sus propios. Los transportes siguientes se admiten en las siguientes configuraciones:

<a id="browser"></a>

**Requisitos de transporte del explorador Web**

| Transporte | Internet Explorer | Chrome (Windows o iOS) | Firefox | Safari (OSX o iOS) | Android |
| --- | --- | --- | --- | --- | --- |
| WebSockets | 10+ | actual - 1 | actual - 1 | actual - 1 | N/D |
| Eventos enviados por el servidor | N/D | actual - 1 | actual - 1 | actual - 1 | N/D |
| ForeverFrame | 8+ | N/D | N/D | N/D | 4.1 |
| Sondeo prolongado | 8+ | actual - 1 | actual - 1 | actual - 1 | 4.1 |

\*: se requiere para una funcionalidad completa de 6 +.

#### <a name="unsupported-browsers"></a>Exploradores no compatibles

Mientras SignalR *puede* ejecutándose sin problemas importantes en las versiones anteriores, probamos no activamente SignalR en ellos y generalmente no corrigen los errores que pueden aparecer en ellos.

### <a name="windows-desktop-and-silverlight-applications"></a>Escritorio de Windows y las aplicaciones de Silverlight

Además de ejecutarse en un explorador web, se puede hospedar SignalR en el cliente de Windows independiente o las aplicaciones de Silverlight. Aplicaciones de escritorio de Windows y Silverlight SignalR tienen los siguientes requisitos de sistema.

- Las aplicaciones que usan .NET 4 se admiten en Windows XP SP3 o posterior.
- Las aplicaciones que usan .NET Framework 4.5 se admiten en Windows Vista o posterior.

Además de sistema operativo y requisitos de .NET framework, los transportes disponibles para SignalR tienen requisitos de sus propios. Los transportes siguientes se admiten en las siguientes configuraciones:

**Escritorio de Windows y los requisitos de transporte de Silverlight**

| Transporte | Aplicación .NET | Silverlight |
| --- | --- | --- |
| Web Sockets | Windows 8 + y .NET Framework 4.5 + | N/D |
| Marco indefinidamente | N/D | N/D |
| Eventos enviados por el servidor | .NET 4 + | 5+ |
| Sondeo prolongado | .NET 4 + | 5+ |

<a id="android"></a>

### <a name="windows-store-and-windows-phone-applications"></a>Tienda Windows y aplicaciones de Windows Phone

SignalR puede utilizarse en aplicaciones de la tienda de Windows y aplicaciones de Windows Phone 8. Los transportes siguientes se admiten en las siguientes configuraciones:

**Tienda Windows y requisitos de transporte de Windows Phone**

| Transporte | Tienda Windows y .NET | Tienda Windows / JavaScript | Windows Phone / IE | Windows Phone / .NET |
| --- | --- | --- | --- | --- |
| WebSockets | N/D | Win8 + | 8+ | N/D |
| Marco indefinidamente | N/D | Win8 + | 7.5+ | N/D |
| Eventos enviados por el servidor | Win8 + | N/D | N/D | 8+ |
| Sondeo prolongado | Win8 + | Win8 + | 7.5+ | 8+ |

<a id="updates"></a>

## <a name="recommended-updates"></a>Actualizaciones recomendadas

Se recomiendan las siguientes actualizaciones para los servidores de SignalR:

- Hay disponible una actualización para .NET Framework 4.5 [aquí](https://support.microsoft.com/kb/2750149).
- Microsoft lanzará periódicamente QFE para ASP.NET. Estos se deben aplicar como disponibles.
