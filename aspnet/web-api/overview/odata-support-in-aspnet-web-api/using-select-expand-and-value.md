---
uid: web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
title: Con $select, $expand y $value en ASP.NET Web API 2 OData | Documentos de Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/11/2013
ms.topic: article
ms.assetid: 43279a80-a96c-4564-b6ea-ad992a2d6828
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
msc.type: authoredcontent
ms.openlocfilehash: f229cdbd8850a787dd3585e0640e8e66f6109331
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
ms.locfileid: "26508064"
---
<a name="using-select-expand-and-value-in-aspnet-web-api-2-odata"></a><span data-ttu-id="d317b-102">Con $select, $expand y $value en ASP.NET Web API 2 OData</span><span class="sxs-lookup"><span data-stu-id="d317b-102">Using $select, $expand, and $value in ASP.NET Web API 2 OData</span></span>
====================
<span data-ttu-id="d317b-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="d317b-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="d317b-104">Web API 2 agrega compatibilidad para expandir $expand, $select y las opciones de $value de OData.</span><span class="sxs-lookup"><span data-stu-id="d317b-104">Web API 2 adds support for the $expand, $select, and $value options in OData.</span></span> <span data-ttu-id="d317b-105">Estas opciones permiten un cliente controlar la representación que se recibe desde el servidor.</span><span class="sxs-lookup"><span data-stu-id="d317b-105">These options allow a client to control the representation that it gets back from the server.</span></span>

- <span data-ttu-id="d317b-106">**$expand** hace que las entidades relacionadas pueden insertar en la respuesta.</span><span class="sxs-lookup"><span data-stu-id="d317b-106">**$expand** causes related entities to be included inline in the response.</span></span>
- <span data-ttu-id="d317b-107">**$select** selecciona un subconjunto de propiedades para incluir en la respuesta.</span><span class="sxs-lookup"><span data-stu-id="d317b-107">**$select** selects a subset of properties to include in the response.</span></span>
- <span data-ttu-id="d317b-108">**$value** Obtiene el valor sin formato de una propiedad.</span><span class="sxs-lookup"><span data-stu-id="d317b-108">**$value** gets the raw value of a property.</span></span>

## <a name="example-schema"></a><span data-ttu-id="d317b-109">Esquema de ejemplo</span><span class="sxs-lookup"><span data-stu-id="d317b-109">Example Schema</span></span>

<span data-ttu-id="d317b-110">En este artículo, utilizará un servicio de OData que define tres entidades: categoría, proveedor y producto.</span><span class="sxs-lookup"><span data-stu-id="d317b-110">For this article, I'll use an OData service that defines three entities: Product, Supplier, and Category.</span></span> <span data-ttu-id="d317b-111">Cada producto tiene una categoría y un proveedor.</span><span class="sxs-lookup"><span data-stu-id="d317b-111">Each product has one category and one supplier.</span></span>

![](using-select-expand-and-value/_static/image1.png)

<span data-ttu-id="d317b-112">Estas son las clases de C# que definen los modelos de entidad:</span><span class="sxs-lookup"><span data-stu-id="d317b-112">Here are the C# classes that define the entity models:</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample1.cs)]

<span data-ttu-id="d317b-113">Tenga en cuenta que la `Product` clase define propiedades de navegación para la `Supplier` y `Category`.</span><span class="sxs-lookup"><span data-stu-id="d317b-113">Notice that the `Product` class defines navigation properties for the `Supplier` and `Category`.</span></span> <span data-ttu-id="d317b-114">La `Category` clase define una propiedad de navegación para los productos en cada categoría.</span><span class="sxs-lookup"><span data-stu-id="d317b-114">The `Category` class defines a navigation property for the products in each category.</span></span>

