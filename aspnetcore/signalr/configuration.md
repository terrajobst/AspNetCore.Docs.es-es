---
title: Configuración de ASP.NET Core SignalR
author: bradygaster
description: Obtenga información sobre cómo configurar aplicaciones de ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 02/07/2019
uid: signalr/configuration
ms.openlocfilehash: 070d6fed26b6d14c4b8a35d0f7d94abafb08993b
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665420"
---
# <a name="aspnet-core-signalr-configuration"></a>Configuración de ASP.NET Core SignalR

## <a name="jsonmessagepack-serialization-options"></a>Opciones de serialización de JSON/MessagePack

SignalR de ASP.NET Core admite dos protocolos para la codificación de mensajes: [JSON](https://www.json.org/) y [MessagePack](https://msgpack.org/index.html). Cada protocolo tiene opciones de configuración de serialización.

Serialización de JSON se puede configurar en el servidor mediante el [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) método de extensión, que se puede agregar después [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) en su `Startup.ConfigureServices` método. El `AddJsonProtocol` método toma un delegado que recibe un `options` objeto. El [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) propiedad en ese objeto es un JSON.NET `JsonSerializerSettings` objeto que puede usarse para configurar la serialización de argumentos y valores devueltos. Consulte la [documentación JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm) para obtener más detalles.

Por ejemplo, para configurar el serializador para usar nombres de propiedad "PascalCase", en lugar de los nombres predeterminados "camelCase", use el código siguiente:

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
    });
```

En el cliente. NET, la misma `AddJsonProtocol` existe en el método de extensión [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder). El `Microsoft.Extensions.DependencyInjection` se debe importar el espacio de nombres para resolver el método de extensión:

```csharp
// At the top of the file:
using Microsoft.Extensions.DependencyInjection;

// When constructing your connection:
var connection = new HubConnectionBuilder()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver = 
            new DefaultContractResolver();
    })
    .Build();
```

> [!NOTE]
> No es posible configurar la serialización de JSON en el cliente de JavaScript en este momento.

### <a name="messagepack-serialization-options"></a>Opciones de serialización MessagePack

MessagePack serialización se puede configurar si se proporciona un delegado para el [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) llamar. Consulte [MessagePack en SignalR](xref:signalr/messagepackhubprotocol) para obtener más detalles.

> [!NOTE]
> No es posible configurar la serialización de MessagePack en el cliente de JavaScript en este momento.

## <a name="configure-server-options"></a>Configurar opciones de servidor

En la tabla siguiente se describe las opciones para configurar los concentradores de SignalR:

| Opción | Valor predeterminado | Descripción |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | 30 segundos | El servidor tendrá en cuenta el cliente desconectado si no ha recibido un mensaje (incluido keep-alive) en este intervalo. Puede tardar más de este intervalo de tiempo de espera para el cliente realmente marcarse desconectado debido a su implementación. El valor recomendado es doble el `KeepAliveInterval` valor.|
| `HandshakeTimeout` | 15 segundos | Si el cliente no envía un mensaje de protocolo de enlace inicial dentro de este intervalo de tiempo, se cierra la conexión. Se trata de una opción avanzada que sólo debería modificarse si se producen errores de tiempo de espera del protocolo de enlace debido a la latencia de red graves. Para obtener más detalles sobre el proceso de negociación, consulte el [especificación del protocolo SignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |
| `KeepAliveInterval` | 15 segundos | Si el servidor no ha enviado un mensaje dentro de este intervalo, se envía automáticamente un mensaje ping para mantener abierta la conexión. Al cambiar `KeepAliveInterval`, cambie el `ServerTimeout` / `serverTimeoutInMilliseconds` configuración del cliente. Recomendado `ServerTimeout` / `serverTimeoutInMilliseconds` valor es doble el `KeepAliveInterval` valor.  |
| `SupportedProtocols` | Todos los protocolos instalados | Protocolos admitidos por este concentrador. De forma predeterminada, se permiten todos los protocolos registrados en el servidor, pero se pueden quitar protocolos de esta lista para deshabilitar los protocolos específicos para los concentradores individuales. |
| `EnableDetailedErrors` | `false` | Si `true`detallados se devuelven los mensajes de excepción a los clientes cuando se produce una excepción en un método de concentrador. El valor predeterminado es `false`, ya que estos mensajes de excepción pueden contener información confidencial. |

Se pueden configurar opciones para todos los centros proporcionando un delegado de opciones para la `AddSignalR` llamar `Startup.ConfigureServices`.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSignalR(hubOptions =>
    {
        hubOptions.EnableDetailedErrors = true;
        hubOptions.KeepAliveInterval = TimeSpan.FromMinutes(1);
    });
}
```

