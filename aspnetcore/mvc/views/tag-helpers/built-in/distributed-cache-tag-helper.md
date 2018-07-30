---
title: Aplicación auxiliar de etiquetas de caché distribuida en ASP.NET Core
author: pkellner
description: Muestra cómo trabajar con la aplicación auxiliar de etiqueta de caché
ms.author: riande
ms.date: 02/14/2017
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: a35a5795c086273e773c613c483fc6343c694bf2
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342177"
---
# <a name="distributed-cache-tag-helper-in-aspnet-core"></a><span data-ttu-id="2f196-103">Aplicación auxiliar de etiquetas de caché distribuida en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2f196-103">Distributed Cache Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="2f196-104">Por [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="2f196-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="2f196-105">La aplicación auxiliar de etiquetas de caché distribuida proporciona la capacidad de mejorar drásticamente el rendimiento de la aplicación ASP.NET Core al permitir almacenar en caché su contenido en un origen de caché distribuida.</span><span class="sxs-lookup"><span data-stu-id="2f196-105">The Distributed Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to a distributed cache source.</span></span>

<span data-ttu-id="2f196-106">La aplicación auxiliar de etiquetas de caché distribuida hereda de la misma clase base que la aplicación auxiliar de etiquetas de caché.</span><span class="sxs-lookup"><span data-stu-id="2f196-106">The Distributed Cache Tag Helper inherits from the same base class as the Cache Tag Helper.</span></span> <span data-ttu-id="2f196-107">Todos los atributos asociados a la aplicación auxiliar de etiquetas de caché también funcionarán en la aplicación auxiliar de etiquetas de caché distribuida.</span><span class="sxs-lookup"><span data-stu-id="2f196-107">All attributes associated with the Cache Tag Helper will also work on the Distributed Tag Helper.</span></span>

<span data-ttu-id="2f196-108">La aplicación auxiliar de etiquetas de caché distribuida sigue el **principio de dependencias explícitas** conocido como **inserción de constructores**.</span><span class="sxs-lookup"><span data-stu-id="2f196-108">The Distributed Cache Tag Helper follows the **Explicit Dependencies Principle** known as **Constructor Injection**.</span></span> <span data-ttu-id="2f196-109">En concreto, el contenedor de interfaz `IDistributedCache` se pasa al constructor de la aplicación auxiliar de etiquetas de caché distribuida.</span><span class="sxs-lookup"><span data-stu-id="2f196-109">Specifically, the `IDistributedCache` interface container is passed into the Distributed Cache Tag Helper's constructor.</span></span> <span data-ttu-id="2f196-110">Si no se ha creado ninguna implementación específica de `IDistributedCache` en `ConfigureServices`, que normalmente se encuentra en startup.cs, la aplicación auxiliar de etiquetas de caché distribuida usará el mismo proveedor en memoria para almacenar datos en caché que la aplicación auxiliar de etiquetas de caché básica.</span><span class="sxs-lookup"><span data-stu-id="2f196-110">If no specific concrete implementation of `IDistributedCache` has been created in `ConfigureServices`, usually found in startup.cs, then the Distributed Cache Tag Helper will use the same in-memory provider for storing cached data as the basic Cache Tag Helper.</span></span>

## <a name="distributed-cache-tag-helper-attributes"></a><span data-ttu-id="2f196-111">Atributos de la aplicación auxiliar de etiquetas de caché distribuida</span><span class="sxs-lookup"><span data-stu-id="2f196-111">Distributed Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled-expires-on-expires-after-expires-sliding-vary-by-header-vary-by-query-vary-by-route-vary-by-cookie-vary-by-user-vary-by-priority"></a><span data-ttu-id="2f196-112">enabled expires-on expires-after expires-sliding vary-by-header vary-by-query vary-by-route vary-by-cookie vary-by-user vary-by priority</span><span class="sxs-lookup"><span data-stu-id="2f196-112">enabled expires-on expires-after expires-sliding vary-by-header vary-by-query vary-by-route vary-by-cookie vary-by-user vary-by priority</span></span>

