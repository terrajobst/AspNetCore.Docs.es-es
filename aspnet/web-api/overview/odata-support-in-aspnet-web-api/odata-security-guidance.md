---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
title: Guía de seguridad para ASP.NET Web API 2 OData | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 02/06/2013
ms.assetid: b91e6424-1544-4747-bd0b-d1f8418c9653
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
msc.type: authoredcontent
ms.openlocfilehash: 4ba53e15dab83368097a58ba4d0d2e46d113d1d2
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325723"
---
<a name="security-guidance-for-aspnet-web-api-2-odata"></a><span data-ttu-id="ec3dc-102">Guía de seguridad para ASP.NET Web API 2 OData</span><span class="sxs-lookup"><span data-stu-id="ec3dc-102">Security Guidance for ASP.NET Web API 2 OData</span></span>
====================
<span data-ttu-id="ec3dc-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ec3dc-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="ec3dc-104">En este tema se describe algunos de los problemas de seguridad que se deben considerar al exponer un conjunto de datos a través de OData.</span><span class="sxs-lookup"><span data-stu-id="ec3dc-104">This topic describes some of the security issues that you should consider when exposing a dataset through OData.</span></span>

## <a name="edm-security"></a><span data-ttu-id="ec3dc-105">Seguridad EDM</span><span class="sxs-lookup"><span data-stu-id="ec3dc-105">EDM Security</span></span>

<span data-ttu-id="ec3dc-106">La semántica de consulta se basa en el entity data model (EDM), no los tipos del modelo subyacente.</span><span class="sxs-lookup"><span data-stu-id="ec3dc-106">The query semantics are based on the entity data model (EDM), not the underlying model types.</span></span> <span data-ttu-id="ec3dc-107">Puede excluir una propiedad de EDM y no será visible para la consulta.</span><span class="sxs-lookup"><span data-stu-id="ec3dc-107">You can exclude a property from the EDM and it will not be visible to the query.</span></span> <span data-ttu-id="ec3dc-108">Por ejemplo, suponga que el modelo incluye un tipo de empleado con una propiedad de sueldo.</span><span class="sxs-lookup"><span data-stu-id="ec3dc-108">For example, suppose your model includes an Employee type with a Salary property.</span></span> <span data-ttu-id="ec3dc-109">Es posible que desee excluir de esta propiedad de EDM para ocultarlo de los clientes.</span><span class="sxs-lookup"><span data-stu-id="ec3dc-109">You might want to exclude this property from the EDM to hide it from clients.</span></span>

<span data-ttu-id="ec3dc-110">Hay dos maneras de excluye una propiedad de EDM.</span><span class="sxs-lookup"><span data-stu-id="ec3dc-110">There are two ways to exlude a property from the EDM.</span></span> <span data-ttu-id="ec3dc-111">Puede establecer el **[IgnoreDataMember]** atributo en la propiedad de la clase del modelo:</span><span class="sxs-lookup"><span data-stu-id="ec3dc-111">You can set the **[IgnoreDataMember]** attribute on the property in the model class:</span></span>

[!code-csharp[Main](odata-security-guidance/samples/sample1.cs)]

<span data-ttu-id="ec3dc-112">También puede quitar mediante programación la propiedad de EDM:</span><span class="sxs-lookup"><span data-stu-id="ec3dc-112">You can also remove the property from the EDM programmatically:</span></span>

[!code-csharp[Main](odata-security-guidance/samples/sample2.cs)]

## <a name="query-security"></a><span data-ttu-id="ec3dc-113">Seguridad de la consulta</span><span class="sxs-lookup"><span data-stu-id="ec3dc-113">Query Security</span></span>

<span data-ttu-id="ec3dc-114">Un cliente malintencionado o naive puede ser capaz de crear una consulta que tarda mucho en ejecutar.</span><span class="sxs-lookup"><span data-stu-id="ec3dc-114">A malicious or naive client may be able to construct a query that takes a very long time to execute.</span></span> <span data-ttu-id="ec3dc-115">En el peor de los casos esto puede interrumpir el acceso a su servicio.</span><span class="sxs-lookup"><span data-stu-id="ec3dc-115">In the worst case this can disrupt access to your service.</span></span>

