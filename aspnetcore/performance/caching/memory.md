---
title: Almacenar en caché en memoria en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo almacenar los datos en la memoria caché en ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 02/11/2019
uid: performance/caching/memory
ms.openlocfilehash: c115e43b9dd4f838ab9600c2e105d86732d857ad
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58208281"
---
# <a name="cache-in-memory-in-aspnet-core"></a><span data-ttu-id="12e2f-103">Almacenar en caché en memoria en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="12e2f-103">Cache in-memory in ASP.NET Core</span></span>

<span data-ttu-id="12e2f-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo), y [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="12e2f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="12e2f-105">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="12e2f-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="caching-basics"></a><span data-ttu-id="12e2f-106">Conceptos básicos de almacenamiento en caché</span><span class="sxs-lookup"><span data-stu-id="12e2f-106">Caching basics</span></span>

<span data-ttu-id="12e2f-107">Almacenamiento en caché puede mejorar significativamente el rendimiento y escalabilidad de una aplicación al reducir el trabajo necesario para generar el contenido.</span><span class="sxs-lookup"><span data-stu-id="12e2f-107">Caching can significantly improve the performance and scalability of an app by reducing the work required to generate content.</span></span> <span data-ttu-id="12e2f-108">Almacenamiento en caché funciona mejor con datos que cambian con poca frecuencia.</span><span class="sxs-lookup"><span data-stu-id="12e2f-108">Caching works best with data that changes infrequently.</span></span> <span data-ttu-id="12e2f-109">Almacenamiento en caché hace una copia de datos que pueden devolverse mucho más rápido que el origen original.</span><span class="sxs-lookup"><span data-stu-id="12e2f-109">Caching makes a copy of data that can be returned much faster than from the original source.</span></span> <span data-ttu-id="12e2f-110">Debe escribir y probar la aplicación para que no dependa nunca datos almacenados en caché.</span><span class="sxs-lookup"><span data-stu-id="12e2f-110">You should write and test your app to never depend on cached data.</span></span>

<span data-ttu-id="12e2f-111">ASP.NET Core admite varias memorias caché diferentes.</span><span class="sxs-lookup"><span data-stu-id="12e2f-111">ASP.NET Core supports several different caches.</span></span> <span data-ttu-id="12e2f-112">La memoria caché más sencilla se basa en el [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache), que representa una memoria caché que se almacenan en la memoria del servidor web.</span><span class="sxs-lookup"><span data-stu-id="12e2f-112">The simplest cache is based on the [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache), which represents a cache stored in the memory of the web server.</span></span> <span data-ttu-id="12e2f-113">Las aplicaciones que se ejecutan en una granja de servidores de varios servidores deben asegurarse de que las sesiones son rápidas cuando se usa la memoria caché en memoria.</span><span class="sxs-lookup"><span data-stu-id="12e2f-113">Apps which run on a server farm of multiple servers should ensure that sessions are sticky when using the in-memory cache.</span></span> <span data-ttu-id="12e2f-114">Sesiones permanentes Asegúrese de que van desde un cliente de todas las solicitudes posteriores al mismo servidor.</span><span class="sxs-lookup"><span data-stu-id="12e2f-114">Sticky sessions ensure that subsequent requests from a client all go to the same server.</span></span> <span data-ttu-id="12e2f-115">Por ejemplo, el uso de aplicaciones Web de Azure [enrutamiento de solicitud de aplicación](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) para enrutar todas las solicitudes posteriores al mismo servidor.</span><span class="sxs-lookup"><span data-stu-id="12e2f-115">For example, Azure Web apps use [Application Request Routing](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) to route all subsequent requests to the same server.</span></span>

