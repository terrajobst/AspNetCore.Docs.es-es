---
title: Respuesta de almacenamiento en caché de Middleware en ASP.NET Core
author: guardrex
description: Obtenga información acerca de cómo configurar y usar el Middleware de almacenamiento en caché de respuesta en ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/26/2017
ms.prod: asp.net-core
ms.topic: article
uid: performance/caching/middleware
ms.openlocfilehash: ff92b032fe8bbbcb7bc26a34fdfbc56a0fcc0e2c
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2018
---
# <a name="response-caching-middleware-in-aspnet-core"></a>Respuesta de almacenamiento en caché de Middleware en ASP.NET Core

Por [Luke Latham](https://github.com/guardrex) y [John Luo](https://github.com/JunTaoLuo)

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/middleware/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

En este artículo se explica cómo configurar el Middleware de almacenamiento en caché de respuesta en una aplicación de ASP.NET Core. El middleware determina cuando las respuestas son almacenable en caché, las respuestas de los almacenes y actúa las respuestas de caché. Para obtener una introducción al almacenamiento en caché de HTTP y el `ResponseCache` de atributo, vea [las respuestas en caché](xref:performance/caching/response).

## <a name="package"></a>Package

Para incluir el software intermedio en un proyecto, agregue una referencia a la [ `Microsoft.AspNetCore.ResponseCaching` ](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/) empaquetar o usar el [ `Microsoft.AspNetCore.All` ](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) paquete (núcleo ASP.NET 2.0 o posterior cuando el destino es .NET Core).

## <a name="configuration"></a>Configuración

En `ConfigureServices`, agregue el middleware a la colección de servicio.

[!code-csharp[](middleware/sample/Startup.cs?name=snippet1&highlight=3)]

Configurar la aplicación para usar el middleware con el `UseResponseCaching` método de extensión, que agrega el middleware a la canalización de procesamiento de la solicitud. La aplicación de ejemplo agrega un [ `Cache-Control` ](https://tools.ietf.org/html/rfc7234#section-5.2) encabezado a la respuesta que se almacena en caché las respuestas almacenable en caché durante 10 segundos. El ejemplo envía una [ `Vary` ](https://tools.ietf.org/html/rfc7231#section-7.1.4) encabezado para configurar el middleware para dar servicio a un respuesta almacenada en caché solo si la [ `Accept-Encoding` ](https://tools.ietf.org/html/rfc7231#section-5.3.4) coincide con el encabezado de las solicitudes posteriores de la solicitud original.

[!code-csharp[](middleware/sample/Startup.cs?name=snippet2&highlight=3,7-12)]

Middleware de almacenamiento en caché de respuesta sólo almacena en caché las respuestas del servidor que dan como resultado un código de estado 200 (OK). Otras respuestas, incluidos los [páginas de errores](xref:fundamentals/error-handling), se omiten el middleware.

> [!WARNING]
> Las respuestas que incluye contenido para clientes autenticados deben marcarse como no almacenable en caché para evitar que el middleware de almacenamiento y envío de las respuestas. Vea [condiciones para almacenar en caché](#conditions-for-caching) para obtener más información acerca de cómo el middleware determina si es almacenable en caché una respuesta.

## <a name="options"></a>Opciones

El middleware ofrece tres opciones para controlar las respuestas en caché.

| Opción                | Valor predeterminado |
| --------------------- | ------------- |
| UseCaseSensitivePaths | Determina si se almacenan en caché las respuestas en las rutas de acceso entre mayúsculas y minúsculas.</p><p>El valor predeterminado es `false`. |
| MaximumBodySize       | El mayor tamaño almacenable en caché para el cuerpo de respuesta en bytes.</p>El valor predeterminado es `64 * 1024 * 1024` (64 MB). |
| SizeLimit             | El límite de tamaño para el middleware de caché de respuesta en bytes. El valor predeterminado es `100 * 1024 * 1024` (100 MB). |

En el ejemplo siguiente se configura el middleware de:

* Almacenar en caché las respuestas menores o iguales a 1.024 bytes.
* Almacene las respuestas por las rutas de acceso entre mayúsculas y minúsculas (por ejemplo, `/page1` y `/Page1` se almacenan por separado).

```csharp
services.AddResponseCaching(options =>
{
    options.UseCaseSensitivePaths = true;
    options.MaximumBodySize = 1024;
});
```

## <a name="varybyquerykeys"></a>VaryByQueryKeys

Al utilizar controladores de MVC o Web API o modelos de página de las páginas de Razor, el `ResponseCache` atributo especifica los parámetros necesarios para establecer los encabezados adecuados para las respuestas en caché. El único parámetro de la `ResponseCache` atributo que estrictamente requiere el software intermedio es `VaryByQueryKeys`, que no se corresponde con un encabezado HTTP real. Para obtener más información, consulte [ResponseCache atributo](xref:performance/caching/response#responsecache-attribute).

Si no usa el `ResponseCache` las respuestas en caché pueden variar atributo, con el `VaryByQueryKeys` característica. Use la `ResponseCachingFeature` directamente desde el `IFeatureCollection` de la `HttpContext`:

```csharp
var responseCachingFeature = context.HttpContext.Features.Get<IResponseCachingFeature>();
if (responseCachingFeature != null)
{
    responseCachingFeature.VaryByQueryKeys = new[] { "MyKey" };
}
```

Usa un solo valor igual a `*` en `VaryByQueryKeys` varía en la memoria caché por todos los parámetros de consulta de solicitud.

## <a name="http-headers-used-by-response-caching-middleware"></a>Encabezados HTTP utilizadas por el Middleware de almacenamiento en caché de respuesta

Las respuestas en caché el middleware se configura mediante encabezados HTTP.

| Header | Detalles |
| ------ | ------- |
| Autorización | La respuesta no está almacenado en memoria caché si el encabezado no existe. |
| Cache-Control | El middleware solamente tiene en cuenta el almacenamiento en caché las respuestas que se marca con el `public` directiva de caché. Controlar el almacenamiento en caché con los parámetros siguientes:<ul><li>max-age</li><li>max-stale&#8224;</li><li>min-nuevo</li><li>Directiva must-revalidate</li><li>sin caché</li><li>ningún almacén</li><li>solo if-almacenamiento en caché</li><li>private</li><li>public</li><li>s-maxage</li><li>proxy-revalidate&#8225;</li></ul>&#8224;Si no se especifica ningún límite para `max-stale`, el middleware no realiza ninguna acción.<br>&#8225;`proxy-revalidate`tiene el mismo efecto que `must-revalidate`.<br><br>Para obtener más información, consulte [RFC 7231: solicitar directivas de Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2.1). |
| Pragma | A `Pragma: no-cache` encabezado en la solicitud produce el mismo efecto que `Cache-Control: no-cache`. Este encabezado se haya reemplazado por las directivas correspondientes en el `Cache-Control` encabezado, si está presente. Cuenta para mantener la compatibilidad con HTTP/1.0. |
| Set-Cookie | La respuesta no está almacenado en memoria caché si el encabezado no existe. |
| Variar | El `Vary` encabezado se utiliza para modificar la respuesta almacenada en caché por otro encabezado. Por ejemplo, almacenar en caché las respuestas mediante la codificación mediante la inclusión de la `Vary: Accept-Encoding` encabezado, que se almacena en caché las respuestas para las solicitudes con encabezados `Accept-Encoding: gzip` y `Accept-Encoding: text/plain` por separado. Una respuesta con un valor de encabezado de `*` nunca se almacena. |
| Expira | Una respuesta considera obsoleta por este encabezado no almacenan o recuperan a menos que se reemplaza por otro `Cache-Control` encabezados. |
| If-None-Match | La respuesta completa se sirve desde la memoria caché si el valor no es `*` y `ETag` de la respuesta no coincide con ninguno de los valores proporcionados. En caso contrario, se otorga una respuesta 304 (no modificado). |
| If-Modified-Since | Si el `If-None-Match` encabezado no está presente, una respuesta completa se sirve desde la memoria caché si la fecha de la respuesta almacenada en caché es más reciente que el valor proporcionado. En caso contrario, se otorga una respuesta 304 (no modificado). |
| Fecha | Cuando se trabaja desde la memoria caché, el `Date` encabezado será ajustado por el middleware si no se proporcionó en la respuesta original. |
| Longitud del contenido | Cuando se trabaja desde la memoria caché, el `Content-Length` encabezado será ajustado por el middleware si no se proporcionó en la respuesta original. |
| Edad | El `Age` encabezado enviado en la respuesta original se omite. El middleware calcula un nuevo valor al ofrecer servicio a una respuesta almacenada en caché. |

## <a name="caching-respects-request-cache-control-directives"></a>Almacenamiento en caché respeta las directivas de solicitud Cache-Control

El middleware respeta las reglas de la [especificación HTTP 1.1 Caching](https://tools.ietf.org/html/rfc7234#section-5.2). Las reglas requieren una memoria caché que se respeten válido `Cache-Control` encabezado enviado por el cliente. En la especificación, un cliente puede realizar las solicitudes con un `no-cache` valor del encabezado y se fuerza el servidor para generar una nueva respuesta para cada solicitud. Actualmente no hay ningún control del desarrollador sobre este comportamiento de almacenamiento en caché cuando se usa el middleware porque el middleware se adhiere a la especificación oficial de almacenamiento en caché.

Para tener más control sobre el almacenamiento en caché de comportamiento, explore otras características de almacenamiento en caché de ASP.NET Core. Consulte los temas siguientes:

* [En la memoria de caché](xref:performance/caching/memory)
* [Trabajar con una memoria caché distribuida](xref:performance/caching/distributed)
* [Almacenar en caché auxiliar de etiqueta en el núcleo de ASP.NET MVC](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [Aplicación auxiliar de etiquetas de caché distribuida](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)

## <a name="troubleshooting"></a>Solución de problemas

Si almacenamiento en caché de comportamiento no es como se esperaba, confirme que las respuestas son almacenable en caché y puede que se va a obtener de la caché. Examine los encabezados de la solicitud entrante y encabezados de salida de la respuesta. Habilitar [registro](xref:fundamentals/logging/index) para ayudar con la depuración.

Al probar y solucionar problemas de comportamiento de almacenamiento en caché, un explorador puede establecer encabezados de solicitud que afectan al almacenamiento en caché de maneras no deseados. Por ejemplo, puede establecer un explorador la `Cache-Control` encabezado a `no-cache` o `max-age=0` al actualizar una página. Las siguientes herramientas pueden establecer explícitamente los encabezados de solicitud y son preferibles para las pruebas de almacenamiento en caché:

* [Fiddler](https://www.telerik.com/fiddler)
* [Postman](https://www.getpostman.com/)

### <a name="conditions-for-caching"></a>Condiciones para el almacenamiento en caché

* La solicitud debe producir una respuesta del servidor con un código de estado 200 (OK).
* El método de solicitud debe ser GET o HEAD.
* Middleware de Terminal, como [Middleware de archivos estáticos](xref:fundamentals/static-files), no se debe procesar la respuesta antes de Middleware de almacenamiento en caché de respuesta.
* El `Authorization` encabezado no debe estar presente.
* `Cache-Control` parámetros del encabezado deben ser válidos y debe marcarse la respuesta `public` y no marcado como `private`.
* El `Pragma: no-cache` encabezado no debe estar presente si el `Cache-Control` encabezado no está presente, como la `Cache-Control` encabezado reemplaza la `Pragma` encabezado cuando está presente.
* El `Set-Cookie` encabezado no debe estar presente.
* `Vary` parámetros del encabezado deben ser válida y no es igual a `*`.
* La `Content-Length` valor de encabezado (si establece) debe coincidir con el tamaño del cuerpo de respuesta.
* El [IHttpSendFileFeature](/aspnet/core/api/microsoft.aspnetcore.http.features.ihttpsendfilefeature) no se utiliza.
* La respuesta no debe ser obsoleta según lo especificado por el `Expires` encabezado y el `max-age` y `s-maxage` directivas de caché.
* Búfer de respuesta debe ser correcta, y el tamaño de la respuesta debe ser menor que el configurado o default `SizeLimit`.
* La respuesta debe ser almacenable en caché según el [RFC 7234](https://tools.ietf.org/html/rfc7234) especificaciones. Por ejemplo, el `no-store` directiva no debe existir en los campos de encabezado de solicitud o respuesta. Vea *sección 3: almacenar respuestas en las cachés* de [RFC 7234](https://tools.ietf.org/html/rfc7234) para obtener más información.

> [!NOTE]
> El sistema Antiforgery para generar tokens seguros para evitar la falsificación de solicitud entre sitios (CSRF) ataques conjuntos el `Cache-Control` y `Pragma` encabezados a `no-cache` para que no se almacenan en caché las respuestas.

## <a name="additional-resources"></a>Recursos adicionales

* [Inicio de aplicaciones](xref:fundamentals/startup)
* [Middleware](xref:fundamentals/middleware/index)
* [En la memoria de caché](xref:performance/caching/memory)
* [Trabajar con una memoria caché distribuida](xref:performance/caching/distributed)
* [Detectar cambios con tokens de cambio](xref:fundamentals/primitives/change-tokens)
* [Almacenamiento en caché de respuestas](xref:performance/caching/response)
* [Aplicación auxiliar de etiquetas de caché](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [Aplicación auxiliar de etiquetas de caché distribuida](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
