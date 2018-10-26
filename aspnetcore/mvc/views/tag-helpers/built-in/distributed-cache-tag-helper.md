---
title: Asistente de etiquetas de caché distribuida en ASP.NET Core
author: pkellner
description: Obtenga información sobre cómo usar el asistente de etiquetas de caché distribuida.
ms.author: riande
ms.custom: mvc
ms.date: 10/10/2018
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: a5b33451a763c297c6d7885855a321c43435abb4
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325216"
---
# <a name="distributed-cache-tag-helper-in-aspnet-core"></a>Asistente de etiquetas de caché distribuida en ASP.NET Core

Por [Peter Kellner](http://peterkellner.net) y [Luke Latham](https://github.com/guardrex)

El asistente de etiquetas de caché distribuida proporciona la capacidad de mejorar drásticamente el rendimiento de la aplicación ASP.NET Core al permitir almacenar en caché su contenido en un origen de caché distribuida.

Para obtener información general sobre asistentes de etiquetas, vea <xref:mvc/views/tag-helpers/intro>.

El asistente de etiquetas de caché distribuida hereda de la misma clase base que el asistente de etiquetas de caché. Todos los atributos del [asistente de etiquetas de caché](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper) están disponibles para el asistente de etiquetas distribuidas.

El asistente de etiquetas de caché distribuida usa la [inserción de constructor](xref:fundamentals/dependency-injection#constructor-injection-behavior). La interfaz <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> se pasa al constructor del asistente de etiquetas de caché distribuida. Si no se ha creado ninguna implementación específica de `IDistributedCache` en `Startup.ConfigureServices` (*Startup.cs*), el asistente de etiquetas de caché distribuida usa el mismo proveedor en memoria para almacenar datos en caché como el [asistente de etiquetas de caché](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).

## <a name="distributed-cache-tag-helper-attributes"></a>Atributos del asistente de etiquetas de caché distribuida

### <a name="attributes-shared-with-the-cache-tag-helper"></a>Atributos compartidos con el asistente de etiquetas de caché

* `enabled`
* `expires-on`
* `expires-after`
* `expires-sliding`
* `vary-by-header`
* `vary-by-query`
* `vary-by-route`
* `vary-by-cookie`
* `vary-by-user`
* `vary-by priority`

El asistente de etiquetas de caché distribuida hereda de la misma clase que el asistente de etiquetas de caché. Para obtener descripciones de estos atributos, vea el [asistente de etiquetas de caché](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).

### <a name="name"></a>name

| Tipo de atributo | Ejemplo                               |
| -------------- | ------------------------------------- |
| String         | `my-distributed-cache-unique-key-101` |

`name` es obligatorio. El atributo `name` se usa como clave para cada instancia de caché almacenada. A diferencia del asistente de etiquetas de caché, que asigna una clave de caché a cada instancia en función del nombre de la página de Razor y la ubicación en la página de Razor, el asistente de etiquetas de caché distribuida solo basa su clave en el atributo `name`.

Ejemplo:

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a>Implementaciones de IDistributedCache del asistente de etiquetas de caché distribuida

Hay dos implementaciones de <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> integradas en ASP.NET Core. Una se basa en SQL Server y la otra en Redis. En <xref:performance/caching/distributed> encontrará detalles de estas implementaciones. Para ambas implementaciones hay que establecer una instancia de `IDistributedCache` en `Startup`.

No hay atributos de etiqueta asociados específicamente con el uso de implementaciones concretas de `IDistributedCache`.

## <a name="additional-resources"></a>Recursos adicionales

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