<span data-ttu-id="12e2f-116">Las sesiones que no son permanentes en una granja de servidores web requieren un [caché distribuida](distributed.md) para evitar problemas de coherencia de la memoria caché.</span><span class="sxs-lookup"><span data-stu-id="12e2f-116">Non-sticky sessions in a web farm require a [distributed cache](distributed.md) to avoid cache consistency problems.</span></span> <span data-ttu-id="12e2f-117">Para algunas aplicaciones, una caché distribuida puede admitir mayor escalabilidad horizontal de una caché en memoria.</span><span class="sxs-lookup"><span data-stu-id="12e2f-117">For some apps, a distributed cache can support higher scale-out than an in-memory cache.</span></span> <span data-ttu-id="12e2f-118">Utilizando una caché distribuida, descarga la memoria caché a un proceso externo.</span><span class="sxs-lookup"><span data-stu-id="12e2f-118">Using a distributed cache offloads the cache memory to an external process.</span></span>

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="12e2f-119">El `IMemoryCache` caché expulsará las entradas de caché bajo presión de memoria, a menos que el [caché prioridad](/dotnet/api/microsoft.extensions.caching.memory.cacheitempriority) está establecido en `CacheItemPriority.NeverRemove`.</span><span class="sxs-lookup"><span data-stu-id="12e2f-119">The `IMemoryCache` cache will evict cache entries under memory pressure unless the [cache priority](/dotnet/api/microsoft.extensions.caching.memory.cacheitempriority) is set to `CacheItemPriority.NeverRemove`.</span></span> <span data-ttu-id="12e2f-120">Puede establecer el `CacheItemPriority` para ajustar la prioridad con la que la memoria caché extrae elementos bajo presión de memoria.</span><span class="sxs-lookup"><span data-stu-id="12e2f-120">You can set the `CacheItemPriority` to adjust the priority with which the cache evicts items under memory pressure.</span></span>

::: moniker-end

