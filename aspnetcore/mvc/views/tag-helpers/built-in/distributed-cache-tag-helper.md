---
title: "Aplicación auxiliar de etiquetas de caché distribuida en ASP.NET Core"
author: pkellner
description: "Se muestra cómo trabajar con la aplicación auxiliar de etiquetas de caché."
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: 710477732b865e2e3821102d34545bbd4e0a5919
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="distributed-cache-tag-helper"></a>Aplicación auxiliar de etiquetas de caché distribuida

Por [Peter Kellner](http://peterkellner.net) 


La aplicación auxiliar de etiquetas de caché distribuida proporciona la capacidad de mejorar drásticamente el rendimiento de la aplicación ASP.NET Core al permitir almacenar en caché su contenido en un origen de caché distribuida.

La aplicación auxiliar de etiquetas de caché distribuida hereda de la misma clase base que la aplicación auxiliar de etiquetas de caché.  Todos los atributos asociados a la aplicación auxiliar de etiquetas de caché también funcionarán en la aplicación auxiliar de etiquetas de caché distribuida.


La aplicación auxiliar de etiquetas de caché distribuida sigue el **principio de dependencias explícitas** conocido como **inserción de constructores**.  En concreto, el contenedor de interfaz `IDistributedCache` se pasa al constructor de la aplicación auxiliar de etiquetas de caché distribuida.  Si no se ha creado ninguna implementación específica de `IDistributedCache` en `ConfigureServices`, que normalmente se encuentra en startup.cs, la aplicación auxiliar de etiquetas de caché distribuida usará el mismo proveedor en memoria para almacenar datos en caché que la aplicación auxiliar de etiquetas de caché básica.

## <a name="distributed-cache-tag-helper-attributes"></a>Atributos de la aplicación auxiliar de etiquetas de caché distribuida

- - -

### <a name="enabled-expires-on-expires-after-expires-sliding-vary-by-header-vary-by-query-vary-by-route-vary-by-cookie-vary-by-user-vary-by-priority"></a>enabled expires-on expires-after expires-sliding vary-by-header vary-by-query vary-by-route vary-by-cookie vary-by-user vary-by priority

Vea la aplicación auxiliar de etiquetas de caché para obtener las definiciones. La aplicación auxiliar de etiquetas de caché distribuida hereda de la misma clase que la aplicación auxiliar de etiquetas de caché, de modo que todos estos atributos son iguales que los de la aplicación auxiliar de etiquetas de caché.

- - -

### <a name="name-required"></a>Atributo name (obligatorio)

| Tipo de atributo    | Valor de ejemplo     |
|----------------   |----------------   |
| cadena    | "my-distributed-cache-unique-key-101"     |

El atributo `name` obligatorio se usa como clave de la caché almacenada para cada instancia de una aplicación auxiliar de etiquetas de caché distribuida.  A diferencia de la aplicación auxiliar de etiquetas de caché básica, que asigna una clave a cada instancia de la aplicación auxiliar de etiquetas de caché en función del nombre de la página de Razor y la ubicación de la aplicación auxiliar de etiquetas en la página de Razor, la aplicación auxiliar de etiquetas de caché distribuida solo basa su clave en el atributo `name`.

Ejemplo de uso:

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a>Implementaciones de IDistributedCache de la aplicación auxiliar de etiquetas de caché distribuida

Hay dos implementaciones de `IDistributedCache` integradas en ASP.NET Core.  Una se basa en **Sql Server** y la otra en **Redis**. Encontrará más información sobre estas implementaciones en el recurso denominado "Trabajar con una memoria caché distribuida", al que se hace referencia a continuación. Ambas implementaciones requieren establecer una instancia de `IDistributedCache` en el archivo **startup.cs** de ASP.NET Core.

No hay atributos de etiqueta asociados específicamente con el uso de implementaciones concretas de `IDistributedCache`.



- - -



## <a name="additional-resources"></a>Recursos adicionales

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
