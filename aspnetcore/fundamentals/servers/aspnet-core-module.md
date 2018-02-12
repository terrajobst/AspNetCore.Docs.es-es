---
title: "Módulo ASP.NET Core"
author: tdykstra
description: "En este artículo se presenta el módulo ASP.NET Core (ANCM), un módulo de IIS que permite que el servidor web Kestrel use IIS o IIS Express como un servidor proxy inverso."
manager: wpickett
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 08/03/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/aspnet-core-module
ms.openlocfilehash: 4337bc42c5454d6a9634a396d9c89f3518af148b
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-aspnet-core-module"></a>Introducción al módulo ASP.NET Core

Por [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl) y [Chris Ross](https://github.com/Tratcher) 

El módulo ASP.NET Core (ANCM) permite ejecutar aplicaciones ASP.NET Core por detrás de IIS: usa IIS por sus puntos fuertes (seguridad, facilidad de uso y mucho más) y usa [Kestrel](kestrel.md) por sus puntos fuertes (agiliza mucho los procesos), logrando así reunir las ventajas de las dos tecnologías en una sola. **ANCM solo funciona con Kestrel; no es compatible con WebListener (en ASP.NET Core 1.x) ni HTTP.sys (en 2.x).** 

Versiones de Windows compatibles:

* Windows 7 y Windows Server 2008 R2 y versiones posteriores

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/aspnet-core-module/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-aspnet-core-module-does"></a>Qué hace el módulo ASP.NET Core

ANCM es un módulo de IIS nativo que se enlaza a la canalización IIS y redirige el tráfico a la aplicación back-end de ASP.NET Core. La mayoría de los demás módulos, como Autenticación de Windows, siguen teniendo una oportunidad de ejecutarse. ANCM solo toma el control cuando se selecciona un controlador para la solicitud y se define la asignación del controlador en el archivo *web.config* de la aplicación.

Dado que las aplicaciones ASP.NET Core se ejecutan en un proceso independiente del proceso de trabajo de IIS, ANCM también realiza la administración de procesos. ANCM inicia el proceso de la aplicación ASP.NET Core cuando entra la primera solicitud y lo reinicia cuando se bloquea. Este comportamiento es básicamente el mismo que el de las aplicaciones ASP.NET clásicas que se ejecutan en el proceso en IIS y se administran mediante el servicio WAS (Windows Activation Service).

En este diagrama se muestra la relación entre las aplicaciones IIS, ANCM y ASP.NET Core.

![Módulo ASP.NET Core](aspnet-core-module/_static/ancm.png)

Las solicitudes proceden de la web y alcanzan al controlador Http.Sys en modo kernel, que las enruta en IIS en el puerto principal (80) o el puerto SSL (443). ANCM reenvía las solicitudes a la aplicación ASP.NET Core en el puerto HTTP configurado para la aplicación, que no es el puerto 80 o 443.

Kestrel escucha el tráfico procedente de ANCM.  ANCM especifica el puerto a través de la variable de entorno en el inicio y el método [UseIISIntegration](#call-useiisintegration) configura el servidor para que escuche en `http://localhost:{port}`. Existen comprobaciones adicionales para rechazar las solicitudes que no procedan de ANCM. (ANCM no admite el reenvío de HTTPS, por lo que las solicitudes se reenvían a través de HTTP, aunque IIS las reciba a través de HTTPS).

Kestrel toma las solicitudes de ANCM y las envía a la canalización de middleware de ASP.NET Core que, a su vez, las administra y las pasa como instancias de `HttpContext` a la lógica de la aplicación. Después, las respuestas de la aplicación se pasan de nuevo a IIS, que las envía de nuevo al cliente HTTP que inició las solicitudes.

ANCM tiene también otras funciones:

* Establece las variables de entorno.
* Registra los resultados de `stdout` en el almacenamiento de archivos.
* Reenvía tokens de autenticación de Windows.

## <a name="how-to-use-ancm-in-aspnet-core-apps"></a>Cómo usar ANCM en aplicaciones ASP.NET Core

En esta sección se ofrece información general sobre el proceso de configuración de un servidor IIS y la aplicación ASP.NET Core. Para instrucciones más detalladas, vea [Hospedaje de ASP.NET Core en Windows con IIS](xref:host-and-deploy/iis/index).

### <a name="install-ancm"></a>Instalar ANCM


El módulo ASP.NET Core debe instalarse en IIS para los servidores y en IIS Express para las máquinas de desarrollo. Para los servidores, ANCM se incluye en el [conjunto de hospedaje de Windows Server de .NET Core](https://aka.ms/dotnetcore-2-windowshosting). Para las máquinas de desarrollo, Visual Studio instala automáticamente ANCM en IIS Express o en IIS si este ya está instalado en la máquina.

### <a name="install-the-iisintegration-nuget-package"></a>Instale el paquete NuGet IISIntegration

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

El paquete [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) se incluye en los metapaquetes de ASP.NET Core ([Microsoft.AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore/) y [Microsoft.AspNetCore.All](xref:fundamentals/metapackage)). Si no usa uno de los metapaquetes, instale `Microsoft.AspNetCore.Server.IISIntegration` por separado. El paquete `IISIntegration` es un paquete de interoperabilidad que lee las variables de entorno difundidas por ANCM para configurar la aplicación. Las variables de entorno proporcionan información sobre la configuración, como el puerto donde escuchar. 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

En la aplicación, instale [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/). El paquete `IISIntegration` es un paquete de interoperabilidad que lee las variables de entorno difundidas por ANCM para configurar la aplicación. Las variables de entorno proporcionan información sobre la configuración, como el puerto donde escuchar. 

---

### <a name="call-useiisintegration"></a>Llamar a UseIISIntegration

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Cuando se ejecuta con IIS se llama automáticamente al método de extensión `UseIISIntegration` en [`WebHostBuilder`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder).

Si no está usando uno de los metapaquetes de ASP.NET Core y no ha instalado el paquete `Microsoft.AspNetCore.Server.IISIntegration`, obtendrá un error de tiempo de ejecución. Si llama expresamente a `UseIISIntegration`, obtendrá un error en tiempo de compilación si el paquete no está instalado.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

En el método `Main` de la aplicación, llame al método de extensión `UseIISIntegration` en [`WebHostBuilder`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder). 

[!code-csharp[](aspnet-core-module/sample/Program.cs?name=snippet_Main&highlight=12)]

---

El método `UseIISIntegration` busca variables de entorno establecidas por ANCM y no realiza ninguna operación si no las encuentra. Este comportamiento facilita escenarios, como el de desarrollo y prueba en macOS o Linux, y el de implementación en un servidor que ejecuta IIS. Mientras se ejecuta en MacOS o Linux, Kestrel actúa como el servidor web; pero cuando la aplicación se implementa en el entorno de IIS, usa automáticamente ANCM e IIS.

### <a name="ancm-port-binding-overrides-other-port-bindings"></a>El enlace de puerto de ANCM invalida otros enlaces de puerto

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

ANCM genera un puerto dinámico que se asigna al proceso back-end. El método `UseIISIntegration` toma este puerto dinámico y configura Kestrel para que escuche en `http://locahost:{dynamicPort}/`. Esto invalida otras configuraciones de URL, como las llamadas a `UseUrls` o a la [API Listen de Kestrel](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration). Por tanto, no es necesario llamar a `UseUrls` o a la API `Listen` de Kestrel cuando se usa ANCM. Si se llama a `UseUrls` o `Listen`, Kestrel escucha en el puerto que especificó al ejecutar la aplicación sin IIS.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

ANCM genera un puerto dinámico que se asigna al proceso back-end. El método `UseIISIntegration` toma este puerto dinámico y configura Kestrel para que escuche en `http://locahost:{dynamicPort}/`. Esto invalida otras configuraciones de URL, como las llamadas a `UseUrls`. Por tanto, no es necesario llamar a `UseUrls` cuando usa ANCM. Si se llama a `UseUrls`, Kestrel escucha en el puerto que especificó al ejecutar la aplicación sin IIS.

En ASP.NET Core 1.0, si llama a `UseUrls`, hágalo **antes** de llamar a `UseIISIntegration` para que el puerto configurado de ANCM no se sobrescriba. Este orden de llamada no es necesario en ASP.NET Core 1.1, ya que la configuración de ANCM reemplaza `UseUrls`.

---

### <a name="configure-ancm-options-in-webconfig"></a>Configurar opciones de ANCM en web.config

La configuración para el módulo ASP.NET Core se almacena en el archivo *web.config* que se encuentra en la carpeta raíz de la aplicación. La configuración de este archivo señala el comando de inicio y los argumentos que inician la aplicación ASP.NET Core. Para obtener código de ejemplo de *web.config* e instrucciones para las opciones de configuración, vea [ASP.NET Core Module Configuration Reference](xref:host-and-deploy/aspnet-core-module) (Referencia de configuración del módulo ASP.NET Core).

### <a name="run-with-iis-express-in-development"></a>Ejecutar con IIS Express en el desarrollo

Visual Studio puede iniciar IIS Express usando el perfil predeterminado definido por las plantillas de ASP.NET Core.

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a>La configuración de proxy usa el protocolo HTTP y un token de emparejamiento

El proxy creado entre ANCM y Kestrel usa el protocolo HTTP. Al usar HTTP se optimiza el rendimiento, ya que el tráfico entre ANCM y Kestrel se realiza en una dirección de bucle invertido fuera de la interfaz de red. No hay ningún riesgo de interceptación del tráfico entre ANCM y Kestrel desde una ubicación fuera del servidor.

Un token de emparejamiento sirve para garantizar que las solicitudes recibidas por Kestrel se redirigieron mediante proxy por IIS y no procedieron de otra fuente. ANCM crea el token de emparejamiento y lo establece en una variable de entorno (`ASPNETCORE_TOKEN`). El token de emparejamiento también se establece en un encabezado (`MSAspNetCoreToken`) en cada solicitud redirigida mediante proxy. El middleware de IIS comprueba cada solicitud recibida para confirmar que el valor del encabezado del token de emparejamiento coincida con el valor de la variable de entorno. Si los valores del token no coinciden, la solicitud se registra y se rechaza. No se puede acceder a la variable de entorno del token de emparejamiento y al tráfico entre ANCM y Kestrel desde una ubicación fuera del servidor. Sin conocer el valor del token de emparejamiento, un atacante no puede enviar solicitudes que omitan la comprobación en el middleware de IIS.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información, vea los siguientes recursos:

* [Aplicación de ejemplo para este artículo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/aspnet-core-module/sample)
* [Código de origen del módulo ASP.NET Core](https://github.com/aspnet/AspNetCoreModule)
* [Referencia de configuración del módulo de ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
* [Hospedaje en Windows con IIS](xref:host-and-deploy/iis/index)
