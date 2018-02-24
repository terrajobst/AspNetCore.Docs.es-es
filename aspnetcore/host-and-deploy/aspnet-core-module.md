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
# <a name="aspnet-core-module-configuration-reference"></a><span data-ttu-id="76ee8-103">Referencia de configuración del módulo principal ASP.NET</span><span class="sxs-lookup"><span data-stu-id="76ee8-103">ASP.NET Core Module configuration reference</span></span>

<span data-ttu-id="76ee8-104">Por [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), y [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="76ee8-104">By [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="76ee8-105">Este documento proporciona instrucciones sobre cómo configurar el módulo principal de ASP.NET para hospedar aplicaciones ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="76ee8-105">This document provides instructions on how to configure the ASP.NET Core Module for hosting ASP.NET Core apps.</span></span> <span data-ttu-id="76ee8-106">Para obtener una introducción para el módulo principal de ASP.NET y las instrucciones de instalación, consulte el [información general del módulo de ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="76ee8-106">For an introduction to the ASP.NET Core Module and installation instructions, see the [ASP.NET Core Module overview](xref:fundamentals/servers/aspnet-core-module).</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="76ee8-107">Configuración de web.config</span><span class="sxs-lookup"><span data-stu-id="76ee8-107">Configuration with web.config</span></span>

<span data-ttu-id="76ee8-108">El módulo de núcleo de ASP.NET está configurado con el `aspNetCore` sección de la `system.webServer` nodo en el sitio *web.config* archivo.</span><span class="sxs-lookup"><span data-stu-id="76ee8-108">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="76ee8-109">El siguiente *web.config* archivo se publica para un [framework dependiente implementación](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) y configura el módulo principal de ASP.NET para controlar las solicitudes de sitios:</span><span class="sxs-lookup"><span data-stu-id="76ee8-109">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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

<span data-ttu-id="76ee8-110">El siguiente *web.config* se publica para un [implementación independiente](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="76ee8-110">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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

<span data-ttu-id="76ee8-111">Cuando se implementa una aplicación en [servicio de aplicaciones de Azure](https://azure.microsoft.com/services/app-service/), `stdoutLogFile` ruta de acceso se establece en `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="76ee8-111">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="76ee8-112">La ruta de acceso guarda los registros de stdout para la *LogFiles* carpeta, que es una ubicación automáticamente creados por el servicio.</span><span class="sxs-lookup"><span data-stu-id="76ee8-112">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="76ee8-113">Vea [configuración de la Sub-aplicación](xref:host-and-deploy/iis/index#sub-application-configuration) para una nota importante relacionada con la configuración de *web.config* archivos en las subcarpetas de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="76ee8-113">See [Sub-application configuration](xref:host-and-deploy/iis/index#sub-application-configuration) for an important note pertaining to the configuration of *web.config* files in sub-apps.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="76ee8-114">Atributos del elemento aspNetCore</span><span class="sxs-lookup"><span data-stu-id="76ee8-114">Attributes of the aspNetCore element</span></span>

| <span data-ttu-id="76ee8-115">Atributo</span><span class="sxs-lookup"><span data-stu-id="76ee8-115">Attribute</span></span> | <span data-ttu-id="76ee8-116">Descripción</span><span class="sxs-lookup"><span data-stu-id="76ee8-116">Description</span></span> | <span data-ttu-id="76ee8-117">Default</span><span class="sxs-lookup"><span data-stu-id="76ee8-117">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="76ee8-118">Atributo de cadena opcional.</span><span class="sxs-lookup"><span data-stu-id="76ee8-118">Optional string attribute.</span></span></p><p><span data-ttu-id="76ee8-119">Argumentos para el ejecutable especificado en **processPath**.</span><span class="sxs-lookup"><span data-stu-id="76ee8-119">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <span data-ttu-id="76ee8-120">true o false</span><span class="sxs-lookup"><span data-stu-id="76ee8-120">true or false.</span></span></p><p><span data-ttu-id="76ee8-121">Si es true, el **502.5 - Error de proceso** se suprime la página y la página de códigos de 502 estado configurado en el *web.config* tiene prioridad.</span><span class="sxs-lookup"><span data-stu-id="76ee8-121">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <span data-ttu-id="76ee8-122">true o false</span><span class="sxs-lookup"><span data-stu-id="76ee8-122">true or false.</span></span></p><p><span data-ttu-id="76ee8-123">Si es true, el token se reenvía al proceso secundario escucha en % ASPNETCORE_PORT % como un encabezado 'MS-ASPNETCORE-WINAUTHTOKEN' por solicitud.</span><span class="sxs-lookup"><span data-stu-id="76ee8-123">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="76ee8-124">Es responsabilidad de dicho proceso para llamar a CloseHandle en este token por solicitud.</span><span class="sxs-lookup"><span data-stu-id="76ee8-124">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processPath` | <p><span data-ttu-id="76ee8-125">Atributo de cadena necesario.</span><span class="sxs-lookup"><span data-stu-id="76ee8-125">Required string attribute.</span></span></p><p><span data-ttu-id="76ee8-126">Ruta de acceso al archivo ejecutable que se inicia un proceso de escucha las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="76ee8-126">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="76ee8-127">Se admiten rutas de acceso relativas.</span><span class="sxs-lookup"><span data-stu-id="76ee8-127">Relative paths are supported.</span></span> <span data-ttu-id="76ee8-128">Si la ruta de acceso comienza con `.`, la ruta de acceso se considera que son relativas a la raíz del sitio.</span><span class="sxs-lookup"><span data-stu-id="76ee8-128">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="76ee8-129">Atributo de entero opcional.</span><span class="sxs-lookup"><span data-stu-id="76ee8-129">Optional integer attribute.</span></span></p><p><span data-ttu-id="76ee8-130">Especifica el número de veces especificado por el proceso de **processPath** se permite el bloqueo por minuto.</span><span class="sxs-lookup"><span data-stu-id="76ee8-130">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="76ee8-131">Si se supera este límite, el módulo deja de iniciar el proceso durante el resto del minuto.</span><span class="sxs-lookup"><span data-stu-id="76ee8-131">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | `10` |
| `requestTimeout` | <p><span data-ttu-id="76ee8-132">Atributo timespan opcional.</span><span class="sxs-lookup"><span data-stu-id="76ee8-132">Optional timespan attribute.</span></span></p><p><span data-ttu-id="76ee8-133">Especifica la duración para la que el módulo principal de ASP.NET espera una respuesta desde el proceso de escucha en % ASPNETCORE_PORT %.</span><span class="sxs-lookup"><span data-stu-id="76ee8-133">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="76ee8-134">El `requestTimeout` debe especificarse en minutos enteros, en caso contrario, el valor predeterminado es 2 minutos.</span><span class="sxs-lookup"><span data-stu-id="76ee8-134">The `requestTimeout` must be specified in whole minutes only, otherwise it defaults to 2 minutes.</span></span></p> | `00:02:00` |
| `shutdownTimeLimit` | <p><span data-ttu-id="76ee8-135">Atributo de entero opcional.</span><span class="sxs-lookup"><span data-stu-id="76ee8-135">Optional integer attribute.</span></span></p><p><span data-ttu-id="76ee8-136">Duración en segundos que espera a que el módulo de archivo ejecutable que desea apagar normalmente cuando la *app_offline.htm* se detecta que el archivo.</span><span class="sxs-lookup"><span data-stu-id="76ee8-136">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | `10` |
| `startupTimeLimit` | <p><span data-ttu-id="76ee8-137">Atributo de entero opcional.</span><span class="sxs-lookup"><span data-stu-id="76ee8-137">Optional integer attribute.</span></span></p><p><span data-ttu-id="76ee8-138">Duración en segundos que espera a que el módulo para que el ejecutable iniciar un proceso escuchando en el puerto.</span><span class="sxs-lookup"><span data-stu-id="76ee8-138">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="76ee8-139">Si se supera este límite de tiempo, el módulo elimina el proceso.</span><span class="sxs-lookup"><span data-stu-id="76ee8-139">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="76ee8-140">El módulo intenta reiniciar el proceso cuando se recibe una solicitud nueva y sigue intentando reiniciar el proceso en las posteriores solicitudes entrantes a menos que la aplicación no se puede iniciar **rapidFailsPerMinute** número de veces en los últimos minuto gradual.</span><span class="sxs-lookup"><span data-stu-id="76ee8-140">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p> | `120` |
| `stdoutLogEnabled` | <p><span data-ttu-id="76ee8-141">Atributo Boolean opcional.</span><span class="sxs-lookup"><span data-stu-id="76ee8-141">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="76ee8-142">Si es true, **stdout** y **stderr** para el proceso especificado en **processPath** se redirigen al archivo especificado en **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="76ee8-142">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="76ee8-143">Atributo de cadena opcional.</span><span class="sxs-lookup"><span data-stu-id="76ee8-143">Optional string attribute.</span></span></p><p><span data-ttu-id="76ee8-144">Especifica la ruta de acceso relativa o absoluta para el que **stdout** y **stderr** desde el proceso especificado en **processPath** se registran.</span><span class="sxs-lookup"><span data-stu-id="76ee8-144">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="76ee8-145">Rutas de acceso relativas son relativas a la raíz del sitio.</span><span class="sxs-lookup"><span data-stu-id="76ee8-145">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="76ee8-146">Cualquier ruta de acceso a partir de `.` son relativa al sitio raíz y todas las demás rutas de acceso se tratan como rutas de acceso absolutas.</span><span class="sxs-lookup"><span data-stu-id="76ee8-146">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="76ee8-147">Las carpetas que se proporcionan en la ruta de acceso deben existir en orden para el módulo crear el archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="76ee8-147">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="76ee8-148">Usar delimitadores de carácter de subrayado, una marca de tiempo, Id. de proceso y extensión de archivo (*.log*) se agregan al último segmento de la **stdoutLogFile** ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="76ee8-148">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="76ee8-149">Si `.\logs\stdout` se proporciona como un valor, se guarda un registro de ejemplo stdout como *stdout_20180205194132_1934.log* en el *registros* carpeta, cuando se guardan en 2/5/2018 a 19:41:32 con un identificador de proceso de 1934.</span><span class="sxs-lookup"><span data-stu-id="76ee8-149">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

### <a name="setting-environment-variables"></a><span data-ttu-id="76ee8-150">Establecer variables de entorno</span><span class="sxs-lookup"><span data-stu-id="76ee8-150">Setting environment variables</span></span>

<span data-ttu-id="76ee8-151">Se pueden especificar variables de entorno para el proceso en el `processPath` atributo.</span><span class="sxs-lookup"><span data-stu-id="76ee8-151">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="76ee8-152">Especifique una variable de entorno con el `environmentVariable` elemento secundario de un `environmentVariables` elemento de la colección.</span><span class="sxs-lookup"><span data-stu-id="76ee8-152">Specify an environment variable with the `environmentVariable` child element of an `environmentVariables` collection element.</span></span> <span data-ttu-id="76ee8-153">Las variables de entorno establecidas en esta sección tienen prioridad sobre el sistema las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="76ee8-153">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="76ee8-154">En el ejemplo siguiente se establece dos variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="76ee8-154">The following example sets two environment variables.</span></span> <span data-ttu-id="76ee8-155">`ASPNETCORE_ENVIRONMENT` configura el entorno de la aplicación para `Development`.</span><span class="sxs-lookup"><span data-stu-id="76ee8-155">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="76ee8-156">Un programador puede establecer temporalmente este valor el *web.config* archivo con el fin de forzar la [Developer excepción página](xref:fundamentals/error-handling) cargar al depurar una excepción de aplicación.</span><span class="sxs-lookup"><span data-stu-id="76ee8-156">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="76ee8-157">`CONFIG_DIR` es un ejemplo de una variable de entorno definida por el usuario, donde el desarrollador ha escrito código que lee el valor de inicio para formar una ruta de acceso para cargar el archivo de configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="76ee8-157">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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
> <span data-ttu-id="76ee8-158">Solo establecer la `ASPNETCORE_ENVIRONMENT` envirnonment variable `Development` en probar y ensayar los servidores que no son accesibles a redes de confianza, como Internet.</span><span class="sxs-lookup"><span data-stu-id="76ee8-158">Only set the `ASPNETCORE_ENVIRONMENT` envirnonment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="appofflinehtm"></a><span data-ttu-id="76ee8-159">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="76ee8-159">app_offline.htm</span></span>

<span data-ttu-id="76ee8-160">Si un archivo con el nombre *app_offline.htm* se detecta en el directorio raíz de una aplicación, el módulo principal de ASP.NET intenta correctamente, cierre la aplicación y detener el procesamiento de las solicitudes entrantes.</span><span class="sxs-lookup"><span data-stu-id="76ee8-160">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="76ee8-161">Si la aplicación se está ejecutando después del número de segundos que se definen en `shutdownTimeLimit`, el módulo principal de ASP.NET elimina el proceso en ejecución.</span><span class="sxs-lookup"><span data-stu-id="76ee8-161">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="76ee8-162">Mientras el *app_offline.htm* archivo está presente, el módulo principal de ASP.NET responde a solicitudes devolviendo el contenido de la *app_offline.htm* archivo.</span><span class="sxs-lookup"><span data-stu-id="76ee8-162">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="76ee8-163">Cuando el *app_offline.htm* se quita el archivo, la solicitud siguiente inicia la aplicación.</span><span class="sxs-lookup"><span data-stu-id="76ee8-163">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

## <a name="start-up-error-page"></a><span data-ttu-id="76ee8-164">Página de error de inicio</span><span class="sxs-lookup"><span data-stu-id="76ee8-164">Start-up error page</span></span>

<span data-ttu-id="76ee8-165">Si se produce un error en el módulo principal de ASP.NET iniciar el proceso de back-end o el proceso de back-end comienza pero no puede escuchar en el puerto configurado, un *502.5 Error de un proceso* aparece la página de códigos de estado.</span><span class="sxs-lookup"><span data-stu-id="76ee8-165">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 Process Failure* status code page appears.</span></span> <span data-ttu-id="76ee8-166">Para suprimir esta página y volver a la página de códigos de estado de IIS 502 predeterminado, utilice el `disableStartUpErrorPage` atributo.</span><span class="sxs-lookup"><span data-stu-id="76ee8-166">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="76ee8-167">Para obtener más información acerca de cómo configurar mensajes de error personalizados, vea [errores HTTP `<httpErrors>` ](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="76ee8-167">For more information on configuring custom error messages, see [HTTP Errors `<httpErrors>`](/iis/configuration/system.webServer/httpErrors/).</span></span>

![502.5 página de códigos de estado de error de proceso de](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a><span data-ttu-id="76ee8-169">Creación de registro y de redirección</span><span class="sxs-lookup"><span data-stu-id="76ee8-169">Log creation and redirection</span></span>

<span data-ttu-id="76ee8-170">El módulo principal de ASP.NET redirige `stdout` y `stderr` registros en el disco si la `stdoutLogEnabled` y `stdoutLogFile` atributos de la `aspNetCore` se establecen el elemento.</span><span class="sxs-lookup"><span data-stu-id="76ee8-170">The ASP.NET Core Module redirects `stdout` and `stderr` logs to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="76ee8-171">Las carpetas en el `stdoutLogFile` ruta debe existir en orden para el módulo crear el archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="76ee8-171">Any folders in the `stdoutLogFile` path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="76ee8-172">Una marca de tiempo y extensión de archivo se agregan automáticamente cuando se crea el archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="76ee8-172">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="76ee8-173">Los registros no estén girado, salvo que se produzca el reinicio/reciclaje del proceso.</span><span class="sxs-lookup"><span data-stu-id="76ee8-173">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="76ee8-174">Es responsabilidad del proveedor de hospedaje para limitar los registros de consumen el espacio en disco.</span><span class="sxs-lookup"><span data-stu-id="76ee8-174">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span> <span data-ttu-id="76ee8-175">Mediante el `stdout` registro sólo se recomienda para solucionar problemas de inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="76ee8-175">Using the `stdout` log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="76ee8-176">No use el registro de stdout para fines de registro de aplicación general.</span><span class="sxs-lookup"><span data-stu-id="76ee8-176">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="76ee8-177">Para registrar la rutina en una aplicación de ASP.NET Core, utiliza una biblioteca de registro que limita el tamaño del archivo de registro y gira registros.</span><span class="sxs-lookup"><span data-stu-id="76ee8-177">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="76ee8-178">Para obtener más información, consulte [proveedores de registro de aplicaciones de terceros](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="76ee8-178">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="76ee8-179">El nombre de archivo de registro se crea anexando la marca de tiempo, el Id. de proceso y la extensión de archivo (*.log*) para el último segmento de la `stdoutLogFile` ruta de acceso (normalmente *stdout*) delimitados por caracteres de subrayado.</span><span class="sxs-lookup"><span data-stu-id="76ee8-179">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="76ee8-180">Si el `stdoutLogFile` ruta de acceso finaliza con *stdout*, un registro de una aplicación con un PID de 1934 creado en 2/5/2018 a 19:42:32 tiene el nombre de archivo *stdout_20180205194132_1934.log*.</span><span class="sxs-lookup"><span data-stu-id="76ee8-180">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

<span data-ttu-id="76ee8-181">El ejemplo siguiente `aspNetCore` elemento configura `stdout` el registro para una aplicación hospedada en el servicio de aplicación de Azure.</span><span class="sxs-lookup"><span data-stu-id="76ee8-181">The following sample `aspNetCore` element configures `stdout` logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="76ee8-182">Una ruta de acceso local o una ruta de acceso de recurso compartido de red es aceptable para el registro local.</span><span class="sxs-lookup"><span data-stu-id="76ee8-182">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="76ee8-183">Confirme que la identidad del usuario AppPool tiene permiso para escribir en la ruta de acceso proporcionada.</span><span class="sxs-lookup"><span data-stu-id="76ee8-183">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

<span data-ttu-id="76ee8-184">Vea [configuración con web.config](#configuration-with-webconfig) para obtener un ejemplo de la `aspNetCore` elemento en el *web.config* archivo.</span><span class="sxs-lookup"><span data-stu-id="76ee8-184">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="76ee8-185">Configuración compartida de módulo de núcleo de ASP.NET con un servicio de IIS</span><span class="sxs-lookup"><span data-stu-id="76ee8-185">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="76ee8-186">El instalador del módulo de núcleo de ASP.NET se ejecuta con los privilegios de la **SYSTEM** cuenta.</span><span class="sxs-lookup"><span data-stu-id="76ee8-186">The ASP.NET Core Module installer runs with the privileges of the **SYSTEM** account.</span></span> <span data-ttu-id="76ee8-187">Dado que la cuenta de sistema local no tiene permiso para la ruta de acceso de recurso compartido utilizado por la configuración de IIS compartido para modificar, el instalador llega a un error de acceso denegado al intentar configurar los ajustes del módulo en *applicationHost.config* en el recurso compartido.</span><span class="sxs-lookup"><span data-stu-id="76ee8-187">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer hits an access denied error when attempting to configure the module settings in *applicationHost.config* on the share.</span></span> <span data-ttu-id="76ee8-188">Cuando se usa una configuración de IIS compartido, siga estos pasos:</span><span class="sxs-lookup"><span data-stu-id="76ee8-188">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="76ee8-189">Deshabilitar la configuración de IIS compartido.</span><span class="sxs-lookup"><span data-stu-id="76ee8-189">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="76ee8-190">Ejecute el instalador.</span><span class="sxs-lookup"><span data-stu-id="76ee8-190">Run the installer.</span></span>
1. <span data-ttu-id="76ee8-191">Exportar actualizado *applicationHost.config* archivos al recurso compartido.</span><span class="sxs-lookup"><span data-stu-id="76ee8-191">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="76ee8-192">Volver a habilitar la configuración de IIS compartido.</span><span class="sxs-lookup"><span data-stu-id="76ee8-192">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="76ee8-193">Versión del módulo y el hospedaje de registros del instalador de paquete</span><span class="sxs-lookup"><span data-stu-id="76ee8-193">Module version and hosting bundle installer logs</span></span>

<span data-ttu-id="76ee8-194">Para determinar la versión del módulo de núcleo de ASP.NET instaladas:</span><span class="sxs-lookup"><span data-stu-id="76ee8-194">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="76ee8-195">En el sistema host, vaya a *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="76ee8-195">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="76ee8-196">Busque la *aspnetcore.dll* archivo.</span><span class="sxs-lookup"><span data-stu-id="76ee8-196">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="76ee8-197">Haga clic en el archivo y seleccione **propiedades** en el menú contextual.</span><span class="sxs-lookup"><span data-stu-id="76ee8-197">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="76ee8-198">Seleccione el **detalles** ficha. El **versión del archivo** y **versión del producto** representan la versión instalada del módulo.</span><span class="sxs-lookup"><span data-stu-id="76ee8-198">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="76ee8-199">Los registros de instalador de agrupación de hospedaje de Windows Server para el módulo se encuentran en *C:\\usuarios\\% UserName %\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span><span class="sxs-lookup"><span data-stu-id="76ee8-199">The Windows Server Hosting bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="76ee8-200">Ubicaciones de archivo de módulo, esquema y configuración</span><span class="sxs-lookup"><span data-stu-id="76ee8-200">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="76ee8-201">Module</span><span class="sxs-lookup"><span data-stu-id="76ee8-201">Module</span></span>

<span data-ttu-id="76ee8-202">**IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="76ee8-202">**IIS (x86/amd64):**</span></span>

   * <span data-ttu-id="76ee8-203">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="76ee8-203">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

   * <span data-ttu-id="76ee8-204">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="76ee8-204">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

<span data-ttu-id="76ee8-205">**IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="76ee8-205">**IIS Express (x86/amd64):**</span></span>

   * <span data-ttu-id="76ee8-206">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="76ee8-206">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

   * <span data-ttu-id="76ee8-207">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="76ee8-207">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

### <a name="schema"></a><span data-ttu-id="76ee8-208">Schema</span><span class="sxs-lookup"><span data-stu-id="76ee8-208">Schema</span></span>

<span data-ttu-id="76ee8-209">**IIS**</span><span class="sxs-lookup"><span data-stu-id="76ee8-209">**IIS**</span></span>

   * <span data-ttu-id="76ee8-210">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="76ee8-210">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

<span data-ttu-id="76ee8-211">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="76ee8-211">**IIS Express**</span></span>

   * <span data-ttu-id="76ee8-212">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="76ee8-212">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

### <a name="configuration"></a><span data-ttu-id="76ee8-213">Configuración</span><span class="sxs-lookup"><span data-stu-id="76ee8-213">Configuration</span></span>

<span data-ttu-id="76ee8-214">**IIS**</span><span class="sxs-lookup"><span data-stu-id="76ee8-214">**IIS**</span></span>

   * <span data-ttu-id="76ee8-215">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="76ee8-215">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="76ee8-216">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="76ee8-216">**IIS Express**</span></span>

   * <span data-ttu-id="76ee8-217">.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="76ee8-217">.vs\config\applicationHost.config</span></span>

<span data-ttu-id="76ee8-218">Los archivos se pueden encontrar buscando *aspnetcore.dll* en el *applicationHost.config* archivo.</span><span class="sxs-lookup"><span data-stu-id="76ee8-218">The files can be found by searching for *aspnetcore.dll* in the *applicationHost.config* file.</span></span> <span data-ttu-id="76ee8-219">Para IIS Express, la *applicationHost.config* el archivo no existe de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="76ee8-219">For IIS Express, the *applicationHost.config* file won't exist by default.</span></span> <span data-ttu-id="76ee8-220">El archivo se crea en  *\<application_root >\\VS\\config* al iniciar cualquier proyecto de aplicación web en la solución de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="76ee8-220">The file is created at *\<application_root>\\.vs\\config* when starting any web app project in the Visual Studio solution.</span></span>
