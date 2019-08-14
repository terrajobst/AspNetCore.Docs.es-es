---
title: Compresión de respuesta en ASP.NET Core
author: guardrex
description: Obtenga información sobre la compresión de respuesta y cómo usar Middleware de compresión de respuesta en aplicaciones de ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/09/2019
uid: performance/response-compression
ms.openlocfilehash: e320e87179f9f1b9773a55c380684a3f3f712632
ms.sourcegitcommit: 89fcc6cb3e12790dca2b8b62f86609bed6335be9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2019
ms.locfileid: "68993472"
---
# <a name="response-compression-in-aspnet-core"></a>Compresión de respuesta en ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))

El ancho de banda de red es un recurso limitado. Reducir el tamaño de la respuesta normalmente aumenta la capacidad de respuesta de una aplicación, a menudo drásticamente. Una manera de reducir los tamaños de carga es comprimir las respuestas de una aplicación.

## <a name="when-to-use-response-compression-middleware"></a>Cuándo usar el middleware de compresión de respuesta

Usar tecnologías de compresión de respuesta basadas en servidor en IIS, Apache o Nginx. El rendimiento del middleware probablemente no coincidirá con el de los módulos del servidor. El servidor [http. sys](xref:fundamentals/servers/httpsys) Server y el servidor [Kestrel](xref:fundamentals/servers/kestrel) no ofrecen actualmente compatibilidad con la compresión integrada.

Use middleware de compresión de respuesta cuando esté:

