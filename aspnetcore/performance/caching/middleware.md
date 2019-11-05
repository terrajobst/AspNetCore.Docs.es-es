---
title: Middleware de almacenamiento en caché de respuesta en ASP.NET Core
author: guardrex
description: Obtenga información sobre cómo configurar y usar Middleware de almacenamiento en caché de respuestas en ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/04/2019
uid: performance/caching/middleware
ms.openlocfilehash: a8e656e1d59114e2e953323e98e0a2399efca98a
ms.sourcegitcommit: 09f4a5ded39cc8204576fe801d760bd8b611f3aa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2019
ms.locfileid: "73611453"
---
# <a name="response-caching-middleware-in-aspnet-core"></a>Middleware de almacenamiento en caché de respuesta en ASP.NET Core

Por [Luke Latham](https://github.com/guardrex) y [John Luo](https://github.com/JunTaoLuo)

[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/middleware/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))

En este artículo se explica cómo configurar middleware de almacenamiento en caché de respuestas en una aplicación ASP.NET Core. El middleware determina cuándo se pueden almacenar en caché las respuestas, almacena las respuestas y atiende las respuestas de la memoria caché. Para obtener una introducción al almacenamiento en caché de HTTP y el atributo [[ResponseCache]](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) , consulte el artículo sobre el [almacenamiento en caché de respuestas](xref:performance/caching/response).

## <a name="configuration"></a>Configuración

::: moniker range=">= aspnetcore-3.0"

El middleware de almacenamiento en caché de respuestas está disponible implícitamente para ASP.NET Core aplicaciones a través del marco de trabajo compartido.

En `Startup.ConfigureServices`, agregue el middleware de almacenamiento en caché de respuesta a la colección de servicios:

[!code-csharp[](middleware/samples/3.x/ResponseCachingMiddleware/Startup.cs?name=snippet1&highlight=3)]

Configure la aplicación para usar el middleware con el método de extensión <xref:Microsoft.AspNetCore.Builder.ResponseCachingExtensions.UseResponseCaching*>, que agrega el middleware a la canalización de procesamiento de solicitudes en `Startup.Configure`:

[!code-csharp[](middleware/samples/3.x/ResponseCachingMiddleware/Startup.cs?name=snippet2&highlight=16)]

La aplicación de ejemplo agrega encabezados para controlar el almacenamiento en caché en solicitudes posteriores:

