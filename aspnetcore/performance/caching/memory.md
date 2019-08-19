---
title: Almacenar en memoria caché en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo almacenar los datos en la memoria caché en ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 8/11/2019
uid: performance/caching/memory
ms.openlocfilehash: 474f71225371ea89b023ee077d4ecc03e9751681
ms.sourcegitcommit: f5f0ff65d4e2a961939762fb00e654491a2c772a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/15/2019
ms.locfileid: "69030320"
---
# <a name="cache-in-memory-in-aspnet-core"></a>Almacenar en memoria caché en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo)y [Steve Smith](https://ardalis.com/)

[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))

## <a name="caching-basics"></a>Conceptos básicos de almacenamiento en caché

El almacenamiento en caché puede mejorar significativamente el rendimiento y la escalabilidad de una aplicación reduciendo el trabajo necesario para generar contenido. El almacenamiento en caché funciona mejor con los datos que cambian con poca frecuencia. Caching realiza una copia de los datos que se pueden devolver mucho más rápido que desde el origen original. Las aplicaciones deben escribirse y probarse para que **nunca** dependan de los datos almacenados en caché.

ASP.NET Core admite varias memorias caché diferentes. La memoria caché más sencilla se basa en el [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache), que representa una memoria caché almacenada en la memoria del servidor Web. Las aplicaciones que se ejecutan en una granja de servidores de varios servidores deben asegurarse de que las sesiones son permanentes al usar la memoria caché en memoria. Las sesiones permanentes garantizan que todas las solicitudes posteriores de un cliente van al mismo servidor. Por ejemplo, Azure Web Apps usa el [enrutamiento de solicitud de aplicaciones](https://www.iis.net/learn/extensions/planning-for-arr) (arr) para enrutar todas las solicitudes posteriores al mismo servidor.

Las sesiones no permanentes en una granja de servidores Web requieren una [caché distribuida](distributed.md) para evitar problemas de coherencia de la memoria caché. En algunas aplicaciones, una caché distribuida puede admitir una mayor escalabilidad horizontal que una caché en memoria. El uso de una caché distribuida descarga la memoria caché en un proceso externo.

::: moniker range="< aspnetcore-2.0"

La `IMemoryCache` memoria caché expulsará las entradas de caché bajo presión de memoria, a `CacheItemPriority.NeverRemove`menos que la prioridad de [caché](/dotnet/api/microsoft.extensions.caching.memory.cacheitempriority) esté establecida en. Puede establecer `CacheItemPriority` para ajustar la prioridad con la que la memoria caché expulsa los elementos bajo presión de memoria.

::: moniker-end

La caché en memoria puede almacenar cualquier objeto; la interfaz de caché distribuida se `byte[]`limita a. Los elementos de caché del almacén de caché distribuida y en memoria como pares de clave y valor.

## <a name="systemruntimecachingmemorycache"></a>System.Runtime.Caching/MemoryCache

<xref:System.Runtime.Caching>/<xref:System.Runtime.Caching.MemoryCache>([Paquete NuGet](https://www.nuget.org/packages/System.Runtime.Caching/)) se puede usar con:

* .NET Standard 2,0 o posterior.
* Cualquier [implementación de .net](/dotnet/standard/net-standard#net-implementation-support) que tenga como destino .net Standard 2,0 o posterior. Por ejemplo, ASP.NET Core 2,0 o posterior.
* .NET Framework 4,5 o posterior.

[Microsoft. Extensions. Caching. Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/) / `IMemoryCache` (que se describe en este artículo `System.Runtime.Caching` ) se recomienda en lugar / `MemoryCache` de que se integre mejor en ASP.net Core. Por ejemplo, `IMemoryCache` funciona de forma nativa con la [inserción](xref:fundamentals/dependency-injection)de dependencias ASP.net Core.

Use `System.Runtime.Caching` comopuentede`MemoryCache` compatibilidad al trasladar código de ASP.net 4. x a ASP.net Core. /

## <a name="cache-guidelines"></a>Instrucciones de caché

* El código siempre debe tener una opción de reserva para capturar datos y **no** depender de que un valor almacenado en caché esté disponible.
* La memoria caché utiliza un recurso y una memoria insuficientes. Limitar el crecimiento de la caché:
  * **No** utilice la entrada externa como claves de caché.
  * Use las expiraciones para limitar el crecimiento de la memoria caché.
  * [Use setSize, size y SizeLimit para limitar el tamaño de la memoria caché](#use-setsize-size-and-sizelimit-to-limit-cache-size). El tiempo de ejecución de ASP.NET Core no limita el tamaño de la memoria caché en función de la presión de memoria. Depende del desarrollador limitar el tamaño de la memoria caché.

## <a name="using-imemorycache"></a>Usar IMemoryCache

> [!WARNING]
> El uso de una caché de memoria *compartida* de la `SetSize`inserción `Size`de [dependencias](xref:fundamentals/dependency-injection) y la llamada a, o `SizeLimit` para limitar el tamaño de la memoria caché puede provocar un error en la aplicación. Cuando se establece un límite de tamaño en una memoria caché, todas las entradas deben especificar un tamaño al agregarse. Esto puede dar lugar a problemas, ya que es posible que los desarrolladores no tengan control total sobre lo que utiliza la memoria caché compartida. Por ejemplo, Entity Framework Core usa la memoria caché compartida y no especifica un tamaño. Si una aplicación establece un límite de tamaño de caché y usa EF Core, la aplicación produce `InvalidOperationException`una excepción.
> Al usar `SetSize`, `Size`o `SizeLimit` para limitar la memoria caché, cree un singleton de caché para el almacenamiento en caché. Para obtener más información y un ejemplo, vea [uso de setSize, size y SizeLimit para limitar el tamaño de la memoria caché](#use-setsize-size-and-sizelimit-to-limit-cache-size).

El almacenamiento en caché en memoria es un *servicio* al que se hace referencia desde la aplicación mediante la [inserción](xref:fundamentals/dependency-injection)de dependencias. Llamar `AddMemoryCache` a `ConfigureServices`en:

[!code-csharp[](memory/sample/WebCache/Startup.cs?highlight=9)]

Solicite la `IMemoryCache` instancia en el constructor:

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ctor)]

::: moniker range="< aspnetcore-2.0"

`IMemoryCache`requiere el paquete NuGet [Microsoft. Extensions. Caching. Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/).

::: moniker-end

::: moniker range="= aspnetcore-2.0"

`IMemoryCache`requiere el paquete NuGet [Microsoft. Extensions. Caching. Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), que está disponible en el metapaquete [Microsoft. AspNetCore. All](xref:fundamentals/metapackage).

::: moniker-end

::: moniker range="> aspnetcore-2.0"

`IMemoryCache`requiere el paquete NuGet [Microsoft. Extensions. Caching. Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), que está disponible en el metapaquete [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).

::: moniker-end

En el código siguiente se usa [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) para comprobar si una hora está en la memoria caché. Si una hora no está almacenada en caché, se crea una nueva entrada y se agrega a la memoria caché con [set](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).

[!code-csharp [](memory/sample/WebCache/CacheKeys.cs)]

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet1)]

Se muestran la hora actual y la hora almacenada en caché:

[!code-cshtml[](memory/sample/WebCache/Views/Home/Cache.cshtml)]

El `DateTime` valor almacenado en caché permanece en la memoria caché mientras haya solicitudes dentro del período de tiempo de espera. En la imagen siguiente se muestra la hora actual y una hora anterior recuperada de la caché:

![Vista de índice con dos horas diferentes mostradas](memory/_static/time.png)

En el código siguiente se usa [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.getorcreate#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) y [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.getorcreateasync#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) para almacenar datos en caché.

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

El siguiente código llama a [Get](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) para capturar la hora almacenada en caché:

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_gct)]

<xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreate*>, <xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreateAsync*>y [Get](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) son métodos de extensión que forman parte de la clase [CacheExtensions](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) que extiende la <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>capacidad de. Vea métodos [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) y [métodos CacheExtensions](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) para obtener una descripción de otros métodos de caché.

## <a name="memorycacheentryoptions"></a>MemoryCacheEntryOptions

El ejemplo siguiente:

* Establece la hora de expiración absoluta. Este es el tiempo máximo que se puede almacenar en caché la entrada y evita que el elemento se vuelva demasiado obsoleto cuando la expiración variable se renueve continuamente.
* Establece una hora de expiración variable. Las solicitudes que tengan acceso a este elemento almacenado en caché restablecerán el reloj de expiración variable.
* Establece la prioridad de la `CacheItemPriority.NeverRemove`memoria caché en.
* Establece un [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) que se llamará después de que la entrada se expulse de la memoria caché. La devolución de llamada se ejecuta en un subproceso diferente del código que quita el elemento de la memoria caché.

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_et&highlight=14-21)]

