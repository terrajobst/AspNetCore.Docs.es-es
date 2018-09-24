---
title: Referencia de configuración del módulo ASP.NET Core
author: guardrex
description: Obtenga información sobre cómo configurar el módulo de ASP.NET Core para hospedar aplicaciones ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: bf7a60b67b1ea78bb346e6dd5eeef38b54bfdbe4
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2018
ms.locfileid: "46010954"
---
# <a name="aspnet-core-module-configuration-reference"></a>Referencia de configuración del módulo ASP.NET Core

Por [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT) y [Sourabh Shirhatti](https://twitter.com/sshirhatti)

En este documento se proporcionan instrucciones sobre cómo configurar el módulo ASP.NET Core para hospedar aplicaciones de ASP.NET Core. Puede encontrar una introducción al módulo ASP.NET Core e instrucciones de instalación en el artículo de [introducción al módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).

## <a name="configuration-with-webconfig"></a>Configuración con web.config

El módulo ASP.NET Core se configura con la sección `aspNetCore` del nodo `system.webServer` del archivo *web.config* del sitio.

El siguiente archivo *web.config* se publica para una [implementación dependiente del marco](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) y configura el módulo ASP.NET Core para controlar las solicitudes de sitios:

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

El siguiente archivo *web.config* se publica para una [implementación independiente](/dotnet/articles/core/deploying/#self-contained-deployments-scd):

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

Cuando se implementa una aplicación en [Azure App Service](https://azure.microsoft.com/services/app-service/), la ruta de acceso de `stdoutLogFile` se establece en `\\?\%home%\LogFiles\stdout`. La ruta de acceso guarda los registros de stdout en la carpeta *LogFiles*, que es una ubicación que el servicio crea automáticamente.

Consulte [Configuración de aplicaciones secundarias](xref:host-and-deploy/iis/index#sub-application-configuration) para ver una nota importante relativa a la configuración de archivos *web.config* en aplicaciones secundarias.

### <a name="attributes-of-the-aspnetcore-element"></a>Atributos del elemento aspNetCore

::: moniker range="<= aspnetcore-2.0"

| Atributo | Descripción | Default |
| --------- | ----------- | :-----: |
| `arguments` | <p>Atributo de cadena opcional.</p><p>Argumentos para el archivo ejecutable especificado en **processPath**.</p>| |
| `disableStartUpErrorPage` | true o false</p><p>Si es true, la página **502.5 - Error en el proceso** se suprime, y tiene prioridad la página de código de estado 502 configurada en *web.config*.</p> | `false` |
| `forwardWindowsAuthToken` | true o false</p><p>Si es true, el token se reenvía al proceso secundario que escucha en % ASPNETCORE_PORT % como un encabezado "MS-ASPNETCORE-WINAUTHTOKEN" por solicitud. Es responsabilidad de dicho proceso llamar a CloseHandle en este token por solicitud.</p> | `true` |
| `processPath` | <p>Atributo de cadena necesario.</p><p>Ruta de acceso al archivo ejecutable que inicia un proceso que escucha las solicitudes HTTP. No se admiten rutas de acceso relativas. Si la ruta de acceso comienza con `.`, se considera que es relativa a la raíz del sitio.</p> | |
| `rapidFailsPerMinute` | <p>Atributo integer opcional.</p><p>Especifica el número de veces que el proceso indicado en **processPath** puede bloquearse por minuto. Si se supera este límite, el módulo deja de iniciar el proceso durante lo que resta del minuto.</p> | `10` |
| `requestTimeout` | <p>Atributo timespan opcional.</p><p>Especifica el tiempo que el módulo ASP.NET Core espera una respuesta del proceso que escucha en % ASPNETCORE_PORT %.</p><p>En las versiones del módulo ASP.NET Core que se envían con la versión de ASP.NET Core 2.0 o anterior, `requestTimeout` solo se debe especificar en minutos enteros, si no, adopta el valor predeterminado de 2 minutos.</p> | `00:02:00` |
| `shutdownTimeLimit` | <p>Atributo integer opcional.</p><p>Tiempo en segundos que el módulo espera a que se cierre correctamente el archivo ejecutable cuando se detecta el archivo *app_offline.htm*.</p> | `10` |
| `startupTimeLimit` | <p>Atributo integer opcional.</p><p>Tiempo en segundos que espera el módulo a que el archivo ejecutable inicie u proceso que escucha en el puerto. Si se supera este límite de tiempo, el módulo termina el proceso. El módulo intenta reiniciar el proceso cuando se recibe una nueva solicitud y lo sigue intentando en las sucesivas solicitudes entrantes a no ser que la aplicación no pueda iniciar **rapidFailsPerMinute** un número de veces en el último minuto acumulado.</p> | `120` |
| `stdoutLogEnabled` | <p>Atributo Boolean opcional.</p><p>Si es true, **stdout** y **stderr** en el proceso especificado en **processPath** se redirigen al archivo especificado en **stdoutLogFile**.</p> | `false` |
| `stdoutLogFile` | <p>Atributo de cadena opcional.</p><p>Especifica la ruta de acceso relativa o absoluta para la que se registran **stdout** y **stderr** desde el proceso especificado en **processPath**. Las rutas de acceso relativas son relativas a la raíz del sitio. Cualquier ruta de acceso que se inicia con `.` es relativa a la raíz del sitio y todas las demás rutas de acceso se tratan como absolutas. Las carpetas que se proporcionan en la ruta de acceso deben estar en orden para que el módulo cree el archivo de registro. Mediante delimitadores se agrega una marca de tiempo, un identificador de proceso y una extensión de archivo (*.log*) al último segmento de la ruta de acceso **stdoutLogFile**. Si `.\logs\stdout` se proporciona como valor, se guarda un registro de ejemplo de stdout como *stdout_20180205194132_1934.log* en la carpeta *logs*, cuando se guarda el 5/2/2018 a las 19:41:32 con un identificador de proceso de 1934.</p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

| Atributo | Descripción | Default |
| --------- | ----------- | :-----: |
| `arguments` | <p>Atributo de cadena opcional.</p><p>Argumentos para el archivo ejecutable especificado en **processPath**.</p>| |
| `disableStartUpErrorPage` | true o false</p><p>Si es true, la página **502.5 - Error en el proceso** se suprime, y tiene prioridad la página de código de estado 502 configurada en *web.config*.</p> | `false` |
| `forwardWindowsAuthToken` | true o false</p><p>Si es true, el token se reenvía al proceso secundario que escucha en % ASPNETCORE_PORT % como un encabezado "MS-ASPNETCORE-WINAUTHTOKEN" por solicitud. Es responsabilidad de dicho proceso llamar a CloseHandle en este token por solicitud.</p> | `true` |
| `processPath` | <p>Atributo de cadena necesario.</p><p>Ruta de acceso al archivo ejecutable que inicia un proceso que escucha las solicitudes HTTP. No se admiten rutas de acceso relativas. Si la ruta de acceso comienza con `.`, se considera que es relativa a la raíz del sitio.</p> | |
| `rapidFailsPerMinute` | <p>Atributo integer opcional.</p><p>Especifica el número de veces que el proceso indicado en **processPath** puede bloquearse por minuto. Si se supera este límite, el módulo deja de iniciar el proceso durante lo que resta del minuto.</p> | `10` |
| `requestTimeout` | <p>Atributo timespan opcional.</p><p>Especifica el tiempo que el módulo ASP.NET Core espera una respuesta del proceso que escucha en % ASPNETCORE_PORT %.</p><p>En las versiones del módulo ASP.NET Core que se envían con la versión de ASP.NET Core 2.1 o posterior, el valor `requestTimeout` se especifica en horas, minutos y segundos.</p> | `00:02:00` |
| `shutdownTimeLimit` | <p>Atributo integer opcional.</p><p>Tiempo en segundos que el módulo espera a que se cierre correctamente el archivo ejecutable cuando se detecta el archivo *app_offline.htm*.</p> | `10` |
| `startupTimeLimit` | <p>Atributo integer opcional.</p><p>Tiempo en segundos que espera el módulo a que el archivo ejecutable inicie u proceso que escucha en el puerto. Si se supera este límite de tiempo, el módulo termina el proceso. El módulo intenta reiniciar el proceso cuando se recibe una nueva solicitud y lo sigue intentando en las sucesivas solicitudes entrantes a no ser que la aplicación no pueda iniciar **rapidFailsPerMinute** un número de veces en el último minuto acumulado.</p> | `120` |
| `stdoutLogEnabled` | <p>Atributo Boolean opcional.</p><p>Si es true, **stdout** y **stderr** en el proceso especificado en **processPath** se redirigen al archivo especificado en **stdoutLogFile**.</p> | `false` |
| `stdoutLogFile` | <p>Atributo de cadena opcional.</p><p>Especifica la ruta de acceso relativa o absoluta para la que se registran **stdout** y **stderr** desde el proceso especificado en **processPath**. Las rutas de acceso relativas son relativas a la raíz del sitio. Cualquier ruta de acceso que se inicia con `.` es relativa a la raíz del sitio y todas las demás rutas de acceso se tratan como absolutas. Las carpetas que se proporcionan en la ruta de acceso deben estar en orden para que el módulo cree el archivo de registro. Mediante delimitadores se agrega una marca de tiempo, un identificador de proceso y una extensión de archivo (*.log*) al último segmento de la ruta de acceso **stdoutLogFile**. Si `.\logs\stdout` se proporciona como valor, se guarda un registro de ejemplo de stdout como *stdout_20180205194132_1934.log* en la carpeta *logs*, cuando se guarda el 5/2/2018 a las 19:41:32 con un identificador de proceso de 1934.</p> | `aspnetcore-stdout` |

::: moniker-end

### <a name="setting-environment-variables"></a>Configuración de las variables de entorno

Se pueden especificar variables de entorno para el proceso en el atributo `processPath`. Especifique una variable de entorno con el elemento secundario `environmentVariable` de un elemento de la colección `environmentVariables`. Las variables de entorno establecidas en esta sección tienen prioridad sobre las variables del entorno del sistema.

En el ejemplo siguiente se establecen dos variables de entorno. `ASPNETCORE_ENVIRONMENT` configura el entorno de la aplicación como `Development`. Un desarrollador puede establecer temporalmente este valor en el archivo *web.config* con el fin de forzar a que se cargue la [página de excepciones del desarrollador](xref:fundamentals/error-handling) al depurar una excepción de aplicación. `CONFIG_DIR` es un ejemplo de una variable de entorno definida por el usuario, donde el desarrollador ha escrito código que lee el valor al inicio para formar una ruta de acceso destinada a la carga del archivo de configuración de la aplicación.

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
> Establezca solo la variable de entorno `ASPNETCORE_ENVIRONMENT` en `Development` en servidores de ensayo y pruebas a los que no puedan acceder redes que no son de confianza, como Internet.

## <a name="appofflinehtm"></a>app_offline.htm

Si un archivo con el nombre *app_offline.htm* se detecta en el directorio raíz de una aplicación, el módulo ASP.NET Core intenta cerrar correctamente la aplicación y deja de procesar las solicitudes entrantes. Si la aplicación se sigue ejecutando después del número definido en `shutdownTimeLimit`, el módulo ASP.NET Core termina el proceso en ejecución.

Mientras el archivo *app_offline.htm* existe, el módulo ASP.NET Core responde a solicitudes con la devolución del contenido del archivo *app_offline.htm*. Cuando se quita el archivo *app_offline.htm*, la solicitud siguiente inicia la aplicación.

## <a name="start-up-error-page"></a>Página de errores de inicio

Si el módulo ASP.NET Core no es capaz de iniciar el proceso de back-end o este se inicia pero no puede escuchar en el puerto configurado, aparece una página de código de estado *502.5 Error de proceso*. Para suprimir esta página y volver a la página de código de estado 502 de IIS predeterminada, use el atributo `disableStartUpErrorPage`. Para más información sobre cómo configurar mensajes de error personalizados, consulte [Errores HTTP`<httpErrors>`](/iis/configuration/system.webServer/httpErrors/).

![Página de códigos de estado 502.5 Error de proceso](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a>Creación y redireccionamiento de registros

El módulo ASP.NET Core redirige los resultados de consola stdout y stderr al disco si se establecen los atributos `stdoutLogEnabled` y `stdoutLogFile` del elemento `aspNetCore`. Las carpetas de la ruta de acceso `stdoutLogFile` debe estar en orden para que el módulo cree el archivo de registro. El grupo de aplicaciones debe tener acceso de escritura a la ubicación en la que se escriben los registros (use `IIS AppPool\<app_pool_name>` para proporcionar permiso de escritura).

Los registros no se rotan, a no ser que se produzca un reinicio o reciclaje del proceso. Es responsabilidad del proveedor de servicios de hospedaje limitar el espacio en disco que consumen los registros.

El uso del registro de stdout solo se recomienda para solucionar problemas de inicio de la aplicación. No use el registro de stdout con fines de registro de aplicaciones general. Para el registro rutinario en una aplicación ASP.NET Core, use una biblioteca de registro que limite el tamaño del archivo de registro y realice la rotación de los registros. Para más información, consulte los [proveedores de registro de terceros](xref:fundamentals/logging/index#third-party-logging-providers).

Cuando se crea el archivo de registro, se agregan automáticamente una marca de tiempo y una extensión de archivo. El nombre del archivo de registro se forma mediante la anexión de la marca de tiempo, el identificador de proceso y la extensión de archivo (*.log*) al último segmento de la ruta de acceso `stdoutLogFile` (normalmente *stdout*) delimitados por caracteres de subrayado. Si la ruta de acceso de `stdoutLogFile` finaliza con *stdout*, el registro de una aplicación con un PID de 1934 creado el 5/2/2018 a las 19:42:32 tiene el nombre de archivo *stdout_20180205194132_1934.log*.

El elemento de ejemplo siguiente, `aspNetCore`, configura el registro de stdout para una aplicación hospedada en Azure App Service. Una ruta de acceso local o una ruta de acceso de recurso compartido de red son aceptables para el registro local. Confirme que la identidad del usuario de AppPool tenga permiso para escribir en la ruta de acceso proporcionada.

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

Consulte [Configuración con web.config](#configuration-with-webconfig) para ver un ejemplo del elemento `aspNetCore` en el archivo *web.config*.

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a>La configuración de proxy usa el protocolo HTTP y un token de emparejamiento

El proxy creado entre el módulo ASP.NET Core y Kestrel usa el protocolo HTTP. El uso de HTTP optimiza el rendimiento, ya que el tráfico entre el módulo y Kestrel se realiza en una dirección de bucle invertido fuera de la interfaz de red. No hay ningún riesgo de interceptación del tráfico entre el módulo y Kestrel desde una ubicación fuera del servidor.

Un token de emparejamiento sirve para garantizar que las solicitudes recibidas por Kestrel se redirigieron mediante proxy por IIS y no procedieron de otra fuente. El módulo crea el token de emparejamiento y lo establece en una variable de entorno (`ASPNETCORE_TOKEN`). El token de emparejamiento también se establece en un encabezado (`MSAspNetCoreToken`) en cada solicitud redirigida mediante proxy. El middleware de IIS comprueba cada solicitud recibida para confirmar que el valor del encabezado del token de emparejamiento coincida con el valor de la variable de entorno. Si los valores del token no coinciden, la solicitud se registra y se rechaza. No se puede acceder a la variable de entorno del token de emparejamiento y al tráfico entre el módulo y Kestrel desde una ubicación fuera del servidor. Sin conocer el valor del token de emparejamiento, un atacante no puede enviar solicitudes que omitan la comprobación en el middleware de IIS.

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a>El módulo ASP.NET Core con una configuración compartida de IIS

El instalador del módulo ASP.NET Core se ejecuta con los privilegios de la cuenta **SYSTEM**. Dado que la cuenta local del sistema no tiene permiso de modificación en la ruta de acceso de recurso compartido que se usa en la configuración compartida de IIS, el instalador recibe un error de acceso denegado al intentar configurar los valores del módulo en *applicationHost.config* en el recurso compartido. Al usar una configuración compartida de IIS, siga estos pasos:

1. Deshabilite la configuración compartida de IIS.
1. Ejecute el instalador.
1. Exporte el archivo *applicationHost.config* actualizado al recurso compartido.
1. Vuelva a habilitar la configuración compartida de IIS.

## <a name="module-version-and-hosting-bundle-installer-logs"></a>Versión del módulo y registros del instalador de la agrupación de hospedaje

Para determinar la versión instalada del módulo ASP.NET Core, siga estos pasos:

1. En el sistema de hospedaje, vaya a *%windir%\System32\inetsrv*.
1. Busque el archivo *aspnetcore.dll*.
1. Haga clic con el botón derecho en el archivo y seleccione **Propiedades** en el menú contextual.
1. Seleccione la pestaña **Detalles**. La **versión del archivo** y la **versión del producto** representan la versión instalada del módulo.

Los registros del instalador de la agrupación de hospedaje del módulo se encuentran en *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. El archivo se llama *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.

## <a name="module-schema-and-configuration-file-locations"></a>Ubicaciones del módulo, el esquema y el archivo de configuración

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

Los archivos se pueden encontrar mediante la búsqueda de *aspnetcore.dll* en el archivo *applicationHost.config*. Para IIS Express, el archivo *applicationHost.config* no existe de forma predeterminada. El archivo se crea en *\<application_root>\\.vs\\config* cuando se inicia cualquier proyecto de aplicación web en la solución de Visual Studio.
