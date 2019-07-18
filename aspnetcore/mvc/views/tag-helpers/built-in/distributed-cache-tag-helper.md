---
title: Asistente de etiquetas de caché distribuida en ASP.NET Core
author: pkellner
description: Obtenga información sobre cómo usar el asistente de etiquetas de caché distribuida.
ms.author: riande
ms.custom: mvc
ms.date: 10/10/2018
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: 4e4d383bac67c73bad8b0a31b9ceb9452251761b
ms.sourcegitcommit: 7a40c56bf6a6aaa63a7ee83a2cac9b3a1d77555e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/12/2019
ms.locfileid: "67856201"
---
# <a name="distributed-cache-tag-helper-in-aspnet-core"></a><span data-ttu-id="0c7c6-103">Asistente de etiquetas de caché distribuida en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0c7c6-103">Distributed Cache Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="0c7c6-104">Por [Peter Kellner](https://peterkellner.net) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="0c7c6-104">By [Peter Kellner](https://peterkellner.net) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="0c7c6-105">El asistente de etiquetas de caché distribuida proporciona la capacidad de mejorar drásticamente el rendimiento de la aplicación ASP.NET Core al permitir almacenar en caché su contenido en un origen de caché distribuida.</span><span class="sxs-lookup"><span data-stu-id="0c7c6-105">The Distributed Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to a distributed cache source.</span></span>

<span data-ttu-id="0c7c6-106">Para obtener información general sobre asistentes de etiquetas, vea <xref:mvc/views/tag-helpers/intro>.</span><span class="sxs-lookup"><span data-stu-id="0c7c6-106">For an overview of Tag Helpers, see <xref:mvc/views/tag-helpers/intro>.</span></span>

<span data-ttu-id="0c7c6-107">El asistente de etiquetas de caché distribuida hereda de la misma clase base que el asistente de etiquetas de caché.</span><span class="sxs-lookup"><span data-stu-id="0c7c6-107">The Distributed Cache Tag Helper inherits from the same base class as the Cache Tag Helper.</span></span> <span data-ttu-id="0c7c6-108">Todos los atributos del [asistente de etiquetas de caché](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper) están disponibles para el asistente de etiquetas distribuidas.</span><span class="sxs-lookup"><span data-stu-id="0c7c6-108">All of the [Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper) attributes are available to the Distributed Tag Helper.</span></span>

