---
uid: web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
title: Compatibilidad con las opciones de consulta de OData en ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 02/04/2013
ms.assetid: 50e6e62b-e72e-4a29-8293-4b67377bd21f
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
msc.type: authoredcontent
ms.openlocfilehash: 8745183125c9dd1dcc7cb0e146367a893bdb0170
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828432"
---
<a name="supporting-odata-query-options-in-aspnet-web-api-2"></a><span data-ttu-id="c7278-102">Compatibilidad con las opciones de consulta de OData en ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="c7278-102">Supporting OData Query Options in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="c7278-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c7278-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="c7278-104">OData define los parámetros que pueden usarse para modificar una consulta de OData.</span><span class="sxs-lookup"><span data-stu-id="c7278-104">OData defines parameters that can be used to modify an OData query.</span></span> <span data-ttu-id="c7278-105">El cliente envía estos parámetros en la cadena de consulta del URI de solicitud.</span><span class="sxs-lookup"><span data-stu-id="c7278-105">The client sends these parameters in the query string of the request URI.</span></span> <span data-ttu-id="c7278-106">Por ejemplo, para ordenar los resultados, un cliente usa el parámetro $orderby:</span><span class="sxs-lookup"><span data-stu-id="c7278-106">For example, to sort the results, a client uses the $orderby parameter:</span></span>

`http://localhost/Products?$orderby=Name`

<span data-ttu-id="c7278-107">La especificación OData llama a estos parámetros *opciones de consulta*.</span><span class="sxs-lookup"><span data-stu-id="c7278-107">The OData specification calls these parameters *query options*.</span></span> <span data-ttu-id="c7278-108">Puede habilitar las opciones de consulta de OData para cualquier controlador de API Web en el proyecto &#8212; el controlador no necesita ser un extremo de OData.</span><span class="sxs-lookup"><span data-stu-id="c7278-108">You can enable OData query options for any Web API controller in your project &#8212; the controller does not need to be an OData endpoint.</span></span> <span data-ttu-id="c7278-109">Esto le ofrece una manera cómoda de agregar características como el filtrado y ordenación, en cualquier aplicación de API Web.</span><span class="sxs-lookup"><span data-stu-id="c7278-109">This gives you a convenient way to add features such as filtering and sorting to any Web API application.</span></span>

