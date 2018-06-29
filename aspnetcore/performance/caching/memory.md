---
title: Almacenar en memoria caché en memoria en el núcleo de ASP.NET
author: rick-anderson
description: Obtenga información acerca de cómo almacenar en caché los datos en memoria en ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 12/14/2016
uid: performance/caching/memory
ms.openlocfilehash: 5a7085269e255ae233a0e7eeb860a04b2c6bede3
ms.sourcegitcommit: 931b6a2d7eb28a0f1295e8a95690b8c4c5f58477
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2018
ms.locfileid: "37077591"
---
# <a name="cache-in-memory-in-aspnet-core"></a><span data-ttu-id="4870e-103">Almacenar en memoria caché en memoria en el núcleo de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="4870e-103">Cache in-memory in ASP.NET Core</span></span>

<span data-ttu-id="4870e-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo), y [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="4870e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="4870e-105">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4870e-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="caching-basics"></a><span data-ttu-id="4870e-106">Conceptos básicos sobre el almacenamiento en caché</span><span class="sxs-lookup"><span data-stu-id="4870e-106">Caching basics</span></span>

<span data-ttu-id="4870e-107">Almacenamiento en caché puede mejorar significativamente el rendimiento y la escalabilidad de una aplicación al reducir el trabajo necesario para generar el contenido.</span><span class="sxs-lookup"><span data-stu-id="4870e-107">Caching can significantly improve the performance and scalability of an app by reducing the work required to generate content.</span></span> <span data-ttu-id="4870e-108">Almacenamiento en caché funciona mejor con datos que cambian con poca frecuencia.</span><span class="sxs-lookup"><span data-stu-id="4870e-108">Caching works best with data that changes infrequently.</span></span> <span data-ttu-id="4870e-109">Almacenamiento en caché realiza una copia de datos que se pueden devolver mucho más rápida que la de la fuente original.</span><span class="sxs-lookup"><span data-stu-id="4870e-109">Caching makes a copy of data that can be returned much faster than from the original source.</span></span> <span data-ttu-id="4870e-110">Debe escribir y probar la aplicación para que nunca dependen de datos almacenados en caché.</span><span class="sxs-lookup"><span data-stu-id="4870e-110">You should write and test your app to never depend on cached data.</span></span>

<span data-ttu-id="4870e-111">ASP.NET Core es compatible con varias memorias caché diferentes.</span><span class="sxs-lookup"><span data-stu-id="4870e-111">ASP.NET Core supports several different caches.</span></span> <span data-ttu-id="4870e-112">La memoria caché más sencilla se basa en el [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache), que representa una caché almacenada en la memoria del servidor web.</span><span class="sxs-lookup"><span data-stu-id="4870e-112">The simplest cache is based on the [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache), which represents a cache stored in the memory of the web server.</span></span> <span data-ttu-id="4870e-113">Las aplicaciones que se ejecutan en una granja de servidores de varios servidores deben asegurarse de que las sesiones son rápidas cuando se usa la memoria caché en memoria.</span><span class="sxs-lookup"><span data-stu-id="4870e-113">Apps which run on a server farm of multiple servers should ensure that sessions are sticky when using the in-memory cache.</span></span> <span data-ttu-id="4870e-114">Sesiones permanentes Asegúrese de que las solicitudes posteriores de un cliente todos los vayan al mismo servidor.</span><span class="sxs-lookup"><span data-stu-id="4870e-114">Sticky sessions ensure that subsequent requests from a client all go to the same server.</span></span> <span data-ttu-id="4870e-115">Por ejemplo, el uso de aplicaciones Web de Azure [enrutamiento de solicitud de aplicación](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) para enrutar las solicitudes subsiguientes en el mismo servidor.</span><span class="sxs-lookup"><span data-stu-id="4870e-115">For example, Azure Web apps use [Application Request Routing](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) to route all subsequent requests to the same server.</span></span>