<span data-ttu-id="ec3dc-116">El **[Queryable]** atributo es un filtro de acción que analiza, valida y aplica la consulta.</span><span class="sxs-lookup"><span data-stu-id="ec3dc-116">The **[Queryable]** attribute is an action filter that parses, validates, and applies the query.</span></span> <span data-ttu-id="ec3dc-117">El filtro convierte las opciones de consulta en una expresión LINQ.</span><span class="sxs-lookup"><span data-stu-id="ec3dc-117">The filter converts the query options into a LINQ expression.</span></span> <span data-ttu-id="ec3dc-118">Cuando se devuelve el controlador de OData un **IQueryable** tipo, el **IQueryable** proveedor LINQ convierte la expresión LINQ en una consulta.</span><span class="sxs-lookup"><span data-stu-id="ec3dc-118">When the OData controller returns an **IQueryable** type, the **IQueryable** LINQ provider converts the LINQ expression into a query.</span></span> <span data-ttu-id="ec3dc-119">Por lo tanto, el rendimiento depende en el proveedor LINQ que se usa y también en las características específicas de su esquema de conjunto de datos o base de datos.</span><span class="sxs-lookup"><span data-stu-id="ec3dc-119">Therefore, performance depends on the LINQ provider that is used, and also on the particular characteristics of your dataset or database schema.</span></span>

<span data-ttu-id="ec3dc-120">Para obtener más información sobre el uso de las opciones de consulta de OData en ASP.NET Web API, consulte [que admiten opciones de consulta de OData](supporting-odata-query-options.md).</span><span class="sxs-lookup"><span data-stu-id="ec3dc-120">For more information about using OData query options in ASP.NET Web API, see [Supporting OData Query Options](supporting-odata-query-options.md).</span></span>

<span data-ttu-id="ec3dc-121">Si sabe que todos los clientes son de confianza (por ejemplo, en un entorno empresarial), o si el conjunto de datos es pequeño, rendimiento de las consultas no puede ser un problema.</span><span class="sxs-lookup"><span data-stu-id="ec3dc-121">If you know that all clients are trusted (for example, in an enterprise environment), or if your dataset is small, query performance might not be an issue.</span></span> <span data-ttu-id="ec3dc-122">De lo contrario, considere las siguientes recomendaciones.</span><span class="sxs-lookup"><span data-stu-id="ec3dc-122">Otherwise, you should consider the following recommendations.</span></span>

