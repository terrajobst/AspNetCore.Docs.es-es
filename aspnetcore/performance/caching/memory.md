---
title: Almacenar en caché en memoria en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo almacenar los datos en la memoria caché en ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 09/15/2018
uid: performance/caching/memory
ms.openlocfilehash: be2e81d1aa6a89d65414d53a70ca2d2fb5d2d3a3
ms.sourcegitcommit: f5d403004f3550e8c46585fdbb16c49e75f495f3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/20/2018
ms.locfileid: "49477194"
---
# <a name="cache-in-memory-in-aspnet-core"></a>Almacenar en caché en memoria en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo), y [Steve Smith](https://ardalis.com/)

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

## <a name="caching-basics"></a>Conceptos básicos de almacenamiento en caché

Almacenamiento en caché puede mejorar significativamente el rendimiento y escalabilidad de una aplicación al reducir el trabajo necesario para generar el contenido. Almacenamiento en caché funciona mejor con datos que cambian con poca frecuencia. Almacenamiento en caché hace una copia de datos que pueden devolverse mucho más rápido que el origen original. Debe escribir y probar la aplicación para que no dependa nunca datos almacenados en caché.

ASP.NET Core admite varias memorias caché diferentes. La memoria caché más sencilla se basa en el [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache), que representa una memoria caché que se almacenan en la memoria del servidor web. Las aplicaciones que se ejecutan en una granja de servidores de varios servidores deben asegurarse de que las sesiones son rápidas cuando se usa la memoria caché en memoria. Sesiones permanentes Asegúrese de que van desde un cliente de todas las solicitudes posteriores al mismo servidor. Por ejemplo, el uso de aplicaciones Web de Azure [enrutamiento de solicitud de aplicación](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) para enrutar todas las solicitudes posteriores al mismo servidor.

Las sesiones que no son permanentes en una granja de servidores web requieren un [caché distribuida](distributed.md) para evitar problemas de coherencia de la memoria caché. Para algunas aplicaciones, una caché distribuida puede admitir mayor escalabilidad horizontal de una caché en memoria. Utilizando una caché distribuida, descarga la memoria caché a un proceso externo.

::: moniker range="< aspnetcore-2.0"

El `IMemoryCache` caché expulsará las entradas de caché bajo presión de memoria, a menos que el [caché prioridad](/dotnet/api/microsoft.extensions.caching.memory.cacheitempriority) está establecido en `CacheItemPriority.NeverRemove`. Puede establecer el `CacheItemPriority` para ajustar la prioridad con la que la memoria caché extrae elementos bajo presión de memoria.

::: moniker-end

La memoria caché en memoria puede almacenar cualquier objeto; la interfaz de la memoria caché distribuida se limita a `byte[]`.

## <a name="systemruntimecachingmemorycache"></a>System.Runtime.Caching/MemoryCache