<span data-ttu-id="12e2f-121">La memoria caché en memoria puede almacenar cualquier objeto; la interfaz de la memoria caché distribuida se limita a `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="12e2f-121">The in-memory cache can store any object; the distributed cache interface is limited to `byte[]`.</span></span> <span data-ttu-id="12e2f-122">Los elementos de caché de almacén de caché en memoria y distribuidas como pares clave-valor.</span><span class="sxs-lookup"><span data-stu-id="12e2f-122">The in-memory and distributed cache store cache items as key-value pairs.</span></span>

## <a name="systemruntimecachingmemorycache"></a><span data-ttu-id="12e2f-123">System.Runtime.Caching/MemoryCache</span><span class="sxs-lookup"><span data-stu-id="12e2f-123">System.Runtime.Caching/MemoryCache</span></span>

<span data-ttu-id="12e2f-124"><xref:System.Runtime.Caching>/<xref:System.Runtime.Caching.MemoryCache> ([Paquete NuGet](https://www.nuget.org/packages/System.Runtime.Caching/)) se pueden usar con:</span><span class="sxs-lookup"><span data-stu-id="12e2f-124"><xref:System.Runtime.Caching>/<xref:System.Runtime.Caching.MemoryCache> ([NuGet package](https://www.nuget.org/packages/System.Runtime.Caching/)) can be used with:</span></span>

* <span data-ttu-id="12e2f-125">.NET standard 2.0 o posterior.</span><span class="sxs-lookup"><span data-stu-id="12e2f-125">.NET Standard 2.0 or later.</span></span>
* <span data-ttu-id="12e2f-126">Cualquier [implementación .NET](/dotnet/standard/net-standard#net-implementation-support) que tiene como destino .NET Standard 2.0 o posterior.</span><span class="sxs-lookup"><span data-stu-id="12e2f-126">Any [.NET implementation](/dotnet/standard/net-standard#net-implementation-support) that targets .NET Standard 2.0 or later.</span></span> <span data-ttu-id="12e2f-127">Por ejemplo, ASP.NET Core 2.0 o posterior.</span><span class="sxs-lookup"><span data-stu-id="12e2f-127">For example, ASP.NET Core 2.0 or later.</span></span>
* <span data-ttu-id="12e2f-128">.NET framework 4.5 o posterior.</span><span class="sxs-lookup"><span data-stu-id="12e2f-128">.NET Framework 4.5 or later.</span></span>

<span data-ttu-id="12e2f-129">[Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/) / `IMemoryCache` (descrita en este tema) es preferible a `System.Runtime.Caching` / `MemoryCache` porque se integra mejor en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="12e2f-129">[Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/)/`IMemoryCache` (described in this topic) is recommended over `System.Runtime.Caching`/`MemoryCache` because it's better integrated into ASP.NET Core.</span></span> <span data-ttu-id="12e2f-130">Por ejemplo, `IMemoryCache` funciona de forma nativa con ASP.NET Core [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="12e2f-130">For example, `IMemoryCache` works natively with ASP.NET Core [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="12e2f-131">Use `System.Runtime.Caching` / `MemoryCache` como un puente de compatibilidad al trasladar código de ASP.NET 4.x a ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="12e2f-131">Use `System.Runtime.Caching`/`MemoryCache` as a compatibility bridge when porting code from ASP.NET 4.x to ASP.NET Core.</span></span>

## <a name="cache-guidelines"></a><span data-ttu-id="12e2f-132">Directrices de caché</span><span class="sxs-lookup"><span data-stu-id="12e2f-132">Cache guidelines</span></span>

* <span data-ttu-id="12e2f-133">Código debería tener siempre una opción de reserva para capturar los datos y **no** dependen de un valor almacenado en caché que están disponibles.</span><span class="sxs-lookup"><span data-stu-id="12e2f-133">Code should always have a fallback option to fetch data and **not** depend on a cached value being available.</span></span>
* <span data-ttu-id="12e2f-134">La memoria caché usa un recurso escaso, la memoria.</span><span class="sxs-lookup"><span data-stu-id="12e2f-134">The cache uses a scarce resource, memory.</span></span> <span data-ttu-id="12e2f-135">Limitar el crecimiento de la memoria caché:</span><span class="sxs-lookup"><span data-stu-id="12e2f-135">Limit cache growth:</span></span>
  * <span data-ttu-id="12e2f-136">Hacer **no** utilizar entrada externa como las claves de caché.</span><span class="sxs-lookup"><span data-stu-id="12e2f-136">Do **not** use external input as cache keys.</span></span>
  * <span data-ttu-id="12e2f-137">Utilice la caducidad para limitar el crecimiento de la memoria caché.</span><span class="sxs-lookup"><span data-stu-id="12e2f-137">Use expirations to limit cache growth.</span></span>
  * [<span data-ttu-id="12e2f-138">Usar SetSize, tamaño y SizeLimit para limitar el tamaño de caché</span><span class="sxs-lookup"><span data-stu-id="12e2f-138">Use SetSize, Size, and SizeLimit to limit cache size</span></span>](#use-setsize-size-and-sizelimit-to-limit-cache-size)

## <a name="using-imemorycache"></a><span data-ttu-id="12e2f-139">Uso de IMemoryCache</span><span class="sxs-lookup"><span data-stu-id="12e2f-139">Using IMemoryCache</span></span>

<span data-ttu-id="12e2f-140">Almacenamiento en caché en memoria es un *servicio* que se hace referencia desde la aplicación mediante [inserción de dependencias](../../fundamentals/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="12e2f-140">In-memory caching is a *service* that's referenced from your app using [Dependency Injection](../../fundamentals/dependency-injection.md).</span></span> <span data-ttu-id="12e2f-141">Llame a `AddMemoryCache` en `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="12e2f-141">Call `AddMemoryCache` in `ConfigureServices`:</span></span>

[!code-csharp[](memory/sample/WebCache/Startup.cs?highlight=9)]

<span data-ttu-id="12e2f-142">Solicitar el `IMemoryCache` instancia en el constructor:</span><span class="sxs-lookup"><span data-stu-id="12e2f-142">Request the `IMemoryCache` instance in the constructor:</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ctor)]

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="12e2f-143">`IMemoryCache` requiere el paquete NuGet [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/).</span><span class="sxs-lookup"><span data-stu-id="12e2f-143">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="12e2f-144">`IMemoryCache` requiere el paquete NuGet [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), que está disponible en el [metapaquete Microsoft.AspNetCore.All](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="12e2f-144">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), which is available in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.0"