<span data-ttu-id="2f196-113">Vea la aplicación auxiliar de etiquetas de caché para obtener las definiciones.</span><span class="sxs-lookup"><span data-stu-id="2f196-113">See Cache Tag Helper for definitions.</span></span> <span data-ttu-id="2f196-114">La aplicación auxiliar de etiquetas de caché distribuida hereda de la misma clase que la aplicación auxiliar de etiquetas de caché, de modo que todos estos atributos son iguales que los de la aplicación auxiliar de etiquetas de caché.</span><span class="sxs-lookup"><span data-stu-id="2f196-114">Distributed Cache Tag Helper inherits from the same class as Cache Tag Helper so all these attributes are common from Cache Tag Helper.</span></span>

- - -

### <a name="name-required"></a><span data-ttu-id="2f196-115">Atributo name (obligatorio)</span><span class="sxs-lookup"><span data-stu-id="2f196-115">name (required)</span></span>

| <span data-ttu-id="2f196-116">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="2f196-116">Attribute Type</span></span>    | <span data-ttu-id="2f196-117">Valor de ejemplo</span><span class="sxs-lookup"><span data-stu-id="2f196-117">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="2f196-118">cadena</span><span class="sxs-lookup"><span data-stu-id="2f196-118">string</span></span>    | <span data-ttu-id="2f196-119">"my-distributed-cache-unique-key-101"</span><span class="sxs-lookup"><span data-stu-id="2f196-119">"my-distributed-cache-unique-key-101"</span></span>     |

<span data-ttu-id="2f196-120">El atributo `name` obligatorio se usa como clave de la caché almacenada para cada instancia de una aplicación auxiliar de etiquetas de caché distribuida.</span><span class="sxs-lookup"><span data-stu-id="2f196-120">The required `name` attribute is used as a key to that cache stored for each instance of a Distributed Cache Tag Helper.</span></span> <span data-ttu-id="2f196-121">A diferencia de la aplicación auxiliar de etiquetas de caché básica, que asigna una clave a cada instancia de la aplicación auxiliar de etiquetas de caché en función del nombre de la página de Razor y la ubicación de la aplicación auxiliar de etiquetas en la página de Razor, la aplicación auxiliar de etiquetas de caché distribuida solo basa su clave en el atributo `name`.</span><span class="sxs-lookup"><span data-stu-id="2f196-121">Unlike the basic Cache Tag Helper that assigns a key to each Cache Tag Helper instance based on the Razor page name and location of the Tag Helper in the razor page, the Distributed Cache Tag Helper only bases its key on the attribute `name`</span></span>

<span data-ttu-id="2f196-122">Ejemplo de uso:</span><span class="sxs-lookup"><span data-stu-id="2f196-122">Usage Example:</span></span>

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a><span data-ttu-id="2f196-123">Implementaciones de IDistributedCache de la aplicación auxiliar de etiquetas de caché distribuida</span><span class="sxs-lookup"><span data-stu-id="2f196-123">Distributed Cache Tag Helper IDistributedCache implementations</span></span>

<span data-ttu-id="2f196-124">Hay dos implementaciones de `IDistributedCache` integradas en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2f196-124">There are two implementations of `IDistributedCache` built in to ASP.NET Core.</span></span> <span data-ttu-id="2f196-125">Una se basa en SQL Server y la otra, en Redis.</span><span class="sxs-lookup"><span data-stu-id="2f196-125">One is based on SQL Server and the other is based on Redis.</span></span> <span data-ttu-id="2f196-126">En <xref:performance/caching/distributed> encontrará detalles de estas implementaciones.</span><span class="sxs-lookup"><span data-stu-id="2f196-126">Details of these implementations can be found at <xref:performance/caching/distributed>.</span></span> <span data-ttu-id="2f196-127">Ambas implementaciones requieren establecer una instancia de `IDistributedCache` en el archivo *Startup.cs* de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2f196-127">Both implementations involve setting an instance of `IDistributedCache` in ASP.NET Core's *Startup.cs*.</span></span>

<span data-ttu-id="2f196-128">No hay atributos de etiqueta asociados específicamente con el uso de implementaciones concretas de `IDistributedCache`.</span><span class="sxs-lookup"><span data-stu-id="2f196-128">There are no tag attributes specifically associated with using any specific implementation of `IDistributedCache`.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2f196-129">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="2f196-129">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