<span data-ttu-id="4870e-116">Las sesiones no son permanentes en una granja de servidores web requieren un [caché distribuida](distributed.md) para evitar problemas de coherencia de la memoria caché.</span><span class="sxs-lookup"><span data-stu-id="4870e-116">Non-sticky sessions in a web farm require a [distributed cache](distributed.md) to avoid cache consistency problems.</span></span> <span data-ttu-id="4870e-117">Para algunas aplicaciones, una memoria caché distribuida puede admitir una escala mayor espera que una caché en memoria.</span><span class="sxs-lookup"><span data-stu-id="4870e-117">For some apps, a distributed cache can support higher scale out than an in-memory cache.</span></span> <span data-ttu-id="4870e-118">Uso de una memoria caché distribuida, descarga la memoria caché para un proceso externo.</span><span class="sxs-lookup"><span data-stu-id="4870e-118">Using a distributed cache offloads the cache memory to an external process.</span></span>

<span data-ttu-id="4870e-119">El `IMemoryCache` caché expulsará las entradas de caché bajo presión de memoria, a menos que la [almacenar en caché prioridad](/dotnet/api/microsoft.extensions.caching.memory.cacheitempriority) está establecido en `CacheItemPriority.NeverRemove`.</span><span class="sxs-lookup"><span data-stu-id="4870e-119">The `IMemoryCache` cache will evict cache entries under memory pressure unless the [cache priority](/dotnet/api/microsoft.extensions.caching.memory.cacheitempriority) is set to `CacheItemPriority.NeverRemove`.</span></span> <span data-ttu-id="4870e-120">Puede establecer el `CacheItemPriority` para ajustar la prioridad con la que la memoria caché extrae elementos bajo presión de memoria.</span><span class="sxs-lookup"><span data-stu-id="4870e-120">You can set the `CacheItemPriority` to adjust the priority with which the cache evicts items under memory pressure.</span></span>

<span data-ttu-id="4870e-121">La memoria caché en memoria puede almacenar cualquier objeto; la interfaz de caché distribuida se limita a `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="4870e-121">The in-memory cache can store any object; the distributed cache interface is limited to `byte[]`.</span></span>

## <a name="using-imemorycache"></a><span data-ttu-id="4870e-122">Usar IMemoryCache</span><span class="sxs-lookup"><span data-stu-id="4870e-122">Using IMemoryCache</span></span>

<span data-ttu-id="4870e-123">Almacenamiento en caché en memoria es un *servicio* que se hace referencia desde la aplicación con [inyección de dependencia](../../fundamentals/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="4870e-123">In-memory caching is a *service* that's referenced from your app using [Dependency Injection](../../fundamentals/dependency-injection.md).</span></span> <span data-ttu-id="4870e-124">Llame a `AddMemoryCache` en `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="4870e-124">Call `AddMemoryCache` in `ConfigureServices`:</span></span>

[!code-csharp[](memory/sample/WebCache/Startup.cs?highlight=9)]

<span data-ttu-id="4870e-125">Solicitar la `IMemoryCache` instancia en el constructor:</span><span class="sxs-lookup"><span data-stu-id="4870e-125">Request the `IMemoryCache` instance in the constructor:</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ctor)]

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="4870e-126">`IMemoryCache` requiere el paquete NuGet [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/).</span><span class="sxs-lookup"><span data-stu-id="4870e-126">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="4870e-127">`IMemoryCache` requiere el paquete NuGet [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), que está disponible en la [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="4870e-127">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), which is avaiable in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.0"

<span data-ttu-id="4870e-128">`IMemoryCache` requiere el paquete NuGet [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), que está disponible en la [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="4870e-128">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), which is avaiable in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

<span data-ttu-id="4870e-129">El siguiente código utiliza [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) para comprobar si es una hora en la memoria caché.</span><span class="sxs-lookup"><span data-stu-id="4870e-129">The following code uses [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) to check if a time is in the cache.</span></span> <span data-ttu-id="4870e-130">Si no está almacenado en caché una vez, se crea y se agrega a la caché con una nueva entrada [establecer](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).</span><span class="sxs-lookup"><span data-stu-id="4870e-130">If a time isn't cached, a new entry is created and added to the cache with [Set](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet1)]

<span data-ttu-id="4870e-131">Se muestran la hora actual y el tiempo en caché:</span><span class="sxs-lookup"><span data-stu-id="4870e-131">The current time and the cached time are displayed:</span></span>

[!code-cshtml[](memory/sample/WebCache/Views/Home/Cache.cshtml)]

