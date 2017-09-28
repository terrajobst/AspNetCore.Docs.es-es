---
title: Implementaciones de servidores web en ASP.NET Core
author: tdykstra
description: "Presenta los servidores web Kestrel y WebListener para ASP.NET Core. Proporciona instrucciones sobre cómo elegir uno y cuándo se debe usar con un servidor proxy inverso."
keywords: ASP.NET Core, IServer, servidor web, Kestrel, WebListener, proxy inverso
ms.author: tdykstra
manager: wpickett
ms.date: 08/03/2017
ms.topic: article
ms.assetid: dba74f39-58cd-4dee-a061-6d15f7346959
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/index
ms.openlocfilehash: 04dee100dff91f7868175ff4be01156787e13e81
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/22/2017
---
# <a name="web-server-implementations-in-aspnet-core"></a>Implementaciones de servidores web en ASP.NET Core

Por [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73) y [Chris Ross](https://github.com/Tratcher)

Una aplicación ASP.NET Core se ejecuta con una implementación de servidor HTTP en proceso. La implementación del servidor realiza escuchas de solicitudes HTTP y las muestra en la aplicación como conjuntos de [características de solicitud](https://docs.microsoft.com/aspnet/core/fundamentals/request-features) compuestos en un `HttpContext`.

ASP.NET Core incluye dos implementaciones de servidor:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

* [Kestrel](kestrel.md) es un servidor HTTP multiplataforma basado en [libuv](https://github.com/libuv/libuv), una biblioteca de E/S asincrónica multiplataforma.

* [HTTP.sys](httpsys.md) es un servidor HTTP solo para Windows que se basa en el [controlador de kernel de Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* [Kestrel](kestrel.md) es un servidor HTTP multiplataforma basado en [libuv](https://github.com/libuv/libuv), una biblioteca de E/S asincrónica multiplataforma.

* [WebListener](weblistener.md) es un servidor HTTP solo para Windows que se basa en el [controlador de kernel de Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).

---

## <a name="kestrel"></a>Kestrel

Kestrel es el servidor web que se incluye de forma predeterminada en las plantillas de proyecto nuevo de ASP.NET Core. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Puede usar Kestrel por sí solo o con un *servidor proxy inverso*, como IIS, Nginx o Apache. Un servidor proxy inverso recibe las solicitudes HTTP de Internet y las reenvía a Kestrel después de un control preliminar.

![Kestrel se comunica directamente con Internet sin ningún servidor proxy inverso](kestrel/_static/kestrel-to-internet2.png)

![Kestrel se comunica indirectamente con Internet a través de un servidor proxy inverso, como IIS, Nginx o Apache](kestrel/_static/kestrel-to-internet.png)

También se puede usar cualquiera de las configuraciones (con o sin servidor proxy inverso) si Kestrel está expuesto solo a una red interna.

Para información sobre cuándo se debe usar Kestrel con un proxy inverso, vea [Introduction to Kestrel](kestrel.md) (Introducción a Kestrel).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Si la aplicación acepta solicitudes únicamente de una red interna, puede usar Kestrel por sí mismo.

![Kestrel se comunica directamente con la red interna](kestrel/_static/kestrel-to-internal.png)

Si expone la aplicación a Internet, debe usar IIS, Nginx o Apache como *servidor proxy inverso*. Un servidor proxy inverso recibe las solicitudes HTTP de Internet y las reenvía a Kestrel después de un control preliminar, como se muestra en el siguiente diagrama.

![Kestrel se comunica indirectamente con Internet a través de un servidor proxy inverso, como IIS, Nginx o Apache](kestrel/_static/kestrel-to-internet.png)

El motivo más importante por el que se debe usar un proxy inverso para las implementaciones de servidores perimetrales (expuestas al tráfico de Internet) es la seguridad. Las versiones 1.x de Kestrel no tienen un complemento completo de defensas frente a los ataques. Esto incluye (aunque no de forma limitada) los tiempos de espera, los límites de tamaño y los límites de conexiones simultáneas correspondientes.

Para información sobre cuándo se debe usar Kestrel con un proxy inverso, vea [Introduction to Kestrel](kestrel.md) (Introducción a Kestrel).

---

No puede usar IIS, Nginx o Apache sin Kestrel ni ninguna [implementación de servidor personalizado](#custom-servers). ASP.NET Core se ha diseñado para ejecutarse en su propio proceso de modo que pueda comportarse de forma coherente entre varias plataformas. IIS, Nginx y Apache dictan su propio entorno y proceso de inicio; para poder usarlos directamente, ASP.NET Core tendría que adaptarse a las necesidades de cada uno de ellos. El uso de una implementación de servidor web como Kestrel proporciona a ASP.NET Core el control sobre el entorno y el proceso de inicio. Por lo tanto, en lugar de intentar adaptar ASP.NET Core a IIS, Nginx o Apache, debe configurar esos servidores web en las solicitudes de proxy a Kestrel. Esta disposición permite que las clases `Program.Main` y `Startup` sean básicamente las mismas, independientemente de dónde las implemente.

### <a name="iis-with-kestrel"></a>IIS con Kestrel

Al usar IIS o IIS Express como proxy inverso para ASP.NET Core, la aplicación ASP.NET Core se ejecuta en un proceso independiente del proceso de trabajo de IIS. En el proceso de IIS se ejecuta un módulo de IIS especial para coordinar la relación del proxy inverso.  Se trata del *módulo de ASP.NET Core*. Las funciones principales del módulo de ASP.NET Core consisten en iniciar la aplicación ASP.NET Core, reiniciarla cuando se bloquea y reenviar el tráfico HTTP a ella. Para más información, vea [ASP.NET Core Module](aspnet-core-module.md) (Módulo de ASP.NET Core). 

### <a name="nginx-with-kestrel"></a>Nginx con Kestrel

Para información sobre cómo usar Nginx en Linux como servidor proxy inverso para Kestrel, vea [Publish to a Linux Production Environment](../../publishing/linuxproduction.md) (Publicar en un entorno de producción de Linux).

### <a name="apache-with-kestrel"></a>Apache con Kestrel

Para información sobre cómo usar Apache en Linux como servidor proxy inverso para Kestrel, vea [Using Apache Web Server as a reverse proxy](../../publishing/apache-proxy.md) (Usar el servidor web de Apache como proxy inverso).

## <a name="httpsys"></a>HTTP.sys

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Si ejecuta la aplicación ASP.NET Core en Windows, HTTP.sys es una alternativa a Kestrel. Puede usar HTTP.sys en los escenarios en los que expone la aplicación a Internet y necesita características de HTTP.sys con las que Kestrel no es compatible. 

![HTTP.sys se comunica directamente con Internet](httpsys/_static/httpsys-to-internet.png)

HTTP.sys también se puede usar para las aplicaciones que se exponen solo a una red interna. 

![HTTP.sys se comunica directamente con la red interna](httpsys/_static/httpsys-to-internal.png)

Para los escenarios de red interna, se suele recomendar Kestrel para lograr un mejor rendimiento, pero en algunos escenarios es posible que quiera usar una característica que solo ofrece HTTP.sys. Para información sobre las características de HTTP.sys, vea [HTTP.sys](httpsys.md).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

HTTP.sys se denomina WebListener en ASP.NET Core 1.x. Si ejecuta la aplicación ASP.NET Core en Windows, WebListener es una alternativa que puede usar para los escenarios en los que quiere exponer la aplicación a Internet, pero no puede usar IIS.

![WebListener se comunica directamente con Internet](weblistener/_static/weblistener-to-internet.png)

WebListener también se puede usar en lugar de Kestrel para las aplicaciones que se exponen solo a una red interna, en el caso de que necesite características de WebListener con las que Kestrel no es compatible. 

![WebListener se comunica directamente con la red interna](weblistener/_static/weblistener-to-internal.png)

Para los escenarios de red interna, se suele recomendar Kestrel para lograr un mejor rendimiento, pero en algunos escenarios es posible que quiera usar una característica que solo ofrece WebListener. Para información sobre las características de WebListener, consulte [WebListener](weblistener.md).

---

## <a name="notes-about-aspnet-core-server-infrastructure"></a>Notas sobre la infraestructura de servidores de ASP.NET Core

El [`IApplicationBuilder`](/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder) disponible en el método `Configure` de la clase `Startup` expone la propiedad `ServerFeatures` de tipo [`IFeatureCollection`](/aspnet/core/api/microsoft.aspnetcore.http.features.ifeaturecollection). Kestrel y WebListener exponen una sola característica, [`IServerAddressesFeature`](/aspnet/core/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), pero las distintas implementaciones del servidor pueden exponer una funcionalidad adicional.

Se puede usar `IServerAddressesFeature` para averiguar a qué puerto se ha enlazado la implementación del servidor en tiempo de ejecución.

## <a name="custom-servers"></a>Servidores personalizados

Si los servidores integrados no satisfacen sus necesidades, puede crear una implementación de servidor personalizado. En la [guía de Open Web Interface for .NET (OWIN)](../owin.md) se muestra cómo escribir una implementación de [IServer](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.server.iserver) basada en [Nowin](https://github.com/Bobris/Nowin). Tiene la posibilidad de implementar únicamente las interfaces de las características necesarias para la aplicación, aunque como mínimo debe admitir [IHttpRequestFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttprequestfeature) e [IHttpResponseFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttpresponsefeature).

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información, vea los siguientes recursos:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

- [Kestrel](kestrel.md)
- [Kestrel con IIS](aspnet-core-module.md)
- [Kestrel con Nginx](../../publishing/linuxproduction.md)
- [Kestrel con Apache](../../publishing/apache-proxy.md)
- [HTTP.sys](httpsys.md)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

- [Kestrel](kestrel.md)
- [Kestrel con IIS](aspnet-core-module.md)
- [Kestrel con Nginx](../../publishing/linuxproduction.md)
- [Kestrel con Apache](../../publishing/apache-proxy.md)
- [WebListener](weblistener.md)

---