* No se pueden usar las siguientes tecnologías de compresión basadas en servidor:
  * [Módulo de compresión dinámica de IIS](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [Módulo de Apache mod_deflate](https://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [Compresión y descompresión de nginx](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* Hospedaje directo en:
  * [Servidor http. sys](xref:fundamentals/servers/httpsys) (anteriormente denominado weblistener)
  * [Servidor de Kestrel](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a>Compresión de las respuestas

Normalmente, cualquier respuesta no comprimida de forma nativa puede beneficiarse de la compresión de respuesta. Las respuestas no comprimidas de forma nativa suelen incluir: CSS, JavaScript, HTML, XML y JSON. No debe comprimir los recursos comprimidos de forma nativa, como los archivos PNG. Si intenta comprimir aún más una respuesta comprimida de forma nativa, es probable que cualquier reducción adicional en el tamaño y el tiempo de transmisión esté sobrecurrida en el tiempo que se tarda en procesar la compresión. No comprimir archivos de menos de 150-1000 bytes (según el contenido del archivo y la eficacia de la compresión). La sobrecarga de comprimir archivos pequeños puede generar un archivo comprimido mayor que el archivo sin comprimir.

Cuando un cliente puede procesar contenido comprimido, el cliente debe informar al servidor de sus funcionalidades enviando el `Accept-Encoding` encabezado con la solicitud. Cuando un servidor envía contenido comprimido, debe incluir información en el `Content-Encoding` encabezado sobre cómo se codifica la respuesta comprimida. En la tabla siguiente se muestran las designaciones de codificación de contenido que admite el middleware.

::: moniker range=">= aspnetcore-2.2"

| `Accept-Encoding`valores de encabezado | Middleware compatible | DESCRIPCIÓN |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | Sí (valor predeterminado)        | [Brotli formato de datos comprimidos](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | Sin                   | [Desinflar formato de datos comprimidos](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | Sin                   | [Intercambio XML eficaz de W3C](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | Sí                  | [Formato de archivo gzip](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | Sí                  | Identificador de "sin codificación": No se debe codificar la respuesta. |
| `pack200-gzip`                  | Sin                   | [Formato de transferencia de red para archivos de Java](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | Sí                  | Cualquier codificación de contenido disponible no se ha solicitado explícitamente |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

| `Accept-Encoding`valores de encabezado | Middleware compatible | DESCRIPCIÓN |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | Sin                   | [Brotli formato de datos comprimidos](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | Sin                   | [Desinflar formato de datos comprimidos](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | Sin                   | [Intercambio XML eficaz de W3C](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | Sí (valor predeterminado)        | [Formato de archivo gzip](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | Sí                  | Identificador de "sin codificación": No se debe codificar la respuesta. |
| `pack200-gzip`                  | Sin                   | [Formato de transferencia de red para archivos de Java](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | Sí                  | Cualquier codificación de contenido disponible no se ha solicitado explícitamente |

::: moniker-end

Para obtener más información, consulte la [lista de codificación de contenido oficial de IANA](https://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).

El middleware permite agregar proveedores de compresión adicionales para los valores `Accept-Encoding` de encabezado personalizados. Para obtener más información, vea [proveedores personalizados](#custom-providers) a continuación.

El middleware es capaz de reaccionar a la ponderación del `q`valor de calidad (qvalue) cuando lo envía el cliente para priorizar los esquemas de compresión. Para obtener más información, [consulte RFC 7231: Aceptación: codificación](https://tools.ietf.org/html/rfc7231#section-5.3.4).

Los algoritmos de compresión están sujetos a un equilibrio entre la velocidad de compresión y la eficacia de la compresión. La *eficacia* en este contexto hace referencia al tamaño de la salida después de la compresión. La compresión más *óptima* consigue el menor tamaño.

En la tabla siguiente se describen los encabezados implicados en la solicitud, el envío, el almacenamiento en caché y la recepción de contenido comprimido.

| Encabezado             | Rol |
| ------------------ | ---- |
| `Accept-Encoding`  | Se envía desde el cliente al servidor para indicar los esquemas de codificación de contenido aceptables para el cliente. |
| `Content-Encoding` | Se envía desde el servidor al cliente para indicar la codificación del contenido en la carga. |
| `Content-Length`   | Cuando se produce la compresión `Content-Length` , se quita el encabezado, ya que el contenido del cuerpo cambia cuando se comprime la respuesta. |
| `Content-MD5`      | Cuando se produce la compresión `Content-MD5` , se quita el encabezado, ya que el contenido del cuerpo ha cambiado y el hash ya no es válido. |
| `Content-Type`     | Especifica el tipo MIME del contenido. Cada respuesta debe especificar su `Content-Type`. El middleware comprueba este valor para determinar si se debe comprimir la respuesta. El middleware especifica un conjunto de [tipos MIME](#mime-types) predeterminados que se pueden codificar, pero puede reemplazar o agregar tipos MIME. |
| `Vary`             | Cuando lo envía el servidor con un valor de `Accept-Encoding` a los clientes y servidores proxy, `Vary` el encabezado indica al cliente o al proxy que debe almacenar en caché (variar) respuestas en función del valor `Accept-Encoding` del encabezado de la solicitud. El resultado de devolver el contenido con `Vary: Accept-Encoding` el encabezado es que las respuestas comprimidas y sin comprimir se almacenan en caché por separado. |

Explore las características del middleware de compresión de respuesta con la [aplicación de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples). En el ejemplo se muestra:

* Compresión de las respuestas de la aplicación mediante gzip y proveedores de compresión personalizados.
* Cómo agregar un tipo MIME a la lista predeterminada de tipos MIME para la compresión.

## <a name="package"></a>Paquete

::: moniker range=">= aspnetcore-3.0"

El middleware de compresión de respuesta lo proporciona el paquete [Microsoft. AspNetCore. ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) , que se incluye implícitamente en ASP.net Core aplicaciones.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Para incluir el middleware en un proyecto, agregue una referencia al [metapaquete Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), que incluye el paquete [Microsoft. AspNetCore. ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) .

::: moniker-end

## <a name="configuration"></a>Configuración

::: moniker range=">= aspnetcore-2.2"

En el código siguiente se muestra cómo habilitar el middleware de compresión de respuesta para los tipos MIME predeterminados y los proveedores de compresión ([Brotli](#brotli-compression-provider) y [gzip](#gzip-compression-provider)):

::: moniker-end

::: moniker range="< aspnetcore-2.2"

En el código siguiente se muestra cómo habilitar el middleware de compresión de respuesta para los tipos MIME predeterminados y el [proveedor de compresión gzip](#gzip-compression-provider):

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

* `app.UseResponseCompression`se debe llamar a `app.UseMvc`antes de.
* Use una herramienta como [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/)o [Postman](https://www.getpostman.com/) para establecer el encabezado de la `Accept-Encoding` solicitud y estudiar los encabezados de respuesta, el tamaño y el cuerpo.

Envíe una solicitud a la aplicación de ejemplo sin `Accept-Encoding` el encabezado y observe que la respuesta se ha descomprimido. Los `Content-Encoding` encabezados `Vary` y no están presentes en la respuesta.

![Ventana de Fiddler que muestra el resultado de una solicitud sin el encabezado Accept-Encoding. La respuesta no se comprime.](response-compression/_static/request-uncompressed.png)

::: moniker range=">= aspnetcore-2.2"

Envíe una solicitud a la aplicación de ejemplo con `Accept-Encoding: br` el encabezado (compresión Brotli) y observe que la respuesta está comprimida. Los `Content-Encoding` encabezados `Vary` y están presentes en la respuesta.

![Ventana de Fiddler que muestra el resultado de una solicitud con el encabezado Accept-Encoding y un valor de br. Los encabezados Vary y Content-Encoding se agregan a la respuesta. La respuesta se comprime.](response-compression/_static/request-compressed-br.png)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Envíe una solicitud a la aplicación de ejemplo con `Accept-Encoding: gzip` el encabezado y observe que la respuesta está comprimida. Los `Content-Encoding` encabezados `Vary` y están presentes en la respuesta.

![Ventana de Fiddler que muestra el resultado de una solicitud con el encabezado Accept-Encoding y un valor de gzip. Los encabezados Vary y Content-Encoding se agregan a la respuesta. La respuesta se comprime.](response-compression/_static/request-compressed.png)

::: moniker-end

## <a name="providers"></a>Proveedores

::: moniker range=">= aspnetcore-2.2"

### <a name="brotli-compression-provider"></a>Proveedor de compresión Brotli

Use el <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProvider> para comprimir las respuestas con el [formato de datos comprimidos Brotli](https://tools.ietf.org/html/rfc7932).

Si no se agregan explícitamente proveedores de <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>compresión a:

* El proveedor de compresión Brotli se agrega de forma predeterminada a la matriz de proveedores de compresión junto con el [proveedor de compresión gzip](#gzip-compression-provider).
* La compresión toma como valor predeterminado la compresión Brotli cuando el cliente admite el formato de datos comprimidos Brotli. Si el cliente no admite Brotli, el valor predeterminado de la compresión es gzip cuando el cliente admite la compresión gzip.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

El proveedor de compresión Brotoli debe agregarse cuando se agreguen explícitamente proveedores de compresión:

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

Establezca el nivel de compresión <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProviderOptions>con. El proveedor de compresión Brotli tiene como valor predeterminado el nivel de compresión más rápido ([CompressionLevel. Fast](xref:System.IO.Compression.CompressionLevel)), que podría no producir la compresión más eficaz. Si se desea la compresión más eficaz, configure el middleware para una compresión óptima.

| Nivel de compresión | DESCRIPCIÓN |
| ----------------- | ----------- |
| [CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel) | La compresión debe completarse tan pronto como sea posible, incluso si la salida resultante no se comprime de forma óptima. |
| [CompressionLevel. NoCompression](xref:System.IO.Compression.CompressionLevel) | No se debe realizar ninguna compresión. |
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

Utilice el <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> para comprimir las respuestas con el [formato de archivo gzip](https://tools.ietf.org/html/rfc1952).

Si no se agregan explícitamente proveedores de <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>compresión a:

::: moniker range=">= aspnetcore-2.2"

* El proveedor de compresión gzip se agrega de forma predeterminada a la matriz de proveedores de compresión junto con el [proveedor de compresión Brotli](#brotli-compression-provider).
* La compresión toma como valor predeterminado la compresión Brotli cuando el cliente admite el formato de datos comprimidos Brotli. Si el cliente no admite Brotli, el valor predeterminado de la compresión es gzip cuando el cliente admite la compresión gzip.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* El proveedor de compresión gzip se agrega de forma predeterminada a la matriz de proveedores de compresión.
* De forma predeterminada, la compresión es gzip cuando el cliente admite la compresión gzip.

::: moniker-end

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

El proveedor de compresión gzip debe agregarse cuando se agreguen explícitamente proveedores de compresión:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=6)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=6)]

::: moniker-end

Establezca el nivel de compresión <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>con. El proveedor de compresión gzip tiene como valor predeterminado el nivel de compresión más rápido ([CompressionLevel. Fast](xref:System.IO.Compression.CompressionLevel)), que podría no producir la compresión más eficaz. Si se desea la compresión más eficaz, configure el middleware para una compresión óptima.

| Nivel de compresión | DESCRIPCIÓN |
| ----------------- | ----------- |
| [CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel) | La compresión debe completarse tan pronto como sea posible, incluso si la salida resultante no se comprime de forma óptima. |
| [CompressionLevel. NoCompression](xref:System.IO.Compression.CompressionLevel) | No se debe realizar ninguna compresión. |
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

Cree implementaciones de compresión personalizadas <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>con. Representa la codificación de contenido que produce este `ICompressionProvider`. <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> El middleware usa esta información para elegir el proveedor en función de la lista especificada en `Accept-Encoding` el encabezado de la solicitud.

Mediante el uso de la aplicación de ejemplo, el cliente envía una `Accept-Encoding: mycustomcompression` solicitud con el encabezado. El middleware usa la implementación de la compresión personalizada y devuelve la respuesta `Content-Encoding: mycustomcompression` con un encabezado. El cliente debe ser capaz de descomprimir la codificación personalizada para que funcione una implementación de compresión personalizada.

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=7)]

[!code-csharp[](response-compression/samples/3.x/SampleApp/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=7)]

[!code-csharp[](response-compression/samples/2.x/SampleApp/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

Envíe una solicitud a la aplicación de ejemplo con `Accept-Encoding: mycustomcompression` el encabezado y observe los encabezados de respuesta. Los `Vary` encabezados `Content-Encoding` y están presentes en la respuesta. El cuerpo de la respuesta (no se muestra) no se comprime en el ejemplo. No hay ninguna implementación de compresión en `CustomCompressionProvider` la clase del ejemplo. Sin embargo, en el ejemplo se muestra dónde implementaría este tipo de algoritmo de compresión.

![Ventana de Fiddler que muestra el resultado de una solicitud con el encabezado Accept-Encoding y un valor de mycustomcompression. Los encabezados Vary y Content-Encoding se agregan a la respuesta.](response-compression/_static/request-custom-compression.png)

## <a name="mime-types"></a>tipos MIME

El middleware especifica un conjunto predeterminado de tipos MIME para la compresión:

* `application/javascript`
* `application/json`
* `application/xml`
* `text/css`
* `text/html`
* `text/json`
* `text/plain`
* `text/xml`

Reemplazar o anexar tipos MIME con las opciones de middleware de compresión de respuesta. Tenga en `text/*` cuenta que no se admiten los tipos MIME comodín, como. La aplicación de ejemplo agrega un tipo MIME `image/svg+xml` para y comprime y sirve la imagen de banner ASP.net Core (*banner. svg*).

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=8-10)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=8-10)]

::: moniker-end

## <a name="compression-with-secure-protocol"></a>Compresión con protocolo seguro

Las respuestas comprimidas a través de conexiones seguras se `EnableForHttps` pueden controlar con la opción, que está deshabilitada de forma predeterminada. El uso de la compresión con páginas generadas dinámicamente puede provocar problemas de seguridad, como los ataques de [delitos](https://wikipedia.org/wiki/CRIME_(security_exploit)) y [brechas](https://wikipedia.org/wiki/BREACH_(security_exploit)) .

## <a name="adding-the-vary-header"></a>Agregar el encabezado Vary

Al comprimir las respuestas en función del `Accept-Encoding` encabezado, existen potencialmente varias versiones comprimidas de la respuesta y una versión sin comprimir. Con el fin de indicar a las memorias caché de cliente y proxy que existen varias versiones y deben `Vary` almacenarse, el encabezado `Accept-Encoding` se agrega con un valor. En ASP.net Core 2,0 o posterior, el middleware agrega el `Vary` encabezado automáticamente cuando se comprime la respuesta.

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a>Problema de middleware al estar detrás de un proxy inverso de nginx

Cuando una solicitud es un proxy por Nginx, `Accept-Encoding` se quita el encabezado. La eliminación del `Accept-Encoding` encabezado impide que el middleware comprima la respuesta. Para obtener más información, consulte [NGINX: Compresión y descompresión](https://www.nginx.com/resources/admin-guide/compression-and-decompression/). Se realiza un seguimiento de este problema mediante la [compresión de paso a través de Nginx (ASPNET \#/BasicMiddleware 123)](https://github.com/aspnet/BasicMiddleware/issues/123).

## <a name="working-with-iis-dynamic-compression"></a>Trabajar con la compresión dinámica de IIS

Si tiene un módulo de compresión dinámica de IIS activo configurado en el nivel de servidor que quiere deshabilitar para una aplicación, deshabilite el módulo con una adición al archivo *Web. config* . Para más información, vea [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules) (Deshabilitación de módulos de IIS).

## <a name="troubleshooting"></a>solución de problemas

Use una herramienta como [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/)o [Postman](https://www.getpostman.com/), que le permite establecer el `Accept-Encoding` encabezado de la solicitud y estudiar los encabezados de respuesta, el tamaño y el cuerpo. De forma predeterminada, el middleware de compresión de respuesta comprime las respuestas que cumplen las condiciones siguientes:

::: moniker range=">= aspnetcore-2.2"

* El `Accept-Encoding` encabezado está presente con un valor de `br`, `gzip`, `*`o codificación personalizada que coincide con un proveedor de compresión personalizado que ha establecido. El valor no debe ser `identity` ni tener un valor de calidad ( `q`qvalue) de 0 (cero).
* Se debe establecer el`Content-Type`tipo MIME () y debe coincidir con un tipo MIME configurado <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>en.
* La solicitud no debe incluir el `Content-Range` encabezado.
* La solicitud debe usar el Protocolo no seguro (http), a menos que se configure el protocolo seguro (https) en las opciones de middleware de compresión de respuesta. *Tenga en cuenta el riesgo [descrito anteriormente](#compression-with-secure-protocol) al habilitar la compresión de contenido segura.*

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* El `Accept-Encoding` encabezado está presente con un valor de `gzip`, `*`o codificación personalizada que coincide con un proveedor de compresión personalizado que ha establecido. El valor no debe ser `identity` ni tener un valor de calidad ( `q`qvalue) de 0 (cero).
* Se debe establecer el`Content-Type`tipo MIME () y debe coincidir con un tipo MIME configurado <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>en.
* La solicitud no debe incluir el `Content-Range` encabezado.
* La solicitud debe usar el Protocolo no seguro (http), a menos que se configure el protocolo seguro (https) en las opciones de middleware de compresión de respuesta. *Tenga en cuenta el riesgo [descrito anteriormente](#compression-with-secure-protocol) al habilitar la compresión de contenido segura.*

::: moniker-end

## <a name="additional-resources"></a>Recursos adicionales

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* [Red del desarrollador de Mozilla: Accept-Encoding](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [Sección 3.1.2.1 de RFC 7231: Codificaciones de contenido](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [RFC 7230 sección 4.2.3: Codificación gzip](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [Versión de especificación de formato de archivo GZIP 4,3](https://www.ietf.org/rfc/rfc1952.txt)
