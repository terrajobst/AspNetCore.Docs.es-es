---
uid: web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
title: Compatibilidad con las opciones de consulta de OData en ASP.NET Web API 2 | Documentos de Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/04/2013
ms.topic: article
ms.assetid: 50e6e62b-e72e-4a29-8293-4b67377bd21f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
msc.type: authoredcontent
ms.openlocfilehash: 004c029db6f01627f7cadff26aaf5554ce2b93a5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="supporting-odata-query-options-in-aspnet-web-api-2"></a><span data-ttu-id="ce6af-102">Compatibilidad con opciones de consulta de OData en ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="ce6af-102">Supporting OData Query Options in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="ce6af-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ce6af-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="ce6af-104">OData define los parámetros que pueden utilizarse para modificar una consulta de OData.</span><span class="sxs-lookup"><span data-stu-id="ce6af-104">OData defines parameters that can be used to modify an OData query.</span></span> <span data-ttu-id="ce6af-105">El cliente envía estos parámetros en la cadena de consulta de la URI de solicitud.</span><span class="sxs-lookup"><span data-stu-id="ce6af-105">The client sends these parameters in the query string of the request URI.</span></span> <span data-ttu-id="ce6af-106">Por ejemplo, para ordenar los resultados, un cliente usa el parámetro $orderby:</span><span class="sxs-lookup"><span data-stu-id="ce6af-106">For example, to sort the results, a client uses the $orderby parameter:</span></span>

`http://localhost/Products?$orderby=Name`

<span data-ttu-id="ce6af-107">La especificación OData llama a estos parámetros *opciones de consulta*.</span><span class="sxs-lookup"><span data-stu-id="ce6af-107">The OData specification calls these parameters *query options*.</span></span> <span data-ttu-id="ce6af-108">Puede habilitar las opciones de consulta de OData para cualquier controlador de Web API en su proyecto & #8212; el controlador no deben ser un extremo de OData.</span><span class="sxs-lookup"><span data-stu-id="ce6af-108">You can enable OData query options for any Web API controller in your project &#8212; the controller does not need to be an OData endpoint.</span></span> <span data-ttu-id="ce6af-109">Esto proporciona una manera cómoda de agregar características como el filtrado y ordenación, en cualquier aplicación de API Web.</span><span class="sxs-lookup"><span data-stu-id="ce6af-109">This gives you a convenient way to add features such as filtering and sorting to any Web API application.</span></span>

