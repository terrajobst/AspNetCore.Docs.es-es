---
title: "Referencia de configuración del módulo principal ASP.NET"
author: guardrex
description: "Cómo configurar el módulo principal de ASP.NET para hospedar aplicaciones ASP.NET Core."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 03/07/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: 7bb7e5b9c821f87e73763f5f5c4f9fbcd751235f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
# <a name="aspnet-core-module-configuration-reference"></a>Referencia de configuración del módulo principal ASP.NET

Por [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), y [Sourabh Shirhatti](https://twitter.com/sshirhatti)

Este documento proporciona información sobre cómo configurar el módulo de núcleo de ASP.NET para hospedar aplicaciones ASP.NET Core. Para obtener una introducción para el módulo principal de ASP.NET y las instrucciones de instalación, consulte el [información general del módulo de ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).

## <a name="configuration-via-webconfig"></a>Configuración a través de web.config

El módulo principal de ASP.NET se configura a través de una aplicación o sitio *web.config* de archivos y tiene su propio `aspNetCore` sección de configuración dentro de `system.webServer`. Este es un ejemplo *web.config* de archivos que la `Microsoft.NET.Sdk.Web` SDK se proporcionará cuando se publica el proyecto para un [framework dependiente implementación](https://docs.microsoft.com/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) con marcadores de posición para el `processPath` y `arguments`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="%LAUNCHER_PATH%" 
        arguments="%LAUNCHER_ARGS%" 
        stdoutLogEnabled="false" 
        stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

El *web.config* ejemplo siguiente es para un [implementación independiente](https://docs.microsoft.com/dotnet/articles/core/deploying/#self-contained-deployments-scd) a la [servicio de aplicaciones de Azure](https://azure.microsoft.com/services/app-service/). Para obtener más información, consulte [Host en Windows con IIS](xref:host-and-deploy/iis/index). Vea [configuración de aplicaciones del subelemento](xref:host-and-deploy/iis/index#configuration-of-sub-applications) para una nota importante relacionada con la configuración de *web.config* archivos en subcarpetas de las aplicaciones.

```xml
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath=".\MyApp.exe" 
        stdoutLogEnabled="false" 
        stdoutLogFile="\\?\%home%\LogFiles\stdout" />
  </system.webServer>
</configuration>
```

### <a name="attributes-of-the-aspnetcore-element"></a>Atributos del elemento aspNetCore

| Atributo | Descripción |
| --- | --- |
| processPath | <p>Atributo de cadena necesario.</p><p>Ruta de acceso al archivo ejecutable que se iniciará un proceso de escucha las solicitudes HTTP. Se admiten rutas de acceso relativas. Si la ruta de acceso comienza con '.', la ruta de acceso se considera que son relativas a la raíz del sitio.</p><p>No hay ningún valor predeterminado.</p> |
| argumentos | <p>Atributo de cadena opcional.</p><p>Argumentos para el ejecutable especificado en **processPath**.</p><p>El valor predeterminado es una cadena vacía.</p> |
| startupTimeLimit | <p>Atributo de entero opcional.</p><p>Duración en segundos que esperará el módulo para que el ejecutable iniciar un proceso escuchando en el puerto. Si se supera este límite de tiempo, el módulo terminará el proceso. El módulo intentará iniciar el proceso de nuevo cuando recibe una nueva solicitud y continuará intentar reiniciar el proceso en las posteriores solicitudes entrantes a menos que la aplicación no se puede iniciar **rapidFailsPerMinute** número de veces en el último minuto gradual.</p><p>El valor predeterminado es 120.</p> |
| shutdownTimeLimit | <p>Atributo de entero opcional.</p><p>Duración en segundos para que el módulo esperará a que el ejecutable que se debe apagar normalmente cuando la *app_offline.htm* se detecta que el archivo.</p><p>El valor predeterminado es 10.</p> |
| rapidFailsPerMinute | <p>Atributo de entero opcional.</p><p>Especifica el número de veces especificado por el proceso de **processPath** se permite el bloqueo por minuto. Si se supera este límite, el módulo dejará de iniciar el proceso para el resto del minuto.</p><p>El valor predeterminado es 10.</p> |
| requestTimeout | <p>Atributo timespan opcional.</p><p>Especifica la duración para la que el módulo principal de ASP.NET esperará una respuesta del proceso escucha en % ASPNETCORE_PORT %.</p><p>El valor predeterminado es "00:02:00".</p><p>El `requestTimeout` debe especificarse en minutos enteros, en caso contrario, el valor predeterminado es 2 minutos.</p> |
| stdoutLogEnabled | <p>Atributo Boolean opcional.</p><p>Si es true, **stdout** y **stderr** para el proceso especificado en **processPath** se redirigirán al archivo especificado en **stdoutLogFile**.</p><p>El valor predeterminado es false.</p> |
| stdoutLogFile | <p>Atributo de cadena opcional.</p><p>Especifica la ruta de acceso relativa o absoluta para el que **stdout** y **stderr** desde el proceso especificado en **processPath** se registrarán. Rutas de acceso relativas son relativas a la raíz del sitio. Cualquier ruta de acceso a partir de '.' será relativa a la raíz del sitio y todas las demás rutas de acceso se deben tratar como rutas de acceso absolutas. Las carpetas que se proporcionan en la ruta de acceso deben existir en orden para el módulo crear el archivo de registro. El identificador de proceso, marca de tiempo (*yyyyMdhms*) y la extensión de archivo (*.log*) con el carácter de subrayado delimitadores se agregan al último segmento de la **stdoutLogFile** proporcionado.</p><p>El valor predeterminado es `aspnetcore-stdout`.</p> |
| forwardWindowsAuthToken | true o false</p><p>Si es true, el token se reenviarán al proceso secundario escucha en % ASPNETCORE_PORT % como un encabezado 'MS-ASPNETCORE-WINAUTHTOKEN' por solicitud. Es responsabilidad de dicho proceso para llamar a CloseHandle en este token por solicitud.</p><p>El valor predeterminado es true.</p> |
| disableStartUpErrorPage | true o false</p><p>Si es true, el **502.5 - Error de proceso** página que se van a eliminar, y la página de códigos de 502 estado configurado en su *web.config* tendrá prioridad.</p><p>El valor predeterminado es false.</p> |

### <a name="setting-environment-variables"></a>Establecer variables de entorno

El módulo principal de ASP.NET le permite especificar las variables de entorno del proceso especificado en el `processPath` atributo especificándolas en uno o varios `environmentVariable` elementos secundarios de un `environmentVariables` elemento de la colección en el `aspNetCore` elemento. Las variables de entorno establecidas en esta sección tienen prioridad sobre el sistema las variables de entorno para el proceso.

En el ejemplo siguiente establece dos variables de entorno. `ASPNETCORE_ENVIRONMENT`va a configurar el entorno de la aplicación para `Development`. Un programador puede establecer temporalmente este valor el *web.config* archivo con el fin de forzar la [página de excepción para desarrolladores](xref:fundamentals/error-handling) cargar al depurar una excepción de aplicación. `CONFIG_DIR`es un ejemplo de una variable de entorno definida por el usuario, donde el desarrollador ha escrito el código que va a leer el valor de inicio para formar una ruta de acceso para cargar el archivo de configuración de la aplicación.

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

## <a name="appofflinehtm"></a>app_offline.htm

Si se coloca un archivo con el nombre *app_offline.htm* en la raíz de un directorio de aplicación web, el módulo de núcleo de ASP.NET se intenta correctamente, cierre la aplicación y detener el procesamiento de las solicitudes entrantes. Si la aplicación sigue ejecutándose después de `shutdownTimeLimit` número de segundos, el módulo principal de ASP.NET terminará el proceso de ejecución.

Mientras el *app_offline.htm* archivo está presente, el módulo principal de ASP.NET responderá a las solicitudes mediante el envío de nuevo el contenido de la *app_offline.htm* archivo. Una vez el *app_offline.htm* se quita el archivo, la siguiente solicitud de carga de la aplicación, que, a continuación, responde a las solicitudes.

## <a name="start-up-error-page"></a>Página de error de inicio

Si se produce un error en el módulo principal de ASP.NET iniciar el proceso de back-end o el proceso de back-end comienza pero no puede escuchar en el puerto configurado, verá una página de códigos de estado HTTP 502.5. Para suprimir esta página y volver a la página de códigos de estado de IIS 502 predeterminado, utilice el `disableStartUpErrorPage` atributo. Para obtener más información acerca de cómo configurar mensajes de error personalizados, vea [errores HTTP `<httpErrors>` ](https://docs.microsoft.com/iis/configuration/system.webServer/httpErrors/).

![Página de estado 502](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a>Creación de registro y de redirección

El módulo principal de ASP.NET redirige `stdout` y `stderr` registros en el disco si se establece la `stdoutLogEnabled` y `stdoutLogFile` atributos de la `aspNetCore` elemento. Las carpetas en el `stdoutLogFile` ruta debe existir en orden para el módulo crear el archivo de registro. Una marca de tiempo y extensión de archivo se agregará automáticamente cuando se crea el archivo de registro. Los registros no se giran, salvo que se produzca el reinicio/reciclaje del proceso. Es responsabilidad del proveedor de hospedaje para limitar los registros de consumen el espacio en disco. Mediante el `stdout` registro sólo se recomienda para solucionar problemas de inicio de la aplicación y no para fines de registro de aplicación general.

El nombre de archivo de registro se crea anexando el identificador de proceso (PID), marca de tiempo (*yyyyMdhms*) y la extensión de archivo (*.log*) para el último segmento de la `stdoutLogFile` ruta de acceso (normalmente *stdout* ) delimitados por caracteres de subrayado. Por ejemplo si el `stdoutLogFile` ruta de acceso finaliza con *stdout*, un registro de una aplicación con un PID de 10652 8/10/2017 creada a las 12:05:02 tiene el nombre de archivo *stdout_10652_20178101252.log*.

Este es un ejemplo `aspNetCore` elemento que configura `stdout` registro. El `stdoutLogFile` se muestra en el ejemplo de la ruta de acceso es adecuado para el servicio de aplicaciones de Azure. Una ruta de acceso local o una ruta de acceso de recurso compartido de red es aceptable para el registro local. Confirme que la identidad del usuario AppPool tiene permiso para escribir en la ruta de acceso proporcionada.

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```
Vea [configuración a través de web.config](#configuration-via-webconfig) para obtener un ejemplo de la `aspNetCore` elemento en el *web.config* archivo.

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a>Configuración compartida de módulo de núcleo de ASP.NET con un servicio de IIS

El instalador del módulo de núcleo de ASP.NET se ejecuta con los privilegios de la **SYSTEM** cuenta. Dado que la cuenta de sistema local no tiene permiso para la ruta de acceso de recurso compartido que es utilizado por la configuración de IIS compartido para modificar, el programa de instalación llegará a un error de acceso denegado al intentar configurar los ajustes del módulo de  *applicationHost.config* en el recurso compartido.

La solución no compatible consiste en deshabilitar la configuración de IIS compartido, ejecute el programa de instalación o exportar actualizado *applicationHost.config* al recurso compartido de archivos y vuelva a habilitar la configuración de IIS compartido.

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

Puede buscar *aspnetcore.dll* en el *applicationHost.config* archivo. Para IIS Express, la *applicationHost.config* el archivo no existe de forma predeterminada. El archivo se crea en *{raíz de la aplicación}\.vs\config* cuando se inicia ningún proyecto de aplicación web en la solución de Visual Studio.