<xref:System.Runtime.Caching>/<xref:System.Runtime.Caching.MemoryCache> ([Paquete NuGet](https://www.nuget.org/packages/System.Runtime.Caching/)) se pueden usar con:

* .NET standard 2.0 o posterior.
* Cualquier [implementación .NET](/dotnet/standard/net-standard#net-implementation-support) que tiene como destino .NET Standard 2.0 o posterior. Por ejemplo, ASP.NET Core 2.0 o posterior.
* .NET framework 4.5 o posterior.

[Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/) / `IMemoryCache` (descrita en este tema) es preferible a `System.Runtime.Caching` / `MemoryCache` porque se integra mejor en ASP.NET Core. Por ejemplo, `IMemoryCache` funciona de forma nativa con ASP.NET Core [inserción de dependencias](xref:fundamentals/dependency-injection).

Use `System.Runtime.Caching` / `MemoryCache` como un puente de compatibilidad al trasladar código de ASP.NET 4.x a ASP.NET Core.

## <a name="cache-guidelines"></a>Directrices de caché

* Código debería tener siempre una opción de reserva para capturar los datos y **no** dependen de un valor almacenado en caché que están disponibles.
* La memoria caché usa un recurso escaso, la memoria. Limitar el crecimiento de la memoria caché:
  * Hacer **no** utilizar entrada externa como las claves de caché.
  * Utilice la caducidad para limitar el crecimiento de la memoria caché.
  * [Usar SetSize, tamaño y SizeLimit para limitar el tamaño de caché](#use-setsize-size-and-sizelimit-to-limit-cache-size)

## <a name="using-imemorycache"></a>Uso de IMemoryCache

Almacenamiento en caché en memoria es un *servicio* que se hace referencia desde la aplicación mediante [inserción de dependencias](../../fundamentals/dependency-injection.md). Llame a `AddMemoryCache` en `ConfigureServices`:

[!code-csharp[](memory/sample/WebCache/Startup.cs?highlight=9)]

Solicitar el `IMemoryCache` instancia en el constructor:

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ctor)]

::: moniker range="< aspnetcore-2.0"

`IMemoryCache` requiere el paquete NuGet [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/).

::: moniker-end

::: moniker range="= aspnetcore-2.0"

`IMemoryCache` requiere el paquete NuGet [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), que está disponible en el [metapaquete Microsoft.AspNetCore.All](xref:fundamentals/metapackage).

::: moniker-end

::: moniker range="> aspnetcore-2.0"

`IMemoryCache` requiere el paquete NuGet [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), que está disponible en el [Microsoft.AspNetCore.App metapaquete](xref:fundamentals/metapackage-app).

::: moniker-end

El siguiente código utiliza [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) para comprobar si es una hora en la memoria caché. Si no se almacena en caché una vez, se crea y se agrega a la caché con una nueva entrada [establecer](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).

[!code-csharp [](memory/sample/WebCache/CacheKeys.cs)]

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet1)]

Se muestran la hora actual y el tiempo en caché:

[!code-cshtml[](memory/sample/WebCache/Views/Home/Cache.cshtml)]

Almacenado en caché `DateTime` valor permanece en la memoria caché mientras hay solicitudes dentro del tiempo de espera (y ningún expulsión debido a presión de memoria). La siguiente imagen muestra la hora actual y una hora anterior recuperadas de la caché:

![Vista de índice con dos horas diferentes muestra](memory/_static/time.png)

El siguiente código utiliza [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) y [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) en caché los datos. 

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

El código siguiente llama [obtener](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) para capturar el tiempo en caché:

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_gct)]

Consulte [IMemoryCache métodos](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) y [CacheExtensions métodos](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) para obtener una descripción de los métodos de la memoria caché.

## <a name="memorycacheentryoptions"></a>MemoryCacheEntryOptions

El ejemplo siguiente:

- Establece la hora de expiración absoluta. Este es el tiempo máximo que puede almacenarse en caché la entrada e impide que el elemento se vuelva demasiado obsoleta cuando continuamente se renueva el plazo de caducidad.
- Establece un tiempo de expiración variable. Las solicitudes que tienen acceso a esta elemento almacenado en caché restablecerán el reloj de expiración deslizante.
- Establece la prioridad de la memoria caché en `CacheItemPriority.NeverRemove`.
- Establece un [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) al que se llama después de la entrada se expulsa de la memoria caché. La devolución de llamada se ejecuta en un subproceso distinto del código que se quita el elemento de la memoria caché.

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_et&highlight=14-21)]

::: moniker range=">= aspnetcore-2.0"

## <a name="use-setsize-size-and-sizelimit-to-limit-cache-size"></a>Usar SetSize, tamaño y SizeLimit para limitar el tamaño de caché

Un `MemoryCache` instancia opcionalmente puede especificar y aplicar un límite de tamaño. El límite de tamaño de memoria no tiene una unidad de medida definida porque la memoria caché no tiene ningún mecanismo para medir el tamaño de las entradas. Si se establece el límite de tamaño de la memoria caché, todas las entradas deben especificar el tamaño. El tamaño especificado está en unidades que se elige el desarrollador.

Por ejemplo:

* Si la aplicación web se principalmente almacenamiento en caché de cadenas, cada tamaño de la entrada de caché podría ser la longitud de cadena.
* La aplicación puede especificar el tamaño de todas las entradas como 1 y el límite de tamaño es el número de entradas.

El código siguiente crea una sin unidades de tamaño fijo [MemoryCache](/dotnet/api/microsoft.extensions.caching.memory.memorycache?view=aspnetcore-2.1) accesible por [inserción de dependencias](xref:fundamentals/dependency-injection):

[!code-csharp[](memory/sample/RPcache/Services/MyMemoryCache.cs?name=snippet)]

[SizeLimit](/dotnet/api/microsoft.extensions.caching.memory.memorycacheoptions.sizelimit?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheOptions_SizeLimit) no tiene unidades. Las entradas en caché deben especificar el tamaño de las unidades que consideren más adecuados si se ha establecido el tamaño de la memoria caché. Todos los usuarios de una instancia de la memoria caché deben usar el mismo sistema de la unidad. Una entrada no se almacenarán si la suma de los tamaños de entrada de caché supera el valor especificado por `SizeLimit`. Si no se establece ningún límite de tamaño de caché, se omitirá el tamaño de caché en la entrada.

El siguiente código registra `MyMemoryCache` con el [inserción de dependencias](xref:fundamentals/dependency-injection) contenedor.

[!code-csharp[](memory/sample/RPcache/Startup.cs?name=snippet&highlight=5)]

`MyMemoryCache` se crea como una caché de memoria independientes para los componentes que son conscientes de esta memoria caché de tamaño limitado y saber cómo establecer el tamaño de la entrada de caché correctamente.

El siguiente código utiliza `MyMemoryCache`:

[!code-csharp[](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet)]

Se puede establecer el tamaño de la entrada de caché [tamaño](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.size?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_Size) o [SetSize](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.setsize?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryExtensions_SetSize_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_System_Int64_) método de extensión:

[!code-csharp[](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet2&highlight=9,10,14,15)]

::: moniker-end

## <a name="cache-dependencies"></a>Dependencias de caché

El ejemplo siguiente muestra cómo expiran una entrada de caché si expira una entrada dependiente. Un `CancellationChangeToken` se agrega al elemento almacenado en caché. Cuando `Cancel` se llama en el `CancellationTokenSource`, ambas entradas de caché se expulsan.

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ed)]

Mediante un `CancellationTokenSource` permite varias entradas de caché se expulsen como un grupo. Con el `using` patrón en el código anterior, las entradas de caché se crean dentro de la `using` bloque heredarán los desencadenadores y los valores de expiración.

## <a name="additional-notes"></a>Notas adicionales

- Cuando se usa una devolución de llamada para rellenar un elemento de caché:

  - Varias solicitudes pueden encontrar el valor almacenado en caché de clave vacía porque no se ha completado la devolución de llamada.
  - Esto puede dar lugar a que varios subprocesos volver a rellenar el elemento en caché.

- Cuando una entrada de caché se utiliza para crear otro, el elemento secundario copia los tokens de expiración y la configuración de basado en tiempo de expiración de la entrada principal. El elemento secundario no está expirada mediante la eliminación manual o actualización de la entrada principal.

- Use [PostEvictionCallbacks](/dotnet/api/microsoft.extensions.caching.memory.icacheentry.postevictioncallbacks#Microsoft_Extensions_Caching_Memory_ICacheEntry_PostEvictionCallbacks) para establecer las devoluciones de llamada que se desencadena después de la entrada de caché se expulsa de la memoria caché.

## <a name="additional-resources"></a>Recursos adicionales

* [Trabajar con una memoria caché distribuida](xref:performance/caching/distributed)
* [Detectar cambios con tokens de cambio](xref:fundamentals/change-tokens)
* [Almacenamiento en caché de respuestas](xref:performance/caching/response)
* [Middleware de almacenamiento en caché de respuestas](xref:performance/caching/middleware)
* [Asistente de etiquetas de caché](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [Asistente de etiquetas de caché distribuida](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
