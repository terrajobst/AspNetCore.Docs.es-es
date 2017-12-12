---
title: "Middleware de compresión de respuesta para ASP.NET Core"
author: guardrex
description: "Obtenga información acerca de la compresión de respuesta y cómo usar el Middleware de compresión de respuesta en aplicaciones de ASP.NET Core."
keywords: "Núcleo de ASP.NET, rendimiento, compresión de respuesta, gzip, codificación aceptada, middleware"
ms.author: riande
manager: wpickett
ms.date: 08/20/2017
ms.topic: article
ms.assetid: de621887-c5c9-4ac8-9efd-f5cc0457a134
ms.technology: aspnet
ms.prod: asp.net-core
uid: performance/response-compression
ms.openlocfilehash: fdb396d8857dc9c118cc19da1f7d1d498dfaacd5
ms.sourcegitcommit: 8ab9d0065fad23400757e4e08033787e42c97d41
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2017
---
# <a name="response-compression-middleware-for-aspnet-core"></a>Middleware de compresión de respuesta para ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

Ancho de banda de red es un recurso limitado. Reducir el tamaño de la respuesta normalmente aumenta la capacidad de respuesta de una aplicación, a menudo drásticamente. Una manera de reducir el tamaño de carga es comprimir las respuestas de una aplicación.

## <a name="when-to-use-response-compression-middleware"></a>Cuándo utilizar Middleware de compresión de respuesta
Utilizar tecnologías de compresión de respuesta basada en servidor en IIS, Apache o Nginx. El rendimiento del middleware de probablemente no coincida con el de los módulos del servidor. [Servidor HTTP.sys](xref:fundamentals/servers/httpsys) y [Kestrel](xref:fundamentals/servers/kestrel) actualmente no ofrecen compatibilidad con la compresión integrada.

Use Middleware de compresión de respuesta cuando esté:

* No se puede usar las siguientes tecnologías de compresión basada en servidor:
  * [Módulo de compresión dinámica de IIS](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [Módulo de mod_deflate de Apache](http://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [NGINX compresión y descompresión](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* Hospeda directamente en:
  * [Servidor HTTP.sys](xref:fundamentals/servers/httpsys) (anteriormente denominados [WebListener](xref:fundamentals/servers/weblistener))
  * [Kestrel](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a>Compresión de respuesta
Por lo general, cualquier respuesta que comprimen de forma nativa puede beneficiarse de compresión de respuesta. Respuestas de forma no nativa comprimidas normalmente son: CSS, JavaScript, HTML, XML y JSON. No debe comprimir activos comprimidos de forma nativa, como archivos PNG. Si se intentan volver a comprimir aún más una respuesta cifrada de forma nativa, cualquier pequeña reducción adicional en tiempo de tamaño y la transmisión probablemente resultar mínimo comparado con el tiempo que tardó en procesarse la compresión. No comprimir los archivos de menos de aproximadamente 150-1000 bytes (según el contenido del archivo y la eficacia de compresión). La sobrecarga de la compresión de archivos pequeños puede generar un archivo comprimido mayor que el archivo sin comprimir.

Cuando un cliente puede procesar contenido comprimido, el cliente debe informar al servidor de sus capacidades mediante el envío de la `Accept-Encoding` encabezado con la solicitud. Cuando un servidor envía contenido comprimido, debe incluir información de la `Content-Encoding` encabezado en cómo se codifican las respuestas comprimidas. Contenido designaciones de codificación admitidas por el middleware se muestran en la tabla siguiente.

| `Accept-Encoding`valores de encabezado | Software intermedio compatible | Descripción                                                 |
| :-----------------------------: | :------------------: | ----------------------------------------------------------- |
| `br`                            | No                   | Formato de los datos comprimidos de Brotli                               |
| `compress`                      | No                   | Formato de datos de "comprimir" de UNIX                                 |
| `deflate`                       | No                   | "deflate" datos comprimidos en el formato de datos de "zlib"     |
| `exi`                           | No                   | Intercambio eficaz de XML de W3C                               |
| `gzip`                          | Sí (valor predeterminado)        | formato de archivo GZIP                                            |
| `identity`                      | Sí                  | Identificador de "Sin codificación": no se debe codificar la respuesta. |
| `pack200-gzip`                  | No                   | Formato de transferencia de red para archivos de Java                   |
| `*`                             | Sí                  | Cualquier contenido disponible no codificación explícitamente solicitada     |

Para obtener más información, consulte el [lista de codificación oficial contenido de IANA](http://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).

El middleware le permite agregar proveedores de compresión adicional para personalizado `Accept-Encoding` valores de encabezado. Para obtener más información, consulte [proveedores personalizados](#custom-providers) a continuación.

El software intermedio es capaz de reaccionar ante el valor de calidad (qvalue, `q`) ponderación cuando enviada por el cliente para dar prioridad a los esquemas de compresión. Para obtener más información, consulte [RFC 7231: codificación aceptada](https://tools.ietf.org/html/rfc7231#section-5.3.4).

Algoritmos de compresión se están sujetas a un equilibrio entre la velocidad de compresión y la eficacia de la compresión. *Eficacia* en este contexto hace referencia al tamaño de la salida después de la compresión. El tamaño más pequeño se logra mediante la mayoría *óptimo* compresión.

Los encabezados implicados en la solicitud, enviar, almacenamiento en caché y recibir contenido comprimido se describen en la tabla siguiente.

| Header             | Rol |
| ------------------ | ---- |
| `Accept-Encoding`  | Envía desde el cliente al servidor para indicar el contenido aceptable para el cliente de esquemas de codificación. |
| `Content-Encoding` | Envía desde el servidor al cliente para indicar la codificación del contenido de la carga. |
| `Content-Length`   | Cuando se produce la compresión, el `Content-Length` se quita el encabezado, desde los cambios de contenido del cuerpo cuando se comprime la respuesta. |
| `Content-MD5`      | Cuando se produce la compresión, el `Content-MD5` se quita el encabezado, puesto que ha cambiado el contenido del cuerpo y el hash ya no es válido. |
| `Content-Type`     | Especifica el tipo MIME del contenido. Debe especificar cada respuesta su `Content-Type`. El middleware comprueba este valor para determinar si se debe comprimir la respuesta. El middleware especifica un conjunto de [predeterminado tipos MIME](#mime-types) que puede codificar, pero puede reemplazar o agregar un tipo MIME. |
| `Vary`             | Cuando envía el servidor con un valor de `Accept-Encoding` a los clientes y servidores proxy, la `Vary` encabezado indica al cliente o proxy que debe almacenar en caché (varían) las respuestas en función del valor de la `Accept-Encoding` encabezado de la solicitud. El resultado de la devolución del contenido con el `Vary: Accept-Encoding` encabezado es que ambos comprimen y sin comprimir las respuestas se almacenan en caché por separado. |

Puede explorar las características del Middleware de compresión de respuesta con el [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples). El ejemplo muestra:
* La compresión de las respuestas de aplicación mediante gzip y proveedores de compresión personalizada.
* Cómo agregar un tipo MIME a la lista predeterminada de tipos MIME para la compresión.

## <a name="package"></a>Package
Para incluir el software intermedio en el proyecto, agregue una referencia a la [ `Microsoft.AspNetCore.ResponseCompression` ](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) empaquetar o usar el [ `Microsoft.AspNetCore.All` ](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) paquete. Esta característica está disponible para las aplicaciones que tienen como destino ASP.NET Core 1.1 o posterior.

## <a name="configuration"></a>Configuración
El código siguiente muestra cómo habilitar el Middleware de compresión de respuesta con el con la compresión de gzip predeterminada y para los tipos MIME predeterminado.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](response-compression/samples/2.x/StartupBasic.cs?name=snippet1&highlight=4,8)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](response-compression/samples/1.x/StartupBasic.cs?name=snippet1&highlight=3,8)]

---

> [!NOTE]
> Utilice una herramienta como [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), o [Postman](https://www.getpostman.com/) para establecer el `Accept-Encoding` encabezado de solicitud y estudiar los encabezados de respuesta, el tamaño y el cuerpo.

Enviar una solicitud a la aplicación de ejemplo sin la `Accept-Encoding` encabezado y observe que la respuesta es sin comprimir. El `Content-Encoding` y `Vary` encabezados no están presentes en la respuesta.

![Ventana de Fiddler que muestra el resultado de una solicitud sin el encabezado Accept-Encoding. No se comprime la respuesta.](response-compression/_static/request-uncompressed.png)

Enviar una solicitud a la aplicación de ejemplo con el `Accept-Encoding: gzip` encabezado y observe que la respuesta está comprimida. El `Content-Encoding` y `Vary` encabezados están presentes en la respuesta.

![Ventana de Fiddler Mostrar resultado de una solicitud con el encabezado Accept-Encoding y un valor de gzip. Los encabezados pueden variar y la codificación de contenido se agregan a la respuesta. La respuesta se comprime.](response-compression/_static/request-compressed.png)

## <a name="providers"></a>Proveedores
### <a name="gzipcompressionprovider"></a>GzipCompressionProvider
Use la `GzipCompressionProvider` para comprimir las respuestas con gzip. Esto es el proveedor de compresión predeterminado si no se especifica ninguno. Puede establecer la compresión de nivel con el `GzipCompressionProviderOptions`. 

El proveedor de compresión gzip usa de forma predeterminada el nivel de compresión más rápido (`CompressionLevel.Fastest`), que no puede producir la compresión más eficaz. Si se desea la compresión más eficaz, puede configurar el middleware de compresión óptima.

| Nivel de compresión                | Descripción                                                                                                   |
| -------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| `CompressionLevel.Fastest`       | Compresión debe completarse tan pronto como sea posible, incluso si el resultado no se comprime un rendimiento óptimo. |
| `CompressionLevel.NoCompression` | Es necesario realizar ninguna compresión.                                                                           |
| `CompressionLevel.Optimal`       | Las respuestas se deben comprimir un rendimiento óptimo, incluso si la compresión tarda más tiempo en completarse.                |


# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](response-compression/samples/2.x/Program.cs?name=snippet1&highlight=3,8-11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=5,10-13)]

---

## <a name="mime-types"></a>tipos MIME
El middleware especifica un conjunto predeterminado de tipos MIME para la compresión:
* `text/plain`
* `text/css`
* `application/javascript`
* `text/html`
* `application/xml`
* `text/xml`
* `application/json`
* `text/json`

Puede reemplazar o anexar los tipos MIME con las opciones de Middleware de compresión de respuesta. Tenga en cuenta que MIME de comodines tipos, como `text/*` no son compatibles. La aplicación de ejemplo agrega un tipo MIME para `image/svg+xml` y comprime y sirve de imagen de titular de ASP.NET Core (*banner.svg*).

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](response-compression/samples/2.x/Program.cs?name=snippet1&highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=7)]

---

### <a name="custom-providers"></a>Proveedores personalizados
Puede crear implementaciones de compresión personalizada con `ICompressionProvider`. El `EncodingName` representa la codificación que el control de contenido `ICompressionProvider` genera. El middleware que usa esta información para elegir el proveedor basándose en la lista especificada en el `Accept-Encoding` encabezado de la solicitud.

Mediante la aplicación de ejemplo, el cliente envía una solicitud con el `Accept-Encoding: mycustomcompression` encabezado. El software intermedio utiliza la implementación de compresión personalizada y devuelve la respuesta con un `Content-Encoding: mycustomcompression` encabezado. El cliente debe poder descomprimir la codificación personalizada en orden para una implementación de la compresión personalizado para que funcione.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](response-compression/samples/2.x/Program.cs?name=snippet1&highlight=4)]

[!code-csharp[Main](response-compression/samples/2.x/CustomCompressionProvider.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=6)]

[!code-csharp[Main](response-compression/samples/1.x/CustomCompressionProvider.cs?name=snippet1)]

---

Enviar una solicitud a la aplicación de ejemplo con el `Accept-Encoding: mycustomcompression` encabezado y observe los encabezados de respuesta. El `Vary` y `Content-Encoding` encabezados están presentes en la respuesta. El cuerpo de respuesta (no mostrado) no está comprimido en el ejemplo. No hay una implementación de la compresión en el `CustomCompressionProvider` clase del ejemplo. Sin embargo, en el ejemplo se muestra donde se implementa un algoritmo de compresión de este tipo.

![Ventana de Fiddler Mostrar resultado de una solicitud con el encabezado Accept-Encoding y un valor de mycustomcompression. Los encabezados pueden variar y la codificación de contenido se agregan a la respuesta.](response-compression/_static/request-custom-compression.png)

## <a name="compression-with-secure-protocol"></a>Compresión con protocolo seguro
Las respuestas comprimidas a través de conexiones seguras pueden controlarse con el `EnableForHttps` opción, que está deshabilitada de forma predeterminada. Utilice la compresión con páginas generadas dinámicamente puede provocar problemas de seguridad como la [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) y [infracción](https://wikipedia.org/wiki/BREACH_(security_exploit)) ataques.

## <a name="adding-the-vary-header"></a>Agregar el encabezado Vary
Al comprimir las respuestas según el `Accept-Encoding` encabezado, hay potencialmente varias versiones comprimidas de la respuesta y una versión sin comprimir. Para indicar a las memorias caché de cliente y servidor proxy que existen varias versiones y deben almacenarse el `Vary` encabezado se agrega con un `Accept-Encoding` valor. En ASP.NET Core 1.x, agregar el `Vary` encabezado en la respuesta se realiza manualmente. En ASP.NET Core 2.x, el middleware agrega el `Vary` encabezado automáticamente cuando se comprime la respuesta.

**ASP.NET Core solo 1.x**

[!code-csharp[Main](response-compression/samples/1.x/Startup.cs?name=snippet1)]

## <a name="middlware-issue-when-behind-an-nginx-reverse-proxy"></a>Problema de Middlware al estar detrás de un proxy inverso Nginx
Una vez procesada por Nginx, una solicitud de la `Accept-Encoding` se quita el encabezado. Esto evita que el middleware de la compresión de la respuesta. Para obtener más información, consulte [NGINX: compresión y descompresión](https://www.nginx.com/resources/admin-guide/compression-and-decompression/). Este problema se realiza un seguimiento por [averiguar compresión de paso a través de nginx (BasicMiddleware n.º 123)](https://github.com/aspnet/BasicMiddleware/issues/123).

## <a name="working-with-iis-dynamic-compression"></a>Trabajar con la compresión dinámica de IIS
Si tiene un módulo de compresión dinámica de IIS activo configurado en el nivel de servidor que desea deshabilitar para una aplicación, puede hacerlo con un complemento de la *web.config* archivo. Para obtener más información, consulte [módulos IIS deshabilitar](xref:hosting/iis-modules#disabling-iis-modules).

## <a name="troubleshooting"></a>Solución de problemas
Utilice una herramienta como [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), o [Postman](https://www.getpostman.com/), que permite establecer el `Accept-Encoding` encabezado de solicitud y estudiar los encabezados de respuesta, el tamaño y el cuerpo. El Middleware de compresión de respuesta comprime las respuestas que cumplen las condiciones siguientes:
* El `Accept-Encoding` encabezado está presente con un valor de `gzip`, `*`, o codificación personalizada que coincida con un proveedor de compresión personalizada que ha establecido. El valor no debe ser `identity` o tener un valor de calidad (qvalue, `q`) de 0 (cero).
* El tipo MIME (`Content-Type`) debe establecerse y debe coincidir con un tipo MIME configurado en el `ResponseCompressionOptions`.
* La solicitud no debe incluir el `Content-Range` encabezado.
* La solicitud debe utilizar inseguro protocolo (http), a menos que el protocolo seguro (https) se configura en las opciones de Middleware de compresión de respuesta. *Tenga en cuenta el peligro [se ha descrito anteriormente](#compression-with-secure-protocol) al habilitar la compresión de contenido segura.*

## <a name="additional-resources"></a>Recursos adicionales
* [Inicio de aplicaciones](xref:fundamentals/startup)
* [Middleware](xref:fundamentals/middleware)
* [Red de desarrollador de Mozilla: Codificación aceptada](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [RFC 7231 sección 3.1.2.1: Códigos contenidos](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [RFC 7230 sección 4.2.3: Codificación Gzip](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [Versión 4.3 de la especificación de formato del archivo GZIP](http://www.ietf.org/rfc/rfc1952.txt)
