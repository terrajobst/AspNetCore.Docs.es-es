---
title: Trabajar con una memoria caché distribuida en ASP.NET Core
author: ardalis
description: Aprenda a usar ASP.NET Core distribuido de almacenamiento en caché para mejorar el rendimiento de la aplicación y la escalabilidad, especialmente en un entorno de granja de servidores en la nube o servidor.
ms.author: riande
ms.custom: mvc
ms.date: 02/14/2017
uid: performance/caching/distributed
ms.openlocfilehash: 9c41a6e008045231bd2e1c1f53a9161e11daafa9
ms.sourcegitcommit: cb0c27fa0184f954fce591d417e6ab2a51d8bb22
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39123845"
---
# <a name="work-with-a-distributed-cache-in-aspnet-core"></a><span data-ttu-id="2de7b-103">Trabajar con una memoria caché distribuida en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2de7b-103">Work with a distributed cache in ASP.NET Core</span></span>

<span data-ttu-id="2de7b-104">Por [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="2de7b-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="2de7b-105">Las memorias caché distribuidas pueden mejorar el rendimiento y escalabilidad de las aplicaciones ASP.NET Core, especialmente cuando se hospeda en la nube o una granja de servidores.</span><span class="sxs-lookup"><span data-stu-id="2de7b-105">Distributed caches can improve the performance and scalability of ASP.NET Core apps, especially when hosted in the cloud or a server farm.</span></span>

<span data-ttu-id="2de7b-106">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2de7b-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-a-distributed-cache"></a><span data-ttu-id="2de7b-107">¿Qué es una memoria caché distribuida</span><span class="sxs-lookup"><span data-stu-id="2de7b-107">What is a distributed cache</span></span>

<span data-ttu-id="2de7b-108">Una memoria caché distribuida es compartida por varios servidores de aplicación (consulte [conceptos básicos de la memoria caché](memory.md#caching-basics)).</span><span class="sxs-lookup"><span data-stu-id="2de7b-108">A distributed cache is shared by multiple app servers (see [Cache Basics](memory.md#caching-basics)).</span></span> <span data-ttu-id="2de7b-109">La información de la memoria caché no se guarda en la memoria de los servidores web individuales y los datos almacenados en caché están disponibles para todos los servidores de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="2de7b-109">The information in the cache isn't stored in the memory of individual web servers, and the cached data is available to all of the app's servers.</span></span> <span data-ttu-id="2de7b-110">Esto proporciona varias ventajas:</span><span class="sxs-lookup"><span data-stu-id="2de7b-110">This provides several advantages:</span></span>

1. <span data-ttu-id="2de7b-111">Los datos almacenados en caché son coherentes en todos los servidores web.</span><span class="sxs-lookup"><span data-stu-id="2de7b-111">Cached data is coherent on all web servers.</span></span> <span data-ttu-id="2de7b-112">Los usuarios no verán resultados distintos sin importar el servidor web que controla su solicitud.</span><span class="sxs-lookup"><span data-stu-id="2de7b-112">Users don't see different results depending on which web server handles their request</span></span>

2. <span data-ttu-id="2de7b-113">Los datos almacenados en caché sobreviven a reinicios del servidor web y las implementaciones.</span><span class="sxs-lookup"><span data-stu-id="2de7b-113">Cached data survives web server restarts and deployments.</span></span> <span data-ttu-id="2de7b-114">Servidores web individuales se pueden quitar o agregar sin afectar a la memoria caché.</span><span class="sxs-lookup"><span data-stu-id="2de7b-114">Individual web servers can be removed or added without impacting the cache.</span></span>

3. <span data-ttu-id="2de7b-115">El almacén de datos de origen tiene menos de las solicitudes realizadas a él (que con varias cachés en memoria o no almacenar en caché en absoluto).</span><span class="sxs-lookup"><span data-stu-id="2de7b-115">The source data store has fewer requests made to it (than with multiple in-memory caches or no cache at all).</span></span>

> [!NOTE]
> <span data-ttu-id="2de7b-116">Si usa una memoria caché distribuida de SQL Server, algunas de las siguientes ventajas solo son ciertas si se utiliza una instancia de base de datos independiente para la memoria caché que para los datos de origen de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2de7b-116">If using a SQL Server Distributed Cache, some of these advantages are only true if a separate database instance is used for the cache than for the app's source data.</span></span>

<span data-ttu-id="2de7b-117">Al igual que cualquier caché, una caché distribuida puede mejorar considerablemente la capacidad de respuesta de una aplicación, ya que normalmente se pueden recuperar datos de la memoria caché mucho más rápida que de una base de datos relacional (o servicio web).</span><span class="sxs-lookup"><span data-stu-id="2de7b-117">Like any cache, a distributed cache can dramatically improve an app's responsiveness, since typically data can be retrieved from the cache much faster than from a relational database (or web service).</span></span>

<span data-ttu-id="2de7b-118">La configuración de la caché es específica de la implementación.</span><span class="sxs-lookup"><span data-stu-id="2de7b-118">Cache configuration is implementation specific.</span></span> <span data-ttu-id="2de7b-119">En este artículo se describe cómo configurar los almacenes distribuidos en caché Redis y SQL Server.</span><span class="sxs-lookup"><span data-stu-id="2de7b-119">This article describes how to configure both Redis and SQL Server distributed caches.</span></span> <span data-ttu-id="2de7b-120">Independientemente de qué implementación se seleccione, la aplicación interactúa con la memoria caché utilizando una interfaz común `IDistributedCache`.</span><span class="sxs-lookup"><span data-stu-id="2de7b-120">Regardless of which implementation is selected, the app interacts with the cache using a common `IDistributedCache` interface.</span></span>

## <a name="the-idistributedcache-interface"></a><span data-ttu-id="2de7b-121">La interfaz de IDistributedCache</span><span class="sxs-lookup"><span data-stu-id="2de7b-121">The IDistributedCache Interface</span></span>

<span data-ttu-id="2de7b-122">El `IDistributedCache` interfaz incluye métodos sincrónicos y asincrónicos.</span><span class="sxs-lookup"><span data-stu-id="2de7b-122">The `IDistributedCache` interface includes synchronous and asynchronous methods.</span></span> <span data-ttu-id="2de7b-123">La interfaz permite a los elementos se agregan, recuperar y quita de la implementación de caché distribuida.</span><span class="sxs-lookup"><span data-stu-id="2de7b-123">The interface allows items to be added, retrieved, and removed from the distributed cache implementation.</span></span> <span data-ttu-id="2de7b-124">El `IDistributedCache` interfaz incluye los siguientes métodos:</span><span class="sxs-lookup"><span data-stu-id="2de7b-124">The `IDistributedCache` interface includes the following methods:</span></span>

<span data-ttu-id="2de7b-125">**Get, GetAsync**</span><span class="sxs-lookup"><span data-stu-id="2de7b-125">**Get, GetAsync**</span></span>

<span data-ttu-id="2de7b-126">Toma una clave de cadena y recupera un elemento almacenado en caché como un `byte[]` si se encuentra en la memoria caché.</span><span class="sxs-lookup"><span data-stu-id="2de7b-126">Takes a string key and retrieves a cached item as a `byte[]` if found in the cache.</span></span>

<span data-ttu-id="2de7b-127">**Conjunto de SetAsync**</span><span class="sxs-lookup"><span data-stu-id="2de7b-127">**Set, SetAsync**</span></span>

<span data-ttu-id="2de7b-128">Agrega un elemento (como `byte[]`) a la memoria caché utilizando una clave de cadena.</span><span class="sxs-lookup"><span data-stu-id="2de7b-128">Adds an item (as `byte[]`) to the cache using a string key.</span></span>

<span data-ttu-id="2de7b-129">**Actualización, RefreshAsync**</span><span class="sxs-lookup"><span data-stu-id="2de7b-129">**Refresh, RefreshAsync**</span></span>

<span data-ttu-id="2de7b-130">Actualiza un elemento en la memoria caché en función de su clave, restablecer el tiempo de espera de expiración deslizante (si existe).</span><span class="sxs-lookup"><span data-stu-id="2de7b-130">Refreshes an item in the cache based on its key, resetting its sliding expiration timeout (if any).</span></span>

<span data-ttu-id="2de7b-131">**Quitar, aplica removeasync a**</span><span class="sxs-lookup"><span data-stu-id="2de7b-131">**Remove, RemoveAsync**</span></span>

<span data-ttu-id="2de7b-132">Quita una entrada de caché en función de su clave.</span><span class="sxs-lookup"><span data-stu-id="2de7b-132">Removes a cache entry based on its key.</span></span>

<span data-ttu-id="2de7b-133">Para usar el `IDistributedCache` interfaz:</span><span class="sxs-lookup"><span data-stu-id="2de7b-133">To use the `IDistributedCache` interface:</span></span>

   1. <span data-ttu-id="2de7b-134">Agregue los paquetes de NuGet necesarios para el archivo de proyecto.</span><span class="sxs-lookup"><span data-stu-id="2de7b-134">Add the required NuGet packages to your project file.</span></span>

   2. <span data-ttu-id="2de7b-135">Configurar la implementación específica de `IDistributedCache` en su `Startup` la clase `ConfigureServices` método y agregarlo al contenedor no existe.</span><span class="sxs-lookup"><span data-stu-id="2de7b-135">Configure the specific implementation of `IDistributedCache` in your `Startup` class's `ConfigureServices` method, and add it to the container there.</span></span>

   3. <span data-ttu-id="2de7b-136">Desde la aplicación [Middleware](xref:fundamentals/middleware/index) o las clases de controlador MVC, solicitar una instancia de `IDistributedCache` desde el constructor.</span><span class="sxs-lookup"><span data-stu-id="2de7b-136">From the app's [Middleware](xref:fundamentals/middleware/index) or MVC controller classes, request an instance of `IDistributedCache` from the constructor.</span></span> <span data-ttu-id="2de7b-137">La instancia se proporcionarán con [inserción de dependencias](../../fundamentals/dependency-injection.md) (DI).</span><span class="sxs-lookup"><span data-stu-id="2de7b-137">The instance will be provided by [Dependency Injection](../../fundamentals/dependency-injection.md) (DI).</span></span>

> [!NOTE]
> <span data-ttu-id="2de7b-138">No hay ninguna necesidad de usar una duración Singleton o en el ámbito para `IDistributedCache` instancias (al menos para las implementaciones integradas).</span><span class="sxs-lookup"><span data-stu-id="2de7b-138">There's no need to use a Singleton or Scoped lifetime for `IDistributedCache` instances (at least for the built-in implementations).</span></span> <span data-ttu-id="2de7b-139">También puede crear una instancia siempre es posible que necesite uno (en lugar de usar [inserción de dependencias](../../fundamentals/dependency-injection.md)), pero esto puede hacer que el código sea más difícil de probar y se infringe la [dependencias explicitas](http://deviq.com/explicit-dependencies-principle/).</span><span class="sxs-lookup"><span data-stu-id="2de7b-139">You can also create an instance wherever you might need one (instead of using [Dependency Injection](../../fundamentals/dependency-injection.md)), but this can make your code harder to test, and violates the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/).</span></span>

<span data-ttu-id="2de7b-140">El ejemplo siguiente muestra cómo utilizar una instancia de `IDistributedCache` en un componente de middleware simple:</span><span class="sxs-lookup"><span data-stu-id="2de7b-140">The following example shows how to use an instance of `IDistributedCache` in a simple middleware component:</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/StartTimeHeader.cs)]

<span data-ttu-id="2de7b-141">En el código anterior, el valor almacenado en caché es leer, pero nunca se ha escrito.</span><span class="sxs-lookup"><span data-stu-id="2de7b-141">In the code above, the cached value is read, but never written.</span></span> <span data-ttu-id="2de7b-142">En este ejemplo, el valor se establece solo cuando se inicia un servidor y no cambia.</span><span class="sxs-lookup"><span data-stu-id="2de7b-142">In this sample, the value is only set when a server starts up, and doesn't change.</span></span> <span data-ttu-id="2de7b-143">En un escenario de varios servidor, el servidor más reciente para iniciar sobrescribirá los valores anteriores que se establecieron con otros servidores.</span><span class="sxs-lookup"><span data-stu-id="2de7b-143">In a multi-server scenario, the most recent server to start will overwrite any previous values that were set by other servers.</span></span> <span data-ttu-id="2de7b-144">El `Get` y `Set` métodos usan el `byte[]` tipo.</span><span class="sxs-lookup"><span data-stu-id="2de7b-144">The `Get` and `Set` methods use the `byte[]` type.</span></span> <span data-ttu-id="2de7b-145">Por lo tanto, se debe convertir el valor de cadena mediante `Encoding.UTF8.GetString` (para `Get`) y `Encoding.UTF8.GetBytes` (para `Set`).</span><span class="sxs-lookup"><span data-stu-id="2de7b-145">Therefore, the string value must be converted using `Encoding.UTF8.GetString` (for `Get`) and `Encoding.UTF8.GetBytes` (for `Set`).</span></span>

<span data-ttu-id="2de7b-146">El siguiente código de *Startup.cs* muestra el valor que se va a establecer:</span><span class="sxs-lookup"><span data-stu-id="2de7b-146">The following code from *Startup.cs* shows the value being set:</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet1)]

