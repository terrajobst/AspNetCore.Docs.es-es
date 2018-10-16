---
title: Implementación del servidor web Kestrel en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre Kestrel, el servidor web multiplataforma de ASP.NET Core.
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/13/2018
uid: fundamentals/servers/kestrel
ms.openlocfilehash: d6157ac2bdf046c66f4b740ad2263f6b7485c05d
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912311"
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a>Implementación del servidor web Kestrel en ASP.NET Core

Por [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher) y [Stephen Halter](https://twitter.com/halter73)

Kestrel es un [servidor web multiplataforma de ASP.NET Core](xref:fundamentals/servers/index). Kestrel es el servidor web que se incluye de forma predeterminada en las plantillas de proyecto de ASP.NET Core.

Kestrel admite las siguientes características:

::: moniker range=">= aspnetcore-2.2"

* HTTPS
* Actualización opaca para habilitar [WebSockets](https://github.com/aspnet/websockets)
* Sockets de Unix para alto rendimiento detrás de Nginx
* HTTP/2 (excepto en macOS&dagger;)

&dagger;HTTP/2 se admitirá en una versión futura en macOS.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* HTTPS
* Actualización opaca para habilitar [WebSockets](https://github.com/aspnet/websockets)
* Sockets de Unix para alto rendimiento detrás de Nginx

::: moniker-end

Kestrel admite todas las plataformas y versiones que sean compatibles con .NET Core.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

::: moniker range=">= aspnetcore-2.2"

## <a name="http2-support"></a>Compatibilidad con HTTP/2

[HTTP/2](https://httpwg.org/specs/rfc7540.html) está disponible para las aplicaciones de ASP.NET Core si se cumplen los siguientes requisitos básicos:

* Sistema operativo&dagger;
  * Windows Server 2012 R2 o Windows 8.1 o posterior
  * Linux con OpenSSL 1.0.2 o posterior (por ejemplo, Ubuntu 16.04 o posterior)
* Plataforma de destino: .NET Core 2.2 o posterior
* Conexión con [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3)
* Conexión con TLS 1.2 o una versión posterior

&dagger;HTTP/2 se admitirá en una versión futura en macOS.

Si se establece una conexión HTTP/2, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) notifica `HTTP/2`.

HTTP/2 está deshabilitado de manera predeterminada. Para más información sobre la configuración, vea las secciones de [opciones de Kestrel](#kestrel-options) y [configuración del punto de conexión](#endpoint-configuration).

::: moniker-end

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a>Cuándo usar Kestrel con un proxy inverso

::: moniker range=">= aspnetcore-2.0"

Puede usar Kestrel por sí solo o con un *servidor proxy inverso*, como IIS, Nginx o Apache. Un servidor proxy inverso recibe las solicitudes HTTP de Internet y las reenvía a Kestrel después de un control preliminar.

![Kestrel se comunica directamente con Internet sin ningún servidor proxy inverso](kestrel/_static/kestrel-to-internet2.png)

![Kestrel se comunica indirectamente con Internet a través de un servidor proxy inverso, como IIS, Nginx o Apache](kestrel/_static/kestrel-to-internet.png)

Cualquiera de las configuraciones&mdash;con o sin un servidor proxy inverso&mdash;es una configuración de hospedaje válida y admitida para ASP.NET 2.0 o aplicaciones posteriores.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Si una aplicación acepta únicamente solicitudes de una red interna, Kestrel se puede usar directamente como servidor de la aplicación.

![Kestrel se comunica directamente con la red interna](kestrel/_static/kestrel-to-internal.png)

Si expone la aplicación en Internet, use IIS, Nginx o Apache como *servidor proxy inverso*. Un servidor proxy inverso recibe las solicitudes HTTP de Internet y las reenvía a Kestrel después de un control preliminar.

![Kestrel se comunica indirectamente con Internet a través de un servidor proxy inverso, como IIS, Nginx o Apache](kestrel/_static/kestrel-to-internet.png)

Se necesita un proxy inverso para las implementaciones de borde (expuestas al tráfico de Internet) por motivos de seguridad. Las versiones 1.x de Kestrel no tienen un complemento completo de defensas frente a los ataques, como unos tiempos de espera, unos límites de tamaño y unos límites de conexiones simultáneas adecuados.

::: moniker-end

Un escenario de proxy inverso tiene lugar cuando varias aplicaciones comparten el mismo puerto y dirección IP, y se ejecutan en un solo servidor. Este escenario no es viable con Kestrel, ya que Kestrel no permite compartir la misma dirección IP y el mismo puerto entre varios procesos. Si Kestrel se configura para escuchar en un puerto, controla todo el tráfico de ese puerto, independientemente del encabezado de host de la solicitud. Un proxy inverso que puede compartir puertos es capaz de reenviar solicitudes a Kestrel en una única dirección IP y puerto.

Aunque no sea necesario un servidor proxy inverso, su uso puede ser útil:

* Puede limitar el área expuesta públicamente de las aplicaciones que hospeda.
* Proporciona una capa extra de configuración y defensa.
* Es posible que se integre mejor con la infraestructura existente.
* Simplifica el equilibrio de carga y la configuración SSL. Solo el servidor proxy inverso requiere un certificado SSL, y dicho servidor se puede comunicar con los servidores de aplicaciones en la red interna por medio de HTTP sin formato.

> [!WARNING]
> Si no usa un proxy inverso con el [filtrado de hosts](#host-filtering) habilitado, deberá habilitarlo.

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a>Cómo usar Kestrel en aplicaciones ASP.NET Core

::: moniker range=">= aspnetcore-2.0"

El paquete [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) se incluye en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 o posterior).

Las plantillas de proyecto de ASP.NET Core usan Kestrel de forma predeterminada. En *Program.cs*, el código de plantilla llama a [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), que a su vez llama a [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) en segundo plano.

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

Para proporcionar configuración adicional después de llamar a `CreateDefaultBuilder`, use `ConfigureKestrel`:

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            // Set properties and call methods on options
        });
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

Para proporcionar configuración adicional después de llamar a `CreateDefaultBuilder`, llame a [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel):

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            // Set properties and call methods on options
        })
        .Build();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Instale el paquete NuGet [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/).