<span data-ttu-id="d317b-115">Para crear un extremo de OData para este esquema, usar el scaffolding de Visual Studio 2013, como se describe en [creación de un extremo de OData en ASP.NET Web API](odata-v3/creating-an-odata-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="d317b-115">To create an OData endpoint for this schema, use the Visual Studio 2013 scaffolding, as described in [Creating an OData Endpoint in ASP.NET Web API](odata-v3/creating-an-odata-endpoint.md).</span></span> <span data-ttu-id="d317b-116">Agregar controladores independientes para los productos, categorías y proveedores.</span><span class="sxs-lookup"><span data-stu-id="d317b-116">Add separate controllers for Product, Category, and Supplier.</span></span>

## <a name="enabling-expand-and-select"></a><span data-ttu-id="d317b-117">Habilitar $expanda y $select</span><span class="sxs-lookup"><span data-stu-id="d317b-117">Enabling $expand and $select</span></span>

<span data-ttu-id="d317b-118">En Visual Studio 2013, el scaffolding de Web API OData crea un controlador que automáticamente admite $expand y $select.</span><span class="sxs-lookup"><span data-stu-id="d317b-118">In Visual Studio 2013, the Web API OData scaffolding creates a controller that automatically supports $expand and $select.</span></span> <span data-ttu-id="d317b-119">Como referencia, estos son los requisitos para admitir $expanda y $select en un controlador.</span><span class="sxs-lookup"><span data-stu-id="d317b-119">For reference, here are the requirements to support $expand and $select in a controller.</span></span>

<span data-ttu-id="d317b-120">Para de las colecciones, el controlador `Get` método debe devolver un **IQueryable**.</span><span class="sxs-lookup"><span data-stu-id="d317b-120">For collections, the controller's `Get` method must return an **IQueryable**.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample2.cs)]

<span data-ttu-id="d317b-121">Para entidades únicas, devolver un **SingleResult&lt;T&gt;**, donde T es un **IQueryable** que contiene cero entidades o una entidad.</span><span class="sxs-lookup"><span data-stu-id="d317b-121">For single entities, return a **SingleResult&lt;T&gt;**, where T is an **IQueryable** that contains zero or one entities.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample3.cs)]

<span data-ttu-id="d317b-122">Además, decorar la `Get` métodos con el **[Queryable]** atributo, como se muestra en los fragmentos de código anterior.</span><span class="sxs-lookup"><span data-stu-id="d317b-122">Also, decorate your `Get` methods with the **[Queryable]** attribute, as shown in the previous code snippets.</span></span> <span data-ttu-id="d317b-123">O bien, llamar a **EnableQuerySupport** en el **HttpConfiguration** objeto en el inicio.</span><span class="sxs-lookup"><span data-stu-id="d317b-123">Alternatively, call **EnableQuerySupport** on the **HttpConfiguration** object at startup.</span></span> <span data-ttu-id="d317b-124">(Para obtener más información, consulte [habilitar opciones de consulta de OData](supporting-odata-query-options.md#enable).)</span><span class="sxs-lookup"><span data-stu-id="d317b-124">(For more information, see [Enabling OData Query Options](supporting-odata-query-options.md#enable).)</span></span>

## <a name="using-expand"></a><span data-ttu-id="d317b-125">$Expanda</span><span class="sxs-lookup"><span data-stu-id="d317b-125">Using $expand</span></span>

<span data-ttu-id="d317b-126">Al consultar una entidad OData o una colección, la respuesta predeterminada no incluye las entidades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="d317b-126">When you query an OData entity or collection, the default response does not include related entities.</span></span> <span data-ttu-id="d317b-127">Por ejemplo, aquí está la respuesta predeterminada para el conjunto de entidades de categorías:</span><span class="sxs-lookup"><span data-stu-id="d317b-127">For example, here is the default response for the Categories entity set:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample4.cmd)]

<span data-ttu-id="d317b-128">Como puede ver, la respuesta no incluye los productos, aunque la entidad Category tiene un vínculo de navegación de productos.</span><span class="sxs-lookup"><span data-stu-id="d317b-128">As you can see, the response does not include any products, even though the Category entity has a Products navigation link.</span></span> <span data-ttu-id="d317b-129">Sin embargo, el cliente puede utilizar $ se expanden para obtener la lista de productos de cada categoría.</span><span class="sxs-lookup"><span data-stu-id="d317b-129">However, the client can use $expand to get the list of products for each category.</span></span> <span data-ttu-id="d317b-130">La $expand opción va en la cadena de consulta de la solicitud:</span><span class="sxs-lookup"><span data-stu-id="d317b-130">The $expand option goes in the query string of the request:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample5.cmd)]

<span data-ttu-id="d317b-131">Ahora el servidor incluirá los productos para cada categoría, en línea con las categorías.</span><span class="sxs-lookup"><span data-stu-id="d317b-131">Now the server will include the products for each category, inline with the categories.</span></span> <span data-ttu-id="d317b-132">Esta es la carga de respuesta:</span><span class="sxs-lookup"><span data-stu-id="d317b-132">Here is the response payload:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample6.cmd)]