<span data-ttu-id="12e2f-145">`IMemoryCache` requiere el paquete NuGet [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), que está disponible en el [Microsoft.AspNetCore.App metapaquete](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="12e2f-145">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), which is available in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

<span data-ttu-id="12e2f-146">El siguiente código utiliza [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) para comprobar si es una hora en la memoria caché.</span><span class="sxs-lookup"><span data-stu-id="12e2f-146">The following code uses [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) to check if a time is in the cache.</span></span> <span data-ttu-id="12e2f-147">Si no se almacena en caché una vez, se crea y se agrega a la caché con una nueva entrada [establecer](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).</span><span class="sxs-lookup"><span data-stu-id="12e2f-147">If a time isn't cached, a new entry is created and added to the cache with [Set](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).</span></span>

[!code-csharp [](memory/sample/WebCache/CacheKeys.cs)]

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet1)]

<span data-ttu-id="12e2f-148">Se muestran la hora actual y el tiempo en caché:</span><span class="sxs-lookup"><span data-stu-id="12e2f-148">The current time and the cached time are displayed:</span></span>

[!code-cshtml[](memory/sample/WebCache/Views/Home/Cache.cshtml)]

<span data-ttu-id="12e2f-149">Almacenado en caché `DateTime` valor permanece en la memoria caché mientras hay solicitudes dentro del tiempo de espera (y ningún expulsión debido a presión de memoria).</span><span class="sxs-lookup"><span data-stu-id="12e2f-149">The cached `DateTime` value remains in the cache while there are requests within the timeout period (and no eviction due to memory pressure).</span></span> <span data-ttu-id="12e2f-150">La siguiente imagen muestra la hora actual y una hora anterior recuperadas de la caché:</span><span class="sxs-lookup"><span data-stu-id="12e2f-150">The following image shows the current time and an older time retrieved from the cache:</span></span>

![Vista de índice con dos horas diferentes muestra](memory/_static/time.png)

<span data-ttu-id="12e2f-152">El siguiente código utiliza [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) y [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) en caché los datos.</span><span class="sxs-lookup"><span data-stu-id="12e2f-152">The following code uses [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) and [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) to cache data.</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

<span data-ttu-id="12e2f-153">El código siguiente llama [obtener](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) para capturar el tiempo en caché:</span><span class="sxs-lookup"><span data-stu-id="12e2f-153">The following code calls [Get](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) to fetch the cached time:</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_gct)]

<span data-ttu-id="12e2f-154"><xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreate*> , <xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreateAsync*>, y [obtener](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) forman parte de los métodos de extensión de la [CacheExtensions](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) clase que amplía la funcionalidad de <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>.</span><span class="sxs-lookup"><span data-stu-id="12e2f-154"><xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreate*> , <xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreateAsync*>, and [Get](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) are extension methods part of the [CacheExtensions](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) class that extends the capability of <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>.</span></span> <span data-ttu-id="12e2f-155">Consulte [IMemoryCache métodos](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) y [CacheExtensions métodos](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) para obtener una descripción de otros métodos de la memoria caché.</span><span class="sxs-lookup"><span data-stu-id="12e2f-155">See [IMemoryCache methods](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) and [CacheExtensions methods](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) for a description of other cache methods.</span></span>

## <a name="memorycacheentryoptions"></a><span data-ttu-id="12e2f-156">MemoryCacheEntryOptions</span><span class="sxs-lookup"><span data-stu-id="12e2f-156">MemoryCacheEntryOptions</span></span>

<span data-ttu-id="12e2f-157">El ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="12e2f-157">The following sample:</span></span>

