---
title: "En la memoria de almacenamiento en caché de ASP.NET Core"
author: rick-anderson
description: "Muestra cómo almacenar en caché los datos en memoria en ASP.NET Core."
keywords: "Núcleo de ASP.NET, rendimiento de la memoria, caché,"
ms.author: riande
manager: wpickett
ms.date: 12/14/2016
ms.topic: article
ms.assetid: 819511cf-d33e-410a-b5a9-bef7fa64d2f3
ms.technology: aspnet
ms.prod: asp.net-core
uid: performance/caching/memory
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b5dca6a81a66ce2a8771f1a16e63834d6504d8b6
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/22/2017
---
# <a name="introduction-to-in-memory-caching-in-aspnet-core"></a>Introducción al almacenamiento en caché en memoria de ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo), y [Steve Smith](https://ardalis.com/)

[Ver o descargar el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample)

## <a name="caching-basics"></a>Conceptos básicos sobre el almacenamiento en caché

Almacenamiento en caché puede mejorar significativamente el rendimiento y la escalabilidad de una aplicación al reducir el trabajo necesario para generar el contenido. Almacenamiento en caché funciona mejor con datos que cambian con poca frecuencia. Almacenamiento en caché realiza una copia de datos que se pueden devolver mucho más rápida que la de la fuente original. Debe escribir y probar la aplicación para que nunca dependen de datos almacenados en caché.

ASP.NET Core es compatible con varias memorias caché diferentes. La memoria caché más sencilla se basa en el [IMemoryCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.imemorycache), que representa una caché almacenada en la memoria del servidor web. Las aplicaciones que se ejecutan en una granja de servidores de varios servidores deben asegurarse de que las sesiones son rápidas cuando se usa la memoria caché en memoria. Sesiones permanentes Asegúrese de que las solicitudes posteriores de un cliente todos los vayan al mismo servidor. Por ejemplo, el uso de aplicaciones Web de Azure [enrutamiento de solicitud de aplicación](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) para enrutar las solicitudes subsiguientes en el mismo servidor.

Las sesiones no son permanentes en una granja de servidores web requieren un [caché distribuida](distributed.md) para evitar problemas de coherencia de la memoria caché. Para algunas aplicaciones, una memoria caché distribuida puede admitir una escala mayor espera que una caché en memoria. Uso de una memoria caché distribuida, descarga la memoria caché para un proceso externo. 

El `IMemoryCache` caché expulsará las entradas de caché bajo presión de memoria, a menos que la [almacenar en caché prioridad](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheitempriority) está establecido en `CacheItemPriority.NeverRemove`. Puede establecer el `CacheItemPriority` para ajustar la prioridad de la memoria caché extrae elementos bajo presión de memoria.

La memoria caché en memoria puede almacenar cualquier objeto; la interfaz de caché distribuida se limita a `byte[]`.

## <a name="using-imemorycache"></a>Usar IMemoryCache

Almacenamiento en caché en memoria es un *servicio* que se hace referencia desde la aplicación con [inyección de dependencia](../../fundamentals/dependency-injection.md). Llame a `AddMemoryCache` en `ConfigureServices`:

[!code-csharp[Main](memory/sample/WebCache/Startup.cs?highlight=8)] 

Solicitar la `IMemoryCache` instancia en el constructor:

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ctor&highlight=3,5-)] 

`IMemoryCache`requiere el paquete NuGet "Microsoft.Extensions.Caching.Memory".

El siguiente código utiliza [TryGetValue](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.imemorycache#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) para comprobar si la hora actual está en la memoria caché. Si el elemento no se almacena en caché, se crea y se agrega a la caché con una nueva entrada [establecer](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_).

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet1)]

Se muestra la hora actual y el tiempo en caché:

[!code-html[Main](memory/sample/WebCache/Views/Home/Cache.cshtml)]

Las almacenadas en caché `DateTime` valor permanecerá en la memoria caché mientras haya solicitudes dentro del tiempo de espera (y ningún expulsión debido a la presión de memoria). La imagen siguiente muestra la hora actual y una hora anterior obtenido de la caché:

![Vista de índice con dos momentos diferentes que se muestran](memory/_static/time.png)

El siguiente código utiliza [GetOrCreate](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) y [GetOrCreateAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) en caché los datos. 

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

El código siguiente llama [obtener](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) para capturar el tiempo en caché:

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_gct)]

Vea [IMemoryCache métodos](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.imemorycache) y [CacheExtensions métodos](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions) para obtener una descripción de los métodos de la memoria caché.

## <a name="using-memorycacheentryoptions"></a>Usar MemoryCacheEntryOptions

El ejemplo siguiente:

- Establece la hora de expiración absoluta. Esto es el tiempo máximo que puede almacenarse en caché la entrada y evita que el elemento se conviertan en obsoletos cuando continuamente se renueva el plazo de caducidad.
- Establece un tiempo de expiración variable. Las solicitudes que tienen acceso a este elemento almacenado en caché, restablecerán el reloj de expiración deslizante.
- Establece la prioridad de la memoria caché en `CacheItemPriority.NeverRemove`. 
- Establece un [PostEvictionDelegate](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.postevictiondelegate) al que se llama después de la entrada se expulsa de la memoria caché. La devolución de llamada se ejecuta en un subproceso diferente del código que se quita el elemento de la memoria caché.

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_et&highlight=14-20)]

## <a name="cache-dependencies"></a>Dependencias de caché

El ejemplo siguiente muestra cómo expirar una entrada de caché si expira una entrada dependiente. Un `CancellationChangeToken` se agrega al elemento almacenado en caché. Cuando `Cancel` se llama en el `CancellationTokenSource`, ambas entradas de caché se desalojan. 

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ed)]

Mediante un `CancellationTokenSource` permite varias entradas de caché que se expulsen como un grupo. Con el `using` patrón en el código anterior, las entradas de caché se crean dentro de la `using` bloque heredarán opciones de expiración y desencadenadores.

### <a name="additional-notes"></a>Notas adicionales

- Cuando se usa una devolución de llamada para rellenar un elemento de caché:

  - Varias solicitudes pueden encontrar el valor de clave almacenada en caché vacía porque no se ha completado la devolución de llamada. 
  - Esto puede dar lugar a que varios subprocesos al rellenar el elemento en caché.

- Cuando una entrada de caché se utiliza para crear otro, el elemento secundario copia tokens de expiración y la configuración de expiración basada en el momento de la entrada primaria. El elemento secundario no está expirada para eliminación manual o actualización de la entrada primaria.

### <a name="other-resources"></a>Otros recursos

* [Trabajar con una memoria caché distribuida](distributed.md)
* [Middleware de almacenamiento en caché de respuestas](middleware.md)
