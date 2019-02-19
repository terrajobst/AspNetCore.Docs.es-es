---
title: Respuesta de almacenamiento en caché de Middleware en ASP.NET Core
author: guardrex
description: Obtenga información sobre cómo configurar y usar Middleware de almacenamiento en caché de respuestas en ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/16/2019
uid: performance/caching/middleware
ms.openlocfilehash: bb265d04022ec2f8fdb3f2f3bc42f6b3f0b2b338
ms.sourcegitcommit: d75d8eb26c2cce19876c8d5b65ac8a4b21f625ef
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2019
ms.locfileid: "56410328"
---
# <a name="response-caching-middleware-in-aspnet-core"></a>Respuesta de almacenamiento en caché de Middleware en ASP.NET Core

Por [Halter](https://github.com/guardrex) y [John Luo](https://github.com/JunTaoLuo)

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/middleware/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample)).

En este artículo se explica cómo configurar el Middleware de almacenamiento en caché de respuestas en una aplicación ASP.NET Core. El middleware determina cuando las respuestas son almacenables en caché, las respuestas de los almacenes y respuestas de sirve de memoria caché. Para obtener una introducción al almacenamiento en caché de HTTP y el `ResponseCache` atributo, vea [almacenamiento en caché de respuesta](xref:performance/caching/response).

## <a name="package"></a>Paquete

::: moniker range=">= aspnetcore-2.1"

Referencia de la [Microsoft.AspNetCore.App metapaquete](xref:fundamentals/metapackage-app) o agregar una referencia de paquete para el [Microsoft.AspNetCore.ResponseCaching](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/) paquete.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Referencia de la [metapaquete Microsoft.AspNetCore.All](xref:fundamentals/metapackage) o agregar una referencia de paquete para el [Microsoft.AspNetCore.ResponseCaching](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/) paquete.

::: moniker-end

::: moniker range="= aspnetcore-1.1"

Agregue una referencia de paquete a la [Microsoft.AspNetCore.ResponseCaching](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/) paquete.

::: moniker-end

## <a name="configuration"></a>Configuración

En `Startup.ConfigureServices`, agregue el middleware a la colección de servicios.

[!code-csharp[](middleware/samples/2.x/ResponseCachingMiddleware/Startup.cs?name=snippet1&highlight=9)]