* <span data-ttu-id="12e2f-158">Establece la hora de expiración absoluta.</span><span class="sxs-lookup"><span data-stu-id="12e2f-158">Sets the absolute expiration time.</span></span> <span data-ttu-id="12e2f-159">Este es el tiempo máximo que puede almacenarse en caché la entrada e impide que el elemento se vuelva demasiado obsoleta cuando continuamente se renueva el plazo de caducidad.</span><span class="sxs-lookup"><span data-stu-id="12e2f-159">This is the maximum time the entry can be cached and prevents the item from becoming too stale when the sliding expiration is continuously renewed.</span></span>
* <span data-ttu-id="12e2f-160">Establece un tiempo de expiración variable.</span><span class="sxs-lookup"><span data-stu-id="12e2f-160">Sets a sliding expiration time.</span></span> <span data-ttu-id="12e2f-161">Las solicitudes que tienen acceso a esta elemento almacenado en caché restablecerán el reloj de expiración deslizante.</span><span class="sxs-lookup"><span data-stu-id="12e2f-161">Requests that access this cached item will reset the sliding expiration clock.</span></span>
* <span data-ttu-id="12e2f-162">Establece la prioridad de la memoria caché en `CacheItemPriority.NeverRemove`.</span><span class="sxs-lookup"><span data-stu-id="12e2f-162">Sets the cache priority to `CacheItemPriority.NeverRemove`.</span></span>
* <span data-ttu-id="12e2f-163">Establece un [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) al que se llama después de la entrada se expulsa de la memoria caché.</span><span class="sxs-lookup"><span data-stu-id="12e2f-163">Sets a [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) that will be called after the entry is evicted from the cache.</span></span> <span data-ttu-id="12e2f-164">La devolución de llamada se ejecuta en un subproceso distinto del código que se quita el elemento de la memoria caché.</span><span class="sxs-lookup"><span data-stu-id="12e2f-164">The callback is run on a different thread from the code that removes the item from the cache.</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_et&highlight=14-21)]

::: moniker range=">= aspnetcore-2.0"

## <a name="use-setsize-size-and-sizelimit-to-limit-cache-size"></a><span data-ttu-id="12e2f-165">Usar SetSize, tamaño y SizeLimit para limitar el tamaño de caché</span><span class="sxs-lookup"><span data-stu-id="12e2f-165">Use SetSize, Size, and SizeLimit to limit cache size</span></span>

<span data-ttu-id="12e2f-166">Un `MemoryCache` instancia opcionalmente puede especificar y aplicar un límite de tamaño.</span><span class="sxs-lookup"><span data-stu-id="12e2f-166">A `MemoryCache` instance may optionally specify and enforce a size limit.</span></span> <span data-ttu-id="12e2f-167">El límite de tamaño de memoria no tiene una unidad de medida definida porque la memoria caché no tiene ningún mecanismo para medir el tamaño de las entradas.</span><span class="sxs-lookup"><span data-stu-id="12e2f-167">The memory size limit does not have a defined unit of measure because the cache has no mechanism to measure the size of entries.</span></span> <span data-ttu-id="12e2f-168">Si se establece el límite de tamaño de la memoria caché, todas las entradas deben especificar el tamaño.</span><span class="sxs-lookup"><span data-stu-id="12e2f-168">If the cache memory size limit is set, all entries must specify size.</span></span> <span data-ttu-id="12e2f-169">El tamaño especificado está en unidades que se elige el desarrollador.</span><span class="sxs-lookup"><span data-stu-id="12e2f-169">The size specified is in units the developer chooses.</span></span>

<span data-ttu-id="12e2f-170">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="12e2f-170">For example:</span></span>

* <span data-ttu-id="12e2f-171">Si la aplicación web se principalmente almacenamiento en caché de cadenas, cada tamaño de la entrada de caché podría ser la longitud de cadena.</span><span class="sxs-lookup"><span data-stu-id="12e2f-171">If the web app was primarily caching strings, each cache entry size could be the string length.</span></span>
* <span data-ttu-id="12e2f-172">La aplicación puede especificar el tamaño de todas las entradas como 1 y el límite de tamaño es el número de entradas.</span><span class="sxs-lookup"><span data-stu-id="12e2f-172">The app could specify the size of all entries as 1, and the size limit is the count of entries.</span></span>

<span data-ttu-id="12e2f-173">El código siguiente crea una sin unidades de tamaño fijo [MemoryCache](/dotnet/api/microsoft.extensions.caching.memory.memorycache?view=aspnetcore-2.1) accesible por [inserción de dependencias](xref:fundamentals/dependency-injection):</span><span class="sxs-lookup"><span data-stu-id="12e2f-173">The following code creates a unitless fixed size [MemoryCache](/dotnet/api/microsoft.extensions.caching.memory.memorycache?view=aspnetcore-2.1) accessible by [dependency injection](xref:fundamentals/dependency-injection):</span></span>

