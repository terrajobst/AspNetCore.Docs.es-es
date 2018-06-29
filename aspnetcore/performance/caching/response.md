---
title: Las respuestas en caché de ASP.NET Core
author: rick-anderson
description: Aprenda a usar el almacenamiento en caché a menores requisitos de ancho de banda de respuesta y aumentar el rendimiento de las aplicaciones de ASP.NET Core.
ms.author: riande
ms.date: 09/20/2017
uid: performance/caching/response
ms.openlocfilehash: c53ae3f6ab8d26588533772dd4fdacb36ec12059
ms.sourcegitcommit: 931b6a2d7eb28a0f1295e8a95690b8c4c5f58477
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2018
ms.locfileid: "37077769"
---
# <a name="response-caching-in-aspnet-core"></a>Las respuestas en caché de ASP.NET Core

Por [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), y [Luke Latham](https://github.com/guardrex)

> [!NOTE]
> Las respuestas en caché en las páginas de Razor está disponible en ASP.NET Core 2.1 o posterior.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/samples) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

Las respuestas en caché reducen el número de solicitudes de que un cliente o proxy se realiza en un servidor web. Las respuestas en caché también reducen la cantidad de trabajo realiza el servidor web para generar una respuesta. Las respuestas en caché se controlan mediante encabezados que especifican cómo desea que cliente, el proxy y middleware para la memoria caché las respuestas.

El servidor web puede almacenar en caché las respuestas al agregar [Middleware de almacenamiento en caché de respuesta](xref:performance/caching/middleware).

## <a name="http-based-response-caching"></a>Almacenamiento en caché de respuesta basado en HTTP