Opciones para un único centro de invalidan las opciones globales de `AddSignalR` y pueden configurarse mediante <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a>Opciones de configuración de HTTP avanzadas

Use `HttpConnectionDispatcherOptions` para configurar opciones avanzadas relacionadas con transportes y administración de búfer de memoria. Estas opciones se configuran pasando un delegado para [MapHub\<T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) en `Startup.Configure`.

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseSignalR((configure) => 
    {
        var desiredTransports = 
            HttpTransportType.WebSockets |
            HttpTransportType.LongPolling;

        configure.MapHub<MyHub>("/myhub", (options) => 
        {
            options.Transports = desiredTransports;
        });
    });
}
```

En la tabla siguiente se describe las opciones para configurar las opciones avanzadas de HTTP de ASP.NET Core SignalR:

| Opción | Valor predeterminado | Descripción |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | 32 KB | El número máximo de bytes recibidos del cliente que los búferes del servidor. Al aumentar este valor permite que el servidor recibir los mensajes más grandes, pero puede repercutir negativamente en el consumo de memoria. |
| `AuthorizationData` | Recopilar automáticamente los datos de la `Authorize` atributos aplicados a la clase de concentrador. | Una lista de [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) los objetos utilizados para determinar si un cliente está autorizado para conectarse al concentrador. |
| `TransportMaxBufferSize` | 32 KB | El número máximo de bytes enviados por la aplicación que los búferes del servidor. Al aumentar este valor permite que el servidor enviar los mensajes más grandes, pero puede repercutir negativamente en el consumo de memoria. |
| `Transports` | Se habilitan todos los transportes. | Una máscara de bits de `HttpTransportType` valores que pueden restringir los transportes que un cliente puede usar para conectarse. |
| `LongPolling` | Véalo a continuación. | Opciones adicionales específicas para el transporte de sondeo largo. |
| `WebSockets` | Véalo a continuación. | Opciones adicionales específicas para el transporte de WebSockets. |

El transporte de sondeo largo tiene opciones adicionales que se pueden configurar mediante el `LongPolling` propiedad:

| Opción | Valor predeterminado | Descripción |
| ------ | ------------- | ----------- |
| `PollTimeout` | 90 segundos | La cantidad máxima de tiempo que el servidor espera para que un mensaje para enviárselo al cliente antes de finalizar una solicitud de sondeo única. Al disminuir este valor hace que el cliente emitir solicitudes de sondeo de nuevo con más frecuencia. |

El transporte de WebSocket tiene opciones adicionales que se pueden configurar mediante el `WebSockets` propiedad:

| Opción | Valor predeterminado | Descripción |
| ------ | ------------- | ----------- |
| `CloseTimeout` | 5 segundos | Después de cerrar el servidor, si se produce un error en el cliente cerrar dentro de este intervalo de tiempo, se termina la conexión. |
| `SubProtocolSelector` | `null` | Un delegado que puede usarse para establecer el `Sec-WebSocket-Protocol` encabezado en un valor personalizado. El delegado recibe los valores solicitados por el cliente como entrada y se espera que devuelva el valor deseado. |

## <a name="configure-client-options"></a>Configurar las opciones de cliente

Se pueden configurar las opciones de cliente en el `HubConnectionBuilder` tipo (disponible en los clientes de .NET y JavaScript). También está disponible en el cliente de Java, pero la `HttpHubConnectionBuilder` subclase es lo que contiene las opciones de configuración de generador, así como en el `HubConnection` propio.

### <a name="configure-logging"></a>Configurar el registro

El registro está configurado en el cliente de .NET mediante el `ConfigureLogging` método. Registro de proveedores y los filtros se puede registrar en la misma manera, tal como están en el servidor. Consulte la [registro en ASP.NET Core](xref:fundamentals/logging/index) documentación para obtener más información.

> [!NOTE]
> Con el fin de registrar los proveedores de registro, debe instalar los paquetes necesarios. Consulte la [proveedores de registro integrados](xref:fundamentals/logging/index#built-in-logging-providers) sección de la documentación para obtener una lista completa.

Por ejemplo, para habilitar el registro de consola, instale el `Microsoft.Extensions.Logging.Console` paquete NuGet. Llame a la `AddConsole` método de extensión:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

En el cliente de JavaScript, un proceso similar `configureLogging` método existe. Proporcione un `LogLevel` valor que indica el nivel mínimo de los mensajes de registro para generar. Los registros se escriben en la ventana de consola del explorador.

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> Para deshabilitar el registro por completo, especifique `signalR.LogLevel.None` en el `configureLogging` método.

Para obtener más información sobre el registro, consulte el [documentación de diagnóstico de SignalR](xref:signalr/diagnostics).

El cliente de SignalR Java usa el [SLF4J](https://www.slf4j.org/) biblioteca para el registro. Es una API de alto nivel de registro que permite a los usuarios de la biblioteca elegir su propia implementación de registro específico al incorporar una dependencia de registro específico. El fragmento de código siguiente muestra cómo usar `java.util.logging` con el cliente de SignalR Java.

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

Si no configura el registro de las dependencias, SLF4J carga un registrador de ninguna operación de forma predeterminada con el mensaje de advertencia siguiente:

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

Esto puede pasar por alto.

### <a name="configure-allowed-transports"></a>Configurar transportes permitidos

Los transportes utilizados por SignalR se pueden configurar en el `WithUrl` llamar (`withUrl` en JavaScript). Una operación OR bit a bit de los valores de `HttpTransportType` puede usarse para restringir el cliente para que solo use los transportes especificados. Todos los transportes están habilitados de forma predeterminada.

Por ejemplo, para deshabilitar el transporte de los eventos, pero permitir conexiones WebSockets y Long Polling:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

En el cliente de JavaScript, se configuran los transportes estableciendo el `transport` campo en el objeto de opciones proporcionado para `withUrl`:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

::: moniker range=">= aspnetcore-2.2"

En esta versión de Java websockets de cliente es el transporte solo está disponible.

::: moniker-end

::: moniker range="= aspnetcore-3.0"

En el cliente de Java, el transporte está activado con la `withTransport` método en el `HttpHubConnectionBuilder`. De forma predeterminada, el cliente de Java que usa el transporte de WebSockets.

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```
> [!NOTE]
> El cliente de SignalR Java todavía no admite transporte de reserva.

