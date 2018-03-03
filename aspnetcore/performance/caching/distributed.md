---
title: "Trabajar con una memoria caché distribuida en ASP.NET Core"
author: ardalis
description: "Obtenga información acerca de cómo usar ASP.NET Core distribuida almacenamiento en caché para mejorar el rendimiento de la aplicación y la escalabilidad, especialmente en un entorno de granja de servidores en la nube o de servidor."
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: performance/caching/distributed
ms.openlocfilehash: 635c61cbb72a6a9eb822307bbc80936ee73bedc8
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2018
---
# <a name="working-with-a-distributed-cache-in-aspnet-core"></a><span data-ttu-id="31d18-103">Trabajar con una memoria caché distribuida en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="31d18-103">Working with a distributed cache in ASP.NET Core</span></span>

<span data-ttu-id="31d18-104">Por [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="31d18-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="31d18-105">Las memorias caché distribuidas pueden mejorar el rendimiento y la escalabilidad de las aplicaciones de ASP.NET Core, especialmente cuando se hospeda en un entorno de granja de servidores en la nube o de servidor.</span><span class="sxs-lookup"><span data-stu-id="31d18-105">Distributed caches can improve the performance and scalability of ASP.NET Core apps, especially when hosted in a cloud or server farm environment.</span></span> <span data-ttu-id="31d18-106">Este artículo explica cómo trabajar con implementaciones y abstracciones de caché distribuida integrado del núcleo de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="31d18-106">This article explains how to work with ASP.NET Core's built-in distributed cache abstractions and implementations.</span></span>

<span data-ttu-id="31d18-107">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="31d18-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-a-distributed-cache"></a><span data-ttu-id="31d18-108">¿Qué es una memoria caché distribuida</span><span class="sxs-lookup"><span data-stu-id="31d18-108">What is a distributed cache</span></span>

<span data-ttu-id="31d18-109">Una memoria caché distribuida es compartida por varios servidores de aplicación (consulte [conceptos básicos de almacenamiento en caché](memory.md#caching-basics)).</span><span class="sxs-lookup"><span data-stu-id="31d18-109">A distributed cache is shared by multiple app servers (see [Caching Basics](memory.md#caching-basics)).</span></span> <span data-ttu-id="31d18-110">La información de la memoria caché no se guardan en la memoria de servidores web individuales y los datos almacenados en caché están disponibles para todos los servidores de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="31d18-110">The information in the cache isn't stored in the memory of individual web servers, and the cached data is available to all of the app's servers.</span></span> <span data-ttu-id="31d18-111">Esto proporciona varias ventajas:</span><span class="sxs-lookup"><span data-stu-id="31d18-111">This provides several advantages:</span></span>

1. <span data-ttu-id="31d18-112">Datos almacenados en caché están coherentes en todos los servidores web.</span><span class="sxs-lookup"><span data-stu-id="31d18-112">Cached data is coherent on all web servers.</span></span> <span data-ttu-id="31d18-113">Los usuarios no verán los resultados diferentes dependiendo de qué web server controla su solicitud</span><span class="sxs-lookup"><span data-stu-id="31d18-113">Users don't see different results depending on which web server handles their request</span></span>

2. <span data-ttu-id="31d18-114">Datos almacenados en caché sobrevive reinicios del servidor web y las implementaciones.</span><span class="sxs-lookup"><span data-stu-id="31d18-114">Cached data survives web server restarts and deployments.</span></span> <span data-ttu-id="31d18-115">Servidores web individuales se pueden quitar o agregar sin afectar a la memoria caché.</span><span class="sxs-lookup"><span data-stu-id="31d18-115">Individual web servers can be removed or added without impacting the cache.</span></span>

3. <span data-ttu-id="31d18-116">El almacén de datos de origen tiene menos las solicitudes realizadas a él (de con varias cachés en memoria o en no caché en absoluto).</span><span class="sxs-lookup"><span data-stu-id="31d18-116">The source data store has fewer requests made to it (than with multiple in-memory caches or no cache at all).</span></span>

> [!NOTE]
> <span data-ttu-id="31d18-117">Si usa una memoria caché distribuida de SQL Server, algunas de las siguientes ventajas solo son true si se utiliza una instancia de base de datos independiente para la memoria caché que para los datos de origen de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="31d18-117">If using a SQL Server Distributed Cache, some of these advantages are only true if a separate database instance is used for the cache than for the app's source data.</span></span>

<span data-ttu-id="31d18-118">Al igual que cualquier memoria caché, una memoria caché distribuida puede mejorar considerablemente la capacidad de respuesta de una aplicación, ya que normalmente se pueden recuperar datos de la memoria caché mucho más rápida que de una base de datos relacional (o servicio web).</span><span class="sxs-lookup"><span data-stu-id="31d18-118">Like any cache, a distributed cache can dramatically improve an app's responsiveness, since typically data can be retrieved from the cache much faster than from a relational database (or web service).</span></span>

<span data-ttu-id="31d18-119">Configuración de la caché es específico de la implementación.</span><span class="sxs-lookup"><span data-stu-id="31d18-119">Cache configuration is implementation specific.</span></span> <span data-ttu-id="31d18-120">En este artículo se describe cómo configurar ambos Redis y distribuidas de SQL Server almacena en caché.</span><span class="sxs-lookup"><span data-stu-id="31d18-120">This article describes how to configure both Redis and SQL Server distributed caches.</span></span> <span data-ttu-id="31d18-121">Independientemente de qué implementación está seleccionada, la aplicación interactúa con la memoria caché utilizando una común `IDistributedCache` interfaz.</span><span class="sxs-lookup"><span data-stu-id="31d18-121">Regardless of which implementation is selected, the app interacts with the cache using a common `IDistributedCache` interface.</span></span>

## <a name="the-idistributedcache-interface"></a><span data-ttu-id="31d18-122">The IDistributedCache Interface</span><span class="sxs-lookup"><span data-stu-id="31d18-122">The IDistributedCache Interface</span></span>

<span data-ttu-id="31d18-123">El `IDistributedCache` interfaz incluye métodos sincrónicos y asincrónicos.</span><span class="sxs-lookup"><span data-stu-id="31d18-123">The `IDistributedCache` interface includes synchronous and asynchronous methods.</span></span> <span data-ttu-id="31d18-124">La interfaz permite elementos agregar, recuperar y quitar de la implementación de caché distribuida.</span><span class="sxs-lookup"><span data-stu-id="31d18-124">The interface allows items to be added, retrieved, and removed from the distributed cache implementation.</span></span> <span data-ttu-id="31d18-125">El `IDistributedCache` interfaz incluye los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="31d18-125">The `IDistributedCache` interface includes the following methods:</span></span>

<span data-ttu-id="31d18-126">**Get, GetAsync**</span><span class="sxs-lookup"><span data-stu-id="31d18-126">**Get, GetAsync**</span></span>

<span data-ttu-id="31d18-127">Toma una clave de cadena y recupera un elemento almacenado en caché como un `byte[]` si se encuentra en la memoria caché.</span><span class="sxs-lookup"><span data-stu-id="31d18-127">Takes a string key and retrieves a cached item as a `byte[]` if found in the cache.</span></span>

<span data-ttu-id="31d18-128">**Set, SetAsync**</span><span class="sxs-lookup"><span data-stu-id="31d18-128">**Set, SetAsync**</span></span>

<span data-ttu-id="31d18-129">Agrega un elemento (como `byte[]`) a la memoria caché utilizando una clave de cadena.</span><span class="sxs-lookup"><span data-stu-id="31d18-129">Adds an item (as `byte[]`) to the cache using a string key.</span></span>

<span data-ttu-id="31d18-130">**Refresh, RefreshAsync**</span><span class="sxs-lookup"><span data-stu-id="31d18-130">**Refresh, RefreshAsync**</span></span>

<span data-ttu-id="31d18-131">Actualiza un elemento en la memoria caché basado en su clave, restablecer su tiempo de espera de expiración deslizante (si existe).</span><span class="sxs-lookup"><span data-stu-id="31d18-131">Refreshes an item in the cache based on its key, resetting its sliding expiration timeout (if any).</span></span>

<span data-ttu-id="31d18-132">**Remove, RemoveAsync**</span><span class="sxs-lookup"><span data-stu-id="31d18-132">**Remove, RemoveAsync**</span></span>

<span data-ttu-id="31d18-133">Quita una entrada de caché basada en su clave.</span><span class="sxs-lookup"><span data-stu-id="31d18-133">Removes a cache entry based on its key.</span></span>

<span data-ttu-id="31d18-134">Para usar el `IDistributedCache` interfaz:</span><span class="sxs-lookup"><span data-stu-id="31d18-134">To use the `IDistributedCache` interface:</span></span>

   1. <span data-ttu-id="31d18-135">Agregue los paquetes de NuGet necesarios para el archivo de proyecto.</span><span class="sxs-lookup"><span data-stu-id="31d18-135">Add the required NuGet packages to your project file.</span></span>

   2. <span data-ttu-id="31d18-136">Configurar la implementación específica de `IDistributedCache` en su `Startup` la clase `ConfigureServices` método y lo agrega al contenedor no existe.</span><span class="sxs-lookup"><span data-stu-id="31d18-136">Configure the specific implementation of `IDistributedCache` in your `Startup` class's `ConfigureServices` method, and add it to the container there.</span></span>

   3. <span data-ttu-id="31d18-137">Desde la aplicación [Middleware](xref:fundamentals/middleware/index) o las clases de controlador MVC, solicite una instancia de `IDistributedCache` desde el constructor.</span><span class="sxs-lookup"><span data-stu-id="31d18-137">From the app's [Middleware](xref:fundamentals/middleware/index) or MVC controller classes, request an instance of `IDistributedCache` from the constructor.</span></span> <span data-ttu-id="31d18-138">La instancia se proporcionarán con [inyección de dependencia](../../fundamentals/dependency-injection.md) (DI).</span><span class="sxs-lookup"><span data-stu-id="31d18-138">The instance will be provided by [Dependency Injection](../../fundamentals/dependency-injection.md) (DI).</span></span>

> [!NOTE]
> <span data-ttu-id="31d18-139">No es necesario usar una duración de Singleton o en el ámbito para `IDistributedCache` instancias (al menos para las implementaciones integradas).</span><span class="sxs-lookup"><span data-stu-id="31d18-139">There's no need to use a Singleton or Scoped lifetime for `IDistributedCache` instances (at least for the built-in implementations).</span></span> <span data-ttu-id="31d18-140">También puede crear una instancia donde podría necesitar uno (en lugar de usar [inyección de dependencia](../../fundamentals/dependency-injection.md)), pero esto puede dificultar el código probar e infringe el [principio de dependencias explícitas](http://deviq.com/explicit-dependencies-principle/).</span><span class="sxs-lookup"><span data-stu-id="31d18-140">You can also create an instance wherever you might need one (instead of using [Dependency Injection](../../fundamentals/dependency-injection.md)), but this can make your code harder to test, and violates the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/).</span></span>

<span data-ttu-id="31d18-141">En el ejemplo siguiente se muestra cómo utilizar una instancia de `IDistributedCache` en un componente de middleware simple:</span><span class="sxs-lookup"><span data-stu-id="31d18-141">The following example shows how to use an instance of `IDistributedCache` in a simple middleware component:</span></span>

[!code-csharp[](./distributed/sample/src/DistCacheSample/StartTimeHeader.cs?highlight=15,18,21,27,28,29,30,31)]

<span data-ttu-id="31d18-142">En el código anterior, el valor almacenado en caché se lee pero nunca se ha escrito.</span><span class="sxs-lookup"><span data-stu-id="31d18-142">In the code above, the cached value is read, but never written.</span></span> <span data-ttu-id="31d18-143">En este ejemplo, el valor sólo se establece cuando se inicia un servidor y no cambia.</span><span class="sxs-lookup"><span data-stu-id="31d18-143">In this sample, the value is only set when a server starts up, and doesn't change.</span></span> <span data-ttu-id="31d18-144">En un escenario de varios servidores, el servidor más reciente para iniciar sobrescribirá los valores anteriores que se establecieron con otros servidores.</span><span class="sxs-lookup"><span data-stu-id="31d18-144">In a multi-server scenario, the most recent server to start will overwrite any previous values that were set by other servers.</span></span> <span data-ttu-id="31d18-145">El `Get` y `Set` métodos utilizan el `byte[]` tipo.</span><span class="sxs-lookup"><span data-stu-id="31d18-145">The `Get` and `Set` methods use the `byte[]` type.</span></span> <span data-ttu-id="31d18-146">Por lo tanto, se debe convertir el valor de cadena mediante `Encoding.UTF8.GetString` (para `Get`) y `Encoding.UTF8.GetBytes` (para `Set`).</span><span class="sxs-lookup"><span data-stu-id="31d18-146">Therefore, the string value must be converted using `Encoding.UTF8.GetString` (for `Get`) and `Encoding.UTF8.GetBytes` (for `Set`).</span></span>

<span data-ttu-id="31d18-147">El siguiente código *Startup.cs* muestra el valor que se va a establecer:</span><span class="sxs-lookup"><span data-stu-id="31d18-147">The following code from *Startup.cs* shows the value being set:</span></span>

[!code-csharp[](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=2,4,5,6&range=58-66)]

> [!NOTE]
> <span data-ttu-id="31d18-148">Puesto que `IDistributedCache` está configurado en el `ConfigureServices` método, está disponible para el `Configure` método como parámetro.</span><span class="sxs-lookup"><span data-stu-id="31d18-148">Since `IDistributedCache` is configured in the `ConfigureServices` method, it's available to the `Configure` method as a parameter.</span></span> <span data-ttu-id="31d18-149">Agregar como un parámetro permitirá la instancia configurada proporcionar a través de DI.</span><span class="sxs-lookup"><span data-stu-id="31d18-149">Adding it as a parameter will allow the configured instance to be provided through DI.</span></span>

## <a name="using-a-redis-distributed-cache"></a><span data-ttu-id="31d18-150">Uso de una caché de Redis distribuida</span><span class="sxs-lookup"><span data-stu-id="31d18-150">Using a Redis distributed cache</span></span>

<span data-ttu-id="31d18-151">[Redis](https://redis.io/) es un almacén de datos en memoria de código abierto, que a menudo se usa como una memoria caché distribuida.</span><span class="sxs-lookup"><span data-stu-id="31d18-151">[Redis](https://redis.io/) is an open source in-memory data store, which is often used as a distributed cache.</span></span> <span data-ttu-id="31d18-152">Puede usar de forma local, y puede configurar un [Azure Redis Cache](https://azure.microsoft.com/services/cache/) para las aplicaciones principales de ASP.NET hospedados en Azure.</span><span class="sxs-lookup"><span data-stu-id="31d18-152">You can use it locally, and you can configure an [Azure Redis Cache](https://azure.microsoft.com/services/cache/) for your Azure-hosted ASP.NET Core apps.</span></span> <span data-ttu-id="31d18-153">La aplicación de ASP.NET Core configura la implementación de caché mediante un `RedisDistributedCache` instancia.</span><span class="sxs-lookup"><span data-stu-id="31d18-153">Your ASP.NET Core app configures the cache implementation using a `RedisDistributedCache` instance.</span></span>

<span data-ttu-id="31d18-154">Configurar la implementación de Redis en `ConfigureServices` y tener acceso a él en el código de aplicación solicitando una instancia de `IDistributedCache` (vea el código anterior).</span><span class="sxs-lookup"><span data-stu-id="31d18-154">You configure the Redis implementation in `ConfigureServices` and access it in your app code by requesting an instance of `IDistributedCache` (see the code above).</span></span>

<span data-ttu-id="31d18-155">En el código de ejemplo, un `RedisCache` implementación se utiliza cuando el servidor está configurado para un `Staging` entorno.</span><span class="sxs-lookup"><span data-stu-id="31d18-155">In the sample code, a `RedisCache` implementation is used when the server is configured for a `Staging` environment.</span></span> <span data-ttu-id="31d18-156">Por lo tanto la `ConfigureStagingServices` método configura el `RedisCache`:</span><span class="sxs-lookup"><span data-stu-id="31d18-156">Thus the `ConfigureStagingServices` method configures the `RedisCache`:</span></span>

[!code-csharp[](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=8,9,10,11,12,13&range=27-40)]

> [!NOTE]
> <span data-ttu-id="31d18-157">Para instalar Redis en el equipo local, instale el paquete chocolatey [https://chocolatey.org/packages/redis-64/](https://chocolatey.org/packages/redis-64/) y ejecutar `redis-server` desde un símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="31d18-157">To install Redis on your local machine, install the chocolatey package [https://chocolatey.org/packages/redis-64/](https://chocolatey.org/packages/redis-64/) and run `redis-server` from a command prompt.</span></span>

## <a name="using-a-sql-server-distributed-cache"></a><span data-ttu-id="31d18-158">Uso de un servidor SQL de caché distribuida</span><span class="sxs-lookup"><span data-stu-id="31d18-158">Using a SQL Server distributed cache</span></span>

<span data-ttu-id="31d18-159">La implementación de SqlServerCache permite que la memoria caché distribuida usar una base de datos de SQL Server como su memoria auxiliar.</span><span class="sxs-lookup"><span data-stu-id="31d18-159">The SqlServerCache implementation allows the distributed cache to use a SQL Server database as its backing store.</span></span> <span data-ttu-id="31d18-160">Para crear SQL Server se puede utilizar la herramienta de caché de sql, la herramienta crea una tabla con el nombre y el esquema que especifica.</span><span class="sxs-lookup"><span data-stu-id="31d18-160">To create SQL Server table you can use sql-cache tool, the tool creates a table with the name and schema you specify.</span></span>

<span data-ttu-id="31d18-161">Para usar la herramienta de caché de sql, agregue `SqlConfig.Tools` a la `<ItemGroup>` elemento de la *.csproj* archivo y ejecutar la restauración de dotnet.</span><span class="sxs-lookup"><span data-stu-id="31d18-161">To use the sql-cache tool, add `SqlConfig.Tools` to the `<ItemGroup>` element of the *.csproj* file and run dotnet restore.</span></span>

[!code-xml[](./distributed/sample/src/DistCacheSample/DistCacheSample.csproj?range=23-25)]

<span data-ttu-id="31d18-162">Ejecute el siguiente comando para comprobar SqlConfig.Tools</span><span class="sxs-lookup"><span data-stu-id="31d18-162">Test SqlConfig.Tools by running the following command</span></span>

```none
C:\DistCacheSample\src\DistCacheSample>dotnet sql-cache create --help
   ```

<span data-ttu-id="31d18-163">herramienta de caché de SQL muestra ayuda sobre uso, opciones y comandos, ahora puede crear tablas en sql server, ejecuta el comando "creación de caché de sql":</span><span class="sxs-lookup"><span data-stu-id="31d18-163">sql-cache tool  will display usage, options and command help, now you can create tables into sql server, running "sql-cache create" command :</span></span>

```none
C:\DistCacheSample\src\DistCacheSample>dotnet sql-cache create "Data Source=(localdb)\v11.0;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
   info: Microsoft.Extensions.Caching.SqlConfig.Tools.Program[0]
       Table and index were created successfully.
   ```

<span data-ttu-id="31d18-164">La tabla creada tiene el siguiente esquema:</span><span class="sxs-lookup"><span data-stu-id="31d18-164">The created table has the following schema:</span></span>

![Tabla de la caché de SQL Server](distributed/_static/SqlServerCacheTable.png)

<span data-ttu-id="31d18-166">Al igual que todas las implementaciones de caché, la aplicación debe obtener y establecer valores de caché mediante una instancia de `IDistributedCache`, no un `SqlServerCache`.</span><span class="sxs-lookup"><span data-stu-id="31d18-166">Like all cache implementations, your app should get and set cache values using an instance of `IDistributedCache`, not a `SqlServerCache`.</span></span> <span data-ttu-id="31d18-167">El ejemplo implementa `SqlServerCache` en el `Production` entorno (por lo que se configura en `ConfigureProductionServices`).</span><span class="sxs-lookup"><span data-stu-id="31d18-167">The sample implements `SqlServerCache` in the `Production` environment (so it's configured in `ConfigureProductionServices`).</span></span>

[!code-csharp[](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=7,8,9,10,11,12&range=42-56)]

> [!NOTE]
> <span data-ttu-id="31d18-168">El `ConnectionString` (y, opcionalmente, `SchemaName` y `TableName`) normalmente deberían almacenarse fuera de control de código fuente (por ejemplo, UserSecrets), ya que pueden contener las credenciales.</span><span class="sxs-lookup"><span data-stu-id="31d18-168">The `ConnectionString` (and optionally, `SchemaName` and `TableName`) should typically be stored outside of source control (such as UserSecrets), as they may contain credentials.</span></span>

## <a name="recommendations"></a><span data-ttu-id="31d18-169">Recomendaciones</span><span class="sxs-lookup"><span data-stu-id="31d18-169">Recommendations</span></span>

<span data-ttu-id="31d18-170">La hora de decidir qué implementación de `IDistributedCache` es adecuado para la aplicación, elija entre Redis y SQL Server se basa en la infraestructura existente y entorno, los requisitos de rendimiento y experiencia de su equipo.</span><span class="sxs-lookup"><span data-stu-id="31d18-170">When deciding which implementation of `IDistributedCache` is right for your app, choose between Redis and SQL Server based on your existing infrastructure and environment, your performance requirements, and your team's experience.</span></span> <span data-ttu-id="31d18-171">Si su equipo es más fácil trabajar con Redis, es una excelente opción.</span><span class="sxs-lookup"><span data-stu-id="31d18-171">If your team is more comfortable working with Redis, it's an excellent choice.</span></span> <span data-ttu-id="31d18-172">Si el equipo prefiera SQL Server, puede estar seguro de que así la implementación.</span><span class="sxs-lookup"><span data-stu-id="31d18-172">If your team prefers SQL Server, you can be confident in that implementation as well.</span></span> <span data-ttu-id="31d18-173">Tenga en cuenta que una solución de almacenamiento en caché tradicional almacena datos en memoria que permite la recuperación rápida de datos.</span><span class="sxs-lookup"><span data-stu-id="31d18-173">Note that A traditional caching solution stores data in-memory which allows for fast retrieval of data.</span></span> <span data-ttu-id="31d18-174">Debe almacenar los datos de uso frecuente en una memoria caché y almacenar todos los datos en un almacén persistente de back-end como SQL Server o el almacenamiento de Azure.</span><span class="sxs-lookup"><span data-stu-id="31d18-174">You should store commonly used data in a cache and store the entire data in a backend persistent store such as SQL Server or Azure Storage.</span></span> <span data-ttu-id="31d18-175">Caché en Redis es una solución de almacenamiento en caché que proporciona un alto rendimiento y baja latencia en comparación con la memoria caché de SQL.</span><span class="sxs-lookup"><span data-stu-id="31d18-175">Redis Cache is a caching solution which gives you high throughput and low latency as compared to SQL Cache.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="31d18-176">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="31d18-176">Additional resources</span></span>

* [<span data-ttu-id="31d18-177">Caché en Azure en Redis</span><span class="sxs-lookup"><span data-stu-id="31d18-177">Redis Cache on Azure</span></span>](https://azure.microsoft.com/documentation/services/redis-cache/)
* [<span data-ttu-id="31d18-178">Base de datos SQL en Azure</span><span class="sxs-lookup"><span data-stu-id="31d18-178">SQL Database on Azure</span></span>](https://azure.microsoft.com/documentation/services/sql-database/)
* [<span data-ttu-id="31d18-179">Almacenamiento caché en memoria</span><span class="sxs-lookup"><span data-stu-id="31d18-179">In-memory caching</span></span>](xref:performance/caching/memory)
* [<span data-ttu-id="31d18-180">Detectar cambios con tokens de cambio</span><span class="sxs-lookup"><span data-stu-id="31d18-180">Detect changes with change tokens</span></span>](xref:fundamentals/primitives/change-tokens)
* [<span data-ttu-id="31d18-181">Almacenamiento en caché de respuestas</span><span class="sxs-lookup"><span data-stu-id="31d18-181">Response caching</span></span>](xref:performance/caching/response)
* [<span data-ttu-id="31d18-182">Middleware de almacenamiento en caché de respuestas</span><span class="sxs-lookup"><span data-stu-id="31d18-182">Response Caching Middleware</span></span>](xref:performance/caching/middleware)
* [<span data-ttu-id="31d18-183">Aplicación auxiliar de etiquetas de caché</span><span class="sxs-lookup"><span data-stu-id="31d18-183">Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [<span data-ttu-id="31d18-184">Aplicación auxiliar de etiquetas de caché distribuida</span><span class="sxs-lookup"><span data-stu-id="31d18-184">Distributed Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
