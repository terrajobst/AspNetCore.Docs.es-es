---
uid: signalr/overview/getting-started/supported-platforms
title: Plataformas compatibles | Microsoft Docs
author: bradygaster
description: En este artículo se describe qué servidores y clientes son compatibles con SignalR.
ms.author: bradyg
ms.date: 04/18/2018
ms.assetid: eac31beb-0f46-4afa-9def-e80904dea4f0
msc.legacyurl: /signalr/overview/getting-started/supported-platforms
msc.type: authoredcontent
ms.openlocfilehash: 60fa74b54797efbe14ba525160b2f750a4f5a451
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836056"
---
<a name="supported-platforms"></a>Plataformas compatibles
====================
por [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> En este artículo se describe qué servidores y clientes son compatibles con SignalR. 
> 
> ## <a name="questions-and-comments"></a>Preguntas y comentarios
> 
> Deje comentarios sobre cómo le gustó de este tutorial y que podíamos mejorar en los comentarios en la parte inferior de la página. Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicarlos en el [foro de ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).

SignalR es compatible con una variedad de configuraciones de cliente y servidor. Además, cada opción de transporte tiene un conjunto de requisitos de su propio; Si los requisitos del sistema para un transporte no están disponibles, SignalR se conmutarán por error a otros transportes. Para obtener más información sobre los transportes que es compatible con SignalR, consulte [transportes y reservas](introduction-to-signalr.md#transports).

## <a name="server-system-requirements"></a>Requisitos del sistema de servidor

El componente de servidor de SignalR puede hospedarse en una gran variedad de configuraciones de servidor. Esta sección describen las versiones compatibles de sistemas operativos, .NET framework, Internet Information Server y otros componentes.

### <a name="supported-server-operating-systems"></a>Sistemas operativos de servidor admitidos

El componente de servidor de SignalR puede hospedarse en los siguientes sistemas operativos de servidor o cliente. Tenga en cuenta que para que SignalR usar WebSockets, Windows Server 2012, Windows Server 2016 o Windows 8 se requiere (WebSocket puede utilizarse en sitios Web de Windows Azure, siempre que se establece la versión de la carpeta del sitio de .NET framework 4.5 y Web Sockets está habilitada en la carpeta del sitio Página de configuración).

- Windows Server 2016
- Windows Server 2012
- Windows Server 2008 r2
- Windows 10
- Windows 8
- Windows 7
- Windows Azure

### <a name="supported-server-net-framework-version"></a>Versión de .NET Framework de servidor compatibles

SignalR 2 solo se admite en .NET Framework 4.5. Consulte la [actualizaciones recomendadas](#updates) sección para las actualizaciones que mejoran la confiabilidad, compatibilidad, estabilidad y rendimiento.

### <a name="supported-server-iis-versions"></a>Versiones de IIS del servidor admitidos

Cuando SignalR se hospeda en IIS, se admiten las siguientes versiones. Tenga en cuenta que si se usa un sistema operativo cliente, como para el desarrollo (Windows 8 o Windows 7), versiones completas de IIS o Cassini no se recomienda, ya que habrá un límite de 10 conexiones simultáneas impuesto, que se alcanzará muy rápidamente dado que las conexiones transitorio, con frecuencia volver a establecer y no se eliminan inmediatamente cuando ya no se usan. IIS Express debe usarse en sistemas operativos cliente.

Tenga en cuenta que para SignalR con WebSocket, se debe usar IIS 8 o Express de IIS 8, debe usar el servidor de Windows 8, Windows Server 2012 o posterior, y WebSocket debe estar habilitado en IIS. Para obtener información sobre cómo habilitar WebSocket en IIS, consulte [IIS 8.0 compatibilidad con el protocolo WebSocket](https://www.iis.net/learn/get-started/whats-new-in-iis-8/iis-80-websocket-protocol-support).

- Express IIS 8 o IIS 8.
- IIS 7 y 7.5. Compatibilidad con [direcciones URL sin extensión](https://support.microsoft.com/kb/980368) es necesario.
- IIS debe ejecutarse en modo integrado; no se admite el modo clásico. Si IIS se ejecuta en modo clásico mediante el transporte de los eventos se pueden producir retrasos de mensaje de hasta 30 segundos.
- La aplicación de hospedaje debe ejecutarse en modo de plena confianza.

## <a name="client-system-requirements"></a>Requisitos del sistema cliente

SignalR se puede usar en una variedad de plataformas de cliente. Esta sección describen los requisitos del sistema para usar SignalR en exploradores web, aplicaciones de escritorio de Windows, las aplicaciones de Silverlight y dispositivos móviles.

### <a name="web-browsers"></a>Exploradores web

Se puede usar SignalR en una variedad de exploradores web, pero por lo general, se admiten solo las dos últimas versiones.

Las aplicaciones que usan SignalR en exploradores deben usar la versión de jQuery 1.6.4 o principales versiones posteriores (por ejemplo, 1.7.2, 1.8.2 o 1.9.1).

SignalR se puede usar en los siguientes exploradores:

- Versiones de Microsoft Internet Explorer 8, 9, 10 y 11. Moderna, escritorio y móviles versiones se admiten.
- Mozilla Firefox: versión actual - 1, versiones de Windows y Mac.
- Google Chrome: versión actual - 1, versiones de Windows y Mac.
- Safari: versión actual - 1, versiones de Mac e iOS.
- Opera: versión actual - 1, solo de Windows.
- Explorador de Android

Además de requerir algunos exploradores, los transportes distintos que usa SignalR tienen requisitos de sus propios. Los transportes siguientes se admiten en las siguientes configuraciones:

<a id="browser"></a>

**Requisitos de transporte del explorador Web**

| Transporte | Internet Explorer | Chrome (Windows o iOS) | Firefox | Safari (iOS o OSX) | Android |
| --- | --- | --- | --- | --- | --- |
| WebSockets | 10+ | actual - 1 | actual - 1 | actual - 1 | N/D |
| Eventos enviados por el servidor | N/D | actual - 1 | actual - 1 | actual - 1 | N/D |
| ForeverFrame | 8+ | N/D | N/D | N/D | 4.1 |
| Sondeo largo | 8+ | actual - 1 | actual - 1 | actual - 1 | 4.1 |

\*: 6 + necesarios para una funcionalidad completa.

#### <a name="unsupported-browsers"></a>Exploradores no compatibles

Mientras SignalR *puede* ejecutándose sin problemas importantes en las versiones anteriores, probaremos no activamente SignalR en ellos y por lo general no se resuelven los errores que pueden aparecer en ellos.

### <a name="windows-desktop-and-silverlight-applications"></a>Escritorio de Windows y aplicaciones de Silverlight

Además de ejecutarse en un explorador web, SignalR se puede hospedar en aplicaciones de Silverlight o cliente de Windows independiente. Las aplicaciones de escritorio de Windows y Silverlight SignalR tienen los siguientes requisitos del sistema.

- Las aplicaciones que usan .NET 4 se admiten en Windows XP SP3 o posterior.
- Se admiten las aplicaciones que usan .NET Framework 4.5 en Windows Vista o posterior.

Además del sistema operativo y requisitos de .NET framework, los transportes disponibles para SignalR tienen requisitos de sus propios. Los transportes siguientes se admiten en las siguientes configuraciones:

**Escritorio de Windows y los requisitos de transporte de Silverlight**

| Transporte | Aplicación .NET | Silverlight |
| --- | --- | --- |
| Web Sockets | Windows 8 y versiones posteriores y .NET 4.5 + | N/D |
| Marco para siempre | N/D | N/D |
| Eventos enviados por el servidor | .NET 4 + | 5+ |
| Sondeo largo | .NET 4 + | 5+ |

<a id="android"></a>

### <a name="windows-store-and-windows-phone-applications"></a>Windows Store y las aplicaciones de Windows Phone

SignalR se puede usar en aplicaciones de Windows Store y aplicaciones de Windows Phone 8. Los transportes siguientes se admiten en las siguientes configuraciones:

**Windows Store y los requisitos de transporte de Windows Phone**

| Transporte | Windows Store/ .NET | Windows Store / JavaScript | Windows Phone / IE | Windows Phone y .NET |
| --- | --- | --- | --- | --- |
| WebSockets | N/D | Win8 + | 8+ | N/D |
| Marco para siempre | N/D | Win8 + | 7.5+ | N/D |
| Eventos enviados por el servidor | Win8 + | N/D | N/D | 8+ |
| Sondeo largo | Win8 + | Win8 + | 7.5+ | 8+ |

<a id="updates"></a>

## <a name="recommended-updates"></a>Actualizaciones recomendadas

Se recomiendan las siguientes actualizaciones para los servidores de SignalR:

- Hay disponible una actualización para .NET Framework 4.5 [aquí](https://support.microsoft.com/kb/2750149).
- Microsoft lanzará periódicamente QFE para ASP.NET. Se debe aplicar como disponibles.
