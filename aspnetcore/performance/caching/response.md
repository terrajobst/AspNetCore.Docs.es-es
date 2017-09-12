---
title: "Las respuestas en caché"
author: rick-anderson
description: "Explica cómo utilizar el almacenamiento en caché para reducir el ancho de banda y mejorar el rendimiento de respuesta."
keywords: "Núcleo de ASP.NET, almacenamiento en caché, encabezados HTTP de respuesta"
ms.author: riande
manager: wpickett
ms.date: 7/10/2017
ms.topic: article
ms.assetid: cb42035a-60b0-472e-a614-cb79f443f654
ms.prod: asp.net-core
uid: performance/caching/response
ms.openlocfilehash: 8b20ac70f257213ae3830749e729156ee5ab6447
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2017
---
# <a name="response-caching"></a>Las respuestas en caché

Por [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), y [Steve Smith](https://ardalis.com/)

[Ver o descargar el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/sample)

## <a name="what-is-response-caching"></a>¿Qué es el almacenamiento en caché de respuesta

*Las respuestas en caché* aumenta encabezados relacionados con la memoria caché las respuestas. Estos encabezados especifican cómo desea que cliente, el proxy y middleware para la memoria caché las respuestas. Las respuestas en caché pueden reducir el número de solicitudes de que un cliente o proxy realiza al servidor web. Las respuestas en caché también pueden reducir la cantidad de trabajo realiza el servidor web para generar la respuesta. 

El encabezado HTTP principal que se usa para almacenar en caché es `Cache-Control`. Consulte la [caché de HTTP 1.1](https://tools.ietf.org/html/rfc7234#section-5.2) para obtener más información. Directivas de caché comunes:

* [public](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)
* [private](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)
* [sin caché](https://tools.ietf.org/html/rfc7234#section-5.2.1.4)
* [Pragma](https://tools.ietf.org/html/rfc7234#section-5.4)
* [Variar](https://tools.ietf.org/html/rfc7231#section-7.1.4)

El servidor web puede almacenar en caché las respuestas mediante la adición de la respuesta de almacenamiento en caché de middleware. Vea [respuesta almacenamiento en caché middleware](middleware.md) para obtener más información.

## <a name="distributed-cache-tag-helper"></a>Aplicación auxiliar de etiqueta de caché distribuida

El [auxiliar de etiqueta de caché distribuida](xref:mvc/views/tag-helpers/builtin-th/DistributedCacheTagHelper) permite caching distribuido.


## <a name="responsecache-attribute"></a>Atributo ResponseCache

El `ResponseCacheAttribute` especifica los parámetros necesarios para establecer encabezados apropiados en las respuestas en caché. Vea [ResponseCacheAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.responsecacheattribute) para obtener una descripción de los parámetros.

>[!WARNING]
> Deshabilitar el almacenamiento en caché para el contenido que contiene información para clientes autenticados. Almacenamiento en caché solo debería habilitarse para contenido que no cambia en función de la identidad de un usuario, o si un usuario ha iniciado sesión.

`VaryByQueryKeys string[]`(requiere ASP.NET Core 1.1.0 y versiones posteriores): cuando se establece, la respuesta de almacenamiento en caché middleware variará la respuesta almacenada en los valores de la lista de claves de consulta determinada. La respuesta middleware el almacenamiento en caché debe estar habilitada para establecer el `VaryByQueryKeys` propiedad, en caso contrario, una excepción en tiempo de ejecución se producirá. No hay ningún encabezado HTTP correspondiente para el `VaryByQueryKeys` propiedad. Esta propiedad es una característica HTTP controlada la respuesta middleware el almacenamiento en caché. Para que el middleware atender una respuesta almacenada en caché, la cadena de consulta y el valor de cadena de consulta deben coincidir con una solicitud anterior. Por ejemplo, considere la siguiente secuencia:

| Solicitud          | Resultado |
| ----------------- | ------------ | 
| `http://example.com?key1=value1` | devuelta por el servidor |
| `http://example.com?key1=value1` | procedentes de middleware |
| `http://example.com?key1=value2` | devuelta por el servidor |

La primera solicitud es devuelto por el servidor y en memoria caché de middleware. Dado que la cadena de consulta coincide con la solicitud anterior, se devuelve la segunda solicitud middleware. La tercera solicitud no está en la caché de middleware porque el valor de cadena de consulta no coincide con una solicitud anterior. 

El `ResponseCacheAttribute` se utiliza para crear y configurar (a través de `IFilterFactory`) un `ResponseCacheFilter`. El `ResponseCacheFilter` realiza el trabajo de actualización de los encabezados HTTP adecuados y características de la respuesta. El filtro:

* Quita los encabezados existentes para `Vary`, `Cache-Control`, y `Pragma`. 
* Escribe los encabezados adecuados en función de las propiedades establecidas en el `ResponseCacheAttribute`. 
* Actualiza la respuesta si el almacenamiento en caché característica HTTP `VaryByQueryKeys` se establece.

### <a name="vary"></a>Variar

Este encabezado solo cuando se escribe el `VaryByHeader` se establece la propiedad. Se establece en el `Vary` valor de la propiedad. El siguiente ejemplo se utiliza la `VaryByHeader` propiedad.

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

Puede ver los encabezados de respuesta con las herramientas de red de los exploradores. La siguiente imagen muestra la F12 de borde de salida en el **red** ficha cuando el `About2` se actualiza el método de acción. 

![Salida de F12 de arista en la ** red ** pestaña cuando se llama al método de acción 'About2'](response/_static/vary.png)

### <a name="nostore-and-locationnone"></a>NoStore y Location.None

`NoStore`invalida la mayoría de las demás propiedades. Cuando esta propiedad se establece en `true`, el `Cache-Control` encabezado se establecerá en "no-store". Si `Location` está establecido en `None`:

* El valor de `Cache-Control` está establecido en `"no-store, no-cache"`. 
* El valor de `Pragma` está establecido en `no-cache`. 

Si `NoStore` es `false` y `Location` es `None`, `Cache-Control` y `Pragma` se establecerá en `no-cache`.

Normalmente establece `NoStore` a `true` en las páginas de error. Por ejemplo:

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

Esto dará como resultado de los encabezados siguientes:

```javascript
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a>Ubicación y la duración

Para habilitar el almacenamiento en caché, `Duration` debe establecerse en un valor positivo y `Location` debe ser `Any` (valor predeterminado) o `Client`. En este caso, el `Cache-Control` encabezado se establecerá en el valor de ubicación seguido por "max-age" de la respuesta.

> [!NOTE]
> `Location`de opciones de `Any` y `Client` traducir `Cache-Control` valores de encabezado `public` y `private`, respectivamente. Como se indicó anteriormente, establecer `Location` a `None` establecerá ambos `Cache-Control` y `Pragma` encabezados para `no-cache`.

A continuación se muestra un ejemplo que muestra los encabezados genera estableciendo `Duration` y deja el valor predeterminado `Location` valor.

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

Genera los encabezados siguientes:

```javascript
Cache-Control: public,max-age=60
   ```

### <a name="cache-profiles"></a>Perfiles de memoria caché

En lugar de duplicar `ResponseCache` configuración en muchos atributos de acción de controlador, perfiles de memoria caché se puede configurar como opciones al configurar MVC en la `ConfigureServices` método `Startup`. Valores que se encuentran en un perfil de caché que se hace referencia se usará como los valores predeterminados por el `ResponseCache` de atributo y serán reemplazadas por las propiedades especificadas en el atributo.

Cómo configurar un perfil de caché:

[!code-csharp[Main](response/sample/Startup.cs?name=snippet1)] 

Hacer referencia a un perfil de caché:

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

El `ResponseCache` atributo se puede aplicar tanto para acciones (métodos), así como para controladores (clases). Atributos de nivel de método invalidará la configuración especificada en los atributos de nivel de clase.

En el ejemplo anterior, un atributo de nivel de clase especifica una duración de 30 segundos, mientras que un atributos de nivel de método hace referencia a un perfil de caché con una duración establecida en 60 segundos.

El encabezado resultante:

```
Cache-Control: public,max-age=60
   ```

  ### <a name="additional-resources"></a>Recursos adicionales

* [Almacenamiento en caché de HTTP de la especificación](https://tools.ietf.org/html/rfc7234#section-3)
* [Control de caché](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
