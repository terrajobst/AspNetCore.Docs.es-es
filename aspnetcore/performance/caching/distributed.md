---
title: "Trabajar con una memoria caché distribuida"
author: ardalis
description: 
keywords: "Núcleo de ASP.NET,"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: 870f082d-6d43-453d-b311-45f3aeb4d2c5
ms.technology: aspnet
ms.prod: asp.net-core
uid: performance/caching/distributed
ms.openlocfilehash: 09a1a30de38b9eb40d4fa6a684a7d43ac3e0413c
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2017
---
# <a name="working-with-a-distributed-cache"></a><span data-ttu-id="f1ebc-103">Trabajar con una memoria caché distribuida</span><span class="sxs-lookup"><span data-stu-id="f1ebc-103">Working with a distributed cache</span></span>

<span data-ttu-id="f1ebc-104">Por [Steve Smith](http://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="f1ebc-104">By [Steve Smith](http://ardalis.com)</span></span>

<span data-ttu-id="f1ebc-105">Las memorias caché distribuidas pueden mejorar el rendimiento y la escalabilidad de las aplicaciones de ASP.NET Core, especialmente cuando se hospeda en un entorno de granja de servidores en la nube o de servidor.</span><span class="sxs-lookup"><span data-stu-id="f1ebc-105">Distributed caches can improve the performance and scalability of ASP.NET Core apps, especially when hosted in a cloud or server farm environment.</span></span> <span data-ttu-id="f1ebc-106">Este artículo explica cómo trabajar con implementaciones y abstracciones de caché distribuida integrado del núcleo de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f1ebc-106">This article explains how to work with ASP.NET Core's built-in distributed cache abstractions and implementations.</span></span>

[<span data-ttu-id="f1ebc-107">Ver o descargar el código de ejemplo</span><span class="sxs-lookup"><span data-stu-id="f1ebc-107">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample)

## <a name="what-is-a-distributed-cache"></a><span data-ttu-id="f1ebc-108">¿Qué es una memoria caché distribuida</span><span class="sxs-lookup"><span data-stu-id="f1ebc-108">What is a Distributed Cache</span></span>

<span data-ttu-id="f1ebc-109">Una memoria caché distribuida es compartida por varios servidores de aplicación (consulte [conceptos básicos de almacenamiento en caché](memory.md#caching-basics)).</span><span class="sxs-lookup"><span data-stu-id="f1ebc-109">A distributed cache is shared by multiple app servers (see [Caching Basics](memory.md#caching-basics)).</span></span> <span data-ttu-id="f1ebc-110">La información en la memoria caché no se almacena en la memoria de servidores web individuales y los datos almacenados en caché están disponibles para todos los servidores de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f1ebc-110">The information in the cache is not stored in the memory of individual web servers, and the cached data is available to all of the app's servers.</span></span> <span data-ttu-id="f1ebc-111">Esto proporciona varias ventajas:</span><span class="sxs-lookup"><span data-stu-id="f1ebc-111">This provides several advantages:</span></span>

1. <span data-ttu-id="f1ebc-112">Datos almacenados en caché están coherentes en todos los servidores web.</span><span class="sxs-lookup"><span data-stu-id="f1ebc-112">Cached data is coherent on all web servers.</span></span> <span data-ttu-id="f1ebc-113">Los usuarios no verán los resultados diferentes dependiendo de qué web server controla su solicitud</span><span class="sxs-lookup"><span data-stu-id="f1ebc-113">Users don't see different results depending on which web server handles their request</span></span>

2. <span data-ttu-id="f1ebc-114">Datos almacenados en caché sobrevive reinicios del servidor web y las implementaciones.</span><span class="sxs-lookup"><span data-stu-id="f1ebc-114">Cached data survives web server restarts and deployments.</span></span> <span data-ttu-id="f1ebc-115">Servidores web individuales se pueden quitar o agregar sin afectar a la memoria caché.</span><span class="sxs-lookup"><span data-stu-id="f1ebc-115">Individual web servers can be removed or added without impacting the cache.</span></span>

3. <span data-ttu-id="f1ebc-116">El almacén de datos de origen tiene menos las solicitudes realizadas a él (de con varias cachés en memoria o en no caché en absoluto).</span><span class="sxs-lookup"><span data-stu-id="f1ebc-116">The source data store has fewer requests made to it (than with multiple in-memory caches or no cache at all).</span></span>

> [!NOTE]
> <span data-ttu-id="f1ebc-117">Si usa una memoria caché distribuida de SQL Server, algunas de las siguientes ventajas solo son true si se utiliza una instancia de base de datos independiente para la memoria caché que para los datos de origen de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f1ebc-117">If using a SQL Server Distributed Cache, some of these advantages are only true if a separate database instance is used for the cache than for the app's source data.</span></span>

<span data-ttu-id="f1ebc-118">Al igual que cualquier memoria caché, una memoria caché distribuida puede mejorar considerablemente la capacidad de respuesta de una aplicación, ya que normalmente se pueden recuperar datos de la memoria caché mucho más rápida que de una base de datos relacional (o servicio web).</span><span class="sxs-lookup"><span data-stu-id="f1ebc-118">Like any cache, a distributed cache can dramatically improve an app's responsiveness, since typically data can be retrieved from the cache much faster than from a relational database (or web service).</span></span>

<span data-ttu-id="f1ebc-119">Configuración de la caché es específico de la implementación.</span><span class="sxs-lookup"><span data-stu-id="f1ebc-119">Cache configuration is implementation specific.</span></span> <span data-ttu-id="f1ebc-120">En este artículo se describe cómo configurar ambos Redis y distribuidas de SQL Server almacena en caché.</span><span class="sxs-lookup"><span data-stu-id="f1ebc-120">This article describes how to configure both Redis and SQL Server distributed caches.</span></span> <span data-ttu-id="f1ebc-121">Independientemente de qué implementación está seleccionada, la aplicación interactúa con la memoria caché utilizando una común `IDistributedCache` interfaz.</span><span class="sxs-lookup"><span data-stu-id="f1ebc-121">Regardless of which implementation is selected, the app interacts with the cache using a common `IDistributedCache` interface.</span></span>

## <a name="the-idistributedcache-interface"></a><span data-ttu-id="f1ebc-122">La interfaz de IDistributedCache</span><span class="sxs-lookup"><span data-stu-id="f1ebc-122">The IDistributedCache Interface</span></span>

<span data-ttu-id="f1ebc-123">El `IDistributedCache` interfaz incluye métodos sincrónicos y asincrónicos.</span><span class="sxs-lookup"><span data-stu-id="f1ebc-123">The `IDistributedCache` interface includes synchronous and asynchronous methods.</span></span> <span data-ttu-id="f1ebc-124">La interfaz permite elementos agregar, recuperar y quitar de la implementación de caché distribuida.</span><span class="sxs-lookup"><span data-stu-id="f1ebc-124">The interface allows items to be added, retrieved, and removed from the distributed cache implementation.</span></span> <span data-ttu-id="f1ebc-125">El `IDistributedCache` interfaz incluye los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="f1ebc-125">The `IDistributedCache` interface includes the following methods:</span></span>

<span data-ttu-id="f1ebc-126">**Get, GetAsync**</span><span class="sxs-lookup"><span data-stu-id="f1ebc-126">**Get, GetAsync**</span></span>

<span data-ttu-id="f1ebc-127">Toma una clave de cadena y recupera un elemento almacenado en caché como un `byte[]` si se encuentra en la memoria caché.</span><span class="sxs-lookup"><span data-stu-id="f1ebc-127">Takes a string key and retrieves a cached item as a `byte[]` if found in the cache.</span></span>

<span data-ttu-id="f1ebc-128">**Conjunto, SetAsync**</span><span class="sxs-lookup"><span data-stu-id="f1ebc-128">**Set, SetAsync**</span></span>

<span data-ttu-id="f1ebc-129">Agrega un elemento (como `byte[]`) a la memoria caché utilizando una clave de cadena.</span><span class="sxs-lookup"><span data-stu-id="f1ebc-129">Adds an item (as `byte[]`) to the cache using a string key.</span></span>

<span data-ttu-id="f1ebc-130">**Actualización de RefreshAsync**</span><span class="sxs-lookup"><span data-stu-id="f1ebc-130">**Refresh, RefreshAsync**</span></span>

<span data-ttu-id="f1ebc-131">Actualiza un elemento en la memoria caché basado en su clave, restablecer su tiempo de espera de expiración deslizante (si existe).</span><span class="sxs-lookup"><span data-stu-id="f1ebc-131">Refreshes an item in the cache based on its key, resetting its sliding expiration timeout (if any).</span></span>

<span data-ttu-id="f1ebc-132">**Quitar, aplica removeasync a**</span><span class="sxs-lookup"><span data-stu-id="f1ebc-132">**Remove, RemoveAsync**</span></span>

<span data-ttu-id="f1ebc-133">Quita una entrada de caché basada en su clave.</span><span class="sxs-lookup"><span data-stu-id="f1ebc-133">Removes a cache entry based on its key.</span></span>

<span data-ttu-id="f1ebc-134">Para usar el `IDistributedCache` interfaz:</span><span class="sxs-lookup"><span data-stu-id="f1ebc-134">To use the `IDistributedCache` interface:</span></span>

   1. <span data-ttu-id="f1ebc-135">Agregue los paquetes de NuGet necesarios para el archivo de proyecto.</span><span class="sxs-lookup"><span data-stu-id="f1ebc-135">Add the required NuGet packages to your project file.</span></span>

   2. <span data-ttu-id="f1ebc-136">Configurar la implementación específica de `IDistributedCache` en su `Startup` la clase `ConfigureServices` método y lo agrega al contenedor no existe.</span><span class="sxs-lookup"><span data-stu-id="f1ebc-136">Configure the specific implementation of `IDistributedCache` in your `Startup` class's `ConfigureServices` method, and add it to the container there.</span></span>

   3. <span data-ttu-id="f1ebc-137">Desde la aplicación [`Middleware](../../fundamentals/middleware.md) or MVC controller classes, request an instance of `IDistributedCache' desde el constructor.</span><span class="sxs-lookup"><span data-stu-id="f1ebc-137">From the app's [`Middleware](../../fundamentals/middleware.md) or MVC controller classes, request an instance of `IDistributedCache\` from the constructor.</span></span> <span data-ttu-id="f1ebc-138">La instancia se proporcionarán con [inyección de dependencia](../../fundamentals/dependency-injection.md) (DI).</span><span class="sxs-lookup"><span data-stu-id="f1ebc-138">The instance will be provided by [Dependency Injection](../../fundamentals/dependency-injection.md) (DI).</span></span>

> [!NOTE]
> <span data-ttu-id="f1ebc-139">No es necesario usar una duración de Singleton o en el ámbito para `IDistributedCache` instancias (al menos para las implementaciones integradas).</span><span class="sxs-lookup"><span data-stu-id="f1ebc-139">There is no need to use a Singleton or Scoped lifetime for `IDistributedCache` instances (at least for the built-in implementations).</span></span> <span data-ttu-id="f1ebc-140">También puede crear una instancia donde podría necesitar uno (en lugar de usar [inyección de dependencia](../../fundamentals/dependency-injection.md)), pero esto puede dificultar el código probar e infringe el [principio de dependencias explícitas](http://deviq.com/explicit-dependencies-principle/).</span><span class="sxs-lookup"><span data-stu-id="f1ebc-140">You can also create an instance wherever you might need one (instead of using [Dependency Injection](../../fundamentals/dependency-injection.md)), but this can make your code harder to test, and violates the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/).</span></span>

<span data-ttu-id="f1ebc-141">En el ejemplo siguiente se muestra cómo utilizar una instancia de `IDistributedCache` en un componente de middleware simple:</span><span class="sxs-lookup"><span data-stu-id="f1ebc-141">The following example shows how to use an instance of `IDistributedCache` in a simple middleware component:</span></span>

<span data-ttu-id="f1ebc-142">[!code-csharp[Main](./distributed/sample/src/DistCacheSample/StartTimeHeader.cs?highlight=15,18,21,27,28,29,30,31)]</span><span class="sxs-lookup"><span data-stu-id="f1ebc-142">[!code-csharp[Main](./distributed/sample/src/DistCacheSample/StartTimeHeader.cs?highlight=15,18,21,27,28,29,30,31)]</span></span>

<span data-ttu-id="f1ebc-143">En el código anterior, el valor almacenado en caché se lee pero nunca se ha escrito.</span><span class="sxs-lookup"><span data-stu-id="f1ebc-143">In the code above, the cached value is read, but never written.</span></span> <span data-ttu-id="f1ebc-144">En este ejemplo, el valor sólo se establece cuando se inicia un servidor y no cambia.</span><span class="sxs-lookup"><span data-stu-id="f1ebc-144">In this sample, the value is only set when a server starts up, and doesn't change.</span></span> <span data-ttu-id="f1ebc-145">En un escenario de varios servidores, el servidor más reciente para iniciar sobrescribirá los valores anteriores que se establecieron con otros servidores.</span><span class="sxs-lookup"><span data-stu-id="f1ebc-145">In a multi-server scenario, the most recent server to start will overwrite any previous values that were set by other servers.</span></span> <span data-ttu-id="f1ebc-146">El `Get` y `Set` métodos utilizan el `byte[]` tipo.</span><span class="sxs-lookup"><span data-stu-id="f1ebc-146">The `Get` and `Set` methods use the `byte[]` type.</span></span> <span data-ttu-id="f1ebc-147">Por lo tanto, se debe convertir el valor de cadena mediante `Encoding.UTF8.GetString` (para `Get`) y `Encoding.UTF8.GetBytes` (para `Set`).</span><span class="sxs-lookup"><span data-stu-id="f1ebc-147">Therefore, the string value must be converted using `Encoding.UTF8.GetString` (for `Get`) and `Encoding.UTF8.GetBytes` (for `Set`).</span></span>

<span data-ttu-id="f1ebc-148">El siguiente código *Startup.cs* muestra el valor que se va a establecer:</span><span class="sxs-lookup"><span data-stu-id="f1ebc-148">The following code from *Startup.cs* shows the value being set:</span></span>

<span data-ttu-id="f1ebc-149">[!code-csharp[Main](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=2,4,5,6&range=58-66)]</span><span class="sxs-lookup"><span data-stu-id="f1ebc-149">[!code-csharp[Main](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=2,4,5,6&range=58-66)]</span></span>

> [!NOTE]
> <span data-ttu-id="f1ebc-150">Puesto que `IDistributedCache` está configurado en el `ConfigureServices` método, está disponible para el `Configure` método como parámetro.</span><span class="sxs-lookup"><span data-stu-id="f1ebc-150">Since `IDistributedCache` is configured in the `ConfigureServices` method, it is available to the `Configure` method as a parameter.</span></span> <span data-ttu-id="f1ebc-151">Agregar como un parámetro permitirá la instancia configurada proporcionar a través de DI.</span><span class="sxs-lookup"><span data-stu-id="f1ebc-151">Adding it as a parameter will allow the configured instance to be provided through DI.</span></span>

## <a name="using-a-redis-distributed-cache"></a><span data-ttu-id="f1ebc-152">Con un Redis caché distribuida</span><span class="sxs-lookup"><span data-stu-id="f1ebc-152">Using a Redis Distributed Cache</span></span>

<span data-ttu-id="f1ebc-153">[Redis](http://redis.io) es un almacén de datos en memoria de código abierto, que a menudo se usa como una memoria caché distribuida.</span><span class="sxs-lookup"><span data-stu-id="f1ebc-153">[Redis](http://redis.io) is an open source in-memory data store, which is often used as a distributed cache.</span></span> <span data-ttu-id="f1ebc-154">Puede usar de forma local, y puede configurar un [Azure Redis Cache](https://azure.microsoft.com/services/cache/) para las aplicaciones principales de ASP.NET hospedados en Azure.</span><span class="sxs-lookup"><span data-stu-id="f1ebc-154">You can use it locally, and you can configure an [Azure Redis Cache](https://azure.microsoft.com/services/cache/) for your Azure-hosted ASP.NET Core apps.</span></span> <span data-ttu-id="f1ebc-155">La aplicación de ASP.NET Core configura la implementación de caché mediante un `RedisDistributedCache` instancia.</span><span class="sxs-lookup"><span data-stu-id="f1ebc-155">Your ASP.NET Core app configures the cache implementation using a `RedisDistributedCache` instance.</span></span>

<span data-ttu-id="f1ebc-156">Configurar la implementación de Redis en `ConfigureServices` y tener acceso a él en el código de aplicación solicitando una instancia de `IDistributedCache` (vea el código anterior).</span><span class="sxs-lookup"><span data-stu-id="f1ebc-156">You configure the Redis implementation in `ConfigureServices` and access it in your app code by requesting an instance of `IDistributedCache` (see the code above).</span></span>

<span data-ttu-id="f1ebc-157">En el código de ejemplo, un `RedisCache` implementación se utiliza cuando el servidor está configurado para un `Staging` entorno.</span><span class="sxs-lookup"><span data-stu-id="f1ebc-157">In the sample code, a `RedisCache` implementation is used when the server is configured for a `Staging` environment.</span></span> <span data-ttu-id="f1ebc-158">Por lo tanto la `ConfigureStagingServices` método configura el `RedisCache`:</span><span class="sxs-lookup"><span data-stu-id="f1ebc-158">Thus the `ConfigureStagingServices` method configures the `RedisCache`:</span></span>

<span data-ttu-id="f1ebc-159">[!code-csharp[Main](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=8,9,10,11,12,13&range=27-40)]</span><span class="sxs-lookup"><span data-stu-id="f1ebc-159">[!code-csharp[Main](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=8,9,10,11,12,13&range=27-40)]</span></span>

> [!NOTE]
> <span data-ttu-id="f1ebc-160">Para instalar Redis en el equipo local, instale el paquete chocolatey [http://chocolatey.org/packages/redis-64/](http://chocolatey.org/packages/redis-64/) y ejecutar `redis-server` desde un símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="f1ebc-160">To install Redis on your local machine, install the chocolatey package [http://chocolatey.org/packages/redis-64/](http://chocolatey.org/packages/redis-64/) and run `redis-server` from a command prompt.</span></span>

## <a name="using-a-sql-server-distributed-cache"></a><span data-ttu-id="f1ebc-161">Uso de un servidor SQL de caché distribuida</span><span class="sxs-lookup"><span data-stu-id="f1ebc-161">Using a SQL Server Distributed Cache</span></span>

<span data-ttu-id="f1ebc-162">La implementación de SqlServerCache permite que la memoria caché distribuida usar una base de datos de SQL Server como su memoria auxiliar.</span><span class="sxs-lookup"><span data-stu-id="f1ebc-162">The SqlServerCache implementation allows the distributed cache to use a SQL Server database as its backing store.</span></span> <span data-ttu-id="f1ebc-163">Para crear SQL Server se puede utilizar la herramienta de caché de sql, la herramienta crea una tabla con el nombre y el esquema que especifica.</span><span class="sxs-lookup"><span data-stu-id="f1ebc-163">To create SQL Server table you can use sql-cache tool, the tool creates a table with the name and schema you specify.</span></span>

<span data-ttu-id="f1ebc-164">Para usar la herramienta de caché de sql, agregue `SqlConfig.Tools` a la `<ItemGroup>` elemento de la *.csproj* archivo y ejecutar la restauración de dotnet.</span><span class="sxs-lookup"><span data-stu-id="f1ebc-164">To use the sql-cache tool, add `SqlConfig.Tools` to the `<ItemGroup>` element of the *.csproj* file and run dotnet restore.</span></span>

<span data-ttu-id="f1ebc-165">[!code-csharp[Main](./distributed/sample/src/DistCacheSample/DistCacheSample.csproj?range=23-25)]</span><span class="sxs-lookup"><span data-stu-id="f1ebc-165">[!code-csharp[Main](./distributed/sample/src/DistCacheSample/DistCacheSample.csproj?range=23-25)]</span></span>

<span data-ttu-id="f1ebc-166">Ejecute el siguiente comando para comprobar SqlConfig.Tools</span><span class="sxs-lookup"><span data-stu-id="f1ebc-166">Test SqlConfig.Tools by running the following command</span></span>

```none
C:\DistCacheSample\src\DistCacheSample>dotnet sql-cache create --help
   ```

<span data-ttu-id="f1ebc-167">herramienta de caché de SQL muestra ayuda sobre uso, opciones y comandos, ahora puede crear tablas en sql server, ejecuta el comando "creación de caché de sql":</span><span class="sxs-lookup"><span data-stu-id="f1ebc-167">sql-cache tool  will display usage, options and command help, now you can create tables into sql server, running "sql-cache create" command :</span></span>

```none
C:\DistCacheSample\src\DistCacheSample>dotnet sql-cache create "Data Source=(localdb)\v11.0;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
   info: Microsoft.Extensions.Caching.SqlConfig.Tools.Program[0]
       Table and index were created successfully.
   ```

<span data-ttu-id="f1ebc-168">La tabla creada tiene el siguiente esquema:</span><span class="sxs-lookup"><span data-stu-id="f1ebc-168">The created table has the following schema:</span></span>

![Tabla de la caché de SQL Server](distributed/_static/SqlServerCacheTable.png)

<span data-ttu-id="f1ebc-170">Al igual que todas las implementaciones de caché, la aplicación debe obtener y establecer valores de caché mediante una instancia de `IDistributedCache`, no un `SqlServerCache`.</span><span class="sxs-lookup"><span data-stu-id="f1ebc-170">Like all cache implementations, your app should get and set cache values using an instance of `IDistributedCache`, not a `SqlServerCache`.</span></span> <span data-ttu-id="f1ebc-171">El ejemplo implementa `SqlServerCache` en el `Production` entorno (por lo que se configura en `ConfigureProductionServices`).</span><span class="sxs-lookup"><span data-stu-id="f1ebc-171">The sample implements `SqlServerCache` in the `Production` environment (so it is configured in `ConfigureProductionServices`).</span></span>

<span data-ttu-id="f1ebc-172">[!code-csharp[Main](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=7,8,9,10,11,12&range=42-56)]</span><span class="sxs-lookup"><span data-stu-id="f1ebc-172">[!code-csharp[Main](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=7,8,9,10,11,12&range=42-56)]</span></span>

> [!NOTE]
> <span data-ttu-id="f1ebc-173">El `ConnectionString` (y, opcionalmente, `SchemaName` y `TableName`) normalmente deberían almacenarse fuera de control de código fuente (por ejemplo, UserSecrets), ya que pueden contener las credenciales.</span><span class="sxs-lookup"><span data-stu-id="f1ebc-173">The `ConnectionString` (and optionally, `SchemaName` and `TableName`) should typically be stored outside of source control (such as UserSecrets), as they may contain credentials.</span></span>

## <a name="recommendations"></a><span data-ttu-id="f1ebc-174">Recomendaciones</span><span class="sxs-lookup"><span data-stu-id="f1ebc-174">Recommendations</span></span>

<span data-ttu-id="f1ebc-175">La hora de decidir qué implementación de `IDistributedCache` es adecuado para la aplicación, elija entre Redis y SQL Server se basa en la infraestructura existente y entorno, los requisitos de rendimiento y experiencia de su equipo.</span><span class="sxs-lookup"><span data-stu-id="f1ebc-175">When deciding which implementation of `IDistributedCache` is right for your app, choose between Redis and SQL Server based on your existing infrastructure and environment, your performance requirements, and your team's experience.</span></span> <span data-ttu-id="f1ebc-176">Si su equipo es más fácil trabajar con Redis, es una excelente opción.</span><span class="sxs-lookup"><span data-stu-id="f1ebc-176">If your team is more comfortable working with Redis, it's an excellent choice.</span></span> <span data-ttu-id="f1ebc-177">Si el equipo prefiera SQL Server, puede estar seguro de que así la implementación.</span><span class="sxs-lookup"><span data-stu-id="f1ebc-177">If your team prefers SQL Server, you can be confident in that implementation as well.</span></span> <span data-ttu-id="f1ebc-178">Tenga en cuenta que una solución de almacenamiento en caché tradicional almacena datos en memoria que permite la recuperación rápida de datos.</span><span class="sxs-lookup"><span data-stu-id="f1ebc-178">Note that A traditional caching solution stores data in-memory which allows for fast retrieval of data.</span></span> <span data-ttu-id="f1ebc-179">Debe almacenar los datos de uso frecuente en una memoria caché y almacenar todos los datos en un almacén persistente de back-end como SQL Server o el almacenamiento de Azure.</span><span class="sxs-lookup"><span data-stu-id="f1ebc-179">You should store commonly used data in a cache and store the entire data in a backend persistent store such as SQL Server or Azure Storage.</span></span> <span data-ttu-id="f1ebc-180">Caché en Redis es una solución de almacenamiento en caché que proporciona un alto rendimiento y baja latencia en comparación con la memoria caché de SQL.</span><span class="sxs-lookup"><span data-stu-id="f1ebc-180">Redis Cache is a caching solution which gives you high throughput and low latency as compared to SQL Cache.</span></span>

<span data-ttu-id="f1ebc-181">Recursos adicionales:</span><span class="sxs-lookup"><span data-stu-id="f1ebc-181">Additional resources:</span></span>

* [<span data-ttu-id="f1ebc-182">En la memoria caché</span><span class="sxs-lookup"><span data-stu-id="f1ebc-182">In memory caching</span></span>](memory.md)
* [<span data-ttu-id="f1ebc-183">Caché en Azure en Redis</span><span class="sxs-lookup"><span data-stu-id="f1ebc-183">Redis Cache on Azure</span></span>](https://azure.microsoft.com/documentation/services/redis-cache/)
* [<span data-ttu-id="f1ebc-184">Base de datos SQL en Azure</span><span class="sxs-lookup"><span data-stu-id="f1ebc-184">SQL Database on Azure</span></span>](https://azure.microsoft.com/documentation/services/sql-database/)