<span data-ttu-id="d317b-133">Tenga en cuenta que cada entrada de la matriz de "value" contiene una lista de productos.</span><span class="sxs-lookup"><span data-stu-id="d317b-133">Notice that each entry in the "value" array contains a Products list.</span></span>

<span data-ttu-id="d317b-134">La $expand lista toma separados por comas de opciones de propiedades de navegación para expandir.</span><span class="sxs-lookup"><span data-stu-id="d317b-134">The $expand option takes a comma-separated list of navigation properties to expand.</span></span> <span data-ttu-id="d317b-135">La siguiente solicitud expande la categoría y el proveedor de un producto.</span><span class="sxs-lookup"><span data-stu-id="d317b-135">The following request expands both the category and the supplier for a product.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample7.cmd)]

<span data-ttu-id="d317b-136">Este es el cuerpo de respuesta:</span><span class="sxs-lookup"><span data-stu-id="d317b-136">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample8.cmd)]

<span data-ttu-id="d317b-137">Puede expandir más de un nivel de propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="d317b-137">You can expand more than one level of navigation property.</span></span> <span data-ttu-id="d317b-138">En el ejemplo siguiente se incluye todos los productos para una categoría así como el proveedor de cada producto.</span><span class="sxs-lookup"><span data-stu-id="d317b-138">The following example includes all the products for a category and also the supplier for each product.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample9.cmd)]

<span data-ttu-id="d317b-139">Este es el cuerpo de respuesta:</span><span class="sxs-lookup"><span data-stu-id="d317b-139">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample10.cmd)]

<span data-ttu-id="d317b-140">De forma predeterminada, Web API limita la profundidad de expansión máxima a 2.</span><span class="sxs-lookup"><span data-stu-id="d317b-140">By default, Web API limits the maximum expansion depth to 2.</span></span> <span data-ttu-id="d317b-141">Que impide que el cliente envía solicitudes complejas como `$expand=Orders/OrderDetails/Product/Supplier/Region`, que podría ser ineficaz para consultar y crear respuestas de gran tamaño.</span><span class="sxs-lookup"><span data-stu-id="d317b-141">That prevents the client from sending complex requests like `$expand=Orders/OrderDetails/Product/Supplier/Region`, which might be inefficient to query and create large responses.</span></span> <span data-ttu-id="d317b-142">Para reemplazar el valor predeterminado, establezca el **MaxExpansionDepth** propiedad en el **[Queryable]** atributo.</span><span class="sxs-lookup"><span data-stu-id="d317b-142">To override the default, set the **MaxExpansionDepth** property on the **[Queryable]** attribute.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample11.cs)]

<span data-ttu-id="d317b-143">Para obtener más información sobre la $expandir opción, consulte [expanda System Query Option ($expand)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand) en la documentación oficial de OData.</span><span class="sxs-lookup"><span data-stu-id="d317b-143">For more information about the $expand option, see [Expand System Query Option ($expand)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand) in the official OData documentation.</span></span>

## <a name="using-select"></a><span data-ttu-id="d317b-144">Uso de $select</span><span class="sxs-lookup"><span data-stu-id="d317b-144">Using $select</span></span>

<span data-ttu-id="d317b-145">La opción $select especifica un subconjunto de propiedades para incluir en el cuerpo de respuesta.</span><span class="sxs-lookup"><span data-stu-id="d317b-145">The $select option specifies a subset of properties to include in the response body.</span></span> <span data-ttu-id="d317b-146">Por ejemplo, para obtener solo el nombre y el precio de cada producto, use la siguiente consulta:</span><span class="sxs-lookup"><span data-stu-id="d317b-146">For example, to get only the name and price of each product, use the following query:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample12.cmd)]

<span data-ttu-id="d317b-147">Este es el cuerpo de respuesta:</span><span class="sxs-lookup"><span data-stu-id="d317b-147">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample13.cmd)]

