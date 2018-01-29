---
title: Referencia de errores comunes de IIS con ASP.NET Core y de servicio de aplicaciones de Azure
author: guardrex
description: Distinguir los errores comunes al hospedar aplicaciones ASP.NET Core en IIS y el servicio de aplicaciones de Azure.
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 03/13/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/azure-iis-errors-reference
ms.openlocfilehash: 58c5ec6bb2603499332698fd4225e2fe636256e4
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
# <a name="common-errors-reference-for-azure-app-service-and-iis-with-aspnet-core"></a>Referencia de errores comunes de IIS con ASP.NET Core y de servicio de aplicaciones de Azure

Por [Luke Latham](https://github.com/guardrex)

La siguiente no es una lista completa de errores. Si se produce un error no aparece aquí, [abrir un nuevo asunto](https://github.com/aspnet/Docs/issues/new) con instrucciones detalladas para reproducir el error.

## <a name="installer-unable-to-obtain-vc-redistributable"></a>El instalador no puede obtener VC++ Redistributable

* **Excepción del instalador:** 0x80072efd o 0x80072f76 - Error no especificado

* **Excepción del registro del instalador&#8224;:** Error 0x80072efd o 0x80072f76: Error al ejecutar el paquete EXE

  &#8224;El registro se encuentra en C:\Users\\{USUARIO}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log.

Solución del problema:

* Si el sistema no tiene acceso a Internet al instalar el lote de hospedaje del servidor, se produce esta excepción cuando se evita que el instalador obtenga *Microsoft Visual C++ 2015 Redistributable*. Obtener un instalador de la [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840). Si se produce un error en el programa de instalación, es posible que el servidor no reciba el tiempo de ejecución de .NET Core necesaria para hospedar una implementación de framework dependiente (FDD). Si hospeda un FDD, confirme que está instalado el tiempo de ejecución en programas &amp; características. Si es necesario, obtenga un instalador en tiempo de ejecución de [.NET descarga](https://www.microsoft.com/net/download/core). Después de instalar el runtime, reinicie el sistema o IIS al ejecutar **net stop was /y** seguido de **net start w3svc** desde un símbolo del sistema.

## <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a>La actualización del sistema operativo ha quitado el módulo ASP.NET Core de 32 bits

* **Registro de aplicación:** Error al cargar el archivo DLL del módulo **C:\WINDOWS\system32\inetsrv\aspnetcore.dll**. Los datos son el error.

Solución del problema:

* Los archivos que no son de SO del directorio **C:\Windows\SysWOW64\inetsrv** no se conservan durante una actualización del sistema operativo. Si está instalado el módulo de núcleo de ASP.NET anterior a una actualización del sistema operativo y, a continuación, en cualquier grupo de aplicaciones se ejecuta en modo de 32 bits después de actualizar el sistema operativo, donde se produzca este problema. Después de actualizar el sistema operativo, repare el módulo ASP.NET Core. Vea [Instalación del lote de hospedaje .NET Core Windows Server](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle). Seleccione **reparación** cuando se ejecuta el programa de instalación.

## <a name="platform-conflicts-with-rid"></a>Conflictos de plataforma con RID

* **Explorador:** Error HTTP 502.5 - Error en el proceso

* **Registro de aplicación:** aplicación ' MACHINE/WEBROOT/APPHOST / {ENSAMBLADO}' con raíz física ' C:\{ruta}\' no se pudo iniciar el proceso con la línea de comandos ' "C:\\{PATH} {ensamblado}. {} exe | dll} "', ErrorCode = ' 0 x 80004005: ff.

* **Registro de módulo de ASP.NET Core:** una excepción no controlada: System.BadImageFormatException: no se pudo cargar archivo o ensamblado '.dll {ensamblado}'. Se ha intentado cargar un programa con un formato incorrecto.

Solución del problema:

* Confirme que la aplicación se ejecuta localmente en Kestrel. Un error de proceso puede ser el resultado de un problema en la aplicación. Para obtener más información, consulte [solución de problemas](xref:host-and-deploy/iis/troubleshoot).

* Confirme que la `<PlatformTarget>` en el *.csproj* no entra en conflicto con el RID. Por ejemplo, no se especifica un `<PlatformTarget>` de `x86` y publicar con un RID de `win10-x64`, ya sea mediante *dotnet publicar - c - r de versión win10-x64* o estableciendo la `<RuntimeIdentifiers>` en el *.csproj*  a `win10-x64`. El proyecto se publica sin advertencias ni errores, pero se produce un error con las excepciones registradas anteriores en el sistema.

* Si esta excepción se produce para ver una implementación de aplicaciones de Azure al actualizar una aplicación y la implementación de ensamblados más recientes, elimine manualmente todos los archivos de la implementación anterior. Los ensamblados persistentes no compatibles pueden producir una excepción `System.BadImageFormatException` al implementar una aplicación actualizada.

## <a name="uri-endpoint-wrong-or-stopped-website"></a>Punto de conexión de URI incorrecto o sitio web detenido

* **Explorador:** ERR_CONNECTION_REFUSED

* **Registro de aplicación:** Sin entrada

* **Registro del módulo ASP.NET Core:** Archivo de registro no creado

Solución del problema:

* Confirme que se usa el extremo URI correcto para la aplicación. Compruebe los enlaces.

* Confirme que el sitio Web IIS no se encuentra en la *detenido* estado.

## <a name="corewebengine-or-w3svc-server-features-disabled"></a>Características de servidor CoreWebEngine o W3SVC deshabilitadas

* **Excepción de sistema operativo:** Se deben instalar las características de IIS 7.0 CoreWebEngine y W3SVC para usar el módulo ASP.NET Core.

Solución del problema:

* Confirme que la función adecuada y las características están habilitados. Vea [Configuración de IIS](xref:host-and-deploy/iis/index#iis-configuration).

## <a name="incorrect-website-physical-path-or-app-missing"></a>Ruta de acceso física del sitio Web incorrecto o falta de aplicación

* **Explorador:** 403 Prohibido - Acceso denegado **--O BIEN--** 403.14 Prohibido - El servidor web está configurado para no mostrar una lista de los contenidos de este directorio.

* **Registro de aplicación:** Sin entrada

* **Registro del módulo ASP.NET Core:** Archivo de registro no creado

Solución del problema:

* Consulte el sitio Web IIS **configuración básica** y la carpeta de aplicaciones físicas. Confirme que la aplicación se encuentra en la carpeta en el sitio Web IIS **ruta de acceso física**.

## <a name="incorrect-role-module-not-installed-or-incorrect-permissions"></a>Rol incorrecto, módulo no instalado o permisos incorrectos

* **Explorador:** 500.19 Error interno del servidor - No se puede obtener acceso a la página solicitada porque los datos de configuración relacionados de la página no son válidos.

* **Registro de aplicación:** Sin entrada

* **Registro del módulo ASP.NET Core:** Archivo de registro no creado

Solución del problema:

* Confirme que está habilitada la función adecuada. Vea [Configuración de IIS](xref:host-and-deploy/iis/index#iis-configuration).

* Compruebe **Programas &amp; Características** y confirme que el **módulo Microsoft ASP.NET Core** se ha instalado. Si el **módulo Microsoft ASP.NET Core** no aparece en la lista de programas instalados, instálelo. Vea [Instalación del lote de hospedaje .NET Core Windows Server](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle).

* Asegúrese de que el **grupo de aplicaciones** > **modelo de proceso** > **identidad** está establecido en **ApplicationPoolIdentity** o la identidad personalizada tiene los permisos correctos para tener acceso a la carpeta de implementación de la aplicación.

## <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a>Elemento processPath incorrecto, variable PATH que falta, lote de hospedaje no instalado, sistema o IIS no reiniciado, VC++ Redistributable no instalado o infracción de acceso de dotnet.exe

* **Explorador:** Error HTTP 502.5 - Error en el proceso

* **Registro de aplicación:** aplicación ' MACHINE/WEBROOT/APPHOST / {ENSAMBLADO}' con raíz física ' C:\\{PATH}\' no se pudo iniciar el proceso con la línea de comandos ' ".\{ ".exe" ensamblado} ', ErrorCode = ' 0 x 80070002: 0.

* **Registro del módulo ASP.NET Core:** Archivo de registro creado pero vacío

Solución del problema:

* Confirme que la aplicación se ejecuta localmente en Kestrel. Un error de proceso puede ser el resultado de un problema en la aplicación. Para obtener más información, consulte [solución de problemas](xref:host-and-deploy/iis/troubleshoot).

* Compruebe el *processPath* del atributo en el `<aspNetCore>` elemento *web.config* para confirmar que es *dotnet* para una implementación de framework dependiente (FDD) o *. \{ensamblado} .exe* para una implementación independiente (SCD).

* En el caso de una FDD, *dotnet.exe* podría no ser accesible a través del valor PATH. Confirme que *C:\Archivos de programa\dotnet\* existe en el valor PATH del sistema.

* En el caso de una FDD, *dotnet.exe* podría no ser accesible para la identidad del usuario del grupo de aplicaciones. Confirme que la identidad del usuario de AppPool tiene acceso al directorio *C:\Archivos de programa\dotnet*. Confirme que no hay ninguna regla de denegación configurada para la identidad del usuario AppPool en el *Files\dotnet C:\Program* y directorios de la aplicación.

* Se han implementado un FDD y .NET Core instalado sin necesidad de reiniciar IIS. Ejecute **net stop was /y** seguido de **net start w3svc** desde un símbolo del sistema para reiniciar el servidor o IIS.

* Puede haberse implementado un FDD sin necesidad de instalar el runtime de .NET Core en el sistema host. Si no se instaló el runtime de .NET Core, ejecute el **instalador del paquete de hospedaje de .NET Core Windows Server** en el sistema. Vea [Instalación del lote de hospedaje .NET Core Windows Server](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle). Si intenta instalar el runtime de .NET Core en un sistema sin una conexión a Internet, obtener el tiempo de ejecución de [descarga .NET](https://www.microsoft.com/net/download/core) y ejecute el instalador de paquete de hospedaje para instalar el módulo de núcleo de ASP.NET. Para completar la instalación, reinicie el sistema o IIS mediante la ejecución de **net stop was /y** seguido de **net start w3svc** desde un símbolo del sistema.

* Se han implementado un FDD y *Microsoft Visual C++ 2015 Redistributable (x64)* no está instalado en el sistema. Obtener un instalador de la [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).

## <a name="incorrect-arguments-of-aspnetcore-element"></a>Argumentos incorrectos del elemento \<aspNetCore\>

* **Explorador:** Error HTTP 502.5 - Error en el proceso

* **Registro de aplicación:** aplicación ' MACHINE/WEBROOT/APPHOST / {ENSAMBLADO}' con raíz física ' C:\\{PATH}\' no se pudo iniciar el proceso con la línea de comandos ' "dotnet".\{ DLL de ensamblado}', ErrorCode = ' 0 x 80004005: 80008081.

* **Registro de módulo de ASP.NET Core:** la aplicación para ejecutar no existe: ' ruta de acceso\{ensamblado} .dll '

Solución del problema:

* Confirme que la aplicación se ejecuta localmente en Kestrel. Un error de proceso puede ser el resultado de un problema en la aplicación. Para obtener más información, consulte [solución de problemas](xref:host-and-deploy/iis/troubleshoot).

* Examinar el *argumentos* del atributo en el `<aspNetCore>` elemento *web.config* para confirmar que es (a) *.\{ DLL de ensamblado}* para una implementación de framework dependiente (FDD); o (b) no está presente, una cadena vacía (*argumentos = ""*), o una lista de argumentos de la aplicación (*argumentos = "arg1, arg2,..."*) para una implementación independiente (SCD).

## <a name="missing-net-framework-version"></a>Falta la versión de .NET Framework

* **Explorador:** 502.3 Puerta de enlace incorrecta - Error de conexión al intentar enrutar la solicitud.

* **Registro de aplicación:** ErrorCode = Application ' MACHINE/WEBROOT/APPHOST / {ENSAMBLADO}' con raíz física ' C:\\{PATH}\' no se pudo iniciar el proceso con la línea de comandos ' "dotnet".\{ DLL de ensamblado}', ErrorCode = ' 0 x 80004005: 80008081.

* **Registro del módulo ASP.NET Core:** Excepción de falta de método, archivo o ensamblado. El método, el archivo o el ensamblado especificado en la excepción es un método, archivo o ensamblado de .NET Framework.

Solución del problema:

* Instale la versión de .NET Framework que falta en el sistema.

* Para una implementación de framework dependiente (FDD), confirme que el tiempo de ejecución correcto instalado en el sistema. Si el proyecto se actualiza de la versión 1.1 a 2.0, implementado en el sistema de hospedaje, y produce esta excepción, compruebe que el marco de trabajo 2.0 en el sistema host.

## <a name="stopped-application-pool"></a>Grupo de aplicaciones detenido

* **Explorador:** 503 Servicio no disponible

* **Registro de aplicación:** Sin entrada

* **Registro del módulo ASP.NET Core:** Archivo de registro no creado

Solución de problemas

* Confirme que el grupo de aplicaciones no se encuentra en la *detenido* estado.

## <a name="iis-integration-middleware-not-implemented"></a>Middleware de IIS Integration no implementado

* **Explorador:** Error HTTP 502.5 - Error en el proceso

* **Registro de aplicación:** aplicación ' MACHINE/WEBROOT/APPHOST / {ENSAMBLADO}' con raíz física ' C:\\{PATH}\' creó el proceso con la línea de comandos ' "C:\\{PATH}\{ensamblado}. {} exe | dll} "' pero se bloqueó o no la respuesta no o no atendió en el puerto especificado '{PORT}', ErrorCode = '0x800705B4 '.

* **Registro del módulo ASP.NET Core:** Archivo de registro creado que muestra un funcionamiento normal.

Solución de problemas

* Confirme que la aplicación se ejecuta localmente en Kestrel. Un error de proceso puede ser el resultado de un problema en la aplicación. Para obtener más información, consulte [solución de problemas](xref:host-and-deploy/iis/troubleshoot).

* Confirme que ya sea:
  * El middleware de integración de IIS es referencedby al llamar a la `UseIISIntegration` método en la aplicación `WebHostBuilder` (ASP.NET Core 1.x)
  * Los usos de las aplicaciones la `CreateDefaultBuilder` método (ASP.NET Core 2.x).
  
  Vea [Hospedar en ASP.NET Core](xref:fundamentals/hosting) para obtener detalles.

## <a name="sub-application-includes-a-handlers-section"></a>La aplicación secundaria incluye una sección de \<controladores\>

* **Explorador:** Error HTTP 500.19 - Error interno del servidor

* **Registro de aplicación:** Sin entrada

* **Registro de módulo de ASP.NET Core:** archivo de registro se crea y se muestra el funcionamiento normal de la aplicación raíz. Archivo de registro no se creó para la aplicación de sub.

Solución de problemas

* Confirme que el archivo *web.config* de la aplicación secundaria no incluye una sección `<handlers>`.

## <a name="application-configuration-general-issue"></a>Problema general de configuración de aplicación

* **Explorador:** Error HTTP 502.5 - Error en el proceso

* **Registro de aplicación:** aplicación ' MACHINE/WEBROOT/APPHOST / {ENSAMBLADO}' con raíz física ' C:\\{PATH}\' creó el proceso con la línea de comandos ' "C:\\{PATH}\{ensamblado}. {} exe | dll} "' pero se bloqueó o no la respuesta no o no atendió en el puerto especificado '{PORT}', ErrorCode = '0x800705B4 '.

* **Registro del módulo ASP.NET Core:** Archivo de registro creado pero vacío

Solución de problemas

* Esta excepción general indica que el proceso no pudo iniciarse, probablemente debido a un problema de configuración de aplicación. Que hace referencia a [estructura de directorios](xref:host-and-deploy/directory-structure), confirme que la aplicación implementa archivos y carpetas son adecuadas y que están presentes los archivos de configuración de la aplicación y contienen la configuración correcta para la aplicación y el entorno. Para obtener más información, consulte [solución de problemas](xref:host-and-deploy/iis/troubleshoot).
