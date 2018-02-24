---
title: "Referencia de configuración del módulo principal ASP.NET"
author: guardrex
description: "Obtenga información acerca de cómo configurar el módulo principal de ASP.NET para hospedar aplicaciones ASP.NET Core."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: c01abed767a226eae68725c1c53d922eac2f705e
ms.sourcegitcommit: 49fb3b7669b504d35edad34db8285e56b958a9fc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/23/2018
---
# <a name="aspnet-core-module-configuration-reference"></a>Referencia de configuración del módulo principal ASP.NET

Por [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), y [Sourabh Shirhatti](https://twitter.com/sshirhatti)

Este documento proporciona instrucciones sobre cómo configurar el módulo principal de ASP.NET para hospedar aplicaciones ASP.NET Core. Para obtener una introducción para el módulo principal de ASP.NET y las instrucciones de instalación, consulte el [información general del módulo de ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).

## <a name="configuration-with-webconfig"></a>Configuración de web.config

El módulo de núcleo de ASP.NET está configurado con el `aspNetCore` sección de la `system.webServer` nodo en el sitio *web.config* archivo.

El siguiente *web.config* archivo se publica para un [framework dependiente implementación](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) y configura el módulo principal de ASP.NET para controlar las solicitudes de sitios:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="dotnet" 
                arguments=".\MyApp.dll" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

El siguiente *web.config* se publica para un [implementación independiente](/dotnet/articles/core/deploying/#self-contained-deployments-scd):

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath=".\MyApp.exe" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

Cuando se implementa una aplicación en [servicio de aplicaciones de Azure](https://azure.microsoft.com/services/app-service/), `stdoutLogFile` ruta de acceso se establece en `\\?\%home%\LogFiles\stdout`. La ruta de acceso guarda los registros de stdout para la *LogFiles* carpeta, que es una ubicación automáticamente creados por el servicio.

Vea [configuración de la Sub-aplicación](xref:host-and-deploy/iis/index#sub-application-configuration) para una nota importante relacionada con la configuración de *web.config* archivos en las subcarpetas de aplicaciones.

### <a name="attributes-of-the-aspnetcore-element"></a>Atributos del elemento aspNetCore

| Atributo | Descripción | Default |
| --------- | ----------- | :-----: |
| `arguments` | <p>Atributo de cadena opcional.</p><p>Argumentos para el ejecutable especificado en **processPath**.</p>| |
| `disableStartUpErrorPage` | true o false</p><p>Si es true, el **502.5 - Error de proceso** se suprime la página y la página de códigos de 502 estado configurado en el *web.config* tiene prioridad.</p> | `false` |
| `forwardWindowsAuthToken` | true o false</p><p>Si es true, el token se reenvía al proceso secundario escucha en % ASPNETCORE_PORT % como un encabezado 'MS-ASPNETCORE-WINAUTHTOKEN' por solicitud. Es responsabilidad de dicho proceso para llamar a CloseHandle en este token por solicitud.</p> | `true` |
| `processPath` | <p>Atributo de cadena necesario.</p><p>Ruta de acceso al archivo ejecutable que se inicia un proceso de escucha las solicitudes HTTP. Se admiten rutas de acceso relativas. Si la ruta de acceso comienza con `.`, la ruta de acceso se considera que son relativas a la raíz del sitio.</p> | |
| `rapidFailsPerMinute` | <p>Atributo de entero opcional.</p><p>Especifica el número de veces especificado por el proceso de **processPath** se permite el bloqueo por minuto. Si se supera este límite, el módulo deja de iniciar el proceso durante el resto del minuto.</p> | `10` |
| `requestTimeout` | <p>Atributo timespan opcional.</p><p>Especifica la duración para la que el módulo principal de ASP.NET espera una respuesta desde el proceso de escucha en % ASPNETCORE_PORT %.</p><p>El `requestTimeout` debe especificarse en minutos enteros, en caso contrario, el valor predeterminado es 2 minutos.</p> | `00:02:00` |
| `shutdownTimeLimit` | <p>Atributo de entero opcional.</p><p>Duración en segundos que espera a que el módulo de archivo ejecutable que desea apagar normalmente cuando la *app_offline.htm* se detecta que el archivo.</p> | `10` |
| `startupTimeLimit` | <p>Atributo de entero opcional.</p><p>Duración en segundos que espera a que el módulo para que el ejecutable iniciar un proceso escuchando en el puerto. Si se supera este límite de tiempo, el módulo elimina el proceso. El módulo intenta reiniciar el proceso cuando se recibe una solicitud nueva y sigue intentando reiniciar el proceso en las posteriores solicitudes entrantes a menos que la aplicación no se puede iniciar **rapidFailsPerMinute** número de veces en los últimos minuto gradual.</p> | `120` |
| `stdoutLogEnabled` | <p>Atributo Boolean opcional.</p><p>Si es true, **stdout** y **stderr** para el proceso especificado en **processPath** se redirigen al archivo especificado en **stdoutLogFile**.</p> | `false` |
| `stdoutLogFile` | <p>Atributo de cadena opcional.</p><p>Especifica la ruta de acceso relativa o absoluta para el que **stdout** y **stderr** desde el proceso especificado en **processPath** se registran. Rutas de acceso relativas son relativas a la raíz del sitio. Cualquier ruta de acceso a partir de `.` son relativa al sitio raíz y todas las demás rutas de acceso se tratan como rutas de acceso absolutas. Las carpetas que se proporcionan en la ruta de acceso deben existir en orden para el módulo crear el archivo de registro. Usar delimitadores de carácter de subrayado, una marca de tiempo, Id. de proceso y extensión de archivo (*.log*) se agregan al último segmento de la **stdoutLogFile** ruta de acceso. Si `.\logs\stdout` se proporciona como un valor, se guarda un registro de ejemplo stdout como *stdout_20180205194132_1934.log* en el *registros* carpeta, cuando se guardan en 2/5/2018 a 19:41:32 con un identificador de proceso de 1934.</p> | `aspnetcore-stdout` |

### <a name="setting-environment-variables"></a>Establecer variables de entorno

Se pueden especificar variables de entorno para el proceso en el `processPath` atributo. Especifique una variable de entorno con el `environmentVariable` elemento secundario de un `environmentVariables` elemento de la colección. Las variables de entorno establecidas en esta sección tienen prioridad sobre el sistema las variables de entorno.

En el ejemplo siguiente se establece dos variables de entorno. `ASPNETCORE_ENVIRONMENT` configura el entorno de la aplicación para `Development`. Un programador puede establecer temporalmente este valor el *web.config* archivo con el fin de forzar la [Developer excepción página](xref:fundamentals/error-handling) cargar al depurar una excepción de aplicación. `CONFIG_DIR` es un ejemplo de una variable de entorno definida por el usuario, donde el desarrollador ha escrito código que lee el valor de inicio para formar una ruta de acceso para cargar el archivo de configuración de la aplicación.

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

> [!WARNING]
> Solo establecer la `ASPNETCORE_ENVIRONMENT` envirnonment variable `Development` en probar y ensayar los servidores que no son accesibles a redes de confianza, como Internet.

## <a name="appofflinehtm"></a>app_offline.htm

Si un archivo con el nombre *app_offline.htm* se detecta en el directorio raíz de una aplicación, el módulo principal de ASP.NET intenta correctamente, cierre la aplicación y detener el procesamiento de las solicitudes entrantes. Si la aplicación se está ejecutando después del número de segundos que se definen en `shutdownTimeLimit`, el módulo principal de ASP.NET elimina el proceso en ejecución.

Mientras el *app_offline.htm* archivo está presente, el módulo principal de ASP.NET responde a solicitudes devolviendo el contenido de la *app_offline.htm* archivo. Cuando el *app_offline.htm* se quita el archivo, la solicitud siguiente inicia la aplicación.

## <a name="start-up-error-page"></a>Página de error de inicio

Si se produce un error en el módulo principal de ASP.NET iniciar el proceso de back-end o el proceso de back-end comienza pero no puede escuchar en el puerto configurado, un *502.5 Error de un proceso* aparece la página de códigos de estado. Para suprimir esta página y volver a la página de códigos de estado de IIS 502 predeterminado, utilice el `disableStartUpErrorPage` atributo. Para obtener más información acerca de cómo configurar mensajes de error personalizados, vea [errores HTTP `<httpErrors>` ](/iis/configuration/system.webServer/httpErrors/).

![502.5 página de códigos de estado de error de proceso de](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a>Creación de registro y de redirección

El módulo principal de ASP.NET redirige `stdout` y `stderr` registros en el disco si la `stdoutLogEnabled` y `stdoutLogFile` atributos de la `aspNetCore` se establecen el elemento. Las carpetas en el `stdoutLogFile` ruta debe existir en orden para el módulo crear el archivo de registro. Una marca de tiempo y extensión de archivo se agregan automáticamente cuando se crea el archivo de registro. Los registros no estén girado, salvo que se produzca el reinicio/reciclaje del proceso. Es responsabilidad del proveedor de hospedaje para limitar los registros de consumen el espacio en disco. Mediante el `stdout` registro sólo se recomienda para solucionar problemas de inicio de la aplicación. No use el registro de stdout para fines de registro de aplicación general. Para registrar la rutina en una aplicación de ASP.NET Core, utiliza una biblioteca de registro que limita el tamaño del archivo de registro y gira registros. Para obtener más información, consulte [proveedores de registro de aplicaciones de terceros](xref:fundamentals/logging/index#third-party-logging-providers).

El nombre de archivo de registro se crea anexando la marca de tiempo, el Id. de proceso y la extensión de archivo (*.log*) para el último segmento de la `stdoutLogFile` ruta de acceso (normalmente *stdout*) delimitados por caracteres de subrayado. Si el `stdoutLogFile` ruta de acceso finaliza con *stdout*, un registro de una aplicación con un PID de 1934 creado en 2/5/2018 a 19:42:32 tiene el nombre de archivo *stdout_20180205194132_1934.log*.

El ejemplo siguiente `aspNetCore` elemento configura `stdout` el registro para una aplicación hospedada en el servicio de aplicación de Azure. Una ruta de acceso local o una ruta de acceso de recurso compartido de red es aceptable para el registro local. Confirme que la identidad del usuario AppPool tiene permiso para escribir en la ruta de acceso proporcionada.

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

Vea [configuración con web.config](#configuration-with-webconfig) para obtener un ejemplo de la `aspNetCore` elemento en el *web.config* archivo.

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a>Configuración compartida de módulo de núcleo de ASP.NET con un servicio de IIS

El instalador del módulo de núcleo de ASP.NET se ejecuta con los privilegios de la **SYSTEM** cuenta. Dado que la cuenta de sistema local no tiene permiso para la ruta de acceso de recurso compartido utilizado por la configuración de IIS compartido para modificar, el instalador llega a un error de acceso denegado al intentar configurar los ajustes del módulo en *applicationHost.config* en el recurso compartido. Cuando se usa una configuración de IIS compartido, siga estos pasos:

1. Deshabilitar la configuración de IIS compartido.
1. Ejecute el instalador.
1. Exportar actualizado *applicationHost.config* archivos al recurso compartido.
1. Volver a habilitar la configuración de IIS compartido.

## <a name="module-version-and-hosting-bundle-installer-logs"></a>Versión del módulo y el hospedaje de registros del instalador de paquete

Para determinar la versión del módulo de núcleo de ASP.NET instaladas:

1. En el sistema host, vaya a *%windir%\System32\inetsrv*.
1. Busque la *aspnetcore.dll* archivo.
1. Haga clic en el archivo y seleccione **propiedades** en el menú contextual.
1. Seleccione el **detalles** ficha. El **versión del archivo** y **versión del producto** representan la versión instalada del módulo.

Los registros de instalador de agrupación de hospedaje de Windows Server para el módulo se encuentran en *C:\\usuarios\\% UserName %\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.

## <a name="module-schema-and-configuration-file-locations"></a>Ubicaciones de archivo de módulo, esquema y configuración

### <a name="module"></a>Module

**IIS (x86/amd64):**

   * %windir%\System32\inetsrv\aspnetcore.dll

   * %windir%\SysWOW64\inetsrv\aspnetcore.dll

**IIS Express (x86/amd64):**

   * %ProgramFiles%\IIS Express\aspnetcore.dll

   * %ProgramFiles(x86)%\IIS Express\aspnetcore.dll

### <a name="schema"></a>Schema

**IIS**

   * %windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml

**IIS Express**

   * %ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml

### <a name="configuration"></a>Configuración

**IIS**

   * %windir%\System32\inetsrv\config\applicationHost.config

**IIS Express**

   * .vs\config\applicationHost.config

Los archivos se pueden encontrar buscando *aspnetcore.dll* en el *applicationHost.config* archivo. Para IIS Express, la *applicationHost.config* el archivo no existe de forma predeterminada. El archivo se crea en  *\<application_root >\\VS\\config* al iniciar cualquier proyecto de aplicación web en la solución de Visual Studio.
