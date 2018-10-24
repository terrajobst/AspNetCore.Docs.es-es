---
title: Implementaciones de servidores web en ASP.NET Core
author: rick-anderson
description: Detecte los servidores web Kestrel y HTTP.sys de ASP.NET Core. Obtenga más información sobre cómo elegir un servidor y cuándo se debe usar un servidor proxy inverso.
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/21/2018
uid: fundamentals/servers/index
ms.openlocfilehash: 161ab3fdf48e58d8c9af991dc5531e46d9c5adff
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325866"
---
# <a name="web-server-implementations-in-aspnet-core"></a>Implementaciones de servidores web en ASP.NET Core

Por [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73) y [Chris Ross](https://github.com/Tratcher)

Una aplicación ASP.NET Core se ejecuta con una implementación de servidor HTTP en proceso. La implementación del servidor realiza escuchas de solicitudes HTTP y las muestra en la aplicación como conjuntos de [características de solicitud](xref:fundamentals/request-features) compuestos en un <xref:Microsoft.AspNetCore.Http.HttpContext>.

ASP.NET Core incluye tres implementaciones de servidor:

::: moniker range=">= aspnetcore-2.2"

* [Kestrel](xref:fundamentals/servers/kestrel) es el servidor HTTP multiplataforma predeterminado de ASP.NET Core.
* `IISHttpServer` se usa con el [modelo de hospedaje en proceso](xref:fundamentals/servers/aspnet-core-module#in-process-hosting-model) y el [módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) en Windows.
* [HTTP.sys](xref:fundamentals/servers/httpsys) es un servidor HTTP solo para Windows que se basa en el [controlador de kernel de HTTP.Sys y HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx). (HTTP.sys se denomina [WebListener](xref:fundamentals/servers/weblistener) en ASP.NET Core 1.x).

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [Kestrel](xref:fundamentals/servers/kestrel) es el servidor HTTP multiplataforma predeterminado de ASP.NET Core.
* [HTTP.sys](xref:fundamentals/servers/httpsys) es un servidor HTTP solo para Windows que se basa en el [controlador de kernel de HTTP.Sys y HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx). (HTTP.sys se denomina [WebListener](xref:fundamentals/servers/weblistener) en ASP.NET Core 1.x).

::: moniker-end

## <a name="kestrel"></a>Kestrel

Kestrel es el servidor web predeterminado que se incluye en las plantillas de proyecto de ASP.NET Core.

::: moniker range=">= aspnetcore-2.0"

Se puede usar Kestrel por sí solo o con un *servidor proxy inverso*, como IIS, Nginx o Apache. Un servidor proxy inverso recibe las solicitudes HTTP de Internet y las reenvía a Kestrel después de un control preliminar.

![Kestrel se comunica directamente con Internet sin ningún servidor proxy inverso](kestrel/_static/kestrel-to-internet2.png)

![Kestrel se comunica indirectamente con Internet a través de un servidor proxy inverso, como IIS, Nginx o Apache](kestrel/_static/kestrel-to-internet.png)

Cualquiera de las configuraciones&mdash;con o sin un servidor proxy inverso&mdash;es una configuración de hospedaje válida y admitida para ASP.NET 2.0 o aplicaciones posteriores. Para más información, vea [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy) (Cuándo se debe usar Kestrel con un proxy inverso).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Si la aplicación únicamente acepta solicitudes de una red interna, Kestrel puede usarse por sí solo.

![Kestrel se comunica directamente con la red interna](kestrel/_static/kestrel-to-internal.png)

Si la aplicación se expone a Internet, Kestrel debe usar IIS, Nginx o Apache como *servidor proxy inverso*. Un servidor proxy inverso recibe las solicitudes HTTP de Internet y las reenvía a Kestrel después de un control preliminar, como se muestra en el diagrama siguiente:

![Kestrel se comunica indirectamente con Internet a través de un servidor proxy inverso, como IIS, Nginx o Apache](kestrel/_static/kestrel-to-internet.png)

El motivo más importante por el que se debe usar un proxy inverso para las implementaciones de servidores perimetrales de acceso público expuestos directamente a Internet es la seguridad. Las versiones 1.x de Kestrel no tienen características de seguridad importantes para defenderse contra ataques procedentes de Internet. Esto incluye (aunque no de forma limitada) los tiempos de espera, los límites de tamaño de solicitud y los límites de conexiones simultáneas correspondientes.

Para más información, vea [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy) (Cuándo se debe usar Kestrel con un proxy inverso).

::: moniker-end

No se puede usar IIS, Nginx o Apache sin Kestrel ni ninguna [implementación de servidor personalizado](#custom-servers). ASP.NET Core se ha diseñado para ejecutarse en su propio proceso de modo que pueda comportarse de forma coherente entre varias plataformas. IIS, Nginx y Apache dictan su propio procedimiento y entorno de inicio. Para usar estas tecnologías de servidor directamente, ASP.NET Core tendría que adaptarse a los requisitos de cada servidor. El uso de una implementación de servidor web como Kestrel proporciona a ASP.NET Core el control sobre el entorno y el proceso de inicio cuando se hospedan en tecnologías de servidor diferentes.

### <a name="iis-with-kestrel"></a>IIS con Kestrel

::: moniker range=">= aspnetcore-2.2"

Cuando se usa [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) o [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), la aplicación de ASP.NET Core se ejecuta en el mismo proceso que el proceso de trabajo de IIS (el modelo de hospedaje *en proceso*) o en un proceso independiente del proceso de trabajo de IIS (el modelo de hospedaje *fuera de proceso*).

El [módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) es un módulo nativo de IIS que controla las solicitudes de IIS nativas entre el servidor de HTTP de IIS en proceso o el servidor Kestrel fuera de proceso. Para obtener más información, vea <xref:fundamentals/servers/aspnet-core-module>.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Al usar [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) o [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) como proxy inverso para ASP.NET Core, la aplicación ASP.NET Core se ejecuta en un proceso independiente del proceso de trabajo de IIS. En el proceso IIS, el [módulo de ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) coordina la relación de proxy inverso. Las funciones principales del módulo de ASP.NET Core consisten en iniciar la aplicación ASP.NET Core, reiniciarla cuando se bloquea y reenviar el tráfico HTTP a la aplicación. Para obtener más información, vea <xref:fundamentals/servers/aspnet-core-module>.

::: moniker-end

### <a name="nginx-with-kestrel"></a>Nginx con Kestrel

Para información sobre cómo usar Nginx en Linux como servidor proxy inverso para Kestrel, vea [Host on Linux with Nginx](xref:host-and-deploy/linux-nginx) (Hospedar en Linux con Nginx).

### <a name="apache-with-kestrel"></a>Apache con Kestrel

Para información sobre cómo usar Apache en Linux como servidor proxy inverso para Kestrel, vea [Host on Linux with Apache](xref:host-and-deploy/linux-apache) (Hospedar en Linux con Apache).

## <a name="httpsys"></a>HTTP.sys

::: moniker range=">= aspnetcore-2.0"

Si las aplicaciones ASP.NET Core se ejecutan en Windows, HTTP.sys es una alternativa a Kestrel. Suele recomendarse Kestrel para un rendimiento óptimo. HTTP.sys se puede usar en escenarios en los que la aplicación se expone a Internet y las funcionalidades necesarias son compatibles con HTTP.sys pero no con Kestrel. Para información sobre HTTP.sys, vea [HTTP.sys](xref:fundamentals/servers/httpsys).

![HTTP.sys se comunica directamente con Internet](httpsys/_static/httpsys-to-internet.png)

HTTP.sys también se puede usar para las aplicaciones que solo se exponen a una red interna.

![HTTP.sys se comunica directamente con la red interna](httpsys/_static/httpsys-to-internal.png)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

HTTP.sys se denomina [WebListener](xref:fundamentals/servers/weblistener) en ASP.NET Core 1.x. Si las aplicaciones ASP.NET Core se ejecutan en Windows, WebListener es una alternativa para los escenarios en los que IIS no está disponible para hospedar aplicaciones.

![WebListener se comunica directamente con Internet](weblistener/_static/weblistener-to-internet.png)

WebListener también se puede usar en lugar de Kestrel para las aplicaciones que solo se exponen a una red interna, en el caso de que las funcionalidades necesarias sean compatibles con WebListener pero no con Kestrel. Para información sobre WebListener, vea [WebListener](xref:fundamentals/servers/weblistener).

![WebListener se comunica directamente con la red interna](weblistener/_static/weblistener-to-internal.png)

::: moniker-end

## <a name="aspnet-core-server-infrastructure"></a>Infraestructura de servidores de ASP.NET Core

La interfaz [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) disponible en el método `Startup.Configure` expone la propiedad [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) de tipo [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection). Kestrel y HTTP.sys (WebListener en ASP.NET Core 1.x) solo exponen una característica cada uno, [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), pero otras implementaciones de servidor pueden exponer funcionalidades adicionales.

Se puede usar `IServerAddressesFeature` para averiguar a qué puerto se ha enlazado la implementación del servidor en tiempo de ejecución.

## <a name="custom-servers"></a>Servidores personalizados

Si los servidores integrados no cumplen los requisitos de la aplicación, se puede crear una implementación de servidor personalizado. En la [guía de Open Web Interface for .NET (OWIN)](xref:fundamentals/owin) se muestra cómo escribir una implementación de [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) basada en [Nowin](https://github.com/Bobris/Nowin). Solo requieren la implementación las interfaces de características que usa la aplicación, aunque como mínimo se debe admitir [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) e [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature).

## <a name="server-startup"></a>Inicio del servidor

Cuando se usa [Visual Studio](https://www.visualstudio.com/vs/), [Visual Studio para Mac](https://www.visualstudio.com/vs/mac/) o [Visual Studio Code](https://code.visualstudio.com/), el servidor se inicia cuando el entorno de desarrollo integrado (IDE) inicia la aplicación. En Visual Studio en Windows, se pueden usar perfiles de inicio para iniciar la aplicación y el servidor con [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[módulo de ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) o la consola. En Visual Studio Code, [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode) inicia la aplicación y el servidor y activa el depurador CoreCLR. En Visual Studio para Mac, [Mono Soft-Mode Debugger](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/) inicia la aplicación y el servidor.

Al iniciar una aplicación desde un símbolo del sistema en la carpeta del proyecto, [dotnet run](/dotnet/core/tools/dotnet-run) inicia la aplicación y el servidor (solo Kestrel y HTTP.sys). La configuración se especifica mediante la opción `-c|--configuration`, que está establecida en `Debug` (valor predeterminado) o `Release`. Si no hay perfiles de inicio en un archivo *launchSettings.json*, use la opción `--launch-profile <NAME>` para establecer el perfil de inicio (por ejemplo, `Development` o `Production`). Para más información, vea los temas [dotnet run](/dotnet/core/tools/dotnet-run) y [Empaquetado de distribución de .NET Core](/dotnet/core/build/distribution-packaging).

## <a name="http2-support"></a>Compatibilidad con HTTP/2

[HTTP/2](https://httpwg.org/specs/rfc7540.html) es compatible con ASP.NET Core en los escenarios de implementación siguientes:

::: moniker range=">= aspnetcore-2.2"

* [Kestrel](xref:fundamentals/servers/kestrel#http2-support)
  * Sistema operativo
    * Windows Server 2012 R2 o Windows 8.1 o posterior
    * Linux con OpenSSL 1.0.2 o posterior (por ejemplo, Ubuntu 16.04 o posterior)
    * HTTP/2 se admitirá en una versión futura en macOS.
  * Plataforma de destino: .NET Core 2.2 o posterior
* [HTTP.sys](xref:fundamentals/servers/httpsys#http2-support)
  * Windows Server 2016/Windows 10 o posterior
  * Plataforma de destino: no aplicable a implementaciones de HTTP.sys.
* [IIS (en proceso)](xref:host-and-deploy/iis/index#http2-support)
  * Windows Server 2016/Windows 10 o posterior; IIS 10 o posterior
  * Plataforma de destino: .NET Core 2.2 o posterior
* [IIS (fuera de proceso)](xref:host-and-deploy/iis/index#http2-support)
  * Windows Server 2016/Windows 10 o posterior; IIS 10 o posterior
  * Las conexiones de servidor perimetral de acceso público usan HTTP/2, pero la conexión de proxy inverso a Kestrel usa HTTP/1.1.
  * Plataforma de destino: no aplicable a implementaciones fuera de proceso de IIS.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [HTTP.sys](xref:fundamentals/servers/httpsys#http2-support)
  * Windows Server 2016/Windows 10 o posterior
  * Plataforma de destino: no aplicable a implementaciones de HTTP.sys.
* [IIS (fuera de proceso)](xref:host-and-deploy/iis/index#http2-support)
  * Windows Server 2016/Windows 10 o posterior; IIS 10 o posterior
  * Las conexiones de servidor perimetral de acceso público usan HTTP/2, pero la conexión de proxy inverso a Kestrel usa HTTP/1.1.
  * Plataforma de destino: no aplicable a implementaciones fuera de proceso de IIS.

::: moniker-end

Una conexión HTTP/2 debe usar [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) y TLS 1.2 o posterior. Para más información, vea los temas que pertenecen a los escenarios de implementación de servidor.

## <a name="additional-resources"></a>Recursos adicionales

* <xref:fundamentals/servers/kestrel>
* <xref:fundamentals/servers/aspnet-core-module>
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:fundamentals/servers/httpsys> (para ASP.NET Core 1.x, vea <xref:fundamentals/servers/weblistener>)
