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
ms.locfileid: "34153710"
---
# <a name="aspnet-core-module"></a>Módulo ASP.NET Core

Por [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl) y [Chris Ross](https://github.com/Tratcher) 

El módulo ASP.NET Core permite a las aplicaciones ASP.NET Core ejecutarse tras IIS en una configuración de proxy inverso. IIS proporciona características de administración y seguridad de aplicaciones web avanzadas.

Versiones de Windows compatibles:

* Windows 7 o posterior
* Windows Server 2008 R2 o posterior

El módulo ASP.NET Core funciona únicamente con Kestrel. El módulo no es compatible con [HTTP.sys](xref:fundamentals/servers/httpsys) (anteriormente denominado [WebListener](xref:fundamentals/servers/weblistener)).

## <a name="aspnet-core-module-description"></a>Descripción del módulo ASP.NET Core

El módulo ASP.NET Core es un módulo de IIS nativo que se conecta a la canalización IIS para redirigir solicitudes web a aplicaciones ASP.NET Core de back-end. Muchos de los módulos nativos, como la autenticación de Windows, permanecen activos. Para más información sobre los módulos de IIS activos con el módulo, vea [IIS modules](xref:host-and-deploy/iis/modules) (Módulos de IIS).

Dado que las aplicaciones ASP.NET Core se ejecutan en un proceso independiente del proceso de trabajo de IIS, el módulo también se encarga de la administración de procesos. El módulo inicia el proceso de la aplicación ASP.NET Core cuando entra la primera solicitud, y reinicia la aplicación si esta se bloquea. Este comportamiento es básicamente el mismo que el de las aplicaciones ASP.NET 4.x que se ejecutan en el proceso en IIS y se administran a través del [Servicio de activación de procesos de Windows (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).

En el siguiente diagrama se muestra la relación entre las aplicaciones IIS, el módulo ASP.NET Core y las aplicaciones ASP.NET Core:

![Módulo ASP.NET Core](aspnet-core-module/_static/ancm.png)

Las solicitudes llegan procedentes de Internet al controlador HTTP.sys en modo kernel. El controlador enruta las solicitudes a IIS en el puerto configurado del sitio web, que suele ser el puerto 80 (HTTP) o 443 (HTTPS). El módulo reenvía las solicitudes a Kestrel en un puerto aleatorio de la aplicación, que no es ni 80 ni 443.

El módulo especifica el puerto a través de la variable de entorno en el inicio y el middleware de integración de IIS configura el servidor para que escuche en `http://localhost:{port}`. Se realizan más comprobaciones y se rechazan las solicitudes que no se hayan originado en el módulo. El módulo no admite el reenvío de HTTPS, por lo que las solicitudes se reenvían a través de HTTP, aunque IIS las reciba a través de HTTPS.

Una vez que Kestrel toma una solicitud del módulo, la envía a la canalización de middleware de ASP.NET Core. La canalización de middleware controla la solicitud y la pasa como una instancia de `HttpContext` a la lógica de la aplicación. La respuesta de la aplicación se vuelve a pasar a IIS, que la envía de nuevo al cliente HTTP que inició la solicitud.

El módulo ASP.NET Core tiene otras funciones. Puede:

* Establecer variables de entorno para un proceso de trabajo.
* Registrar la salida en un almacenamiento de archivos para solucionar problemas de inicio.
* Reenviar tokens de autenticación de Windows.

## <a name="how-to-install-and-use-the-aspnet-core-module"></a>Cómo instalar y usar el módulo ASP.NET Core

Para obtener instrucciones detalladas sobre cómo instalar y usar el módulo ASP.NET Core, vea [Hospedaje de ASP.NET Core en Windows con IIS](xref:host-and-deploy/iis/index). Para más información sobre cómo configurar el módulo, vea [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module) (Referencia de configuración del módulo de ASP.NET Core).

## <a name="additional-resources"></a>Recursos adicionales

* [Hospedaje en Windows con IIS](xref:host-and-deploy/iis/index)
* [Referencia de configuración del módulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
* [Repositorio GitHub del módulo ASP.NET Core (código fuente)](https://github.com/aspnet/AspNetCoreModule)