::: moniker-end

### <a name="configure-bearer-authentication"></a>Configurar la autenticación de portador

Para proporcionar datos de autenticación junto con las solicitudes de SignalR, use el `AccessTokenProvider` opción (`accessTokenFactory` en JavaScript) para especificar una función que devuelve el token de acceso deseado. En el cliente. NET, este token de acceso se pasa como un HTTP "Autenticación del portador" token (mediante el `Authorization` encabezado con un tipo de `Bearer`). En el cliente de JavaScript, se usa el token de acceso como un token de portador, **excepto** en algunos casos donde las API de explorador restringir la capacidad de aplicar los encabezados (específicamente, en las solicitudes de los eventos y WebSockets). En estos casos, el token de acceso se proporciona como un valor de cadena de consulta `access_token`.

En el cliente. NET, el `AccessTokenProvider` opción puede especificarse utilizando el delegado de opciones en `WithUrl`:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

En el cliente de JavaScript, el token de acceso se configura estableciendo el `accessTokenFactory` campo en el objeto de opciones de `withUrl`:

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


En el cliente de SignalR Java, puede configurar un token de portador a usar para la autenticación mediante un generador de token de acceso a la [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java). Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) para proporcionar una [RxJava](https://github.com/ReactiveX/RxJava) [único<String>](http://reactivex.io/documentation/single.html). Con una llamada a [Single.defer](http://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), puede escribir la lógica para generar tokens de acceso para el cliente.

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a>Configurar el tiempo de espera y las opciones de mantenimiento

Opciones adicionales para configurar el tiempo de espera y el comportamiento de mantenimiento están disponibles en el `HubConnection` propio objeto:

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

| Opción | Valor predeterminado | Descripción |
| ------ | ------------- | ----------- |
| `ServerTimeout` | 30 segundos (30.000 milisegundos) | Tiempo de espera para la actividad del servidor. Si el servidor no ha enviado un mensaje en este intervalo, el cliente considera que las ha desconectado el servidor y los desencadenadores la `Closed` eventos (`onclose` en JavaScript). Este valor debe ser lo suficientemente grande como para un mensaje ping se envíe desde el servidor **y** recibidos por el cliente dentro del intervalo de tiempo de espera. El valor recomendado es de un número al menos el doble del servidor `KeepAliveInterval` valor, para dejar tiempo para pings de llegada. |
| `HandshakeTimeout` | 15 segundos | Tiempo de espera para el protocolo de enlace de servidor inicial. Si el servidor no envía una respuesta de protocolo de enlace en este intervalo, el cliente cancela el protocolo de enlace y los desencadenadores la `Closed` eventos (`onclose` en JavaScript). Se trata de una opción avanzada que sólo debería modificarse si se producen errores de tiempo de espera del protocolo de enlace debido a la latencia de red graves. Para obtener más detalles sobre el proceso de negociación, consulte el [especificación del protocolo SignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |

En el cliente. NET, se especifican los valores de tiempo de espera como `TimeSpan` valores.

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

| Opción | Valor predeterminado | Descripción |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | 30 segundos (30.000 milisegundos) | Tiempo de espera para la actividad del servidor. Si el servidor no ha enviado un mensaje en este intervalo, el cliente considera que las ha desconectado el servidor y los desencadenadores la `onclose` eventos. Este valor debe ser lo suficientemente grande como para un mensaje ping se envíe desde el servidor **y** recibidos por el cliente dentro del intervalo de tiempo de espera. El valor recomendado es de un número al menos el doble del servidor `KeepAliveInterval` valor, para dejar tiempo para pings de llegada. |

# <a name="javatabjava"></a>[Java](#tab/java)

| Opción | Valor predeterminado | Descripción |
| ----------- | ------------- | ----------- |
|`getServerTimeout` `setServerTimeout` | 30 segundos (30.000 milisegundos) | Tiempo de espera para la actividad del servidor. Si el servidor no ha enviado un mensaje en este intervalo, el cliente considera que las ha desconectado el servidor y los desencadenadores la `onClose` eventos. Este valor debe ser lo suficientemente grande como para un mensaje ping se envíe desde el servidor **y** recibidos por el cliente dentro del intervalo de tiempo de espera. El valor recomendado es de un número al menos el doble del servidor `KeepAliveInterval` valor, para dejar tiempo para pings de llegada. |
| `withHandshakeResponseTimeout` | 15 segundos | Tiempo de espera para el protocolo de enlace de servidor inicial. Si el servidor no envía una respuesta de protocolo de enlace en este intervalo, el cliente cancela el protocolo de enlace y los desencadenadores la `onClose` eventos. Se trata de una opción avanzada que sólo debería modificarse si se producen errores de tiempo de espera del protocolo de enlace debido a la latencia de red graves. Para obtener más detalles sobre el proceso de negociación, consulte el [especificación del protocolo SignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |

---

### <a name="configure-additional-options"></a>Configurar opciones adicionales

Se pueden configurar opciones adicionales en el `WithUrl` (`withUrl` en JavaScript) método `HubConnectionBuilder` o en las diversas API de configuración en el `HttpHubConnectionBuilder` en el cliente de Java:

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

| Opción de .NET |  Valor predeterminado | Descripción |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | Una función que devuelve una cadena que se proporciona como un token de autenticación de portador en solicitudes HTTP. |
| `SkipNegotiation` | `false` | Establezca esta opción en `true` para omitir el paso de negociación. **Solo se admite cuando el transporte de WebSockets es el único tipo de transporte habilitado**. No se puede habilitar esta configuración al usar Azure SignalR Service. |
| `ClientCertificates` | Empty | Una colección de certificados TLS que se envían autenticar las solicitudes. |
| `Cookies` | Empty | Una colección de cookies HTTP para enviar con cada solicitud HTTP. |
| `Credentials` | Empty | Credenciales que se enviará con todas las solicitudes HTTP. |
| `CloseTimeout` | 5 segundos | Solo WebSockets. La cantidad máxima de tiempo de espera a que el cliente después del cierre del servidor confirmar la solicitud de cierre. Si el servidor no reconoció el cierre dentro de este tiempo, el cliente se desconecta. |
| `Headers` | Empty | Una asignación de encabezados HTTP adicionales que se enviará con todas las solicitudes HTTP. |
| `HttpMessageHandlerFactory` | `null` | Un delegado que puede usarse para configurar o reemplazar el `HttpMessageHandler` utilizado para enviar solicitudes HTTP. No se utiliza para las conexiones de WebSocket. Este delegado debe devolver un valor distinto de null, y recibe el valor predeterminado como un parámetro. Modificar la configuración en ese valor predeterminado y devolverlo o devolver un nuevo `HttpMessageHandler` instancia. **Cuando Asegúrese de reemplazar el controlador copiar la configuración que desee impedir que el controlador proporcionado, en caso contrario, las opciones configuradas (por ejemplo, Cookies y encabezados) no se aplicarán al nuevo controlador.** |
| `Proxy` | `null` | Un proxy HTTP que se utilizará al enviar solicitudes HTTP. |
| `UseDefaultCredentials` | `false` | Establezca este valor booleano para enviar las credenciales predeterminadas para las solicitudes HTTP y WebSockets. Esto permite el uso de autenticación de Windows. |
| `WebSocketConfiguration` | `null` | Un delegado que puede usarse para configurar opciones adicionales de WebSocket. Recibe una instancia de [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) que puede utilizarse para configurar las opciones. |

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)
| Opción de JavaScript | Valor predeterminado | Descripción |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | Una función que devuelve una cadena que se proporciona como un token de autenticación de portador en solicitudes HTTP. |
| `skipNegotiation` | `false` | Establezca esta opción en `true` para omitir el paso de negociación. **Solo se admite cuando el transporte de WebSockets es el único tipo de transporte habilitado**. No se puede habilitar esta configuración al usar Azure SignalR Service. |

# <a name="javatabjava"></a>[Java](#tab/java)
| Opción de Java | Valor predeterminado | Descripción |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | Una función que devuelve una cadena que se proporciona como un token de autenticación de portador en solicitudes HTTP. |
| `shouldSkipNegotiate` | `false` | Establezca esta opción en `true` para omitir el paso de negociación. **Solo se admite cuando el transporte de WebSockets es el único tipo de transporte habilitado**. No se puede habilitar esta configuración al usar Azure SignalR Service. |
| `withHeader` `withHeaders` | Empty | Una asignación de encabezados HTTP adicionales que se enviará con todas las solicitudes HTTP. |

---

En el cliente. NET, estas opciones se pueden modificar por el delegado de las opciones proporcionado para `WithUrl`:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

En el cliente de JavaScript, estas opciones se pueden proporcionar en un objeto de JavaScript proporcionado a `withUrl`:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

En el cliente de Java, estas opciones pueden configurarse con los métodos en el `HttpHubConnectionBuilder` devuelto desde el `HubConnectionBuilder.create("HUB URL")`


```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a>Recursos adicionales

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