Llame al método de extensión [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) en [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder?view=aspnetcore-1.1) en el método `Main`, y especifique cualquier [opción de Kestrel](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1) que necesite, como se muestra en la siguiente sección.

[!code-csharp[](kestrel/samples/1.x/KestrelSample/Program.cs?name=snippet_Main&highlight=13-19)]

::: moniker-end

## <a name="kestrel-options"></a>Opciones de Kestrel

::: moniker range=">= aspnetcore-2.0"

El servidor web Kestrel tiene opciones de configuración de restricción que son especialmente útiles en las implementaciones con conexión a Internet. Estos son algunos de los límites importantes que se pueden personalizar:

* Las conexiones máximas de cliente
* El tamaño máximo del cuerpo de solicitud
* La velocidad mínima de los datos del cuerpo de solicitud.

Establezca estas y otras restricciones en la propiedad [Limits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.limits) de la clase [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions). La propiedad `Limits` contiene una instancia de la clase [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits).

### <a name="maximum-client-connections"></a>Las conexiones máximas de cliente

[MaxConcurrentConnections](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentconnections)  
[MaxConcurrentUpgradedConnections](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentupgradedconnections)

::: moniker-end

El número máximo de conexiones de TCP abiertas simultáneas que se pueden establecer para toda la aplicación con este código:

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Limits.MaxConcurrentConnections = 100;
        });
```

::: moniker-end

Hay un límite independiente para las conexiones que se han actualizado desde HTTP o HTTPS a otro protocolo (por ejemplo, en una solicitud de WebSockets). Cuando se actualiza una conexión, no se cuenta con respecto al límite de `MaxConcurrentConnections`.

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Limits.MaxConcurrentUpgradedConnections = 100;
        });
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

El número máximo de conexiones es ilimitado de forma predeterminada (null).

### <a name="maximum-request-body-size"></a>El tamaño máximo del cuerpo de solicitud

[MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize)

El tamaño máximo predeterminado del cuerpo de solicitud es 30 000 000 bytes, que son aproximadamente 28,6 MB.

El método recomendado para invalidar el límite de una aplicación ASP.NET Core MVC es usar el atributo [RequestSizeLimit](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) en un método de acción:

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

::: moniker-end

Este es un ejemplo que muestra cómo configurar la restricción en la aplicación y todas las solicitudes:

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 10 * 1024;
        });
```

Puede invalidar la configuración en una solicitud específica de middleware:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

Se inicia una excepción si la aplicación intenta configurar el límite de una solicitud después de que la aplicación haya empezado a leer la solicitud. Hay una propiedad `IsReadOnly` que señala si la propiedad `MaxRequestBodySize` tiene el estado de solo lectura, lo que significa que es demasiado tarde para configurar el límite.

### <a name="minimum-request-body-data-rate"></a>La velocidad mínima de los datos del cuerpo de solicitud.

[MinRequestBodyDataRate](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minrequestbodydatarate)  
[MinResponseDataRate](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minresponsedatarate)

Kestrel comprueba cada segundo si los datos entran a la velocidad especificada en bytes por segundo. Si la velocidad está por debajo del mínimo, se agota el tiempo de espera de la conexión. El período de gracia es la cantidad de tiempo que Kestrel da al cliente para aumentar su velocidad de envío hasta el mínimo; no se comprueba la velocidad durante ese tiempo. Este período de gracia permite evitar que se interrumpan las conexiones que inicialmente están enviando datos a una velocidad lenta debido a un inicio lento de TCP.

La velocidad mínima predeterminada es 240 bytes por segundo, con un período de gracia de cinco segundos.

También se aplica una velocidad mínima a la respuesta. El código para establecer el límite de solicitudes y el límite de respuestas es el mismo, salvo que tienen `RequestBody` o `Response` en los nombres de propiedad y de interfaz.

Este es un ejemplo que muestra cómo configurar las velocidades de datos mínimas en *Program.cs*:

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-9)]

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Limits.MinRequestBodyDataRate =
                new MinDataRate(bytesPerSecond: 100, gracePeriod: TimeSpan.FromSeconds(10));
            options.Limits.MinResponseDataRate =
                new MinDataRate(bytesPerSecond: 100, gracePeriod: TimeSpan.FromSeconds(10));
        });
```

Puede configurar las velocidades por solicitud de middleware:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=5-8)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="maximum-streams-per-connection"></a>Secuencias máximas por conexión

`Http2.MaxStreamsPerConnection` limita el número de secuencias de solicitudes simultáneas por conexión HTTP/2. Se rechazarán las secuencias en exceso.

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.MaxStreamsPerConnection = 100;
        });
```

El valor predeterminado es 100.

### <a name="header-table-size"></a>Tamaño de la tabla de encabezado

El descodificador HPACK descomprime los encabezados HTTP para las conexiones HTTP/2. `Http2.HeaderTableSize` limita el tamaño de la tabla de compresión de encabezado que usa el descodificador HPACK. El valor se proporciona en octetos y debe ser mayor que cero (0).

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.HeaderTableSize = 4096;
        });
```

El valor predeterminado es 4096.

### <a name="maximum-frame-size"></a>Tamaño máximo de marco

`Http2.MaxFrameSize` indica el tamaño máximo de la carga útil de la trama de conexión HTTP/2 que se puede recibir. El valor se proporciona en octetos y debe estar comprendido entre 2^14 (16 384) y 2^24-1 (16 777 215).

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.MaxFrameSize = 16384;
        });
```

El valor predeterminado es 2^14 (16 384).

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

Para más información sobre otras opciones y límites de Kestrel, vea:

* [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions)
* [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits)
* [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Para más información sobre las opciones y límites de Kestrel, vea:

* [KestrelServerOptions (clase)](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1)
* [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserverlimits?view=aspnetcore-1.1)

::: moniker-end

## <a name="endpoint-configuration"></a>Configuración de punto de conexión

::: moniker range="= aspnetcore-2.0"

ASP.NET Core se enlaza a `http://localhost:5000` de forma predeterminada. Llame a los métodos [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) o [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) en [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) para configurar los puertos y los prefijos de dirección URL de Kestrel. `UseUrls`, el argumento de línea de comandos `--urls`, la clave de configuración de host `urls` y la variable de entorno `ASPNETCORE_URLS` también funcionan, pero tienen las limitaciones que se indican más adelante en esta sección.

La clave de configuración de host `urls` debe proceder de la configuración del host, no de la configuración de la aplicación. Si se agrega una clave y un valor `urls` a *appsettings.json*, ello no repercute en la configuración de host, porque el host se inicializa completamente cuando se lee la configuración del archivo de configuración. Con todo, para configurar el host se puede usar una clave `urls` de *appsettings.json* con [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) en el generador de hosts:

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddJsonFile("appSettings.json", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseKestrel()
    .UseConfiguration(config)
    .UseContentRoot(Directory.GetCurrentDirectory())
    .UseStartup<Startup>()
    .Build();
```

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

ASP.NET Core enlaza de forma predeterminada a:

* `http://localhost:5000`
* `https://localhost:5001` (cuando hay presente un certificado de desarrollo local)

Un certificado de desarrollo se crea:

* Cuando el [SDK de .NET Core](/dotnet/core/sdk) está instalado.
* Para crear un certificado, se usa la [herramienta dev-certs](xref:aspnetcore-2.1#https).

Algunos exploradores requieren que se conceda permiso explícito en el explorador para poder confiar en el certificado de desarrollo local.

ASP.NET Core 2.1 y las plantillas de proyecto posteriores configuran aplicaciones para que se ejecuten en HTTPS de forma predeterminada e incluyen [redirección de HTTPS y compatibilidad con HSTS](xref:security/enforcing-ssl).

Llame a los métodos [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) o [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) en [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) para configurar los puertos y los prefijos de dirección URL de Kestrel.

`UseUrls`, el argumento de línea de comandos `--urls`, la clave de configuración de host `urls` y la variable de entorno `ASPNETCORE_URLS` también funcionan, pero tienen las limitaciones que se indican más adelante en esta sección (debe haber disponible un certificado predeterminado para la configuración de puntos de conexión HTTPS).

La configuración `KestrelServerOptions` de ASP.NET Core 2.1:

### <a name="configureendpointdefaultsactionltlistenoptionsgt"></a>ConfigureEndpointDefaults(Action&lt;ListenOptions&gt;)

Especifica una `Action` de configuración para que se ejecute con cada punto de conexión especificado. Al llamar a `ConfigureEndpointDefaults` varias veces, se reemplazan las `Action` anteriores por la última `Action` especificada.

### <a name="configurehttpsdefaultsactionlthttpsconnectionadapteroptionsgt"></a>ConfigureHttpsDefaults(Action&lt;HttpsConnectionAdapterOptions&gt;)

Especifica una `Action` de configuración para que se ejecute con cada punto de conexión HTTPS. Al llamar a `ConfigureHttpsDefaults` varias veces, se reemplazan las `Action` anteriores por la última `Action` especificada.

### <a name="configureiconfiguration"></a>Configure(IConfiguration)

Crea un cargador de configuración para configurar Kestrel que toma una [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) como entrada. El ámbito de la configuración debe corresponderse con la sección de configuración de Kestrel.

### <a name="listenoptionsusehttps"></a>ListenOptions.UseHttps

Configure Kestrel para que use HTTPS.

Extensiones de `ListenOptions.UseHttps`:

* `UseHttps`: configure Kestrel para que use HTTPS con el certificado predeterminado. Produce una excepción si no hay ningún certificado predeterminado configurado.
* `UseHttps(string fileName)`
* `UseHttps(string fileName, string password)`
* `UseHttps(string fileName, string password, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(StoreName storeName, string subject)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(X509Certificate2 serverCertificate)`
* `UseHttps(X509Certificate2 serverCertificate, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(Action<HttpsConnectionAdapterOptions> configureOptions)`

Parámetros de `ListenOptions.UseHttps`:

* `filename` es la ruta de acceso y el nombre de archivo de un archivo de certificado correspondiente al directorio donde están los archivos de contenido de la aplicación.
* `password` es la contraseña necesaria para obtener acceso a los datos del certificado X.509.
* `configureOptions` es una `Action` para configurar `HttpsConnectionAdapterOptions`. Devuelve `ListenOptions`.
* `storeName` es el almacén de certificados desde el que se carga el certificado.
* `subject` es el nombre del sujeto del certificado.
* `allowInvalid` indica si se deben tener en cuenta los certificados no válidos, como los certificados autofirmados.
* `location` es la ubicación del almacén desde el que se carga el certificado.
* `serverCertificate` es el certificado X.509.

En un entorno de producción, HTTPS se debe configurar explícitamente. Como mínimo, debe existir un certificado predeterminado.

Estas son las configuraciones compatibles:

* Sin configuración
* Reemplazar el certificado predeterminado de configuración
* Cambiar los valores predeterminados en el código

*Sin configuración*

Kestrel escucha en `http://localhost:5000` y en `https://localhost:5001` (si hay disponible un certificado predeterminado).

Especifique direcciones URL mediante los siguientes elementos:

* La variable de entorno `ASPNETCORE_URLS`.
* El argumento de la línea de comandos `--urls`.
* La clave de configuración de host `urls`.
* El método de extensión `UseUrls`.

Para obtener más información, vea [Direcciones URL del servidor](xref:fundamentals/host/web-host#server-urls) e [Invalidar la configuración](xref:fundamentals/host/web-host#override-configuration).

El valor que estos métodos suministran puede ser uno o más puntos de conexión HTTP y HTTPS (este último, si hay disponible un certificado predeterminado). Configure el valor como una lista separada por punto y coma (por ejemplo, `"Urls": "http://localhost:8000; http://localhost:8001"`).

*Reemplazar el certificado predeterminado de configuración*

[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) llama a `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` de forma predeterminada para cargar la configuración de Kestrel. Hay disponible un esquema de configuración de aplicación HTTPS predeterminado para Kestrel. Configure varios puntos de conexión (incluidas las direcciones URL y los certificados que va a usar) desde un archivo en disco o desde un almacén de certificados.

En el siguiente ejemplo de *appsettings.json*:

* Establezca **AllowInvalid** en `true` para permitir el uso de certificados no válidos (por ejemplo, certificados autofirmados).
* Cualquier punto de conexión HTTPS que no especifique un certificado (**HttpsDefaultCert** en el siguiente ejemplo) revierte al certificado definido en **Certificados** > **Predeterminado** o al certificado de desarrollo.

```json
{
"Kestrel": {
  "EndPoints": {
    "Http": {
      "Url": "http://localhost:5000"
    },

    "HttpsInlineCertFile": {
      "Url": "https://localhost:5001",
      "Certificate": {
        "Path": "<path to .pfx file>",
        "Password": "<certificate password>"
      }
    },

    "HttpsInlineCertStore": {
      "Url": "https://localhost:5002",
      "Certificate": {
        "Subject": "<subject; required>",
        "Store": "<certificate store; defaults to My>",
        "Location": "<location; defaults to CurrentUser>",
        "AllowInvalid": "<true or false; defaults to false>"
      }
    },

    "HttpsDefaultCert": {
      "Url": "https://localhost:5003"
    },

    "Https": {
      "Url": "https://*:5004",
      "Certificate": {
      "Path": "<path to .pfx file>",
      "Password": "<certificate password>"
      }
    }
    },
    "Certificates": {
      "Default": {
        "Path": "<path to .pfx file>",
        "Password": "<certificate password>"
      }
    }
  }
}
```

Una alternativa al uso de **Path** y **Password** en cualquier nodo de certificado consiste en especificar el certificado por medio de campos del almacén de certificados. Por ejemplo, el certificado en **Certificados** > **Predeterminado** se puede especificar así:

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; defaults to My>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

Notas sobre el esquema:

* En los nombres de los puntos de conexión se distingue entre mayúsculas y minúsculas. Por ejemplo, `HTTPS` y `Https` son válidos.
* El parámetro `Url` es necesario en cada punto de conexión. El formato de este parámetro es el mismo que el del parámetro de configuración `Urls` de nivel superior, excepto por el hecho de que está limitado a un único valor.
* En vez de agregarse, estos puntos de conexión reemplazan a los que están definidos en la configuración `Urls` de nivel superior. Los puntos de conexión definidos en el código a través de `Listen` son acumulativos con respecto a los puntos de conexión definidos en la sección de configuración.
* La sección `Certificate` es opcional. Si la sección `Certificate` no se especifica, se usan los valores predeterminados definidos en escenarios anteriores. Si no hay valores predeterminados disponibles, el servidor produce una excepción y no se inicia.
* La sección `Certificate` admite certificados tanto **Path**&ndash;**Password** como **Subject**&ndash;**Store**.
* Se puede definir el número de puntos de conexión que se quiera de esta manera, siempre y cuando no produzcan conflictos de puerto.
* `options.Configure(context.Configuration.GetSection("Kestrel"))` devuelve un `KestrelConfigurationLoader` con un método `.Endpoint(string name, options => { })` que se puede usar para complementar la configuración de un punto de conexión configurado:

  ```csharp
  options.Configure(context.Configuration.GetSection("Kestrel"))
      .Endpoint("HTTPS", opt =>
      {
          opt.HttpsOptions.SslProtocols = SslProtocols.Tls12;
      });
  ```

  También puede acceder directamente a `KestrelServerOptions.ConfigurationLoader` para proseguir con la iteración en el cargador existente, como la proporcionada por [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).

* La sección de configuración de cada punto de conexión está disponible en las opciones del método `Endpoint` para que se pueda leer la configuración personalizada.
* Se pueden cargar varias configuraciones volviendo a llamar a `options.Configure(context.Configuration.GetSection("Kestrel"))` con otra sección. Se usa la última configuración, a menos que se llame explícitamente a `Load` en instancias anteriores. El metapaquete no llama a `Load`, con lo cual su sección de configuración predeterminada se puede reemplazar.
* `KestrelConfigurationLoader` refleja la familia `Listen` de API de `KestrelServerOptions` como sobrecargas de `Endpoint`, por lo que los puntos de conexión de configuración y código se pueden configurar en el mismo lugar. En estas sobrecargas no se usan nombres y solo consumen valores predeterminados de la configuración.

*Cambiar los valores predeterminados en el código*

`ConfigureEndpointDefaults` y `ConfigureHttpsDefaults` se pueden usar para cambiar la configuración predeterminada de `ListenOptions` y `HttpsConnectionAdapterOptions`, incluido sustituir el certificado predeterminado especificado en el escenario anterior. Se debe llamar a `ConfigureEndpointDefaults` y a `ConfigureHttpsDefaults` antes de que se configure algún punto de conexión.

```csharp
options.ConfigureEndpointDefaults(opt =>
{
    opt.NoDelay = true;
});

options.ConfigureHttpsDefaults(httpsOptions =>
{
    httpsOptions.SslProtocols = SslProtocols.Tls12;
});
```

*Compatibilidad de Kestrel con SNI*

[Indicación de nombre de servidor (SNI)](https://tools.ietf.org/html/rfc6066#section-3) se puede usar para hospedar varios dominios en la misma dirección IP y puerto. Para que SNI funcione, el cliente envía el nombre de host de la sesión segura al servidor durante el protocolo de enlace TLS para que, de este modo, el servidor pueda proporcionar el certificado correcto. El cliente usa el certificado proporcionado para la comunicación cifrada con el servidor durante la sesión segura que sigue al protocolo de enlace TLS.

Kestrel admite SNI a través de la devolución de llamada `ServerCertificateSelector`. La devolución de llamada se invoca una vez por conexión para permitir que la aplicación inspeccione el nombre de host y seleccione el certificado adecuado.

La compatibilidad con SNI requiere lo siguiente:

* Ejecutarse en el marco de destino `netcoreapp2.1`. En `netcoreapp2.0` y `net461`, se invoca la devolución de llamada, pero `name` siempre es `null`. `name` también será `null` si el cliente no proporciona el parámetro de nombre de host en el protocolo de enlace TLS.
* Todos los sitios web deben ejecutarse en la misma instancia de Kestrel. Kestrel no admite el uso compartido de una dirección IP y un puerto entre varias instancias sin un proxy inverso.

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.ListenAnyIP(5005, listenOptions =>
            {
                listenOptions.UseHttps(httpsOptions =>
                {
                    var localhostCert = CertificateLoader.LoadFromStoreCert(
                        "localhost", "My", StoreLocation.CurrentUser, 
                        allowInvalid: true);
                    var exampleCert = CertificateLoader.LoadFromStoreCert(
                        "example.com", "My", StoreLocation.CurrentUser, 
                        allowInvalid: true);
                    var subExampleCert = CertificateLoader.LoadFromStoreCert(
                        "sub.example.com", "My", StoreLocation.CurrentUser, 
                        allowInvalid: true);
                    var certs = new Dictionary<string, X509Certificate2>(
                        StringComparer.OrdinalIgnoreCase);
                    certs["localhost"] = localhostCert;
                    certs["example.com"] = exampleCert;
                    certs["sub.example.com"] = subExampleCert;

                    httpsOptions.ServerCertificateSelector = (connectionContext, name) =>
                    {
                        if (name != null && certs.TryGetValue(name, out var cert))
                        {
                            return cert;
                        }

                        return exampleCert;
                    };
                });
            });
        });
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel((context, options) =>
        {
            options.ListenAnyIP(5005, listenOptions =>
            {
                listenOptions.UseHttps(httpsOptions =>
                {
                    var localhostCert = CertificateLoader.LoadFromStoreCert(
                        "localhost", "My", StoreLocation.CurrentUser, 
                        allowInvalid: true);
                    var exampleCert = CertificateLoader.LoadFromStoreCert(
                        "example.com", "My", StoreLocation.CurrentUser, 
                        allowInvalid: true);
                    var subExampleCert = CertificateLoader.LoadFromStoreCert(
                        "sub.example.com", "My", StoreLocation.CurrentUser, 
                        allowInvalid: true);
                    var certs = new Dictionary<string, X509Certificate2>(
                        StringComparer.OrdinalIgnoreCase);
                    certs["localhost"] = localhostCert;
                    certs["example.com"] = exampleCert;
                    certs["sub.example.com"] = subExampleCert;

                    httpsOptions.ServerCertificateSelector = (connectionContext, name) =>
                    {
                        if (name != null && certs.TryGetValue(name, out var cert))
                        {
                            return cert;
                        }

                        return exampleCert;
                    };
                });
            });
        })
        .Build();
```

::: moniker-end

### <a name="bind-to-a-tcp-socket"></a>Enlazar a un socket TCP

El método [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) se enlaza a un socket TCP y una lambda de opciones hace posible la configuración de un certificado SSL:

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

```csharp
public static void Main(string[] args)
{
    CreateWebHostBuilder(args).Build().Run();
}

public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Listen(IPAddress.Loopback, 5000);
            options.Listen(IPAddress.Loopback, 5001, listenOptions =>
            {
                listenOptions.UseHttps("testCert.pfx", "testPassword");
            });
        });
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

En el ejemplo se configura SSL para un punto de conexión con [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions). Use la misma API para configurar otras opciones de Kestrel para puntos de conexión específicos.

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a>Enlazar a un socket de Unix

Escuche en un socket de Unix con [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) para mejorar el rendimiento con Nginx, tal y como se muestra en este ejemplo:

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.ListenUnixSocket("/tmp/kestrel-test.sock");
            options.ListenUnixSocket("/tmp/kestrel-test.sock", listenOptions =>
            {
                listenOptions.UseHttps("testCert.pfx", "testpassword");
            });
        });
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="port-0"></a>Puerto 0