<span data-ttu-id="ce6af-110">Antes de habilitar las opciones de consulta, lea el tema [Guía de seguridad de OData](odata-security-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="ce6af-110">Before enabling query options, please read the topic [OData Security Guidance](odata-security-guidance.md).</span></span>

- [<span data-ttu-id="ce6af-111">Habilitar las opciones de consulta de OData</span><span class="sxs-lookup"><span data-stu-id="ce6af-111">Enabling OData Query Options</span></span>](#enable)
- [<span data-ttu-id="ce6af-112">Consultas de ejemplo</span><span class="sxs-lookup"><span data-stu-id="ce6af-112">Example Queries</span></span>](#examples)
- [<span data-ttu-id="ce6af-113">Paginación controlada por el servidor</span><span class="sxs-lookup"><span data-stu-id="ce6af-113">Server-Driven Paging</span></span>](#server-paging)
- [<span data-ttu-id="ce6af-114">Limitar las opciones de consulta</span><span class="sxs-lookup"><span data-stu-id="ce6af-114">Limiting the Query Options</span></span>](#limiting_query_options)
- [<span data-ttu-id="ce6af-115">Invocar directamente las opciones de consulta</span><span class="sxs-lookup"><span data-stu-id="ce6af-115">Invoking Query Options Directly</span></span>](#ODataQueryOptions)
- [<span data-ttu-id="ce6af-116">Validación de consulta</span><span class="sxs-lookup"><span data-stu-id="ce6af-116">Query Validation</span></span>](#query-validation)

<a id="enable"></a>
## <a name="enabling-odata-query-options"></a><span data-ttu-id="ce6af-117">Habilitar las opciones de consulta de OData</span><span class="sxs-lookup"><span data-stu-id="ce6af-117">Enabling OData Query Options</span></span>

<span data-ttu-id="ce6af-118">Web API es compatible con las siguientes opciones de consulta de OData:</span><span class="sxs-lookup"><span data-stu-id="ce6af-118">Web API supports the following OData query options:</span></span>

| <span data-ttu-id="ce6af-119">Opción</span><span class="sxs-lookup"><span data-stu-id="ce6af-119">Option</span></span> | <span data-ttu-id="ce6af-120">Descripción</span><span class="sxs-lookup"><span data-stu-id="ce6af-120">Description</span></span> |
| --- | --- |
| <span data-ttu-id="ce6af-121">$expand</span><span class="sxs-lookup"><span data-stu-id="ce6af-121">$expand</span></span> | <span data-ttu-id="ce6af-122">Se expande en línea de las entidades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="ce6af-122">Expands related entities inline.</span></span> |
| <span data-ttu-id="ce6af-123">$filter</span><span class="sxs-lookup"><span data-stu-id="ce6af-123">$filter</span></span> | <span data-ttu-id="ce6af-124">Filtra los resultados, en función de una condición booleana.</span><span class="sxs-lookup"><span data-stu-id="ce6af-124">Filters the results, based on a Boolean condition.</span></span> |
| <span data-ttu-id="ce6af-125">$inlinecount</span><span class="sxs-lookup"><span data-stu-id="ce6af-125">$inlinecount</span></span> | <span data-ttu-id="ce6af-126">Indica al servidor que incluya el recuento total de entidades coincidentes en la respuesta.</span><span class="sxs-lookup"><span data-stu-id="ce6af-126">Tells the server to include the total count of matching entities in the response.</span></span> <span data-ttu-id="ce6af-127">(Útil para la paginación del lado servidor).</span><span class="sxs-lookup"><span data-stu-id="ce6af-127">(Useful for server-side paging.)</span></span> |
| <span data-ttu-id="ce6af-128">$orderby</span><span class="sxs-lookup"><span data-stu-id="ce6af-128">$orderby</span></span> | <span data-ttu-id="ce6af-129">Ordena los resultados.</span><span class="sxs-lookup"><span data-stu-id="ce6af-129">Sorts the results.</span></span> |
| <span data-ttu-id="ce6af-130">$select</span><span class="sxs-lookup"><span data-stu-id="ce6af-130">$select</span></span> | <span data-ttu-id="ce6af-131">Selecciona las propiedades que desea incluir en la respuesta.</span><span class="sxs-lookup"><span data-stu-id="ce6af-131">Selects which properties to include in the response.</span></span> |
| <span data-ttu-id="ce6af-132">$skip</span><span class="sxs-lookup"><span data-stu-id="ce6af-132">$skip</span></span> | <span data-ttu-id="ce6af-133">Omite los n primeros resultados.</span><span class="sxs-lookup"><span data-stu-id="ce6af-133">Skips the first n results.</span></span> |
| <span data-ttu-id="ce6af-134">$top</span><span class="sxs-lookup"><span data-stu-id="ce6af-134">$top</span></span> | <span data-ttu-id="ce6af-135">Devuelve solo lo primeros n los resultados.</span><span class="sxs-lookup"><span data-stu-id="ce6af-135">Returns only the first n the results.</span></span> |

<span data-ttu-id="ce6af-136">Para utilizar las opciones de consulta de OData, debe habilitarlas explícitamente.</span><span class="sxs-lookup"><span data-stu-id="ce6af-136">To use OData query options, you must enable them explicitly.</span></span> <span data-ttu-id="ce6af-137">Puede habilitarlas globalmente para toda la aplicación o habilitarlas para determinados controladores o acciones específicas.</span><span class="sxs-lookup"><span data-stu-id="ce6af-137">You can enable them globally for the entire application, or enable them for specific controllers or specific actions.</span></span>

<span data-ttu-id="ce6af-138">Para habilitar las opciones de consulta de OData globalmente, llame a **EnableQuerySupport** en el **HttpConfiguration** clase durante el inicio:</span><span class="sxs-lookup"><span data-stu-id="ce6af-138">To enable OData query options globally, call **EnableQuerySupport** on the **HttpConfiguration** class at startup:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample1.cs)]

<span data-ttu-id="ce6af-139">El **EnableQuerySupport** método habilita opciones de consulta global para cualquier acción de controlador que devuelve un **IQueryable** tipo.</span><span class="sxs-lookup"><span data-stu-id="ce6af-139">The **EnableQuerySupport** method enables query options globally for any controller action that returns an **IQueryable** type.</span></span> <span data-ttu-id="ce6af-140">Si no desea que las opciones de consulta habilitadas para toda la aplicación, puede habilitarlas para las acciones de un controlador específico agregando el **[Queryable]** atributo al método de acción.</span><span class="sxs-lookup"><span data-stu-id="ce6af-140">If you don't want query options enabled for the entire application, you can enable them for specific controller actions by adding the **[Queryable]** attribute to the action method.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample2.cs)]

<a id="examples"></a>
## <a name="example-queries"></a><span data-ttu-id="ce6af-141">Consultas de ejemplo</span><span class="sxs-lookup"><span data-stu-id="ce6af-141">Example Queries</span></span>

<span data-ttu-id="ce6af-142">Esta sección muestra los tipos de consultas que son posibles mediante las opciones de consulta de OData.</span><span class="sxs-lookup"><span data-stu-id="ce6af-142">This section shows the types of queries that are possible using the OData query options.</span></span> <span data-ttu-id="ce6af-143">Para obtener detalles sobre las opciones de consulta, consulte la documentación de OData en [www.odata.org](http://www.odata.org/).</span><span class="sxs-lookup"><span data-stu-id="ce6af-143">For specific details about the query options, refer to the OData documentation at [www.odata.org](http://www.odata.org/).</span></span>

<span data-ttu-id="ce6af-144">Para obtener información acerca de $expanda y $select, consulte [con $select, $expand y $value de OData de ASP.NET Web API](using-select-expand-and-value.md).</span><span class="sxs-lookup"><span data-stu-id="ce6af-144">For information about $expand and $select, see [Using $select, $expand, and $value in ASP.NET Web API OData](using-select-expand-and-value.md).</span></span>

<span data-ttu-id="ce6af-145">**Paginación controlada por el cliente**</span><span class="sxs-lookup"><span data-stu-id="ce6af-145">**Client-Driven Paging**</span></span>

<span data-ttu-id="ce6af-146">Para los conjuntos de entidades de gran tamaño, el cliente desee limitar el número de resultados.</span><span class="sxs-lookup"><span data-stu-id="ce6af-146">For large entity sets, the client might want to limit the number of results.</span></span> <span data-ttu-id="ce6af-147">Por ejemplo, un cliente podría mostrar 10 entradas a la vez, con vínculos "siguiente" para obtener la siguiente página de resultados.</span><span class="sxs-lookup"><span data-stu-id="ce6af-147">For example, a client might show 10 entries at a time, with "next" links to get the next page of results.</span></span> <span data-ttu-id="ce6af-148">Para ello, el cliente utiliza las opciones $top y $skip.</span><span class="sxs-lookup"><span data-stu-id="ce6af-148">To do this, the client uses the $top and $skip options.</span></span>

`http://localhost/Products?$top=10&$skip=20`

<span data-ttu-id="ce6af-149">La opción $top indica el número máximo de entradas que se devolverá, y la opción $skip proporciona el número de entradas que se omitirán.</span><span class="sxs-lookup"><span data-stu-id="ce6af-149">The $top option gives the maximum number of entries to return, and the $skip option gives the number of entries to skip.</span></span> <span data-ttu-id="ce6af-150">El ejemplo anterior captura las entradas de 21 a 30.</span><span class="sxs-lookup"><span data-stu-id="ce6af-150">The previous example fetches entries 21 through 30.</span></span>

<span data-ttu-id="ce6af-151">**Filtrado**</span><span class="sxs-lookup"><span data-stu-id="ce6af-151">**Filtering**</span></span>

<span data-ttu-id="ce6af-152">La opción $filter permite que un cliente filtrar los resultados mediante la aplicación de una expresión booleana.</span><span class="sxs-lookup"><span data-stu-id="ce6af-152">The $filter option lets a client filter the results by applying a Boolean expression.</span></span> <span data-ttu-id="ce6af-153">Las expresiones de filtro son muy eficaces; incluyen operadores lógicos y aritméticos, funciones de cadena y funciones de fecha.</span><span class="sxs-lookup"><span data-stu-id="ce6af-153">The filter expressions are quite powerful; they include logical and arithmetic operators, string functions, and date functions.</span></span>

| <span data-ttu-id="ce6af-154">Devolver todos los productos con categoría igual a "Toys".</span><span class="sxs-lookup"><span data-stu-id="ce6af-154">Return all products with category equal to "Toys".</span></span> | <span data-ttu-id="ce6af-155">`http://localhost/Products?$filter=Category`EQ 'Toys'</span><span class="sxs-lookup"><span data-stu-id="ce6af-155">`http://localhost/Products?$filter=Category` eq 'Toys'</span></span> |
| --- | --- |
| <span data-ttu-id="ce6af-156">Devolver todos los productos con precio inferior a 10.</span><span class="sxs-lookup"><span data-stu-id="ce6af-156">Return all products with price less than 10.</span></span> | <span data-ttu-id="ce6af-157">`http://localhost/Products?$filter=Price`lt 10</span><span class="sxs-lookup"><span data-stu-id="ce6af-157">`http://localhost/Products?$filter=Price` lt 10</span></span> |
| <span data-ttu-id="ce6af-158">Operadores lógicos: devolver todos los productos where precio > = 5 y el precio < = 15.</span><span class="sxs-lookup"><span data-stu-id="ce6af-158">Logical operators: Return all products where price >= 5 and price <= 15.</span></span> | <span data-ttu-id="ce6af-159">`http://localhost/Products?$filter=Price`GE 5 y precio le 15</span><span class="sxs-lookup"><span data-stu-id="ce6af-159">`http://localhost/Products?$filter=Price` ge 5 and Price le 15</span></span> |
| <span data-ttu-id="ce6af-160">Funciones de cadena: devolver todos los productos con "zz" en el nombre.</span><span class="sxs-lookup"><span data-stu-id="ce6af-160">String functions: Return all products with "zz" in the name.</span></span> | `http://localhost/Products?$filter=substringof('zz',Name)` |
| <span data-ttu-id="ce6af-161">Funciones de fecha: devolver todos los productos con ReleaseDate después 2005.</span><span class="sxs-lookup"><span data-stu-id="ce6af-161">Date functions: Return all products with ReleaseDate after 2005.</span></span> | <span data-ttu-id="ce6af-162">`http://localhost/Products?$filter=year(ReleaseDate)`gt 2005</span><span class="sxs-lookup"><span data-stu-id="ce6af-162">`http://localhost/Products?$filter=year(ReleaseDate)` gt 2005</span></span> |

<span data-ttu-id="ce6af-163">**Ordenar**</span><span class="sxs-lookup"><span data-stu-id="ce6af-163">**Sorting**</span></span>

<span data-ttu-id="ce6af-164">Para ordenar los resultados, use el filtro de $orderby.</span><span class="sxs-lookup"><span data-stu-id="ce6af-164">To sort the results, use the $orderby filter.</span></span>

| <span data-ttu-id="ce6af-165">Ordenar por precio.</span><span class="sxs-lookup"><span data-stu-id="ce6af-165">Sort by price.</span></span> | `http://localhost/Products?$orderby=Price` |
| --- | --- |
| <span data-ttu-id="ce6af-166">Ordenar por precio de forma descendente (de mayor a menor).</span><span class="sxs-lookup"><span data-stu-id="ce6af-166">Sort by price in descending order (highest to lowest).</span></span> | `http://localhost/Products?$orderby=Price desc` |
| <span data-ttu-id="ce6af-167">Ordenar por categoría y, a continuación, ordenar por precio de forma descendente dentro de las categorías.</span><span class="sxs-lookup"><span data-stu-id="ce6af-167">Sort by category, then sort by price in descending order within categories.</span></span> | `http://localhost/odata/Products?$orderby=Category,Price desc` |

<a id="server-paging"></a>
## <a name="server-driven-paging"></a><span data-ttu-id="ce6af-168">Paginación controlada por el servidor</span><span class="sxs-lookup"><span data-stu-id="ce6af-168">Server-Driven Paging</span></span>

<span data-ttu-id="ce6af-169">Si la base de datos contiene millones de registros, que no desea enviarlos en una carga.</span><span class="sxs-lookup"><span data-stu-id="ce6af-169">If your database contains millions of records, you don't want to send them all in one payload.</span></span> <span data-ttu-id="ce6af-170">Para evitar esto, el servidor puede limitar el número de entradas que envía en una única respuesta.</span><span class="sxs-lookup"><span data-stu-id="ce6af-170">To prevent this, the server can limit the number of entries that it sends in a single response.</span></span> <span data-ttu-id="ce6af-171">Para habilitar la paginación en el servidor, establezca el **PageSize** propiedad en el **Queryable** atributo.</span><span class="sxs-lookup"><span data-stu-id="ce6af-171">To enable server paging, set the **PageSize** property in the **Queryable** attribute.</span></span> <span data-ttu-id="ce6af-172">El valor es el número máximo de entradas que se devolverá.</span><span class="sxs-lookup"><span data-stu-id="ce6af-172">The value is the maximum number of entries to return.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample3.cs)]

<span data-ttu-id="ce6af-173">Si el controlador devuelve el formato OData, el cuerpo de respuesta contendrá un vínculo a la página siguiente de datos:</span><span class="sxs-lookup"><span data-stu-id="ce6af-173">If your controller returns OData format, the response body will contain a link to the next page of data:</span></span>

[!code-json[Main](supporting-odata-query-options/samples/sample4.json?highlight=8)]

<span data-ttu-id="ce6af-174">El cliente puede usar este vínculo para obtener la página siguiente.</span><span class="sxs-lookup"><span data-stu-id="ce6af-174">The client can use this link to fetch the next page.</span></span> <span data-ttu-id="ce6af-175">Para obtener información sobre el número total de entradas en el conjunto de resultados, el cliente puede establecer la opción de consulta $inlinecount con el valor "allpages".</span><span class="sxs-lookup"><span data-stu-id="ce6af-175">To learn the total number of entries in the result set, the client can set the $inlinecount query option with the value "allpages".</span></span>

`http://localhost/Products?$inlinecount=allpages`

<span data-ttu-id="ce6af-176">El valor "allpages" indica al servidor que incluya el recuento total de la respuesta:</span><span class="sxs-lookup"><span data-stu-id="ce6af-176">The value "allpages" tells the server to include the total count in the response:</span></span>

[!code-json[Main](supporting-odata-query-options/samples/sample5.json?highlight=3)]

> [!NOTE]
> <span data-ttu-id="ce6af-177">Vínculos de página siguiente y el recuento alineado requieren un formato OData.</span><span class="sxs-lookup"><span data-stu-id="ce6af-177">Next-page links and inline count both require OData format.</span></span> <span data-ttu-id="ce6af-178">La razón es que OData define campos especiales en el cuerpo de respuesta para mantener el vínculo y el recuento.</span><span class="sxs-lookup"><span data-stu-id="ce6af-178">The reason is that OData defines special fields in the response body to hold the link and count.</span></span>


<span data-ttu-id="ce6af-179">Para conocer los formatos no OData, todavía es posible admitir el número de vínculos y en línea de la página siguiente, ajustando los resultados de consulta en un **PageResult&lt;T&gt;**  objeto.</span><span class="sxs-lookup"><span data-stu-id="ce6af-179">For non-OData formats, it is still possible to support next-page links and inline count, by wrapping the query results in a **PageResult&lt;T&gt;** object.</span></span> <span data-ttu-id="ce6af-180">Sin embargo, requiere un poco más código.</span><span class="sxs-lookup"><span data-stu-id="ce6af-180">However, it requires a bit more code.</span></span> <span data-ttu-id="ce6af-181">A continuación se muestra un ejemplo:</span><span class="sxs-lookup"><span data-stu-id="ce6af-181">Here is an example:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample6.cs)]

<span data-ttu-id="ce6af-182">Este es un respuesta JSON de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="ce6af-182">Here is an example JSON response:</span></span>

[!code-json[Main](supporting-odata-query-options/samples/sample7.json)]

<a id="limiting_query_options"></a>
## <a name="limiting-the-query-options"></a><span data-ttu-id="ce6af-183">Limitar las opciones de consulta</span><span class="sxs-lookup"><span data-stu-id="ce6af-183">Limiting the Query Options</span></span>

<span data-ttu-id="ce6af-184">Las opciones de consulta proporcionan al cliente un gran control sobre la consulta que se ejecuta en el servidor.</span><span class="sxs-lookup"><span data-stu-id="ce6af-184">The query options give the client a lot of control over the query that is run on the server.</span></span> <span data-ttu-id="ce6af-185">En algunos casos, puede limitar las opciones disponibles por motivos de seguridad o de rendimiento.</span><span class="sxs-lookup"><span data-stu-id="ce6af-185">In some cases, you might want to limit the available options for security or performance reasons.</span></span> <span data-ttu-id="ce6af-186">El **[Queryable]** atributo ha algunos creados en las propiedades para este.</span><span class="sxs-lookup"><span data-stu-id="ce6af-186">The **[Queryable]** attribute has some built in properties for this.</span></span> <span data-ttu-id="ce6af-187">Estos son algunos ejemplos:</span><span class="sxs-lookup"><span data-stu-id="ce6af-187">Here are some examples.</span></span>

<span data-ttu-id="ce6af-188">Permitir sólo $skip y $top, para admitir la paginación y nada más:</span><span class="sxs-lookup"><span data-stu-id="ce6af-188">Allow only $skip and $top, to support paging and nothing else:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample8.cs)]

<span data-ttu-id="ce6af-189">Permite ordenar solo por determinadas propiedades evitar la ordenación en las propiedades que no están indizadas en la base de datos:</span><span class="sxs-lookup"><span data-stu-id="ce6af-189">Allow ordering only by certain properties, to prevent sorting on properties that are not indexed in the database:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample9.cs)]