* [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2) &ndash; almacena en caché las respuestas almacenables en caché durante 10 segundos.
* [Vary](https://tools.ietf.org/html/rfc7231#section-7.1.4) &ndash; configura el middleware para dar servicio a una respuesta almacenada en caché solo si el encabezado [`Accept-Encoding`](https://tools.ietf.org/html/rfc7231#section-5.3.4) de las solicitudes posteriores coincide con el de la solicitud original.

[!code-csharp[](middleware/samples_snippets/3.x/AddHeaders.cs)]

El middleware de almacenamiento en caché de respuestas solo almacena en caché las respuestas del servidor que dan como resultado un código de estado 200 (correcto). El middleware omite cualquier otra respuesta, incluidas [las páginas de error](xref:fundamentals/error-handling).

> [!WARNING]
> Las respuestas que contienen contenido para clientes autenticados deben marcarse como no almacenables en caché para evitar que el middleware almacene y proporcione dichas respuestas. Consulte [condiciones para el almacenamiento en caché](#conditions-for-caching) para obtener más información sobre cómo el middleware determina si una respuesta se almacena en caché.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Use el [metapaquete Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) o agregue una referencia de paquete al paquete [Microsoft. AspNetCore. ResponseCaching](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/) .

En `Startup.ConfigureServices`, agregue el middleware de almacenamiento en caché de respuesta a la colección de servicios:

[!code-csharp[](middleware/samples/2.x/ResponseCachingMiddleware/Startup.cs?name=snippet1&highlight=3)]

Configure la aplicación para usar el middleware con el método de extensión <xref:Microsoft.AspNetCore.Builder.ResponseCachingExtensions.UseResponseCaching*>, que agrega el middleware a la canalización de procesamiento de solicitudes en `Startup.Configure`:

[!code-csharp[](middleware/samples/2.x/ResponseCachingMiddleware/Startup.cs?name=snippet2&highlight=14)]

La aplicación de ejemplo agrega encabezados para controlar el almacenamiento en caché en solicitudes posteriores:

* [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2) &ndash; almacena en caché las respuestas almacenables en caché durante 10 segundos.
* [Vary](https://tools.ietf.org/html/rfc7231#section-7.1.4) &ndash; configura el middleware para dar servicio a una respuesta almacenada en caché solo si el encabezado [`Accept-Encoding`](https://tools.ietf.org/html/rfc7231#section-5.3.4) de las solicitudes posteriores coincide con el de la solicitud original.

[!code-csharp[](middleware/samples_snippets/2.x/AddHeaders.cs)]

El middleware de almacenamiento en caché de respuestas solo almacena en caché las respuestas del servidor que dan como resultado un código de estado 200 (correcto). El middleware omite cualquier otra respuesta, incluidas [las páginas de error](xref:fundamentals/error-handling).

> [!WARNING]
> Las respuestas que contienen contenido para clientes autenticados deben marcarse como no almacenables en caché para evitar que el middleware almacene y proporcione dichas respuestas. Consulte [condiciones para el almacenamiento en caché](#conditions-for-caching) para obtener más información sobre cómo el middleware determina si una respuesta se almacena en caché.

::: moniker-end

## <a name="options"></a>Opciones

En la tabla siguiente se muestran las opciones de almacenamiento en caché de respuestas.

| Opción | Descripción |
| ------ | ----------- |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize> | Tamaño de almacenamiento en caché más grande para el cuerpo de respuesta en bytes. El valor predeterminado es `64 * 1024 * 1024` (64 MB). |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit> | Límite de tamaño para el middleware de la caché de respuesta en bytes. El valor predeterminado es `100 * 1024 * 1024` (100 MB). |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.UseCaseSensitivePaths> | Determina si las respuestas se almacenan en caché en rutas de acceso que distinguen mayúsculas de minúsculas. El valor predeterminado es `false`. |

En el ejemplo siguiente se configura el middleware para:

* Almacenar en caché las respuestas con un tamaño de cuerpo menor o igual que 1.024 bytes.
* Almacenar las respuestas por rutas de acceso que distinguen mayúsculas de minúsculas. Por ejemplo, `/page1` y `/Page1` se almacenan por separado.

```csharp
services.AddResponseCaching(options =>
{
    options.MaximumBodySize = 1024;
    options.UseCaseSensitivePaths = true;
});
```

## <a name="varybyquerykeys"></a>VaryByQueryKeys

Al usar los controladores de páginas de MVC/Web API o Razor Pages modelos de página, el atributo [[ResponseCache]](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) especifica los parámetros necesarios para establecer los encabezados adecuados para el almacenamiento en caché de respuestas. El único parámetro del `[ResponseCache]` atributo que requiere estrictamente el middleware es <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute.VaryByQueryKeys>, que no se corresponde con un encabezado HTTP real. Para obtener más información, vea <xref:performance/caching/response#responsecache-attribute>.

Cuando no se usa el atributo `[ResponseCache]`, el almacenamiento en caché de respuestas puede variar con `VaryByQueryKeys`. Use el <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingFeature> directamente desde [HttpContext. Features](xref:Microsoft.AspNetCore.Http.HttpContext.Features):

```csharp
var responseCachingFeature = context.HttpContext.Features.Get<IResponseCachingFeature>();

if (responseCachingFeature != null)
{
    responseCachingFeature.VaryByQueryKeys = new[] { "MyKey" };
}
```

El uso de un valor único igual a `*` en `VaryByQueryKeys` varía la memoria caché por todos los parámetros de consulta de solicitud.

## <a name="http-headers-used-by-response-caching-middleware"></a>Encabezados HTTP usados por middleware de almacenamiento en caché de respuestas

En la tabla siguiente se proporciona información sobre los encabezados HTTP que afectan al almacenamiento en caché de las respuestas.

| Header | Detalles |
| ------ | ------- |
| `Authorization` | Si el encabezado existe, la respuesta no se almacena en caché. |
| `Cache-Control` | El middleware solo tiene en cuenta las respuestas de almacenamiento en caché marcadas con la Directiva de caché de `public`. Controlar el almacenamiento en caché con los parámetros siguientes:<ul><li>Max-Age</li><li>Max: obsoleto&#8224;</li><li>mín. actualizado</li><li>debe volver a validar</li><li>no-cache</li><li>sin almacén</li><li>solo si se almacena en caché</li><li>private</li><li>public</li><li>s-maxage</li><li>proxy: revalidar&#8225;</li></ul>&#8224;Si no se especifica ningún límite para `max-stale`, el middleware no realiza ninguna acción.<br>&#8225;`proxy-revalidate` tiene el mismo efecto que `must-revalidate`.<br><br>Para obtener más información, vea [RFC 7231: directivas de control de caché de solicitudes](https://tools.ietf.org/html/rfc7234#section-5.2.1). |
| `Pragma` | Un encabezado `Pragma: no-cache` en la solicitud produce el mismo efecto que `Cache-Control: no-cache`. Este encabezado es invalidado por las directivas pertinentes del encabezado `Cache-Control`, si está presente. Se tiene en cuenta para la compatibilidad con versiones anteriores con HTTP/1.0. |
| `Set-Cookie` | Si el encabezado existe, la respuesta no se almacena en caché. Cualquier middleware de la canalización de procesamiento de solicitudes que establece una o más cookies impide que el middleware de almacenamiento en caché de la respuesta almacene en caché la respuesta (por ejemplo, el [proveedor TempData basado en cookies](xref:fundamentals/app-state#tempdata)).  |
| `Vary` | El encabezado `Vary` se usa para modificar la respuesta almacenada en caché por otro encabezado. Por ejemplo, almacenar en memoria caché las respuestas mediante la codificación incluyendo el encabezado `Vary: Accept-Encoding`, que almacena en caché las respuestas de las solicitudes con encabezados `Accept-Encoding: gzip` y `Accept-Encoding: text/plain` por separado. Nunca se almacena una respuesta con un valor de encabezado de `*`. |
| `Expires` | Una respuesta considerada obsoleta por este encabezado no se almacena ni se recupera a menos que otros encabezados de `Cache-Control` invalide. |
| `If-None-Match` | La respuesta completa se sirve desde la memoria caché si el valor no es `*` y el `ETag` de la respuesta no coincide con ninguno de los valores proporcionados. De lo contrario, se proporciona una respuesta 304 (no modificada). |
| `If-Modified-Since` | Si no está presente el encabezado `If-None-Match`, se proporciona una respuesta completa desde la memoria caché si la fecha de respuesta almacenada en caché es más reciente que el valor proporcionado. De lo contrario, se proporciona una respuesta *304 no modificada* . |
| `Date` | Cuando se atiende desde la memoria caché, el middleware establece el encabezado de `Date` si no se proporcionó en la respuesta original. |
| `Content-Length` | Cuando se atiende desde la memoria caché, el middleware establece el encabezado de `Content-Length` si no se proporcionó en la respuesta original. |
| `Age` | Se omite el encabezado `Age` enviado en la respuesta original. El middleware calcula un nuevo valor cuando se atiende a una respuesta almacenada en caché. |

## <a name="caching-respects-request-cache-control-directives"></a>El almacenamiento en caché respeta las directivas de control de caché de solicitudes

El middleware respeta las reglas de la [especificación de almacenamiento en caché HTTP 1,1](https://tools.ietf.org/html/rfc7234#section-5.2). Las reglas requieren una memoria caché para respetar un encabezado de `Cache-Control` válido enviado por el cliente. En la especificación, un cliente puede realizar solicitudes con un valor de encabezado `no-cache` y forzar al servidor a generar una nueva respuesta para cada solicitud. Actualmente, no hay ningún control de desarrollador sobre este comportamiento de almacenamiento en caché al usar el middleware porque el middleware se adhiere a la especificación oficial de almacenamiento en caché.

Para obtener más control sobre el comportamiento del almacenamiento en caché, explore otras características de almacenamiento en caché de ASP.NET Core. Consulte los temas siguientes:

* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>

## <a name="troubleshooting"></a>Solución de problemas

Si el comportamiento de almacenamiento en caché no es el esperado, confirme que las respuestas se pueden almacenar en caché y que se puede atender desde la memoria caché. Examine los encabezados entrantes de la solicitud y los encabezados de salida de la respuesta. Habilitar el [registro](xref:fundamentals/logging/index) para ayudar con la depuración.

Al probar y solucionar problemas de comportamiento de almacenamiento en caché, un explorador puede establecer encabezados de solicitud que afectan al almacenamiento en caché de maneras no deseadas. Por ejemplo, un explorador puede establecer el encabezado `Cache-Control` en `no-cache` o `max-age=0` al actualizar una página. Las herramientas siguientes pueden establecer explícitamente encabezados de solicitud y son preferibles para probar el almacenamiento en caché:

* [Fiddler](https://www.telerik.com/fiddler)
* [Postman](https://www.getpostman.com/)

### <a name="conditions-for-caching"></a>Condiciones para el almacenamiento en caché

* La solicitud debe dar como resultado una respuesta del servidor con un código de estado 200 (correcto).
* El método de solicitud debe ser GET o HEAD.
* En `Startup.Configure`, el middleware de almacenamiento en caché de respuestas debe colocarse antes que el middleware que requiere almacenamiento en caché. Para obtener más información, vea <xref:fundamentals/middleware/index>.
* El encabezado `Authorization` no debe estar presente.
* `Cache-Control` parámetros de encabezado deben ser válidos y la respuesta debe estar marcada como `public` y no estar marcada como `private`.
* El encabezado `Pragma: no-cache` no debe estar presente si el encabezado `Cache-Control` no está presente, ya que el encabezado `Cache-Control` invalida el encabezado `Pragma` cuando está presente.
* El encabezado `Set-Cookie` no debe estar presente.
* `Vary` parámetros de encabezado deben ser válidos y no ser iguales a `*`.
* El valor del encabezado `Content-Length` (si se establece) debe coincidir con el tamaño del cuerpo de la respuesta.
* No se usa el <xref:Microsoft.AspNetCore.Http.Features.IHttpSendFileFeature>.
* La respuesta no debe estar obsoleta según lo especificado en el encabezado `Expires` y las directivas `max-age` y `s-maxage` cache.
* El almacenamiento en búfer de respuesta debe ser correcto. El tamaño de la respuesta debe ser menor que el <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit>configurado o predeterminado. El tamaño del cuerpo de la respuesta debe ser menor que el <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize>configurado o predeterminado.
* La respuesta debe almacenarse en caché de acuerdo con las especificaciones [RFC 7234](https://tools.ietf.org/html/rfc7234) . Por ejemplo, la Directiva `no-store` no debe existir en los campos de encabezado de solicitud o respuesta. Vea la *sección 3: almacenar respuestas en cachés* de [RFC 7234](https://tools.ietf.org/html/rfc7234) para obtener más información.

> [!NOTE]
> El sistema antifalsificación para generar tokens seguros para evitar ataques de falsificación de solicitud entre sitios (CSRF) establece los encabezados `Cache-Control` y `Pragma` en `no-cache` de modo que las respuestas no estén almacenadas en caché. Para obtener información sobre cómo deshabilitar los tokens antifalsificación para los elementos de formulario HTML, vea <xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration>.

## <a name="additional-resources"></a>Recursos adicionales

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
