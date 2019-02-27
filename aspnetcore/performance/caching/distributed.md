---
title: Almacenamiento en caché en ASP.NET Core distribuido
author: guardrex
description: Obtenga información sobre cómo usar una caché distribuida de ASP.NET Core para mejorar el rendimiento de la aplicación y la escalabilidad, especialmente en un entorno de granja de servidores en la nube o servidor.
ms.author: riande
ms.custom: mvc
ms.date: 02/26/2019
uid: performance/caching/distributed
ms.openlocfilehash: 7337ee3b823064c942832d8a44e4d4289bc4fd0e
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/27/2019
ms.locfileid: "56899429"
---
# <a name="distributed-caching-in-aspnet-core"></a>Almacenamiento en caché en ASP.NET Core distribuido

Por [Steve Smith](https://ardalis.com/) y [Luke Latham](https://github.com/guardrex)

Una caché distribuida es una caché compartida por varios servidores de aplicación, que normalmente se mantiene como un servicio externo a los servidores de aplicaciones que acceden a ellos. Una memoria caché distribuida puede mejorar el rendimiento y escalabilidad de una aplicación ASP.NET Core, especialmente cuando la aplicación está hospedada en un servicio en la nube o una granja de servidores.

Una memoria caché distribuida tiene varias ventajas sobre otros escenarios de almacenamiento en caché se almacenan en caché datos en los servidores de aplicaciones individuales.

Cuando se distribuyen los datos almacenados en caché, los datos:

* Es *coherente* (coherentes) a través de solicitudes en varios servidores.
* Sobrevive a los reinicios del servidor y las implementaciones de aplicaciones.
* No utilice memoria local.

Configuración de caché distribuida es específico de la implementación. En este artículo se describe cómo configurar SQL Server y las memorias caché distribuidas de Redis. Las implementaciones de otros fabricantes también están disponibles, como [NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache en GitHub](https://github.com/Alachisoft/NCache)). Independientemente de qué implementación está seleccionada, la aplicación interactúa con la caché mediante el <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interfaz.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Requisitos previos

::: moniker range=">= aspnetcore-2.2"

Para usar un servidor SQL Server distributed cache, referencia del [Microsoft.AspNetCore.App metapaquete](xref:fundamentals/metapackage-app) o agregar una referencia de paquete para el [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) paquete.

Para usar un Redis caché, referencia distribuida el [Microsoft.AspNetCore.App metapaquete](xref:fundamentals/metapackage-app) y agregue una referencia de paquete para el [Microsoft.Extensions.Caching.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) paquete. El paquete de Redis no se incluye en el `Microsoft.AspNetCore.App` del paquete, por lo que debe hacer referencia el paquete de Redis por separado en el archivo de proyecto.

::: moniker-end

::: moniker range="= aspnetcore-2.1"

Para usar un servidor SQL Server distributed cache, referencia del [Microsoft.AspNetCore.App metapaquete](xref:fundamentals/metapackage-app) o agregar una referencia de paquete para el [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) paquete.

Para usar un Redis caché, referencia distribuida el [Microsoft.AspNetCore.App metapaquete](xref:fundamentals/metapackage-app) y agregue una referencia de paquete para el [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) paquete. El paquete de Redis no se incluye en el `Microsoft.AspNetCore.App` del paquete, por lo que debe hacer referencia el paquete de Redis por separado en el archivo de proyecto.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Para usar un servidor SQL Server distributed cache, referencia del [metapaquete Microsoft.AspNetCore.All](xref:fundamentals/metapackage) o agregar una referencia de paquete para el [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) paquete.

Para usar un Redis caché, referencia distribuida el [metapaquete Microsoft.AspNetCore.All](xref:fundamentals/metapackage) o agregar una referencia de paquete para el [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) paquete. El paquete de Redis se incluye en `Microsoft.AspNetCore.All` del paquete, por lo que no es necesario hacer referencia al paquete de Redis por separado en el archivo de proyecto.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Para utilizar un servidor SQL de caché distribuida, agregue una referencia de paquete para el [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) paquete.

Para usar un Redis caché distribuida, agregue una referencia de paquete para el [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) paquete.

::: moniker-end

## <a name="idistributedcache-interface"></a>Interfaz IDistributedCache

El <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interfaz proporciona los siguientes métodos para manipular los elementos en la implementación de caché distribuida:

* <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> &ndash; Acepta una clave de cadena y recupera un elemento almacenado en caché como un `byte[]` matriz si se encuentra en la memoria caché.
* <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> &ndash; Agrega un elemento (como `byte[]` matriz) a la memoria caché utilizando una clave de cadena.
* <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*> &ndash; Actualiza un elemento en la memoria caché en función de su clave, restablecer el tiempo de espera de expiración deslizante (si existe).
* <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> &ndash; Quita un elemento de caché en función de su clave de cadena.

## <a name="establish-distributed-caching-services"></a>Establezca los servicios de almacenamiento en caché distribuidos

Registrar una implementación de <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> en `Startup.ConfigureServices`. Las implementaciones de proporcionados por .NET Framework que se describe en este tema se incluyen:

* [Caché distribuida de memoria](#distributed-memory-cache)
* [Caché distribuida de SQL Server](#distributed-sql-server-cache)
* [Caché distribuida de Redis](#distributed-redis-cache)

### <a name="distributed-memory-cache"></a>Caché distribuida de memoria

La caché de memoria distribuida (<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*>) es una implementación proporcionados por el marco de trabajo de <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> que almacena los elementos en la memoria. La memoria caché distribuida no es una caché distribuida real. Los elementos en caché se almacenan por la instancia de aplicación en el servidor donde se ejecuta la aplicación.

La caché de memoria distribuido es una implementación útil:

* En escenarios de prueba y desarrollo.
* Cuando se usa un único servidor de producción y consumo de memoria no supone un problema. Implementar los resúmenes de caché distribuida de memoria, almacenamiento de datos en caché. Permite implementar que un verdadero distribuir soluciones de almacenamiento en caché en el futuro cuando varios nodos o tolerancia a errores que sea necesario.

La aplicación de ejemplo hace uso de la caché de memoria distribuida cuando la aplicación se ejecuta en el entorno de desarrollo:

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_ConfigureServices&highlight=5)]

### <a name="distributed-sql-server-cache"></a>Caché distribuida de SQL Server

La implementación de caché distribuida de SQL Server (<xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*>) permite que la memoria caché distribuida usar una base de datos de SQL Server como almacén de respaldo. Para crear una tabla de elemento almacenado en caché de SQL Server en una instancia de SQL Server, puede usar el `sql-cache` herramienta. La herramienta crea una tabla con el nombre y el esquema que especifique.

::: moniker range="< aspnetcore-2.1"

Agregar `SqlConfig.Tools` a la `<ItemGroup>` elemento del archivo de proyecto y ejecute `dotnet restore`.

```xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.Extensions.Caching.SqlConfig.Tools"
                          Version="2.0.2" />
</ItemGroup>
```

::: moniker-end

Crear una tabla en SQL Server ejecutando el `sql-cache create` comando. Proporcionar la instancia de SQL Server (`Data Source`), base de datos (`Initial Catalog`), esquema (por ejemplo, `dbo`) y el nombre de tabla (por ejemplo, `TestCache`):

```console
dotnet sql-cache create "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
```

Se registra un mensaje para indicar que la herramienta se realizó correctamente:

```console
Table and index were created successfully.
```

La tabla creada por el `sql-cache` herramienta tiene el siguiente esquema:

![Tabla de la caché de SQL Server](distributed/_static/SqlServerCacheTable.png)

> [!NOTE]
> Una aplicación debe manipular los valores de caché mediante una instancia de <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>, no un <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>.

La aplicación de ejemplo implementa <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> en un entorno de desarrollo que no son:

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_ConfigureServices&highlight=9-15)]

> [!NOTE]
> Un <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*> (y, opcionalmente, <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*> y <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*>) normalmente se almacenan fuera de control de código fuente (por ejemplo, almacenando el [Secret Manager](xref:security/app-secrets) o en *appsettings.json* / *appsettings. {Entorno} .json* archivos). La cadena de conexión puede contener las credenciales que se deben mantener fuera de los sistemas de control de código fuente.

### <a name="distributed-redis-cache"></a>Caché en Redis distribuida

[Redis](https://redis.io/) es un almacén de datos en memoria de código abierto, que a menudo se usa como una memoria caché distribuida. Puede usar Redis localmente, y puede configurar un [Azure Redis Cache](https://azure.microsoft.com/services/cache/) para una aplicación ASP.NET Core hospedadas en Azure. Una aplicación configura la implementación de caché mediante un <xref:Microsoft.Extensions.Caching.Redis.RedisCache> instancia (<xref:Microsoft.Extensions.DependencyInjection.RedisCacheServiceCollectionExtensions.AddDistributedRedisCache*>):

```csharp
services.AddDistributedRedisCache(options =>
{
    options.Configuration = "localhost";
    options.InstanceName = "SampleInstance";
});
```

Para instalar Redis en el equipo local:

* Instalar el [paquete Redis Chocolatey](https://chocolatey.org/packages/redis-64/).
* Ejecute `redis-server` desde un símbolo del sistema.

## <a name="use-the-distributed-cache"></a>Usar la memoria caché distribuida

Para usar el <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interfaz, solicitar una instancia de <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> desde cualquier constructor en la aplicación. Proporciona la instancia [inserción de dependencias (DI)](xref:fundamentals/dependency-injection).

Cuando se inicia la aplicación, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> se inyecta en `Startup.Configure`. La hora actual se almacena en caché mediante <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime> (para obtener más información, consulte [Host Web: Interfaz IApplicationLifetime](xref:fundamentals/host/web-host#iapplicationlifetime-interface)):

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_Configure&highlight=10)]

La aplicación de ejemplo inyecta <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> en el `IndexModel` para su uso en la página de índice.

Cada vez que se carga la página de índice, se comprueba la memoria caché para el tiempo en caché en `OnGetAsync`. Si no ha expirado el tiempo en caché, se muestra el tiempo. Si 20 segundos transcurridos desde la última vez que se tuvo acceso el tiempo en caché (la última vez que se cargó esta página), aparece la página *tiempo en caché expirado*.

Actualizar inmediatamente el tiempo en caché a la hora actual, seleccione el **Restablecer tiempo en caché** botón. Los desencadenadores de botón el `OnPostResetCachedTime` método del controlador.

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Pages/Index.cshtml.cs?name=snippet_IndexModel&highlight=7,14-20,25-29)]

> [!NOTE]
> No hay ninguna necesidad de usar una duración Singleton o en el ámbito para <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> instancias (al menos para las implementaciones integradas).
>
> También puede crear un <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> instancia donde es posible que necesite uno en lugar de usar la inserción de dependencias, pero crear una instancia en el código puede hacer que su código sea más difícil de probar e infringe el [dependencias explicitas](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies).

## <a name="recommendations"></a>Recomendaciones

La hora de decidir qué implementación de <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> es mejor para su aplicación, considere lo siguiente:

* Infraestructura existente
* Requisitos de rendimiento
* Costo
* Experiencia del equipo

Soluciones de almacenamiento en caché normalmente se basan en el almacenamiento en memoria para proporcionar una recuperación rápida de los datos almacenados en caché, pero la memoria es un recurso limitado y expanda el costo. Único almacén de datos utilizados normalmente en una memoria caché.

Por lo general, una instancia de Redis cache proporciona un mayor rendimiento y latencia más baja que una memoria caché de SQL Server. Sin embargo, las pruebas comparativas se suelen ser necesarias para determinar las características de rendimiento de las estrategias de almacenamiento en caché.

Cuando se usa SQL Server como un almacén de respaldo de caché distribuida, use la misma base de datos para la memoria caché y almacenamiento de datos normal de la aplicación y recuperación puede afectar negativamente al rendimiento de ambos. Se recomienda usar una instancia de SQL Server dedicada para la memoria caché distribuida auxiliar.

## <a name="additional-resources"></a>Recursos adicionales

* [Redis Cache en Azure](https://azure.microsoft.com/documentation/services/redis-cache/)
* [SQL Database en Azure](https://azure.microsoft.com/documentation/services/sql-database/)
* [ASP.NET Core proveedor IDistributedCache de NCache en granjas de servidores Web](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache en GitHub](https://github.com/Alachisoft/NCache))
* <xref:performance/caching/memory>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>