<span data-ttu-id="2de7b-147">Puesto que `IDistributedCache` está configurado en el `ConfigureServices` método, está disponible para el `Configure` método como parámetro.</span><span class="sxs-lookup"><span data-stu-id="2de7b-147">Since `IDistributedCache` is configured in the `ConfigureServices` method, it's available to the `Configure` method as a parameter.</span></span> <span data-ttu-id="2de7b-148">Agregado como un parámetro permitirá proporcionar la instancia configurada a través de DI.</span><span class="sxs-lookup"><span data-stu-id="2de7b-148">Adding it as a parameter will allow the configured instance to be provided through DI.</span></span>

## <a name="using-a-redis-distributed-cache"></a><span data-ttu-id="2de7b-149">Uso de una caché en Redis distribuida</span><span class="sxs-lookup"><span data-stu-id="2de7b-149">Using a Redis distributed cache</span></span>

<span data-ttu-id="2de7b-150">[Redis](https://redis.io/) es un almacén de datos en memoria de código abierto, que a menudo se usa como una memoria caché distribuida.</span><span class="sxs-lookup"><span data-stu-id="2de7b-150">[Redis](https://redis.io/) is an open source in-memory data store, which is often used as a distributed cache.</span></span> <span data-ttu-id="2de7b-151">Puede usarlo de forma local, y puede configurar un [Azure Redis Cache](https://azure.microsoft.com/services/cache/) para las aplicaciones de ASP.NET Core hospedadas en Azure.</span><span class="sxs-lookup"><span data-stu-id="2de7b-151">You can use it locally, and you can configure an [Azure Redis Cache](https://azure.microsoft.com/services/cache/) for your Azure-hosted ASP.NET Core apps.</span></span> <span data-ttu-id="2de7b-152">La aplicación ASP.NET Core configura la implementación de caché mediante un `RedisDistributedCache` instancia.</span><span class="sxs-lookup"><span data-stu-id="2de7b-152">Your ASP.NET Core app configures the cache implementation using a `RedisDistributedCache` instance.</span></span>

<span data-ttu-id="2de7b-153">La caché en Redis requiere [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis/)</span><span class="sxs-lookup"><span data-stu-id="2de7b-153">The Redis cache requires [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis/)</span></span>

<span data-ttu-id="2de7b-154">Configurar la implementación de Redis en `ConfigureServices` y obtener acceso a él en el código de aplicación mediante la solicitud de una instancia de `IDistributedCache` (vea el código anterior).</span><span class="sxs-lookup"><span data-stu-id="2de7b-154">You configure the Redis implementation in `ConfigureServices` and access it in your app code by requesting an instance of `IDistributedCache` (see the code above).</span></span>

<span data-ttu-id="2de7b-155">En el código de ejemplo, un `RedisCache` implementación se utiliza cuando el servidor está configurado para un `Staging` entorno.</span><span class="sxs-lookup"><span data-stu-id="2de7b-155">In the sample code, a `RedisCache` implementation is used when the server is configured for a `Staging` environment.</span></span> <span data-ttu-id="2de7b-156">Por lo tanto el `ConfigureStagingServices` método configura el `RedisCache`:</span><span class="sxs-lookup"><span data-stu-id="2de7b-156">Thus the `ConfigureStagingServices` method configures the `RedisCache`:</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet2)]