Configurar la aplicación para usar el middleware con el `UseResponseCaching` método de extensión, que agrega el middleware a la canalización de procesamiento de la solicitud. La aplicación de ejemplo agrega un [ `Cache-Control` ](https://tools.ietf.org/html/rfc7234#section-5.2) encabezado a la respuesta que se almacena en caché las respuestas almacenables en caché hasta 10 segundos. El ejemplo envía un [ `Vary` ](https://tools.ietf.org/html/rfc7231#section-7.1.4) encabezado para configurar el middleware para servir un respuesta almacenada en caché solo si la [ `Accept-Encoding` ](https://tools.ietf.org/html/rfc7231#section-5.3.4) coincide con el encabezado de las solicitudes posteriores de la solicitud original. En el ejemplo de código siguiente, [CacheControlHeaderValue](/dotnet/api/microsoft.net.http.headers.cachecontrolheadervalue) y [HeaderNames](/dotnet/api/microsoft.net.http.headers.headernames) requieren un `using` instrucción para el [Microsoft.Net.Http.Headers](/dotnet/api/microsoft.net.http.headers) espacio de nombres.

[!code-csharp[](middleware/samples/2.x/ResponseCachingMiddleware/Startup.cs?name=snippet2&highlight=17,22-29)]

Middleware de almacenamiento en caché de respuestas solo almacena en caché las respuestas del servidor que como resultado un código de estado 200 (OK). Otras respuestas, incluidos [páginas de error](xref:fundamentals/error-handling), se omiten el middleware.

> [!WARNING]
> Las respuestas que incluye contenido de los clientes autenticados deben marcarse como no almacenable en caché para evitar que el software intermedio de almacenar y atender las respuestas. Consulte [condiciones para almacenar en caché](#conditions-for-caching) para obtener más información sobre cómo el middleware determina si una respuesta se puede almacenar en caché.

## <a name="options"></a>Opciones

El middleware ofrece tres opciones para el control del almacenamiento en caché de respuesta.

| Opción                | Descripción |
| --------------------- | ----------- |
| UseCaseSensitivePaths | Determina si se almacenan en caché las respuestas en las rutas de acceso distingue mayúsculas de minúsculas. El valor predeterminado es `false`. |
| MaximumBodySize       | El tamaño más grande almacenables en caché para el cuerpo de respuesta en bytes. El valor predeterminado es `64 * 1024 * 1024` (64 MB). |
| SizeLimit             | El límite de tamaño para el middleware de la memoria caché de respuesta en bytes. El valor predeterminado es `100 * 1024 * 1024` (100 MB). |

El ejemplo siguiente configura el middleware de:

* Almacenar en caché las respuestas menores o iguales a 1.024 bytes.
* Store las respuestas por las rutas de acceso distingue mayúsculas de minúsculas (por ejemplo, `/page1` y `/Page1` se almacenan por separado).

```csharp
services.AddResponseCaching(options =>
{
    options.UseCaseSensitivePaths = true;
    options.MaximumBodySize = 1024;
});
```

## <a name="varybyquerykeys"></a>VaryByQueryKeys

Al usar controladores de MVC o Web API o modelos de página de las páginas de Razor, el `ResponseCache` atributo especifica los parámetros necesarios para establecer los encabezados adecuados para el almacenamiento en caché de respuesta. El único parámetro de la `ResponseCache` atributo que requiere estrictamente el middleware es `VaryByQueryKeys`, que no se corresponde con un encabezado HTTP real. Para obtener más información, consulte [del atributo ResponseCache](xref:performance/caching/response#responsecache-attribute).

Cuando no se usa el `ResponseCache` atributo, el almacenamiento en caché de respuesta puede modificarse con el `VaryByQueryKeys` característica. Use la `ResponseCachingFeature` directamente desde el `IFeatureCollection` de la `HttpContext`:

```csharp
var responseCachingFeature = context.HttpContext.Features.Get<IResponseCachingFeature>();
if (responseCachingFeature != null)
{
    responseCachingFeature.VaryByQueryKeys = new[] { "MyKey" };
}
```

Usa un solo valor igual a `*` en `VaryByQueryKeys` varía la memoria caché por todos los parámetros de consulta de solicitud.

## <a name="http-headers-used-by-response-caching-middleware"></a>Encabezados HTTP utilizadas por el Middleware de almacenamiento en caché de respuestas

Almacenamiento en caché de respuesta por el middleware se configura mediante los encabezados HTTP.

| Header | Detalles |
| ------ | ------- |
| Autorización | La respuesta no se almacenan en caché si existe el encabezado. |
| Cache-Control | El middleware solo tiene en cuenta de almacenamiento en caché las respuestas marcadas con el `public` la directiva de caché. Controlar el almacenamiento en caché con los parámetros siguientes:<ul><li>max-age</li><li>Max-stale&#8224;</li><li>min-nuevo</li><li>must-revalidate</li><li>no-cache</li><li>no-store</li><li>only-if-cached</li><li>private</li><li>public</li><li>s-maxage</li><li>proxy-revalidate.&#8225;</li></ul>&#8224;Si no se especifica ningún límite para `max-stale`, el middleware no realiza ninguna acción.<br>&#8225;`proxy-revalidate`tiene el mismo efecto que `must-revalidate`.<br><br>Para obtener más información, consulte [RFC 7231: Las directivas de Control de caché de solicitud](https://tools.ietf.org/html/rfc7234#section-5.2.1). |
| Pragma | Un `Pragma: no-cache` encabezado de la solicitud produce el mismo efecto que `Cache-Control: no-cache`. Este encabezado se haya reemplazado por las directivas correspondientes en el `Cache-Control` encabezado, si está presente. Se considera para mantener la compatibilidad con HTTP/1.0. |
| Set-Cookie | La respuesta no se almacenan en caché si existe el encabezado. Cualquier software intermedio en la canalización de procesamiento de solicitudes que establece una o más cookies impide que el Middleware de almacenamiento en caché de respuestas de almacenamiento en caché la respuesta (por ejemplo, el [proveedor TempData basado en cookie](xref:fundamentals/app-state#tempdata)).  |
| Variar | El `Vary` encabezado se usa para variar la respuesta almacenada en caché por otro encabezado. Por ejemplo, almacenar en caché las respuestas mediante la codificación mediante la inclusión de la `Vary: Accept-Encoding` encabezado, que se almacena en caché las respuestas de solicitudes con encabezados `Accept-Encoding: gzip` y `Accept-Encoding: text/plain` por separado. Una respuesta con un valor de encabezado de `*` nunca se almacena. |
| Expires | Una respuesta que se considera obsoleta en este encabezado no es almacenar o recuperar a menos que se reemplaza por otro `Cache-Control` encabezados. |
| If-None-Match | La respuesta completa se sirve desde la memoria caché si el valor no es `*` y `ETag` de la respuesta no coincide con ninguno de los valores proporcionados. En caso contrario, se envía una respuesta 304 (no modificado). |
| If-Modified-Since | Si el `If-None-Match` encabezado no está presente, una respuesta completa se sirve desde la memoria caché si la fecha de la respuesta almacenada en caché es más reciente que el valor proporcionado. En caso contrario, se envía una respuesta 304 (no modificado). |
| Fecha | Cuando se trabaja desde la memoria caché, el `Date` encabezado será ajustado por el middleware si no se proporciona en la respuesta original. |
| Longitud del contenido | Cuando se trabaja desde la memoria caché, el `Content-Length` encabezado será ajustado por el middleware si no se proporciona en la respuesta original. |
| Edad | El `Age` se omite el encabezado enviado en la respuesta original. El middleware calcula un nuevo valor al proporcionar una respuesta almacenada en caché. |

## <a name="caching-respects-request-cache-control-directives"></a>Almacenamiento en caché respeta las directivas de solicitud de Cache-Control

El middleware respeta las reglas de la [especificación HTTP 1.1 Caching](https://tools.ietf.org/html/rfc7234#section-5.2). Las reglas requieren una memoria caché que se respeten válido `Cache-Control` encabezado enviado por el cliente. En la especificación, un cliente puede realizar solicitudes con un `no-cache` fuerza el servidor para generar una nueva respuesta para cada solicitud y el valor de encabezado. Actualmente, no hay ningún control del desarrollador sobre este comportamiento de almacenamiento en caché cuando se usa el software intermedio porque el middleware se adhiere a la especificación oficial de almacenamiento en caché.

Para obtener más control sobre el comportamiento de almacenamiento en caché, explore otras características de almacenamiento en caché de ASP.NET Core. Consulte los temas siguientes:

* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>

## <a name="troubleshooting"></a>Solución de problemas

Si el comportamiento de almacenamiento en caché no es como se esperaba, confirme que las respuestas son almacenables en caché y es capaz de que se sirven desde la memoria caché. Examine los encabezados de la solicitud entrante y los encabezados de la respuesta saliente. Habilitar [registro](xref:fundamentals/logging/index) para ayudar con la depuración.

Cuando las pruebas y solución de problemas de comportamiento de almacenamiento en caché, un explorador puede establecer encabezados de solicitud que afectan al almacenamiento en caché de manera no deseada. Por ejemplo, puede establecer un explorador el `Cache-Control` encabezado a `no-cache` o `max-age=0` al actualizar una página. Las siguientes herramientas pueden establecer explícitamente los encabezados de solicitud y son preferibles para las pruebas de almacenamiento en caché:

* [Fiddler](https://www.telerik.com/fiddler)
* [Postman](https://www.getpostman.com/)

### <a name="conditions-for-caching"></a>Condiciones para almacenar en caché

* La solicitud debe dar como resultado una respuesta del servidor con un código de estado 200 (OK).
* El método de solicitud debe ser GET o HEAD.
* Middleware Terminal no debe procesar la respuesta antes de Middleware de almacenamiento en caché de respuestas.
* El `Authorization` encabezado no debe estar presente.
* `Cache-Control` parámetros del encabezado deben ser válidos y se debe marcar la respuesta `public` y no marcado `private`.
* El `Pragma: no-cache` encabezado no debe estar presente si el `Cache-Control` encabezado no está presente, como la `Cache-Control` encabezado reemplaza el `Pragma` encabezado cuando está presente.
* El `Set-Cookie` encabezado no debe estar presente.
* `Vary` parámetros del encabezado deben ser válida y no es igual a `*`.
* El `Content-Length` valor del encabezado (Si establecer) debe coincidir con el tamaño del cuerpo de respuesta.
* El [IHttpSendFileFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpsendfilefeature) no se usa.
* La respuesta no debe ser obsoleta según lo especificado por el `Expires` encabezado y el `max-age` y `s-maxage` las directivas de caché.
* Almacenamiento en búfer de respuesta debe ser correcta, y el tamaño de la respuesta debe ser menor que el configurado o default `SizeLimit`.
* La respuesta debe ser almacenables en caché según la [RFC 7234](https://tools.ietf.org/html/rfc7234) especificaciones. Por ejemplo, el `no-store` directiva no debe existir en los campos de encabezado de solicitud o respuesta. Consulte *sección 3: Almacenar las respuestas en las memorias caché* de [RFC 7234](https://tools.ietf.org/html/rfc7234) para obtener más información.

> [!NOTE]
> El sistema antifalsificación para generar tokens de seguridad para evitar la falsificación de solicitud entre sitios (CSRF) attacks conjuntos el `Cache-Control` y `Pragma` encabezados a `no-cache` para que no se almacenan en caché las respuestas. Para obtener información sobre cómo deshabilitar los tokens antifalsificación para los elementos de formulario HTML, vea [configuración de ASP.NET Core antifalsificación](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration).

## <a name="additional-resources"></a>Recursos adicionales

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
