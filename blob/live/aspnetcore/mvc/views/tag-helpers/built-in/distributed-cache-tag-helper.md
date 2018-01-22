---
title: "Distribuye la aplicación auxiliar de etiqueta de caché | Documentos de Microsoft"
author: pkellner
description: "Muestra cómo trabajar con la aplicación auxiliar de etiqueta de caché"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: f5844dade218fdba1169a55fe3ce251a9cc03db2
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
# <a name="distributed-cache-tag-helper"></a><span data-ttu-id="30dd5-103">Aplicación auxiliar de etiqueta de caché distribuida</span><span class="sxs-lookup"><span data-stu-id="30dd5-103">Distributed Cache Tag Helper</span></span>

<span data-ttu-id="30dd5-104">Por [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="30dd5-104">By [Peter Kellner](http://peterkellner.net)</span></span> 


<span data-ttu-id="30dd5-105">El Ayudante de etiqueta de caché distribuida proporciona la capacidad para mejorar drásticamente el rendimiento de la aplicación de ASP.NET Core almacenando en memoria caché su contenido a un origen de caché distribuida.</span><span class="sxs-lookup"><span data-stu-id="30dd5-105">The Distributed Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to a distributed cache source.</span></span>

<span data-ttu-id="30dd5-106">El Ayudante de etiqueta de caché distribuida se hereda de la misma clase base que la aplicación auxiliar de etiqueta de caché.</span><span class="sxs-lookup"><span data-stu-id="30dd5-106">The Distributed Cache Tag Helper inherits from the same base class as the Cache Tag Helper.</span></span>  <span data-ttu-id="30dd5-107">Todos los atributos asociados a la aplicación auxiliar de etiqueta de caché también funcionará en la aplicación auxiliar distribuidas de etiqueta.</span><span class="sxs-lookup"><span data-stu-id="30dd5-107">All attributes associated with the Cache Tag Helper will also work on the Distributed Tag Helper.</span></span>


<span data-ttu-id="30dd5-108">El Ayudante de etiqueta de caché distribuida sigue la **principio de dependencias explícitas** conocido como **inyección de Constructor**.</span><span class="sxs-lookup"><span data-stu-id="30dd5-108">The Distributed Cache Tag Helper follows the **Explicit Dependencies Principle** known as **Constructor Injection**.</span></span>  <span data-ttu-id="30dd5-109">En concreto, la `IDistributedCache` contenedor de interfaz se haya pasado al constructor de la Distributed caché etiqueta del Ayudante.</span><span class="sxs-lookup"><span data-stu-id="30dd5-109">Specifically, the `IDistributedCache` interface container is passed into the Distributed Cache Tag Helper's constructor.</span></span>  <span data-ttu-id="30dd5-110">Si ninguna implementación concreta específica de `IDistributedCache` se ha creado en `ConfigureServices`, normalmente se encuentran en startup.cs, a continuación, el Ayudante de etiqueta de caché distribuida utilizará el mismo proveedor en memoria para almacenar datos en caché como el Ayudante de etiqueta de caché básico.</span><span class="sxs-lookup"><span data-stu-id="30dd5-110">If no specific concrete implementation of `IDistributedCache` has been created in `ConfigureServices`, usually found in startup.cs, then the Distributed Cache Tag Helper will use the same in-memory provider for storing cached data as the basic Cache Tag Helper.</span></span>

## <a name="distributed-cache-tag-helper-attributes"></a><span data-ttu-id="30dd5-111">Distribuye los atributos de aplicación auxiliar de etiqueta de caché</span><span class="sxs-lookup"><span data-stu-id="30dd5-111">Distributed Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled-expires-on-expires-after-expires-sliding-vary-by-header-vary-by-query-vary-by-route-vary-by-cookie-vary-by-user-vary-by-priority"></a><span data-ttu-id="30dd5-112">habilitado expira la sesión expira después expira-deslizante por encabezado vary variar por consulta varían por ruta varían cookie varían por usuario varían por prioridad</span><span class="sxs-lookup"><span data-stu-id="30dd5-112">enabled expires-on expires-after expires-sliding vary-by-header vary-by-query vary-by-route vary-by-cookie vary-by-user vary-by priority</span></span>

<span data-ttu-id="30dd5-113">Vea auxiliar de etiqueta de caché para las definiciones.</span><span class="sxs-lookup"><span data-stu-id="30dd5-113">See Cache Tag Helper for definitions.</span></span> <span data-ttu-id="30dd5-114">Auxiliar de etiqueta de caché distribuida se hereda de la misma clase que la aplicación auxiliar de etiqueta de caché para que todos estos atributos son comunes de aplicación auxiliar de etiqueta de caché.</span><span class="sxs-lookup"><span data-stu-id="30dd5-114">Distributed Cache Tag Helper inherits from the same class as Cache Tag Helper so all these attributes are common from Cache Tag Helper.</span></span>

- - -

### <a name="name-required"></a><span data-ttu-id="30dd5-115">nombre (obligatorio)</span><span class="sxs-lookup"><span data-stu-id="30dd5-115">name (required)</span></span>

| <span data-ttu-id="30dd5-116">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="30dd5-116">Attribute Type</span></span>    | <span data-ttu-id="30dd5-117">Valor de ejemplo</span><span class="sxs-lookup"><span data-stu-id="30dd5-117">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="30dd5-118">cadena</span><span class="sxs-lookup"><span data-stu-id="30dd5-118">string</span></span>    | <span data-ttu-id="30dd5-119">"my-distributed-cache-unique-key-101"</span><span class="sxs-lookup"><span data-stu-id="30dd5-119">"my-distributed-cache-unique-key-101"</span></span>     |

<span data-ttu-id="30dd5-120">Requerido `name` atributo se utiliza como clave para esa caché almacenada para cada instancia de un Ayudante de etiqueta de caché distribuida.</span><span class="sxs-lookup"><span data-stu-id="30dd5-120">The required `name` attribute is used as a key to that cache stored for each instance of a Distributed Cache Tag Helper.</span></span>  <span data-ttu-id="30dd5-121">Al contrario que el Ayudante básica de la etiqueta de caché que asigna una clave a cada instancia de aplicación auxiliar de etiqueta de caché según el nombre de la página de Razor y la ubicación de la aplicación auxiliar de etiquetas en la página de razor, el Ayudante de etiqueta de caché distribuida solo basa su clave en el atributo`name`</span><span class="sxs-lookup"><span data-stu-id="30dd5-121">Unlike the basic Cache Tag Helper that assigns a key to each Cache Tag Helper instance based on the Razor page name and location of the tag helper in the razor page, the Distributed Cache Tag Helper only bases it's key on the attribute `name`</span></span>

<span data-ttu-id="30dd5-122">Ejemplo de uso:</span><span class="sxs-lookup"><span data-stu-id="30dd5-122">Usage Example:</span></span>

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a><span data-ttu-id="30dd5-123">Distribuye las implementaciones de caché etiqueta auxiliar IDistributedCache</span><span class="sxs-lookup"><span data-stu-id="30dd5-123">Distributed Cache Tag Helper IDistributedCache Implementations</span></span>

<span data-ttu-id="30dd5-124">Hay dos implementaciones de `IDistributedCache` integrada en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="30dd5-124">There are two implementations of `IDistributedCache` built in to ASP.NET Core.</span></span>  <span data-ttu-id="30dd5-125">Uno se basa en **Sql Server** y la otra se basa en **Redis**.</span><span class="sxs-lookup"><span data-stu-id="30dd5-125">One is based on **Sql Server** and the other is based on **Redis**.</span></span> <span data-ttu-id="30dd5-126">Detalles de estas implementaciones se pueden encontrar en el recurso al que hace referencia a continuación con nombre "trabajar con una memoria caché distribuida".</span><span class="sxs-lookup"><span data-stu-id="30dd5-126">Details of these implementations can be found at the resource referenced below named "Working with a distributed cache".</span></span> <span data-ttu-id="30dd5-127">Ambas implementaciones implican establecer una instancia de `IDistributedCache` en ASP.NET Core **startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="30dd5-127">Both implementations involve setting an instance of `IDistributedCache` in ASP.NET Core's **startup.cs**.</span></span>

<span data-ttu-id="30dd5-128">No hay ningún atributo de etiqueta están asociado específicamente con el uso de cualquier implementación específica de `IDistributedCache`.</span><span class="sxs-lookup"><span data-stu-id="30dd5-128">There are no tag attributes specifically associated with using any specific implementation of `IDistributedCache`.</span></span>



- - -



## <a name="additional-resources"></a><span data-ttu-id="30dd5-129">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="30dd5-129">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