<span data-ttu-id="ce6af-190">Permitir la función lógica "eq" pero no otras funciones lógicas:</span><span class="sxs-lookup"><span data-stu-id="ce6af-190">Allow the "eq" logical function but no other logical functions:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample10.cs)]

<span data-ttu-id="ce6af-191">No se permiten a los operadores aritméticos:</span><span class="sxs-lookup"><span data-stu-id="ce6af-191">Do not allow any arithmetic operators:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample11.cs)]

<span data-ttu-id="ce6af-192">Puede restringir opciones globalmente mediante la creación de un **QueryableAttribute** instancia y pasarlo a la **EnableQuerySupport** función:</span><span class="sxs-lookup"><span data-stu-id="ce6af-192">You can restrict options globally by constructing a **QueryableAttribute** instance and passing it to the **EnableQuerySupport** function:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample12.cs)]

<a id="ODataQueryOptions"></a>
## <a name="invoking-query-options-directly"></a><span data-ttu-id="ce6af-193">Invocar directamente las opciones de consulta</span><span class="sxs-lookup"><span data-stu-id="ce6af-193">Invoking Query Options Directly</span></span>

<span data-ttu-id="ce6af-194">En lugar de utilizar el **[Queryable]** atributo, puede invocar las opciones de consulta directamente en el controlador.</span><span class="sxs-lookup"><span data-stu-id="ce6af-194">Instead of using the **[Queryable]** attribute, you can invoke the query options directly in your controller.</span></span> <span data-ttu-id="ce6af-195">Para ello, agregue un **ODataQueryOptions** parámetro para el método del controlador.</span><span class="sxs-lookup"><span data-stu-id="ce6af-195">To do so, add an **ODataQueryOptions** parameter to the controller method.</span></span> <span data-ttu-id="ce6af-196">En este caso, no es necesario el **[Queryable]** atributo.</span><span class="sxs-lookup"><span data-stu-id="ce6af-196">In this case, you don't need the **[Queryable]** attribute.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample13.cs)]

