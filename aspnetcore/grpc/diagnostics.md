---
title: Registro y diagnóstico en gRPC en .NET
author: jamesnk
description: Obtenga información sobre cómo recopilar diagnósticos de la aplicación de gRPC en .NET.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 09/23/2019
uid: grpc/diagnostics
ms.openlocfilehash: 17607b734e6d777de9516aa14e81c97f87b61023
ms.sourcegitcommit: 80286715afb93c4d13c931b008016d6086c0312b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2020
ms.locfileid: "77074528"
---
# <a name="logging-and-diagnostics-in-grpc-on-net"></a>Registro y diagnóstico en gRPC en .NET

Por [James Newton-King](https://twitter.com/jamesnk)

En este artículo se proporcionan instrucciones para recopilar diagnósticos de una aplicación de gRPC para ayudar a solucionar problemas. Temas cubiertos:

* Registros estructurados de **registro** escritos en el [registro de .net Core](xref:fundamentals/logging/index). los marcos de trabajo de aplicaciones usan <xref:Microsoft.Extensions.Logging.ILogger> para escribir registros y los usuarios para su propio registro en una aplicación.
* **Seguimiento** : eventos relacionados con una operación escrita mediante `DiaganosticSource` y `Activity`. Los seguimientos del origen de diagnóstico se suelen usar para recopilar la telemetría de la aplicación mediante bibliotecas como [Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core) y [OpenTelemetry](https://github.com/open-telemetry/opentelemetry-dotnet).
* **Métricas** : representación de medidas de datos a lo largo de intervalos de tiempo, por ejemplo, solicitudes por segundo. Las métricas se emiten mediante `EventCounter` y se pueden observar mediante la herramienta de línea [de comandos dotnet-counters](https://docs.microsoft.com/dotnet/core/diagnostics/dotnet-counters) o con [Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/eventcounters).

## <a name="logging"></a>Registro

los servicios de gRPC y el cliente de gRPC escriben registros con el [registro de .net Core](xref:fundamentals/logging/index). Los registros son un buen punto de partida cuando se necesita depurar un comportamiento inesperado en las aplicaciones.

### <a name="grpc-services-logging"></a>registro de gRPC Services

> [!WARNING]
> Los registros del lado servidor pueden contener información confidencial de la aplicación. No publique **nunca** los registros sin procesar de las aplicaciones de producción en foros públicos como github.

Dado que los servicios de gRPC se hospedan en ASP.NET Core, usa el sistema de registro de ASP.NET Core. En la configuración predeterminada, gRPC registra muy poca información, pero esto puede configurarse. Consulte la documentación sobre el [registro de ASP.net Core](xref:fundamentals/logging/index#configuration) para obtener más información sobre la configuración del registro de ASP.net Core.

gRPC agrega registros en la categoría `Grpc`. Para habilitar registros detallados de gRPC, configure los prefijos de `Grpc` en el nivel de `Debug` del archivo *appSettings. JSON* agregando los siguientes elementos a la subsección `LogLevel` en `Logging`:

[!code-json[](diagnostics/sample/logging-config.json?highlight=7)]

También puede configurarlo en *Startup.CS* con `ConfigureLogging`:

[!code-csharp[](diagnostics/sample/logging-config-code.cs?highlight=5)]

Si no está usando la configuración basada en JSON, establezca el siguiente valor de configuración en el sistema de configuración:

* `Logging:LogLevel:Grpc` = `Debug`

Consulte la documentación del sistema de configuración para determinar cómo especificar valores de configuración anidados. Por ejemplo, al usar variables de entorno, se usan dos caracteres `_` en lugar del `:` (por ejemplo, `Logging__LogLevel__Grpc`).

Se recomienda usar el nivel de `Debug` al recopilar diagnósticos más detallados para la aplicación. El nivel de `Trace` produce diagnósticos de nivel muy bajo y rara vez es necesario para diagnosticar problemas en la aplicación.

#### <a name="sample-logging-output"></a>Salida de registro de ejemplo

Este es un ejemplo de la salida de la consola en el nivel de `Debug` de un servicio gRPC:

```console
info: Microsoft.AspNetCore.Hosting.Diagnostics[1]
      Request starting HTTP/2 POST https://localhost:5001/Greet.Greeter/SayHello application/grpc
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[0]
      Executing endpoint 'gRPC - /Greet.Greeter/SayHello'
dbug: Grpc.AspNetCore.Server.ServerCallHandler[1]
      Reading message.
info: GrpcService.GreeterService[0]
      Hello World
dbug: Grpc.AspNetCore.Server.ServerCallHandler[6]
      Sending message.
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[1]
      Executed endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Hosting.Diagnostics[2]
      Request finished in 1.4113ms 200 application/grpc
```

### <a name="access-server-side-logs"></a>Acceder a los registros del lado servidor

La forma de acceder a los registros del lado servidor depende del entorno en el que se está ejecutando.

#### <a name="as-a-console-app"></a>Como aplicación de consola

Si está ejecutando en una aplicación de consola, el [registrador de consola](xref:fundamentals/logging/index#console-provider) debe estar habilitado de forma predeterminada. los registros de gRPC aparecerán en la consola de.

#### <a name="other-environments"></a>Otros entornos

Si la aplicación se implementa en otro entorno (por ejemplo, Docker, Kubernetes o servicio de Windows), consulte <xref:fundamentals/logging/index> para obtener más información sobre cómo configurar los proveedores de registro adecuados para el entorno.

### <a name="grpc-client-logging"></a>registro de cliente de gRPC

> [!WARNING]
> Los registros del lado cliente pueden contener información confidencial de la aplicación. No publique **nunca** los registros sin procesar de las aplicaciones de producción en foros públicos como github.

Para obtener los registros del cliente .NET, puede establecer la propiedad `GrpcChannelOptions.LoggerFactory` cuando se crea el canal del cliente. Si está llamando a un servicio de gRPC desde una aplicación ASP.NET Core, el generador del registrador se puede resolver desde la inserción de dependencias (DI):

[!code-csharp[](diagnostics/sample/net-client-dependency-injection.cs?highlight=7,16)]

Una manera alternativa de habilitar el registro de cliente es usar el [generador de cliente de gRPC](xref:grpc/clientfactory) para crear el cliente. Un cliente de gRPC registrado con el generador de cliente y resuelto desde DI usará automáticamente el registro configurado de la aplicación.

Si la aplicación no usa DI, puede crear una nueva instancia de `ILoggerFactory` con [LoggerFactory. Create](xref:Microsoft.Extensions.Logging.LoggerFactory.Create*). Para tener acceso a este método, agregue el paquete [Microsoft. Extensions. Logging](https://www.nuget.org/packages/microsoft.extensions.logging/) a la aplicación.

[!code-csharp[](diagnostics/sample/net-client-loggerfactory-create.cs?highlight=1,8)]

#### <a name="grpc-client-log-scopes"></a>ámbitos de registro de cliente de gRPC

El cliente gRPC agrega un [ámbito de registro](https://docs.microsoft.com/aspnet/core/fundamentals/logginglog-scopes) a los registros realizados durante una llamada a gRPC. El ámbito tiene metadatos relacionados con la llamada a gRPC:

* **GrpcMethodType** : el tipo de método gRPC. Los valores posibles son los nombres de `Grpc.Core.MethodType` enumeración, por ejemplo, unario
* **GrpcUri** : el URI relativo del método gRPC, por ejemplo,/Greet. Greeter/SayHellos

#### <a name="sample-logging-output"></a>Salida de registro de ejemplo

Este es un ejemplo de la salida de la consola en el nivel de `Debug` de un cliente de gRPC:

```console
dbug: Grpc.Net.Client.Internal.GrpcCall[1]
      Starting gRPC call. Method type: 'Unary', URI: 'https://localhost:5001/Greet.Greeter/SayHello'.
dbug: Grpc.Net.Client.Internal.GrpcCall[6]
      Sending message.
dbug: Grpc.Net.Client.Internal.GrpcCall[1]
      Reading message.
dbug: Grpc.Net.Client.Internal.GrpcCall[4]
      Finished gRPC call.
```

## <a name="tracing"></a>Traza

los servicios de gRPC y el cliente de gRPC proporcionan información acerca de las llamadas gRPC mediante la [actividad](https://docs.microsoft.com/dotnet/api/system.diagnostics.activity) [DiagnosticSource](https://docs.microsoft.com/dotnet/api/system.diagnostics.diagnosticsource) y.

* .NET gRPC usa una actividad para representar una llamada a gRPC.
* Los eventos de seguimiento se escriben en el origen de diagnóstico al iniciar y detener la actividad de llamada gRPC.
* El seguimiento no captura información sobre cuándo se envían los mensajes a lo largo de la duración de las llamadas de streaming de gRPC.

### <a name="grpc-service-tracing"></a>seguimiento del servicio gRPC

los servicios de gRPC se hospedan en ASP.NET Core que notifican eventos acerca de las solicitudes HTTP entrantes. los metadatos específicos de gRPC se agregan al diagnóstico de solicitud HTTP existente que proporciona ASP.NET Core.

* El nombre del origen de diagnóstico es `Microsoft.AspNetCore`.
* El nombre de la actividad es `Microsoft.AspNetCore.Hosting.HttpRequestIn`.
  * El nombre del método gRPC invocado por la llamada a gRPC se agrega como una etiqueta con el nombre `grpc.method`.
  * El código de estado de la llamada a gRPC cuando se completa se agrega como una etiqueta con el nombre `grpc.status_code`.

### <a name="grpc-client-tracing"></a>seguimiento de cliente de gRPC

El cliente gRPC de .NET usa `HttpClient` para realizar llamadas de gRPC. Aunque `HttpClient` escribe eventos de diagnóstico, el cliente gRPC de .NET proporciona un origen de diagnóstico, actividad y eventos personalizados para que se pueda recopilar información completa sobre una llamada a gRPC.

* El nombre del origen de diagnóstico es `Grpc.Net.Client`.
* El nombre de la actividad es `Grpc.Net.Client.GrpcOut`.
  * El nombre del método gRPC invocado por la llamada a gRPC se agrega como una etiqueta con el nombre `grpc.method`.
  * El código de estado de la llamada a gRPC cuando se completa se agrega como una etiqueta con el nombre `grpc.status_code`.

### <a name="collecting-tracing"></a>Recopilando seguimiento

La forma más fácil de usar `DiagnosticSource` es configurar una biblioteca de telemetría como [Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core) o [OpenTelemetry](https://github.com/open-telemetry/opentelemetry-dotnet) en la aplicación. La biblioteca procesará información acerca de las llamadas a gRPC en el lado de la telemetría de la aplicación.

El seguimiento se puede ver en un servicio administrado como Application Insights, o bien puede elegir ejecutar su propio sistema de seguimiento distribuido. OpenTelemetry admite la exportación de datos de seguimiento a [Jaeger](https://www.jaegertracing.io/) y [Zipkin](https://zipkin.io/).

`DiagnosticSource` puede consumir eventos de traza en el código mediante `DiagnosticListener`. Para obtener información sobre cómo escuchar un origen de diagnóstico con código, vea la [Guía del usuario de DiagnosticSource](https://github.com/dotnet/corefx/blob/d3942d4671919edb0cca6ddc1840190f524a809d/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md#consuming-data-with-diagnosticlistener).

> [!NOTE]
> Actualmente, las bibliotecas de telemetría no capturan la telemetría `Grpc.Net.Client.GrpcOut` específica de gRPC. El trabajo para mejorar las bibliotecas de telemetría que capturan este seguimiento está en curso.

## <a name="metrics"></a>metrics

Métricas es una representación de medidas de datos a lo largo de intervalos de tiempo, por ejemplo, solicitudes por segundo. Los datos de las métricas permiten la observación del estado de una aplicación en un nivel alto. Las métricas de .NET gRPC se emiten mediante `EventCounter`.

### <a name="grpc-service-metrics"></a>métricas de servicio de gRPC

las métricas del servidor de gRPC se indican en `Grpc.AspNetCore.Server` origen del evento.

| nombre                      | Descripción                   |
| --------------------------|-------------------------------|
| `total-calls`             | Total de llamadas                   |
| `current-calls`           | Llamadas actuales                 |
| `calls-failed`            | Total de llamadas erróneas            |
| `calls-deadline-exceeded` | Fecha límite de llamadas totales superada |
| `messages-sent`           | total de mensajes enviados           |
| `messages-received`       | Mensajes totales recibidos       |
| `calls-unimplemented`     | Total de llamadas no implementadas     |

ASP.NET Core también proporciona sus propias métricas en `Microsoft.AspNetCore.Hosting` origen de eventos.

### <a name="grpc-client-metrics"></a>métricas de cliente de gRPC

las métricas de cliente de gRPC se indican en `Grpc.Net.Client` origen de eventos.

| nombre                      | Descripción                   |
| --------------------------|-------------------------------|
| `total-calls`             | Total de llamadas                   |
| `current-calls`           | Llamadas actuales                 |
| `calls-failed`            | Total de llamadas erróneas            |
| `calls-deadline-exceeded` | Fecha límite de llamadas totales superada |
| `messages-sent`           | total de mensajes enviados           |
| `messages-received`       | Mensajes totales recibidos       |

### <a name="observe-metrics"></a>Observar métricas

[dotnet-counters](https://docs.microsoft.com/dotnet/core/diagnostics/dotnet-counters) es una herramienta de supervisión del rendimiento para la supervisión de estado ad hoc y la investigación de rendimiento de primer nivel. Supervise una aplicación .NET con `Grpc.AspNetCore.Server` o `Grpc.Net.Client` como el nombre del proveedor.

```console
> dotnet-counters monitor --process-id 1902 Grpc.AspNetCore.Server

Press p to pause, r to resume, q to quit.
    Status: Running
[Grpc.AspNetCore.Server]
    Total Calls                                 300
    Current Calls                               5
    Total Calls Failed                          0
    Total Calls Deadline Exceeded               0
    Total Messages Sent                         295
    Total Messages Received                     300
    Total Calls Unimplemented                   0
```

Otra forma de observar las métricas de gRPC es capturar los datos de contador mediante el [paquete Microsoft. ApplicationInsights. EventCounterCollector](https://docs.microsoft.com/azure/azure-monitor/app/eventcounters)de Application Insights. Una vez realizada la instalación, Application Insights recopila Contadores comunes de .NET en tiempo de ejecución. los contadores de gRPC no se recopilan de forma predeterminada, pero App Insights se puede [personalizar para incluir contadores adicionales](https://docs.microsoft.com/azure/azure-monitor/app/eventcounters#customizing-counters-to-be-collected).

Especifique los contadores de gRPC para que Application Insights recopile en *Startup.CS*:

```csharp
    using Microsoft.ApplicationInsights.Extensibility.EventCounterCollector;

    public void ConfigureServices(IServiceCollection services)
    {
        //... other code...

        services.ConfigureTelemetryModule<EventCounterCollectionModule>(
            (module, o) =>
            {
                // Configure App Insights to collect gRPC counters gRPC services hosted in an ASP.NET Core app
                module.Counters.Add(new EventCounterCollectionRequest("Grpc.AspNetCore.Server", "current-calls"));
                module.Counters.Add(new EventCounterCollectionRequest("Grpc.AspNetCore.Server", "total-calls"));
                module.Counters.Add(new EventCounterCollectionRequest("Grpc.AspNetCore.Server", "calls-failed"));
            }
        );
    }
```

## <a name="additional-resources"></a>Recursos adicionales

* <xref:fundamentals/logging/index>
* <xref:grpc/configuration>
* <xref:grpc/clientfactory>
