---
title: Reutilización de los objetos con ObjectPool en ASP.NET Core
author: rick-anderson
description: Sugerencias para aumentar el rendimiento en aplicaciones de ASP.NET Core mediante ObjectPool.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.date: 04/11/2019
uid: performance/ObjectPool
ms.openlocfilehash: 771f19e54a908b8b2cd85ff72f368f16e94a2310
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815525"
---
# <a name="object-reuse-with-objectpool-in-aspnet-core"></a><span data-ttu-id="9ce10-103">Reutilización de los objetos con ObjectPool en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9ce10-103">Object reuse with ObjectPool in ASP.NET Core</span></span>

<span data-ttu-id="9ce10-104">Por [Steve Gordon](https://twitter.com/stevejgordon), [Ryan Nowak](https://github.com/rynowak), y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="9ce10-104">By [Steve Gordon](https://twitter.com/stevejgordon), [Ryan Nowak](https://github.com/rynowak), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="9ce10-105"><xref:Microsoft.Extensions.ObjectPool> forma parte de la infraestructura de ASP.NET Core que admita mantener un grupo de objetos en memoria para su reutilización en lugar de permitir que los objetos de elementos no utilizados recopilados.</span><span class="sxs-lookup"><span data-stu-id="9ce10-105"><xref:Microsoft.Extensions.ObjectPool> is part of the ASP.NET Core infrastructure that supports keeping a group of objects in memory for reuse rather than allowing the objects to be garbage collected.</span></span>

<span data-ttu-id="9ce10-106">Es posible que desea utilizar el grupo de objetos si los objetos que se van a administrar son:</span><span class="sxs-lookup"><span data-stu-id="9ce10-106">You might want to use the object pool if the objects that are being managed are:</span></span>

- <span data-ttu-id="9ce10-107">Asignar o inicializar resulta caro.</span><span class="sxs-lookup"><span data-stu-id="9ce10-107">Expensive to allocate/initialize.</span></span>
- <span data-ttu-id="9ce10-108">Representan algunos recursos limitados.</span><span class="sxs-lookup"><span data-stu-id="9ce10-108">Represent some limited resource.</span></span>
- <span data-ttu-id="9ce10-109">Usa de forma predecible y con frecuencia.</span><span class="sxs-lookup"><span data-stu-id="9ce10-109">Used predictably and frequently.</span></span>

<span data-ttu-id="9ce10-110">Por ejemplo, el marco de ASP.NET Core usa el grupo de objetos en algunos lugares reutilizar <xref:System.Text.StringBuilder> instancias.</span><span class="sxs-lookup"><span data-stu-id="9ce10-110">For example, the ASP.NET Core framework uses the object pool in some places to reuse <xref:System.Text.StringBuilder> instances.</span></span> <span data-ttu-id="9ce10-111">`StringBuilder` asigna y administra su propia búferes para almacenar datos de caracteres.</span><span class="sxs-lookup"><span data-stu-id="9ce10-111">`StringBuilder` allocates and manages its own buffers to hold character data.</span></span> <span data-ttu-id="9ce10-112">ASP.NET Core se usa con frecuencia `StringBuilder` para implementar las características y reutilizarlos mejora el rendimiento.</span><span class="sxs-lookup"><span data-stu-id="9ce10-112">ASP.NET Core regularly uses `StringBuilder` to implement features, and reusing them provides a performance benefit.</span></span>

<span data-ttu-id="9ce10-113">Agrupación de objetos siempre no mejora el rendimiento:</span><span class="sxs-lookup"><span data-stu-id="9ce10-113">Object pooling doesn't always improve performance:</span></span>

- <span data-ttu-id="9ce10-114">A menos que el costo de inicialización de un objeto es alto, es normalmente más lento obtener el objeto del grupo.</span><span class="sxs-lookup"><span data-stu-id="9ce10-114">Unless the initialization cost of an object is high, it's usually slower to get the object from the pool.</span></span>
- <span data-ttu-id="9ce10-115">Objetos administrados por el grupo no se anulará la asignación hasta que se anule la asignación del grupo.</span><span class="sxs-lookup"><span data-stu-id="9ce10-115">Objects managed by the pool aren't de-allocated until the pool is de-allocated.</span></span>

<span data-ttu-id="9ce10-116">Usar agrupación de objetos solo después de recopilar datos de rendimiento mediante escenarios realistas para su aplicación o biblioteca.</span><span class="sxs-lookup"><span data-stu-id="9ce10-116">Use object pooling only after collecting performance data using realistic scenarios for your app or library.</span></span>

<span data-ttu-id="9ce10-117">**ADVERTENCIA: El `ObjectPool` no implementa `IDisposable`. No se recomienda utilizarlo con tipos que necesitan la eliminación.**</span><span class="sxs-lookup"><span data-stu-id="9ce10-117">**WARNING: The `ObjectPool` doesn't implement `IDisposable`. We don't recommend using it with types that need disposal.**</span></span>

<span data-ttu-id="9ce10-118">**NOTA: El ObjectPool no pone un límite en el número de objetos que va a asignar, impone un límite en el número de objetos que conservarán.**</span><span class="sxs-lookup"><span data-stu-id="9ce10-118">**NOTE: The ObjectPool doesn't place a limit on the number of objects that it will allocate, it places a limit on the number of objects it will retain.**</span></span>

## <a name="concepts"></a><span data-ttu-id="9ce10-119">Conceptos</span><span class="sxs-lookup"><span data-stu-id="9ce10-119">Concepts</span></span>

<span data-ttu-id="9ce10-120"><xref:Microsoft.Extensions.ObjectPool.ObjectPool`1> -la abstracción del grupo de objetos básicos.</span><span class="sxs-lookup"><span data-stu-id="9ce10-120"><xref:Microsoft.Extensions.ObjectPool.ObjectPool`1> - the basic object pool abstraction.</span></span> <span data-ttu-id="9ce10-121">Se utiliza para obtener y devolver objetos.</span><span class="sxs-lookup"><span data-stu-id="9ce10-121">Used to get and return objects.</span></span>

<span data-ttu-id="9ce10-122"><xref:Microsoft.Extensions.ObjectPool.PooledObjectPolicy%601> -implementar esto para personalizar cómo se crea un objeto y cómo es *restablecer* cuando regrese al grupo.</span><span class="sxs-lookup"><span data-stu-id="9ce10-122"><xref:Microsoft.Extensions.ObjectPool.PooledObjectPolicy%601> - implement this to customize how an object is created and how it is *reset* when returned to the pool.</span></span> <span data-ttu-id="9ce10-123">Esto se puede pasar a un grupo de objetos que se construye directamente... OR</span><span class="sxs-lookup"><span data-stu-id="9ce10-123">This can be passed into an object pool that you construct directly.... OR</span></span>

<span data-ttu-id="9ce10-124"><xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider.Create*> actúa como un generador para crear los grupos de objetos.</span><span class="sxs-lookup"><span data-stu-id="9ce10-124"><xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider.Create*> acts as a factory for creating object pools.</span></span>
<!-- REview, there is no ObjectPoolProvider<T> -->

<span data-ttu-id="9ce10-125">El ObjectPool puede usarse en una aplicación de varias maneras:</span><span class="sxs-lookup"><span data-stu-id="9ce10-125">The ObjectPool can be used in an app in multiple ways:</span></span>

* <span data-ttu-id="9ce10-126">Crear instancias de un grupo.</span><span class="sxs-lookup"><span data-stu-id="9ce10-126">Instantiating a pool.</span></span>
* <span data-ttu-id="9ce10-127">Registrar un grupo en [inserción de dependencias](xref:fundamentals/dependency-injection) (DI) como una instancia.</span><span class="sxs-lookup"><span data-stu-id="9ce10-127">Registering a pool in [Dependency injection](xref:fundamentals/dependency-injection) (DI) as an instance.</span></span>
* <span data-ttu-id="9ce10-128">Registrar el `ObjectPoolProvider<>` en DI y usarlo como una fábrica.</span><span class="sxs-lookup"><span data-stu-id="9ce10-128">Registering the `ObjectPoolProvider<>` in DI and using it as a factory.</span></span>

## <a name="how-to-use-objectpool"></a><span data-ttu-id="9ce10-129">Cómo usar ObjectPool</span><span class="sxs-lookup"><span data-stu-id="9ce10-129">How to use ObjectPool</span></span>

<span data-ttu-id="9ce10-130">Llame a <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1> para obtener un objeto y <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1.Return*> para devolver el objeto.</span><span class="sxs-lookup"><span data-stu-id="9ce10-130">Call <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1> to get an object and <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1.Return*> to return the object.</span></span>  <span data-ttu-id="9ce10-131">No hay ningún requisito que devuelva todos los objetos.</span><span class="sxs-lookup"><span data-stu-id="9ce10-131">There's no requirement that you return every object.</span></span> <span data-ttu-id="9ce10-132">Si no devuelve un objeto, será recolectado.</span><span class="sxs-lookup"><span data-stu-id="9ce10-132">If you don't return an object, it will be garbage collected.</span></span>

## <a name="objectpool-sample"></a><span data-ttu-id="9ce10-133">Ejemplo de ObjectPool</span><span class="sxs-lookup"><span data-stu-id="9ce10-133">ObjectPool sample</span></span>

<span data-ttu-id="9ce10-134">El código siguiente:</span><span class="sxs-lookup"><span data-stu-id="9ce10-134">The following code:</span></span>

* <span data-ttu-id="9ce10-135">Agrega `ObjectPoolProvider` a la [inserción de dependencias](xref:fundamentals/dependency-injection) contenedor (DI).</span><span class="sxs-lookup"><span data-stu-id="9ce10-135">Adds `ObjectPoolProvider` to the [Dependency injection](xref:fundamentals/dependency-injection) (DI) container.</span></span>
* <span data-ttu-id="9ce10-136">Agrega y configura `ObjectPool<StringBuilder>` para el contenedor de DI.</span><span class="sxs-lookup"><span data-stu-id="9ce10-136">Adds and configures `ObjectPool<StringBuilder>` to the DI container.</span></span>
* <span data-ttu-id="9ce10-137">Agrega el `BirthdayMiddleware`.</span><span class="sxs-lookup"><span data-stu-id="9ce10-137">Adds the `BirthdayMiddleware`.</span></span>

[!code-csharp[](ObjectPool/ObjectPoolSample/Startup.cs?name=snippet)]

<span data-ttu-id="9ce10-138">El código siguiente implementa `BirthdayMiddleware`</span><span class="sxs-lookup"><span data-stu-id="9ce10-138">The following code implements `BirthdayMiddleware`</span></span>

[!code-csharp[](ObjectPool/ObjectPoolSample/BirthdayMiddleware.cs?name=snippet)]