<span data-ttu-id="ce6af-197">API Web rellena la **ODataQueryOptions** del URI de cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="ce6af-197">Web API populates the **ODataQueryOptions** from the URI query string.</span></span> <span data-ttu-id="ce6af-198">Para aplicar la consulta, pase una **IQueryable** a la **ApplyTo** método.</span><span class="sxs-lookup"><span data-stu-id="ce6af-198">To apply the query, pass an **IQueryable** to the **ApplyTo** method.</span></span> <span data-ttu-id="ce6af-199">El método devuelve otro **IQueryable**.</span><span class="sxs-lookup"><span data-stu-id="ce6af-199">The method returns another **IQueryable**.</span></span>

<span data-ttu-id="ce6af-200">Para escenarios avanzados, si no tiene un **IQueryable** consultar al proveedor, puede examinar la **ODataQueryOptions** y traducir las opciones de consulta a otra forma.</span><span class="sxs-lookup"><span data-stu-id="ce6af-200">For advanced scenarios, if you do not have an **IQueryable** query provider, you can examine the **ODataQueryOptions** and translate the query options into another form.</span></span> <span data-ttu-id="ce6af-201">(Por ejemplo, consulte Entrada de blog de RaghuRam Nadiminti [consultas traducir OData en HQL](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx), que también incluye una [ejemplo](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt).)</span><span class="sxs-lookup"><span data-stu-id="ce6af-201">(For example, see RaghuRam Nadiminti's blog post [Translating OData queries to HQL](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx), which also includes a [sample](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt).)</span></span>

<a id="query-validation"></a>
## <a name="query-validation"></a><span data-ttu-id="ce6af-202">Validación de consulta</span><span class="sxs-lookup"><span data-stu-id="ce6af-202">Query Validation</span></span>

<span data-ttu-id="ce6af-203">El **[Queryable]** atributo valida la consulta antes de ejecutarlo.</span><span class="sxs-lookup"><span data-stu-id="ce6af-203">The **[Queryable]** attribute validates the query before executing it.</span></span> <span data-ttu-id="ce6af-204">El paso de validación se realiza en el **QueryableAttribute.ValidateQuery** método.</span><span class="sxs-lookup"><span data-stu-id="ce6af-204">The validation step is performed in the **QueryableAttribute.ValidateQuery** method.</span></span> <span data-ttu-id="ce6af-205">También puede personalizar el proceso de validación.</span><span class="sxs-lookup"><span data-stu-id="ce6af-205">You can also customize the validation process.</span></span>

<span data-ttu-id="ce6af-206">Consulte también [Guía de seguridad de OData](odata-security-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="ce6af-206">Also see [OData Security Guidance](odata-security-guidance.md).</span></span>

<span data-ttu-id="ce6af-207">En primer lugar, invalidación uno del validador clases que es definido en el **Web.Http.OData.Query.Validators** espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="ce6af-207">First, override one of the validator classes that is defined in the **Web.Http.OData.Query.Validators** namespace.</span></span> <span data-ttu-id="ce6af-208">Por ejemplo, la siguiente clase de validador deshabilita la opción "desc" para la opción $orderby.</span><span class="sxs-lookup"><span data-stu-id="ce6af-208">For example, the following validator class disables the 'desc' option for the $orderby option.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample14.cs)]

<span data-ttu-id="ce6af-209">Subclase la **[Queryable]** atributo para invalidar la **ValidateQuery** método.</span><span class="sxs-lookup"><span data-stu-id="ce6af-209">Subclass the **[Queryable]** attribute to override the **ValidateQuery** method.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample15.cs)]

<span data-ttu-id="ce6af-210">A continuación, establezca el atributo personalizado ya sea globalmente o por el controlador:</span><span class="sxs-lookup"><span data-stu-id="ce6af-210">Then set your custom attribute either globally or per-controller:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample16.cs)]

<span data-ttu-id="ce6af-211">Si utilizas **ODataQueryOptions** establecer directamente, el validador en las opciones:</span><span class="sxs-lookup"><span data-stu-id="ce6af-211">If you are using **ODataQueryOptions** directly, set the validator on the options:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample17.cs)]