<span data-ttu-id="0c7c6-109">El asistente de etiquetas de caché distribuida usa la [inserción de constructor](xref:fundamentals/dependency-injection#constructor-injection-behavior).</span><span class="sxs-lookup"><span data-stu-id="0c7c6-109">The Distributed Cache Tag Helper uses [constructor injection](xref:fundamentals/dependency-injection#constructor-injection-behavior).</span></span> <span data-ttu-id="0c7c6-110">La interfaz <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> se pasa al constructor del asistente de etiquetas de caché distribuida.</span><span class="sxs-lookup"><span data-stu-id="0c7c6-110">The <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface is passed into the Distributed Cache Tag Helper's constructor.</span></span> <span data-ttu-id="0c7c6-111">Si no se ha creado ninguna implementación específica de `IDistributedCache` en `Startup.ConfigureServices` (*Startup.cs*), el asistente de etiquetas de caché distribuida usa el mismo proveedor en memoria para almacenar datos en caché como el [asistente de etiquetas de caché](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="0c7c6-111">If no concrete implementation of `IDistributedCache` is created in `Startup.ConfigureServices` (*Startup.cs*), the Distributed Cache Tag Helper uses the same in-memory provider for storing cached data as the [Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span></span>

## <a name="distributed-cache-tag-helper-attributes"></a><span data-ttu-id="0c7c6-112">Atributos del asistente de etiquetas de caché distribuida</span><span class="sxs-lookup"><span data-stu-id="0c7c6-112">Distributed Cache Tag Helper Attributes</span></span>

### <a name="attributes-shared-with-the-cache-tag-helper"></a><span data-ttu-id="0c7c6-113">Atributos compartidos con el asistente de etiquetas de caché</span><span class="sxs-lookup"><span data-stu-id="0c7c6-113">Attributes shared with the Cache Tag Helper</span></span>

* `enabled`
* `expires-on`
* `expires-after`
* `expires-sliding`
* `vary-by-header`
* `vary-by-query`
* `vary-by-route`
* `vary-by-cookie`
* `vary-by-user`
* `vary-by priority`

<span data-ttu-id="0c7c6-114">El asistente de etiquetas de caché distribuida hereda de la misma clase que el asistente de etiquetas de caché.</span><span class="sxs-lookup"><span data-stu-id="0c7c6-114">The Distributed Cache Tag Helper inherits from the same class as Cache Tag Helper.</span></span> <span data-ttu-id="0c7c6-115">Para obtener descripciones de estos atributos, vea el [asistente de etiquetas de caché](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="0c7c6-115">For descriptions of these attributes, see the [Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span></span>

### <a name="name"></a><span data-ttu-id="0c7c6-116">Nombre</span><span class="sxs-lookup"><span data-stu-id="0c7c6-116">name</span></span>

| <span data-ttu-id="0c7c6-117">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="0c7c6-117">Attribute Type</span></span> | <span data-ttu-id="0c7c6-118">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="0c7c6-118">Example</span></span>                               |
| -------------- | ------------------------------------- |
| <span data-ttu-id="0c7c6-119">Cadena</span><span class="sxs-lookup"><span data-stu-id="0c7c6-119">String</span></span>         | `my-distributed-cache-unique-key-101` |

<span data-ttu-id="0c7c6-120">`name` es obligatorio.</span><span class="sxs-lookup"><span data-stu-id="0c7c6-120">`name` is required.</span></span> <span data-ttu-id="0c7c6-121">El atributo `name` se usa como clave para cada instancia de caché almacenada.</span><span class="sxs-lookup"><span data-stu-id="0c7c6-121">The `name` attribute is used as a key for each stored cache instance.</span></span> <span data-ttu-id="0c7c6-122">A diferencia del asistente de etiquetas de caché, que asigna una clave de caché a cada instancia en función del nombre de la página de Razor y la ubicación en la página de Razor, el asistente de etiquetas de caché distribuida solo basa su clave en el atributo `name`.</span><span class="sxs-lookup"><span data-stu-id="0c7c6-122">Unlike the Cache Tag Helper that assigns a cache key to each instance based on the Razor page name and location in the Razor page, the Distributed Cache Tag Helper only bases its key on the attribute `name`.</span></span>

<span data-ttu-id="0c7c6-123">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="0c7c6-123">Example:</span></span>

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a><span data-ttu-id="0c7c6-124">Implementaciones de IDistributedCache del asistente de etiquetas de caché distribuida</span><span class="sxs-lookup"><span data-stu-id="0c7c6-124">Distributed Cache Tag Helper IDistributedCache implementations</span></span>

<span data-ttu-id="0c7c6-125">Hay dos implementaciones de <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> integradas en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0c7c6-125">There are two implementations of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> built in to ASP.NET Core.</span></span> <span data-ttu-id="0c7c6-126">Una se basa en SQL Server y la otra en Redis.</span><span class="sxs-lookup"><span data-stu-id="0c7c6-126">One is based on SQL Server, and the other is based on Redis.</span></span> <span data-ttu-id="0c7c6-127">En <xref:performance/caching/distributed> encontrará detalles de estas implementaciones.</span><span class="sxs-lookup"><span data-stu-id="0c7c6-127">Details of these implementations can be found at <xref:performance/caching/distributed>.</span></span> <span data-ttu-id="0c7c6-128">Para ambas implementaciones hay que establecer una instancia de `IDistributedCache` en `Startup`.</span><span class="sxs-lookup"><span data-stu-id="0c7c6-128">Both implementations involve setting an instance of `IDistributedCache` in `Startup`.</span></span>

<span data-ttu-id="0c7c6-129">No hay atributos de etiqueta asociados específicamente con el uso de implementaciones concretas de `IDistributedCache`.</span><span class="sxs-lookup"><span data-stu-id="0c7c6-129">There are no tag attributes specifically associated with using any specific implementation of `IDistributedCache`.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0c7c6-130">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="0c7c6-130">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
