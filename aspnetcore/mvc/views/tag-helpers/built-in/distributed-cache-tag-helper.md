---
title: Asistente de etiquetas de caché distribuida en ASP.NET Core
author: pkellner
description: Obtenga información sobre cómo usar el asistente de etiquetas de caché distribuida.
ms.author: riande
ms.date: 02/14/2017
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: 1b51164a6d3dab2eeaf64262d6f0d9961bd00d12
ms.sourcegitcommit: 4d5f8680d68b39c411b46c73f7014f8aa0f12026
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "47028101"
---
# <a name="distributed-cache-tag-helper-in-aspnet-core"></a>Asistente de etiquetas de caché distribuida en ASP.NET Core

Por [Peter Kellner](http://peterkellner.net) 

El asistente de etiquetas de caché distribuida proporciona la capacidad de mejorar drásticamente el rendimiento de la aplicación ASP.NET Core al permitir almacenar en caché su contenido en un origen de caché distribuida.

El asistente de etiquetas de caché distribuida hereda de la misma clase base que el asistente de etiquetas de caché. Todos los atributos asociados al asistente de etiquetas de caché también funcionarán en el asistente de etiquetas de caché distribuida.

El asistente de etiquetas de caché distribuida sigue el **principio de dependencias explícitas** conocido como **inserción de constructores**. En concreto, el contenedor de interfaz `IDistributedCache` se pasa al constructor del asistente de etiquetas de caché distribuida. Si no se ha creado ninguna implementación específica de `IDistributedCache` en `ConfigureServices`, que normalmente se encuentra en startup.cs, el asistente de etiquetas de caché distribuida usará el mismo proveedor en memoria para almacenar datos en caché que el asistente de etiquetas de caché básica.

## <a name="distributed-cache-tag-helper-attributes"></a>Atributos del asistente de etiquetas de caché distribuida

- - -

### <a name="enabled-expires-on-expires-after-expires-sliding-vary-by-header-vary-by-query-vary-by-route-vary-by-cookie-vary-by-user-vary-by-priority"></a>enabled expires-on expires-after expires-sliding vary-by-header vary-by-query vary-by-route vary-by-cookie vary-by-user vary-by priority

Vea el asistente de etiquetas de caché para obtener las definiciones. El asistente de etiquetas de caché distribuida hereda de la misma clase que el asistente de etiquetas de caché, de modo que todos estos atributos son iguales que los del asistente de etiquetas de caché.

- - -

### <a name="name-required"></a>Atributo name (obligatorio)

| Tipo de atributo    | Valor de ejemplo     |
|----------------   |----------------   |
| cadena    | "my-distributed-cache-unique-key-101"     |

El atributo `name` obligatorio se usa como clave de la caché almacenada para cada instancia de un asistente de etiquetas de caché distribuida. A diferencia del asistente de etiquetas de caché básica, que asigna una clave a cada instancia del asistente de etiquetas de caché en función del nombre de la página de Razor y la ubicación del asistente de etiquetas en la página de Razor, el asistente de etiquetas de caché distribuida solo basa su clave en el atributo `name`.

Ejemplo de uso:

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a>Implementaciones de IDistributedCache del asistente de etiquetas de caché distribuida

Hay dos implementaciones de `IDistributedCache` integradas en ASP.NET Core. Una se basa en SQL Server y la otra, en Redis. En <xref:performance/caching/distributed> encontrará detalles de estas implementaciones. Ambas implementaciones requieren establecer una instancia de `IDistributedCache` en el archivo *Startup.cs* de ASP.NET Core.

No hay atributos de etiqueta asociados específicamente con el uso de implementaciones concretas de `IDistributedCache`.

## <a name="additional-resources"></a>Recursos adicionales

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
