---
title: "Módulo principal de ASP.NET"
author: tdykstra
description: "Presenta el módulo de núcleo de ASP.NET (ANCM), un módulo IIS que permite que el servidor de web Kestrel usa IIS o IIS Express como un servidor proxy inverso."
keywords: "Módulo de núcleo Express,ASP.NET núcleo de ASP.NET, IIS, IIS, UseIISIntegration"
ms.author: tdykstra
manager: wpickett
ms.date: 08/03/2017
ms.topic: article
ms.assetid: 4661af33-34c5-4d71-93a0-8c7632f43580
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/aspnet-core-module
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5eef9405c0c3d219755d7cffa5d45c3df45ddb5c
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2018
---
# <a name="introduction-to-aspnet-core-module"></a>Introducción al módulo principal de ASP.NET

Por [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), y [Chris Ross](https://github.com/Tratcher) 

Módulo de núcleo ASP.NET (ANCM) le permite ejecutar aplicaciones de IIS, de ASP.NET Core mediante IIS para lo que es bueno (seguridad, facilidad de uso y mucho más) y [Kestrel](kestrel.md) para lo que es bueno (que se va a agilizar el proceso) y la obtención de la ventajas de ambas tecnologías al mismo tiempo. **ANCM solo funciona con Kestrel; no es compatible con WebListener (en ASP.NET Core 1.x) o HTTP.sys (en 2.x).** 

Versiones admitidas de Windows:

* Windows 7 y Windows Server 2008 R2 y versiones posteriores

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/aspnet-core-module/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-aspnet-core-module-does"></a>¿Qué módulo principal de ASP.NET

ANCM es un módulo nativo de IIS que enlaza a la canalización IIS y redirija el tráfico a la aplicación ASP.NET básica de back-end. La mayoría otros módulos, como la autenticación de windows, sigue recibiendo una oportunidad de ejecutarse. ANCM solo toma el control cuando se selecciona un controlador para la solicitud y asignación de controlador se define en la aplicación *web.config* archivo.

Dado que las aplicaciones ASP.NET Core se ejecutan en un proceso independientes del proceso de trabajo IIS, ANCM también administración de procesos. ANCM inicia el proceso de la aplicación de ASP.NET Core cuando la primera solicitud entra y lo reinicia cuando se bloquea. Esto es básicamente el mismo comportamiento que las aplicaciones ASP.NET clásicas que se ejecutan en proceso en IIS y se administran mediante el servicio WAS (Windows Activation Service).

Éste es un diagrama que ilustra la relación entre las aplicaciones de IIS, ANCM y ASP.NET Core.

![Módulo principal de ASP.NET](aspnet-core-module/_static/ancm.png)

Las solicitudes proceden la Web y alcanza al controlador Http.Sys en modo kernel que se enruta en IIS en el puerto principal (80) o SSL (443). ANCM reenvía las solicitudes a la aplicación de ASP.NET Core en el puerto HTTP configurado para la aplicación, que no es el puerto 80/443.

Kestrel escucha el tráfico procedente de ANCM.  ANCM especifica el puerto a través de la variable de entorno en el inicio y la [UseIISIntegration](#call-useiisintegration) método configura el servidor para que escuche en `http://localhost:{port}`. Existen comprobaciones adicionales para rechazar las solicitudes no del ANCM. (ANCM no admite HTTPS de reenvío, por lo que las solicitudes se reenvían a través de HTTP, aunque IIS recibe a través de HTTPS.)

Kestrel toma las solicitudes de ANCM y los envía a la canalización de middleware de ASP.NET Core, que, a continuación, reacciona ante ellas y se las pasa como `HttpContext` instancias de lógica de la aplicación. Las respuestas de la aplicación, a continuación, se pasan a IIS, las inserciones ellos revertir al cliente HTTP que inició las solicitudes.

ANCM tiene algunas otras funciones:

* Establece las variables de entorno.
* Registros `stdout` al almacenamiento de archivos de salida.
* Reenvía los tokens de autenticación de Windows.

## <a name="how-to-use-ancm-in-aspnet-core-apps"></a>Cómo usar ANCM en aplicaciones de ASP.NET Core

Esta sección proporciona información general sobre el proceso para configurar un servidor IIS y la aplicación de ASP.NET Core. Para obtener instrucciones detalladas, consulte [Host en Windows con IIS](xref:host-and-deploy/iis/index).

### <a name="install-ancm"></a>Instalar ANCM


El módulo principal de ASP.NET debe instalarse en IIS en los servidores y en IIS Express en los equipos de desarrollo. Para los servidores, ANCM se incluye en el [agrupación de hospedaje de .NET Core Windows Server](https://aka.ms/dotnetcore-2-windowshosting). Para equipos de desarrollo, Visual Studio instala automáticamente ANCM en IIS Express y en IIS si ya está instalado en el equipo.

### <a name="install-the-iisintegration-nuget-package"></a>Instale el paquete IISIntegration NuGet

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

El [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) paquete se incluye en el metapackages de ASP.NET Core ([Microsoft.AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore/) y [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) ). Si no usa uno de los metapackages, instale `Microsoft.AspNetCore.Server.IISIntegration` por separado. El `IISIntegration` paquete es un módulo de interoperabilidad que lee las variables de entorno de difusión por ANCM para configurar la aplicación. Las variables de entorno proporcionan información de configuración, por ejemplo, el puerto para escuchar en. 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

En la aplicación, instale [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/). El `IISIntegration` paquete es un módulo de interoperabilidad que lee las variables de entorno de difusión por ANCM para configurar la aplicación. Las variables de entorno proporcionan información de configuración, por ejemplo, el puerto para escuchar en. 

---

### <a name="call-useiisintegration"></a>Llamar a UseIISIntegration

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

El `UseIISIntegration` método de extensión [ `WebHostBuilder` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) se llama automáticamente cuando se ejecuta con IIS.

Si no está usando uno de los metapackages de ASP.NET Core y no tiene instalado el `Microsoft.AspNetCore.Server.IISIntegration` paquete, obtendrá un error de tiempo de ejecución. Si se llama a `UseIISIntegration` explícitamente, obtendrá un error en tiempo de compilación si el paquete no está instalado.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

En la aplicación `Main` método, llame a la `UseIISIntegration` método de extensión [ `WebHostBuilder` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder). 

[!code-csharp[](aspnet-core-module/sample/Program.cs?name=snippet_Main&highlight=12)]

---

El `UseIISIntegration` método busca variables de entorno que establece ANCM y no ops si no se encuentran. Este comportamiento facilita escenarios, como el desarrollo y pruebas en Mac OS o Linux y la implementación en un servidor que ejecuta IIS. Mientras se ejecuta en Mac OS o Linux, Kestrel actúa como el servidor web; pero cuando la aplicación se implementa en el entorno de IIS, utiliza automáticamente ANCM e IIS.

### <a name="ancm-port-binding-overrides-other-port-bindings"></a>Enlace de puerto ANCM invalida otros enlaces de puerto

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

ANCM genera un puerto dinámico que se asigna al proceso de back-end. El `UseIISIntegration` método toma este puerto dinámico y configura Kestrel para que escuche en `http://locahost:{dynamicPort}/`. Esto reemplaza a otras configuraciones de direcciones URL, como las llamadas a `UseUrls` o [escuchar API del Kestrel](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration). Por lo tanto, no es necesario llamar a `UseUrls` o de Kestrel `Listen` API cuando usa ANCM. Si se llama a `UseUrls` o `Listen`, Kestrel escucha en el puerto que especifique al ejecutar la aplicación sin IIS.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

ANCM genera un puerto dinámico que se asigna al proceso de back-end. El `UseIISIntegration` método toma este puerto dinámico y configura Kestrel para que escuche en `http://locahost:{dynamicPort}/`. Esto reemplaza a otras configuraciones de direcciones URL, como las llamadas a `UseUrls`. Por lo tanto, no es necesario llamar a `UseUrls` cuando usa ANCM. Si se llama a `UseUrls`, Kestrel escucha en el puerto que especifique al ejecutar la aplicación sin IIS.

En ASP.NET Core 1.0, si se llama a `UseUrls`, llamarlo **antes de** llama a `UseIISIntegration` para que el puerto configurado ANCM no se sobrescribe. Este orden de llamada no es necesario en ASP.NET Core 1.1, dado que reemplaza la configuración de ANCM `UseUrls`.

---

### <a name="configure-ancm-options-in-webconfig"></a>Configurar las opciones de ANCM en el archivo Web.config

Configuración para el módulo de núcleo de ASP.NET se almacena en la *web.config* archivo que se encuentra en la carpeta raíz de la aplicación. Punto de configuración de este archivo para el comando de inicio y los argumentos que se inicie la aplicación de ASP.NET Core. Para el ejemplo *web.config* código e instrucciones sobre las opciones de configuración, consulte [referencia de configuración del módulo de ASP.NET principal](xref:host-and-deploy/aspnet-core-module).

### <a name="run-with-iis-express-in-development"></a>Ejecutar con IIS Express en el desarrollo

IIS Express se puede iniciar Visual Studio utilizando el perfil predeterminado definido por las plantillas de ASP.NET Core.

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a>Configuración de proxy usa el protocolo HTTP y un token de emparejamiento

El proxy creado entre el ANCM y Kestrel utiliza el protocolo HTTP. Uso de HTTP es una optimización del rendimiento donde el tráfico entre el ANCM y Kestrel realiza en una dirección de bucle invertido fuera de la interfaz de red. No hay ningún riesgo de interceptación el tráfico entre el ANCM y Kestrel desde una ubicación fuera del servidor.

Un token de emparejamiento se utiliza para garantizar que las solicitudes recibidas por Kestrel eran procesadas por IIS y que no proceden de algún otro origen. El token de emparejamiento se crea y se ha establecido en una variable de entorno (`ASPNETCORE_TOKEN`) por el ANCM. El token de emparejamiento también se establece en un encabezado (`MSAspNetCoreToken`) en todas las solicitudes procesadas por el proxy. Middleware de IIS comprueba cada solicitud recibida para confirmar que el valor de encabezado del token emparejamiento coincide con el valor de la variable de entorno. Si no coinciden con los valores del token, la solicitud se registran y se rechaza. La variable de entorno token emparejamiento y el tráfico entre el ANCM y Kestrel no son accesibles desde una ubicación fuera del servidor. Sin conocer el valor del token de emparejamiento, un atacante no puede enviar las solicitudes que se omisión la comprobación en el Middleware de IIS.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información, vea los siguientes recursos:

* [Aplicación de ejemplo de este artículo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/aspnet-core-module/sample)
* [Código de origen del módulo principal ASP.NET](https://github.com/aspnet/AspNetCoreModule)
* [Referencia de configuración del módulo de ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
* [Hospedaje en Windows con IIS](xref:host-and-deploy/iis/index)