::: moniker range=">= aspnetcore-2.0"

## <a name="use-setsize-size-and-sizelimit-to-limit-cache-size"></a>Use setSize, size y SizeLimit para limitar el tamaño de la memoria caché

Opcionalmente, una `MemoryCache` instancia puede especificar y exigir un límite de tamaño. El límite de tamaño de memoria no tiene una unidad de medida definida porque la memoria caché no tiene ningún mecanismo para medir el tamaño de las entradas. Si se establece el límite de tamaño de la memoria caché, todas las entradas deben especificar el tamaño. El tiempo de ejecución de ASP.NET Core no limita el tamaño de la memoria caché en función de la presión de memoria. Depende del desarrollador limitar el tamaño de la memoria caché. El tamaño especificado se encuentra en las unidades que el desarrollador elige.

Por ejemplo:

* Si la aplicación web estaba principalmente almacenando en caché cadenas, cada tamaño de entrada de caché podría ser la longitud de la cadena.
* La aplicación puede especificar el tamaño de todas las entradas como 1 y el límite de tamaño es el recuento de entradas.

En el código siguiente se crea una [MemoryCache](/dotnet/api/microsoft.extensions.caching.memory.memorycache?view=aspnetcore-2.1) de tamaño fijo de una unidad no disponible que es accesible por la [inserciónde dependencias](xref:fundamentals/dependency-injection):

