---
title: Trabajar con una memoria caché distribuida en ASP.NET Core
author: ardalis
description: Obtenga información acerca de cómo usar ASP.NET Core distribuida almacenamiento en caché para mejorar el rendimiento de la aplicación y la escalabilidad, especialmente en un entorno de granja de servidores en la nube o de servidor.
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: performance/caching/distributed
ms.openlocfilehash: d9c7c1c3b2c052ba11f9ea5eaaa424d69bc43eb2
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2018
---
# <a name="work-with-a-distributed-cache-in-aspnet-core"></a>Trabajar con una memoria caché distribuida en ASP.NET Core

Por [Steve Smith](https://ardalis.com/)

Las memorias caché distribuidas pueden mejorar el rendimiento y la escalabilidad de las aplicaciones de ASP.NET Core, especialmente cuando se hospeda en un entorno de granja de servidores en la nube o de servidor. Este artículo explica cómo trabajar con implementaciones y abstracciones de caché distribuida integrado del núcleo de ASP.NET.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-a-distributed-cache"></a>¿Qué es una memoria caché distribuida

Una memoria caché distribuida es compartida por varios servidores de aplicación (consulte [conceptos básicos de la memoria caché](memory.md#caching-basics)). La información de la memoria caché no se guarda en la memoria de los servidores web individuales y los datos almacenados en caché están disponibles para todos los servidores de aplicaciones. Esto proporciona varias ventajas:

1. Los datos almacenados en caché son coherentes en todos los servidores web. Los usuarios no verán resultados distintos sin importar el servidor web que controla su solicitud.

2. Los datos almacenados en caché sobreviven a reinicios del servidor web y las implementaciones. Servidores web individuales se pueden quitar o agregar sin afectar a la memoria caché.

3. El almacén de datos de origen tiene menos las solicitudes realizadas a él (de con varias cachés en memoria o en no caché en absoluto).

> [!NOTE]
> Si usa una memoria caché distribuida de SQL Server, algunas de las siguientes ventajas solo son ciertas si se utiliza una instancia de base de datos independiente para la memoria caché que para los datos de origen de la aplicación.

Al igual que cualquier memoria caché, una memoria caché distribuida puede mejorar considerablemente la capacidad de respuesta de una aplicación, ya que normalmente se pueden recuperar datos de la memoria caché mucho más rápida que de una base de datos relacional (o servicio web).

La configuración de la caché es específica de la implementación. En este artículo se describe cómo configurar los almacenes distribuidos en caché Redis y SQL Server. Independientemente de qué implementación se seleccione, la aplicación interactúa con la memoria caché utilizando una interfaz común `IDistributedCache`.

## <a name="the-idistributedcache-interface"></a>The IDistributedCache Interface

El `IDistributedCache` interfaz incluye métodos sincrónicos y asincrónicos. La interfaz permite elementos agregar, recuperar y quitar de la implementación de caché distribuida. El `IDistributedCache` interfaz incluye los métodos siguientes:

**Get, GetAsync**

Toma una clave de cadena y recupera un elemento almacenado en caché como un `byte[]` si se encuentra en la memoria caché.

**Set, SetAsync**

Agrega un elemento (como `byte[]`) a la memoria caché utilizando una clave de cadena.

**Refresh, RefreshAsync**

Actualiza un elemento en la memoria caché basado en su clave, restablecer su tiempo de espera de expiración deslizante (si existe).

**Remove, RemoveAsync**

Quita una entrada de caché basada en su clave.

Para usar el `IDistributedCache` interfaz:

   1. Agregue los paquetes de NuGet necesarios para el archivo de proyecto.

   2. Configurar la implementación específica de `IDistributedCache` en su `Startup` la clase `ConfigureServices` método y lo agrega al contenedor no existe.

   3. Desde la aplicación [Middleware](xref:fundamentals/middleware/index) o las clases de controlador MVC, solicite una instancia de `IDistributedCache` desde el constructor. La instancia se proporcionarán con [inyección de dependencia](../../fundamentals/dependency-injection.md) (DI).

> [!NOTE]
> No es necesario usar una duración de Singleton o en el ámbito para `IDistributedCache` instancias (al menos para las implementaciones integradas). También puede crear una instancia donde podría necesitar uno (en lugar de usar [inyección de dependencia](../../fundamentals/dependency-injection.md)), pero esto puede dificultar el código probar e infringe el [principio de dependencias explícitas](http://deviq.com/explicit-dependencies-principle/).

En el ejemplo siguiente se muestra cómo utilizar una instancia de `IDistributedCache` en un componente de middleware simple:

[!code-csharp[](./distributed/sample/src/DistCacheSample/StartTimeHeader.cs?highlight=15,18,21,27,28,29,30,31)]

En el código anterior, el valor almacenado en caché se lee pero nunca se ha escrito. En este ejemplo, el valor sólo se establece cuando se inicia un servidor y no cambia. En un escenario de varios servidores, el servidor más reciente para iniciar sobrescribirá los valores anteriores que se establecieron con otros servidores. El `Get` y `Set` métodos utilizan el `byte[]` tipo. Por lo tanto, se debe convertir el valor de cadena mediante `Encoding.UTF8.GetString` (para `Get`) y `Encoding.UTF8.GetBytes` (para `Set`).

El siguiente código *Startup.cs* muestra el valor que se va a establecer:

[!code-csharp[](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=2,4,5,6&range=58-66)]

> [!NOTE]
> Puesto que `IDistributedCache` está configurado en el método `ConfigureServices`, está disponible para el método `Configure` como parámetro Agregado como un parámetro permitirá proporcionar la instancia configurada a través de DI.

## <a name="using-a-redis-distributed-cache"></a>Uso de una caché de Redis distribuida

[Redis](https://redis.io/) es un almacén de datos en memoria de código abierto, que a menudo se usa como una memoria caché distribuida. Puede usar de forma local, y puede configurar un [Azure Redis Cache](https://azure.microsoft.com/services/cache/) para las aplicaciones principales de ASP.NET hospedados en Azure. La aplicación de ASP.NET Core configura la implementación de caché mediante una instancia `RedisDistributedCache`.

Configurar la implementación de Redis en `ConfigureServices` y tener acceso a él en el código de aplicación solicitando una instancia de `IDistributedCache` (vea el código anterior).

En el código de ejemplo, se utiliza una implementación `RedisCache` cuando el servidor está configurado para un entorno `Staging`. Por lo tanto el método`ConfigureStagingServices` configura el `RedisCache`:

[!code-csharp[](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=8,9,10,11,12,13&range=27-40)]

> [!NOTE]
> Para instalar Redis en el equipo local, instale el paquete chocolatey [ https://chocolatey.org/packages/redis-64/ ](https://chocolatey.org/packages/redis-64/) y ejecutar `redis-server` desde un símbolo del sistema.

## <a name="using-a-sql-server-distributed-cache"></a>Uso de un servidor SQL de caché distribuida

La implementación de SqlServerCache permite que la memoria caché distribuida usar una base de datos de SQL Server como su memoria auxiliar. Para crear SQL Server se puede utilizar la herramienta de caché de sql, la herramienta crea una tabla con el nombre y el esquema que especifica.

Para usar la herramienta de caché de sql, agregue `SqlConfig.Tools` al elemento`<ItemGroup>` del archivo *.csproj* y ejecute la restauración de dotnet.

[!code-xml[](./distributed/sample/src/DistCacheSample/DistCacheSample.csproj?range=23-25)]

Ejecute el siguiente comando para comprobar SqlConfig.Tools

```none
C:\DistCacheSample\src\DistCacheSample>dotnet sql-cache create --help
   ```

La herramienta de caché de SQL muestra ayuda sobre uso, opciones y comandos, ahora puede crear tablas en sql server, ejecuta el comando "creación de caché de sql":

```none
C:\DistCacheSample\src\DistCacheSample>dotnet sql-cache create "Data Source=(localdb)\v11.0;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
   info: Microsoft.Extensions.Caching.SqlConfig.Tools.Program[0]
       Table and index were created successfully.
   ```

La tabla creada tiene el siguiente esquema:

![Tabla de la caché de SQL Server](distributed/_static/SqlServerCacheTable.png)

Al igual que todas las implementaciones de caché, la aplicación debe obtener y establecer valores de caché mediante una instancia de `IDistributedCache`, no un `SqlServerCache`. El ejemplo implementa `SqlServerCache` en el `Production` entorno (por lo que se configura en `ConfigureProductionServices`).

[!code-csharp[](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=7,8,9,10,11,12&range=42-56)]

> [!NOTE]
> El `ConnectionString` (y, opcionalmente, `SchemaName` y `TableName`) normalmente deberían almacenarse fuera de control de código fuente (por ejemplo, UserSecrets), ya que pueden contener las credenciales.

## <a name="recommendations"></a>Recomendaciones

La hora de decidir qué implementación de `IDistributedCache` es adecuado para la aplicación, elija entre Redis y SQL Server se basa en la infraestructura existente y entorno, los requisitos de rendimiento y experiencia de su equipo. Si su equipo es más fácil trabajar con Redis, es una excelente opción. Si el equipo prefiera SQL Server, puede estar seguro de que así la implementación. Tenga en cuenta que una solución de almacenamiento en caché tradicional almacena datos en memoria que permite la recuperación rápida de datos. Debe almacenar los datos de uso frecuente en una memoria caché y almacenar todos los datos en un almacén persistente de back-end como SQL Server o el almacenamiento de Azure. Caché en Redis es una solución de almacenamiento en caché que proporciona un alto rendimiento y baja latencia en comparación con la memoria caché de SQL.

## <a name="additional-resources"></a>Recursos adicionales

* [Caché en Azure en Redis](https://azure.microsoft.com/documentation/services/redis-cache/)
* [Base de datos SQL en Azure](https://azure.microsoft.com/documentation/services/sql-database/)
* [En la memoria de caché](xref:performance/caching/memory)
* [Detectar cambios con tokens de cambio](xref:fundamentals/primitives/change-tokens)
* [Almacenamiento en caché de respuestas](xref:performance/caching/response)
* [Middleware de almacenamiento en caché de respuestas](xref:performance/caching/middleware)
* [Aplicación auxiliar de etiquetas de caché](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [Aplicación auxiliar de etiquetas de caché distribuida](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
