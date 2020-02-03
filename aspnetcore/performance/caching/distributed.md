---
title: Almacenamiento en caché distribuido en ASP.NET Core
author: guardrex
description: Aprenda a usar una ASP.NET Core caché distribuida para mejorar el rendimiento y la escalabilidad de las aplicaciones, especialmente en un entorno de granja de servidores o en la nube.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/22/2020
uid: performance/caching/distributed
ms.openlocfilehash: 3e039a26505aed1bcc0299880039760fef19fd67
ms.sourcegitcommit: eca76bd065eb94386165a0269f1e95092f23fa58
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2020
ms.locfileid: "76727233"
---
# <a name="distributed-caching-in-aspnet-core"></a><span data-ttu-id="79e94-103">Almacenamiento en caché distribuido en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="79e94-103">Distributed caching in ASP.NET Core</span></span>

<span data-ttu-id="79e94-104">Por [Luke Latham](https://github.com/guardrex), [Mohsin Nasir](https://github.com/mohsinnasir)y [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="79e94-104">By [Luke Latham](https://github.com/guardrex), [Mohsin Nasir](https://github.com/mohsinnasir), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="79e94-105">Una caché distribuida es una memoria caché compartida por varios servidores de aplicaciones, que normalmente se mantiene como un servicio externo a los servidores de aplicaciones que tienen acceso a ella.</span><span class="sxs-lookup"><span data-stu-id="79e94-105">A distributed cache is a cache shared by multiple app servers, typically maintained as an external service to the app servers that access it.</span></span> <span data-ttu-id="79e94-106">Una caché distribuida puede mejorar el rendimiento y la escalabilidad de una aplicación ASP.NET Core, especialmente cuando la aplicación está hospedada en un servicio en la nube o en una granja de servidores.</span><span class="sxs-lookup"><span data-stu-id="79e94-106">A distributed cache can improve the performance and scalability of an ASP.NET Core app, especially when the app is hosted by a cloud service or a server farm.</span></span>

<span data-ttu-id="79e94-107">Una caché distribuida tiene varias ventajas con respecto a otros escenarios de almacenamiento en caché en los que los datos almacenados en caché se almacenan en servidores de aplicaciones individuales.</span><span class="sxs-lookup"><span data-stu-id="79e94-107">A distributed cache has several advantages over other caching scenarios where cached data is stored on individual app servers.</span></span>

<span data-ttu-id="79e94-108">Cuando se distribuyen los datos almacenados en caché, los datos:</span><span class="sxs-lookup"><span data-stu-id="79e94-108">When cached data is distributed, the data:</span></span>

* <span data-ttu-id="79e94-109">Es *coherente (coherente)* entre las solicitudes a varios servidores.</span><span class="sxs-lookup"><span data-stu-id="79e94-109">Is *coherent* (consistent) across requests to multiple servers.</span></span>
* <span data-ttu-id="79e94-110">Sobrevive los reinicios del servidor y las implementaciones de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="79e94-110">Survives server restarts and app deployments.</span></span>
* <span data-ttu-id="79e94-111">No usa la memoria local.</span><span class="sxs-lookup"><span data-stu-id="79e94-111">Doesn't use local memory.</span></span>

<span data-ttu-id="79e94-112">La configuración de caché distribuida es específica de la implementación.</span><span class="sxs-lookup"><span data-stu-id="79e94-112">Distributed cache configuration is implementation specific.</span></span> <span data-ttu-id="79e94-113">En este artículo se describe cómo configurar SQL Server y las cachés distribuidas de Redis.</span><span class="sxs-lookup"><span data-stu-id="79e94-113">This article describes how to configure SQL Server and Redis distributed caches.</span></span> <span data-ttu-id="79e94-114">También hay implementaciones de terceros disponibles, como [NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache en github](https://github.com/Alachisoft/NCache)).</span><span class="sxs-lookup"><span data-stu-id="79e94-114">Third party implementations are also available, such as [NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache on GitHub](https://github.com/Alachisoft/NCache)).</span></span> <span data-ttu-id="79e94-115">Independientemente de la implementación que se seleccione, la aplicación interactúa con la memoria caché mediante la interfaz <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>.</span><span class="sxs-lookup"><span data-stu-id="79e94-115">Regardless of which implementation is selected, the app interacts with the cache using the <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface.</span></span>

<span data-ttu-id="79e94-116">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/distributed/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="79e94-116">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/distributed/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="79e94-117">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="79e94-117">Prerequisites</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="79e94-118">Para usar una SQL Server caché distribuida, agregue una referencia de paquete al paquete [Microsoft. Extensions. Caching. SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) .</span><span class="sxs-lookup"><span data-stu-id="79e94-118">To use a SQL Server distributed cache, add a package reference to the [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span></span>

<span data-ttu-id="79e94-119">Para usar una caché distribuida de Redis, agregue una referencia de paquete al paquete [Microsoft. Extensions. Caching. StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) .</span><span class="sxs-lookup"><span data-stu-id="79e94-119">To use a Redis distributed cache, add a package reference to the [Microsoft.Extensions.Caching.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) package.</span></span>

<span data-ttu-id="79e94-120">Para usar caché distribuida de NCache, agregue una referencia de paquete al paquete [NCache. Microsoft. Extensions. Caching. código abierto](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource) .</span><span class="sxs-lookup"><span data-stu-id="79e94-120">To use NCache distributed cache, add a package reference to the [NCache.Microsoft.Extensions.Caching.OpenSource](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource) package.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="79e94-121">Para usar una SQL Server caché distribuida, haga referencia al [metapaquete Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) o agregue una referencia de paquete al paquete [Microsoft. Extensions. Caching. SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) .</span><span class="sxs-lookup"><span data-stu-id="79e94-121">To use a SQL Server distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span></span>

<span data-ttu-id="79e94-122">Para usar una caché distribuida de Redis, haga referencia al [metapaquete Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) y agregue una referencia de paquete al paquete [Microsoft. Extensions. Caching. StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) .</span><span class="sxs-lookup"><span data-stu-id="79e94-122">To use a Redis distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and add a package reference to the [Microsoft.Extensions.Caching.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) package.</span></span> <span data-ttu-id="79e94-123">El paquete de Redis no está incluido en el paquete de `Microsoft.AspNetCore.App`, por lo que debe hacer referencia al paquete de Redis por separado en el archivo de proyecto.</span><span class="sxs-lookup"><span data-stu-id="79e94-123">The Redis package isn't included in the `Microsoft.AspNetCore.App` package, so you must reference the Redis package separately in your project file.</span></span>

<span data-ttu-id="79e94-124">Para usar caché distribuida de NCache, haga referencia al [metapaquete Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) y agregue una referencia de paquete al paquete [NCache. Microsoft. Extensions. Caching. código abierto](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource) .</span><span class="sxs-lookup"><span data-stu-id="79e94-124">To use NCache distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and add a package reference to the [NCache.Microsoft.Extensions.Caching.OpenSource](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource) package.</span></span> <span data-ttu-id="79e94-125">El paquete de NCache no está incluido en el paquete de `Microsoft.AspNetCore.App`, por lo que debe hacer referencia al paquete de NCache por separado en el archivo de proyecto.</span><span class="sxs-lookup"><span data-stu-id="79e94-125">The NCache package isn't included in the `Microsoft.AspNetCore.App` package, so you must reference the NCache package separately in your project file.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="79e94-126">Para usar una SQL Server caché distribuida, haga referencia al [metapaquete Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) o agregue una referencia de paquete al paquete [Microsoft. Extensions. Caching. SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) .</span><span class="sxs-lookup"><span data-stu-id="79e94-126">To use a SQL Server distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span></span>

<span data-ttu-id="79e94-127">Para usar una caché distribuida de Redis, haga referencia al [metapaquete Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) y agregue una referencia de paquete al paquete [Microsoft. Extensions. Caching. Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) .</span><span class="sxs-lookup"><span data-stu-id="79e94-127">To use a Redis distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and add a package reference to the [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) package.</span></span> <span data-ttu-id="79e94-128">El paquete de Redis no está incluido en el paquete de `Microsoft.AspNetCore.App`, por lo que debe hacer referencia al paquete de Redis por separado en el archivo de proyecto.</span><span class="sxs-lookup"><span data-stu-id="79e94-128">The Redis package isn't included in the `Microsoft.AspNetCore.App` package, so you must reference the Redis package separately in your project file.</span></span>

<span data-ttu-id="79e94-129">Para usar caché distribuida de NCache, haga referencia al [metapaquete Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) y agregue una referencia de paquete al paquete [NCache. Microsoft. Extensions. Caching. código abierto](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource) .</span><span class="sxs-lookup"><span data-stu-id="79e94-129">To use NCache distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and add a package reference to the [NCache.Microsoft.Extensions.Caching.OpenSource](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource) package.</span></span> <span data-ttu-id="79e94-130">El paquete de NCache no está incluido en el paquete de `Microsoft.AspNetCore.App`, por lo que debe hacer referencia al paquete de NCache por separado en el archivo de proyecto.</span><span class="sxs-lookup"><span data-stu-id="79e94-130">The NCache package isn't included in the `Microsoft.AspNetCore.App` package, so you must reference the NCache package separately in your project file.</span></span>

::: moniker-end

## <a name="idistributedcache-interface"></a><span data-ttu-id="79e94-131">Interfaz IDistributedCache</span><span class="sxs-lookup"><span data-stu-id="79e94-131">IDistributedCache interface</span></span>

<span data-ttu-id="79e94-132">La interfaz de <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> proporciona los métodos siguientes para manipular los elementos de la implementación de caché distribuida:</span><span class="sxs-lookup"><span data-stu-id="79e94-132">The <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface provides the following methods to manipulate items in the distributed cache implementation:</span></span>

* <span data-ttu-id="79e94-133"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> &ndash; acepta una clave de cadena y recupera un elemento almacenado en caché como una matriz de `byte[]` si se encuentra en la memoria caché.</span><span class="sxs-lookup"><span data-stu-id="79e94-133"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> &ndash; Accepts a string key and retrieves a cached item as a `byte[]` array if found in the cache.</span></span>
* <span data-ttu-id="79e94-134"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> &ndash; agrega un elemento (como `byte[]` matriz) a la memoria caché utilizando una clave de cadena.</span><span class="sxs-lookup"><span data-stu-id="79e94-134"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> &ndash; Adds an item (as `byte[]` array) to the cache using a string key.</span></span>
* <span data-ttu-id="79e94-135"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*> &ndash; actualiza un elemento en la memoria caché en función de su clave, restableciendo el tiempo de espera de expiración variable (si existe).</span><span class="sxs-lookup"><span data-stu-id="79e94-135"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*> &ndash; Refreshes an item in the cache based on its key, resetting its sliding expiration timeout (if any).</span></span>
* <span data-ttu-id="79e94-136"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> &ndash; quita un elemento de la memoria caché en función de su clave de cadena.</span><span class="sxs-lookup"><span data-stu-id="79e94-136"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> &ndash; Removes a cache item based on its string key.</span></span>

## <a name="establish-distributed-caching-services"></a><span data-ttu-id="79e94-137">Establecer servicios de almacenamiento en caché distribuidos</span><span class="sxs-lookup"><span data-stu-id="79e94-137">Establish distributed caching services</span></span>

<span data-ttu-id="79e94-138">Registre una implementación de <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="79e94-138">Register an implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="79e94-139">Las implementaciones proporcionadas por el marco de trabajo que se describen en este tema incluyen:</span><span class="sxs-lookup"><span data-stu-id="79e94-139">Framework-provided implementations described in this topic include:</span></span>

* [<span data-ttu-id="79e94-140">Caché de memoria distribuida</span><span class="sxs-lookup"><span data-stu-id="79e94-140">Distributed Memory Cache</span></span>](#distributed-memory-cache)
* [<span data-ttu-id="79e94-141">Caché de SQL Server distribuida</span><span class="sxs-lookup"><span data-stu-id="79e94-141">Distributed SQL Server cache</span></span>](#distributed-sql-server-cache)
* [<span data-ttu-id="79e94-142">Caché en Redis distribuida</span><span class="sxs-lookup"><span data-stu-id="79e94-142">Distributed Redis cache</span></span>](#distributed-redis-cache)
* [<span data-ttu-id="79e94-143">Caché de NCache distribuida</span><span class="sxs-lookup"><span data-stu-id="79e94-143">Distributed NCache cache</span></span>](#distributed-ncache-cache)

### <a name="distributed-memory-cache"></a><span data-ttu-id="79e94-144">Caché de memoria distribuida</span><span class="sxs-lookup"><span data-stu-id="79e94-144">Distributed Memory Cache</span></span>

<span data-ttu-id="79e94-145">La memoria caché de memoria distribuida (<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*>) es una implementación proporcionada por el marco de <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> que almacena elementos en la memoria.</span><span class="sxs-lookup"><span data-stu-id="79e94-145">The Distributed Memory Cache (<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*>) is a framework-provided implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> that stores items in memory.</span></span> <span data-ttu-id="79e94-146">La caché de memoria distribuida no es una caché distribuida real.</span><span class="sxs-lookup"><span data-stu-id="79e94-146">The Distributed Memory Cache isn't an actual distributed cache.</span></span> <span data-ttu-id="79e94-147">Los elementos almacenados en caché se almacenan en la instancia de la aplicación en el servidor donde se ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="79e94-147">Cached items are stored by the app instance on the server where the app is running.</span></span>

<span data-ttu-id="79e94-148">La caché de memoria distribuida es una implementación útil:</span><span class="sxs-lookup"><span data-stu-id="79e94-148">The Distributed Memory Cache is a useful implementation:</span></span>

* <span data-ttu-id="79e94-149">En escenarios de desarrollo y pruebas.</span><span class="sxs-lookup"><span data-stu-id="79e94-149">In development and testing scenarios.</span></span>
* <span data-ttu-id="79e94-150">Cuando se usa un solo servidor en producción y el consumo de memoria no es un problema.</span><span class="sxs-lookup"><span data-stu-id="79e94-150">When a single server is used in production and memory consumption isn't an issue.</span></span> <span data-ttu-id="79e94-151">La implementación de la caché de memoria distribuida abstrae el almacenamiento de datos en caché.</span><span class="sxs-lookup"><span data-stu-id="79e94-151">Implementing the Distributed Memory Cache abstracts cached data storage.</span></span> <span data-ttu-id="79e94-152">Permite implementar una solución de almacenamiento en caché distribuida verdadera en el futuro si se necesitan varios nodos o tolerancia a errores.</span><span class="sxs-lookup"><span data-stu-id="79e94-152">It allows for implementing a true distributed caching solution in the future if multiple nodes or fault tolerance become necessary.</span></span>

<span data-ttu-id="79e94-153">La aplicación de ejemplo hace uso de la caché de memoria distribuida cuando la aplicación se ejecuta en el entorno de desarrollo en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="79e94-153">The sample app makes use of the Distributed Memory Cache when the app is run in the Development environment in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedMemoryCache)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedMemoryCache)]

::: moniker-end

### <a name="distributed-sql-server-cache"></a><span data-ttu-id="79e94-154">Caché de SQL Server distribuida</span><span class="sxs-lookup"><span data-stu-id="79e94-154">Distributed SQL Server Cache</span></span>

<span data-ttu-id="79e94-155">La implementación de la memoria caché de SQL Server distribuida (<xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*>) permite que la memoria caché distribuida use una base de datos de SQL Server como su memoria auxiliar.</span><span class="sxs-lookup"><span data-stu-id="79e94-155">The Distributed SQL Server Cache implementation (<xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*>) allows the distributed cache to use a SQL Server database as its backing store.</span></span> <span data-ttu-id="79e94-156">Para crear una SQL Server tabla de elementos almacenados en caché en una instancia de SQL Server, puede usar la herramienta `sql-cache`.</span><span class="sxs-lookup"><span data-stu-id="79e94-156">To create a SQL Server cached item table in a SQL Server instance, you can use the `sql-cache` tool.</span></span> <span data-ttu-id="79e94-157">La herramienta crea una tabla con el nombre y el esquema que especifique.</span><span class="sxs-lookup"><span data-stu-id="79e94-157">The tool creates a table with the name and schema that you specify.</span></span>

<span data-ttu-id="79e94-158">Cree una tabla en SQL Server ejecutando el comando `sql-cache create`.</span><span class="sxs-lookup"><span data-stu-id="79e94-158">Create a table in SQL Server by running the `sql-cache create` command.</span></span> <span data-ttu-id="79e94-159">Proporcione la instancia de SQL Server (`Data Source`), la base de datos (`Initial Catalog`), el esquema (por ejemplo, `dbo`) y el nombre de la tabla (por ejemplo, `TestCache`):</span><span class="sxs-lookup"><span data-stu-id="79e94-159">Provide the SQL Server instance (`Data Source`), database (`Initial Catalog`), schema (for example, `dbo`), and table name (for example, `TestCache`):</span></span>

```dotnetcli
dotnet sql-cache create "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
```

<span data-ttu-id="79e94-160">Se registra un mensaje para indicar que la herramienta se ha realizado correctamente:</span><span class="sxs-lookup"><span data-stu-id="79e94-160">A message is logged to indicate that the tool was successful:</span></span>

```console
Table and index were created successfully.
```

<span data-ttu-id="79e94-161">La tabla creada por la herramienta `sql-cache` tiene el siguiente esquema:</span><span class="sxs-lookup"><span data-stu-id="79e94-161">The table created by the `sql-cache` tool has the following schema:</span></span>

![Tabla de caché SqlServer](distributed/_static/SqlServerCacheTable.png)

> [!NOTE]
> <span data-ttu-id="79e94-163">Una aplicación debe manipular los valores de la memoria caché mediante una instancia de <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>, no una <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>.</span><span class="sxs-lookup"><span data-stu-id="79e94-163">An app should manipulate cache values using an instance of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>, not a <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>.</span></span>

<span data-ttu-id="79e94-164">La aplicación de ejemplo implementa <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> en un entorno que no es de desarrollo en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="79e94-164">The sample app implements <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> in a non-Development environment in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedSqlServerCache)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedSqlServerCache)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="79e94-165">Un <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*> (y, opcionalmente, <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*> y <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*>) se almacena normalmente fuera del control de código fuente (por ejemplo, lo almacena el [Administrador secreto](xref:security/app-secrets) o en *appsettings. JSON*/*appSettings. { ENVIRONMENT}. JSON* files).</span><span class="sxs-lookup"><span data-stu-id="79e94-165">A <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*> (and optionally, <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*> and <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*>) are typically stored outside of source control (for example, stored by the [Secret Manager](xref:security/app-secrets) or in *appsettings.json*/*appsettings.{ENVIRONMENT}.json* files).</span></span> <span data-ttu-id="79e94-166">La cadena de conexión puede contener credenciales que deben mantenerse fuera de los sistemas de control de código fuente.</span><span class="sxs-lookup"><span data-stu-id="79e94-166">The connection string may contain credentials that should be kept out of source control systems.</span></span>

### <a name="distributed-redis-cache"></a><span data-ttu-id="79e94-167">Redis Cache distribuido</span><span class="sxs-lookup"><span data-stu-id="79e94-167">Distributed Redis Cache</span></span>

<span data-ttu-id="79e94-168">[Redis](https://redis.io/) es un almacén de datos en memoria de código abierto, que a menudo se usa como caché distribuida.</span><span class="sxs-lookup"><span data-stu-id="79e94-168">[Redis](https://redis.io/) is an open source in-memory data store, which is often used as a distributed cache.</span></span> <span data-ttu-id="79e94-169">Puede usar Redis localmente, y puede configurar un [Azure Redis cache](https://azure.microsoft.com/services/cache/) para una aplicación de ASP.net Core hospedada en Azure.</span><span class="sxs-lookup"><span data-stu-id="79e94-169">You can use Redis locally, and you can configure an [Azure Redis Cache](https://azure.microsoft.com/services/cache/) for an Azure-hosted ASP.NET Core app.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="79e94-170">Una aplicación configura la implementación de la memoria caché con una instancia de <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> (<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>) en un entorno que no es de desarrollo en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="79e94-170">An app configures the cache implementation using a <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> instance (<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>) in a non-Development environment in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_AddStackExchangeRedisCache)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="79e94-171">Una aplicación configura la implementación de la memoria caché con una instancia de <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> (<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>) en un entorno que no es de desarrollo en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="79e94-171">An app configures the cache implementation using a <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> instance (<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>) in a non-Development environment in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddStackExchangeRedisCache)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="79e94-172">Una aplicación configura la implementación de la memoria caché mediante una instancia de <xref:Microsoft.Extensions.Caching.Redis.RedisCache> (<xref:Microsoft.Extensions.DependencyInjection.RedisCacheServiceCollectionExtensions.AddDistributedRedisCache*>):</span><span class="sxs-lookup"><span data-stu-id="79e94-172">An app configures the cache implementation using a <xref:Microsoft.Extensions.Caching.Redis.RedisCache> instance (<xref:Microsoft.Extensions.DependencyInjection.RedisCacheServiceCollectionExtensions.AddDistributedRedisCache*>):</span></span>

```csharp
services.AddDistributedRedisCache(options =>
{
    options.Configuration = "localhost";
    options.InstanceName = "SampleInstance";
});
```

::: moniker-end

<span data-ttu-id="79e94-173">Para instalar Redis en el equipo local:</span><span class="sxs-lookup"><span data-stu-id="79e94-173">To install Redis on your local machine:</span></span>

1. <span data-ttu-id="79e94-174">Instale el [paquete chocolatey Redis](https://chocolatey.org/packages/redis-64/).</span><span class="sxs-lookup"><span data-stu-id="79e94-174">Install the [Chocolatey Redis package](https://chocolatey.org/packages/redis-64/).</span></span>
1. <span data-ttu-id="79e94-175">Ejecute `redis-server` desde un símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="79e94-175">Run `redis-server` from a command prompt.</span></span>

### <a name="distributed-ncache-cache"></a><span data-ttu-id="79e94-176">Caché de NCache distribuida</span><span class="sxs-lookup"><span data-stu-id="79e94-176">Distributed NCache Cache</span></span>

<span data-ttu-id="79e94-177">[NCache](https://github.com/Alachisoft/NCache) es una caché distribuida en memoria de código abierto desarrollada de forma nativa en .NET y .net Core.</span><span class="sxs-lookup"><span data-stu-id="79e94-177">[NCache](https://github.com/Alachisoft/NCache) is an open source in-memory distributed cache developed natively in .NET and .NET Core.</span></span> <span data-ttu-id="79e94-178">NCache funciona localmente y se configura como un clúster de caché distribuida para una aplicación ASP.NET Core que se ejecuta en Azure o en otras plataformas de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="79e94-178">NCache works both locally and configured as a distributed cache cluster for an ASP.NET Core app running in Azure or on other hosting platforms.</span></span>

<span data-ttu-id="79e94-179">Para instalar y configurar NCache en el equipo local, consulte [Guía de introducción de NCache para Windows](https://www.alachisoft.com/resources/docs/ncache-oss/getting-started-guide-windows/).</span><span class="sxs-lookup"><span data-stu-id="79e94-179">To install and configure NCache on your local machine, see [NCache Getting Started Guide for Windows](https://www.alachisoft.com/resources/docs/ncache-oss/getting-started-guide-windows/).</span></span>

<span data-ttu-id="79e94-180">Para configurar NCache:</span><span class="sxs-lookup"><span data-stu-id="79e94-180">To configure NCache:</span></span>

1. <span data-ttu-id="79e94-181">Instalación de [NuGet de código abierto de NCache](https://www.nuget.org/packages/Alachisoft.NCache.OpenSource.SDK/).</span><span class="sxs-lookup"><span data-stu-id="79e94-181">Install [NCache open source NuGet](https://www.nuget.org/packages/Alachisoft.NCache.OpenSource.SDK/).</span></span>
1. <span data-ttu-id="79e94-182">Configure el clúster de caché en [Client. ncconf](https://www.alachisoft.com/resources/docs/ncache-oss/admin-guide/client-config.html).</span><span class="sxs-lookup"><span data-stu-id="79e94-182">Configure the cache cluster in [client.ncconf](https://www.alachisoft.com/resources/docs/ncache-oss/admin-guide/client-config.html).</span></span>
1. <span data-ttu-id="79e94-183">Agregue el siguiente código a `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="79e94-183">Add the following code to `Startup.ConfigureServices`:</span></span>

   ```csharp
   services.AddNCacheDistributedCache(configuration =>    
   {        
       configuration.CacheName = "demoClusteredCache";
       configuration.EnableLogs = true;
       configuration.ExceptionsEnabled = true;
   });
   ```

## <a name="use-the-distributed-cache"></a><span data-ttu-id="79e94-184">Usar la memoria caché distribuida</span><span class="sxs-lookup"><span data-stu-id="79e94-184">Use the distributed cache</span></span>

<span data-ttu-id="79e94-185">Para usar la interfaz <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>, solicite una instancia de <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> de cualquier constructor de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="79e94-185">To use the <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface, request an instance of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> from any constructor in the app.</span></span> <span data-ttu-id="79e94-186">La instancia se proporciona mediante la [inserción de dependencias (di)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="79e94-186">The instance is provided by [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="79e94-187">Cuando se inicia la aplicación de ejemplo, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> se inserta en `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="79e94-187">When the sample app starts, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> is injected into `Startup.Configure`.</span></span> <span data-ttu-id="79e94-188">La hora actual se almacena en la memoria caché mediante <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (para obtener más información, consulte [host genérico: IHostApplicationLifetime](xref:fundamentals/host/generic-host#ihostapplicationlifetime)):</span><span class="sxs-lookup"><span data-stu-id="79e94-188">The current time is cached using <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (for more information, see [Generic Host: IHostApplicationLifetime](xref:fundamentals/host/generic-host#ihostapplicationlifetime)):</span></span>

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_Configure&highlight=10)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="79e94-189">Cuando se inicia la aplicación de ejemplo, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> se inserta en `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="79e94-189">When the sample app starts, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> is injected into `Startup.Configure`.</span></span> <span data-ttu-id="79e94-190">La hora actual se almacena en la memoria caché mediante <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime> (para obtener más información, vea [host de Web: interfaz IApplicationLifetime](xref:fundamentals/host/web-host#iapplicationlifetime-interface)):</span><span class="sxs-lookup"><span data-stu-id="79e94-190">The current time is cached using <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime> (for more information, see [Web Host: IApplicationLifetime interface](xref:fundamentals/host/web-host#iapplicationlifetime-interface)):</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_Configure&highlight=10)]

::: moniker-end

<span data-ttu-id="79e94-191">La aplicación de ejemplo inserta <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> en el `IndexModel` para que lo use la página de índice.</span><span class="sxs-lookup"><span data-stu-id="79e94-191">The sample app injects <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> into the `IndexModel` for use by the Index page.</span></span>

<span data-ttu-id="79e94-192">Cada vez que se carga la página de índice, se comprueba la hora almacenada en la memoria caché en `OnGetAsync`.</span><span class="sxs-lookup"><span data-stu-id="79e94-192">Each time the Index page is loaded, the cache is checked for the cached time in `OnGetAsync`.</span></span> <span data-ttu-id="79e94-193">Si la hora almacenada en caché no ha expirado, se muestra la hora.</span><span class="sxs-lookup"><span data-stu-id="79e94-193">If the cached time hasn't expired, the time is displayed.</span></span> <span data-ttu-id="79e94-194">Si han transcurrido 20 segundos desde la última vez que se tuvo acceso a la hora almacenada en caché (la última vez que se cargó esta página), la página muestra la *fecha de expiración de la memoria caché*.</span><span class="sxs-lookup"><span data-stu-id="79e94-194">If 20 seconds have elapsed since the last time the cached time was accessed (the last time this page was loaded), the page displays *Cached Time Expired*.</span></span>

<span data-ttu-id="79e94-195">Actualice inmediatamente la hora almacenada en caché a la hora actual; para ello, seleccione el botón **restablecer hora de almacenamiento en caché** .</span><span class="sxs-lookup"><span data-stu-id="79e94-195">Immediately update the cached time to the current time by selecting the **Reset Cached Time** button.</span></span> <span data-ttu-id="79e94-196">El botón desencadena el método de control de `OnPostResetCachedTime`.</span><span class="sxs-lookup"><span data-stu-id="79e94-196">The button triggers the `OnPostResetCachedTime` handler method.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Pages/Index.cshtml.cs?name=snippet_IndexModel&highlight=7,14-20,25-29)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Pages/Index.cshtml.cs?name=snippet_IndexModel&highlight=7,14-20,25-29)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="79e94-197">No es necesario usar un singleton o una duración de ámbito para las instancias de <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> (al menos para las implementaciones integradas).</span><span class="sxs-lookup"><span data-stu-id="79e94-197">There's no need to use a Singleton or Scoped lifetime for <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> instances (at least for the built-in implementations).</span></span>
>
> <span data-ttu-id="79e94-198">También puede crear una instancia de <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> siempre que necesite una en lugar de usar DI, pero la creación de una instancia de en el código puede hacer que el código sea más difícil de probar e infringe el [principio de dependencias explícitas](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies).</span><span class="sxs-lookup"><span data-stu-id="79e94-198">You can also create an <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> instance wherever you might need one instead of using DI, but creating an instance in code can make your code harder to test and violates the [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies).</span></span>

## <a name="recommendations"></a><span data-ttu-id="79e94-199">Recomendaciones</span><span class="sxs-lookup"><span data-stu-id="79e94-199">Recommendations</span></span>

<span data-ttu-id="79e94-200">A la hora de decidir qué implementación de <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> es la más adecuada para su aplicación, tenga en cuenta lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="79e94-200">When deciding which implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> is best for your app, consider the following:</span></span>

* <span data-ttu-id="79e94-201">Infraestructura existente</span><span class="sxs-lookup"><span data-stu-id="79e94-201">Existing infrastructure</span></span>
* <span data-ttu-id="79e94-202">Requisitos de rendimiento</span><span class="sxs-lookup"><span data-stu-id="79e94-202">Performance requirements</span></span>
* <span data-ttu-id="79e94-203">Coste</span><span class="sxs-lookup"><span data-stu-id="79e94-203">Cost</span></span>
* <span data-ttu-id="79e94-204">Experiencia del equipo</span><span class="sxs-lookup"><span data-stu-id="79e94-204">Team experience</span></span>

<span data-ttu-id="79e94-205">Las soluciones de almacenamiento en caché suelen basarse en el almacenamiento en memoria para proporcionar una recuperación rápida de los datos almacenados en memoria caché, pero la memoria es un recurso limitado y se amplía de forma costosa.</span><span class="sxs-lookup"><span data-stu-id="79e94-205">Caching solutions usually rely on in-memory storage to provide fast retrieval of cached data, but memory is a limited resource and costly to expand.</span></span> <span data-ttu-id="79e94-206">Almacene solo los datos usados habitualmente en una memoria caché.</span><span class="sxs-lookup"><span data-stu-id="79e94-206">Only store commonly used data in a cache.</span></span>

<span data-ttu-id="79e94-207">Por lo general, una caché en Redis proporciona un mayor rendimiento y una menor latencia que una memoria caché de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="79e94-207">Generally, a Redis cache provides higher throughput and lower latency than a SQL Server cache.</span></span> <span data-ttu-id="79e94-208">Sin embargo, normalmente es necesario realizar pruebas comparativas para determinar las características de rendimiento de las estrategias de almacenamiento en caché.</span><span class="sxs-lookup"><span data-stu-id="79e94-208">However, benchmarking is usually required to determine the performance characteristics of caching strategies.</span></span>

<span data-ttu-id="79e94-209">Cuando SQL Server se usa como un almacén de respaldo de caché distribuida, el uso de la misma base de datos para la memoria caché y el almacenamiento y la recuperación de datos normales de la aplicación puede afectar negativamente al rendimiento de ambos.</span><span class="sxs-lookup"><span data-stu-id="79e94-209">When SQL Server is used as a distributed cache backing store, use of the same database for the cache and the app's ordinary data storage and retrieval can negatively impact the performance of both.</span></span> <span data-ttu-id="79e94-210">Se recomienda usar una instancia de SQL Server dedicada para el almacén de respaldo de caché distribuida.</span><span class="sxs-lookup"><span data-stu-id="79e94-210">We recommend using a dedicated SQL Server instance for the distributed cache backing store.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="79e94-211">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="79e94-211">Additional resources</span></span>

* [<span data-ttu-id="79e94-212">Redis Cache en Azure</span><span class="sxs-lookup"><span data-stu-id="79e94-212">Redis Cache on Azure</span></span>](/azure/azure-cache-for-redis/)
* [<span data-ttu-id="79e94-213">SQL Database en Azure</span><span class="sxs-lookup"><span data-stu-id="79e94-213">SQL Database on Azure</span></span>](/azure/sql-database/)
* <span data-ttu-id="79e94-214">[ASP.net Core del proveedor de IDistributedCache para NCache en granjas de servidores web](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache en github](https://github.com/Alachisoft/NCache))</span><span class="sxs-lookup"><span data-stu-id="79e94-214">[ASP.NET Core IDistributedCache Provider for NCache in Web Farms](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache on GitHub](https://github.com/Alachisoft/NCache))</span></span>
* <xref:performance/caching/memory>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>
