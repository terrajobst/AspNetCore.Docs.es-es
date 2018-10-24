---
title: Módulo ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo el módulo de ASP.NET Core permite que el servidor web de Kestrel use IIS o IIS Express como servidor proxy inverso.
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/21/2018
uid: fundamentals/servers/aspnet-core-module
ms.openlocfilehash: 2f73a34b7d311c9e98ad2ecba11894d27bb2aa4d
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910894"
---
# <a name="aspnet-core-module"></a>Módulo ASP.NET Core

Por [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl) y [Chris Ross](https://github.com/Tratcher)

::: moniker range=">= aspnetcore-2.2"

El módulo ASP.NET Core permite a las aplicaciones ASP.NET Core ejecutarse en un proceso de trabajo de IIS (*en proceso*) o tras IIS en una configuración de proxy inverso (*fuera de proceso*). IIS proporciona características de administración y seguridad de aplicaciones web avanzadas.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

El módulo ASP.NET Core permite a las aplicaciones ASP.NET Core ejecutarse tras IIS en una configuración de proxy inverso. IIS proporciona características de administración y seguridad de aplicaciones web avanzadas.

::: moniker-end

Versiones de Windows compatibles:

* Windows 7 o posterior
* Windows Server 2008 R2 o posterior

::: moniker range=">= aspnetcore-2.2"

Cuando se hospeda en proceso, el módulo tiene su propia implementación de servidor, `IISHttpServer`.

Cuando se hospeda fuera de proceso, el módulo solo funciona con Kestrel. El módulo no es compatible con [HTTP.sys](xref:fundamentals/servers/httpsys) (anteriormente denominado [WebListener](xref:fundamentals/servers/weblistener)).

::: moniker-end

::: moniker range="< aspnetcore-2.2"

El módulo funciona únicamente con Kestrel. El módulo no es compatible con [HTTP.sys](xref:fundamentals/servers/httpsys) (anteriormente denominado [WebListener](xref:fundamentals/servers/weblistener)).

::: moniker-end

## <a name="aspnet-core-module-description"></a>Descripción del módulo ASP.NET Core

::: moniker range=">= aspnetcore-2.2"

El módulo ASP.NET Core es un módulo nativo de IIS que se conecta a la canalización de IIS para:

* Hospedar una aplicación ASP.NET Core dentro del proceso de trabajo de IIS (`w3wp.exe`), lo que se denomina [modelo de hospedaje en proceso](#in-process-hosting-model).

* Reenviar solicitudes web a una aplicación ASP.NET Core de back-end que ejecuta el [servidor de Kestrel](xref:fundamentals/servers/kestrel), lo que se denomina [modelo de hospedaje fuera de proceso](#out-of-process-hosting-model).

### <a name="in-process-hosting-model"></a>Modelo de hospedaje en proceso

Con el hospedaje en proceso, una aplicación ASP.NET Core se ejecuta en el mismo proceso que su proceso de trabajo de IIS. Esto evita la disminución del rendimiento que supone el envío de solicitudes mediante proxy a través del adaptador de bucle invertido con el modelo de hospedaje fuera de proceso. IIS controla la administración de procesos con el [Servicio de activación de procesos de Windows (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).

El módulo ASP.NET Core:

* Realiza la inicialización de aplicaciones.
  * Carga [CoreCLR](/dotnet/standard/glossary#coreclr).
  * Llama a `Program.Main`.
* Controla la duración de la solicitud nativa de IIS.

En el siguiente diagrama se muestra la relación entre IIS, el módulo ASP.NET Core y una aplicación hospedada en proceso:

![Módulo ASP.NET Core](aspnet-core-module/_static/ancm-inprocess.png)

Una solicitud llega de Internet al controlador HTTP.sys en modo kernel. El controlador enruta la solicitud nativa a IIS en el puerto configurado del sitio web, que suele ser el puerto 80 (HTTP) o 443 (HTTPS). El módulo recibe la solicitud nativa y pasa el control a `IISHttpServer`, que es lo que convierte la solicitud de nativa a administrada.

Una vez que `IISHttpServer` toma la solicitud, la envía a la canalización de middleware de ASP.NET Core. La canalización de middleware controla la solicitud y la pasa como una instancia de `HttpContext` a la lógica de la aplicación. La respuesta de la aplicación se vuelve a pasar a IIS, que la envía de nuevo al cliente HTTP que inició la solicitud.

### <a name="out-of-process-hosting-model"></a>Modelo de hospedaje fuera de proceso

Dado que las aplicaciones ASP.NET Core se ejecutan en un proceso independiente del proceso de trabajo de IIS, el módulo se encarga de la administración de procesos. El módulo inicia el proceso de la aplicación ASP.NET Core cuando entra la primera solicitud y reinicia la aplicación si esta se apaga o se bloquea. Este comportamiento es básicamente el mismo que el de las aplicaciones que se ejecutan en proceso y se administran a través del [Servicio de activación de procesos de Windows (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).

En el siguiente diagrama se muestra la relación entre IIS, el módulo ASP.NET Core y una aplicación hospedada fuera de proceso:

![Módulo ASP.NET Core](aspnet-core-module/_static/ancm-outofprocess.png)

Las solicitudes llegan procedentes de Internet al controlador HTTP.sys en modo kernel. El controlador enruta las solicitudes a IIS en el puerto configurado del sitio web, que suele ser el puerto 80 (HTTP) o 443 (HTTPS). El módulo reenvía las solicitudes a Kestrel en un puerto aleatorio de la aplicación, que no es ni 80 ni 443.

El módulo especifica el puerto a través de la variable de entorno en el inicio y el middleware de integración de IIS configura el servidor para que escuche en `http://localhost:{port}`. Se realizan más comprobaciones y se rechazan las solicitudes que no se hayan originado en el módulo. El módulo no admite el reenvío de HTTPS, por lo que las solicitudes se reenvían a través de HTTP, aunque IIS las reciba a través de HTTPS.

Una vez que Kestrel toma la solicitud del módulo, la envía a la canalización de middleware de ASP.NET Core. La canalización de middleware controla la solicitud y la pasa como una instancia de `HttpContext` a la lógica de la aplicación. El middleware agregado por la integración de IIS actualiza el esquema, la dirección IP remota y PathBase para responder del reenvío de la solicitud a Kestrel. La respuesta de la aplicación se vuelve a pasar a IIS, que la envía de nuevo al cliente HTTP que inició la solicitud.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

El módulo ASP.NET Core es un módulo nativo de IIS que se conecta a la canalización de IIS para reenviar solicitudes web a aplicaciones ASP.NET Core de back-end.

Dado que las aplicaciones ASP.NET Core se ejecutan en un proceso independiente del proceso de trabajo de IIS, el módulo también se encarga de la administración de procesos. El módulo inicia el proceso de la aplicación ASP.NET Core cuando entra la primera solicitud, y reinicia la aplicación si esta se bloquea. Este comportamiento es básicamente el mismo que el de las aplicaciones ASP.NET 4.x que se ejecutan en el proceso en IIS y se administran a través del [Servicio de activación de procesos de Windows (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).

En el siguiente diagrama se muestra la relación entre IIS, el módulo ASP.NET Core y una aplicación:

![Módulo ASP.NET Core](aspnet-core-module/_static/ancm-outofprocess.png)

Las solicitudes llegan procedentes de Internet al controlador HTTP.sys en modo kernel. El controlador enruta las solicitudes a IIS en el puerto configurado del sitio web, que suele ser el puerto 80 (HTTP) o 443 (HTTPS). El módulo reenvía las solicitudes a Kestrel en un puerto aleatorio de la aplicación, que no es ni 80 ni 443.

El módulo especifica el puerto a través de la variable de entorno en el inicio y el middleware de integración de IIS configura el servidor para que escuche en `http://localhost:{port}`. Se realizan más comprobaciones y se rechazan las solicitudes que no se hayan originado en el módulo. El módulo no admite el reenvío de HTTPS, por lo que las solicitudes se reenvían a través de HTTP, aunque IIS las reciba a través de HTTPS.

Una vez que Kestrel toma la solicitud del módulo, la envía a la canalización de middleware de ASP.NET Core. La canalización de middleware controla la solicitud y la pasa como una instancia de `HttpContext` a la lógica de la aplicación. El middleware agregado por la integración de IIS actualiza el esquema, la dirección IP remota y PathBase para responder del reenvío de la solicitud a Kestrel. La respuesta de la aplicación se vuelve a pasar a IIS, que la envía de nuevo al cliente HTTP que inició la solicitud.

::: moniker-end

Muchos de los módulos nativos, como la autenticación de Windows, permanecen activos. Para obtener más información sobre los módulos de IIS activos con el módulo, vea <xref:host-and-deploy/iis/modules>.

El módulo ASP.NET Core tiene otras funciones. Puede:

* Establecer variables de entorno para un proceso de trabajo.
* Registrar la salida en un almacenamiento de archivos para solucionar problemas de inicio.
* Reenviar tokens de autenticación de Windows.

## <a name="how-to-install-and-use-the-aspnet-core-module"></a>Cómo instalar y usar el módulo ASP.NET Core

Para obtener instrucciones detalladas sobre cómo instalar y usar el módulo ASP.NET Core, vea <xref:host-and-deploy/iis/index>. Para obtener información sobre cómo configurar el módulo, vea <xref:host-and-deploy/aspnet-core-module>.

## <a name="additional-resources"></a>Recursos adicionales

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* [Repositorio GitHub del módulo ASP.NET Core (código fuente)](https://github.com/aspnet/AspNetCoreModule)
* <xref:host-and-deploy/iis/modules>