Cuando se especifica el número de puerto `0`, Kestrel se enlaza de forma dinámica a un puerto disponible. En el siguiente ejemplo se muestra cómo averiguar qué puerto Kestrel está realmente enlazado a un runtime:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

Cuando la aplicación se ejecuta, la salida de la ventana de consola indica el puerto dinámico en el que se puede tener acceso a la aplicación:

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a>Limitaciones

Configure puntos de conexión con los siguientes métodos:

* [UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls)
* El argumento de la línea de comandos `--urls`
* La clave de configuración de host `urls`
* La variable de entorno `ASPNETCORE_URLS`

Estos métodos son útiles para que el código funcione con servidores que no sean de Kestrel. Sin embargo, tenga en cuenta las siguientes limitaciones:

* SSL no se puede usar con estos métodos, a menos que se proporcione un certificado predeterminado en la configuración del punto de conexión HTTPS (por ejemplo, por medio de la configuración `KestrelServerOptions` o de un archivo de configuración, tal y como se explicó anteriormente en este tema).
* Cuando los métodos `Listen` y `UseUrls` se usan al mismo tiempo, los puntos de conexión de `Listen` sustituyen a los de `UseUrls`.

### <a name="iis-endpoint-configuration"></a>Configuración de puntos de conexión IIS

