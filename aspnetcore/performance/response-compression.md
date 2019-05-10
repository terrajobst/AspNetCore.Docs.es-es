---
title: Compresión de respuesta en ASP.NET Core
author: guardrex
description: Obtenga información sobre la compresión de respuesta y cómo usar Middleware de compresión de respuesta en aplicaciones de ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/13/2019
uid: performance/response-compression
ms.openlocfilehash: e312d43fb62106f6ecb98367c29daa377bb227c9
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/27/2019
ms.locfileid: "64893352"
---
# <a name="response-compression-in-aspnet-core"></a>Compresión de respuesta en ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))

Ancho de banda de red es un recurso limitado. Reducir el tamaño de la respuesta normalmente aumenta la capacidad de respuesta de una aplicación, a menudo drásticamente. Una forma de reducir los tamaños de carga es comprimir las respuestas de una aplicación.

## <a name="when-to-use-response-compression-middleware"></a>Cuándo usar Middleware de compresión de respuestas

Use las tecnologías de compresión de respuesta basado en servidor en IIS, Apache o Nginx. El rendimiento del middleware probablemente no coincida con los módulos de servidor. [Servidor HTTP.sys](xref:fundamentals/servers/httpsys) server y [Kestrel](xref:fundamentals/servers/kestrel) server actualmente no ofrecen compatibilidad integrada de compresión.

Use el Middleware de compresión de respuesta cuando haya:

* No se puede usar las siguientes tecnologías de compresión basada en servidor:
  * [Módulo de compresión dinámica de IIS](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [Apache mod_deflate module](http://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [Nginx compresión y descompresión](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* Que hospeda directamente en:
  * [Servidor HTTP.sys](xref:fundamentals/servers/httpsys) (anteriormente denominados WebListener)
  * [Servidor kestrel](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a>Compresión de las respuestas

Por lo general, cualquier respuesta comprimida de forma no nativa puede beneficiarse de la compresión de respuesta. Respuestas de forma no nativa comprimidas normalmente incluyen: CSS, JavaScript, HTML, XML y JSON. No debe comprimir activos comprimidos de forma nativa, como archivos PNG. Si se intenta comprimir aún más una respuesta cifrada de forma nativa, reducción adicional ninguna pequeño en el tiempo de tamaño y la transmisión probablemente resultar mínimo comparado con el tiempo que tardó en procesarse la compresión. No comprimir los archivos inferiores a aproximadamente 150-1000 bytes (según el contenido del archivo y la eficacia de compresión). La sobrecarga de la compresión de archivos pequeños puede producir un archivo comprimido mayor que el archivo descomprimido.

Cuando un cliente pueda procesar el contenido comprimido, el cliente debe informar al servidor de sus capacidades enviando el `Accept-Encoding` encabezado con la solicitud. Cuando un servidor envía contenido comprimido, debe incluir información en el `Content-Encoding` encabezado en cómo se codifica la respuesta comprimida. Contenido designaciones de codificación admitidas por el middleware se muestran en la tabla siguiente.

::: moniker range=">= aspnetcore-2.2"

| `Accept-Encoding` valores de encabezado | Middleware compatible | Descripción |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | Sí (valor predeterminado)        | [Formato de datos comprimidos Brotli](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | No                   | [Formato de datos comprimidos DEFLATE](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | No                   | [Intercambio de W3C XML eficaz](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | Sí                  | [Formato de archivo GZIP](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | Sí                  | Identificador de "Ninguna codificación": No se debe codificar la respuesta. |
| `pack200-gzip`                  | No                   | [Formato de transferencia de red para archivos de Java](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | Sí                  | Cualquier contenido disponible no solicitados explícitamente la codificación |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

| `Accept-Encoding` valores de encabezado | Middleware compatible | Descripción |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | No                   | [Formato de datos comprimidos Brotli](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | No                   | [Formato de datos comprimidos DEFLATE](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | No                   | [Intercambio de W3C XML eficaz](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | Sí (valor predeterminado)        | [Formato de archivo GZIP](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | Sí                  | Identificador de "Ninguna codificación": No se debe codificar la respuesta. |
| `pack200-gzip`                  | No                   | [Formato de transferencia de red para archivos de Java](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | Sí                  | Cualquier contenido disponible no solicitados explícitamente la codificación |

::: moniker-end

Para obtener más información, consulte el [IANA oficial de codificación lista del contenido](http://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).

El middleware le permite agregar proveedores de compresión adicional para las instalaciones personalizadas `Accept-Encoding` valores de encabezado. Para obtener más información, consulte [proveedores personalizados](#custom-providers) a continuación.

El software intermedio es capaz de reaccionar ante el valor de calidad (qvalue, `q`) cuando los envía al cliente para dar prioridad a los esquemas de compresión de ponderación. Para obtener más información, consulte [RFC 7231: Codificación aceptada](https://tools.ietf.org/html/rfc7231#section-5.3.4).

Algoritmos de compresión están sujetos a un equilibrio entre la velocidad de compresión y la eficacia de la compresión. *Eficacia* en este contexto se refiere al tamaño de la salida después de la compresión. El tamaño más pequeño se logra mediante el máximo *óptimo* compresión.

Los encabezados implicados en la solicitud, enviar, almacenamiento en caché y recibir contenido comprimido se describen en la tabla siguiente.

| Header             | Rol |
| ------------------ | ---- |
| `Accept-Encoding`  | Enviado desde el cliente al servidor para indicar la codificación esquemas aceptables para el cliente del contenido. |
| `Content-Encoding` | Enviado desde el servidor al cliente para indicar la codificación del contenido de la carga. |
| `Content-Length`   | Cuando se produce la compresión, el `Content-Length` se quita el encabezado, desde los cambios de contenido del cuerpo cuando se comprime la respuesta. |
| `Content-MD5`      | Cuando se produce la compresión, el `Content-MD5` se quita el encabezado, puesto que ha cambiado el contenido del cuerpo y el hash ya no es válido. |
| `Content-Type`     | Especifica el tipo MIME del contenido. Debe especificar cada respuesta su `Content-Type`. El middleware comprueba este valor para determinar si se debe comprimir la respuesta. El middleware especifica un conjunto de [tipos MIME predeterminados](#mime-types) que puede codificar, pero puede reemplazar o agregar tipos MIME. |
| `Vary`             | Cuando envía el servidor con un valor de `Accept-Encoding` a los clientes y servidores proxy, el `Vary` encabezado indica al cliente o servidor proxy que debe almacenar en caché (puede variar) las respuestas en función del valor de la `Accept-Encoding` encabezado de la solicitud. El resultado de la devolución del contenido con el `Vary: Accept-Encoding` encabezado está comprimida y las que se almacenan en caché las respuestas sin comprimir por separado. |

Explore las características de Middleware de compresión de respuesta con el [aplicación de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples). En el ejemplo muestra:

* La compresión de respuestas de la aplicación con Gzip y proveedores de compresión personalizado.
* Cómo agregar un tipo MIME a la lista predeterminada de los tipos MIME para compresión.

## <a name="package"></a>Paquete

::: moniker range=">= aspnetcore-2.1"

Para incluir el software intermedio en un proyecto, agregue una referencia a la [Microsoft.AspNetCore.App metapaquete](xref:fundamentals/metapackage-app), que incluye el [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) paquete.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Para incluir el software intermedio en un proyecto, agregue una referencia a la [metapaquete Microsoft.AspNetCore.All](xref:fundamentals/metapackage), que incluye el [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) paquete.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Para incluir el software intermedio en un proyecto, agregue una referencia a la [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) paquete.

::: moniker-end

## <a name="configuration"></a>Configuración

::: moniker range=">= aspnetcore-2.2"

El código siguiente muestra cómo habilitar el Middleware de compresión de respuestas para tipos MIME predeterminados y los proveedores de compresión ([Brotli](#brotli-compression-provider) y [Gzip](#gzip-compression-provider)):

::: moniker-end

::: moniker range="< aspnetcore-2.2"

El código siguiente muestra cómo habilitar el Middleware de compresión de respuestas para tipos MIME predeterminados y el [proveedor de compresión Gzip](#gzip-compression-provider):

::: moniker-end

```csharp
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddResponseCompression();
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        app.UseResponseCompression();
    }
}
```

Notas:

* `app.UseResponseCompression` debe llamarse antes `app.UseMvc`.
* Usar una herramienta como [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), o [Postman](https://www.getpostman.com/) para establecer el `Accept-Encoding` encabezado de solicitud y estudiar los encabezados de respuesta, el tamaño y el cuerpo.

Enviar una solicitud a la aplicación de ejemplo sin la `Accept-Encoding` encabezado y observe que la respuesta es sin comprimir. El `Content-Encoding` y `Vary` encabezados no están presentes en la respuesta.

![Ventana de Fiddler que muestra el resultado de una solicitud sin el encabezado Accept-Encoding. No se comprime la respuesta.](response-compression/_static/request-uncompressed.png)

::: moniker range=">= aspnetcore-2.2"

Enviar una solicitud a la aplicación de ejemplo con el `Accept-Encoding: br` encabezado (la compresión Brotli) y observe que la respuesta está comprimida. El `Content-Encoding` y `Vary` encabezados están presentes en la respuesta.

![Ventana de Fiddler que muestra el resultado de una solicitud con el encabezado Accept-Encoding y un valor de Brasil. Los encabezados pueden variar y Content-Encoding se agregan a la respuesta. La respuesta se comprime.](response-compression/_static/request-compressed-br.png)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Enviar una solicitud a la aplicación de ejemplo con el `Accept-Encoding: gzip` encabezado y observe que la respuesta está comprimida. El `Content-Encoding` y `Vary` encabezados están presentes en la respuesta.

![Ventana de Fiddler que muestra el resultado de una solicitud con el encabezado Accept-Encoding y un valor de gzip. Los encabezados pueden variar y Content-Encoding se agregan a la respuesta. La respuesta se comprime.](response-compression/_static/request-compressed.png)

::: moniker-end

## <a name="providers"></a>Proveedores

::: moniker range=">= aspnetcore-2.2"

### <a name="brotli-compression-provider"></a>Proveedor de la compresión Brotli

Use la <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProvider> para comprimir las respuestas con el [formato de datos comprimidos Brotli](https://tools.ietf.org/html/rfc7932).

Si no hay proveedores de compresión se agregan explícitamente a la <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:

* El proveedor de la compresión Brotli se agrega de forma predeterminada en la matriz de proveedores de compresión junto con el [proveedor de compresión Gzip](#gzip-compression-provider).
* Valores predeterminados de compresión para la compresión Brotli cuando el formato de datos comprimidos Brotli es compatible con el cliente. Si Brotli no es compatible con el cliente, la compresión predeterminada es Gzip cuando el cliente admite la compresión Gzip.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

Cuando se agregan explícitamente todos los proveedores de compresión, se debe agregar el proveedor de compresión Brotoli:

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=5)]

Establecer nivel con la compresión <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProviderOptions>. El proveedor de la compresión Brotli el valor predeterminado es el nivel de compresión más rápido ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), que no podría producir la compresión más eficaz. Si se desea la compresión más eficaz, configure el middleware de compresión óptima.

| Nivel de compresión | Descripción |
| ----------------- | ----------- |
| [CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel) | Compresión debe completarse tan pronto como sea posible, incluso si la salida resultante no se comprime de forma óptima. |
| [CompressionLevel.NoCompression](xref:System.IO.Compression.CompressionLevel) | No debe realizarse ninguna compresión. |
| [CompressionLevel.Optimal](xref:System.IO.Compression.CompressionLevel) | Las respuestas se deben comprimir óptimamente, incluso si la compresión tarda más tiempo en completarse. |

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();

    services.Configure<BrotliCompressionProviderOptions>(options => 
    {
        options.Level = CompressionLevel.Fastest;
    });
}
```

::: moniker-end

### <a name="gzip-compression-provider"></a>Proveedor de compresión gzip

Use la <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> para comprimir las respuestas con el [formato de archivo Gzip](https://tools.ietf.org/html/rfc1952).

Si no hay proveedores de compresión se agregan explícitamente a la <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:

::: moniker range=">= aspnetcore-2.2"

* El proveedor de compresión Gzip se agrega de forma predeterminada en la matriz de proveedores de compresión junto con el [proveedor de la compresión Brotli](#brotli-compression-provider).
* Valores predeterminados de compresión para la compresión Brotli cuando el formato de datos comprimidos Brotli es compatible con el cliente. Si Brotli no es compatible con el cliente, la compresión predeterminada es Gzip cuando el cliente admite la compresión Gzip.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* El proveedor de compresión Gzip se agrega de forma predeterminada en la matriz de proveedores de compresión.
* Compresión predeterminada en Gzip cuando el cliente admite la compresión Gzip.

::: moniker-end

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

El proveedor de compresión Gzip deben agregarse cuando se agregan explícitamente todos los proveedores de compresión:

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=6)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=5)]

::: moniker-end

Establecer nivel con la compresión <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>. El proveedor de compresión Gzip el valor predeterminado es el nivel de compresión más rápido ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), que no podría producir la compresión más eficaz. Si se desea la compresión más eficaz, configure el middleware de compresión óptima.

| Nivel de compresión | Descripción |
| ----------------- | ----------- |
| [CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel) | Compresión debe completarse tan pronto como sea posible, incluso si la salida resultante no se comprime de forma óptima. |
| [CompressionLevel.NoCompression](xref:System.IO.Compression.CompressionLevel) | No debe realizarse ninguna compresión. |
| [CompressionLevel.Optimal](xref:System.IO.Compression.CompressionLevel) | Las respuestas se deben comprimir óptimamente, incluso si la compresión tarda más tiempo en completarse. |

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();

    services.Configure<GzipCompressionProviderOptions>(options => 
    {
        options.Level = CompressionLevel.Fastest;
    });
}
```

### <a name="custom-providers"></a>Proveedores personalizados

Crear implementaciones de compresión personalizado con <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>. El <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> representa el contenido que este codificación `ICompressionProvider` genera. El middleware usa esta información para elegir el proveedor basándose en la lista especificada en el `Accept-Encoding` encabezado de la solicitud.

Uso de la aplicación de ejemplo, el cliente envía una solicitud con el `Accept-Encoding: mycustomcompression` encabezado. El middleware usa la implementación de compresión personalizado y devuelve la respuesta con un `Content-Encoding: mycustomcompression` encabezado. El cliente debe poder descomprimir la codificación personalizada en orden para una implementación de compresión personalizado para que funcione.

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=7)]

[!code-csharp[](response-compression/samples/2.x/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=6)]

[!code-csharp[](response-compression/samples/1.x/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

Enviar una solicitud a la aplicación de ejemplo con el `Accept-Encoding: mycustomcompression` encabezado y observe los encabezados de respuesta. El `Vary` y `Content-Encoding` encabezados están presentes en la respuesta. El cuerpo de respuesta (no mostrado) no se comprime en el ejemplo. No hay una implementación de la compresión en el `CustomCompressionProvider` clase del ejemplo. Sin embargo, el ejemplo muestra que podría implementar un algoritmo de compresión de este tipo.

![Ventana de Fiddler que muestra el resultado de una solicitud con el encabezado Accept-Encoding y un valor de mycustomcompression. Los encabezados pueden variar y Content-Encoding se agregan a la respuesta.](response-compression/_static/request-custom-compression.png)

## <a name="mime-types"></a>tipos MIME

El middleware especifica un conjunto predeterminado de los tipos MIME para compresión:

* `application/javascript`
* `application/json`
* `application/xml`
* `text/css`
* `text/html`
* `text/json`
* `text/plain`
* `text/xml`

Reemplazar o anexar los tipos MIME con las opciones de Middleware de compresión de respuesta. Tenga en cuenta que comodines MIME tipos, como `text/*` no se admiten. La aplicación de ejemplo agrega un tipo MIME para `image/svg+xml` y comprime y sirve de imagen del banner de ASP.NET Core (*banner.svg*).

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=8-10)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=7-9)]

::: moniker-end

## <a name="compression-with-secure-protocol"></a>Compresión con el protocolo seguro

Pueden controlar las respuestas comprimidas a través de conexiones seguras con el `EnableForHttps` opción, que está deshabilitada de forma predeterminada. Uso de compresión con páginas generadas dinámicamente puede dar lugar a problemas de seguridad, como el [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) y [infracción](https://wikipedia.org/wiki/BREACH_(security_exploit)) ataques.

## <a name="adding-the-vary-header"></a>Agregar el encabezado Vary

::: moniker range=">= aspnetcore-2.0"

Si basa la compresión de respuestas en el `Accept-Encoding` encabezado, hay potencialmente varias versiones comprimidas de la respuesta y una versión sin comprimir. Con el fin de indicar a las memorias caché de cliente y servidor proxy que existen varias versiones y deben almacenarse los `Vary` encabezado se agrega con un `Accept-Encoding` valor. En ASP.NET Core 2.0 o posterior, se agrega el middleware la `Vary` automáticamente cuando se comprime la respuesta de encabezado.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Si basa la compresión de respuestas en el `Accept-Encoding` encabezado, hay potencialmente varias versiones comprimidas de la respuesta y una versión sin comprimir. Con el fin de indicar a las memorias caché de cliente y servidor proxy que existen varias versiones y deben almacenarse los `Vary` encabezado se agrega con un `Accept-Encoding` valor. En ASP.NET Core 1.x, agregar el `Vary` encabezado a la respuesta se lleva a cabo manualmente:

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet1)]

::: moniker-end

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a>Problema de middleware cuando están detrás de un proxy inverso de Nginx

Cuando una solicitud se redirigió mediante un proxy nginx, el `Accept-Encoding` se quita el encabezado. Eliminación de la `Accept-Encoding` encabezado evita que el middleware de compresión de la respuesta. Para obtener más información, consulte [NGINX: Compresión y descompresión](https://www.nginx.com/resources/admin-guide/compression-and-decompression/). Este problema es controlando [averiguar la compresión de paso a través de Nginx (aspnet/BasicMiddleware \#123)](https://github.com/aspnet/BasicMiddleware/issues/123).

## <a name="working-with-iis-dynamic-compression"></a>Trabajar con la compresión dinámica de IIS

Si tiene un módulo de compresión dinámica de IIS activo configurado en el nivel de servidor que desea deshabilitar para una aplicación, deshabilite el módulo con una adición a la *web.config* archivo. Para más información, vea [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules) (Deshabilitación de módulos de IIS).

## <a name="troubleshooting"></a>Solución de problemas

Usar una herramienta como [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), o [Postman](https://www.getpostman.com/), que permiten establecer el `Accept-Encoding` encabezado de solicitud y estudiar los encabezados de respuesta, el tamaño y el cuerpo. De forma predeterminada, el Middleware de compresión de respuestas comprime las respuestas que cumplen las condiciones siguientes:

::: moniker range=">= aspnetcore-2.2"

* El `Accept-Encoding` está presente con un valor de encabezado `br`, `gzip`, `*`, o codificación personalizada que coincide con un proveedor de compresión personalizado que haya establecido. El valor no debe ser `identity` o tiene un valor de calidad (qvalue, `q`) de 0 (cero).
* El tipo MIME (`Content-Type`) deben establecerse y debe coincidir con un tipo MIME configurado en el <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.
* La solicitud no debe incluir el `Content-Range` encabezado.
* La solicitud debe utilizar el protocolo no seguro (http), a menos que el protocolo seguro (https) se configura en las opciones de Middleware de compresión de respuesta. *Tenga en cuenta el peligro [se ha descrito anteriormente](#compression-with-secure-protocol) al habilitar la compresión de contenido segura.*

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* El `Accept-Encoding` está presente con un valor de encabezado `gzip`, `*`, o codificación personalizada que coincide con un proveedor de compresión personalizado que haya establecido. El valor no debe ser `identity` o tiene un valor de calidad (qvalue, `q`) de 0 (cero).
* El tipo MIME (`Content-Type`) deben establecerse y debe coincidir con un tipo MIME configurado en el <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.
* La solicitud no debe incluir el `Content-Range` encabezado.
* La solicitud debe utilizar el protocolo no seguro (http), a menos que el protocolo seguro (https) se configura en las opciones de Middleware de compresión de respuesta. *Tenga en cuenta el peligro [se ha descrito anteriormente](#compression-with-secure-protocol) al habilitar la compresión de contenido segura.*

::: moniker-end

## <a name="additional-resources"></a>Recursos adicionales

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* [Mozilla Developer Network: Accept-Encoding](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [Sección RFC 7231 3.1.2.1: Códigos de contenido](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [RFC 7230 sección 4.2.3: Codificación gzip](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [Versión de especificación del formato de archivo GZIP 4.3](http://www.ietf.org/rfc/rfc1952.txt)