<span data-ttu-id="d317b-148">Puede combinar $select y $expand en la misma consulta.</span><span class="sxs-lookup"><span data-stu-id="d317b-148">You can combine $select and $expand in the same query.</span></span> <span data-ttu-id="d317b-149">Asegúrese de incluir la propiedad expandida en la opción $select.</span><span class="sxs-lookup"><span data-stu-id="d317b-149">Make sure to include the expanded property in the $select option.</span></span> <span data-ttu-id="d317b-150">Por ejemplo, la siguiente solicitud obtiene el nombre de producto y el proveedor.</span><span class="sxs-lookup"><span data-stu-id="d317b-150">For example, the following request gets the product name and supplier.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample14.cmd)]

<span data-ttu-id="d317b-151">Este es el cuerpo de respuesta:</span><span class="sxs-lookup"><span data-stu-id="d317b-151">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample15.cmd)]

<span data-ttu-id="d317b-152">También puede seleccionar las propiedades dentro de una propiedad expandida.</span><span class="sxs-lookup"><span data-stu-id="d317b-152">You can also select the properties within an expanded property.</span></span> <span data-ttu-id="d317b-153">La solicitud siguiente amplía productos y selecciona el nombre de categoría de producto.</span><span class="sxs-lookup"><span data-stu-id="d317b-153">The following request expands Products and selects category name plus product name.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample16.cmd)]

<span data-ttu-id="d317b-154">Este es el cuerpo de respuesta:</span><span class="sxs-lookup"><span data-stu-id="d317b-154">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample17.cmd)]

<span data-ttu-id="d317b-155">Para obtener más información acerca de la opción $select, consulte [Seleccionar opción de consulta de sistema ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select) en la documentación oficial de OData.</span><span class="sxs-lookup"><span data-stu-id="d317b-155">For more information about the $select option, see [Select System Query Option ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select) in the official OData documentation.</span></span>

## <a name="getting-individual-properties-of-an-entity-value"></a><span data-ttu-id="d317b-156">Obtener las propiedades individuales de una entidad ($value)</span><span class="sxs-lookup"><span data-stu-id="d317b-156">Getting Individual Properties of an Entity ($value)</span></span>

<span data-ttu-id="d317b-157">Hay dos formas para que un cliente de OData y obtener una propiedad individual de una entidad.</span><span class="sxs-lookup"><span data-stu-id="d317b-157">There are two ways for an OData client to get an individual property from an entity.</span></span> <span data-ttu-id="d317b-158">El cliente puede obtener el valor de formato de OData u obtener el valor sin formato de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="d317b-158">The client can either get the value in OData format, or get the raw value of the property.</span></span>

<span data-ttu-id="d317b-159">La siguiente solicitud obtiene una propiedad en formato de OData.</span><span class="sxs-lookup"><span data-stu-id="d317b-159">The following request gets a property in OData format.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample18.cmd)]

<span data-ttu-id="d317b-160">Aquí es una respuesta de ejemplo en formato JSON:</span><span class="sxs-lookup"><span data-stu-id="d317b-160">Here is an example response in JSON format:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample19.cmd)]

<span data-ttu-id="d317b-161">Para obtener el valor sin formato de la propiedad, anexe $value al URI:</span><span class="sxs-lookup"><span data-stu-id="d317b-161">To get the raw value of the property, append $value to the URI:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample20.cmd)]

<span data-ttu-id="d317b-162">Aquí está la respuesta.</span><span class="sxs-lookup"><span data-stu-id="d317b-162">Here is the response.</span></span> <span data-ttu-id="d317b-163">Tenga en cuenta que el tipo de contenido "text/plain", no es JSON.</span><span class="sxs-lookup"><span data-stu-id="d317b-163">Notice that the content type is "text/plain", not JSON.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample21.cmd)]

<span data-ttu-id="d317b-164">Para admitir estas consultas en el controlador de OData, agregue un método denominado `GetProperty`, donde `Property` es el nombre de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="d317b-164">To support these queries in your OData controller, add a method named `GetProperty`, where `Property` is the name of the property.</span></span> <span data-ttu-id="d317b-165">Por ejemplo, el método para obtener la propiedad Name se denominará `GetName`.</span><span class="sxs-lookup"><span data-stu-id="d317b-165">For example, the method to get the Name property would be named `GetName`.</span></span> <span data-ttu-id="d317b-166">El método debe devolver el valor de esa propiedad:</span><span class="sxs-lookup"><span data-stu-id="d317b-166">The method should return the value of that property:</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample22.cs)]
