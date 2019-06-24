---
title: Registro y diagnóstico de ASP.NET Core SignalR
author: anurse
description: Obtenga información sobre cómo recopilar los diagnósticos desde la aplicación de ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: signalr
ms.date: 06/19/2019
uid: signalr/diagnostics
ms.openlocfilehash: 69dbd057b3dcadeb3ca5d94ede1234530fb447db
ms.sourcegitcommit: 9f11685382eb1f4dd0fb694dea797adacedf9e20
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/21/2019
ms.locfileid: "67313709"
---
# <a name="logging-and-diagnostics-in-aspnet-core-signalr"></a><span data-ttu-id="ff550-103">Registro y diagnóstico de ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="ff550-103">Logging and diagnostics in ASP.NET Core SignalR</span></span>

<span data-ttu-id="ff550-104">Por [Andrew Stanton-Nurse](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="ff550-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="ff550-105">Este artículo proporcionan instrucciones para recopilar diagnósticos desde la aplicación de ASP.NET Core SignalR para ayudar a solucionar problemas.</span><span class="sxs-lookup"><span data-stu-id="ff550-105">This article provides guidance for gathering diagnostics from your ASP.NET Core SignalR app to help troubleshoot issues.</span></span>

## <a name="server-side-logging"></a><span data-ttu-id="ff550-106">Registro del lado servidor</span><span class="sxs-lookup"><span data-stu-id="ff550-106">Server-side logging</span></span>

> [!WARNING]
> <span data-ttu-id="ff550-107">Registros de servidor pueden contener información confidencial de su aplicación.</span><span class="sxs-lookup"><span data-stu-id="ff550-107">Server-side logs may contain sensitive information from your app.</span></span> <span data-ttu-id="ff550-108">**Nunca** publicar los registros sin procesar de las aplicaciones de producción en los foros públicos como GitHub.</span><span class="sxs-lookup"><span data-stu-id="ff550-108">**Never** post raw logs from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="ff550-109">Dado que SignalR forma parte de ASP.NET Core, se usa el sistema de registro de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ff550-109">Since SignalR is part of ASP.NET Core, it uses the ASP.NET Core logging system.</span></span> <span data-ttu-id="ff550-110">En la configuración predeterminada, los registros de SignalR información muy poco, pero esto se puede configurar.</span><span class="sxs-lookup"><span data-stu-id="ff550-110">In the default configuration, SignalR logs very little information, but this can configured.</span></span> <span data-ttu-id="ff550-111">Consulte la documentación sobre [registro de ASP.NET Core](xref:fundamentals/logging/index#configuration) para obtener más información sobre cómo configurar el registro de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ff550-111">See the documentation on [ASP.NET Core logging](xref:fundamentals/logging/index#configuration) for details on configuring ASP.NET Core logging.</span></span>

<span data-ttu-id="ff550-112">SignalR usa dos categorías de registrador:</span><span class="sxs-lookup"><span data-stu-id="ff550-112">SignalR uses two logger categories:</span></span>

* <span data-ttu-id="ff550-113">`Microsoft.AspNetCore.SignalR` &ndash; para los registros relacionados con los protocolos de concentrador, activación de concentradores, invocar métodos y otras actividades relacionadas con el concentrador.</span><span class="sxs-lookup"><span data-stu-id="ff550-113">`Microsoft.AspNetCore.SignalR` &ndash; for logs related to Hub Protocols, activating Hubs, invoking methods, and other Hub-related activities.</span></span>
* <span data-ttu-id="ff550-114">`Microsoft.AspNetCore.Http.Connections` &ndash; para los registros relacionados con los transportes como WebSockets, Long Polling y los eventos y la infraestructura de SignalR bajo nivel.</span><span class="sxs-lookup"><span data-stu-id="ff550-114">`Microsoft.AspNetCore.Http.Connections` &ndash; for logs related to transports such as WebSockets, Long Polling and Server-Sent Events and low-level SignalR infrastructure.</span></span>

<span data-ttu-id="ff550-115">Para habilitar los registros detallados de SignalR, configure ambos prefijos anteriores a la `Debug` nivel en su *appsettings.json* archivo agregando los elementos siguientes en el `LogLevel` subsección en `Logging`:</span><span class="sxs-lookup"><span data-stu-id="ff550-115">To enable detailed logs from SignalR, configure both of the preceding prefixes to the `Debug` level in your *appsettings.json* file by adding the following items to the `LogLevel` sub-section in `Logging`:</span></span>

[!code-json[](diagnostics/logging-config.json?highlight=7-8)]

<span data-ttu-id="ff550-116">También puede configurar esto en el código en su `CreateWebHostBuilder` método:</span><span class="sxs-lookup"><span data-stu-id="ff550-116">You can also configure this in code in your `CreateWebHostBuilder` method:</span></span>

[!code-csharp[](diagnostics/logging-config-code.cs?highlight=5-6)]

<span data-ttu-id="ff550-117">Si no utiliza la configuración basada en JSON, establezca los siguientes valores de configuración en el sistema de configuración:</span><span class="sxs-lookup"><span data-stu-id="ff550-117">If you aren't using JSON-based configuration, set the following configuration values in your configuration system:</span></span>

* `Logging:LogLevel:Microsoft.AspNetCore.SignalR` = `Debug`
* `Logging:LogLevel:Microsoft.AspNetCore.Http.Connections` = `Debug`

<span data-ttu-id="ff550-118">Consulte la documentación de su sistema de configuración determinar cómo especificar los valores de configuración anidados.</span><span class="sxs-lookup"><span data-stu-id="ff550-118">Check the documentation for your configuration system to determine how to specify nested configuration values.</span></span> <span data-ttu-id="ff550-119">Por ejemplo, al usar variables de entorno, dos `_` caracteres se usan en lugar de la `:` (por ejemplo, `Logging__LogLevel__Microsoft.AspNetCore.SignalR`).</span><span class="sxs-lookup"><span data-stu-id="ff550-119">For example, when using environment variables, two `_` characters are used instead of the `:` (for example, `Logging__LogLevel__Microsoft.AspNetCore.SignalR`).</span></span>

<span data-ttu-id="ff550-120">Se recomienda usar la `Debug` al recopilar más detallada de diagnóstico para la aplicación de nivel.</span><span class="sxs-lookup"><span data-stu-id="ff550-120">We recommend using the `Debug` level when gathering more detailed diagnostics for your app.</span></span> <span data-ttu-id="ff550-121">El `Trace` nivel produce el diagnóstico de nivel muy bajo y rara vez se necesitan para diagnosticar problemas en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ff550-121">The `Trace` level produces very low-level diagnostics and is rarely needed to diagnose issues in your app.</span></span>

## <a name="access-server-side-logs"></a><span data-ttu-id="ff550-122">Acceso a los registros de servidor</span><span class="sxs-lookup"><span data-stu-id="ff550-122">Access server-side logs</span></span>

<span data-ttu-id="ff550-123">El acceso a los registros del lado servidor depende del entorno en el que está ejecutando.</span><span class="sxs-lookup"><span data-stu-id="ff550-123">How you access server-side logs depends on the environment in which you're running.</span></span>

### <a name="as-a-console-app-outside-iis"></a><span data-ttu-id="ff550-124">Como una aplicación de consola fuera de IIS</span><span class="sxs-lookup"><span data-stu-id="ff550-124">As a console app outside IIS</span></span>

<span data-ttu-id="ff550-125">Si está ejecutando en una aplicación de consola, el [registrador de consola](xref:fundamentals/logging/index#console-provider) debe habilitarse de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="ff550-125">If you're running in a console app, the [Console logger](xref:fundamentals/logging/index#console-provider) should be enabled by default.</span></span> <span data-ttu-id="ff550-126">Los registros de SignalR aparecerá en la consola.</span><span class="sxs-lookup"><span data-stu-id="ff550-126">SignalR logs will appear in the console.</span></span>

### <a name="within-iis-express-from-visual-studio"></a><span data-ttu-id="ff550-127">Dentro de IIS Express de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ff550-127">Within IIS Express from Visual Studio</span></span>

<span data-ttu-id="ff550-128">Visual Studio muestra la salida del registro en el **salida** ventana.</span><span class="sxs-lookup"><span data-stu-id="ff550-128">Visual Studio displays the log output in the **Output** window.</span></span> <span data-ttu-id="ff550-129">Seleccione el **servidor Web de ASP.NET Core** lista desplegable de la opción.</span><span class="sxs-lookup"><span data-stu-id="ff550-129">Select the **ASP.NET Core Web Server** drop down option.</span></span>

### <a name="azure-app-service"></a><span data-ttu-id="ff550-130">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="ff550-130">Azure App Service</span></span>

<span data-ttu-id="ff550-131">Habilitar la **registro de la aplicación (Filesystem)** opción el **los registros de diagnóstico** sección del portal de Azure App Service y configurar el **nivel** a `Verbose`.</span><span class="sxs-lookup"><span data-stu-id="ff550-131">Enable the **Application Logging (Filesystem)** option in the **Diagnostics logs** section of the Azure App Service portal and configure the **Level** to `Verbose`.</span></span> <span data-ttu-id="ff550-132">Los registros deben estar disponibles desde el **secuencias de registro** servicio y los registros del sistema de archivos de App Service.</span><span class="sxs-lookup"><span data-stu-id="ff550-132">Logs should be available from the **Log streaming** service and in logs on the file system of the App Service.</span></span> <span data-ttu-id="ff550-133">Para obtener más información, consulte [secuencias de registro Azure](xref:fundamentals/logging/index#azure-log-streaming).</span><span class="sxs-lookup"><span data-stu-id="ff550-133">For more information, see [Azure log streaming](xref:fundamentals/logging/index#azure-log-streaming).</span></span>

### <a name="other-environments"></a><span data-ttu-id="ff550-134">Otros entornos</span><span class="sxs-lookup"><span data-stu-id="ff550-134">Other environments</span></span>

<span data-ttu-id="ff550-135">Si la aplicación se implementa en otro entorno (por ejemplo, Docker, Kubernetes o el servicio de Windows), consulte <xref:fundamentals/logging/index> para obtener más información sobre cómo configurar los proveedores de registro adecuados para el entorno.</span><span class="sxs-lookup"><span data-stu-id="ff550-135">If the app is deployed to another environment (for example, Docker, Kubernetes, or Windows Service), see <xref:fundamentals/logging/index> for more information on how to configure logging providers suitable for the environment.</span></span>

## <a name="javascript-client-logging"></a><span data-ttu-id="ff550-136">Registro de cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="ff550-136">JavaScript client logging</span></span>

> [!WARNING]
> <span data-ttu-id="ff550-137">Registros del lado cliente pueden contener información confidencial de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ff550-137">Client-side logs may contain sensitive information from your app.</span></span> <span data-ttu-id="ff550-138">**Nunca** publicar los registros sin procesar de las aplicaciones de producción en los foros públicos como GitHub.</span><span class="sxs-lookup"><span data-stu-id="ff550-138">**Never** post raw logs from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="ff550-139">Cuando se usa el cliente de JavaScript, puede configurar las opciones de registro mediante el `configureLogging` método `HubConnectionBuilder`:</span><span class="sxs-lookup"><span data-stu-id="ff550-139">When using the JavaScript client, you can configure logging options using the `configureLogging` method on `HubConnectionBuilder`:</span></span>

[!code-javascript[](diagnostics/logging-config-js.js?highlight=3)]

<span data-ttu-id="ff550-140">Para deshabilitar el registro por completo, especifique `signalR.LogLevel.None` en el `configureLogging` método.</span><span class="sxs-lookup"><span data-stu-id="ff550-140">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="ff550-141">La siguiente tabla muestra los niveles de registro disponibles para el cliente de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ff550-141">The following table shows log levels available to the JavaScript client.</span></span> <span data-ttu-id="ff550-142">Establecer el nivel de registro en uno de estos valores, habilita el registro en ese nivel y todos los niveles por encima de él en la tabla.</span><span class="sxs-lookup"><span data-stu-id="ff550-142">Setting the log level to one of these values enables logging at that level and all levels above it in the table.</span></span>

| <span data-ttu-id="ff550-143">Nivel</span><span class="sxs-lookup"><span data-stu-id="ff550-143">Level</span></span> | <span data-ttu-id="ff550-144">Descripción</span><span class="sxs-lookup"><span data-stu-id="ff550-144">Description</span></span> |
| ----- | ----------- |
| `None` | <span data-ttu-id="ff550-145">Se registra ningún mensaje.</span><span class="sxs-lookup"><span data-stu-id="ff550-145">No messages are logged.</span></span> |
| `Critical` | <span data-ttu-id="ff550-146">Mensajes que indican un error en toda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ff550-146">Messages that indicate a failure in the entire app.</span></span> |
| `Error` | <span data-ttu-id="ff550-147">Mensajes que indican un error en la operación actual.</span><span class="sxs-lookup"><span data-stu-id="ff550-147">Messages that indicate a failure in the current operation.</span></span> |
| `Warning` | <span data-ttu-id="ff550-148">Mensajes que indican un problema grave.</span><span class="sxs-lookup"><span data-stu-id="ff550-148">Messages that indicate a non-fatal problem.</span></span> |
| `Information` | <span data-ttu-id="ff550-149">Mensajes informativos.</span><span class="sxs-lookup"><span data-stu-id="ff550-149">Informational messages.</span></span> |
| `Debug` | <span data-ttu-id="ff550-150">Mensajes de diagnóstico útiles para depurar.</span><span class="sxs-lookup"><span data-stu-id="ff550-150">Diagnostic messages useful for debugging.</span></span> |
| `Trace` | <span data-ttu-id="ff550-151">Mensajes de diagnóstico muy detallados diseñados para diagnosticar problemas específicos.</span><span class="sxs-lookup"><span data-stu-id="ff550-151">Very detailed diagnostic messages designed for diagnosing specific issues.</span></span> |

<span data-ttu-id="ff550-152">Una vez haya configurado el nivel de detalle, los registros se escribirán en la consola del explorador (o la salida estándar en una aplicación de NodeJS).</span><span class="sxs-lookup"><span data-stu-id="ff550-152">Once you've configured the verbosity, the logs will be written to the Browser Console (or Standard Output in a NodeJS app).</span></span>

<span data-ttu-id="ff550-153">Si desea enviar registros a un sistema de registro personalizado, puede proporcionar un objeto de JavaScript que implementa el `ILogger` interfaz.</span><span class="sxs-lookup"><span data-stu-id="ff550-153">If you want to send logs to a custom logging system, you can provide a JavaScript object implementing the `ILogger` interface.</span></span> <span data-ttu-id="ff550-154">Es el único método que debe implementarse `log`, que toma el nivel del evento y el mensaje asociado al evento.</span><span class="sxs-lookup"><span data-stu-id="ff550-154">The only method that needs to be implemented is `log`, which takes the level of the event and the message associated with the event.</span></span> <span data-ttu-id="ff550-155">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="ff550-155">For example:</span></span>

[!code-typescript[](diagnostics/custom-logger.ts?highlight=3-7,13)]

## <a name="net-client-logging"></a><span data-ttu-id="ff550-156">Registro de cliente de .NET</span><span class="sxs-lookup"><span data-stu-id="ff550-156">.NET client logging</span></span>

> [!WARNING]
> <span data-ttu-id="ff550-157">Registros del lado cliente pueden contener información confidencial de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ff550-157">Client-side logs may contain sensitive information from your app.</span></span> <span data-ttu-id="ff550-158">**Nunca** publicar los registros sin procesar de las aplicaciones de producción en los foros públicos como GitHub.</span><span class="sxs-lookup"><span data-stu-id="ff550-158">**Never** post raw logs from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="ff550-159">Para obtener los registros del cliente. NET, puede usar el `ConfigureLogging` método `HubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="ff550-159">To get logs from the .NET client, you can use the `ConfigureLogging` method on `HubConnectionBuilder`.</span></span> <span data-ttu-id="ff550-160">Esto funciona del mismo modo que el `ConfigureLogging` método `WebHostBuilder` y `HostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="ff550-160">This works the same way as the `ConfigureLogging` method on `WebHostBuilder` and `HostBuilder`.</span></span> <span data-ttu-id="ff550-161">Puede configurar los mismos proveedores de registro que usa en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ff550-161">You can configure the same logging providers you use in ASP.NET Core.</span></span> <span data-ttu-id="ff550-162">Sin embargo, deberá instalar manualmente y habilitar los paquetes de NuGet para los proveedores de registro individuales.</span><span class="sxs-lookup"><span data-stu-id="ff550-162">However, you have to manually install and enable the NuGet packages for the individual logging providers.</span></span>

### <a name="console-logging"></a><span data-ttu-id="ff550-163">Registro de la consola</span><span class="sxs-lookup"><span data-stu-id="ff550-163">Console logging</span></span>

<span data-ttu-id="ff550-164">Para habilitar el registro de consola, agregue el [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) paquete.</span><span class="sxs-lookup"><span data-stu-id="ff550-164">In order to enable Console logging, add the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) package.</span></span> <span data-ttu-id="ff550-165">A continuación, utilice el `AddConsole` método para configurar el registrador de consola:</span><span class="sxs-lookup"><span data-stu-id="ff550-165">Then, use the `AddConsole` method to configure the console logger:</span></span>

[!code-csharp[](diagnostics/net-client-console-log.cs?highlight=6)]

### <a name="debug-output-window-logging"></a><span data-ttu-id="ff550-166">El registro de la ventana de salida de depuración</span><span class="sxs-lookup"><span data-stu-id="ff550-166">Debug output window logging</span></span>

<span data-ttu-id="ff550-167">También puede configurar registros para ir a la **salida** ventana de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ff550-167">You can also configure logs to go to the **Output** window in Visual Studio.</span></span> <span data-ttu-id="ff550-168">Instalar el [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) empaquetar y utilizar el `AddDebug` método:</span><span class="sxs-lookup"><span data-stu-id="ff550-168">Install the [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) package and use the `AddDebug` method:</span></span>

[!code-csharp[](diagnostics/net-client-debug-log.cs?highlight=6)]

### <a name="other-logging-providers"></a><span data-ttu-id="ff550-169">Otros proveedores de registro</span><span class="sxs-lookup"><span data-stu-id="ff550-169">Other logging providers</span></span>

<span data-ttu-id="ff550-170">SignalR admite otros proveedores de registro como Seq, Serilog, NLog o cualquier otro sistema de registro que se integra con `Microsoft.Extensions.Logging`.</span><span class="sxs-lookup"><span data-stu-id="ff550-170">SignalR supports other logging providers such as Serilog, Seq, NLog, or any other logging system that integrates with `Microsoft.Extensions.Logging`.</span></span> <span data-ttu-id="ff550-171">Si su sistema de registro proporciona una `ILoggerProvider`, puede registrarlo con `AddProvider`:</span><span class="sxs-lookup"><span data-stu-id="ff550-171">If your logging system provides an `ILoggerProvider`, you can register it with `AddProvider`:</span></span>

[!code-csharp[](diagnostics/net-client-custom-log.cs?highlight=6)]

### <a name="control-verbosity"></a><span data-ttu-id="ff550-172">Nivel de detalle de control</span><span class="sxs-lookup"><span data-stu-id="ff550-172">Control verbosity</span></span>

<span data-ttu-id="ff550-173">Si está iniciando sesión desde otros lugares en su aplicación, cambiar el nivel predeterminado que `Debug` puede ser demasiado detallado.</span><span class="sxs-lookup"><span data-stu-id="ff550-173">If you are logging from other places in your app, changing the default level to `Debug` may be too verbose.</span></span> <span data-ttu-id="ff550-174">Puede utilizar un filtro para configurar el nivel de registro para los registros de SignalR.</span><span class="sxs-lookup"><span data-stu-id="ff550-174">You can use a Filter to configure the logging level for SignalR logs.</span></span> <span data-ttu-id="ff550-175">Esto puede hacerse en el código, casi la misma manera que en el servidor:</span><span class="sxs-lookup"><span data-stu-id="ff550-175">This can be done in code, in much the same way as on the server:</span></span>

[!code-csharp[Controlling verbosity in .NET client](diagnostics/logging-config-client-code.cs?highlight=9-10)]

## <a name="network-traces"></a><span data-ttu-id="ff550-176">Rastros de red</span><span class="sxs-lookup"><span data-stu-id="ff550-176">Network traces</span></span>

> [!WARNING]
> <span data-ttu-id="ff550-177">Un seguimiento de red contiene todo el contenido de todos los mensajes enviados por la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ff550-177">A network trace contains the full contents of every message sent by your app.</span></span> <span data-ttu-id="ff550-178">**Nunca** publicar los rastros de red sin procesar de aplicaciones de producción en los foros públicos como GitHub.</span><span class="sxs-lookup"><span data-stu-id="ff550-178">**Never** post raw network traces from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="ff550-179">Si encuentra algún problema, un seguimiento de red en ocasiones, puede proporcionar mucha información útil.</span><span class="sxs-lookup"><span data-stu-id="ff550-179">If you encounter an issue, a network trace can sometimes provide a lot of helpful information.</span></span> <span data-ttu-id="ff550-180">Esto es especialmente útil si va a informar de un problema en nuestro rastreador de problemas.</span><span class="sxs-lookup"><span data-stu-id="ff550-180">This is particularly useful if you're going to file an issue on our issue tracker.</span></span>

## <a name="collect-a-network-trace-with-fiddler-preferred-option"></a><span data-ttu-id="ff550-181">Recopilar un seguimiento de red con Fiddler (opción preferida)</span><span class="sxs-lookup"><span data-stu-id="ff550-181">Collect a network trace with Fiddler (preferred option)</span></span>

<span data-ttu-id="ff550-182">Este método funciona para todas las aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="ff550-182">This method works for all apps.</span></span>

<span data-ttu-id="ff550-183">Fiddler es una herramienta muy eficaz para recopilar seguimientos HTTP.</span><span class="sxs-lookup"><span data-stu-id="ff550-183">Fiddler is a very powerful tool for collecting HTTP traces.</span></span> <span data-ttu-id="ff550-184">Instálelo desde [telerik.com/fiddler](https://www.telerik.com/fiddler), iniciarla y, a continuación, ejecutar la aplicación y reproduzca el problema.</span><span class="sxs-lookup"><span data-stu-id="ff550-184">Install it from [telerik.com/fiddler](https://www.telerik.com/fiddler), launch it, and then run your app and reproduce the issue.</span></span> <span data-ttu-id="ff550-185">Fiddler está disponible para Windows, y existen versiones beta para macOS y Linux.</span><span class="sxs-lookup"><span data-stu-id="ff550-185">Fiddler is available for Windows, and there are beta versions for macOS and Linux.</span></span>

<span data-ttu-id="ff550-186">Si se conecta mediante HTTPS, hay algunos pasos adicionales para asegurarse de que Fiddler puede descifrar el tráfico HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ff550-186">If you connect using HTTPS, there are some extra steps to ensure Fiddler can decrypt the HTTPS traffic.</span></span> <span data-ttu-id="ff550-187">Para obtener más información, consulte el [documentación de Fiddler](https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/DecryptHTTPS).</span><span class="sxs-lookup"><span data-stu-id="ff550-187">For more details, see the [Fiddler documentation](https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/DecryptHTTPS).</span></span>

<span data-ttu-id="ff550-188">Una vez que haya recopilado el seguimiento, puede exportar el seguimiento seleccionando **archivo** > **guardar** > **todas las sesiones** desde la barra de menús.</span><span class="sxs-lookup"><span data-stu-id="ff550-188">Once you've collected the trace, you can export the trace by choosing **File** > **Save** > **All Sessions** from the menu bar.</span></span>

![Exportar todas las sesiones de Fiddler](diagnostics/fiddler-export.png)

## <a name="collect-a-network-trace-with-tcpdump-macos-and-linux-only"></a><span data-ttu-id="ff550-190">Recopilar un seguimiento de red con tcpdump (macOS y Linux solo)</span><span class="sxs-lookup"><span data-stu-id="ff550-190">Collect a network trace with tcpdump (macOS and Linux only)</span></span>

<span data-ttu-id="ff550-191">Este método funciona para todas las aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="ff550-191">This method works for all apps.</span></span>

<span data-ttu-id="ff550-192">Puede recopilar seguimientos TCP sin formato mediante tcpdump ejecutando el siguiente comando desde un shell de comandos.</span><span class="sxs-lookup"><span data-stu-id="ff550-192">You can collect raw TCP traces using tcpdump by running the following command from a command shell.</span></span> <span data-ttu-id="ff550-193">Es posible que deba ser `root` o un prefijo con el comando de `sudo` si se produce un error de permisos:</span><span class="sxs-lookup"><span data-stu-id="ff550-193">You may need to be `root` or prefix the command with `sudo` if you get a permissions error:</span></span>

```console
tcpdump -i [interface] -w trace.pcap
```

<span data-ttu-id="ff550-194">Reemplace `[interface]` con el que desea capturar en la interfaz de red.</span><span class="sxs-lookup"><span data-stu-id="ff550-194">Replace `[interface]` with the network interface you wish to capture on.</span></span> <span data-ttu-id="ff550-195">Normalmente, esto es algo parecido a `/dev/eth0` (para la interfaz Ethernet estándar) o `/dev/lo0` (para el tráfico de localhost).</span><span class="sxs-lookup"><span data-stu-id="ff550-195">Usually, this is something like `/dev/eth0` (for your standard Ethernet interface) or `/dev/lo0` (for localhost traffic).</span></span> <span data-ttu-id="ff550-196">Para obtener más información, consulte la `tcpdump` página man en el sistema host.</span><span class="sxs-lookup"><span data-stu-id="ff550-196">For more information, see the `tcpdump` man page on your host system.</span></span>

## <a name="collect-a-network-trace-in-the-browser"></a><span data-ttu-id="ff550-197">Recopilar un seguimiento de red en el explorador</span><span class="sxs-lookup"><span data-stu-id="ff550-197">Collect a network trace in the browser</span></span>

<span data-ttu-id="ff550-198">Este método solo funciona para las aplicaciones basadas en explorador.</span><span class="sxs-lookup"><span data-stu-id="ff550-198">This method only works for browser-based apps.</span></span>

<span data-ttu-id="ff550-199">Mayoría de las herramientas de desarrollador de explorador tiene una pestaña de "Red" que le permite capturar la actividad de red entre el explorador y el servidor.</span><span class="sxs-lookup"><span data-stu-id="ff550-199">Most browser Developer Tools have a "Network" tab that allows you to capture network activity between the browser and the server.</span></span> <span data-ttu-id="ff550-200">Sin embargo, estos seguimientos no incluyen los mensajes de WebSocket y los eventos.</span><span class="sxs-lookup"><span data-stu-id="ff550-200">However, these traces don't include WebSocket and Server-Sent Event messages.</span></span> <span data-ttu-id="ff550-201">Si utiliza estos transportes, mediante una herramienta como Fiddler o TcpDump (descrito a continuación) es un enfoque más adecuado.</span><span class="sxs-lookup"><span data-stu-id="ff550-201">If you are using those transports, using a tool like Fiddler or TcpDump (described below) is a better approach.</span></span>

### <a name="microsoft-edge-and-internet-explorer"></a><span data-ttu-id="ff550-202">Microsoft Edge e Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="ff550-202">Microsoft Edge and Internet Explorer</span></span>

<span data-ttu-id="ff550-203">(Las instrucciones son los mismos para Edge e Internet Explorer)</span><span class="sxs-lookup"><span data-stu-id="ff550-203">(The instructions are the same for both Edge and Internet Explorer)</span></span>

1. <span data-ttu-id="ff550-204">Presione F12 para abrir las herramientas de desarrollo</span><span class="sxs-lookup"><span data-stu-id="ff550-204">Press F12 to open the Dev Tools</span></span>
2. <span data-ttu-id="ff550-205">Haga clic en la ficha red</span><span class="sxs-lookup"><span data-stu-id="ff550-205">Click the Network Tab</span></span>
3. <span data-ttu-id="ff550-206">Actualice la página (si es necesario) y reproducir el problema</span><span class="sxs-lookup"><span data-stu-id="ff550-206">Refresh the page (if needed) and reproduce the problem</span></span>
4. <span data-ttu-id="ff550-207">Haga clic en el icono Guardar en la barra de herramientas para exportar el seguimiento como un archivo "HAR":</span><span class="sxs-lookup"><span data-stu-id="ff550-207">Click the Save icon in the toolbar to export the trace as a "HAR" file:</span></span>

![Guardar icono en el desarrollo de Microsoft Edge pestaña red de las herramientas](diagnostics/ie-edge-har-export.png)

### <a name="google-chrome"></a><span data-ttu-id="ff550-209">Google Chrome</span><span class="sxs-lookup"><span data-stu-id="ff550-209">Google Chrome</span></span>

1. <span data-ttu-id="ff550-210">Presione F12 para abrir las herramientas de desarrollo</span><span class="sxs-lookup"><span data-stu-id="ff550-210">Press F12 to open the Dev Tools</span></span>
2. <span data-ttu-id="ff550-211">Haga clic en la ficha red</span><span class="sxs-lookup"><span data-stu-id="ff550-211">Click the Network Tab</span></span>
3. <span data-ttu-id="ff550-212">Actualice la página (si es necesario) y reproducir el problema</span><span class="sxs-lookup"><span data-stu-id="ff550-212">Refresh the page (if needed) and reproduce the problem</span></span>
4. <span data-ttu-id="ff550-213">Haga clic en cualquier lugar en la lista de solicitudes y elija "Guardar como HAR con contenido":</span><span class="sxs-lookup"><span data-stu-id="ff550-213">Right click anywhere in the list of requests and choose "Save as HAR with content":</span></span>

![Opción "Guardar como HAR con contenido" en la pestaña de Google Chrome desarrollo las herramientas de red](diagnostics/chrome-har-export.png)

### <a name="mozilla-firefox"></a><span data-ttu-id="ff550-215">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="ff550-215">Mozilla Firefox</span></span>

1. <span data-ttu-id="ff550-216">Presione F12 para abrir las herramientas de desarrollo</span><span class="sxs-lookup"><span data-stu-id="ff550-216">Press F12 to open the Dev Tools</span></span>
2. <span data-ttu-id="ff550-217">Haga clic en la ficha red</span><span class="sxs-lookup"><span data-stu-id="ff550-217">Click the Network Tab</span></span>
3. <span data-ttu-id="ff550-218">Actualice la página (si es necesario) y reproducir el problema</span><span class="sxs-lookup"><span data-stu-id="ff550-218">Refresh the page (if needed) and reproduce the problem</span></span>
4. <span data-ttu-id="ff550-219">Haga clic en cualquier lugar en la lista de solicitudes y elija "Guardar todo como HAR"</span><span class="sxs-lookup"><span data-stu-id="ff550-219">Right click anywhere in the list of requests and choose "Save All As HAR"</span></span>

![Opción "Guardar todo como HAR" en la pestaña de red de herramientas de desarrollo de Mozilla Firefox](diagnostics/firefox-har-export.png)

## <a name="attach-diagnostics-files-to-github-issues"></a><span data-ttu-id="ff550-221">Adjuntar archivos de diagnóstico de problemas de GitHub</span><span class="sxs-lookup"><span data-stu-id="ff550-221">Attach diagnostics files to GitHub issues</span></span>

<span data-ttu-id="ff550-222">Puede adjuntar archivos de diagnóstico a problemas de GitHub cambiando por lo que tienen un `.txt` extensión y, a continuación, arrastrándolos y colocándolos en el problema.</span><span class="sxs-lookup"><span data-stu-id="ff550-222">You can attach Diagnostics files to GitHub issues by renaming them so they have a `.txt` extension and then dragging and dropping them on to the issue.</span></span>

> [!NOTE]
> <span data-ttu-id="ff550-223">No pegue el contenido de los archivos de registro o los rastros de red en un problema de GitHub.</span><span class="sxs-lookup"><span data-stu-id="ff550-223">Please don't paste the content of log files or network traces into a GitHub issue.</span></span> <span data-ttu-id="ff550-224">Estos registros y seguimientos pueden ser bastante grandes y GitHub normalmente los trunca.</span><span class="sxs-lookup"><span data-stu-id="ff550-224">These logs and traces can be quite large, and GitHub usually truncates them.</span></span>

![Arrastre los archivos de registro en un problema de GitHub](diagnostics/attaching-diagnostics-files.png)

## <a name="additional-resources"></a><span data-ttu-id="ff550-226">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="ff550-226">Additional resources</span></span>

* <xref:signalr/configuration>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