[!code-csharp[](memory/sample/RPcache/Services/MyMemoryCache.cs?name=snippet)]

<span data-ttu-id="12e2f-174">[SizeLimit](/dotnet/api/microsoft.extensions.caching.memory.memorycacheoptions.sizelimit?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheOptions_SizeLimit) no tiene unidades.</span><span class="sxs-lookup"><span data-stu-id="12e2f-174">[SizeLimit](/dotnet/api/microsoft.extensions.caching.memory.memorycacheoptions.sizelimit?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheOptions_SizeLimit) does not have units.</span></span> <span data-ttu-id="12e2f-175">Las entradas en caché deben especificar el tamaño de las unidades que consideren más adecuados si se ha establecido el tamaño de la memoria caché.</span><span class="sxs-lookup"><span data-stu-id="12e2f-175">Cached entries must specify size in whatever units they deem most appropriate if the cache memory size has been set.</span></span> <span data-ttu-id="12e2f-176">Todos los usuarios de una instancia de la memoria caché deben usar el mismo sistema de la unidad.</span><span class="sxs-lookup"><span data-stu-id="12e2f-176">All users of a cache instance should use the same unit system.</span></span> <span data-ttu-id="12e2f-177">Una entrada no se almacenarán si la suma de los tamaños de entrada de caché supera el valor especificado por `SizeLimit`.</span><span class="sxs-lookup"><span data-stu-id="12e2f-177">An entry will not be cached if the sum of the cached entry sizes exceeds the value specified by `SizeLimit`.</span></span> <span data-ttu-id="12e2f-178">Si no se establece ningún límite de tamaño de caché, se omitirá el tamaño de caché en la entrada.</span><span class="sxs-lookup"><span data-stu-id="12e2f-178">If no cache size limit is set, the cache size set on the entry will be ignored.</span></span>

<span data-ttu-id="12e2f-179">El siguiente código registra `MyMemoryCache` con el [inserción de dependencias](xref:fundamentals/dependency-injection) contenedor.</span><span class="sxs-lookup"><span data-stu-id="12e2f-179">The following code registers `MyMemoryCache` with the [dependency injection](xref:fundamentals/dependency-injection) container.</span></span>

[!code-csharp[](memory/sample/RPcache/Startup.cs?name=snippet&highlight=5)]

<span data-ttu-id="12e2f-180">`MyMemoryCache` se crea como una caché de memoria independientes para los componentes que son conscientes de esta memoria caché de tamaño limitado y saber cómo establecer el tamaño de la entrada de caché correctamente.</span><span class="sxs-lookup"><span data-stu-id="12e2f-180">`MyMemoryCache` is created as an independent memory cache for components that are aware of this size limited cache and know how to set cache entry size appropriately.</span></span>

<span data-ttu-id="12e2f-181">El siguiente código utiliza `MyMemoryCache`:</span><span class="sxs-lookup"><span data-stu-id="12e2f-181">The following code uses `MyMemoryCache`:</span></span>

[!code-csharp[](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet)]

<span data-ttu-id="12e2f-182">Se puede establecer el tamaño de la entrada de caché [tamaño](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.size?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_Size) o [SetSize](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.setsize?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryExtensions_SetSize_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_System_Int64_) método de extensión:</span><span class="sxs-lookup"><span data-stu-id="12e2f-182">The size of the cache entry can be set by [Size](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.size?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_Size) or the [SetSize](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.setsize?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryExtensions_SetSize_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_System_Int64_) extension method:</span></span>

[!code-csharp[](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet2&highlight=9,10,14,15)]

::: moniker-end

## <a name="cache-dependencies"></a><span data-ttu-id="12e2f-183">Dependencias de caché</span><span class="sxs-lookup"><span data-stu-id="12e2f-183">Cache dependencies</span></span>