<span data-ttu-id="2de7b-157">Para instalar Redis en el equipo local, instale el paquete de chocolatey [ https://chocolatey.org/packages/redis-64/ ](https://chocolatey.org/packages/redis-64/) y ejecute `redis-server` desde un símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="2de7b-157">To install Redis on your local machine, install the chocolatey package [https://chocolatey.org/packages/redis-64/](https://chocolatey.org/packages/redis-64/) and run `redis-server` from a command prompt.</span></span>

## <a name="using-a-sql-server-distributed-cache"></a><span data-ttu-id="2de7b-158">Uso de un servidor SQL de caché distribuida</span><span class="sxs-lookup"><span data-stu-id="2de7b-158">Using a SQL Server distributed cache</span></span>

<span data-ttu-id="2de7b-159">La implementación de SqlServerCache permite que la memoria caché distribuida usar una base de datos de SQL Server como almacén de respaldo.</span><span class="sxs-lookup"><span data-stu-id="2de7b-159">The SqlServerCache implementation allows the distributed cache to use a SQL Server database as its backing store.</span></span> <span data-ttu-id="2de7b-160">Para crear SQL Server se puede utilizar la herramienta de caché de sql, la herramienta crea una tabla con el nombre y el esquema que especifique.</span><span class="sxs-lookup"><span data-stu-id="2de7b-160">To create SQL Server table you can use sql-cache tool, the tool creates a table with the name and schema you specify.</span></span>

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="2de7b-161">Agregar `SqlConfig.Tools` a la `<ItemGroup>` elemento del archivo de proyecto y ejecute `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="2de7b-161">Add `SqlConfig.Tools` to the `<ItemGroup>` element of the project file and run `dotnet restore`.</span></span>

```xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.Extensions.Caching.SqlConfig.Tools" 
                          Version="2.0.2" />
</ItemGroup>
```

::: moniker-end

<span data-ttu-id="2de7b-162">Probar SqlConfig.Tools, ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="2de7b-162">Test SqlConfig.Tools by running the following command:</span></span>

```console
dotnet sql-cache create --help
```

<span data-ttu-id="2de7b-163">SqlConfig.Tools muestra el uso, opciones y ayuda del comando.</span><span class="sxs-lookup"><span data-stu-id="2de7b-163">SqlConfig.Tools displays usage, options, and command help.</span></span>

<span data-ttu-id="2de7b-164">Crear una tabla en SQL Server ejecutando el `sql-cache create` comando:</span><span class="sxs-lookup"><span data-stu-id="2de7b-164">Create a table in SQL Server by running the `sql-cache create` command :</span></span>

```console
dotnet sql-cache create "Data Source=(localdb)\v11.0;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
info: Microsoft.Extensions.Caching.SqlConfig.Tools.Program[0]
Table and index were created successfully.
```

<span data-ttu-id="2de7b-165">La tabla tiene el siguiente esquema:</span><span class="sxs-lookup"><span data-stu-id="2de7b-165">The created table has the following schema:</span></span>

![Tabla de la caché de SQL Server](distributed/_static/SqlServerCacheTable.png)

<span data-ttu-id="2de7b-167">Al igual que todas las implementaciones de caché, debe obtener y establecer los valores de la memoria caché utilizando una instancia de la aplicación `IDistributedCache`, no un `SqlServerCache`.</span><span class="sxs-lookup"><span data-stu-id="2de7b-167">Like all cache implementations, your app should get and set cache values using an instance of `IDistributedCache`, not a `SqlServerCache`.</span></span> <span data-ttu-id="2de7b-168">El ejemplo implementa `SqlServerCache` en el entorno de producción (por lo que está configurado en `ConfigureProductionServices`).</span><span class="sxs-lookup"><span data-stu-id="2de7b-168">The sample implements `SqlServerCache` in the Production environment (so it's configured in `ConfigureProductionServices`).</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet3)]

> [!NOTE]
> <span data-ttu-id="2de7b-169">El `ConnectionString` (y, opcionalmente, `SchemaName` y `TableName`) normalmente deben almacenarse fuera de control de código fuente (por ejemplo, UserSecrets), ya que pueden contener las credenciales.</span><span class="sxs-lookup"><span data-stu-id="2de7b-169">The `ConnectionString` (and optionally, `SchemaName` and `TableName`) should typically be stored outside of source control (such as UserSecrets), as they may contain credentials.</span></span>

## <a name="recommendations"></a><span data-ttu-id="2de7b-170">Recomendaciones</span><span class="sxs-lookup"><span data-stu-id="2de7b-170">Recommendations</span></span>

<span data-ttu-id="2de7b-171">La hora de decidir qué implementación de `IDistributedCache` es adecuado para su aplicación, opte por Redis y SQL Server se basa en la infraestructura existente y entorno, los requisitos de rendimiento y experiencia de su equipo.</span><span class="sxs-lookup"><span data-stu-id="2de7b-171">When deciding which implementation of `IDistributedCache` is right for your app, choose between Redis and SQL Server based on your existing infrastructure and environment, your performance requirements, and your team's experience.</span></span> <span data-ttu-id="2de7b-172">Si su equipo es más fácil trabajar con Redis, es una excelente opción.</span><span class="sxs-lookup"><span data-stu-id="2de7b-172">If your team is more comfortable working with Redis, it's an excellent choice.</span></span> <span data-ttu-id="2de7b-173">Si el equipo prefiera de SQL Server, puede confiar en que así la implementación.</span><span class="sxs-lookup"><span data-stu-id="2de7b-173">If your team prefers SQL Server, you can be confident in that implementation as well.</span></span> <span data-ttu-id="2de7b-174">Tenga en cuenta que una solución de almacenamiento en caché tradicional almacena datos en memoria que permite la recuperación rápida de datos.</span><span class="sxs-lookup"><span data-stu-id="2de7b-174">Note that a traditional caching solution stores data in-memory which allows for fast retrieval of data.</span></span> <span data-ttu-id="2de7b-175">Debe almacenar los datos usados en una memoria caché y almacenar todos los datos en un almacén persistente de back-end, como SQL Server o Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="2de7b-175">You should store commonly used data in a cache and store the entire data in a backend persistent store such as SQL Server or Azure Storage.</span></span> <span data-ttu-id="2de7b-176">Caché en Redis es una solución de almacenamiento en caché que ofrece alto rendimiento y baja latencia en comparación con la memoria caché de SQL.</span><span class="sxs-lookup"><span data-stu-id="2de7b-176">Redis Cache is a caching solution which gives you high throughput and low latency as compared to SQL Cache.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2de7b-177">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="2de7b-177">Additional resources</span></span>

* [<span data-ttu-id="2de7b-178">Redis Cache en Azure</span><span class="sxs-lookup"><span data-stu-id="2de7b-178">Redis Cache on Azure</span></span>](https://azure.microsoft.com/documentation/services/redis-cache/)
* [<span data-ttu-id="2de7b-179">SQL Database en Azure</span><span class="sxs-lookup"><span data-stu-id="2de7b-179">SQL Database on Azure</span></span>](https://azure.microsoft.com/documentation/services/sql-database/)
* <xref:performance/caching/memory>
* <xref:fundamentals/primitives/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>
