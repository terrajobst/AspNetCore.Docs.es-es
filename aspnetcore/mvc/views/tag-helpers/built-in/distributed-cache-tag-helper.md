---
title: Asistente de etiquetas de caché distribuida en ASP.NET Core
author: pkellner
description: Obtenga información sobre cómo usar el asistente de etiquetas de caché distribuida.
ms.author: riande
ms.date: 02/14/2017
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: 1b51164a6d3dab2eeaf64262d6f0d9961bd00d12
ms.sourcegitcommit: 4d5f8680d68b39c411b46c73f7014f8aa0f12026
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "47028101"
---
# <a name="distributed-cache-tag-helper-in-aspnet-core"></a><span data-ttu-id="4ca81-103">Asistente de etiquetas de caché distribuida en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4ca81-103">Distributed Cache Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="4ca81-104">Por [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="4ca81-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="4ca81-105">El asistente de etiquetas de caché distribuida proporciona la capacidad de mejorar drásticamente el rendimiento de la aplicación ASP.NET Core al permitir almacenar en caché su contenido en un origen de caché distribuida.</span><span class="sxs-lookup"><span data-stu-id="4ca81-105">The Distributed Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to a distributed cache source.</span></span>

<span data-ttu-id="4ca81-106">El asistente de etiquetas de caché distribuida hereda de la misma clase base que el asistente de etiquetas de caché.</span><span class="sxs-lookup"><span data-stu-id="4ca81-106">The Distributed Cache Tag Helper inherits from the same base class as the Cache Tag Helper.</span></span> <span data-ttu-id="4ca81-107">Todos los atributos asociados al asistente de etiquetas de caché también funcionarán en el asistente de etiquetas de caché distribuida.</span><span class="sxs-lookup"><span data-stu-id="4ca81-107">All attributes associated with the Cache Tag Helper will also work on the Distributed Tag Helper.</span></span>

<span data-ttu-id="4ca81-108">El asistente de etiquetas de caché distribuida sigue el **principio de dependencias explícitas** conocido como **inserción de constructores**.</span><span class="sxs-lookup"><span data-stu-id="4ca81-108">The Distributed Cache Tag Helper follows the **Explicit Dependencies Principle** known as **Constructor Injection**.</span></span> <span data-ttu-id="4ca81-109">En concreto, el contenedor de interfaz `IDistributedCache` se pasa al constructor del asistente de etiquetas de caché distribuida.</span><span class="sxs-lookup"><span data-stu-id="4ca81-109">Specifically, the `IDistributedCache` interface container is passed into the Distributed Cache Tag Helper's constructor.</span></span> <span data-ttu-id="4ca81-110">Si no se ha creado ninguna implementación específica de `IDistributedCache` en `ConfigureServices`, que normalmente se encuentra en startup.cs, el asistente de etiquetas de caché distribuida usará el mismo proveedor en memoria para almacenar datos en caché que el asistente de etiquetas de caché básica.</span><span class="sxs-lookup"><span data-stu-id="4ca81-110">If no specific concrete implementation of `IDistributedCache` has been created in `ConfigureServices`, usually found in startup.cs, then the Distributed Cache Tag Helper will use the same in-memory provider for storing cached data as the basic Cache Tag Helper.</span></span>

## <a name="distributed-cache-tag-helper-attributes"></a><span data-ttu-id="4ca81-111">Atributos del asistente de etiquetas de caché distribuida</span><span class="sxs-lookup"><span data-stu-id="4ca81-111">Distributed Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled-expires-on-expires-after-expires-sliding-vary-by-header-vary-by-query-vary-by-route-vary-by-cookie-vary-by-user-vary-by-priority"></a><span data-ttu-id="4ca81-112">enabled expires-on expires-after expires-sliding vary-by-header vary-by-query vary-by-route vary-by-cookie vary-by-user vary-by priority</span><span class="sxs-lookup"><span data-stu-id="4ca81-112">enabled expires-on expires-after expires-sliding vary-by-header vary-by-query vary-by-route vary-by-cookie vary-by-user vary-by priority</span></span>