<span data-ttu-id="c7278-110">Antes de habilitar las opciones de consulta, lea el tema [OData Security Guidance](odata-security-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="c7278-110">Before enabling query options, please read the topic [OData Security Guidance](odata-security-guidance.md).</span></span>

- [<span data-ttu-id="c7278-111">Habilitar las opciones de consulta de OData</span><span class="sxs-lookup"><span data-stu-id="c7278-111">Enabling OData Query Options</span></span>](#enable)
- [<span data-ttu-id="c7278-112">Consultas de ejemplo</span><span class="sxs-lookup"><span data-stu-id="c7278-112">Example Queries</span></span>](#examples)
- [<span data-ttu-id="c7278-113">Paginación controlada por servidor</span><span class="sxs-lookup"><span data-stu-id="c7278-113">Server-Driven Paging</span></span>](#server-paging)
- [<span data-ttu-id="c7278-114">Limitar las opciones de consulta</span><span class="sxs-lookup"><span data-stu-id="c7278-114">Limiting the Query Options</span></span>](#limiting_query_options)
- [<span data-ttu-id="c7278-115">Invocar directamente las opciones de consulta</span><span class="sxs-lookup"><span data-stu-id="c7278-115">Invoking Query Options Directly</span></span>](#ODataQueryOptions)
- [<span data-ttu-id="c7278-116">Validación de la consulta</span><span class="sxs-lookup"><span data-stu-id="c7278-116">Query Validation</span></span>](#query-validation)

<a id="enable"></a>
## <a name="enabling-odata-query-options"></a><span data-ttu-id="c7278-117">Habilitar las opciones de consulta de OData</span><span class="sxs-lookup"><span data-stu-id="c7278-117">Enabling OData Query Options</span></span>

<span data-ttu-id="c7278-118">Web API es compatible con las siguientes opciones de consulta de OData:</span><span class="sxs-lookup"><span data-stu-id="c7278-118">Web API supports the following OData query options:</span></span>

| <span data-ttu-id="c7278-119">Opción</span><span class="sxs-lookup"><span data-stu-id="c7278-119">Option</span></span> | <span data-ttu-id="c7278-120">Descripción</span><span class="sxs-lookup"><span data-stu-id="c7278-120">Description</span></span> |
| --- | --- |
| <span data-ttu-id="c7278-121">$expand</span><span class="sxs-lookup"><span data-stu-id="c7278-121">$expand</span></span> | <span data-ttu-id="c7278-122">Se expande en línea de las entidades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="c7278-122">Expands related entities inline.</span></span> |
| <span data-ttu-id="c7278-123">$filter</span><span class="sxs-lookup"><span data-stu-id="c7278-123">$filter</span></span> | <span data-ttu-id="c7278-124">Filtra los resultados, según una condición booleana.</span><span class="sxs-lookup"><span data-stu-id="c7278-124">Filters the results, based on a Boolean condition.</span></span> |
| <span data-ttu-id="c7278-125">$inlinecount</span><span class="sxs-lookup"><span data-stu-id="c7278-125">$inlinecount</span></span> | <span data-ttu-id="c7278-126">Indica al servidor que incluya el recuento total de las entidades coincidentes en la respuesta.</span><span class="sxs-lookup"><span data-stu-id="c7278-126">Tells the server to include the total count of matching entities in the response.</span></span> <span data-ttu-id="c7278-127">(Útil para la paginación del lado servidor).</span><span class="sxs-lookup"><span data-stu-id="c7278-127">(Useful for server-side paging.)</span></span> |
| <span data-ttu-id="c7278-128">$orderby</span><span class="sxs-lookup"><span data-stu-id="c7278-128">$orderby</span></span> | <span data-ttu-id="c7278-129">Ordena los resultados.</span><span class="sxs-lookup"><span data-stu-id="c7278-129">Sorts the results.</span></span> |
| <span data-ttu-id="c7278-130">$select</span><span class="sxs-lookup"><span data-stu-id="c7278-130">$select</span></span> | <span data-ttu-id="c7278-131">Selecciona las propiedades que se va a incluir en la respuesta.</span><span class="sxs-lookup"><span data-stu-id="c7278-131">Selects which properties to include in the response.</span></span> |
| <span data-ttu-id="c7278-132">$skip</span><span class="sxs-lookup"><span data-stu-id="c7278-132">$skip</span></span> | <span data-ttu-id="c7278-133">Omite los primeros n resultados.</span><span class="sxs-lookup"><span data-stu-id="c7278-133">Skips the first n results.</span></span> |
| <span data-ttu-id="c7278-134">$top</span><span class="sxs-lookup"><span data-stu-id="c7278-134">$top</span></span> | <span data-ttu-id="c7278-135">Devuelve solo las primeras n los resultados.</span><span class="sxs-lookup"><span data-stu-id="c7278-135">Returns only the first n the results.</span></span> |

<span data-ttu-id="c7278-136">Para usar las opciones de consulta de OData, debe habilitarlas explícitamente.</span><span class="sxs-lookup"><span data-stu-id="c7278-136">To use OData query options, you must enable them explicitly.</span></span> <span data-ttu-id="c7278-137">Puede habilitar de forma global para toda la aplicación o habilitarlas para determinados controladores o acciones específicas.</span><span class="sxs-lookup"><span data-stu-id="c7278-137">You can enable them globally for the entire application, or enable them for specific controllers or specific actions.</span></span>

<span data-ttu-id="c7278-138">Para habilitar las opciones de consulta de OData de forma global, llame a **EnableQuerySupport** en el **HttpConfiguration** clase en el inicio:</span><span class="sxs-lookup"><span data-stu-id="c7278-138">To enable OData query options globally, call **EnableQuerySupport** on the **HttpConfiguration** class at startup:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample1.cs)]

<span data-ttu-id="c7278-139">El **EnableQuerySupport** método permite opciones de consulta globalmente para cualquier acción de controlador que devuelve un **IQueryable** tipo.</span><span class="sxs-lookup"><span data-stu-id="c7278-139">The **EnableQuerySupport** method enables query options globally for any controller action that returns an **IQueryable** type.</span></span> <span data-ttu-id="c7278-140">Si no desea que las opciones de consulta habilitadas para toda la aplicación, puede habilitarlas para acciones de controlador específico agregando el **[Queryable]** atributo al método de acción.</span><span class="sxs-lookup"><span data-stu-id="c7278-140">If you don't want query options enabled for the entire application, you can enable them for specific controller actions by adding the **[Queryable]** attribute to the action method.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample2.cs)]

<a id="examples"></a>
## <a name="example-queries"></a><span data-ttu-id="c7278-141">Consultas de ejemplo</span><span class="sxs-lookup"><span data-stu-id="c7278-141">Example Queries</span></span>

<span data-ttu-id="c7278-142">Esta sección muestra los tipos de consultas que son posibles mediante las opciones de consulta de OData.</span><span class="sxs-lookup"><span data-stu-id="c7278-142">This section shows the types of queries that are possible using the OData query options.</span></span> <span data-ttu-id="c7278-143">Para obtener información específica sobre las opciones de consulta, consulte la documentación de OData en [www.odata.org](http://www.odata.org/).</span><span class="sxs-lookup"><span data-stu-id="c7278-143">For specific details about the query options, refer to the OData documentation at [www.odata.org](http://www.odata.org/).</span></span>

<span data-ttu-id="c7278-144">Para obtener información acerca de $expanda y $select, consulte [usar $select, $expand y $value en ASP.NET Web API OData](using-select-expand-and-value.md).</span><span class="sxs-lookup"><span data-stu-id="c7278-144">For information about $expand and $select, see [Using $select, $expand, and $value in ASP.NET Web API OData](using-select-expand-and-value.md).</span></span>

<span data-ttu-id="c7278-145">**Paginación controlada por cliente**</span><span class="sxs-lookup"><span data-stu-id="c7278-145">**Client-Driven Paging**</span></span>

<span data-ttu-id="c7278-146">Para los conjuntos de entidades de gran tamaño, el cliente podría limitar el número de resultados.</span><span class="sxs-lookup"><span data-stu-id="c7278-146">For large entity sets, the client might want to limit the number of results.</span></span> <span data-ttu-id="c7278-147">Por ejemplo, un cliente podría mostrar 10 entradas a la vez, con vínculos "siguiente" para obtener la página siguiente de resultados.</span><span class="sxs-lookup"><span data-stu-id="c7278-147">For example, a client might show 10 entries at a time, with "next" links to get the next page of results.</span></span> <span data-ttu-id="c7278-148">Para ello, el cliente usa las opciones de $top y $skip.</span><span class="sxs-lookup"><span data-stu-id="c7278-148">To do this, the client uses the $top and $skip options.</span></span>

`http://localhost/Products?$top=10&$skip=20`

<span data-ttu-id="c7278-149">La opción $top proporciona el número máximo de entradas que se devuelven y la opción de $skip proporciona el número de entradas que se omitirán.</span><span class="sxs-lookup"><span data-stu-id="c7278-149">The $top option gives the maximum number of entries to return, and the $skip option gives the number of entries to skip.</span></span> <span data-ttu-id="c7278-150">El ejemplo anterior captura las entradas de 21 a 30.</span><span class="sxs-lookup"><span data-stu-id="c7278-150">The previous example fetches entries 21 through 30.</span></span>

<span data-ttu-id="c7278-151">**Filtrado**</span><span class="sxs-lookup"><span data-stu-id="c7278-151">**Filtering**</span></span>

<span data-ttu-id="c7278-152">La opción $filter permite filtrar los resultados aplicando una expresión booleana a un cliente.</span><span class="sxs-lookup"><span data-stu-id="c7278-152">The $filter option lets a client filter the results by applying a Boolean expression.</span></span> <span data-ttu-id="c7278-153">Las expresiones de filtro son bastante eficaces; se incluyen operadores aritméticos y lógicos, las funciones de cadena y funciones de fecha.</span><span class="sxs-lookup"><span data-stu-id="c7278-153">The filter expressions are quite powerful; they include logical and arithmetic operators, string functions, and date functions.</span></span>

| <span data-ttu-id="c7278-154">Devolver todos los productos con categoría igual a "Toys".</span><span class="sxs-lookup"><span data-stu-id="c7278-154">Return all products with category equal to "Toys".</span></span> | <span data-ttu-id="c7278-155">`http://localhost/Products?$filter=Category` EQ 'Toys'</span><span class="sxs-lookup"><span data-stu-id="c7278-155">`http://localhost/Products?$filter=Category` eq 'Toys'</span></span> |
| --- | --- |
| <span data-ttu-id="c7278-156">Devolver todos los productos con precio inferior a 10.</span><span class="sxs-lookup"><span data-stu-id="c7278-156">Return all products with price less than 10.</span></span> | <span data-ttu-id="c7278-157">`http://localhost/Products?$filter=Price` lt 10</span><span class="sxs-lookup"><span data-stu-id="c7278-157">`http://localhost/Products?$filter=Price` lt 10</span></span> |
| <span data-ttu-id="c7278-158">Operadores lógicos: devolver todos los productos where precio > = 5 y el precio < = 15.</span><span class="sxs-lookup"><span data-stu-id="c7278-158">Logical operators: Return all products where price >= 5 and price <= 15.</span></span> | <span data-ttu-id="c7278-159">`http://localhost/Products?$filter=Price` GE 5 y el precio 15</span><span class="sxs-lookup"><span data-stu-id="c7278-159">`http://localhost/Products?$filter=Price` ge 5 and Price le 15</span></span> |
| <span data-ttu-id="c7278-160">Funciones de cadena: devolver todos los productos con "zz" en el nombre.</span><span class="sxs-lookup"><span data-stu-id="c7278-160">String functions: Return all products with "zz" in the name.</span></span> | `http://localhost/Products?$filter=substringof('zz',Name)` |
| <span data-ttu-id="c7278-161">Las funciones de fecha: devolver todos los productos con ReleaseDate después de 2005.</span><span class="sxs-lookup"><span data-stu-id="c7278-161">Date functions: Return all products with ReleaseDate after 2005.</span></span> | <span data-ttu-id="c7278-162">`http://localhost/Products?$filter=year(ReleaseDate)` gt 2005</span><span class="sxs-lookup"><span data-stu-id="c7278-162">`http://localhost/Products?$filter=year(ReleaseDate)` gt 2005</span></span> |

<span data-ttu-id="c7278-163">**Ordenación**</span><span class="sxs-lookup"><span data-stu-id="c7278-163">**Sorting**</span></span>

<span data-ttu-id="c7278-164">Para ordenar los resultados, use el filtro $orderby.</span><span class="sxs-lookup"><span data-stu-id="c7278-164">To sort the results, use the $orderby filter.</span></span>

| <span data-ttu-id="c7278-165">Ordenar por precio.</span><span class="sxs-lookup"><span data-stu-id="c7278-165">Sort by price.</span></span> | `http://localhost/Products?$orderby=Price` |
| --- | --- |
| <span data-ttu-id="c7278-166">Ordenar por precio de manera descendente (de mayor a menor).</span><span class="sxs-lookup"><span data-stu-id="c7278-166">Sort by price in descending order (highest to lowest).</span></span> | `http://localhost/Products?$orderby=Price desc` |
| <span data-ttu-id="c7278-167">Ordenar por categoría y, a continuación, ordenar por precio de manera descendente dentro de categorías.</span><span class="sxs-lookup"><span data-stu-id="c7278-167">Sort by category, then sort by price in descending order within categories.</span></span> | `http://localhost/odata/Products?$orderby=Category,Price desc` |

<a id="server-paging"></a>
## <a name="server-driven-paging"></a><span data-ttu-id="c7278-168">Paginación controlada por servidor</span><span class="sxs-lookup"><span data-stu-id="c7278-168">Server-Driven Paging</span></span>

<span data-ttu-id="c7278-169">Si la base de datos contiene millones de registros, no desea enviarles todo en una carga.</span><span class="sxs-lookup"><span data-stu-id="c7278-169">If your database contains millions of records, you don't want to send them all in one payload.</span></span> <span data-ttu-id="c7278-170">Para evitar esto, el servidor puede limitar el número de entradas que envía en una sola respuesta.</span><span class="sxs-lookup"><span data-stu-id="c7278-170">To prevent this, the server can limit the number of entries that it sends in a single response.</span></span> <span data-ttu-id="c7278-171">Para habilitar la paginación en el servidor, establezca el **PageSize** propiedad en el **Queryable** atributo.</span><span class="sxs-lookup"><span data-stu-id="c7278-171">To enable server paging, set the **PageSize** property in the **Queryable** attribute.</span></span> <span data-ttu-id="c7278-172">El valor es el número máximo de entradas que se devolverá.</span><span class="sxs-lookup"><span data-stu-id="c7278-172">The value is the maximum number of entries to return.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample3.cs)]

<span data-ttu-id="c7278-173">Si el controlador devuelve el formato OData, el cuerpo de respuesta contendrá un vínculo a la página siguiente de datos:</span><span class="sxs-lookup"><span data-stu-id="c7278-173">If your controller returns OData format, the response body will contain a link to the next page of data:</span></span>

[!code-json[Main](supporting-odata-query-options/samples/sample4.json?highlight=8)]

<span data-ttu-id="c7278-174">El cliente puede usar este vínculo para capturar la página siguiente.</span><span class="sxs-lookup"><span data-stu-id="c7278-174">The client can use this link to fetch the next page.</span></span> <span data-ttu-id="c7278-175">Para obtener información sobre el número total de entradas en el conjunto de resultados, el cliente puede establecer la opción de consulta $inlinecount con el valor "allpages".</span><span class="sxs-lookup"><span data-stu-id="c7278-175">To learn the total number of entries in the result set, the client can set the $inlinecount query option with the value "allpages".</span></span>

`http://localhost/Products?$inlinecount=allpages`

<span data-ttu-id="c7278-176">El valor "allpages" indica al servidor que incluya el recuento total en la respuesta:</span><span class="sxs-lookup"><span data-stu-id="c7278-176">The value "allpages" tells the server to include the total count in the response:</span></span>

[!code-json[Main](supporting-odata-query-options/samples/sample5.json?highlight=3)]

> [!NOTE]
> <span data-ttu-id="c7278-177">Vínculos de página siguiente y el recuento alineado requieren el formato OData.</span><span class="sxs-lookup"><span data-stu-id="c7278-177">Next-page links and inline count both require OData format.</span></span> <span data-ttu-id="c7278-178">El motivo es que OData define campos especiales en el cuerpo de respuesta que contenga el vínculo y el recuento.</span><span class="sxs-lookup"><span data-stu-id="c7278-178">The reason is that OData defines special fields in the response body to hold the link and count.</span></span>


<span data-ttu-id="c7278-179">Para formatos que no sean OData, resulta todavía posible admitir el recuento de vínculos y en línea de la página siguiente, ajustando los resultados de consulta en un **PageResult&lt;T&gt;**  objeto.</span><span class="sxs-lookup"><span data-stu-id="c7278-179">For non-OData formats, it is still possible to support next-page links and inline count, by wrapping the query results in a **PageResult&lt;T&gt;** object.</span></span> <span data-ttu-id="c7278-180">Sin embargo, requiere un poco más código.</span><span class="sxs-lookup"><span data-stu-id="c7278-180">However, it requires a bit more code.</span></span> <span data-ttu-id="c7278-181">A continuación se muestra un ejemplo:</span><span class="sxs-lookup"><span data-stu-id="c7278-181">Here is an example:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample6.cs)]

<span data-ttu-id="c7278-182">Este es un ejemplo de respuesta JSON:</span><span class="sxs-lookup"><span data-stu-id="c7278-182">Here is an example JSON response:</span></span>

[!code-json[Main](supporting-odata-query-options/samples/sample7.json)]

<a id="limiting_query_options"></a>
## <a name="limiting-the-query-options"></a><span data-ttu-id="c7278-183">Limitar las opciones de consulta</span><span class="sxs-lookup"><span data-stu-id="c7278-183">Limiting the Query Options</span></span>

<span data-ttu-id="c7278-184">Las opciones de consulta proporcionan al cliente de un gran control sobre la consulta que se ejecuta en el servidor.</span><span class="sxs-lookup"><span data-stu-id="c7278-184">The query options give the client a lot of control over the query that is run on the server.</span></span> <span data-ttu-id="c7278-185">Es posible que en algunos casos, conviene que limite las opciones disponibles por motivos de seguridad o rendimiento.</span><span class="sxs-lookup"><span data-stu-id="c7278-185">In some cases, you might want to limit the available options for security or performance reasons.</span></span> <span data-ttu-id="c7278-186">El **[Queryable]** atributo algunos incorpora las propiedades para este.</span><span class="sxs-lookup"><span data-stu-id="c7278-186">The **[Queryable]** attribute has some built in properties for this.</span></span> <span data-ttu-id="c7278-187">Estos son algunos ejemplos:</span><span class="sxs-lookup"><span data-stu-id="c7278-187">Here are some examples.</span></span>

<span data-ttu-id="c7278-188">Permitir solo $skip y $top, para admitir la paginación y nada más:</span><span class="sxs-lookup"><span data-stu-id="c7278-188">Allow only $skip and $top, to support paging and nothing else:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample8.cs)]

<span data-ttu-id="c7278-189">Permitir ordenación sólo con determinadas propiedades evitar la ordenación en las propiedades que no están indizadas en la base de datos:</span><span class="sxs-lookup"><span data-stu-id="c7278-189">Allow ordering only by certain properties, to prevent sorting on properties that are not indexed in the database:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample9.cs)]

