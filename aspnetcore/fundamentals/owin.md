---
title: Interfaz web abierta para .NET (OWIN)
author: ardalis
description: "Descubra cómo ASP.NET Core es compatible con la interfaz Web abierta para .NET (OWIN), que permite a las aplicaciones web se desacople de servidores web."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/owin
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e819037e2ebd1566c778879516e20de8dc7603ea
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
# <a name="introduction-to-open-web-interface-for-net-owin"></a>Introducción a abrir la interfaz Web para .NET (OWIN)

Por [Steve Smith](https://ardalis.com/) y [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core es compatible con la interfaz web abierta para .NET (OWIN). OWIN permite que las aplicaciones web se desacoplen de los servidores web. Define un método estándar para middleware para usarse en una canalización para controlar las solicitudes y respuestas asociadas. El software intermedio y las aplicaciones de ASP.NET Core pueden interoperar con middleware, servidores y aplicaciones basadas en OWIN.

OWIN proporciona una capa de desacoplamiento que permite a dos marcos con modelos de objetos dispares para utilizarse conjuntamente. El `Microsoft.AspNetCore.Owin` paquete proporciona dos implementaciones del adaptador:
- Núcleo de ASP.NET para OWIN 
- OWIN a ASP.NET Core

Esto permite que ASP.NET Core se hospede en un servidor compatible con OWIN/host, o para otros componentes compatibles de OWIN para ejecutarse sobre ASP.NET Core.

Nota: Uso de estos adaptadores viene con un costo de rendimiento. Las aplicaciones que usan solo los componentes principales de ASP.NET no deben usar el paquete de Owin o adaptadores.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

## <a name="running-owin-middleware-in-the-aspnet-pipeline"></a>Ejecuta el middleware de OWIN de la canalización ASP.NET

Compatibilidad con las OWIN del núcleo de ASP.NET se implementa como parte de la `Microsoft.AspNetCore.Owin` paquete. Puede importar compatibilidad con OWIN en el proyecto mediante la instalación de este paquete.

Middleware de OWIN que se ajusta a la [especificación de OWIN](http://owin.org/spec/spec/owin-1.0.0.html), lo que requiere un `Func<IDictionary<string, object>, Task>` interfaz y determinadas claves establecerse (como `owin.ResponseBody`). El siguiente middleware de OWIN simple muestra "Hello World":

```csharp
public Task OwinHello(IDictionary<string, object> environment)
{
    string responseText = "Hello World via OWIN";
    byte[] responseBytes = Encoding.UTF8.GetBytes(responseText);

    // OWIN Environment Keys: http://owin.org/spec/spec/owin-1.0.0.html
    var responseStream = (Stream)environment["owin.ResponseBody"];
    var responseHeaders = (IDictionary<string, string[]>)environment["owin.ResponseHeaders"];

    responseHeaders["Content-Length"] = new string[] { responseBytes.Length.ToString(CultureInfo.InvariantCulture) };
    responseHeaders["Content-Type"] = new string[] { "text/plain" };

    return responseStream.WriteAsync(responseBytes, 0, responseBytes.Length);
}
```

La firma de ejemplo devuelve un `Task` y acepta un `IDictionary<string, object>` según lo solicitado por OWIN.

El código siguiente muestra cómo agregar la `OwinHello` middleware (se muestra arriba) a la canalización ASP.NET con la `UseOwin` método de extensión.

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseOwin(pipeline =>
    {
        pipeline(next => OwinHello);
    });
}
```

Puede configurar otras acciones para tener lugar dentro de la canalización OWIN.

> [!NOTE]
> Encabezados de respuesta sólo deben modificarse antes de la primera escritura en la secuencia de respuesta.

> [!NOTE]
> Varias llamadas a `UseOwin` se desaconseja por motivos de rendimiento. Componentes de OWIN funcionará mejor si no se agrupan juntos.

```csharp
app.UseOwin(pipeline =>
{
    pipeline(next =>
    {
        // do something before
        return OwinHello;
        // do something after
    });
});
```

<a name="hosting-on-owin"></a>

## <a name="using-aspnet-hosting-on-an-owin-based-server"></a>Uso de hospedaje de ASP.NET en un servidor basado en OWIN

Servidores basados en OWIN pueden hospedar aplicaciones ASP.NET. Un servidor de este tipo es [Nowin](https://github.com/Bobris/Nowin), un servidor web de .NET OWIN. En el ejemplo de este artículo, he incluido un proyecto que hace referencia a Nowin y lo usa para crear un `IServer` capaz de autohospedaje ASP.NET Core.

[!code-csharp[Main](owin/sample/src/NowinSample/Program.cs?highlight=15)]

`IServer`es una interfaz que requiera un `Features` propiedad y un `Start` método.

`Start`es responsable de configurar e iniciar el servidor, que en este caso, se realiza a través de una serie de llamadas API fluidas que establecer direcciones analizadas desde el IServerAddressesFeature. Tenga en cuenta que la configuración fluida de la `_builder` variable especifica que las solicitudes se controlarán mediante el `appFunc` definido anteriormente en el método. Esto `Func` se llama en cada solicitud para procesar las solicitudes entrantes.

Agregaremos también un `IWebHostBuilder` extensión para que sea fácil de agregar y configurar el servidor Nowin.

```csharp
using System;
using Microsoft.AspNetCore.Hosting.Server;
using Microsoft.Extensions.DependencyInjection;
using Nowin;
using NowinSample;

namespace Microsoft.AspNetCore.Hosting
{
    public static class NowinWebHostBuilderExtensions
    {
        public static IWebHostBuilder UseNowin(this IWebHostBuilder builder)
        {
            return builder.ConfigureServices(services =>
            {
                services.AddSingleton<IServer, NowinServer>();
            });
        }

        public static IWebHostBuilder UseNowin(this IWebHostBuilder builder, Action<ServerBuilder> configure)
        {
            builder.ConfigureServices(services =>
            {
                services.Configure(configure);
            });
            return builder.UseNowin();
        }
    }
}
```

Teniendo esto en su lugar, todo lo que se necesita para ejecutar una aplicación de ASP.NET mediante este servidor personalizada para llamar a la extensión en *Program.cs*:

```csharp

using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Hosting;

namespace NowinSample
{
    public class Program
    {
        public static void Main(string[] args)
        {
            var host = new WebHostBuilder()
                .UseNowin()
                .UseContentRoot(Directory.GetCurrentDirectory())
                .UseIISIntegration()
                .UseStartup<Startup>()
                .Build();

            host.Run();
        }
    }
}

```

Obtener más información sobre ASP.NET [servidores](servers/index.md).

## <a name="run-aspnet-core-on-an-owin-based-server-and-use-its-websockets-support"></a>Ejecute ASP.NET Core en un servidor basado en OWIN y utilice su compatibilidad con WebSockets

Otro ejemplo de características de servidores basada en OWIN cómo se puede utilizar con ASP.NET Core es el acceso a características como WebSockets. El servidor de web .NET OWIN usado en el ejemplo anterior es compatible con Sockets Web integrada, que se pueden utilizar con una aplicación de ASP.NET Core. El ejemplo siguiente muestra una aplicación web simple que admite Sockets Web y vuelve de todo lo que se envían al servidor a través de WebSockets.

```csharp
public class Startup
{
    public void Configure(IApplicationBuilder app)
    {
        app.Use(async (context, next) =>
        {
            if (context.WebSockets.IsWebSocketRequest)
            {
                WebSocket webSocket = await context.WebSockets.AcceptWebSocketAsync();
                await EchoWebSocket(webSocket);
            }
            else
            {
                await next();
            }
        });

        app.Run(context =>
        {
            return context.Response.WriteAsync("Hello World");
        });
    }

    private async Task EchoWebSocket(WebSocket webSocket)
    {
        byte[] buffer = new byte[1024];
        WebSocketReceiveResult received = await webSocket.ReceiveAsync(
            new ArraySegment<byte>(buffer), CancellationToken.None);

        while (!webSocket.CloseStatus.HasValue)
        {
            // Echo anything we receive
            await webSocket.SendAsync(new ArraySegment<byte>(buffer, 0, received.Count), 
                received.MessageType, received.EndOfMessage, CancellationToken.None);

            received = await webSocket.ReceiveAsync(new ArraySegment<byte>(buffer), 
                CancellationToken.None);
        }

        await webSocket.CloseAsync(webSocket.CloseStatus.Value, 
            webSocket.CloseStatusDescription, CancellationToken.None);
    }
}
```

Esto [ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) está configurado con el mismo `NowinServer` como lo anterior: la única diferencia es cómo está configurada la aplicación en su `Configure` método. Una prueba mediante [un cliente simple websocket](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en) se muestra la aplicación:

![Cliente de prueba de Socket Web](owin/_static/websocket-test.png)

## <a name="owin-environment"></a>Entorno OWIN

Puede construir un entorno de OWIN utilizando el `HttpContext`.

```csharp

   var environment = new OwinEnvironment(HttpContext);
   var features = new OwinFeatureCollection(environment);
   ```

## <a name="owin-keys"></a>Claves OWIN

Depende de OWIN un `IDictionary<string,object>` objeto para comunicar la información a lo largo de un intercambio de solicitud/respuesta HTTP. ASP.NET Core implementa las claves que se enumeran a continuación. Consulte la [especificación principal, las extensiones de](http://owin.org/#spec), y [OWIN clave directrices y claves comunes](http://owin.org/spec/spec/CommonKeys.html).

### <a name="request-data-owin-v100"></a>Datos de la solicitud (OWIN v1.0.0)

| Key               | Valor (tipo) | Descripción |
| ----------------- | ------------ | ----------- |
| owin.RequestScheme | `String` |  |
| owin. RequestMethod  | `String` | |    
| owin.RequestPathBase  | `String` | |    
| owin. RequestPath | `String` | |     
| owin.RequestQueryString  | `String` | |    
| owin.RequestProtocol  | `String` | |    
| owin.RequestHeaders | `IDictionary<string,string[]>`  | |
| owin. RequestBody | `Stream`  | |

### <a name="request-data-owin-v110"></a>Datos de la solicitud (OWIN v1.1.0)

| Key               | Valor (tipo) | Descripción |
| ----------------- | ------------ | ----------- |
| owin. RequestId | `String` | Optional |

### <a name="response-data-owin-v100"></a>Datos de respuesta (OWIN v1.0.0)

| Key               | Valor (tipo) | Descripción |
| ----------------- | ------------ | ----------- |
| owin.ResponseStatusCode | `int` | Optional |
| owin. ResponseReasonPhrase | `String` | Optional |
| owin. ResponseHeaders | `IDictionary<string,string[]>`  | |
| owin. ResponseBody | `Stream`  | |


### <a name="other-data-owin-v100"></a>Otros datos (OWIN v1.0.0)

| Key               | Valor (tipo) | Descripción |
| ----------------- | ------------ | ----------- |
| owin. CallCancelled | `CancellationToken` |  |
| owin. Versión  | `String` | |   


### <a name="common-keys"></a>Claves comunes

| Key               | Valor (tipo) | Descripción |
| ----------------- | ------------ | ----------- |
| ssl.ClientCertificate | `X509Certificate` |  |
| ssl.LoadClientCertAsync  | `Func<Task>` | |    
| server.RemoteIpAddress  | `String` | |    
| server.RemotePort | `String` | |     
| server.LocalIpAddress  | `String` | |    
| server.LocalPort  | `String` | |    
| server.IsLocal  | `bool` | |    
| servidor. OnSendingHeaders  | `Action<Action<object>,object>` | |


### <a name="sendfiles-v030"></a>SendFiles v0.3.0

| Key               | Valor (tipo) | Descripción |
| ----------------- | ------------ | ----------- |
| sendfile.SendAsync | Vea [firma de delegado](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) | Por solicitud |


### <a name="opaque-v030"></a>V0.3.0 opaco

| Key               | Valor (tipo) | Descripción |
| ----------------- | ------------ | ----------- |
| opaque.Version | `String` |  |
| opaque.Upgrade | `OpaqueUpgrade` | Vea [firma de delegado](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) |
| opaque.Stream | `Stream` |  |
| opaque.CallCancelled | `CancellationToken` |  |


### <a name="websocket-v030"></a>WebSocket v0.3.0

| Key               | Valor (tipo) | Descripción |
| ----------------- | ------------ | ----------- |
| websocket.Version | `String` |  |
| websocket.Accept | `WebSocketAccept` | Vea [firma de delegado](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) |
| websocket.AcceptAlt |  | Especificación no |
| websocket.SubProtocol | `String` | Vea [RFC6455 sección 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2) paso 5.5 |
| websocket.SendAsync | `WebSocketSendAsync` | Vea [firma de delegado](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)  |
| websocket.ReceiveAsync | `WebSocketReceiveAsync` | Vea [firma de delegado](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)  |
| websocket.CloseAsync | `WebSocketCloseAsync` | Vea [firma de delegado](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)  |
| websocket.CallCancelled | `CancellationToken` |  |
| websocket.ClientCloseStatus | `int` | Optional |
| websocket.ClientCloseDescription | `String` | Optional |


## <a name="additional-resources"></a>Recursos adicionales

* [Middleware](middleware.md)

* [Servidores](servers/index.md)