- <span data-ttu-id="ec3dc-123">Probar el servicio con varias consultas y generar perfiles de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="ec3dc-123">Test your service with various queries and profile the DB.</span></span>
- <span data-ttu-id="ec3dc-124">Habilitar la paginación controlada por servidor evitar la devolución de un gran conjunto de datos en una consulta.</span><span class="sxs-lookup"><span data-stu-id="ec3dc-124">Enable server-driven paging, to avoid returning a large data set in one query.</span></span> <span data-ttu-id="ec3dc-125">Para obtener más información, consulte [Server-Driven paginación](supporting-odata-query-options.md#server-paging).</span><span class="sxs-lookup"><span data-stu-id="ec3dc-125">For more information, see [Server-Driven Paging](supporting-odata-query-options.md#server-paging).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample3.cs)]
- <span data-ttu-id="ec3dc-126">¿Necesita con $filter y $orderby?</span><span class="sxs-lookup"><span data-stu-id="ec3dc-126">Do you need $filter and $orderby?</span></span> <span data-ttu-id="ec3dc-127">Algunas aplicaciones podrían permitir que el cliente de paginación, usando $top y $skip, pero deshabilitar las otras opciones de consulta.</span><span class="sxs-lookup"><span data-stu-id="ec3dc-127">Some applications might allow client paging, using $top and $skip, but disable the other query options.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample4.cs)]
- <span data-ttu-id="ec3dc-128">Considere la posibilidad de restringir $orderby a las propiedades de un índice agrupado.</span><span class="sxs-lookup"><span data-stu-id="ec3dc-128">Consider restricting $orderby to properties in a clustered index.</span></span> <span data-ttu-id="ec3dc-129">Ordenar datos de gran tamaño sin un índice agrupado es lento.</span><span class="sxs-lookup"><span data-stu-id="ec3dc-129">Sorting large data without a clustered index is slow.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample5.cs)]
- <span data-ttu-id="ec3dc-130">Número máximo de nodos: el **MaxNodeCount** propiedad **[Queryable]** establece los nodos de número máximos permitidos en el árbol de sintaxis $filter.</span><span class="sxs-lookup"><span data-stu-id="ec3dc-130">Maximum node count: The **MaxNodeCount** property on **[Queryable]** sets the maximum number nodes allowed in the $filter syntax tree.</span></span> <span data-ttu-id="ec3dc-131">El valor predeterminado es 100, pero es posible que desea establecer un valor inferior, porque un gran número de nodos puede ser lento para compilar.</span><span class="sxs-lookup"><span data-stu-id="ec3dc-131">The default value is 100, but you may want to set a lower value, because a large number of nodes can be slow to compile.</span></span> <span data-ttu-id="ec3dc-132">Esto es especialmente cierto si está utilizando LINQ to Objects (es decir, las consultas LINQ en una colección en memoria sin el uso de un proveedor LINQ intermedio).</span><span class="sxs-lookup"><span data-stu-id="ec3dc-132">This is particularly true if you are using LINQ to Objects (i.e., LINQ queries on a collection in memory, without the use of an intermediate LINQ provider).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample6.cs)]
- <span data-ttu-id="ec3dc-133">Considere la posibilidad de deshabilitar las funciones any() y all(), como pueden ser lentas.</span><span class="sxs-lookup"><span data-stu-id="ec3dc-133">Consider disabling the any() and all() functions, as these can be slow.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample7.cs)]
- <span data-ttu-id="ec3dc-134">Si las propiedades de cadena contienen cadenas grandes&#8212;por ejemplo, una descripción de producto o una entrada de blog&#8212;considerar la deshabilitación de las funciones de cadena.</span><span class="sxs-lookup"><span data-stu-id="ec3dc-134">If any string properties contain large strings&#8212;for example, a product description or a blog entry&#8212;consider disabling the string functions.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample8.cs)]
- <span data-ttu-id="ec3dc-135">Considere la posibilidad de no permitir el filtrado en las propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="ec3dc-135">Consider disallowing filtering on navigation properties.</span></span> <span data-ttu-id="ec3dc-136">Filtrado en las propiedades de navegación puede dar lugar a una combinación, que podría ser lenta, dependiendo de su esquema de base de datos.</span><span class="sxs-lookup"><span data-stu-id="ec3dc-136">Filtering on navigation properties can result in a join, which might be slow, depending on your database schema.</span></span> <span data-ttu-id="ec3dc-137">El código siguiente muestra un validador de consulta que impide que el filtrado en las propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="ec3dc-137">The following code shows a query validator that prevents filtering on navigation properties.</span></span> <span data-ttu-id="ec3dc-138">Para obtener más información acerca de los validadores de consulta, vea [validación de la consulta](supporting-odata-query-options.md#query-validation).</span><span class="sxs-lookup"><span data-stu-id="ec3dc-138">For more information about query validators, see [Query Validation](supporting-odata-query-options.md#query-validation).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample9.cs)]
- <span data-ttu-id="ec3dc-139">Considere la posibilidad de restringir las consultas $filter escribiendo un validador que se personaliza para la base de datos.</span><span class="sxs-lookup"><span data-stu-id="ec3dc-139">Consider restricting $filter queries by writing a validator that is customized for your database.</span></span> <span data-ttu-id="ec3dc-140">Por ejemplo, tenga en cuenta estas dos consultas:</span><span class="sxs-lookup"><span data-stu-id="ec3dc-140">For example, consider these two queries:</span></span> 

  - <span data-ttu-id="ec3dc-141">Todas las películas con actores cuyo apellido empieza por 'A'.</span><span class="sxs-lookup"><span data-stu-id="ec3dc-141">All movies with actors whose last name starts with ‘A'.</span></span>
  - <span data-ttu-id="ec3dc-142">Todas las películas publicadas en 1994.</span><span class="sxs-lookup"><span data-stu-id="ec3dc-142">All movies released in 1994.</span></span>

    <span data-ttu-id="ec3dc-143">A menos que se indizan películas por actores, la primera consulta puede requerir que el motor de base de datos para analizar toda la lista de películas.</span><span class="sxs-lookup"><span data-stu-id="ec3dc-143">Unless movies are indexed by actors, the first query might require the DB engine to scan the entire list of movies.</span></span> <span data-ttu-id="ec3dc-144">Mientras que la segunda consulta podría ser aceptable, películas; se supone que se indiza por año de lanzamiento.</span><span class="sxs-lookup"><span data-stu-id="ec3dc-144">Whereas the second query might be acceptable, assuming movies are indexed by release year.</span></span>

    <span data-ttu-id="ec3dc-145">El código siguiente muestra un validador que permite el filtrado en las propiedades "ReleaseYear" y "Title", pero ninguna otra propiedad.</span><span class="sxs-lookup"><span data-stu-id="ec3dc-145">The following code shows a validator that allows filtering on the "ReleaseYear" and "Title" properties but no other properties.</span></span>

    [!code-csharp[Main](odata-security-guidance/samples/sample10.cs)]
- <span data-ttu-id="ec3dc-146">En general, considere la posibilidad de que las funciones de $filter que necesita.</span><span class="sxs-lookup"><span data-stu-id="ec3dc-146">In general, consider which $filter functions you need.</span></span> <span data-ttu-id="ec3dc-147">Si los clientes no necesitan la expresividad de $filter completa, puede limitar las funciones permitidas.</span><span class="sxs-lookup"><span data-stu-id="ec3dc-147">If your clients do not need the full expressiveness of $filter, you can limit the allowed functions.</span></span>
