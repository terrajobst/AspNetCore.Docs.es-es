---
title: Referencia de errores comunes de Azure App Service e IIS con ASP.NET Core
author: guardrex
description: Distinción de los errores comunes al hospedar aplicaciones ASP.NET Core en Azure App Service e IIS.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-iis-errors-reference
ms.openlocfilehash: 995890a5e6b0cc1d9cebc21486917a7a39587076
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/17/2018
ms.locfileid: "34233344"
---
# <a name="common-errors-reference-for-azure-app-service-and-iis-with-aspnet-core"></a>Referencia de errores comunes de Azure App Service e IIS con ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

La siguiente no es una lista completa de errores. Si se produce un error que aquí no aparece, [abra un nuevo problema](https://github.com/aspnet/Docs/issues/new) con instrucciones detalladas para reproducir el error.

Recopile la siguiente información:

* Comportamiento del explorador
* Entradas de registro de eventos de la aplicación
* Entradas de registro de stdout de módulo ASP.NET Core

Compare la información con los siguientes errores comunes. Si se encuentra una coincidencia, siga los consejos de solución de problemas.

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

## <a name="installer-unable-to-obtain-vc-redistributable"></a>El instalador no puede obtener VC++ Redistributable

* **Excepción del instalador:** 0x80072efd o 0x80072f76 - Error no especificado

* **Excepción del registro del instalador&#8224;:** Error 0x80072efd o 0x80072f76: Error al ejecutar el paquete EXE

  &#8224;El registro se encuentra en C:\Users\\{USUARIO}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log.

Solución del problema:

* Si el sistema no tiene acceso a Internet al instalar la agrupación de hospedaje, se produce esta excepción cuando se evita que el instalador obtenga *Microsoft Visual C++ 2015 Redistributable*. Obtenga un instalador en el [Centro de descarga de Microsoft](https://www.microsoft.com/download/details.aspx?id=53840). Si se produce un error en el instalador, es posible que no reciba el entorno de tiempo de ejecución de .NET Core necesario para hospedar una implementación dependiente del marco (FDD). Si va a hospedar una FDD, confirme que el tiempo de ejecución está instalado en Programas y características. Si es necesario, puede obtener un instalador de tiempo de ejecución en [.NET All Downloads](https://www.microsoft.com/net/download/all) (.NET Todas las descargas). Después de instalar el runtime, reinicie el sistema o IIS al ejecutar **net stop was /y** seguido de **net start w3svc** desde un símbolo del sistema.

## <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a>La actualización del sistema operativo ha quitado el módulo ASP.NET Core de 32 bits

* **Registro de aplicación:** Error al cargar el archivo DLL del módulo **C:\WINDOWS\system32\inetsrv\aspnetcore.dll**. Los datos son el error.

Solución del problema:

* Los archivos que no son de SO del directorio **C:\Windows\SysWOW64\inetsrv** no se conservan durante una actualización del sistema operativo. Si ha instalado el módulo ASP.NET Core antes de una actualización del sistema operativo y luego se ejecuta AppPool en modo de 32 bits después de una actualización del sistema operativo, se produce este problema. Después de actualizar el sistema operativo, repare el módulo ASP.NET Core. Consulte [Instalación de la agrupación de hospedaje de .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle). Seleccione **Reparar** cuando se ejecute el instalador.

## <a name="platform-conflicts-with-rid"></a>Conflictos de plataforma con RID

* **Explorador:** Error HTTP 502.5 - Error en el proceso

* **Registro de aplicación:** La aplicación "MACHINE/WEBROOT/APPHOST/{ASSEMBLY}" con la raíz física "C:\{PATH}\' no pudo iniciar el proceso con la línea de comandos '"C:\\{PATH}{assembly}.{exe|dll}" ", ErrorCode = '0x80004005 : ff.

* **Registro del módulo ASP.NET Core:** Excepción no controlada: System.BadImageFormatException: No se pudo cargar el archivo o ensamblado "{assembly}.dll". Se ha intentado cargar un programa con un formato incorrecto.

Solución del problema:

* Confirme que la aplicación se ejecuta localmente en Kestrel. Un error de proceso puede ser el resultado de un problema en la aplicación. Para más información, consulte [Solución de problemas](xref:host-and-deploy/iis/troubleshoot).

* Confirme que `<PlatformTarget>` en *.csproj* no entra en conflicto con el RID. Por ejemplo, no especifique un elemento `<PlatformTarget>` de `x86` ni publique con un RID de `win10-x64`, ya sea mediante *dotnet publish -c Release -r win10-x64* o al establecer el elemento `<RuntimeIdentifiers>` de *.csproj*  en `win10-x64`. El proyecto se publica sin advertencias ni errores, pero se produce un error con las excepciones registradas anteriores en el sistema.

* Si esta excepción se produce en una implementación de Azure Apps al actualizar una aplicación e implementar ensamblados más recientes, elimine manualmente todos los archivos de la implementación anterior. Los ensamblados persistentes no compatibles pueden producir una excepción `System.BadImageFormatException` al implementar una aplicación actualizada.

## <a name="uri-endpoint-wrong-or-stopped-website"></a>Punto de conexión de URI incorrecto o sitio web detenido

* **Explorador:** ERR_CONNECTION_REFUSED

* **Registro de aplicación:** Sin entrada

* **Registro del módulo ASP.NET Core:** Archivo de registro no creado

Solución del problema:

* Confirme que se usa el punto de conexión de URI correcto para la aplicación. Compruebe los enlaces.

* Confirme que el sitio web de IIS no está en estado *Detenido*.

## <a name="corewebengine-or-w3svc-server-features-disabled"></a>Características de servidor CoreWebEngine o W3SVC deshabilitadas

* **Excepción de sistema operativo:** Se deben instalar las características de IIS 7.0 CoreWebEngine y W3SVC para usar el módulo ASP.NET Core.

Solución del problema:

* Confirme que están habilitados el rol y las características correctos. Vea [Configuración de IIS](xref:host-and-deploy/iis/index#iis-configuration).

## <a name="incorrect-website-physical-path-or-app-missing"></a>Ruta de acceso física de sitio web incorrecta o aplicación que falta

* **Explorador:** 403 Prohibido - Acceso denegado **--O BIEN--** 403.14 Prohibido - El servidor web está configurado para no mostrar una lista de los contenidos de este directorio.

* **Registro de aplicación:** Sin entrada

* **Registro del módulo ASP.NET Core:** Archivo de registro no creado

Solución del problema:

* Consulte la opción **Configuración básica** del sitio web de IIS y la carpeta de la aplicación física. Confirme que la aplicación está en la carpeta en la **ruta de acceso física** del sitio web de IIS.

## <a name="incorrect-role-module-not-installed-or-incorrect-permissions"></a>Rol incorrecto, módulo no instalado o permisos incorrectos

* **Explorador:** 500.19 Error interno del servidor - No se puede obtener acceso a la página solicitada porque los datos de configuración relacionados de la página no son válidos.

* **Registro de aplicación:** Sin entrada

* **Registro del módulo ASP.NET Core:** Archivo de registro no creado

Solución del problema:

* Confirme que está habilitado el rol adecuado. Vea [Configuración de IIS](xref:host-and-deploy/iis/index#iis-configuration).

* Compruebe **Programas &amp; Características** y confirme que el **módulo Microsoft ASP.NET Core** se ha instalado. Si el **módulo Microsoft ASP.NET Core** no aparece en la lista de programas instalados, instálelo. Consulte [Instalación de la agrupación de hospedaje de .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).

* Asegúrese de que **Grupo de aplicaciones** > **Modelo de proceso** > **Identidad** esté establecido en **ApplicationPoolIdentity** o que la identidad personalizada tenga los permisos correctos para acceder a la carpeta de implementación de la aplicación.

## <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a>Elemento processPath incorrecto, falta la variable PATH, agrupación de hospedaje no instalada, sistema o IIS no reiniciado, VC++ Redistributable no instalado o infracción de acceso de dotnet.exe

* **Explorador:** Error HTTP 502.5 - Error en el proceso

* **Registro de aplicación:** La aplicación "MACHINE/WEBROOT/APPHOST/{ASSEMBLY}" con la raíz física "C:\\{PATH}\' no pudo iniciar el proceso con la línea de comandos '".\{assembly}.exe" ", ErrorCode = '0x80070002 : 0.

* **Registro del módulo ASP.NET Core:** Archivo de registro creado pero vacío

Solución del problema:

* Confirme que la aplicación se ejecuta localmente en Kestrel. Un error de proceso puede ser el resultado de un problema en la aplicación. Para más información, consulte [Solución de problemas](xref:host-and-deploy/iis/troubleshoot).

* Compruebe el atributo *processPath* del elemento `<aspNetCore>` de *web.config* para confirmar que es *dotnet* para una implementación dependiente del marco (FDD) o *.\{assembly}.exe* para una implementación independiente (SCD).

* En el caso de una FDD, *dotnet.exe* podría no ser accesible a través del valor PATH. Confirme que *C:\Archivos de programa\dotnet\* existe en el valor PATH del sistema.

* En el caso de una FDD, *dotnet.exe* podría no ser accesible para la identidad del usuario del grupo de aplicaciones. Confirme que la identidad del usuario de AppPool tiene acceso al directorio *C:\Archivos de programa\dotnet*. Confirme que no haya ninguna regla de denegación configurada para la identidad del usuario de AppPool en los directorios *C:\Archivos de programa\dotnet* y de la aplicación.

* Puede que se haya implementado una FDD y que se instalara .NET Core sin reiniciar IIS. Ejecute **net stop was /y** seguido de **net start w3svc** desde un símbolo del sistema para reiniciar el servidor o IIS.

* Puede que se haya implementado una FDD sin instalar el entorno de tiempo de ejecución de .NET Core en el sistema de hospedaje. Si no se ha instalado el entorno de tiempo de ejecución de .NET Core, ejecute el **instalador de la agrupación de hospedaje de .NET Core** en el sistema. Consulte [Instalación de la agrupación de hospedaje de .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle). Si está intentando instalar el entorno de tiempo de ejecución de .NET Core en un sistema sin conexión a Internet, puede obtenerlo en [.NET All Downloads](https://www.microsoft.com/net/download/all) (.NET Todas las descargas) y ejecute el instalador de la agrupación de hospedaje para instalar el módulo ASP.NET Core. Para completar la instalación, reinicie el sistema o IIS mediante la ejecución de **net stop was /y** seguido de **net start w3svc** desde un símbolo del sistema.

* Puede que se haya implementado una FDD y que el paquete *Microsoft Visual C++ 2015 Redistributable (x64)* no esté instalado en el sistema. Obtenga un instalador en el [Centro de descarga de Microsoft](https://www.microsoft.com/download/details.aspx?id=53840).

## <a name="incorrect-arguments-of-aspnetcore-element"></a>Argumentos incorrectos del elemento \<aspNetCore\>

* **Explorador:** Error HTTP 502.5 - Error en el proceso

* **Registro de aplicación:** La aplicación "MACHINE/WEBROOT/APPHOST/MY_APPLICATION" con la raíz física "C:\\{PATH}\' no pudo iniciar el proceso con la línea de comandos '"dotnet" .\{assembly}.dll', ErrorCode = '0x80004005 : 80008081.

* **Registro del módulo ASP.NET Core:** La aplicación que se va a ejecutar no existe: "PATH\{assembly}.dll"

Solución del problema:

* Confirme que la aplicación se ejecuta localmente en Kestrel. Un error de proceso puede ser el resultado de un problema en la aplicación. Para más información, consulte [Solución de problemas](xref:host-and-deploy/iis/troubleshoot).

* Examine el atributo *arguments* del elemento `<aspNetCore>` en *web.config* para confirmar que (a) es *.\{assembly}.dll* para una implementación dependiente del marco (FDD); o (b) no está presente, es una cadena vacía (*arguments=""*) o una lista de argumentos de la aplicación (*arguments="arg1, arg2, ..."*) para una implementación independiente (SCD).

## <a name="missing-net-framework-version"></a>Falta la versión de .NET Framework

* **Explorador:** 502.3 Puerta de enlace incorrecta - Error de conexión al intentar enrutar la solicitud.

* **Registro de aplicación:** ErrorCode = La aplicación "MACHINE/WEBROOT/APPHOST/{ASSEMBLY}" con la raíz física "C:\\{PATH}\' no pudo iniciar el proceso con la línea de comandos '"dotnet" .\{assembly}.dll", ErrorCode = '0x80004005 : 80008081.

* **Registro del módulo ASP.NET Core:** Excepción de falta de método, archivo o ensamblado. El método, el archivo o el ensamblado especificado en la excepción es un método, archivo o ensamblado de .NET Framework.

Solución del problema:

* Instale la versión de .NET Framework que falta en el sistema.

* En el caso de una implementación dependiente del marco (FDD), confirme que tiene instalado el entorno de tiempo de ejecución correcto en el sistema. Si el proyecto se actualiza de la versión 1.1 a la 2.0, se implementa en el sistema de hospedaje, y se produce esta excepción, compruebe que el marco 2.0 está en el sistema de hospedaje.

## <a name="stopped-application-pool"></a>Grupo de aplicaciones detenido

* **Explorador:** 503 Servicio no disponible

* **Registro de aplicación:** Sin entrada

* **Registro del módulo ASP.NET Core:** Archivo de registro no creado

Solución de problemas

* Confirme que el grupo de aplicaciones no está en estado *Detenido*.

## <a name="iis-integration-middleware-not-implemented"></a>Middleware de IIS Integration no implementado

* **Explorador:** Error HTTP 502.5 - Error en el proceso

* **Registro de aplicación:** La aplicación "MACHINE/WEBROOT/APPHOST/{ASSEMBLY}" con la raíz física "C:\\{PATH}\' creó el proceso con la línea de comandos '"C:\\{PATH}\{assembly}.{exe|dll}" ", pero se ha bloqueado, no ha respondido o no ha escuchado en el puerto especificado "{PUERTO}", ErrorCode = "0x800705b4"

* **Registro del módulo ASP.NET Core:** Archivo de registro creado que muestra un funcionamiento normal.

Solución de problemas

* Confirme que la aplicación se ejecuta localmente en Kestrel. Un error de proceso puede ser el resultado de un problema en la aplicación. Para más información, consulte [Solución de problemas](xref:host-and-deploy/iis/troubleshoot).

* Confirme lo siguiente:
  * Se hace referencia al Middleware de IIS Integration mediante la llamada al método `UseIISIntegration` en el elemento `WebHostBuilder` de la aplicación (ASP.NET Core 1.x)
  * La aplicación usa el método `CreateDefaultBuilder` (ASP.NET Core 2.x).
  
  Para más información, consulte [Hospedaje en ASP.NET Core](xref:fundamentals/host/index).

## <a name="sub-application-includes-a-handlers-section"></a>La aplicación secundaria incluye una sección de \<controladores\>

* **Explorador:** Error HTTP 500.19 - Error interno del servidor

* **Registro de aplicación:** Sin entrada

* **Registro del módulo ASP.NET Core:** archivo de registro creado que muestra un funcionamiento normal de la aplicación raíz. Archivo de registro no creado para la aplicación secundaria.

Solución de problemas

* Confirme que el archivo *web.config* de la aplicación secundaria no incluye una sección `<handlers>`.

## <a name="stdout-log-path-incorrect"></a>Ruta de acceso incorrecta al registro de stdout

* **Explorador:** la aplicación responde normalmente.

* **Registro de aplicación:** Advertencia: No se pudo crear stdoutLogFile \\?\C:\_apps\app_folder\bin\Release\netcoreapp2.0\win10-x64\publish\logs\path_doesnt_exist\stdout_8748_201831835937.log, ErrorCode = -2147024893.

* **Registro del módulo ASP.NET Core:** Archivo de registro no creado

Solución de problemas

* La ruta de acceso `stdoutLogFile` especificada en el elemento `<aspNetCore>` de *web.config* no existe. Para más información, consulte la sección [Creación y redireccionamiento de registros](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) del tema de referencia de configuración del módulo ASP.NET Core.

## <a name="application-configuration-general-issue"></a>Problema general de configuración de aplicación

* **Explorador:** Error HTTP 502.5 - Error en el proceso

* **Registro de aplicación:** La aplicación "MACHINE/WEBROOT/APPHOST/{ASSEMBLY}" con la raíz física "C:\\{PATH}\' creó el proceso con la línea de comandos '"C:\\{PATH}\{assembly}.{exe|dll}" ", pero se ha bloqueado, no ha respondido o no ha escuchado en el puerto especificado "{PUERTO}", ErrorCode = "0x800705b4"

* **Registro del módulo ASP.NET Core:** Archivo de registro creado pero vacío

Solución de problemas

* Esta excepción general indica que el proceso no se ha iniciado, probablemente debido a un problema de configuración de la aplicación. Consulte [Estructura de directorios](xref:host-and-deploy/directory-structure) para confirmar que los archivos y las carpetas implementados de la aplicación son adecuados y que los archivos de configuración de la aplicación están presentes y contienen la configuración correcta para la aplicación y el entorno. Para más información, consulte [Solución de problemas](xref:host-and-deploy/iis/troubleshoot).