El [especificación HTTP 1.1 Caching](https://tools.ietf.org/html/rfc7234) describe el comportamiento de las cachés de Internet. El encabezado HTTP principal que se usa para almacenar en caché es [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), que se utiliza para especificar la caché *directivas*. Las directivas de controlan el comportamiento de almacenamiento en caché las solicitudes lleguen su desde los clientes a los servidores a medida que y respuestas lleguen su de servidores a los clientes. Mover las solicitudes y respuestas a través de servidores proxy y servidores proxy deben también ajustarse a la especificación HTTP 1.1 almacenamiento en memoria caché.

Common `Cache-Control` directivas se muestran en la tabla siguiente.

| Directiva                                                       | Acción |
| --------------------------------------------------------------- | ------ |
| [public](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)   | Una memoria caché puede almacenar la respuesta. |
| [private](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)  | No se debe almacenar la respuesta de una memoria caché compartida. Una caché privada puede almacenar y reutilizar la respuesta. |
| [max-age](https://tools.ietf.org/html/rfc7234#section-5.2.1.1)  | El cliente no aceptará una respuesta cuya antigüedad es superior al número especificado de segundos. Ejemplos: `max-age=60` (60 segundos), `max-age=2592000` (1 mes) |
| [sin caché](https://tools.ietf.org/html/rfc7234#section-5.2.1.4) | **En las solicitudes**: una memoria caché no debe usar una respuesta almacenada para satisfacer la solicitud. Nota: El servidor de origen vuelve a genera la respuesta para el cliente y el software intermedio actualiza la respuesta almacenada en la memoria caché.<br><br>**En las respuestas**: la respuesta no debe usarse para una solicitud posterior sin la validación en el servidor de origen. |
| [ningún almacén](https://tools.ietf.org/html/rfc7234#section-5.2.1.5) | **En las solicitudes**: una memoria caché no debe almacenar la solicitud.<br><br>**En las respuestas**: una memoria caché no debe almacenar cualquier parte de la respuesta. |

Otros encabezados de caché que desempeñan un papel en el almacenamiento en caché se muestran en la tabla siguiente.

| Header                                                     | Función |
| ---------------------------------------------------------- | -------- |
| [Edad](https://tools.ietf.org/html/rfc7234#section-5.1)     | Una estimación de la cantidad de tiempo en segundos desde que se generó la respuesta o se ha validado correctamente en el servidor de origen. |
| [Expira](https://tools.ietf.org/html/rfc7234#section-5.3) | La fecha y hora después de que la respuesta se considera obsoleta. |
| [Pragma](https://tools.ietf.org/html/rfc7234#section-5.4)  | Existe para hacia atrás compatibilidad con HTTP/1.0 almacena en memoria caché de configuración `no-cache` comportamiento. Si el `Cache-Control` encabezado está presente, el `Pragma` se omite el encabezado. |
| [Variar](https://tools.ietf.org/html/rfc7231#section-7.1.4)  | Especifica que una respuesta almacenada en caché no se debe enviar a menos que todos los de la `Vary` coinciden con campos de encabezado de solicitud original de la respuesta almacenada en caché y la solicitud nuevo. |

## <a name="http-based-caching-respects-request-cache-control-directives"></a>Las directivas de Control de caché de solicitud de aspectos de almacenamiento en caché basado en HTTP

El [especificación HTTP 1.1 almacenamiento en memoria caché para el encabezado Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2) requiere una memoria caché que se respeten válido `Cache-Control` encabezado enviado por el cliente. Un cliente puede realizar las solicitudes con un `no-cache` valor del encabezado y se fuerza el servidor para generar una nueva respuesta para cada solicitud.

Siempre respetando cliente `Cache-Control` encabezados de solicitud tiene sentido si considera que el objetivo del almacenamiento en caché de HTTP. En la especificación oficial, el almacenamiento en caché está pensado para reducir la sobrecarga de red y latencia de satisfacer las solicitudes a través de una red de clientes, servidores proxy y servidores. No es necesariamente una manera de controlar la carga en un servidor de origen.

No hay ningún control del desarrollador actual sobre este comportamiento de almacenamiento en caché cuando se usa el [Middleware de almacenamiento en caché de respuesta](xref:performance/caching/middleware) porque el middleware se adhiere a los oficiales especificación el almacenamiento en caché. [Mejoras futuras en el middleware](https://github.com/aspnet/ResponseCaching/issues/96) permitirá configurar el middleware para omitir una solicitud `Cache-Control` encabezado al decidir atender una respuesta almacenada en caché. Esto le ofrece la oportunidad para controlar mejor la carga en el servidor cuando se usa el middleware.

## <a name="other-caching-technology-in-aspnet-core"></a>Otras tecnologías de almacenamiento en caché de ASP.NET Core

### <a name="in-memory-caching"></a>Almacenamiento en caché en memoria

Almacenamiento en caché en memoria utiliza memoria del servidor para almacenar los datos almacenados en caché. Este tipo de almacenamiento en caché es adecuado para un único servidor o varios servidores mediante *sesiones permanentes*. Sesiones permanentes significa que siempre se enrutan las solicitudes de un cliente en el mismo servidor para su procesamiento.

Para obtener más información, consulte [almacenar en memoria caché en memoria](xref:performance/caching/memory).

### <a name="distributed-cache"></a>Caché distribuida

Usar una memoria caché distribuida para almacenar datos en la memoria cuando la aplicación se hospeda en una granja de servidores en la nube o el servidor. La memoria caché se comparte entre los servidores que procesan las solicitudes. Un cliente puede enviar una solicitud que controla cualquier servidor en el grupo si los datos almacenados en caché para el cliente están disponibles. ASP.NET Core ofrece SQL Server y las cachés de Redis distribuida.

Para obtener más información, consulte [trabajar con una memoria caché distribuida](xref:performance/caching/distributed).

### <a name="cache-tag-helper"></a>Aplicación auxiliar de etiqueta de caché

Puede almacenar en caché el contenido de una vista MVC o Razor página a la aplicación auxiliar de etiqueta de caché. La aplicación auxiliar de etiqueta de caché usa almacenamiento en caché en memoria para almacenar datos.

Para obtener más información, consulte [aplicación auxiliar de la etiqueta de caché en MVC de ASP.NET Core](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).

### <a name="distributed-cache-tag-helper"></a>Aplicación auxiliar de etiquetas de caché distribuida

Puede almacenar en caché el contenido de una vista MVC o la página de Razor en nube distribuida o escenarios de granja de servidores web con el Ayudante de etiqueta de caché distribuida. El Ayudante de etiqueta de caché distribuida usa SQL Server o Redis para almacenar datos.

Para obtener más información, consulte [auxiliar de etiqueta de caché distribuida](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper).

## <a name="responsecache-attribute"></a>Atributo ResponseCache

El [ResponseCacheAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) especifica los parámetros necesarios para establecer encabezados apropiados en las respuestas en caché.

> [!WARNING]
> Deshabilitar el almacenamiento en caché para el contenido que contiene información para clientes autenticados. Sólo debe habilitarse el almacenamiento en caché para el contenido que no cambia en función de la identidad de un usuario o si un usuario ha iniciado sesión.

[VaryByQueryKeys](/dotnet/api/microsoft.aspnetcore.mvc.responsecacheattribute.varybyquerykeys) varía la respuesta almacenada en los valores de la lista de claves de consulta determinada. Cuando un valor único de `*` es siempre el middleware varía las respuestas por todos los parámetros de cadena de consulta de solicitud. `VaryByQueryKeys` requiere ASP.NET Core 1.1 o posterior.

El Middleware de almacenamiento en caché de la respuesta debe estar habilitado para establecer el `VaryByQueryKeys` propiedad; en caso contrario, se produce una excepción en tiempo de ejecución. No hay un encabezado HTTP correspondiente para el `VaryByQueryKeys` propiedad. La propiedad es una característica HTTP controlada el Middleware de almacenamiento en caché de respuesta. Para que el middleware atender una respuesta almacenada en caché, la cadena de consulta y el valor de cadena de consulta deben coincidir con una solicitud anterior. Por ejemplo, considere la posibilidad de la secuencia de las solicitudes y resultados que se muestran en la tabla siguiente.

| Solicitud                          | Resultado                   |
| -------------------------------- | ------------------------ |
| `http://example.com?key1=value1` | Devuelta por el servidor     |
| `http://example.com?key1=value1` | Procedentes de middleware |
| `http://example.com?key1=value2` | Devuelta por el servidor     |

La primera solicitud es devuelto por el servidor y en memoria caché de middleware. Dado que la cadena de consulta coincide con la solicitud anterior, se devuelve la segunda solicitud middleware. La tercera solicitud no se encuentra en la caché de middleware porque el valor de cadena de consulta no coincide con una solicitud anterior. 

El `ResponseCacheAttribute` se utiliza para crear y configurar (a través de `IFilterFactory`) un [ResponseCacheFilter](/dotnet/api/microsoft.aspnetcore.mvc.internal.responsecachefilter). El `ResponseCacheFilter` realiza el trabajo de actualización de los encabezados HTTP adecuados y características de la respuesta. El filtro:

* Quita los encabezados existentes para `Vary`, `Cache-Control`, y `Pragma`. 
* Escribe los encabezados adecuados en función de las propiedades establecidas en el `ResponseCacheAttribute`. 
* Actualiza la respuesta si el almacenamiento en caché característica HTTP `VaryByQueryKeys` se establece.

### <a name="vary"></a>Variar

Este encabezado solo cuando se escribe el `VaryByHeader` se establece la propiedad. Se establece en el `Vary` valor de la propiedad. El siguiente ejemplo se utiliza la `VaryByHeader` propiedad:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

::: moniker-end

Puede ver los encabezados de respuesta con las herramientas de red de su explorador. La siguiente imagen muestra la F12 de borde de salida en el **red** ficha cuando el `About2` se actualiza el método de acción:

![Arista F12 salida en la pestaña de la red cuando se llama al método de acción About2](response/_static/vary.png)

### <a name="nostore-and-locationnone"></a>NoStore y Location.None

`NoStore` invalida la mayoría de las demás propiedades. Cuando esta propiedad se establece en `true`, `Cache-Control` encabezado se establece en `no-store`. Si `Location` está establecido en `None`:

* El valor de `Cache-Control` está establecido en `no-store,no-cache`.
* El valor de `Pragma` está establecido en `no-cache`.

Si `NoStore` es `false` y `Location` es `None`, `Cache-Control` y `Pragma` se establecen en `no-cache`.

Normalmente establece `NoStore` a `true` en las páginas de error. Por ejemplo:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

::: moniker-end

Esto genera los encabezados siguientes:

```
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a>Ubicación y la duración

Para habilitar el almacenamiento en caché, `Duration` debe establecerse en un valor positivo y `Location` debe ser `Any` (valor predeterminado) o `Client`. En este caso, el `Cache-Control` encabezado se establece en el valor de ubicación seguido por el `max-age` de la respuesta.

> [!NOTE]
> `Location`de opciones de `Any` y `Client` traducir `Cache-Control` valores de encabezado `public` y `private`, respectivamente. Como se indicó anteriormente, establecer `Location` a `None` establece ambas `Cache-Control` y `Pragma` encabezados para `no-cache`.

A continuación se muestra un ejemplo que muestra los encabezados genera estableciendo `Duration` y deja el valor predeterminado `Location` valor:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

::: moniker-end

Esto produce el siguiente encabezado:

```
Cache-Control: public,max-age=60
```

### <a name="cache-profiles"></a>Perfiles de memoria caché

En lugar de duplicar `ResponseCache` configuración en muchos atributos de acción de controlador, perfiles de memoria caché se puede configurar como opciones al configurar MVC en la `ConfigureServices` método `Startup`. Valores que se encuentran en un perfil de caché que se hace referencia se utilizan como los valores predeterminados por el `ResponseCache` de atributo y se reemplazan por las propiedades especificadas en el atributo.

Cómo configurar un perfil de caché:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Startup.cs?name=snippet1)]

::: moniker-end

Hacer referencia a un perfil de caché:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

::: moniker-end

El `ResponseCache` atributo se puede aplicar tanto a las acciones (métodos) y controladores (clases). Atributos de nivel de método invalidan la configuración especificada en los atributos de nivel de clase.

En el ejemplo anterior, un atributo de nivel de clase especifica una duración de 30 segundos, mientras que un atributo de nivel de método hace referencia a un perfil de caché con una duración establecida en 60 segundos.

El encabezado resultante:

```
Cache-Control: public,max-age=60
```

## <a name="additional-resources"></a>Recursos adicionales

* [Almacenar las respuestas en las cachés](https://tools.ietf.org/html/rfc7234#section-3)
* [Control de caché](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* [Almacenamiento en caché en memoria](xref:performance/caching/memory)
* [Trabajar con una memoria caché distribuida](xref:performance/caching/distributed)
* [Detectar cambios con tokens de cambio](xref:fundamentals/primitives/change-tokens)
* [Middleware de almacenamiento en caché de respuestas](xref:performance/caching/middleware)
* [Aplicación auxiliar de etiquetas de caché](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [Aplicación auxiliar de etiquetas de caché distribuida](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
