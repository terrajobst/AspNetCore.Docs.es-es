---
title: "Distribuye la aplicación auxiliar de etiqueta de caché | Documentos de Microsoft"
author: pkellner
description: "Muestra cómo trabajar con la aplicación auxiliar de etiqueta de caché"
keywords: "Núcleo de ASP.NET, aplicación auxiliar de etiqueta"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: c045d485-d1dc-4cea-a675-46be83b7a022
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/DistributedCacheTagHelper
ms.openlocfilehash: 2b260624fb2d85ab1a2625511397bcb4a85b6e77
ms.sourcegitcommit: d022d4b96795ee473fa3847a1d8a8c7430423a86
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2017
---
# <a name="distributed-cache-tag-helper"></a><span data-ttu-id="b60d0-104">Aplicación auxiliar de etiqueta de caché distribuida</span><span class="sxs-lookup"><span data-stu-id="b60d0-104">Distributed Cache Tag Helper</span></span>

<span data-ttu-id="b60d0-105">Por [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="b60d0-105">By [Peter Kellner](http://peterkellner.net)</span></span> 


<span data-ttu-id="b60d0-106">El Ayudante de etiqueta de caché distribuida proporciona la capacidad para mejorar drásticamente el rendimiento de la aplicación de ASP.NET Core almacenando en memoria caché su contenido a un origen de caché distribuida.</span><span class="sxs-lookup"><span data-stu-id="b60d0-106">The Distributed Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to a distributed cache source.</span></span>

<span data-ttu-id="b60d0-107">El Ayudante de etiqueta de caché distribuida se hereda de la misma clase base que la aplicación auxiliar de etiqueta de caché.</span><span class="sxs-lookup"><span data-stu-id="b60d0-107">The Distributed Cache Tag Helper inherits from the same base class as the Cache Tag Helper.</span></span>  <span data-ttu-id="b60d0-108">Todos los atributos asociados a la aplicación auxiliar de etiqueta de caché también funcionará en la aplicación auxiliar distribuidas de etiqueta.</span><span class="sxs-lookup"><span data-stu-id="b60d0-108">All attributes associated with the Cache Tag Helper will also work on the Distributed Tag Helper.</span></span>


<span data-ttu-id="b60d0-109">El Ayudante de etiqueta de caché distribuida sigue la **principio de dependencias explícitas** conocido como **inyección de Constructor**.</span><span class="sxs-lookup"><span data-stu-id="b60d0-109">The Distributed Cache Tag Helper follows the **Explicit Dependencies Principle** known as **Constructor Injection**.</span></span>  <span data-ttu-id="b60d0-110">En concreto, la `IDistributedCache` contenedor de interfaz se haya pasado al constructor de la Distributed caché etiqueta del Ayudante.</span><span class="sxs-lookup"><span data-stu-id="b60d0-110">Specifically, the `IDistributedCache` interface container is passed into the Distributed Cache Tag Helper's constructor.</span></span>  <span data-ttu-id="b60d0-111">Si ninguna implementación concreta específica de `IDistributedCache` se ha creado en `ConfigureServices`, normalmente se encuentran en startup.cs, a continuación, el Ayudante de etiqueta de caché distribuida utilizará el mismo proveedor en memoria para almacenar datos en caché como el Ayudante de etiqueta de caché básico.</span><span class="sxs-lookup"><span data-stu-id="b60d0-111">If no specific concrete implementation of `IDistributedCache` has been created in `ConfigureServices`, usually found in startup.cs, then the Distributed Cache Tag Helper will use the same in-memory provider for storing cached data as the basic Cache Tag Helper.</span></span>

## <a name="distributed-cache-tag-helper-attributes"></a><span data-ttu-id="b60d0-112">Distribuye los atributos de aplicación auxiliar de etiqueta de caché</span><span class="sxs-lookup"><span data-stu-id="b60d0-112">Distributed Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled-expires-on-expires-after-expires-sliding-vary-by-header-vary-by-query-vary-by-route-vary-by-cookie-vary-by-user-vary-by-priority"></a><span data-ttu-id="b60d0-113">habilitado expira la sesión expira después expira-deslizante por encabezado vary variar por consulta varían por ruta varían cookie varían por usuario varían por prioridad</span><span class="sxs-lookup"><span data-stu-id="b60d0-113">enabled expires-on expires-after expires-sliding vary-by-header vary-by-query vary-by-route vary-by-cookie vary-by-user vary-by priority</span></span>

<span data-ttu-id="b60d0-114">Vea auxiliar de etiqueta de caché para las definiciones.</span><span class="sxs-lookup"><span data-stu-id="b60d0-114">See Cache Tag Helper for definitions.</span></span> <span data-ttu-id="b60d0-115">Auxiliar de etiqueta de caché distribuida se hereda de la misma clase que la aplicación auxiliar de etiqueta de caché para que todos estos atributos son comunes de aplicación auxiliar de etiqueta de caché.</span><span class="sxs-lookup"><span data-stu-id="b60d0-115">Distributed Cache Tag Helper inherits from the same class as Cache Tag Helper so all these attributes are common from Cache Tag Helper.</span></span>

- - -

### <a name="name-required"></a><span data-ttu-id="b60d0-116">nombre (obligatorio)</span><span class="sxs-lookup"><span data-stu-id="b60d0-116">name (required)</span></span>

| <span data-ttu-id="b60d0-117">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="b60d0-117">Attribute Type</span></span>    | <span data-ttu-id="b60d0-118">Valor de ejemplo</span><span class="sxs-lookup"><span data-stu-id="b60d0-118">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="b60d0-119">string</span><span class="sxs-lookup"><span data-stu-id="b60d0-119">string</span></span>    | <span data-ttu-id="b60d0-120">"my-distributed-cache-unique-key-101"</span><span class="sxs-lookup"><span data-stu-id="b60d0-120">"my-distributed-cache-unique-key-101"</span></span>     |

<span data-ttu-id="b60d0-121">Requerido `name` atributo se utiliza como clave para esa caché almacenada para cada instancia de un Ayudante de etiqueta de caché distribuida.</span><span class="sxs-lookup"><span data-stu-id="b60d0-121">The required `name` attribute is used as a key to that cache stored for each instance of a Distributed Cache Tag Helper.</span></span>  <span data-ttu-id="b60d0-122">Al contrario que el Ayudante básica de la etiqueta de caché que asigna una clave a cada instancia de aplicación auxiliar de etiqueta de caché según el nombre de la página de Razor y la ubicación de la aplicación auxiliar de etiquetas en la página de razor, el Ayudante de etiqueta de caché distribuida solo basa su clave en el atributo`name`</span><span class="sxs-lookup"><span data-stu-id="b60d0-122">Unlike the basic Cache Tag Helper that assigns a key to each Cache Tag Helper instance based on the Razor page name and location of the tag helper in the razor page, the Distributed Cache Tag Helper only bases it's key on the attribute `name`</span></span>

<span data-ttu-id="b60d0-123">Ejemplo de uso:</span><span class="sxs-lookup"><span data-stu-id="b60d0-123">Usage Example:</span></span>

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a><span data-ttu-id="b60d0-124">Distribuye las implementaciones de caché etiqueta auxiliar IDistributedCache</span><span class="sxs-lookup"><span data-stu-id="b60d0-124">Distributed Cache Tag Helper IDistributedCache Implementations</span></span>

<span data-ttu-id="b60d0-125">Hay dos implementaciones de `IDistributedCache` integrada en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b60d0-125">There are two implementations of `IDistributedCache` built in to ASP.NET Core.</span></span>  <span data-ttu-id="b60d0-126">Uno se basa en **Sql Server** y la otra se basa en **Redis**.</span><span class="sxs-lookup"><span data-stu-id="b60d0-126">One is based on **Sql Server** and the other is based on **Redis**.</span></span> <span data-ttu-id="b60d0-127">Detalles de estas implementaciones se pueden encontrar en el recurso al que hace referencia a continuación con nombre "trabajar con una memoria caché distribuida".</span><span class="sxs-lookup"><span data-stu-id="b60d0-127">Details of these implementations can be found at the resource referenced below named "Working with a distributed cache".</span></span> <span data-ttu-id="b60d0-128">Ambas implementaciones implican establecer una instancia de `IDistributedCache` en ASP.NET Core **startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="b60d0-128">Both implementations involve setting an instance of `IDistributedCache` in ASP.NET Core's **startup.cs**.</span></span>

<span data-ttu-id="b60d0-129">No hay ningún atributo de etiqueta están asociado específicamente con el uso de cualquier implementación específica de `IDistributedCache`.</span><span class="sxs-lookup"><span data-stu-id="b60d0-129">There are no tag attributes specifically associated with using any specific implementation of `IDistributedCache`.</span></span>



- - -



## <a name="additional-resources"></a><span data-ttu-id="b60d0-130">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="b60d0-130">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/builtin-th/CacheTagHelper>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
