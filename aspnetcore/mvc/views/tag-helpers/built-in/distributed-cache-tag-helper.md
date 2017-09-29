---
title: "Distribuye la aplicación auxiliar de etiqueta de caché | Documentos de Microsoft"
author: pkellner
description: "Muestra cómo trabajar con la aplicación auxiliar de etiqueta de caché"
keywords: "ASP.NET Core, aplicación auxiliar de etiquetas"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: c045d485-d1dc-4cea-a675-46be83b7a022
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: 462c3775677924fc7b9b715cd6de75fe53ada89e
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2017
---
# <a name="distributed-cache-tag-helper"></a>Aplicación auxiliar de etiqueta de caché distribuida

Por [Peter Kellner](http://peterkellner.net) 


El Ayudante de etiqueta de caché distribuida proporciona la capacidad para mejorar drásticamente el rendimiento de la aplicación de ASP.NET Core almacenando en memoria caché su contenido a un origen de caché distribuida.

El Ayudante de etiqueta de caché distribuida se hereda de la misma clase base que la aplicación auxiliar de etiqueta de caché.  Todos los atributos asociados a la aplicación auxiliar de etiqueta de caché también funcionará en la aplicación auxiliar distribuidas de etiqueta.


El Ayudante de etiqueta de caché distribuida sigue la **principio de dependencias explícitas** conocido como **inyección de Constructor**.  En concreto, la `IDistributedCache` contenedor de interfaz se haya pasado al constructor de la Distributed caché etiqueta del Ayudante.  Si ninguna implementación concreta específica de `IDistributedCache` se ha creado en `ConfigureServices`, normalmente se encuentran en startup.cs, a continuación, el Ayudante de etiqueta de caché distribuida utilizará el mismo proveedor en memoria para almacenar datos en caché como el Ayudante de etiqueta de caché básico.

## <a name="distributed-cache-tag-helper-attributes"></a>Distribuye los atributos de aplicación auxiliar de etiqueta de caché

- - -

### <a name="enabled-expires-on-expires-after-expires-sliding-vary-by-header-vary-by-query-vary-by-route-vary-by-cookie-vary-by-user-vary-by-priority"></a>habilitado expira la sesión expira después expira-deslizante por encabezado vary variar por consulta varían por ruta varían cookie varían por usuario varían por prioridad

Vea auxiliar de etiqueta de caché para las definiciones. Auxiliar de etiqueta de caché distribuida se hereda de la misma clase que la aplicación auxiliar de etiqueta de caché para que todos estos atributos son comunes de aplicación auxiliar de etiqueta de caché.

- - -

### <a name="name-required"></a>nombre (obligatorio)

| Tipo de atributo    | Valor de ejemplo     |
|----------------   |----------------   |
| string    | "my-distributed-cache-unique-key-101"     |

Requerido `name` atributo se utiliza como clave para esa caché almacenada para cada instancia de un Ayudante de etiqueta de caché distribuida.  Al contrario que el Ayudante básica de la etiqueta de caché que asigna una clave a cada instancia de aplicación auxiliar de etiqueta de caché según el nombre de la página de Razor y la ubicación de la aplicación auxiliar de etiquetas en la página de razor, el Ayudante de etiqueta de caché distribuida solo basa su clave en el atributo`name`

Ejemplo de uso:

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a>Distribuye las implementaciones de caché etiqueta auxiliar IDistributedCache

Hay dos implementaciones de `IDistributedCache` integrada en ASP.NET Core.  Uno se basa en **Sql Server** y la otra se basa en **Redis**. Detalles de estas implementaciones se pueden encontrar en el recurso al que hace referencia a continuación con nombre "trabajar con una memoria caché distribuida". Ambas implementaciones implican establecer una instancia de `IDistributedCache` en ASP.NET Core **startup.cs**.

No hay ningún atributo de etiqueta están asociado específicamente con el uso de cualquier implementación específica de `IDistributedCache`.



- - -



## <a name="additional-resources"></a>Recursos adicionales

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