<span data-ttu-id="4870e-132">Las almacenadas en caché `DateTime` valor permanece en la memoria caché mientras haya solicitudes dentro del tiempo de espera (y ningún expulsión debido a la presión de memoria).</span><span class="sxs-lookup"><span data-stu-id="4870e-132">The cached `DateTime` value remains in the cache while there are requests within the timeout period (and no eviction due to memory pressure).</span></span> <span data-ttu-id="4870e-133">La siguiente imagen muestra la hora actual y una hora anterior recuperadas desde la caché:</span><span class="sxs-lookup"><span data-stu-id="4870e-133">The following image shows the current time and an older time retrieved from the cache:</span></span>

![Vista de índice con dos momentos diferentes que se muestran](memory/_static/time.png)

<span data-ttu-id="4870e-135">El siguiente código utiliza [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) y [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) en caché los datos.</span><span class="sxs-lookup"><span data-stu-id="4870e-135">The following code uses [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) and [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) to cache data.</span></span> 

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

<span data-ttu-id="4870e-136">El código siguiente llama [obtener](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) para capturar el tiempo en caché:</span><span class="sxs-lookup"><span data-stu-id="4870e-136">The following code calls [Get](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) to fetch the cached time:</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_gct)]

<span data-ttu-id="4870e-137">Vea [IMemoryCache métodos](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) y [CacheExtensions métodos](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) para obtener una descripción de los métodos de la memoria caché.</span><span class="sxs-lookup"><span data-stu-id="4870e-137">See [IMemoryCache methods](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) and [CacheExtensions methods](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) for a description of the cache methods.</span></span>

## <a name="using-memorycacheentryoptions"></a><span data-ttu-id="4870e-138">Usar MemoryCacheEntryOptions</span><span class="sxs-lookup"><span data-stu-id="4870e-138">Using MemoryCacheEntryOptions</span></span>

<span data-ttu-id="4870e-139">El ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="4870e-139">The following sample:</span></span>

- <span data-ttu-id="4870e-140">Establece la hora de expiración absoluta.</span><span class="sxs-lookup"><span data-stu-id="4870e-140">Sets the absolute expiration time.</span></span> <span data-ttu-id="4870e-141">Esto es el tiempo máximo que puede almacenarse en caché la entrada y evita que el elemento se conviertan en obsoletos cuando continuamente se renueva el plazo de caducidad.</span><span class="sxs-lookup"><span data-stu-id="4870e-141">This is the maximum time the entry can be cached and prevents the item from becoming too stale when the sliding expiration is continuously renewed.</span></span>
- <span data-ttu-id="4870e-142">Establece un tiempo de expiración variable.</span><span class="sxs-lookup"><span data-stu-id="4870e-142">Sets a sliding expiration time.</span></span> <span data-ttu-id="4870e-143">Las solicitudes que tienen acceso a este elemento almacenado en caché, restablecerán el reloj de expiración deslizante.</span><span class="sxs-lookup"><span data-stu-id="4870e-143">Requests that access this cached item will reset the sliding expiration clock.</span></span>
- <span data-ttu-id="4870e-144">Establece la prioridad de la memoria caché en `CacheItemPriority.NeverRemove`.</span><span class="sxs-lookup"><span data-stu-id="4870e-144">Sets the cache priority to `CacheItemPriority.NeverRemove`.</span></span>
- <span data-ttu-id="4870e-145">Establece un [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) al que se llama después de la entrada se expulsa de la memoria caché.</span><span class="sxs-lookup"><span data-stu-id="4870e-145">Sets a [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) that will be called after the entry is evicted from the cache.</span></span> <span data-ttu-id="4870e-146">La devolución de llamada se ejecuta en un subproceso diferente del código que se quita el elemento de la memoria caché.</span><span class="sxs-lookup"><span data-stu-id="4870e-146">The callback is run on a different thread from the code that removes the item from the cache.</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_et&highlight=14-21)]

## <a name="cache-dependencies"></a><span data-ttu-id="4870e-147">Dependencias de caché</span><span class="sxs-lookup"><span data-stu-id="4870e-147">Cache dependencies</span></span>

