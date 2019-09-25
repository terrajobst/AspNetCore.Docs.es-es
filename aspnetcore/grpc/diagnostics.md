---
title: Registro y diagnóstico en gRPC en .NET
author: jamesnk
description: Obtenga información sobre cómo recopilar diagnósticos de la aplicación de gRPC en .NET.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 09/23/2019
uid: grpc/diagnostics
ms.openlocfilehash: 7194e91b40a08c4a7ee619b8f207900af2683aa1
ms.sourcegitcommit: fae6f0e253f9d62d8f39de5884d2ba2b4b2a6050
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2019
ms.locfileid: "71250733"
---
# <a name="logging-and-diagnostics-in-grpc-on-net"></a>Registro y diagnóstico en gRPC en .NET

Por [James Newton-King](https://twitter.com/jamesnk)

En este artículo se proporcionan instrucciones para recopilar diagnósticos de la aplicación de gRPC para ayudar a solucionar problemas.

## <a name="grpc-services-logging"></a>registro de gRPC Services

> [!WARNING]
> Los registros del lado servidor pueden contener información confidencial de la aplicación. No publique **nunca** los registros sin procesar de las aplicaciones de producción en foros públicos como github.

Dado que los servicios de gRPC se hospedan en ASP.NET Core, usa el sistema de registro de ASP.NET Core. En la configuración predeterminada, gRPC registra muy poca información, pero esto puede configurarse. Consulte la documentación sobre el [registro de ASP.net Core](xref:fundamentals/logging/index#configuration) para obtener más información sobre la configuración del registro de ASP.net Core.

gRPC agrega registros en la `Grpc` categoría. Para habilitar registros detallados de gRPC, `Grpc` configure los prefijos en el `Debug` nivel del archivo *appSettings. JSON* agregando los siguientes `LogLevel` elementos a la subsección de: `Logging`

[!code-json[](diagnostics/sample/logging-config.json?highlight=7)]

También puede configurarlo en *Startup.CS* con `ConfigureLogging`:

[!code-csharp[](diagnostics/sample/logging-config-code.cs?highlight=5)]

Si no está usando la configuración basada en JSON, establezca el siguiente valor de configuración en el sistema de configuración:

* `Logging:LogLevel:Grpc` = `Debug`

Consulte la documentación del sistema de configuración para determinar cómo especificar valores de configuración anidados. Por ejemplo, al usar variables de entorno, `_` se usan dos caracteres en lugar `:` de (por ejemplo, `Logging__LogLevel__Grpc`).

Se recomienda usar el `Debug` nivel al recopilar diagnósticos más detallados para la aplicación. El `Trace` nivel genera diagnósticos de nivel muy bajo y rara vez es necesario para diagnosticar problemas en la aplicación.

### <a name="sample-logging-output"></a>Salida de registro de ejemplo

Este es un ejemplo de la salida de la `Debug` consola en el nivel de un servicio de gRPC:

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

## <a name="access-server-side-logs"></a>Acceder a los registros del lado servidor

La forma de acceder a los registros del lado servidor depende del entorno en el que se está ejecutando.

### <a name="as-a-console-app"></a>Como aplicación de consola

Si está ejecutando en una aplicación de consola, el [registrador de consola](xref:fundamentals/logging/index#console-provider) debe estar habilitado de forma predeterminada. los registros de gRPC aparecerán en la consola de.

### <a name="other-environments"></a>Otros entornos

Si la aplicación se implementa en otro entorno (por ejemplo, Docker, Kubernetes o servicio de Windows), consulte <xref:fundamentals/logging/index> para obtener más información sobre cómo configurar los proveedores de registro adecuados para el entorno.

## <a name="grpc-client-logging"></a>registro de cliente de gRPC

> [!WARNING]
> Los registros del lado cliente pueden contener información confidencial de la aplicación. No publique **nunca** los registros sin procesar de las aplicaciones de producción en foros públicos como github.

Para obtener los registros del cliente .net, puede establecer la `GrpcChannelOptions.LoggerFactory` propiedad cuando se crea el canal del cliente. Si está llamando a un servicio de gRPC desde una aplicación ASP.NET Core, el generador del registrador se puede resolver desde la inserción de dependencias (DI):

[!code-csharp[](diagnostics/sample/net-client-dependency-injection.cs?highlight=7,16)]

Una manera alternativa de habilitar el registro de cliente es usar el [generador de cliente de gRPC](xref:grpc/clientfactory) para crear el cliente. Un cliente de gRPC registrado con el generador de cliente y resuelto desde DI usará automáticamente el registro configurado de la aplicación.

Si la aplicación no usa di, puede crear una nueva `ILoggerFactory` instancia con [LoggerFactory. Create](xref:Microsoft.Extensions.Logging.LoggerFactory.Create*). Para tener acceso a este método, agregue el paquete [Microsoft. Extensions. Logging](https://www.nuget.org/packages/microsoft.extensions.logging/) a la aplicación.

[!code-csharp[](diagnostics/sample/net-client-loggerfactory-create.cs?highlight=1,8)]

### <a name="sample-logging-output"></a>Salida de registro de ejemplo

Este es un ejemplo de la salida de la `Debug` consola en el nivel de un cliente de gRPC:

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

## <a name="additional-resources"></a>Recursos adicionales

* <xref:fundamentals/logging/index>
* <xref:grpc/configuration>
* <xref:grpc/clientfactory>