<span data-ttu-id="12e2f-184">El ejemplo siguiente muestra cómo expiran una entrada de caché si expira una entrada dependiente.</span><span class="sxs-lookup"><span data-stu-id="12e2f-184">The following sample shows how to expire a cache entry if a dependent entry expires.</span></span> <span data-ttu-id="12e2f-185">Un `CancellationChangeToken` se agrega al elemento almacenado en caché.</span><span class="sxs-lookup"><span data-stu-id="12e2f-185">A `CancellationChangeToken` is added to the cached item.</span></span> <span data-ttu-id="12e2f-186">Cuando `Cancel` se llama en el `CancellationTokenSource`, ambas entradas de caché se expulsan.</span><span class="sxs-lookup"><span data-stu-id="12e2f-186">When `Cancel` is called on the `CancellationTokenSource`, both cache entries are evicted.</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ed)]

<span data-ttu-id="12e2f-187">Mediante un `CancellationTokenSource` permite varias entradas de caché se expulsen como un grupo.</span><span class="sxs-lookup"><span data-stu-id="12e2f-187">Using a `CancellationTokenSource` allows multiple cache entries to be evicted as a group.</span></span> <span data-ttu-id="12e2f-188">Con el `using` patrón en el código anterior, las entradas de caché se crean dentro de la `using` bloque heredarán los desencadenadores y los valores de expiración.</span><span class="sxs-lookup"><span data-stu-id="12e2f-188">With the `using` pattern in the code above, cache entries created inside the `using` block will inherit triggers and expiration settings.</span></span>

## <a name="additional-notes"></a><span data-ttu-id="12e2f-189">Notas adicionales</span><span class="sxs-lookup"><span data-stu-id="12e2f-189">Additional notes</span></span>

* <span data-ttu-id="12e2f-190">Cuando se usa una devolución de llamada para rellenar un elemento de caché:</span><span class="sxs-lookup"><span data-stu-id="12e2f-190">When using a callback to repopulate a cache item:</span></span>

  * <span data-ttu-id="12e2f-191">Varias solicitudes pueden encontrar el valor almacenado en caché de clave vacía porque no se ha completado la devolución de llamada.</span><span class="sxs-lookup"><span data-stu-id="12e2f-191">Multiple requests can find the cached key value empty because the callback hasn't completed.</span></span>
  * <span data-ttu-id="12e2f-192">Esto puede dar lugar a que varios subprocesos volver a rellenar el elemento en caché.</span><span class="sxs-lookup"><span data-stu-id="12e2f-192">This can result in several threads repopulating the cached item.</span></span>

* <span data-ttu-id="12e2f-193">Cuando una entrada de caché se utiliza para crear otro, el elemento secundario copia los tokens de expiración y la configuración de basado en tiempo de expiración de la entrada principal.</span><span class="sxs-lookup"><span data-stu-id="12e2f-193">When one cache entry is used to create another, the child copies the parent entry's expiration tokens and time-based expiration settings.</span></span> <span data-ttu-id="12e2f-194">El elemento secundario no está expirada mediante la eliminación manual o actualización de la entrada principal.</span><span class="sxs-lookup"><span data-stu-id="12e2f-194">The child isn't expired by manual removal or updating of the parent entry.</span></span>

* <span data-ttu-id="12e2f-195">Use [PostEvictionCallbacks](/dotnet/api/microsoft.extensions.caching.memory.icacheentry.postevictioncallbacks#Microsoft_Extensions_Caching_Memory_ICacheEntry_PostEvictionCallbacks) para establecer las devoluciones de llamada que se desencadena después de la entrada de caché se expulsa de la memoria caché.</span><span class="sxs-lookup"><span data-stu-id="12e2f-195">Use [PostEvictionCallbacks](/dotnet/api/microsoft.extensions.caching.memory.icacheentry.postevictioncallbacks#Microsoft_Extensions_Caching_Memory_ICacheEntry_PostEvictionCallbacks) to set the callbacks that will be fired after the cache entry is evicted from the cache.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="12e2f-196">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="12e2f-196">Additional resources</span></span>

* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