<span data-ttu-id="4ca81-113">Vea el asistente de etiquetas de caché para obtener las definiciones.</span><span class="sxs-lookup"><span data-stu-id="4ca81-113">See Cache Tag Helper for definitions.</span></span> <span data-ttu-id="4ca81-114">El asistente de etiquetas de caché distribuida hereda de la misma clase que el asistente de etiquetas de caché, de modo que todos estos atributos son iguales que los del asistente de etiquetas de caché.</span><span class="sxs-lookup"><span data-stu-id="4ca81-114">Distributed Cache Tag Helper inherits from the same class as Cache Tag Helper so all these attributes are common from Cache Tag Helper.</span></span>

- - -

### <a name="name-required"></a><span data-ttu-id="4ca81-115">Atributo name (obligatorio)</span><span class="sxs-lookup"><span data-stu-id="4ca81-115">name (required)</span></span>

| <span data-ttu-id="4ca81-116">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="4ca81-116">Attribute Type</span></span>    | <span data-ttu-id="4ca81-117">Valor de ejemplo</span><span class="sxs-lookup"><span data-stu-id="4ca81-117">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="4ca81-118">cadena</span><span class="sxs-lookup"><span data-stu-id="4ca81-118">string</span></span>    | <span data-ttu-id="4ca81-119">"my-distributed-cache-unique-key-101"</span><span class="sxs-lookup"><span data-stu-id="4ca81-119">"my-distributed-cache-unique-key-101"</span></span>     |

<span data-ttu-id="4ca81-120">El atributo `name` obligatorio se usa como clave de la caché almacenada para cada instancia de un asistente de etiquetas de caché distribuida.</span><span class="sxs-lookup"><span data-stu-id="4ca81-120">The required `name` attribute is used as a key to that cache stored for each instance of a Distributed Cache Tag Helper.</span></span> <span data-ttu-id="4ca81-121">A diferencia del asistente de etiquetas de caché básica, que asigna una clave a cada instancia del asistente de etiquetas de caché en función del nombre de la página de Razor y la ubicación del asistente de etiquetas en la página de Razor, el asistente de etiquetas de caché distribuida solo basa su clave en el atributo `name`.</span><span class="sxs-lookup"><span data-stu-id="4ca81-121">Unlike the basic Cache Tag Helper that assigns a key to each Cache Tag Helper instance based on the Razor page name and location of the Tag Helper in the razor page, the Distributed Cache Tag Helper only bases its key on the attribute `name`</span></span>

<span data-ttu-id="4ca81-122">Ejemplo de uso:</span><span class="sxs-lookup"><span data-stu-id="4ca81-122">Usage Example:</span></span>

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a><span data-ttu-id="4ca81-123">Implementaciones de IDistributedCache del asistente de etiquetas de caché distribuida</span><span class="sxs-lookup"><span data-stu-id="4ca81-123">Distributed Cache Tag Helper IDistributedCache implementations</span></span>

<span data-ttu-id="4ca81-124">Hay dos implementaciones de `IDistributedCache` integradas en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4ca81-124">There are two implementations of `IDistributedCache` built in to ASP.NET Core.</span></span> <span data-ttu-id="4ca81-125">Una se basa en SQL Server y la otra, en Redis.</span><span class="sxs-lookup"><span data-stu-id="4ca81-125">One is based on SQL Server and the other is based on Redis.</span></span> <span data-ttu-id="4ca81-126">En <xref:performance/caching/distributed> encontrará detalles de estas implementaciones.</span><span class="sxs-lookup"><span data-stu-id="4ca81-126">Details of these implementations can be found at <xref:performance/caching/distributed>.</span></span> <span data-ttu-id="4ca81-127">Ambas implementaciones requieren establecer una instancia de `IDistributedCache` en el archivo *Startup.cs* de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4ca81-127">Both implementations involve setting an instance of `IDistributedCache` in ASP.NET Core's *Startup.cs*.</span></span>

<span data-ttu-id="4ca81-128">No hay atributos de etiqueta asociados específicamente con el uso de implementaciones concretas de `IDistributedCache`.</span><span class="sxs-lookup"><span data-stu-id="4ca81-128">There are no tag attributes specifically associated with using any specific implementation of `IDistributedCache`.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4ca81-129">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="4ca81-129">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
