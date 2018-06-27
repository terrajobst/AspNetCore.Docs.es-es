---
title: Configuración de ASP.NET SignalR Core
author: rachelappel
description: Obtenga información acerca de cómo configurar aplicaciones ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/30/2018
uid: signalr/configuration
ms.openlocfilehash: 167b8828efd093d755aeb1c91e080dbd22b5d485
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961991"
---
# <a name="aspnet-core-signalr-configuration"></a>Configuración de ASP.NET SignalR Core

## <a name="jsonmessagepack-serialization-options"></a>Opciones de serialización de JSON/MessagePack

ASP.NET Core SignalR admite dos protocolos para la codificación de mensajes: [JSON](https://www.json.org/) y [MessagePack](https://msgpack.org/index.html). Cada protocolo tiene opciones de configuración de serialización.

Serialización de JSON se puede configurar en el servidor mediante el [ `AddJsonProtocol` ](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) método de extensión, que se puede agregar después [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) en su `Startup.ConfigureServices` método. El `AddJsonProtocol` método toma un delegado que recibe un `options` objeto. El [ `PayloadSerializerSettings` ](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) propiedad en ese objeto es un JSON.NET `JsonSerializerSettings` objeto que puede usarse para configurar la serialización de argumentos y valores devueltos. Consulte la [JSON.NET documentación](https://www.newtonsoft.com/json/help/html/Introduction.htm) para obtener más detalles.

Por ejemplo, para configurar el serializador para usar nombres de propiedad "PascalCase", en lugar de los nombres de "camelCase" predeterminados, use el código siguiente:

```csharp
services.AddSignalR()
    .AddJsonHubProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
    });
```

En el cliente. NET, la misma `AddJsonHubProtocol` método de extensión exista en [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder). El `Microsoft.Extensions.DependencyInjection` se debe importar el espacio de nombres para resolver el método de extensión:

```csharp
// At the top of the file:
using Microsoft.Extensions.DependencyInjection;

// When constructing your connection:
var connection = new HubConnectionBuilder()
.AddJsonHubProtocol(options => {
    options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
});
```

> [!NOTE]
> No es posible configurar la serialización de JSON en el cliente de JavaScript en este momento.

### <a name="messagepack-serialization-options"></a>Opciones de serialización MessagePack

Se puede configurar la serialización MessagePack proporcionando un delegado para la [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) llamar. Vea [MessagePack en SignalR](xref:signalr/messagepackhubprotocol) para obtener más detalles.

> [!NOTE]
> No es posible configurar la serialización de MessagePack en el cliente de JavaScript en este momento.

## <a name="configure-server-options"></a>Configurar opciones de servidor

En la tabla siguiente se describe opciones para configurar los concentradores SignalR:

| Opción | Descripción |
| ------ | ----------- |
| `HandshakeTimeout` | Si el cliente no envía un mensaje de protocolo de enlace inicial dentro de este intervalo de tiempo, se cierra la conexión. |
| `KeepAliveInterval` | Si el servidor no ha enviado un mensaje dentro de este intervalo, se envía automáticamente un mensaje de ping para mantener abierta la conexión. |
| `SupportedProtocols` | Protocolos admitidos por este concentrador. De forma predeterminada, se permiten todos los protocolos registrados en el servidor pero, protocolos se pueden quitar de esta lista para deshabilitar los protocolos concretos para los concentradores individuales. |
| `EnableDetailedErrors` | Si `true`y detallados se devuelven mensajes de excepción a los clientes cuando se produce una excepción en un método de concentrador. El valor predeterminado es `false`, ya que estos mensajes de excepción pueden contener información confidencial. |

Opciones pueden configurarse para todos los centros proporcionando un delegado de opciones para la `AddSignalR` llamar a en `Startup.ConfigureServices`.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSignalR(hubOptions =>
    {
        hubOptions.EnableDetailedErrors = true;
        hubOptions.KeepAliveInterval = TimeSpan.FromMinutes(1);
    })
}
```

Opciones para un único concentrador invalidan las opciones globales de `AddSignalR` y pueden configurarse mediante [ `AddHubOptions<T>` ](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
}
```

Use `HttpConnectionDispatcherOptions` para configurar opciones avanzadas relacionadas con transportes y administración de búfer de memoria. Estas opciones se configuran pasando un delegado para [ `MapHub<T>` ](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).

| Opción | Descripción |
| ------ | ----------- |
| `ApplicationMaxBufferSize` | El número máximo de bytes recibidos desde el cliente que los búferes del servidor. Al aumentar este valor permite que el servidor recibir mensajes más grandes, pero puede repercutir negativamente en el consumo de memoria. El valor predeterminado es 32KB. |
| `AuthorizationData` | Una lista de [ `IAuthorizeData` ](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objetos que se usan para determinar si un cliente está autorizado para conectarse al concentrador. De forma predeterminada, esto se rellenará con valores de la `Authorize` atributos aplicados a la clase de base de datos central. |
| `TransportMaxBufferSize` | El número máximo de bytes enviados por la aplicación que los búferes del servidor. Al aumentar este valor permite que el servidor enviar mensajes más grandes, pero puede repercutir negativamente en el consumo de memoria. El valor predeterminado es 32KB. |
| `Transports` | Máscara de bits de `HttpTransportType` valores que pueden restringir los transportes que un cliente puede utilizar para conectarse. Todos los transportes están habilitados de forma predeterminada. |
| `LongPolling` | Opciones adicionales específicas para el transporte de sondeo largo. |
| `WebSockets` | Opciones adicionales específicas para el transporte de WebSockets. |

El transporte de sondeo largo tiene opciones adicionales que se pueden configurar mediante el `LongPolling` propiedad:

| Opción | Descripción |
| ------ | ----------- |
| `PollTimeout` | La cantidad máxima de tiempo que el servidor espera un mensaje enviar al cliente antes de terminar una solicitud de sondeo sencillo. Al disminuir este valor hace que el cliente emitir solicitudes de sondeo de nuevo con más frecuencia. El valor predeterminado es de 90 segundos. |

El transporte de WebSocket tiene opciones adicionales que se pueden configurar mediante el `WebSockets` propiedad:

| Opción | Descripción |
| ------ | ----------- |
| `CloseTimeout` | Después de cerrar el servidor, si se produce un error en el cliente cerrar dentro de este intervalo de tiempo, se termina la conexión. |
| `SubProtocolSelector` | Un delegado que puede usarse para establecer el `Sec-WebSocket-Protocol` encabezado en un valor personalizado. El delegado recibe los valores solicitados por el cliente como entrada y se espera que devuelva el valor deseado. |

## <a name="configure-client-options"></a>Configurar las opciones de cliente

Opciones de cliente pueden configurarse en el `HubConnectionBuilder` tipo (disponible en los clientes de .NET y JavaScript), así como de la `HubConnection` propio.

### <a name="configure-logging"></a>Configurar el registro

El registro está configurado en el cliente de .NET mediante la `ConfigureLogging` método. Registro de proveedores y los filtros se puede registrar en la misma manera, tal como están en el servidor. Consulte la [inicio de sesión ASP.NET Core](xref:fundamentals/logging/index#how-to-add-providers) documentación para obtener más información.

> [!NOTE]
> Para registrar los proveedores de registro, debe instalar los paquetes necesarios. Consulte la [proveedores de registro integrados](xref:fundamentals/logging/index#built-in-logging-providers) sección de la documentación para obtener una lista completa.

Por ejemplo, para habilitar el registro de la consola, instale el `Microsoft.Extensions.Logging.Console` paquete NuGet. Llame a la `AddConsole` método de extensión:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

En el cliente de JavaScript, un proceso similar `configureLogging` método existe. Proporcionar un `LogLevel` valor que indica el nivel mínimo de mensajes de registro que se va a generar. Los registros se escriben en la ventana de consola del explorador.

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> Para deshabilitar completamente el registro, especifique `signalR.LogLevel.None` en el `configureLogging` método.

Los niveles de registro disponibles para el cliente de JavaScript se muestran a continuación. Establecer el nivel de registro en uno de estos valores habilita el registro de mensajes en **o superior** ese nivel.

| Nivel | Descripción |
| ----- | ----------- |
| `None` | Se registra ningún mensaje. |
| `Critical` | Mensajes que indican un error en toda la aplicación. |
| `Error` | Mensajes que indican un error en la operación actual. |
| `Warning` | Mensajes que indican un problema grave. |
| `Information` | Mensajes informativos. |
| `Debug` | Mensajes de diagnóstico útiles para depurar. |
| `Trace` | Mensajes de diagnóstico muy detallados diseñados para diagnosticar problemas específicos. |

### <a name="configure-allowed-transports"></a>Configurar transportes permitidos

Los transportes utilizados por SignalR pueden configurarse en el `WithUrl` llamar (`withUrl` en JavaScript). Una operación OR bit a bit de los valores de `HttpTransportType` puede usarse para restringir el cliente para usar únicamente los transportes especificados. Todos los transportes están habilitados de forma predeterminada.

Por ejemplo, para deshabilitar el transporte Server-Sent eventos, pero permitir conexiones WebSockets y sondeo largo:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

En el cliente de JavaScript, se configuran los transportes estableciendo la `transport` campo en el objeto de opciones proporcionado para `withUrl`:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a>Configurar la autenticación de portador

Para proporcionar datos de autenticación junto con las solicitudes de SignalR, utilice el `AccessTokenProvider` opción (`accessTokenFactory` en JavaScript) para especificar una función que devuelve el token de acceso deseado. En el cliente. NET, este token de acceso se pasa como un HTTP "Autenticación del portador" token (mediante el `Authorization` encabezado con un tipo de `Bearer`). En el cliente de JavaScript, se utiliza el token de acceso como un token de portador, **excepto** en algunos casos donde explorador API restringir la capacidad de aplicar encabezados (específicamente, en las solicitudes de eventos de Server-Sent y WebSockets). En estos casos, el token de acceso se proporciona como un valor de cadena de consulta `access_token`.

En el cliente. NET, el `AccessTokenProvider` opción se puede especificar utilizando el delegado de opciones en `WithUrl`:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        }
    })
    .Build();
```

En el cliente de JavaScript, el token de acceso se configura al establecer el `accessTokenFactory` campo en el objeto de opciones de `withUrl`:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        accessTokenFactory: () => {
            // Get and return the access token.
            // This function can return a JavaScript Promise if asynchronous
            // logic is required to retrieve the access token.
        }
    })
    .build();
```

### <a name="configure-timeout-and-keep-alive-options"></a>Configurar tiempo de espera y las opciones de mantenimiento

Opciones adicionales para configurar el tiempo de espera y el comportamiento de mantenimiento están disponibles en la `HubConnection` propio objeto:

| Opción de .NET | Opción de JavaScript | Descripción |
| ----------- | ----------------- | ----------- |
| `ServerTimeout` | `serverTimeoutInMilliseconds` | Tiempo de espera para la actividad del servidor. Si el servidor no ha enviado ningún mensaje en este intervalo, el cliente considera que el servidor desconectado y el desencadenador la `Closed` evento (`onclose` en JavaScript). |
| `HandshakeTimeout` | No se puede configurar | Tiempo de espera para el protocolo de enlace inicial del servidor. Si el servidor no envía una respuesta de protocolo de enlace de este intervalo, el cliente cancela el protocolo de enlace y el desencadenador la `Closed` evento (`onclose` en JavaScript). |

En el cliente. NET, se especifican los valores de tiempo de espera como `TimeSpan` valores. En el cliente de JavaScript, los valores de tiempo de espera se especifican como números. Los números representan valores de tiempo en milisegundos.

### <a name="configure-additional-options"></a>Configurar opciones adicionales

Se pueden configurar opciones adicionales en el `WithUrl` (`withUrl` en JavaScript) método en `HubConnectionBuilder`:

| Opción de .NET | Opción de JavaScript | Descripción |
| ----------- | ----------------- | ----------- |
| `AccessTokenProvider` | `accessTokenFactory` | Una función que devuelve una cadena que se proporciona como un token de autenticación de portador en solicitudes HTTP. |
| `SkipNegotiation` | `skipNegotiation` | Establezca esta propiedad en `true` para omitir el paso de negociación. **Solo se admite cuando el transporte de WebSockets es el único tipo de transporte habilitado**. No se puede habilitar esta configuración cuando se usa el servicio de SignalR de Azure. |
| `ClientCertificates` | No se puede configurar * | Una colección de certificados TLS para enviar autenticar las solicitudes. |
| `Cookies` | No se puede configurar * | Una colección de cookies HTTP para enviar con cada solicitud HTTP. |
| `Credentials` | No se puede configurar * | Credenciales que se envían con cada solicitud HTTP. |
| `CloseTimeout` | No se puede configurar * | Sólo WebSockets. La cantidad máxima de tiempo de espera a que el cliente después de cierre para el servidor confirmar la solicitud de cierre. Si el servidor no reconoció el cierre dentro de este tiempo, el cliente se desconecta. |
| `Headers` | No se puede configurar * | Un diccionario de encabezados HTTP adicionales que se envían con cada solicitud HTTP. |
| `HttpMessageHandlerFactory` | No se puede configurar * | Un delegado que puede usarse para configurar o reemplazar la `HttpMessageHandler` usado para enviar solicitudes HTTP. No se utiliza para las conexiones de WebSocket. Este delegado debe devolver un valor distinto de null, y recibe el valor predeterminado como un parámetro. Modificar configuración de en el valor predeterminado y devolverlo o devolver un completamente nuevo `HttpMessageHandler` instancia. |
| `Proxy` | No se puede configurar * | Un proxy HTTP que se utilizará al enviar solicitudes HTTP. |
| `UseDefaultCredentials` | No se puede configurar * | Establezca este valor booleano para enviar las credenciales predeterminadas para las solicitudes HTTP y WebSockets. Esto permite el uso de autenticación de Windows. |
| `WebSocketConfiguration` | No se puede configurar * | Un delegado que puede usarse para configurar opciones adicionales de WebSocket. Recibe una instancia de [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) que se puede utilizar para configurar las opciones. |

Opciones marcados con un asterisco (*) no son configurables en el cliente de JavaScript, debido a las limitaciones en las API del explorador.

En el cliente. NET, estas opciones se pueden modificar por el delegado de opciones proporcionado para `WithUrl`:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

En el cliente de JavaScript, estas opciones pueden proporcionarse en un objeto de JavaScript que se proporcionan para `withUrl`:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    });
    .build();
```

## <a name="additional-resources"></a>Recursos adicionales

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