Cuando se usa IIS, los enlaces de direcciones URL de IIS reemplazan a los enlaces que se hayan establecido por medio de `Listen` o de `UseUrls`. Para más información, vea el tema [Módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

ASP.NET Core se enlaza a `http://localhost:5000` de forma predeterminada. Configure los puertos y los prefijos de dirección URL para Kernel usando lo siguiente:

* El método de extensión [UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls?view=aspnetcore-1.1)
* El argumento de la línea de comandos `--urls`
* La clave de configuración de host `urls`
* El sistema de configuración de ASP.NET Core, incluida la variable de entorno `ASPNETCORE_URLS`

Para más información sobre estos métodos, vea [Hospedaje](xref:fundamentals/host/index).

### <a name="iis-endpoint-configuration"></a>Configuración de puntos de conexión IIS

Cuando se usa IIS, los enlaces de direcciones URL de IIS reemplazan a los enlaces que se hayan establecido por medio de `UseUrls`. Para más información, vea el tema [Módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="listenoptionsprotocols"></a>ListenOptions.Protocols

La propiedad `Protocols` establece los protocolos HTTP (`HttpProtocols`) habilitados en un punto de conexión o para el servidor. Asigne un valor a la propiedad `Protocols` desde el valor de enumeración `HttpProtocols`.

| Valor de enumeración `HttpProtocols` | Protocolo de conexión permitido |
| -------------------------- | ----------------------------- |
| `Http1`                    | HTTP/1.1 solo. Puede usarse con o sin TLS. |
| `Http2`                    | HTTP/2 solo. Se usa principalmente con TLS. Se pueden utilizar sin TLS solo si el cliente admite un [modo de conocimientos previos](https://tools.ietf.org/html/rfc7540#section-3.4). |
| `Http1AndHttp2`            | HTTP/1.1 y HTTP/2. Requiere una conexión TLS y [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) para negociar HTTP/2; de lo contrario, la opción predeterminada para la conexión es HTTP/1.1. |

El protocolo predeterminado es HTTP/1.1.

Restricciones de TLS para HTTP/2:

* TLS 1.2 o versiones posteriores
* Renegociación deshabilitada
* Compresión deshabilitada
* Tamaños de intercambio de claves efímeras mínimos:
  * Curva elíptica Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack;; 224 bits como mínimo
  * Campo finito Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack;; 2048 bits como mínimo
* Conjunto de cifrado no restringido

`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; con la curva elíptica P-256 &lbrack;`FIPS186`&rbrack; es compatible de forma predeterminada.

El siguiente ejemplo permite conexiones HTTP/1.1 y HTTP/2 en el puerto 8000. Las conexiones se protegen mediante TLS con un certificado proporcionado:

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Listen(IPAddress.Any, 8000, listenOptions =>
            {
                listenOptions.Protocols = HttpProtocols.Http1AndHttp2;
                listenOptions.UseHttps("testCert.pfx", "testPassword");
            });
        }
```

Opcionalmente, cree una implementación `IConnectionAdapter` para filtrar los protocolos de enlace TLS en una base por conexión para los cifrados específicos:

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Listen(IPAddress.Any, 8000, listenOptions =>
            {
                listenOptions.Protocols = HttpProtocols.Http1AndHttp2;
                listenOptions.UseHttps("testCert.pfx", "testPassword");
                listenOptions.ConnectionAdapters.Add(new TlsFilterAdapter());
            });
        }
```

```csharp
private class TlsFilterAdapter : IConnectionAdapter
{
    public bool IsHttps => false;

    public Task<IAdaptedConnection> OnConnectionAsync(ConnectionAdapterContext context)
    {
        var tlsFeature = context.Features.Get<ITlsHandshakeFeature>();

        // Throw NotSupportedException for any cipher algorithm that you don't 
        // wish to support. Alternatively, define and compare 
        // ITlsHandshakeFeature.CipherAlgorithm to a list of acceptable cipher 
        // suites.
        //
        // A ITlsHandshakeFeature.CipherAlgorithm of CipherAlgorithmType.Null 
        // indicates that no cipher algorithm supported by Kestrel matches the 
        // requested algorithm(s).
        if (tlsFeature.CipherAlgorithm == CipherAlgorithmType.Null)
        {
            throw new NotSupportedException("Prohibited cipher: " + tlsFeature.CipherAlgorithm);
        }

        return Task.FromResult<IAdaptedConnection>(new AdaptedConnection(context.ConnectionStream));
    }

    private class AdaptedConnection : IAdaptedConnection
    {
        public AdaptedConnection(Stream adaptedStream)
        {
            ConnectionStream = adaptedStream;
        }

        public Stream ConnectionStream { get; }

        public void Dispose()
        {
        }
    }
}
```

*Defina el protocolo a partir de la configuración*

[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) llama a `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` de forma predeterminada para cargar la configuración de Kestrel.

En el siguiente ejemplo de *appsettings.json*, se establece un protocolo de conexión predeterminado (HTTP/1.1 y HTTP/2) para todos los puntos de conexión de Kestrel:

```json
{
  "Kestrel": {
    "EndPointDefaults": {
      "Protocols": "Http1AndHttp2"
    }
  }
}
```

El siguiente ejemplo de archivo de configuración establece un protocolo de conexión para un punto de conexión específico:

```json
{
  "Kestrel": {
    "EndPoints": {
      "HttpsDefaultCert": {
        "Url": "https://localhost:5001",
        "Protocols": "Http1AndHttp2"
      }
    }
  }
}
```

Los protocolos especificados en el código invalidan los valores establecidos por la configuración.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="transport-configuration"></a>Configuración de transporte

Desde el lanzamiento de ASP.NET Core 2.1, el transporte predeterminado de Kestrel deja de basarse en Libuv y pasa a basarse en sockets administrados. Se trata de un cambio muy importante para las aplicaciones ASP.NET Core 2.0 que actualizan a 2.1 y llaman a [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv), y que dependen de cualquiera de los siguientes paquetes:

* [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (referencia de paquete directa)
* [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

En el caso de los proyectos de ASP.NET Core 2.1 o posterior que usan el metapaquete [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) y requieren el uso de Libuv:

* Agregar una dependencia del paquete [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) al archivo de proyecto de la aplicación:

    ```xml
    <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv" 
                      Version="<LATEST_VERSION>" />
    ```

* Llame a [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv):

    ```csharp
    public class Program
    {
        public static void Main(string[] args)
        {
            CreateWebHostBuilder(args).Build().Run();
        }

        public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                .UseLibuv()
                .UseStartup<Startup>();
    }
    ```

::: moniker-end

### <a name="url-prefixes"></a>Prefijos de URL

Al usar `UseUrls`, el argumento de línea de comandos `--urls`, la clave de configuración de host `urls` o una variable de entorno `ASPNETCORE_URLS`, los prefijos de dirección URL pueden tener cualquiera de estos formatos.

::: moniker range=">= aspnetcore-2.0"

Solo son válidos los prefijos de dirección URL HTTP. Kestrel no admite SSL cuando se configuran enlaces de dirección URL con `UseUrls`.

* Dirección IPv4 con número de puerto

  ```
  http://65.55.39.10:80/
  ```

  `0.0.0.0` es un caso especial que enlaza a todas las direcciones IPv4.

* Dirección IPv6 con número de puerto

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  `[::]` es el equivalente en IPv6 de `0.0.0.0` en IPv4.

* Nombre de host con número de puerto

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  Los nombres de host, `*` y `+` no son especiales. Todo lo que no se identifique como una dirección IP o un `localhost` válido se enlaza a todas las direcciones IP de IPv6 e IPv4. Para enlazar distintos nombres de host a distintas aplicaciones ASP.NET Core en el mismo puerto, use [HTTP.sys](xref:fundamentals/servers/httpsys) o un servidor proxy inverso, como IIS, Nginx o Apache.

  > [!WARNING]
  > Si no usa un proxy inverso con el [filtrado de hosts](#host-filtering) habilitado, habilítelo.

* Nombre `localhost` del host con el número de puerto o la IP de bucle invertido con el número de puerto

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  Cuando se especifica `localhost`, Kestrel intenta enlazar a las interfaces de bucle invertido de IPv4 e IPv6. Si el puerto solicitado lo está usando otro servicio en cualquier interfaz de bucle invertido, Kestrel no se puede iniciar. Si ninguna de estas interfaces de bucle invertido está disponible por cualquier otra razón (normalmente porque no se admite IPv6), Kestrel registra una advertencia.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

* Dirección IPv4 con número de puerto

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  `0.0.0.0` es un caso especial que enlaza a todas las direcciones IPv4.

* Dirección IPv6 con número de puerto

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  https://[0:0:0:0:0:ffff:4137:270a]:443/
  ```

  `[::]` es el equivalente en IPv6 de `0.0.0.0` en IPv4.

* Nombre de host con número de puerto

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  Los nombres de host `*` y `+` no son especiales. Todo lo que no sea una dirección IP o un `localhost` reconocido se enlaza a todas las direcciones IP de IPv6 e IPv4. Para enlazar distintos nombres de host a distintas aplicaciones ASP.NET Core en el mismo puerto, use [WebListener](xref:fundamentals/servers/weblistener) o un servidor proxy inverso, como IIS, Nginx o Apache.

* Nombre `localhost` del host con el número de puerto o la IP de bucle invertido con el número de puerto

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  Cuando se especifica `localhost`, Kestrel intenta enlazar a las interfaces de bucle invertido de IPv4 e IPv6. Si el puerto solicitado lo está usando otro servicio en cualquier interfaz de bucle invertido, Kestrel no se puede iniciar. Si ninguna de estas interfaces de bucle invertido está disponible por cualquier otra razón (normalmente porque no se admite IPv6), Kestrel registra una advertencia.

* Socket de UNIX

  ```
  http://unix:/run/dan-live.sock
  ```

**Puerto 0**

Cuando se especifica el número de puerto `0`, Kestrel se enlaza de forma dinámica a un puerto disponible. El enlace al puerto `0` está permitido en cualquier nombre de host o IP, excepto en `localhost`.

Cuando la aplicación se ejecuta, la salida de la ventana de consola indica el puerto dinámico en el que se puede tener acceso a la aplicación:

```console
Now listening on: http://127.0.0.1:48508
```

**Prefijos de URL para SSL**

Si llama al método de extensión `UseHttps`, procure incluir los prefijos de URL con `https:`:

```csharp
var host = new WebHostBuilder()
    .UseKestrel(options =>
    {
        options.UseHttps("testCert.pfx", "testPassword");
    })
   .UseUrls("http://localhost:5000", "https://localhost:5001")
   .UseContentRoot(Directory.GetCurrentDirectory())
   .UseStartup<Startup>()
   .Build();
```

> [!NOTE]
> HTTP y HTTPS no se pueden hospedar en el mismo puerto.

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

::: moniker-end

## <a name="host-filtering"></a>Filtrado de hosts

Si bien Kestrel admite una configuración basada en prefijos como `http://example.com:5000`, pasa por alto completamente el nombre de host. El host `localhost` es un caso especial que se usa para enlazar a direcciones de bucle invertido. Cualquier otro host que no sea una dirección IP explícita se enlaza a todas las direcciones IP públicas. Ninguno de estos datos se usa para validar encabezados `Host` de solicitudes.

::: moniker range="< aspnetcore-2.0"

Como solución alternativa, hospede detrás de un proxy inverso con filtrado de encabezados de host. Este es el único escenario admitido de Kestrel en ASP.NET Core 1.x.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Como solución alternativa, use un middleware para filtrar las solicitudes por el encabezado `Host`:

```csharp
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.Logging;
using Microsoft.Extensions.Primitives;
using Microsoft.Net.Http.Headers;
using System;
using System.Collections.Generic;
using System.Threading.Tasks;

// A normal middleware would provide an options type, config binding, extension methods, etc..
// This intentionally does all of the work inside of the middleware so it can be
// easily copy-pasted into docs and other projects.
public class HostFilteringMiddleware
{
    private readonly RequestDelegate _next;
    private readonly IList<string> _hosts;
    private readonly ILogger<HostFilteringMiddleware> _logger;

    public HostFilteringMiddleware(RequestDelegate next, IConfiguration config, ILogger<HostFilteringMiddleware> logger)
    {
        if (config == null)
        {
            throw new ArgumentNullException(nameof(config));
        }

        _next = next ?? throw new ArgumentNullException(nameof(next));
        _logger = logger ?? throw new ArgumentNullException(nameof(logger));

        // A semicolon separated list of host names without the port numbers.
        // IPv6 addresses must use the bounding brackets and be in their normalized form.
        _hosts = config["AllowedHosts"]?.Split(new[] { ';' }, StringSplitOptions.RemoveEmptyEntries);
        if (_hosts == null || _hosts.Count == 0)
        {
            throw new InvalidOperationException("No configuration entry found for AllowedHosts.");
        }
    }

    public Task Invoke(HttpContext context)
    {
        if (!ValidateHost(context))
        {
            context.Response.StatusCode = 400;
            _logger.LogDebug("Request rejected due to incorrect Host header.");
            return Task.CompletedTask;
        }

        return _next(context);
    }

    // This does not duplicate format validations that are expected to be performed by the host.
    private bool ValidateHost(HttpContext context)
    {
        StringSegment host = context.Request.Headers[HeaderNames.Host].ToString().Trim();

        if (StringSegment.IsNullOrEmpty(host))
        {
            // Http/1.0 does not require the Host header.
            // Http/1.1 requires the header but the value may be empty.
            return true;
        }

        // Drop the port

        var colonIndex = host.LastIndexOf(':');

        // IPv6 special case
        if (host.StartsWith("[", StringComparison.Ordinal))
        {
            var endBracketIndex = host.IndexOf(']');
            if (endBracketIndex < 0)
            {
                // Invalid format
                return false;
            }
            if (colonIndex < endBracketIndex)
            {
                // No port, just the IPv6 Host
                colonIndex = -1;
            }
        }

        if (colonIndex > 0)
        {
            host = host.Subsegment(0, colonIndex);
        }

        foreach (var allowedHost in _hosts)
        {
            if (StringSegment.Equals(allowedHost, host, StringComparison.OrdinalIgnoreCase))
            {
                return true;
            }

            // Sub-domain wildcards: *.example.com
            if (allowedHost.StartsWith("*.", StringComparison.Ordinal) && host.Length >= allowedHost.Length)
            {
                // .example.com
                var allowedRoot = new StringSegment(allowedHost, 1, allowedHost.Length - 1);

                var hostRoot = host.Subsegment(host.Length - allowedRoot.Length, allowedRoot.Length);
                if (hostRoot.Equals(allowedRoot, StringComparison.OrdinalIgnoreCase))
                {
                    return true;
                }
            }
        }

        return false;
    }
}
```

Registre el `HostFilteringMiddleware` anterior en `Startup.Configure`. Cabe mencionar que el [orden de registro del middleware](xref:fundamentals/middleware/index#order) es importante. debe tener lugar inmediatamente después del registro del middleware de diagnóstico (por ejemplo, `app.UseExceptionHandler`).

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
        app.UseBrowserLink();
    }
    else
    {
        app.UseExceptionHandler("/Home/Error");
    }

    app.UseMiddleware<HostFilteringMiddleware>();

    app.UseMvcWithDefaultRoute();
}
```

El middleware espera una clave `AllowedHosts` en *appsettings.json*/*appsettings.\<EnvironmentName>.json*. El valor es una lista delimitada por punto y coma de nombres de host sin los números de puerto:

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

Como solución alternativa, use el Middleware de filtrado de hosts. El Middleware de filtrado de hosts se facilita a través del paquete [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering), que se incluye en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 o posterior). Este middleware se agrega por medio de [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), que llama a [AddHostFiltering](/dotnet/api/microsoft.aspnetcore.builder.hostfilteringservicesextensions.addhostfiltering):

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

El Middleware de filtrado de hosts está deshabilitado de forma predeterminada. Para habilitarlo, defina una clave `AllowedHosts` en *appsettings.json*/*appsettings.\<EnvironmentName>.json*. El valor es una lista delimitada por punto y coma de nombres de host sin los números de puerto:

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

*appsettings.json*:

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> El [Middleware de encabezados reenviados](xref:host-and-deploy/proxy-load-balancer) también tiene una opción [ForwardedHeadersOptions.AllowedHosts](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.allowedhosts). El Middleware de encabezados reenviados y el Middleware de filtrado de hosts tienen una funcionalidad similar en diferentes escenarios. Establecer `AllowedHosts` con el Middleware de encabezados reenviados es adecuado cuando el encabezado de host no se conserva mientras se reenvían solicitudes con un servidor proxy inverso o un equilibrador de carga. Establecer `AllowedHosts` con el Middleware de filtrado de hosts es adecuado cuando se usa Kestrel como un servidor perimetral, o cuando el encabezado de host se reenvía directamente.
>
> Para más información sobre el Middleware de encabezados reenviados, vea [Configurar ASP.NET Core para trabajar con servidores proxy y equilibradores de carga](xref:host-and-deploy/proxy-load-balancer).

::: moniker-end

## <a name="additional-resources"></a>Recursos adicionales

* [Aplicación de HTTPS](xref:security/enforcing-ssl)
* [Código fuente de Kestrel](https://github.com/aspnet/KestrelHttpServer)
* [RFC 7230: Message Syntax and Routing (Section 5.4: Host)](https://tools.ietf.org/html/rfc7230#section-5.4) (RFC 7230: Enrutamiento y sintaxis de mensajes [Sección 5.4: Host])
* [Configurar ASP.NET Core para trabajar con servidores proxy y equilibradores de carga](xref:host-and-deploy/proxy-load-balancer)
