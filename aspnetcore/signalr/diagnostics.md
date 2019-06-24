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
# <a name="logging-and-diagnostics-in-aspnet-core-signalr"></a>Registro y diagnóstico de ASP.NET Core SignalR

Por [Andrew Stanton-Nurse](https://twitter.com/anurse)

Este artículo proporcionan instrucciones para recopilar diagnósticos desde la aplicación de ASP.NET Core SignalR para ayudar a solucionar problemas.

## <a name="server-side-logging"></a>Registro del lado servidor

> [!WARNING]
> Registros de servidor pueden contener información confidencial de su aplicación. **Nunca** publicar los registros sin procesar de las aplicaciones de producción en los foros públicos como GitHub.

Dado que SignalR forma parte de ASP.NET Core, se usa el sistema de registro de ASP.NET Core. En la configuración predeterminada, los registros de SignalR información muy poco, pero esto se puede configurar. Consulte la documentación sobre [registro de ASP.NET Core](xref:fundamentals/logging/index#configuration) para obtener más información sobre cómo configurar el registro de ASP.NET Core.

SignalR usa dos categorías de registrador:

* `Microsoft.AspNetCore.SignalR` &ndash; para los registros relacionados con los protocolos de concentrador, activación de concentradores, invocar métodos y otras actividades relacionadas con el concentrador.
* `Microsoft.AspNetCore.Http.Connections` &ndash; para los registros relacionados con los transportes como WebSockets, Long Polling y los eventos y la infraestructura de SignalR bajo nivel.

Para habilitar los registros detallados de SignalR, configure ambos prefijos anteriores a la `Debug` nivel en su *appsettings.json* archivo agregando los elementos siguientes en el `LogLevel` subsección en `Logging`:

[!code-json[](diagnostics/logging-config.json?highlight=7-8)]

También puede configurar esto en el código en su `CreateWebHostBuilder` método:

[!code-csharp[](diagnostics/logging-config-code.cs?highlight=5-6)]

Si no utiliza la configuración basada en JSON, establezca los siguientes valores de configuración en el sistema de configuración:

* `Logging:LogLevel:Microsoft.AspNetCore.SignalR` = `Debug`
* `Logging:LogLevel:Microsoft.AspNetCore.Http.Connections` = `Debug`

Consulte la documentación de su sistema de configuración determinar cómo especificar los valores de configuración anidados. Por ejemplo, al usar variables de entorno, dos `_` caracteres se usan en lugar de la `:` (por ejemplo, `Logging__LogLevel__Microsoft.AspNetCore.SignalR`).

Se recomienda usar la `Debug` al recopilar más detallada de diagnóstico para la aplicación de nivel. El `Trace` nivel produce el diagnóstico de nivel muy bajo y rara vez se necesitan para diagnosticar problemas en la aplicación.

## <a name="access-server-side-logs"></a>Acceso a los registros de servidor

El acceso a los registros del lado servidor depende del entorno en el que está ejecutando.

### <a name="as-a-console-app-outside-iis"></a>Como una aplicación de consola fuera de IIS

Si está ejecutando en una aplicación de consola, el [registrador de consola](xref:fundamentals/logging/index#console-provider) debe habilitarse de forma predeterminada. Los registros de SignalR aparecerá en la consola.

### <a name="within-iis-express-from-visual-studio"></a>Dentro de IIS Express de Visual Studio

Visual Studio muestra la salida del registro en el **salida** ventana. Seleccione el **servidor Web de ASP.NET Core** lista desplegable de la opción.

### <a name="azure-app-service"></a>Azure App Service

Habilitar la **registro de la aplicación (Filesystem)** opción el **los registros de diagnóstico** sección del portal de Azure App Service y configurar el **nivel** a `Verbose`. Los registros deben estar disponibles desde el **secuencias de registro** servicio y los registros del sistema de archivos de App Service. Para obtener más información, consulte [secuencias de registro Azure](xref:fundamentals/logging/index#azure-log-streaming).

### <a name="other-environments"></a>Otros entornos

Si la aplicación se implementa en otro entorno (por ejemplo, Docker, Kubernetes o el servicio de Windows), consulte <xref:fundamentals/logging/index> para obtener más información sobre cómo configurar los proveedores de registro adecuados para el entorno.

## <a name="javascript-client-logging"></a>Registro de cliente de JavaScript

> [!WARNING]
> Registros del lado cliente pueden contener información confidencial de la aplicación. **Nunca** publicar los registros sin procesar de las aplicaciones de producción en los foros públicos como GitHub.

Cuando se usa el cliente de JavaScript, puede configurar las opciones de registro mediante el `configureLogging` método `HubConnectionBuilder`:

[!code-javascript[](diagnostics/logging-config-js.js?highlight=3)]

Para deshabilitar el registro por completo, especifique `signalR.LogLevel.None` en el `configureLogging` método.

La siguiente tabla muestra los niveles de registro disponibles para el cliente de JavaScript. Establecer el nivel de registro en uno de estos valores, habilita el registro en ese nivel y todos los niveles por encima de él en la tabla.

| Nivel | Descripción |
| ----- | ----------- |
| `None` | Se registra ningún mensaje. |
| `Critical` | Mensajes que indican un error en toda la aplicación. |
| `Error` | Mensajes que indican un error en la operación actual. |
| `Warning` | Mensajes que indican un problema grave. |
| `Information` | Mensajes informativos. |
| `Debug` | Mensajes de diagnóstico útiles para depurar. |
| `Trace` | Mensajes de diagnóstico muy detallados diseñados para diagnosticar problemas específicos. |

Una vez haya configurado el nivel de detalle, los registros se escribirán en la consola del explorador (o la salida estándar en una aplicación de NodeJS).

Si desea enviar registros a un sistema de registro personalizado, puede proporcionar un objeto de JavaScript que implementa el `ILogger` interfaz. Es el único método que debe implementarse `log`, que toma el nivel del evento y el mensaje asociado al evento. Por ejemplo:

[!code-typescript[](diagnostics/custom-logger.ts?highlight=3-7,13)]

## <a name="net-client-logging"></a>Registro de cliente de .NET

> [!WARNING]
> Registros del lado cliente pueden contener información confidencial de la aplicación. **Nunca** publicar los registros sin procesar de las aplicaciones de producción en los foros públicos como GitHub.

Para obtener los registros del cliente. NET, puede usar el `ConfigureLogging` método `HubConnectionBuilder`. Esto funciona del mismo modo que el `ConfigureLogging` método `WebHostBuilder` y `HostBuilder`. Puede configurar los mismos proveedores de registro que usa en ASP.NET Core. Sin embargo, deberá instalar manualmente y habilitar los paquetes de NuGet para los proveedores de registro individuales.

### <a name="console-logging"></a>Registro de la consola

Para habilitar el registro de consola, agregue el [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) paquete. A continuación, utilice el `AddConsole` método para configurar el registrador de consola:

[!code-csharp[](diagnostics/net-client-console-log.cs?highlight=6)]

### <a name="debug-output-window-logging"></a>El registro de la ventana de salida de depuración

También puede configurar registros para ir a la **salida** ventana de Visual Studio. Instalar el [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) empaquetar y utilizar el `AddDebug` método:

[!code-csharp[](diagnostics/net-client-debug-log.cs?highlight=6)]

### <a name="other-logging-providers"></a>Otros proveedores de registro

SignalR admite otros proveedores de registro como Seq, Serilog, NLog o cualquier otro sistema de registro que se integra con `Microsoft.Extensions.Logging`. Si su sistema de registro proporciona una `ILoggerProvider`, puede registrarlo con `AddProvider`:

[!code-csharp[](diagnostics/net-client-custom-log.cs?highlight=6)]

### <a name="control-verbosity"></a>Nivel de detalle de control

Si está iniciando sesión desde otros lugares en su aplicación, cambiar el nivel predeterminado que `Debug` puede ser demasiado detallado. Puede utilizar un filtro para configurar el nivel de registro para los registros de SignalR. Esto puede hacerse en el código, casi la misma manera que en el servidor:

[!code-csharp[Controlling verbosity in .NET client](diagnostics/logging-config-client-code.cs?highlight=9-10)]

## <a name="network-traces"></a>Rastros de red

> [!WARNING]
> Un seguimiento de red contiene todo el contenido de todos los mensajes enviados por la aplicación. **Nunca** publicar los rastros de red sin procesar de aplicaciones de producción en los foros públicos como GitHub.

Si encuentra algún problema, un seguimiento de red en ocasiones, puede proporcionar mucha información útil. Esto es especialmente útil si va a informar de un problema en nuestro rastreador de problemas.

## <a name="collect-a-network-trace-with-fiddler-preferred-option"></a>Recopilar un seguimiento de red con Fiddler (opción preferida)

Este método funciona para todas las aplicaciones.

Fiddler es una herramienta muy eficaz para recopilar seguimientos HTTP. Instálelo desde [telerik.com/fiddler](https://www.telerik.com/fiddler), iniciarla y, a continuación, ejecutar la aplicación y reproduzca el problema. Fiddler está disponible para Windows, y existen versiones beta para macOS y Linux.

Si se conecta mediante HTTPS, hay algunos pasos adicionales para asegurarse de que Fiddler puede descifrar el tráfico HTTPS. Para obtener más información, consulte el [documentación de Fiddler](https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/DecryptHTTPS).

Una vez que haya recopilado el seguimiento, puede exportar el seguimiento seleccionando **archivo** > **guardar** > **todas las sesiones** desde la barra de menús.

![Exportar todas las sesiones de Fiddler](diagnostics/fiddler-export.png)

## <a name="collect-a-network-trace-with-tcpdump-macos-and-linux-only"></a>Recopilar un seguimiento de red con tcpdump (macOS y Linux solo)

Este método funciona para todas las aplicaciones.

Puede recopilar seguimientos TCP sin formato mediante tcpdump ejecutando el siguiente comando desde un shell de comandos. Es posible que deba ser `root` o un prefijo con el comando de `sudo` si se produce un error de permisos:

```console
tcpdump -i [interface] -w trace.pcap
```

Reemplace `[interface]` con el que desea capturar en la interfaz de red. Normalmente, esto es algo parecido a `/dev/eth0` (para la interfaz Ethernet estándar) o `/dev/lo0` (para el tráfico de localhost). Para obtener más información, consulte la `tcpdump` página man en el sistema host.

## <a name="collect-a-network-trace-in-the-browser"></a>Recopilar un seguimiento de red en el explorador

Este método solo funciona para las aplicaciones basadas en explorador.

Mayoría de las herramientas de desarrollador de explorador tiene una pestaña de "Red" que le permite capturar la actividad de red entre el explorador y el servidor. Sin embargo, estos seguimientos no incluyen los mensajes de WebSocket y los eventos. Si utiliza estos transportes, mediante una herramienta como Fiddler o TcpDump (descrito a continuación) es un enfoque más adecuado.

### <a name="microsoft-edge-and-internet-explorer"></a>Microsoft Edge e Internet Explorer

(Las instrucciones son los mismos para Edge e Internet Explorer)

1. Presione F12 para abrir las herramientas de desarrollo
2. Haga clic en la ficha red
3. Actualice la página (si es necesario) y reproducir el problema
4. Haga clic en el icono Guardar en la barra de herramientas para exportar el seguimiento como un archivo "HAR":

![Guardar icono en el desarrollo de Microsoft Edge pestaña red de las herramientas](diagnostics/ie-edge-har-export.png)

### <a name="google-chrome"></a>Google Chrome

1. Presione F12 para abrir las herramientas de desarrollo
2. Haga clic en la ficha red
3. Actualice la página (si es necesario) y reproducir el problema
4. Haga clic en cualquier lugar en la lista de solicitudes y elija "Guardar como HAR con contenido":

![Opción "Guardar como HAR con contenido" en la pestaña de Google Chrome desarrollo las herramientas de red](diagnostics/chrome-har-export.png)

### <a name="mozilla-firefox"></a>Mozilla Firefox

1. Presione F12 para abrir las herramientas de desarrollo
2. Haga clic en la ficha red
3. Actualice la página (si es necesario) y reproducir el problema
4. Haga clic en cualquier lugar en la lista de solicitudes y elija "Guardar todo como HAR"

![Opción "Guardar todo como HAR" en la pestaña de red de herramientas de desarrollo de Mozilla Firefox](diagnostics/firefox-har-export.png)

## <a name="attach-diagnostics-files-to-github-issues"></a>Adjuntar archivos de diagnóstico de problemas de GitHub

Puede adjuntar archivos de diagnóstico a problemas de GitHub cambiando por lo que tienen un `.txt` extensión y, a continuación, arrastrándolos y colocándolos en el problema.

> [!NOTE]
> No pegue el contenido de los archivos de registro o los rastros de red en un problema de GitHub. Estos registros y seguimientos pueden ser bastante grandes y GitHub normalmente los trunca.

![Arrastre los archivos de registro en un problema de GitHub](diagnostics/attaching-diagnostics-files.png)

## <a name="additional-resources"></a>Recursos adicionales

* <xref:signalr/configuration>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