<span data-ttu-id="c7278-190">Permitir la función lógica "eq" pero no hay otras funciones lógicas:</span><span class="sxs-lookup"><span data-stu-id="c7278-190">Allow the "eq" logical function but no other logical functions:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample10.cs)]

<span data-ttu-id="c7278-191">No se permiten a los operadores aritméticos:</span><span class="sxs-lookup"><span data-stu-id="c7278-191">Do not allow any arithmetic operators:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample11.cs)]

<span data-ttu-id="c7278-192">Puede restringir las opciones de globalmente mediante la creación de un **QueryableAttribute** instancia y pasarlo a la **EnableQuerySupport** función:</span><span class="sxs-lookup"><span data-stu-id="c7278-192">You can restrict options globally by constructing a **QueryableAttribute** instance and passing it to the **EnableQuerySupport** function:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample12.cs)]

<a id="ODataQueryOptions"></a>
## <a name="invoking-query-options-directly"></a><span data-ttu-id="c7278-193">Invocar directamente las opciones de consulta</span><span class="sxs-lookup"><span data-stu-id="c7278-193">Invoking Query Options Directly</span></span>

<span data-ttu-id="c7278-194">En lugar de usar el **[Queryable]** atributo, puede invocar las opciones de consulta directamente en el controlador.</span><span class="sxs-lookup"><span data-stu-id="c7278-194">Instead of using the **[Queryable]** attribute, you can invoke the query options directly in your controller.</span></span> <span data-ttu-id="c7278-195">Para ello, agregue un **ODataQueryOptions** parámetro del método de controlador.</span><span class="sxs-lookup"><span data-stu-id="c7278-195">To do so, add an **ODataQueryOptions** parameter to the controller method.</span></span> <span data-ttu-id="c7278-196">En este caso, no es necesario el **[Queryable]** atributo.</span><span class="sxs-lookup"><span data-stu-id="c7278-196">In this case, you don't need the **[Queryable]** attribute.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample13.cs)]