[!code-csharp[](memory/sample/RPcache/Services/MyMemoryCache.cs?name=snippet)]

[SizeLimit](/dotnet/api/microsoft.extensions.caching.memory.memorycacheoptions.sizelimit?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheOptions_SizeLimit) no tiene unidades. Las entradas almacenadas en caché deben especificar el tamaño en las unidades que consideren más adecuadas si se ha establecido el tamaño de la memoria caché. Todos los usuarios de una instancia de caché deben usar el mismo sistema de unidad. Una entrada no se almacenará en caché si la suma de los tamaños de las entradas almacenadas en caché `SizeLimit`supera el valor especificado por. Si no se establece ningún límite de tamaño de caché, se omitirá el tamaño de la memoria caché establecido en la entrada.

El código siguiente se registra `MyMemoryCache` con el contenedor de [inserción](xref:fundamentals/dependency-injection) de dependencias.

[!code-csharp[](memory/sample/RPcache/Startup.cs?name=snippet&highlight=5)]

`MyMemoryCache`se crea como una caché de memoria independiente para los componentes que tienen en cuenta esta caché de tamaño limitado y saben cómo establecer el tamaño de la entrada de caché de forma adecuada.

El código siguiente usa `MyMemoryCache`:

[!code-csharp[](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet)]

El tamaño de la entrada de caché se puede establecer por [tamaño](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.size?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_Size) o por el método de extensión [setSize](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.setsize?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryExtensions_SetSize_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_System_Int64_) :

[!code-csharp[](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet2&highlight=9,10,14,15)]

::: moniker-end

## <a name="cache-dependencies"></a>Dependencias de caché

En el ejemplo siguiente se muestra cómo expirar una entrada de caché si una entrada dependiente expira. `CancellationChangeToken` Se agrega al elemento almacenado en caché. Cuando `Cancel` se llama `CancellationTokenSource`a en, se expulsan ambas entradas de caché.

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ed)]

El uso `CancellationTokenSource` de permite desalojar varias entradas de caché como un grupo. Con el `using` patrón en el código anterior, las entradas de caché que `using` se crean dentro del bloque heredarán los desencadenadores y la configuración de expiración.

## <a name="additional-notes"></a>Notas adicionales

* Cuando se usa una devolución de llamada para volver a rellenar un elemento de caché:

  * Varias solicitudes pueden encontrar el valor de clave en caché vacío porque la devolución de llamada no se ha completado.
  * Esto puede dar lugar a que varios subprocesos rellenen el elemento almacenado en caché.

* Cuando se utiliza una entrada de caché para crear otra, el elemento secundario copia los tokens de expiración de la entrada primaria y la configuración de expiración basada en el tiempo. El elemento secundario no ha expirado porque se ha quitado o actualizado manualmente la entrada principal.

* Use [PostEvictionCallbacks](/dotnet/api/microsoft.extensions.caching.memory.icacheentry.postevictioncallbacks#Microsoft_Extensions_Caching_Memory_ICacheEntry_PostEvictionCallbacks) para establecer las devoluciones de llamada que se activarán después de que la entrada de caché se expulse de la memoria caché.

## <a name="additional-resources"></a>Recursos adicionales

* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