<span data-ttu-id="4870e-148">El ejemplo siguiente muestra cómo expirar una entrada de caché si expira una entrada dependiente.</span><span class="sxs-lookup"><span data-stu-id="4870e-148">The following sample shows how to expire a cache entry if a dependent entry expires.</span></span> <span data-ttu-id="4870e-149">Un `CancellationChangeToken` se agrega al elemento almacenado en caché.</span><span class="sxs-lookup"><span data-stu-id="4870e-149">A `CancellationChangeToken` is added to the cached item.</span></span> <span data-ttu-id="4870e-150">Cuando `Cancel` se llama en el `CancellationTokenSource`, ambas entradas de caché se desalojan.</span><span class="sxs-lookup"><span data-stu-id="4870e-150">When `Cancel` is called on the `CancellationTokenSource`, both cache entries are evicted.</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ed)]

<span data-ttu-id="4870e-151">Mediante un `CancellationTokenSource` permite varias entradas de caché que se expulsen como un grupo.</span><span class="sxs-lookup"><span data-stu-id="4870e-151">Using a `CancellationTokenSource` allows multiple cache entries to be evicted as a group.</span></span> <span data-ttu-id="4870e-152">Con el `using` patrón en el código anterior, las entradas de caché se crean dentro de la `using` bloque heredarán opciones de expiración y desencadenadores.</span><span class="sxs-lookup"><span data-stu-id="4870e-152">With the `using` pattern in the code above, cache entries created inside the `using` block will inherit triggers and expiration settings.</span></span>

## <a name="additional-notes"></a><span data-ttu-id="4870e-153">Notas adicionales</span><span class="sxs-lookup"><span data-stu-id="4870e-153">Additional notes</span></span>

- <span data-ttu-id="4870e-154">Cuando se usa una devolución de llamada para rellenar un elemento de caché:</span><span class="sxs-lookup"><span data-stu-id="4870e-154">When using a callback to repopulate a cache item:</span></span>

  - <span data-ttu-id="4870e-155">Varias solicitudes pueden encontrar el valor de clave almacenada en caché vacía porque no se ha completado la devolución de llamada.</span><span class="sxs-lookup"><span data-stu-id="4870e-155">Multiple requests can find the cached key value empty because the callback hasn't completed.</span></span>
  - <span data-ttu-id="4870e-156">Esto puede dar lugar a que varios subprocesos al rellenar el elemento en caché.</span><span class="sxs-lookup"><span data-stu-id="4870e-156">This can result in several threads repopulating the cached item.</span></span>

- <span data-ttu-id="4870e-157">Cuando una entrada de caché se utiliza para crear otro, el elemento secundario copia tokens de expiración y la configuración de expiración basada en el momento de la entrada primaria.</span><span class="sxs-lookup"><span data-stu-id="4870e-157">When one cache entry is used to create another, the child copies the parent entry's expiration tokens and time-based expiration settings.</span></span> <span data-ttu-id="4870e-158">El elemento secundario no está expirada para eliminación manual o actualización de la entrada primaria.</span><span class="sxs-lookup"><span data-stu-id="4870e-158">The child isn't expired by manual removal or updating of the parent entry.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4870e-159">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="4870e-159">Additional resources</span></span>

* [<span data-ttu-id="4870e-160">Trabajar con una memoria caché distribuida</span><span class="sxs-lookup"><span data-stu-id="4870e-160">Work with a distributed cache</span></span>](xref:performance/caching/distributed)
* [<span data-ttu-id="4870e-161">Detectar cambios con tokens de cambio</span><span class="sxs-lookup"><span data-stu-id="4870e-161">Detect changes with change tokens</span></span>](xref:fundamentals/primitives/change-tokens)
* [<span data-ttu-id="4870e-162">Almacenamiento en caché de respuestas</span><span class="sxs-lookup"><span data-stu-id="4870e-162">Response caching</span></span>](xref:performance/caching/response)
* [<span data-ttu-id="4870e-163">Middleware de almacenamiento en caché de respuestas</span><span class="sxs-lookup"><span data-stu-id="4870e-163">Response Caching Middleware</span></span>](xref:performance/caching/middleware)
* [<span data-ttu-id="4870e-164">Aplicación auxiliar de etiquetas de caché</span><span class="sxs-lookup"><span data-stu-id="4870e-164">Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [<span data-ttu-id="4870e-165">Aplicación auxiliar de etiquetas de caché distribuida</span><span class="sxs-lookup"><span data-stu-id="4870e-165">Distributed Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