<span data-ttu-id="c7278-197">API Web rellena la **ODataQueryOptions** desde el URI de cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="c7278-197">Web API populates the **ODataQueryOptions** from the URI query string.</span></span> <span data-ttu-id="c7278-198">Para aplicar la consulta, pase un **IQueryable** a la **ApplyTo** método.</span><span class="sxs-lookup"><span data-stu-id="c7278-198">To apply the query, pass an **IQueryable** to the **ApplyTo** method.</span></span> <span data-ttu-id="c7278-199">El método devuelve otro **IQueryable**.</span><span class="sxs-lookup"><span data-stu-id="c7278-199">The method returns another **IQueryable**.</span></span>

<span data-ttu-id="c7278-200">Para escenarios avanzados, si no tiene un **IQueryable** proveedor de consultas, puede examinar el **ODataQueryOptions** y traducir las opciones de consulta a otra forma.</span><span class="sxs-lookup"><span data-stu-id="c7278-200">For advanced scenarios, if you do not have an **IQueryable** query provider, you can examine the **ODataQueryOptions** and translate the query options into another form.</span></span> <span data-ttu-id="c7278-201">(Por ejemplo, consulte Entrada de blog de RaghuRam Nadiminti [consultas traducir OData en HQL](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx), que también incluye un [ejemplo](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt).)</span><span class="sxs-lookup"><span data-stu-id="c7278-201">(For example, see RaghuRam Nadiminti's blog post [Translating OData queries to HQL](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx), which also includes a [sample](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt).)</span></span>

<a id="query-validation"></a>
## <a name="query-validation"></a><span data-ttu-id="c7278-202">Validación de la consulta</span><span class="sxs-lookup"><span data-stu-id="c7278-202">Query Validation</span></span>

<span data-ttu-id="c7278-203">El **[Queryable]** atributo valida la consulta antes de ejecutarlo.</span><span class="sxs-lookup"><span data-stu-id="c7278-203">The **[Queryable]** attribute validates the query before executing it.</span></span> <span data-ttu-id="c7278-204">El paso de validación se realiza en el **QueryableAttribute.ValidateQuery** método.</span><span class="sxs-lookup"><span data-stu-id="c7278-204">The validation step is performed in the **QueryableAttribute.ValidateQuery** method.</span></span> <span data-ttu-id="c7278-205">También puede personalizar el proceso de validación.</span><span class="sxs-lookup"><span data-stu-id="c7278-205">You can also customize the validation process.</span></span>

<span data-ttu-id="c7278-206">Consulte también [OData Security Guidance](odata-security-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="c7278-206">Also see [OData Security Guidance](odata-security-guidance.md).</span></span>

<span data-ttu-id="c7278-207">En primer lugar, reemplazo una del validador de clases que es definido en el **Web.Http.OData.Query.Validators** espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="c7278-207">First, override one of the validator classes that is defined in the **Web.Http.OData.Query.Validators** namespace.</span></span> <span data-ttu-id="c7278-208">Por ejemplo, la siguiente clase de validador deshabilita la opción 'desc' para la opción $orderby.</span><span class="sxs-lookup"><span data-stu-id="c7278-208">For example, the following validator class disables the 'desc' option for the $orderby option.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample14.cs)]

<span data-ttu-id="c7278-209">Subclase la **[Queryable]** atributo para invalidar el **validatequery de la** método.</span><span class="sxs-lookup"><span data-stu-id="c7278-209">Subclass the **[Queryable]** attribute to override the **ValidateQuery** method.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample15.cs)]

<span data-ttu-id="c7278-210">A continuación, establezca el atributo personalizado ya sea globalmente o por controlador:</span><span class="sxs-lookup"><span data-stu-id="c7278-210">Then set your custom attribute either globally or per-controller:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample16.cs)]

<span data-ttu-id="c7278-211">Si usas **ODataQueryOptions** establecer directamente, el validador en las opciones:</span><span class="sxs-lookup"><span data-stu-id="c7278-211">If you are using **ODataQueryOptions** directly, set the validator on the options:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample17.cs)]
