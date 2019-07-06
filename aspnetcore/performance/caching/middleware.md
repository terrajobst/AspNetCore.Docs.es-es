---
title: Respuesta de almacenamiento en caché de Middleware en ASP.NET Core
author: guardrex
description: Obtenga información sobre cómo configurar y usar Middleware de almacenamiento en caché de respuestas en ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/05/2019
uid: performance/caching/middleware
ms.openlocfilehash: d6756ce16396133da643cc08ca0f48369479ad3a
ms.sourcegitcommit: b9e914ef274b5ec359582f299724af6234dce135
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2019
ms.locfileid: "67596149"
---
# <a name="response-caching-middleware-in-aspnet-core"></a>Respuesta de almacenamiento en caché de Middleware en ASP.NET Core

Por [Halter](https://github.com/guardrex) y [John Luo](https://github.com/JunTaoLuo)

[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/middleware/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))

En este artículo se explica cómo configurar el Middleware de almacenamiento en caché de respuestas en una aplicación ASP.NET Core. El middleware determina cuando las respuestas son almacenables en caché, las respuestas de los almacenes y respuestas de sirve de memoria caché. Para obtener una introducción al almacenamiento en caché de HTTP y el [[ResponseCache]](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) atributo, vea [almacenamiento en caché de respuesta](xref:performance/caching/response).

## <a name="configuration"></a>Configuración

Use la [Microsoft.AspNetCore.App metapaquete](xref:fundamentals/metapackage-app) o agregar una referencia de paquete para el [Microsoft.AspNetCore.ResponseCaching](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/) paquete.

En `Startup.ConfigureServices`, agregue el Middleware de almacenamiento en caché de respuesta a la colección de servicios:

[!code-csharp[](middleware/samples/2.x/ResponseCachingMiddleware/Startup.cs?name=snippet1&highlight=3)]

Configurar la aplicación para usar el middleware con el <xref:Microsoft.AspNetCore.Builder.ResponseCachingExtensions.UseResponseCaching*> método de extensión, que agrega el middleware a la canalización de procesamiento de solicitudes en `Startup.Configure`:

[!code-csharp[](middleware/samples/2.x/ResponseCachingMiddleware/Startup.cs?name=snippet2&highlight=14)]

La aplicación de ejemplo agrega encabezados para controlar el almacenamiento en caché en las solicitudes posteriores:

* [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2) &ndash; almacena en caché las respuestas almacenables en caché hasta 10 segundos.
* [Variar](https://tools.ietf.org/html/rfc7231#section-7.1.4) &ndash; configura el middleware para servir un respuesta almacenada en caché solo si la [ `Accept-Encoding` ](https://tools.ietf.org/html/rfc7231#section-5.3.4) coincide con el encabezado de las solicitudes posteriores de la solicitud original.

[!code-csharp[](middleware/samples_snippets/2.x/AddHeaders.cs)]

Middleware de almacenamiento en caché de respuestas solo almacena en caché las respuestas del servidor que como resultado un código de estado 200 (OK). Otras respuestas, incluidos [páginas de error](xref:fundamentals/error-handling), se omiten el middleware.

> [!WARNING]
> Las respuestas que incluye contenido de los clientes autenticados deben marcarse como no almacenable en caché para evitar que el software intermedio de almacenar y atender las respuestas. Consulte [condiciones para almacenar en caché](#conditions-for-caching) para obtener más información sobre cómo el middleware determina si una respuesta se puede almacenar en caché.

## <a name="options"></a>Opciones

Opciones de almacenamiento en caché de respuesta se muestran en la tabla siguiente.

| Opción | Descripción |
| ------ | ----------- |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize> | El tamaño más grande almacenables en caché para el cuerpo de respuesta en bytes. El valor predeterminado es `64 * 1024 * 1024` (64 MB). |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit> | El límite de tamaño para el middleware de la memoria caché de respuesta en bytes. El valor predeterminado es `100 * 1024 * 1024` (100 MB). |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.UseCaseSensitivePaths> | Determina si se almacenan en caché las respuestas en las rutas de acceso distingue mayúsculas de minúsculas. El valor predeterminado es `false`. |

El ejemplo siguiente configura el middleware de:

* Almacenar en caché las respuestas con un tamaño de cuerpo menor o igual a 1.024 bytes.
* Store las respuestas por las rutas de acceso distingue mayúsculas de minúsculas. Por ejemplo, `/page1` y `/Page1` se almacenan por separado.

```csharp
services.AddResponseCaching(options =>
{
    options.MaximumBodySize = 1024;
    options.UseCaseSensitivePaths = true;
});
```

## <a name="varybyquerykeys"></a>VaryByQueryKeys

Al usar MVC o web de controladores de API o modelos de página de las páginas de Razor, el [[ResponseCache]](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) atributo especifica los parámetros necesarios para establecer los encabezados adecuados para el almacenamiento en caché de respuesta. El único parámetro de la `[ResponseCache]` atributo que requiere estrictamente el middleware es <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute.VaryByQueryKeys>, que no se corresponde con un encabezado HTTP real. Para obtener más información, consulta <xref:performance/caching/response#responsecache-attribute>.

Cuando no se usa el `[ResponseCache]` atributo, el almacenamiento en caché de respuesta puede modificarse con `VaryByQueryKeys`. Use la <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingFeature> directamente desde el [HttpContext.Features](xref:Microsoft.AspNetCore.Http.HttpContext.Features):

```csharp
var responseCachingFeature = context.HttpContext.Features.Get<IResponseCachingFeature>();

if (responseCachingFeature != null)
{
    responseCachingFeature.VaryByQueryKeys = new[] { "MyKey" };
}
```

Usa un solo valor igual a `*` en `VaryByQueryKeys` varía la memoria caché por todos los parámetros de consulta de solicitud.

## <a name="http-headers-used-by-response-caching-middleware"></a>Encabezados HTTP utilizadas por el Middleware de almacenamiento en caché de respuestas

En la tabla siguiente proporciona información sobre los encabezados HTTP que afectan al almacenamiento en caché de respuesta.

| Header | Detalles |
| ------ | ------- |
| `Authorization` | La respuesta no se almacenan en caché si existe el encabezado. |
| `Cache-Control` | El middleware solo tiene en cuenta de almacenamiento en caché las respuestas marcadas con el `public` la directiva de caché. Controlar el almacenamiento en caché con los parámetros siguientes:<ul><li>max-age</li><li>Max-stale&#8224;</li><li>min-nuevo</li><li>must-revalidate</li><li>no-cache</li><li>no-store</li><li>only-if-cached</li><li>private</li><li>public</li><li>s-maxage</li><li>proxy-revalidate.&#8225;</li></ul>&#8224;Si no se especifica ningún límite para `max-stale`, el middleware no realiza ninguna acción.<br>&#8225;`proxy-revalidate`tiene el mismo efecto que `must-revalidate`.<br><br>Para obtener más información, consulte [RFC 7231: Las directivas de Control de caché de solicitud](https://tools.ietf.org/html/rfc7234#section-5.2.1). |
| `Pragma` | Un `Pragma: no-cache` encabezado de la solicitud produce el mismo efecto que `Cache-Control: no-cache`. Este encabezado se haya reemplazado por las directivas correspondientes en el `Cache-Control` encabezado, si está presente. Se considera para mantener la compatibilidad con HTTP/1.0. |
| `Set-Cookie` | La respuesta no se almacenan en caché si existe el encabezado. Cualquier software intermedio en la canalización de procesamiento de solicitudes que establece una o más cookies impide que el Middleware de almacenamiento en caché de respuestas de almacenamiento en caché la respuesta (por ejemplo, el [proveedor TempData basado en cookie](xref:fundamentals/app-state#tempdata)).  |
| `Vary` | El `Vary` encabezado se usa para variar la respuesta almacenada en caché por otro encabezado. Por ejemplo, almacenar en caché las respuestas mediante la codificación mediante la inclusión de la `Vary: Accept-Encoding` encabezado, que se almacena en caché las respuestas de solicitudes con encabezados `Accept-Encoding: gzip` y `Accept-Encoding: text/plain` por separado. Una respuesta con un valor de encabezado de `*` nunca se almacena. |
| `Expires` | Una respuesta que se considera obsoleta en este encabezado no es almacenar o recuperar a menos que se reemplaza por otro `Cache-Control` encabezados. |
| `If-None-Match` | La respuesta completa se sirve desde la memoria caché si el valor no es `*` y `ETag` de la respuesta no coincide con ninguno de los valores proporcionados. En caso contrario, se envía una respuesta 304 (no modificado). |
| `If-Modified-Since` | Si el `If-None-Match` encabezado no está presente, una respuesta completa se sirve desde la memoria caché si la fecha de la respuesta almacenada en caché es más reciente que el valor proporcionado. En caso contrario, un *304 - no modificado* respuesta se sirve. |
| `Date` | Cuando se trabaja desde la memoria caché, el `Date` encabezado será ajustado por el middleware si no se proporciona en la respuesta original. |
| `Content-Length` | Cuando se trabaja desde la memoria caché, el `Content-Length` encabezado será ajustado por el middleware si no se proporciona en la respuesta original. |
| `Age` | El `Age` se omite el encabezado enviado en la respuesta original. El middleware calcula un nuevo valor al proporcionar una respuesta almacenada en caché. |

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
* En `Startup.Configure`, Middleware de almacenamiento en caché de respuestas deben colocarse antes de middleware que requieren almacenamiento en caché. Para obtener más información, consulta <xref:fundamentals/middleware/index>.
* El `Authorization` encabezado no debe estar presente.
* `Cache-Control` parámetros del encabezado deben ser válidos y se debe marcar la respuesta `public` y no marcado `private`.
* El `Pragma: no-cache` encabezado no debe estar presente si el `Cache-Control` encabezado no está presente, como la `Cache-Control` encabezado reemplaza el `Pragma` encabezado cuando está presente.
* El `Set-Cookie` encabezado no debe estar presente.
* `Vary` parámetros del encabezado deben ser válida y no es igual a `*`.
* El `Content-Length` valor del encabezado (Si establecer) debe coincidir con el tamaño del cuerpo de respuesta.
* El <xref:Microsoft.AspNetCore.Http.Features.IHttpSendFileFeature> no se usa.
* La respuesta no debe ser obsoleta según lo especificado por el `Expires` encabezado y el `max-age` y `s-maxage` las directivas de caché.
* El búfer de respuestas debe ser correcto. El tamaño de la respuesta debe ser menor que el configurado o default <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit>. El tamaño del cuerpo de la respuesta debe ser menor que el configurado o default <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize>.
* La respuesta debe ser almacenables en caché según la [RFC 7234](https://tools.ietf.org/html/rfc7234) especificaciones. Por ejemplo, el `no-store` directiva no debe existir en los campos de encabezado de solicitud o respuesta. Consulte *sección 3: Almacenar las respuestas en las memorias caché* de [RFC 7234](https://tools.ietf.org/html/rfc7234) para obtener más información.

> [!NOTE]
> El sistema antifalsificación para generar tokens de seguridad para evitar la falsificación de solicitud entre sitios (CSRF) attacks conjuntos el `Cache-Control` y `Pragma` encabezados a `no-cache` para que no se almacenan en caché las respuestas. Para obtener información sobre cómo deshabilitar los tokens antifalsificación para los elementos de formulario HTML, vea <xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration>.

## <a name="additional-resources"></a>Recursos adicionales

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
